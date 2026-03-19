# Ships & Modules (Hardware)

## Ships

Each ship has a unique **starting grid layout**, a **signature passive ability**, and a **signature card** (1 copy in starting deck). Ships are unlocked through meta-progression at the station.

### Starting Ships

#### The Drifter
A battered general-purpose scout. Balanced grid, no strong focus.
- **Grid**: 2×3 layout. 2 Weapon slots, 1 Shield slot, 1 Utility slot, 2 Bridge slots.
- **Passive**: *Salvage Protocol* — after each combat, gain 3 bonus Scrap.
- **Signature Card**: *Field Repair* (Hybrid — 1 Blue, or Heal 6 HP as Command)
- **Starting Deck**: Balanced. 2 Red, 2 Blue, 1 Yellow energy; 2 Commands; 2 Hybrids; 1 Glitch.

#### The Vanguard
A military vessel. Heavy weapons, weak shields, starts hot.
- **Grid**: Asymmetric. 3 Weapon slots (top row), 1 Shield slot, 1 Bridge slot, 1 Utility slot.
- **Passive**: *Fire Superiority* — first Weapon fired each turn deals +5 damage.
- **Signature Card**: *Volley* (Command — all currently charged Weapon modules fire simultaneously)
- **Starting Deck**: Red-heavy. 4 Red, 1 Blue, 1 Yellow; 1 Command; 2 Hybrids; 1 Glitch.

#### The Weaver
A hacker ship. Complex bridge network, lower raw damage output.
- **Grid**: Non-linear. 4 Bridge slots, 1 Weapon slot, 1 Shield slot, 1 Utility slot.
- **Passive**: *Feedback Loop* — when a Bridge triggers, gain 1 card draw next turn.
- **Signature Card**: *Signal Mirror* (Hybrid — 1 Any, or copy the last Bridge effect triggered)
- **Starting Deck**: Mixed energy, Command-heavy. 1 Red, 1 Blue, 2 Yellow, 1 Green; 3 Commands; 2 Hybrids.

### Locked Ships (Unlockable)

| Ship | Focus | Unlock Condition |
|------|-------|-----------------|
| The Bastion | Pure defense / thorns build | Survive 3 missions without dying |
| The Phantom | Void energy / chaos builds | Defeat the Act 2 boss |
| The Foundry | Engine-builder / generation loops | Build 10 total Station modules |
| The Remnant | Hybrid / pre-Static megastructure tech | Discover 20 unique rare cards |
| The Convergence | Two-ships-as-one / split grid | Clear a full 3-act mission |

---

## Module Categories

### 1. Active Sinks — Weapons

Consume energy to deal damage to the enemy. The primary win condition.

| Module | Cost | Effect | Notes |
|--------|------|--------|-------|
| Thermal Cannon | `[Red][Red]` | Deal 14 damage | Basic. Bread and butter. |
| Railgun | `[Red][Red][Red]` | Deal 28 damage | High damage, slower to charge |
| Scatter Shot | `[Red]` | Deal 6 damage to enemy; apply Vulnerable | Fast, soft damage |
| Plasma Lance | `[Red][Yellow]` | Deal 18 damage, ignore Shield | Hybrid cost, strong vs shields |
| Cryo Bolt | `[Blue][Red]` | Deal 10 damage, Slow enemy (reduce next intent by 5) | Hybrid cost |
| EM Disruptor | `[Yellow][Yellow]` | Deal 8 damage, Disable one enemy module for 1 turn | Utility weapon |
| Void Spike | `[Purple][Red]` | Deal 20 damage, random extra effect | Unstable but powerful |
| Chain Launcher | `[Red]` | Deal 4 damage; if enemy already Vulnerable, deal 12 instead | Combo weapon |

### 2. Active Sinks — Defensive

Consume energy to generate shields or remove negative status.

| Module | Cost | Effect | Notes |
|--------|------|--------|-------|
| Cryo-Shield Array | `[Blue][Blue]` | Gain 18 Shield | Standard shield |
| Reactive Plating | `[Blue]` | Gain 10 Shield; enemy takes 3 damage next attack (thorns) | Lower shield, has thorns |
| Ablative Layer | `[Blue][Blue][Blue]` | Gain 30 Shield; Shield carries to next turn | High cost, persistent |
| Phase Barrier | `[Blue][Yellow]` | Gain 15 Shield; absorbs next Glitch card inserted | Niche but powerful |
| Nano-Repair | `[Green][Green]` | Heal 10 HP | Direct HP recovery |
| Coolant Core | `[Blue]` | Reduce Heat by 25 | Essential for heat management |
| Purge Port | `[Any]` | Remove all Glitch cards from hand; draw 1 per removed | Anti-Hacker module |

### 3. Active Sinks — Utility / Foundries

Generate resources, manipulate the deck, or produce ongoing benefits.

| Module | Cost | Effect | Notes |
|--------|------|--------|-------|
| Fabricator | `[Yellow][Yellow]` | Add 1 random Hybrid card to hand | Engine building in combat |
| Scrap Compressor | `[Red]` | Gain 4 Scrap | Economy in combat |
| Bio-Synthesizer | `[Green][Green]` | Regenerate 5 HP per turn for 3 turns (stacks) | Persistent healing |
| Void Tap | `[Purple]` | Discard hand, draw 5 | Reset bad hands |
| Signal Jammer | `[Yellow]` | Reduce all enemy intent values by 10 this turn | Stall tool |
| Card Forge | `[Yellow][Green]` | Upgrade 1 card in hand to `+` version | Permanent mid-combat upgrade |

### 4. Bridges (Passive Wiring)

Bridges sit **between** active modules. They trigger automatically when their condition is met. They do not require energy to operate — they are installed passively.

Bridges are what separate a functional engine from a *great* engine.

| Bridge | Trigger | Effect |
|--------|---------|--------|
| Splitter | When adjacent module fires | Duplicate 1 energy card from that module into the next empty adjacent slot |
| Heat Sink | When any Weapon fires | Convert 50% of Heat generated into Shield |
| Relay | When Left module fires | Transfer remaining partial energy from Left to Right module |
| Amplifier | Once per turn, first fire | Next module fired this turn: +50% effect |
| Loop Bridge | When same module fires twice in one turn | Trigger an additional "echo" of that module's effect at half power |
| Bypass | When target module is Disrupted | Route energy to the next valid module in line instead |
| Cascade Tap | When any Bridge triggers | Gain 1 card draw next turn |
| Accumulator | End of turn | Store up to 2 unspent energy cards, replay them at start of next turn |
| Overcharge Bridge | When a module fires at full charge | Add 5 Heat; increase that module's next fire by +30% |
| Signal Bridge | When Glitch card is played | Purge it and generate 1 energy of a random type |

---

## Grid Layout Concepts

### Adjacency
Modules have positional relationships. Bridges check which modules are to their left, right, above, or below. The player builds synergy by **deliberately placing modules next to specific bridges**.

Example: Place a Splitter bridge between two Thermal Cannons → every Red card you slot fires toward both cannons simultaneously.

### Upgrading Modules
At **Shipyard** nodes mid-mission, modules can be upgraded:
- `+` upgrades increase base effect (more damage, more shield, lower cost)
- Some upgrades unlock secondary triggers ("When this fires, also reduce Heat by 5")
- Modules can also be **sold** at Shipyard nodes for Scrap

### Structural Damage
Enemy Saboteur attacks can **damage module slots**:
- `Disrupted`: slot disabled for N turns
- `Overloaded`: slot fires at 50% effectiveness for the rest of combat
- `Destroyed` (boss mechanic only): slot is permanently removed until Shipyard visit

### Visual Integration
Every module is **visible on the ship illustration**. Installing a Thermal Cannon changes the weapon hardpoint art. Building a Cryo-Shield Array shows new plating on the hull. The ship is a living, growing machine that you can see evolve.
