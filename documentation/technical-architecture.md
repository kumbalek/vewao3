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
| Database | PostgreSQL | User accounts, persistent saves, leaderboards |
| Cache/Sessions | Redis | Session tokens, daily challenge seed caching |
| ORM | Drizzle ORM | Type-safe, schema-first, excellent with TypeScript |
| Auth | JWT + refresh token rotation | Stateless auth; works for browser + future mobile |
| Hosting | Fly.io (or Railway) | Simple container deployment; free tier for development |
| File Storage | Cloudflare R2 | Static assets (game sprites, audio); CDN delivery |

### Offline/Guest Mode
- Full game logic runs client-side; no server required to play
- GameState serialized to `localStorage` — persists across browser sessions
- Guest mode: all progression saved locally. Account creation migrates guest save to cloud.

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

Run seed = integer derived from timestamp at run start. Displayed to player. Same seed = deterministic run.

This enables:
- Replay from seed + input log
- Daily Challenge (shared global seed)
- Bug reproduction (share seed, reproduce exact run)

---

## File Structure

```
reactor-core/
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
