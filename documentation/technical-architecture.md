# Technical Architecture

## Platform & Target

- **Primary**: Web browser (Chrome, Firefox, Safari, Edge — modern versions)
- **Secondary** (future): iOS and Android native via Capacitor or React Native wrapper
- **Target devices**: Desktop first; tablet-friendly; mobile layout planned for Phase 2
- **Offline support**: Full offline play via localStorage. Cloud sync when logged in.

---

## Tech Stack

### Frontend

| Layer | Technology | Reason |
|-------|-----------|--------|
| Framework | React 18 + TypeScript | Component model fits UI needs; TypeScript prevents runtime bugs in complex game logic |
| Game Rendering | PixiJS v8 (WebGL 2D) | Hardware-accelerated 2D; excellent for sprites, particle effects, and canvas animations |
| State Management | Zustand | Lightweight, no boilerplate, works well with game state patterns |
| Build Tool | Vite | Fast HMR, excellent TypeScript support, optimal production bundle |
| Styling | Tailwind CSS | For non-canvas UI (menus, station, overlays); rapid iteration |
| UI Animation | GSAP | Smooth tween-based animations for menus and HUD transitions |
| Game Animation | PixiJS Ticker + Tween | In-game animations via PixiJS native animation system |
| Audio | Howler.js | Web Audio API abstraction; sprite support; excellent browser compat |
| Routing | React Router v6 | Screen navigation (station, map, combat, menus) |

### Backend (MVP: minimal; scale later)

| Layer | Technology | Reason |
|-------|-----------|--------|
| Runtime | Node.js 20 + Fastify | Lightweight HTTP; fast startup; TypeScript-native |
| Database | PostgreSQL | User accounts, persistent saves, fuel state, leaderboards |
| Cache/Sessions | Redis | Session tokens, daily challenge seed caching, used launch token registry |
| ORM | Drizzle ORM | Type-safe, schema-first, excellent with TypeScript |
| Auth | JWT (RS256 auth tokens, ES256 launch tokens) + refresh token rotation | Asymmetric signing; public key embedded in client for launch token verification |
| Hosting | Hetzner VPS + Docker Compose | Self-hosted; ~€4–6/mo for CX21; full control; no vendor lock-in |
| Cloudflare | R2 (storage) + Proxy + DNS | Static asset CDN; reverse proxy hiding origin IP; DDoS protection; SSL termination; domain management |

### Infrastructure & Deployment

**Hetzner VPS**
- Recommended tier: **CX21** (2 vCPU, 4 GB RAM) — sufficient for MVP and early traffic. Upgrade path to CX31 (4 vCPU, 8 GB) if load warrants.
- All services run via **Docker Compose**: `app` (Node.js/Fastify), `db` (PostgreSQL 16), `cache` (Redis 7), `nginx` (reverse proxy)
- nginx listens on port 80 on the internal Docker network, forwards to `app`. Only port 80 is exposed to public — Cloudflare handles 443.
- PostgreSQL data persisted via a named Docker volume (survives container restarts and rebuilds)

**Cloudflare**
- **DNS**: Domain A record points to Hetzner IP with proxy enabled (orange cloud) — origin IP is never exposed
- **Proxy / DDoS**: All traffic passes through Cloudflare edge before reaching Hetzner; free WAF rules included
- **SSL**: "Full" mode — Cloudflare terminates HTTPS at the edge, forwards HTTP to nginx. Origin uses a **Cloudflare Origin CA cert** (free, 15-year validity, no certbot/renewal needed)
- **R2**: Static assets (sprites, audio) served from an R2 bucket via Cloudflare CDN — no bandwidth cost between R2 and Cloudflare edge

**Deployment workflow**
```bash
ssh user@hetzner-vps
git pull
docker compose up -d --build
```
Secrets (DB credentials, JWT private keys, Cloudflare tokens) stored in a `.env` file on the server only — never committed to the repo.

---

### Offline/Guest Mode
- Full game logic runs client-side. **The run itself requires no server calls.**
- GameState serialized to `localStorage` — persists across browser sessions
- Guest mode: 3 trial launches stored locally (unprotected — guest cheating has no consequence since they're not monetized). After 3 launches, guests are prompted to register.
- Account creation migrates guest save to cloud. Fuel transitions from local trial to server-controlled.

---

## Game Architecture

### State Machine

The game is modeled as an explicit **Finite State Machine (FSM)**:

```
States:
  STATION           → player is at The Anchorage
  MAP_NAVIGATION    → player is on mission map, selecting next node
  COMBAT_INIT       → setting up a combat encounter
  COMBAT_DRAW       → drawing player hand
  COMBAT_PLAYER     → player routing phase (main interaction)
  COMBAT_RESOLVE    → processing module fires and cascade chains
  COMBAT_ENEMY      → enemy executing intents
  COMBAT_CLEANUP    → status ticks, end-of-turn resolution
  COMBAT_VICTORY    → combat won, reward screen
  COMBAT_DEFEAT     → player HP = 0, death sequence
  EVENT             → text event node active
  SHOP              → shop interface open
  SHIPYARD          → shipyard interface open
  MISSION_RETURN    → player manually returned, summary screen
  RUN_END           → post-run summary (death or completion)

Transitions:
  STATION → MAP_NAVIGATION           (launch mission)
  MAP_NAVIGATION → COMBAT_INIT       (enter combat node)
  MAP_NAVIGATION → EVENT             (enter event node)
  MAP_NAVIGATION → SHOP              (enter shop node)
  MAP_NAVIGATION → SHIPYARD          (enter shipyard node)
  COMBAT_INIT → COMBAT_DRAW
  COMBAT_DRAW → COMBAT_PLAYER
  COMBAT_PLAYER → COMBAT_RESOLVE     (end turn pressed)
  COMBAT_RESOLVE → COMBAT_ENEMY
  COMBAT_ENEMY → COMBAT_CLEANUP
  COMBAT_CLEANUP → COMBAT_DRAW       (next turn)
  COMBAT_CLEANUP → COMBAT_VICTORY    (enemy HP = 0)
  COMBAT_CLEANUP → COMBAT_DEFEAT     (player HP = 0)
  COMBAT_VICTORY → MAP_NAVIGATION
  COMBAT_DEFEAT → RUN_END
  EVENT/SHOP/SHIPYARD → MAP_NAVIGATION
  MAP_NAVIGATION → MISSION_RETURN    (player chooses to return)
  MISSION_RETURN → STATION
  RUN_END → STATION
```

### Entity Component System (Lightweight ECS)

Game objects are modeled as entities with component data. The ECS is intentionally lightweight — no heavy framework needed for this scale.

```typescript
// Core entities
type CardEntity = {
  id: string
  name: string
  type: 'energy' | 'command' | 'hybrid' | 'glitch'
  energyType: EnergyType | null
  energyValue: number
  effects: Effect[]
  art: string           // asset key
  rarity: Rarity
  upgraded: boolean
  exhaust: boolean
  retain: boolean
}

type ModuleEntity = {
  id: string
  name: string
  category: 'weapon' | 'defensive' | 'utility' | 'bridge'
  energyCost: EnergyRequirement[]   // e.g. [{type: 'red', count: 2}]
  effects: Effect[]
  bridgeCondition?: BridgeTrigger   // null for non-bridges
  position: GridPosition
  status: ModuleStatus
  upgraded: boolean
}

type EnemyEntity = {
  id: string
  name: string
  maxHp: number
  currentHp: number
  shield: number
  phase: number
  phases: EnemyPhase[]
  currentIntent: Intent
  statuses: StatusEffect[]
  archetype: EnemyArchetype
}
```

### Event Bus

The cascade chain system requires an **event bus** — modules publish events, bridges subscribe and react:

```typescript
type GameEvent =
  | { type: 'MODULE_FIRED'; moduleId: string; position: GridPosition }
  | { type: 'ENERGY_SLOTTED'; moduleId: string; energyType: EnergyType }
  | { type: 'HEAT_CHANGED'; delta: number; newValue: number }
  | { type: 'DAMAGE_DEALT'; target: 'player' | 'enemy'; amount: number }
  | { type: 'SHIELD_APPLIED'; target: 'player' | 'enemy'; amount: number }
  | { type: 'GLITCH_INSERTED'; cardId: string }
  | { type: 'TURN_ENDED' }
  | { type: 'ENEMY_TURN_STARTED' }

// EventBus: publish/subscribe pattern
eventBus.on('MODULE_FIRED', (event) => {
  // Bridge modules check if they should react to this fire
})
```

The cascade chain cap (12 triggers) is enforced at the event bus level — it counts recursive triggers within a single resolution cycle.

### Data-Driven Design

All game content (cards, modules, enemies, events) is defined in **JSON config files** in `src/game/data/`. The game engine reads these configs at runtime. No game balance numbers are hardcoded in logic.

```
src/game/data/
  cards/
    energy-red.json
    energy-blue.json
    commands.json
    hybrids.json
    glitches.json
  modules/
    weapons.json
    defensive.json
    utility.json
    bridges.json
  enemies/
    act1.json
    act2.json
    act3.json
    bosses.json
  events/
    standalone.json
    chains.json
  ships/
    ships.json
```

Each config entry has a unique `id` string (e.g., `"card_thermal_cannon_red"`) used as the canonical reference throughout the system.

### Randomness & Seeding

All randomness uses a **seeded PRNG** (Mulberry32 algorithm — simple, fast, good distribution):

```typescript
class SeededRandom {
  private seed: number
  constructor(seed: number) { this.seed = seed }
  next(): number { /* mulberry32 impl */ }
  nextInt(min: number, max: number): number { ... }
  shuffle<T>(array: T[]): T[] { ... }
}
```

Run seed = server-generated integer, embedded in the signed launch token. Displayed to player after launch. Same seed = deterministic run. Server-generation serves dual purpose: determinism for sharing/daily challenge, and making the seed a functional dependency of the launch token (no valid token → no seed → no run).

This enables:
- Replay from seed + input log
- Daily Challenge (shared global seed)
- Bug reproduction (share seed, reproduce exact run)

---

## File Structure

```
reactor-core/
├── Dockerfile                         # Node.js app image (production)
├── docker-compose.yml                 # Orchestrates app, db, redis, nginx
├── nginx/
│   └── nginx.conf                     # Reverse proxy: port 80 → app:3000
├── src/
│   ├── game/
│   │   ├── engine/
│   │   │   ├── StateMachine.ts         # Game FSM
│   │   │   ├── GameLoop.ts             # Turn orchestration
│   │   │   └── EventBus.ts             # Module trigger event system
│   │   ├── systems/
│   │   │   ├── CombatSystem.ts         # Damage, shields, HP
│   │   │   ├── CascadeSystem.ts        # Bridge chain resolution
│   │   │   ├── HeatSystem.ts           # Heat tracking and thresholds
│   │   │   ├── DeckSystem.ts           # Draw, discard, shuffle
│   │   │   ├── EnemyAISystem.ts        # Intent generation, enemy turns
│   │   │   └── ProgressionSystem.ts   # Meta currency, unlocks
│   │   ├── entities/
│   │   │   ├── Card.ts
│   │   │   ├── Module.ts
│   │   │   ├── Enemy.ts
│   │   │   └── Ship.ts
│   │   └── data/                       # JSON configs (see above)
│   ├── rendering/
│   │   ├── scenes/
│   │   │   ├── CombatScene.ts          # PixiJS combat rendering
│   │   │   ├── MapScene.ts             # Mission map rendering
│   │   │   └── StationScene.ts         # Station rendering
│   │   └── assets/
│   │       ├── AssetLoader.ts          # PixiJS asset management
│   │       └── atlases/                # Sprite atlas configs
│   ├── ui/
│   │   ├── components/
│   │   │   ├── Card.tsx                # Card React component
│   │   │   ├── HeatMeter.tsx
│   │   │   ├── HPBar.tsx
│   │   │   ├── EnemyIntentDisplay.tsx
│   │   │   └── ...
│   │   └── screens/
│   │       ├── StationScreen.tsx
│   │       ├── MapScreen.tsx
│   │       ├── CombatHUD.tsx
│   │       └── ...
│   ├── store/
│   │   ├── gameStore.ts               # Zustand — active run state
│   │   ├── metaStore.ts               # Zustand — station, unlocks
│   │   └── settingsStore.ts           # Zustand — audio, display prefs
│   └── utils/
│       ├── prng.ts                    # Mulberry32 seeded random
│       ├── serialization.ts           # GameState → JSON and back
│       └── constants.ts               # Game-wide constants
├── server/                            # Backend
│   ├── routes/
│   │   ├── auth.ts
│   │   ├── saves.ts
│   │   ├── fuel.ts                    # /fuel/spend, /fuel/status
│   │   ├── runs.ts                    # /run/complete (launch token validation)
│   │   ├── purchase.ts                # /purchase/verify (payment callback)
│   │   └── leaderboard.ts
│   ├── db/
│   │   ├── schema.ts                  # Drizzle schema
│   │   └── migrations/
│   └── index.ts
├── public/
│   └── assets/                        # Static assets
└── docs/                              # This documentation folder
```

---

## Drive Fuel Security

### Design Intent

Fuel is the only thing that needs to be tamper-resistant — it's the monetization boundary. Everything else (run state, scrap, deck contents) can be freely edited by the player; there's no PVP, so cheating hurts only the cheater and isn't worth defending against.

### Online-at-Launch, Offline-During-Run

The run itself runs 100% client-side with no server calls. Only two moments require connectivity:

1. **Mission launch** — client spends 1 fuel (server validates and issues a signed launch token)
2. **Dock / death** — client syncs run result back (server validates the token and updates meta state)

This preserves the offline-play benefit while keeping the monetization hook server-controlled.

### Launch Token Flow

```
Client                              Server
  |                                    |
  | POST /api/fuel/spend               |
  | { authJwt }                        |
  | ---------------------------------> |
  |                                    | validate JWT
  |                                    | check fuelCount > 0 (or isPurchased)
  |                                    | deduct 1 fuel
  |                                    | compute nextRegenAt
  |                                    | generate nonce, sign token
  | { launchToken, fuelRemaining }     |
  | <--------------------------------- |
  |                                    |
  [RUN PROCEEDS ENTIRELY OFFLINE]      |
  |                                    |
  | POST /api/run/complete             |
  | { launchToken, runResult }         |
  | ---------------------------------> |
  |                                    | verify HMAC signature
  |                                    | check token not expired (48h window)
  |                                    | check nonce not already used (Redis)
  |                                    | update meta state (scrap, unlocks, etc.)
  |                                    | mark nonce used
  | { updatedMetaState }               |
  | <--------------------------------- |
```

**Launch token payload** (ES256 / ECDSA signed — asymmetric):
```typescript
type LaunchToken = {
  userId: string
  runSeed: number       // server-generated — client uses this as the PRNG seed
  issuedAt: number      // unix timestamp
  expiresAt: number     // issuedAt + 4 hours (matches access token window)
  nonce: string         // UUID v4 — single-use, stored in Redis
}
```

**Why asymmetric (ES256) and not HMAC-SHA256**: A player can mock the `/api/fuel/spend` endpoint (via a local proxy or browser extension) to return a fake 200 response. If the client just stores whatever token it receives, a mocked endpoint grants free unlimited launches. With HMAC, the client can't verify the token is genuine — only the server can. With ES256, the **public key is embedded in the client bundle at build time**, and the client *verifies the signature before launching*. A mocked response cannot produce a valid ES256 signature without the private key, which never leaves the server.

**Why the run seed is server-issued**: The seed is embedded in the signed launch token. Without a genuine launch token, the client has no seed and cannot initialize a run. This means the token is not just an authorization check — it is a functional dependency of the run itself.

```typescript
// Client-side, before launching:
import { jwtVerify, importSPKI } from 'jose'

const PUBLIC_KEY = await importSPKI(EMBEDDED_PUBLIC_KEY_PEM, 'ES256')

async function verifyAndExtractLaunchToken(token: string): Promise<LaunchToken | null> {
  try {
    const { payload } = await jwtVerify(token, PUBLIC_KEY)
    return payload as LaunchToken   // includes runSeed
  } catch {
    return null   // forged, expired, or tampered — block launch
  }
}
```

### Token Rotation & Content Gating

All server data requests — meta state, progression, unlocks — require a valid **access token**. Access tokens are short-lived and rotate regularly. This gates ongoing access to progression data without requiring mid-run server calls.

```
Token lifetimes:
  Access token:   4 hours   — required for all API requests
  Refresh token:  30 days   — rotates on every use (refresh token rotation)
  Launch token:   4 hours   — embedded seed; single-use nonce
```

**How rotation closes the residual abuse window:**

Without token rotation, a sophisticated attacker could capture one real launch token, never sync results (keeping the nonce alive in Redis), and replay the same token for 48h. With 4h access tokens:

1. Attacker's access token expires after 4 hours
2. They use their refresh token to get a new access token — real server call, succeeds (auth is separate from fuel)
3. To start a new run, they need a new launch token with a new server-issued seed — requires `/api/fuel/spend`, which checks fuel → **blocked**
4. They can finish their current run (seed already in hand), but cannot start the next one

**Maximum abuse collapses from "one token per 48h of unlimited play" to "one run."**

Additionally, without a valid access token, **progression data is inaccessible on session load**. An attacker playing with an invalidated session sees base-state defaults: no station upgrades, starter ships only, base card pool. The game degrades noticeably without progression, creating a compounding soft deterrent.

```
Session load sequence (account):
  1. App loads → check localStorage for access token
  2. If expired: use refresh token → GET /api/auth/refresh → new access token
  3. If refresh token invalid/expired: redirect to login
  4. Fetch meta state with valid access token → GET /api/meta
  5. Station screen populates with real progression
  6. "Launch" → POST /api/fuel/spend (with access token) → signed launch token (with seed)
  7. Client verifies ES256 signature → run initializes with server seed
  8. Run proceeds fully offline
  9. Dock/death → POST /api/run/complete → meta state synced
```

### What This Protects

| Attack | Protected? | How |
|--------|-----------|-----|
| Edit localStorage to add fuel | Yes | Fuel never lives in localStorage for accounts |
| Mock the fuel endpoint | Yes | Client verifies ES256 signature; forged tokens fail. No valid token = no run seed = no run. |
| Replay an old launch token | Yes | Nonce is single-use (Redis); token expires in 4h |
| Extend a session indefinitely | Partial | Access tokens rotate every 4h; new run requires new launch token via real `/api/fuel/spend` |
| Play with stale progression | Partial | Meta state requires valid access token; expired session sees base defaults |
| Edit run state (scrap, cards, etc.) | No (intentional) | No PVP; run state editing hurts only the player |
| Claim a run result without having launched | Yes | Server validates launch token signature + nonce |

### Residual Attack Surface

A determined attacker can maintain a valid refresh token indefinitely (they log in for real), get new access tokens every 4h via real server calls, and use one real launch token per 4h to get one free run per window. **Maximum abuse: one free run every 4 hours**, which requires active maintenance of the proxy setup and nets the attacker perhaps 5–6 extra runs per day.

This is an acceptable ceiling for an indie game. The effort required (persistent proxy, token management, 4h re-authentication cycle) is disproportionate to the value gained, and these players represent near-zero conversion probability.

### Fuel Regeneration

Fuel is computed on-read — no background cron needed. The DB stores `fuelLastDepleted` and `fuelCount`. On each `/api/fuel/spend` or any read, the server computes:

```typescript
const hoursSinceDepleted = (now - fuelLastDepleted) / 3600_000
const regenned = Math.floor(hoursSinceDepleted / REGEN_INTERVAL_HOURS)  // 2h per fuel
const currentFuel = Math.min(FUEL_CAP, fuelCount + regenned)
```

This is stateless regeneration — no scheduled jobs, no drift.

### Guest Flow

Guests get 3 trial launches stored in `localStorage.guestFuel`. These are unprotected — a guest who edits localStorage to give themselves more fuel is a guest who will never pay anyway, so the loss is zero. When the trial runs out, the prompt is:

> *"Create a free account to keep playing. Your progress saves to the cloud and you start with 5 fuel."*

This converts the trial wall into a registration prompt, not a payment wall. The payment ask comes later, once the player is engaged.

### Failure Modes

| Scenario | Behavior |
|----------|----------|
| Server unreachable at launch | Show error: "Can't reach the server. Check your connection." No launch. |
| Server unreachable at dock | Store `{ launchToken, runResult }` in `localStorage.pendingSync`. Retry on next app load. Cap pending syncs at 2 to prevent abuse. |
| Player closes tab mid-run | `runState` in localStorage resumes on next load. Launch token still valid (48h window). |
| Token expires before player docks | 48h window is generous. If expired, run result is accepted but scored as guest (no meta-state update). Player is notified. |

### New Backend Routes

| Route | Auth | Purpose |
|-------|------|---------|
| `POST /api/auth/refresh` | Refresh token | Rotate access token; refresh token rotates on every use |
| `GET /api/meta` | Access token | Load meta state (station, unlocks, progression) |
| `POST /api/fuel/spend` | Access token | Validate fuel, deduct 1, generate run seed, issue signed ES256 launch token |
| `GET /api/fuel/status` | Access token | Current fuel count + next regen time (for UI display) |
| `POST /api/run/complete` | Access token | Validate launch token signature + nonce, sync run result to meta state |
| `POST /api/purchase/verify` | Access token | Set `isPurchased = true` after payment provider callback |

---

## Save System

### Run State (in-run)
Saved after every action that changes game state. Stored in `localStorage.runState` as JSON. Structure:

```typescript
type RunState = {
  seed: number
  ship: ShipId
  act: 1 | 2 | 3
  nodeHistory: NodeId[]
  currentNode: NodeId | null
  deck: CardEntity[]
  discard: CardEntity[]
  modules: ModuleEntity[]
  gridLayout: GridPosition[]
  hp: number
  maxHp: number
  heat: number
  scrap: number
  components: number
  statuses: StatusEffect[]
  rngState: number          // current PRNG state for determinism
}
```

### Meta State (permanent)
Saved to `localStorage.metaState` (guest) or synced to Postgres (logged in):

```typescript
type MetaState = {
  userId: string | null       // null = guest
  scrapMetal: number
  dataCores: number
  voidCrystals: number
  stationModules: StationModuleId[]
  stationModuleTiers: Record<StationModuleId, number>
  unlockedShips: ShipId[]
  discoveredCards: CardId[]   // for rare pool expansion
  discoveredModules: ModuleId[]
  achievements: AchievementId[]
  ascensionLevel: Record<ShipId, number>
  personalBests: Record<ShipId, number>
  totalRuns: number
  totalDeaths: number
  // Fuel — authoritative on server for accounts; local trial count for guests
  fuelCount: number           // server-controlled for accounts
  fuelLastDepleted: number    // unix timestamp; used to compute regen on read
  isPurchased: boolean        // true → fuel check skipped entirely
}
```

---

## Performance Targets

| Metric | Target |
|--------|--------|
| Initial load time | < 3 seconds on 10Mbps connection |
| Time to first combat | < 5 seconds from load |
| Combat render | 60fps on mid-range desktop |
| Input response time | < 16ms (1 frame) for card drag start |
| Save write time | < 10ms (localStorage, synchronous) |
| Bundle size (gzipped) | < 2MB JS + lazy-loaded assets |

---

## Development Environment

```bash
# Start dev server
npm run dev

# Type check
npm run typecheck

# Run tests
npm run test

# Build for production
npm run build

# Preview production build
npm run preview
```

Testing strategy:
- **Unit tests** (Vitest): All game systems (CombatSystem, CascadeSystem, HeatSystem, etc.)
- **Integration tests**: Full turn cycle tests — given a board state + hand, assert correct resolution
- **No UI tests initially** — UI is thin over game logic; logic tests cover correctness
- **Snapshot tests** for game data configs — ensure no config regressions on changes
