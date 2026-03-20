# Development Roadmap

## Guiding Principle

**Build the core loop first, then expand.** A working, fun combat encounter with 10 cards and 3 modules is more valuable than a half-implemented version of the full game. Each phase ends with a playable, demonstrable build.

---

## Phase 0: Pre-Production (Current)
*Duration: 2–3 weeks*

**Goal**: Complete documentation, technical setup, and asset pipeline before writing game logic.

### Deliverables
- [x] Core concept and vision document (`idea.md`)
- [x] Combat system spec (`micro-game.md`)
- [x] Macro game spec (`macro-game.md`)
- [x] Cards spec (`cards.md`)
- [x] Ships & Modules spec (`ship-and-modules.md`)
- [x] Enemies spec (`enemies.md`)
- [x] Story & lore (`story-and-lore.md`)
- [x] Visual design direction (`visual-design.md`)
- [x] Audio design direction (`audio-design.md`)
- [x] Status effects spec (`status-effects.md`)
- [x] Map & missions spec (`map-and-missions.md`)
- [x] Progression & economy spec (`progression-and-economy.md`)
- [x] Technical architecture spec (`technical-architecture.md`)
- [x] Game balance spec (`game-balance.md`)
- [x] Tone revision: Firefly/Foundation hybrid — frontier Western + galactic decline
- [x] Return mechanic redesign: ship persists on safe return, Faction Alert on waypoint re-entry
- [x] Faction doc pass: The Solean Empire, The Free Reach, faction pool by act
- [x] Drive Fuel monetization design: free-to-play session limits, buy-once for unlimited
- [ ] Repo setup (Vite + React + TypeScript + PixiJS)
- [ ] CI/CD pipeline (GitHub Actions: typecheck, test, deploy preview)
- [ ] Design system (color tokens, typography, component library skeleton)
- [ ] Asset pipeline decision (what format, naming conventions)

---

## Phase 1: Core Combat Prototype
*Duration: 4–6 weeks*

**Goal**: A single combat encounter is fully playable end-to-end. No mission map, no station. Just combat.

### Must Have
- [ ] Ship grid renderer (PixiJS scene — static layout, correct cells)
- [ ] Module data system — at least 6 modules loaded from JSON configs
- [ ] Card data system — at least 15 cards loaded from JSON configs
- [ ] Hand of cards rendered (bottom strip, React component)
- [ ] Drag-and-drop card → module slot (PixiJS interaction)
- [ ] Module charging logic (energy accumulates until cost met)
- [ ] Module fires — basic weapon damage, basic shield gain
- [ ] Enemy display (one enemy, static illustration placeholder)
- [ ] Enemy intents — telegraphed, displayed, executed
- [ ] Heat system — builds, thresholds, Meltdown
- [ ] HP bars (player and enemy)
- [ ] Turn cycle — Draw → Route → End Turn → Enemy Turn → Cleanup → next Draw
- [ ] Win/Lose conditions
- [ ] Basic bridge module (1 bridge type: Splitter)
- [ ] Cascade chain resolution (event bus + depth limit)
- [ ] Combat victory/defeat screen (placeholder)

### End of Phase 1 Demo
A single combat against The Sentinel (Act 1 boss) with The Drifter ship, 15-card deck, 4 modules. Fully playable. Cascade chains work. Heat works. Win and lose states reached.

---

## Phase 2: Full Combat System
*Duration: 3–4 weeks*

**Goal**: Combat is feature-complete. All card types, all module categories, all status effects, all enemy archetypes.

### Must Have
- [ ] All 4 card categories implemented (Raw Energy, Command, Hybrid, Glitch)
- [ ] Full Glitch card set + Purge Port module
- [ ] All 4 bridge module types (Splitter, Heat Sink, Relay, Amplifier)
- [ ] All status effects (Vulnerable, Disrupted, Focused, Burning, etc.)
- [ ] All enemy archetypes (Bruiser, Hacker, Saboteur, Armored)
- [ ] Multi-phase boss fight (The Architect — Phase 2 grid mutation)
- [ ] Module upgrade system (`+` versions)
- [ ] Card upgrade system (`+` versions)
- [ ] Grid rearrangement (drag modules to reposition)
- [ ] Full card reward screen (3-card choice post-combat)
- [ ] Seeded randomness (Mulberry32) — enemy selections, card rewards

### Nice to Have
- [ ] Particle effects for module fires
- [ ] Cascade chain visual animation (energy traveling along connections)
- [ ] Energy type sound effects

### End of Phase 2 Demo
Configurable combat sandbox: choose ship, configure deck, configure board, fight any enemy. Used for balance testing and early playtesters.

---

## Phase 3: Mission Map & Roguelike Loop
*Duration: 4–5 weeks*

**Goal**: A full roguelike run is playable from start to death or return. Mission map, node traversal, shops, events. Ship persists on safe return.

### Must Have
- [ ] Mission map generator (seeded node graph, 3 acts)
- [ ] Node type routing (Combat, Elite, Shop, Event, Shipyard, Rest)
- [ ] Map screen UI (star chart visual, node selection)
- [ ] Shipyard screen (rearrange grid, install modules, upgrade)
- [ ] Shop screen (buy cards, modules, remove cards)
- [ ] Rest node (Repair or Calibrate choice)
- [ ] Event system — at least 8 standalone events implemented (including BORDER DISPUTE, ANOMALOUS TRANSMISSION)
- [ ] Card rewards tied to combat victories
- [ ] Module rewards tied to Elite victories
- [ ] Run persistence (serialized GameState to localStorage)
- [ ] Run resume (reload page mid-run → continue where left off)
- [ ] Death screen (Mind-Save narrative, run summary)
- [ ] **Ship persistence on safe return** (serialize deck + board state across runs)
- [ ] **Waypoint system** (save last act boundary; offer fresh-start vs. waypoint re-entry)
- [ ] **Faction Alert** (+20% enemy HP on waypoint re-entry; reduced by Deep Space Relay module)
- [ ] **Drive Fuel system** — server-side counter; 5 cap for free accounts; regeneration computed on-read (no cron)
- [ ] **Launch token** — HMAC-signed token issued on fuel spend; validated on run complete; nonce stored in Redis
- [ ] **Guest trial** — 3 local launches in `localStorage.guestFuel`; registration prompt on depletion
- [ ] **Pending sync queue** — `localStorage.pendingSync` for dock results when server unreachable; retry on next load
- [ ] Scrap economy (earn, spend, balance)
- [ ] Act 1 and Act 2 fully functional with 2 boss encounters
- [ ] Act 1 enemy pool: Rim Pirates, Scavengers, Border Militia
- [ ] Act 2 enemy pool: Imperial Border Guard, Free Reach Forces

### End of Phase 3 Demo
A full roguelike run. Player can die (losing ship) or return safely (keeping ship). Faction Alert, waypoint system, and Drive Fuel are live. Loop is complete. Send to 10 external playtesters. Gather feedback.

---

## Phase 4: The Station & Meta-Progression
*Duration: 3–4 weeks*

**Goal**: The Stead is built. Meta-progression gives players reasons to keep playing across runs.

### Must Have
- [ ] Station screen (top-down Stead layout)
- [ ] Core rooms (Command Deck, Hangar Bay, Mind-Save Pod, Reactor Core)
- [ ] At least 6 buildable station modules implemented
- [ ] **Diplomatic Contacts module** (faction shop access, faction-specific discounts)
- [ ] **Deep Space Relay module** (deeper waypoint depth, reduced Faction Alert penalty)
- [ ] Meta currency system (Scrap Metal, Data Cores, Void Crystals)
- [ ] Station power budget (Reactor Core limits active modules)
- [ ] Pre-mission setup (select ship/waypoint, choose active station modules)
- [ ] All 3 starter ships with unique grids and passives
- [ ] Unlock system (ships, card pool expansion, module pool expansion)
- [ ] Ascension system (levels 1–5 implemented)
- [ ] Run scoring and personal bests
- [ ] **Full game purchase flow** — payment provider webhook → `isPurchased = true` in DB; fuel check bypassed server-side
- [ ] **`/api/fuel` routes** — `POST /spend`, `GET /status`, `POST ../run/complete`, `POST ../purchase/verify`

### End of Phase 4 Demo
A complete game loop. Station grows. Ships unlock. Cards pool expands. Drive Fuel purchase flow is live. Send to 25 external playtesters. This is the "Early Access candidate" build.

---

## Phase 5: Content & Polish
*Duration: 4–6 weeks*

**Goal**: Content breadth and production quality reach release standards.

### Must Have
- [ ] Act 3 fully implemented (The Galactic Core Approaches biome, final boss The Signal)
- [ ] All remaining station modules
- [ ] At least 5 of 8 unlockable ships implemented
- [ ] 60+ cards in card pool
- [ ] 20+ modules in module pool
- [ ] 20+ events implemented (including IMPERIAL COURIER, RENEGADE BROADCAST, 2 narrative chains)
- [ ] The Quiet Frequency narrative chain (runs 1/3/5, leads to The Integrated)
- [ ] The Survivor narrative chain (full arc)
- [ ] Full audio implementation (Howler.js, all SFX, adaptive music)
- [ ] Visual polish pass (particle effects, module fire animations, cascade animations)
- [ ] Glitch card visual effects (corruption distortion)
- [ ] Tutorial / first-run onboarding (tooltips, guided first combat)
- [ ] Settings screen (audio volume, accessibility options)
- [ ] Responsive layout for tablet

### Nice to Have
- [ ] Leaderboard (requires backend)
- [ ] Daily Challenge mode (shared seed)
- [ ] Cosmetics system (basic)

---

## Phase 6: Launch Preparation
*Duration: 2–3 weeks*

### Must Have
- [ ] Account system (email/password + guest mode)
- [ ] Cloud save sync
- [ ] Privacy policy / terms of service
- [ ] Analytics (anonymized — run counts, completion rates for balance data)
- [ ] Error monitoring (Sentry or similar)
- [ ] Performance profiling and optimization
- [ ] Final balance pass (informed by playtest data)
- [ ] Browser compatibility testing (Chrome, Firefox, Safari, Edge)
- [ ] Load testing (for launch traffic)
- [ ] App store submission preparation (if mobile launch is concurrent)

---

## Post-Launch Roadmap (Planned)

| Feature | Priority | Notes |
|---------|----------|-------|
| Mobile native app (iOS/Android) | High | After web launch is stable |
| Remaining unlockable ships | High | Complete all 8 ships |
| New biome: The Void Rift (optional Act 4) | Medium | Endgame content for hardcore players |
| Multiplayer: Asynchronous "ghost run" | Medium | See other players' run paths on the map |
| Daily/Weekly challenge leaderboards | Medium | Requires backend stability first |
| Modding support (custom cards via JSON) | Low | Stretch goal; data-driven design makes this feasible |
| Localization (Spanish, Portuguese, French) | Medium | After v1.0 content lock |

---

## Team & Roles (If Applicable)

| Role | Responsibilities |
|------|----------------|
| Developer (Lead) | Engine, systems, backend |
| Developer (Frontend) | UI, React components, PixiJS rendering |
| Designer | Game balance, card/module design, event writing |
| Artist | Ship illustrations, card art, enemy art, UI |
| Audio | Music composition, SFX design |
| QA / Playtester | Regression testing, balance feedback |

*For solo development: Phase 1–3 focus purely on programmer-art placeholders. Art/audio are integrated in Phase 5.*

---

## Risk Register

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| Cascade chain performance issues (too many triggers) | Medium | High | Hard cap at 12; profile early |
| Balance never feeling right without playtester feedback | High | High | External playtest at end of Phase 3; iterate on data |
| Drag-and-drop on mobile is awkward | Medium | Medium | Design tap-to-select fallback from start |
| Scope creep on content (too many cards before core loop is fun) | High | Medium | Phase 1–2 content minimum; resist adding until loop works |
| Audio asset production blocking launch | Medium | Low | Use free/licensed placeholder audio until Phase 5 |
| Save corruption on browser localStorage | Low | High | Schema versioning; migration system for save format changes |
| Drive Fuel feels too restrictive — free players hit the wall at a bad moment | Medium | High | 5 fuel is ~2–3 solid runs; tune in playtest; err generous; guest trial + registration prompt softens the wall |
| Drive Fuel feels too generous — no incentive to purchase | Low | Medium | Track conversion rate; can tighten cap or add cosmetic bundle without changing gameplay cap |
| Players resent the fuel system as "pay to play" | Medium | Medium | Clear messaging: buying removes cap only, all content free; no P2W; guest trial delays the ask |
| Launch token abuse — player captures and replays token | Low | Low | Nonce is single-use in Redis; 48h expiry; not worth attacking (run state isn't protected anyway) |
| Server outage blocks all play | Low | High | Pending sync queue; consider short "offline grace" window for trusted accounts (future) |
| Waypoint re-entry Faction Alert confuses new players | Low | Medium | Fresh start is always the default; waypoint is opt-in with clear UI tooltip |
