# Enemies

## Design Philosophy

Enemies don't just chip away at HP — they attack your **engine**. Every archetype is designed to stress a different aspect of your build:

- **Bruisers** test your damage output and Shield generation speed
- **Hackers** test your deck quality and Glitch management
- **Saboteurs** test your routing flexibility and board adaptability
- **Swarmer** types test your ability to prioritize and finish quickly

All enemies **telegraph their intent every turn**. No hidden information. The game is a planning puzzle, not a reaction test.

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
**Stage of Corruption**: Integration
**Role**: High sustained damage. Forces efficient Shield generation and fast DPS.

Behavior pattern:
- Alternates between heavy attacks and brief recovery turns
- Attacks escalate in damage each cycle ("Escalating Protocol")
- Occasionally taunts: grants itself Shield to protect from burst damage
- Does not interact with your deck or board — purely tests HP sustain

Example variants:
- `Heavy Scout` — basic Bruiser, Act 1
- `Assault Carrier` — high HP, hits for 20+ per turn, Act 2
- `Siege Platform` — massive HP pool, telegraphs multi-hit combos, Act 2 Elite

### The Hacker
**Stage of Corruption**: Drift (advanced)
**Role**: Disrupts deck quality. Forces Glitch management and Purge module usage.

Behavior pattern:
- Every 2 turns: "Insert N Glitch cards into your deck (shuffled in)"
- Glitch cards inserted match the level: Act 1 inserts Static Noise, Act 3 inserts Viral Code
- Attacks are weak — the real threat is the growing Glitch payload in your deck
- If not killed before a threshold turn, begins inserting 2 Glitches per turn instead of 1

Example variants:
- `Signal Corruptor` — Act 1 Hacker, inserts 1 Static Noise every 2 turns
- `Viral Node` — Act 2 Hacker, inserts Malware; has self-shield mechanic
- `Cascade Infector` — Elite, inserts 1 Glitch per turn AND attacks moderately

### The Saboteur
**Stage of Corruption**: Integration (specialized)
**Role**: Directly interacts with your board. Forces rerouting decisions.

Behavior pattern:
- Telegraphs specific module slot attacks: "Disrupt Slot 2 for 2 turns"
- Forces the player to route energy differently on the fly
- Sometimes "steals" partial charge from a module (energy evaporates)
- Acts like a puzzle: plan around the slot being disabled before it happens

Example variants:
- `Module Jacker` — disables 1 slot for 1 turn, attacks weakly
- `Grid Corruptor` — disables 2 slots alternately, no attacks (wins through engine starvation)
- `Overload Drone` — targets your most-charged module and triggers it prematurely (early fire, wastes charge)

### The Swarmer (Elite Mechanic)
**Stage of Corruption**: Drift
**Role**: Multi-enemy encounter. Forces damage prioritization.

Appears as 2–3 smaller enemies simultaneously. Each has its own HP and intent. The player must choose which to target with each module fire.

Behavior:
- Individual enemies are weak but their intents add up
- Some Swarmers buff each other if not killed fast enough
- Killing the "leader" unit ends the encounter even if others survive (Act 2+)

### The Armored
**Stage of Corruption**: Core Corruption
**Role**: Tests burst damage / Penetration builds.

Has a large Shield value that regenerates every other turn. Attacks are moderate. The real challenge: you must deal more damage than their Shield recovery rate, or the fight goes infinite. Rewards having Penetrating weapons (ignore Shield) or burst damage combos.

---

## Bosses

### Act 1 Boss: The Sentinel
*A guardian drone network left behind to defend the Debris Field. Originally built to protect mining operations — now protecting only The Static.*

**HP**: 220 (single phase)
**Archetype**: Bruiser/Armored hybrid

**Behavior**:
- Attacks for 18 every turn
- Every 3 turns: "Lockdown Protocol" — Gain 40 Shield
- At 50% HP: "Escalation" — attacks increase to 26, also gains 10 Shield per hit taken

**Challenge**: The Shield cycling makes pure damage builds inefficient. Players learn early to value either Penetration effects or burst-and-wait timing.

---

### Act 2 Boss: The Architect
*A corrupted megafreighter. Too large to be a single vessel — it's a mobile structure. It **restructures itself** mid-fight.*

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

---

### Act 3 Boss: The Signal
*The convergence point of The Static in this sector. It is not a ship. It is a broadcast that has given itself a body.*

**HP**: 150 / 150 / 150 (three phases, each has a different archetype)

**Phase 1 — The Hacker**: Aggressively inserts Glitch cards. Attacks weakly. Tests deck purity.
**Phase 2 — The Bruiser**: Pure escalating damage. No Glitch insertion. Tests sustain.
**Phase 3 — The Saboteur**: Disables module slots each turn. Also attacks. Tests maximum flexibility.

**Phase transition effect**: Each phase ends with a brief "static burst" — clears all Shields on both sides, removes all Glitch cards from hand (mercy mechanic), fully heals The Signal for the next phase.

**Challenge**: This is a comprehensive test of everything the game has taught. No single-axis build wins cleanly.

---

### Secret Boss: The Integrated
*A former human crew, fully assimilated. They fight with terrifying precision because they still have human instincts — now amplified by The Static.*

**HP**: 300 (two phases, HP carries over)
**Unlock**: Reach a hidden node on the Act 3 map (discovered through specific event chains)

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
