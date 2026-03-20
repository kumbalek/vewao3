# Enemies

## Design Philosophy

Enemies don't just chip away at HP — they attack your **engine**. Every archetype is designed to stress a different aspect of your build:

- **Bruisers** test your damage output and Shield generation speed
- **Hackers** test your deck quality and Glitch management
- **Saboteurs** test your routing flexibility and board adaptability
- **Swarmers** test your ability to prioritize and finish quickly

All enemies **telegraph their intent every turn**. No hidden information. The game is a planning puzzle, not a reaction test.

The four archetypes are faction-agnostic — the same mechanical behaviors appear across human faction enemies (Acts 1–2) and unknown contact enemies (Act 3). What changes is the flavor, naming, and specific variant parameters.

---

## Enemy Intent System

Before the player's turn begins, each enemy displays its planned action for the coming enemy turn. Intents are shown as clear iconography above the enemy:

| Icon | Intent Type | Example |
|------|------------|---------|
| ⚔️ + number | Attack | "Attack for 18" |
| 🛡️ + number | Shield | "Gain 20 Shield" |
| 💉 | Status | "Insert 2 Glitch cards into your deck" |
| 🔧 | Board Attack | "Disable Module Slot 2 next turn" |
| 🔄 | Buff | "Increase next attack by 50%" |
| ☠️ | Escalate | "Entering Phase 2" |

Some enemies have **multi-intent turns** (elite/boss tier): they perform multiple actions in sequence.

---

## Enemy Archetypes

### The Bruiser
**Role**: High sustained damage. Forces efficient Shield generation and fast DPS.

Behavior pattern:
- Alternates between heavy attacks and brief recovery turns
- Attacks escalate in damage each cycle ("Escalating Protocol")
- Occasionally taunts: grants itself Shield to protect from burst damage
- Does not interact with your deck or board — purely tests HP sustain

#### Human Faction Variants (Acts 1–2)

| Variant | Faction | Act | Notes |
|---------|---------|-----|-------|
| `Rim Enforcer` | Rim Pirates | Act 1 | Basic Bruiser; hits for 12–14 |
| `Border Cutter` | Imperial Border Guard | Act 1–2 | Slightly higher HP, shield taunt at 60% |
| `Reach Soldier` | The Free Reach | Act 2 | Faster escalation pattern |
| `Reach Heavy` | The Free Reach | Act 2 Elite | High HP, hits for 20+, multi-hit combo |

#### Unknown Contact Variants (Act 3)
| Variant | Notes |
|---------|-------|
| `Siege Platform` | Massive HP pool, telegraphs multi-hit combos |
| `Assault Carrier` | High sustained damage; escalation mechanic accelerated |

---

### The Hacker
**Role**: Disrupts deck quality. Forces Glitch management and Purge module usage.

Behavior pattern:
- Every 2 turns: "Insert N Glitch cards into your deck (shuffled in)"
- Glitch cards inserted match the level: Act 1 inserts Static Noise, Act 3 inserts Viral Code
- Attacks are weak — the real threat is the growing Glitch payload in your deck
- If not killed before a threshold turn, begins inserting 2 Glitches per turn instead of 1

#### Human Faction Variants (Acts 1–2)

| Variant | Faction | Act | Notes |
|---------|---------|-----|-------|
| `Scavenger Jammer` | Scavengers | Act 1 | Inserts 1 Static Noise every 2 turns; basic attacks |
| `Signals Corp` | Imperial Border Guard | Act 2 | Inserts Malware; has self-shield mechanic |
| `Breach Operator` | The Free Reach | Act 2 | Inserts Malware; escalates faster if not prioritized |
| `Cascade Infector` | Unknown | Act 2 Elite | Inserts 1 Glitch per turn AND attacks moderately |

#### Unknown Contact Variants (Act 3)
| Variant | Notes |
|---------|-------|
| `Viral Node` | Inserts 2 Glitches per turn; minimal attacks |
| `Deep Signal Hacker` | Inserts Viral Code; forces deck purity test |

---

### The Saboteur
**Role**: Directly interacts with your board. Forces rerouting decisions.

Behavior pattern:
- Telegraphs specific module slot attacks: "Disrupt Slot 2 for 2 turns"
- Forces the player to route energy differently on the fly
- Sometimes "steals" partial charge from a module (energy evaporates)
- Acts like a puzzle: plan around the slot being disabled before it happens

#### Human Faction Variants (Acts 1–2)

| Variant | Faction | Act | Notes |
|---------|---------|-----|-------|
| `Salvage Stripper` | Scavengers | Act 1 | Disables 1 slot for 1 turn; weak attacks |
| `Imperial Tech Disruptor` | Imperial Border Guard | Act 2 | Targets highest-charged module; moderate HP |
| `Reach Saboteur` | The Free Reach | Act 2 | Disables 2 slots alternately; wins through engine starvation |
| `Overload Drone` | Unknown | Act 1+ | Targets most-charged module, triggers it prematurely |

#### Unknown Contact Variants (Act 3)
| Variant | Notes |
|---------|-------|
| `Grid Corruptor` | Disables 2 slots alternately; no attacks |
| `Module Jacker` | Disables 1 slot, attacks weakly — Act 3 standard variant |

---

### The Swarmer (Elite Mechanic)
**Role**: Multi-enemy encounter. Forces damage prioritization.

Appears as 2–3 smaller enemies simultaneously. Each has its own HP and intent. The player must choose which to target with each module fire.

Behavior:
- Individual enemies are weak but their intents add up
- Some Swarmers buff each other if not killed fast enough
- Killing the "leader" unit ends the encounter even if others survive (Act 2+)

#### Human Faction Variants (Acts 1–2)

| Variant | Faction | Act | Notes |
|---------|---------|-----|-------|
| `Pirate Wing` | Rim Pirates | Act 1 | 3 units; one is the leader |
| `Militia Patrol` | Border Militia | Act 1–2 | 2 units; both have medium HP |
| `Reach Cell` | The Free Reach | Act 2 | 3 units; leader buffs others if alive after turn 2 |

#### Unknown Contact Variants (Act 3)
| Variant | Notes |
|---------|-------|
| `Fragment Cluster` | 3 units, leader has self-repair mechanic |

---

### The Armored
**Role**: Tests burst damage / Penetration builds.

Has a large Shield value that regenerates every other turn. Attacks are moderate. The real challenge: you must deal more damage than their Shield recovery rate, or the fight goes infinite. Rewards having Penetrating weapons (ignore Shield) or burst damage combos.

#### Human Faction Variants (Acts 1–2)

| Variant | Faction | Act | Notes |
|---------|---------|-----|-------|
| `Imperial Heavy` | Imperial Border Guard | Act 2 | High shield regeneration; methodical attacks |
| `Fortified Hauler` | Scavengers | Act 1 Elite | Repurposed mining armor; slower but hits hard |

#### Unknown Contact Variants (Act 3)
| Variant | Notes |
|---------|-------|
| `Hardened Shell` | Full Core Corruption armor; maximum shield regen |

---

## Faction Enemy Pool by Act

Enemy types are fixed to act, not a meta counter. The world doesn't change between docks — what changes is how deep you go.

| Act | Enemy Pool |
|-----|-----------|
| Act 1 — The Outer Belt | Rim Pirates, Scavengers, Border Militia |
| Act 2 — The Imperial Frontier | Imperial Border Guard, Free Reach Forces |
| Act 3 — The Galactic Core Approaches | Unknown contacts (Static-origin) |

*Unknown contacts are never named "The Static" in UI text before Act 3. Use "anomalous contact," "unknown origin," or "unregistered vessel."*

---

## Bosses

### Act 1 Boss: The Sentinel
*An abandoned Imperial drone network, left behind when the mining operation was shuttered. The automated defense protocol never received a stand-down order. It has been running for eleven years.*

**HP**: 220 (single phase)
**Archetype**: Bruiser/Armored hybrid

**Behavior**:
- Attacks for 18 every turn
- Every 3 turns: "Lockdown Protocol" — Gain 40 Shield
- At 50% HP: "Escalation" — attacks increase to 26, also gains 10 Shield per hit taken

**Challenge**: The Shield cycling makes pure damage builds inefficient. Players learn early to value either Penetration effects or burst-and-wait timing.

**Lore**: The Sentinel isn't evil. It's doing exactly what it was designed to do. The tragedy is that no one ever came back to turn it off.

---

### Act 2 Boss: The Architect
*A contested megafreighter — too large to be a single vessel, it functions as a mobile structure. Both the Imperial Border Guard and the Free Reach claim ownership. Neither crew is fully in control. The AI governing its logistics systems has been running on conflicting command authorities for two years. It has found its own resolution.*

**HP**: 180 / 180 (two phases)

**Phase 1 behavior**:
- Hacker intent: inserts 1 Glitch per turn
- Attacks for 12 every other turn
- "Restructure" at 50% HP: announces it is entering Phase 2

**Phase 2 behavior** — Critical mechanic:
- The grid layout of **the player's ship** temporarily shifts — 1-2 Bridge slots are disabled, and 1 new "corrupted" module slot appears (fires for the enemy if you slot energy into it)
- This forces an on-the-fly reroute of your engine
- Attacks increase to 20, inserts 2 Glitches per turn

**Challenge**: Teaches adaptability — your engine can't be a rigid chain that breaks when one piece is removed.

**Lore**: The Architect's behavior looks like something you've heard about in rumors. But it's not that. It can't be. It's just a broken AI on a disputed freighter. Probably.

---

### Act 3 Boss: The Signal
*The convergence point of what's been building in the deep Rim. It is not a ship. It is a broadcast that has given itself a body. This is the first time a player confronts The Static directly and by name.*

**HP**: 150 / 150 / 150 (three phases, each has a different archetype)

**Phase 1 — The Hacker**: Aggressively inserts Glitch cards. Attacks weakly. Tests deck purity.
**Phase 2 — The Bruiser**: Pure escalating damage. No Glitch insertion. Tests sustain.
**Phase 3 — The Saboteur**: Disables module slots each turn. Also attacks. Tests maximum flexibility.

**Phase transition effect**: Each phase ends with a brief "signal burst" — clears all Shields on both sides, removes all Glitch cards from hand (mercy mechanic), fully heals The Signal for the next phase.

**Challenge**: This is a comprehensive test of everything the game has taught. No single-axis build wins cleanly.

---

### Secret Boss: The Integrated
*A former human crew, fully assimilated. They fight with terrifying precision because they still have human instincts — now running on something else's priorities.*

**HP**: 300 (two phases, HP carries over)
**Unlock**: Reach a hidden node on the Act 3 map (discovered through specific event chains — The Quiet Frequency chain, run 5)

**Unique mechanic**: The Integrated **counters your strongest module**. At the start of combat, it identifies your highest-damage module and applies a permanent "Adaptive Shield" specifically against that module's energy type. Forces the player to use a secondary build strategy.

**Reward**: Unique lore entry, unique card (non-craftable elsewhere), Void Crystal ×3.

---

## Enemy Scaling by Act

| Property | Act 1 | Act 2 | Act 3 |
|----------|-------|-------|-------|
| Normal HP | 40–80 | 70–120 | 100–160 |
| Elite HP | 100–140 | 140–200 | 180–260 |
| Attack values | 8–16 | 14–22 | 18–30 |
| Glitch insert rate | 1 per 2 turns | 1 per turn | 2 per turn |
| Slot disruption duration | 1 turn | 1–2 turns | 2–3 turns |
