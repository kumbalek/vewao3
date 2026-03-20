# Macro Game — Missions & Station

## Overview

The macro game is the roguelike layer: node-based mission maps launched from your permanent space station. Between missions, you upgrade the station and prepare for the next dive. The station persists forever. **On a safe return, your ship persists too — deck and modules intact.** Only death resets the ship.

---

## The Mission Loop

1. **At The Stead** — review station upgrades; board your existing ship (or a fresh one if you died last run); choose waypoint re-entry or fresh start
2. **Launch** — enter the mission map (node-based star chart), spending 1 Drive Fuel
3. **Navigate** — choose your path through 3 acts of nodes
4. **Combat, loot, events** — build your engine as you dive deeper
5. **Decide when to return** — you can abort at any non-combat node to keep your loot *and* your ship
6. **Succeed or die** — complete Act 3 boss for full rewards, or die and lose your ship (but not your station)

---

## Mission Maps

### Structure
- **3 Acts** per full mission
- The map is a **branching node graph** — player sees all paths ahead and chooses their route
- Each act ends with a Boss node (mandatory)
- Act transition nodes: brief text vignette + auto-save checkpoint

### Act Sizes
| Act | Nodes | Elites | Boss |
|-----|-------|--------|------|
| Act 1 | 7–9 | 1 | 1 |
| Act 2 | 9–11 | 2 | 1 |
| Act 3 | 11–13 | 3 | 1 |

### Path Design Rules
- At least 2 branching choices at every fork
- No path forces more than 2 consecutive Combat nodes
- Each act always contains: at least 1 Shop, 1 Event, 1 Shipyard
- Elite nodes are visible on the map but path to them is optional

---

## Node Types

### Combat
**Frequency**: ~45% of nodes
**Reward**: Scrap (10–30) + choice of 3 cards

Standard encounters against Normal tier enemies. The baseline of a run.

### Elite Combat
**Frequency**: ~10% of nodes
**Reward**: Scrap (25–45) + choice of Module or Bridge + possible Ship Upgrade

Elite enemies are significantly harder than normal encounters. The reward justifies the risk. Skip them when your build is fragile; seek them when you're strong.

### Event (Distress Signal / Anomaly)
**Frequency**: ~15% of nodes
**Reward**: Varies — can be loot, HP, cards, modules, or nothing

Text-based encounters with 2–3 choices. Risk/reward. No combat. Some events are part of ongoing narrative chains that continue across missions.

See `map-and-missions.md` for full event catalog.

### Shop / Black Market
**Frequency**: ~12% of nodes
**Reward**: Purchase opportunity

Inventory: 3–5 cards, 1–2 modules, card removal service, 1 random upgrade.
Black Market variant: more rare items, higher prices, possible illegal/unstable goods.

### Shipyard
**Frequency**: ~8% of nodes
**Reward**: Hardware modification

The only place in a mission where you can:
- **Rearrange** module positions on the grid
- **Install** new modules from a selection of 3
- **Upgrade** existing modules (`+` version, costs Scrap)
- **Remove** modules (recover some Scrap)

Shipyard visits are precious. Plan your grid rearrangements before reaching one.

### Rest / Repair Beacon
**Frequency**: ~5% of nodes
**Reward**: Choice of HP recovery OR card upgrade

- **Repair**: Heal 25% of max HP
- **Calibrate**: Upgrade any 1 card in your deck to `+` version

Can only choose one.

### Hidden / Fog Node
**Frequency**: ~5% of nodes (requires Archives station upgrade to pre-reveal)
**Reward**: High variance — rare items or ambush combat with big reward

Unknown until you land on them. Can be very good or a trap.

---

## The Return Decision

At any non-combat node, the player can choose **"Return to The Stead"**:
- All Scrap and items collected this mission are kept
- **The ship is preserved** — deck and board carry over to the next mission
- A waypoint is saved at the last completed act boundary

**If you die**: Mind-Save activates at The Stead. Your ship is **lost** — fresh ship on next run. All in-run loot is lost.

**The tension**: Deeper acts have better loot and more story. The risk is losing your ship — and the engine you've built across multiple dives. When do you bank your gains vs. press for more?

---

## Waypoint System

On safe dock, the game saves your last completed act boundary as a **waypoint**.

On re-launch from The Stead, you choose:

- **Fresh Start** — Begin at Act 1. Full map. No penalty. Your ship and deck are unchanged, but you start from the beginning.
- **Continue from Waypoint** — Skip directly to where you left off. First 2 encounters in that region have **+20% enemy HP** ("Faction Alert" — they had time to prepare for your return).

Mid-act retreating has a cost: if you bail before clearing an act boundary, your waypoint still points to the start of that act. You'll either restart fresh or face prepared enemies on return. Players who complete an act cleanly before docking avoid the Faction Alert entirely.

---

## Drive Fuel

Each mission launch costs **1 Drive Fuel** — representing the ship's ability to make a long-range jump out to the Rim and back. Fuel regenerates over time and is stored server-side.

| | Free Player | Full Game |
|--|------------|-----------|
| Max Fuel | 5 | Unlimited |
| Regeneration | 1 per 2 hours | — |
| Refill time | ~10 hours | — |

**Full game purchase** removes the cap entirely. No fuel limits, no waiting. This is the only thing the purchase unlocks — it is not pay-to-win. Free players have full access to all content; they simply play fewer sessions per day.

**Why this structure works as a depth incentive**: Free players get 5 launches per day. Spending all 5 on quick Act 1 clears is obviously worse than 2 well-executed deep runs. The system nudges players toward depth without mandating it. A player who genuinely wants to grind Act 1 can — they'll just have less fuel to spend.

**Fuel is not lost on death.** Only the ship and loot are lost. Dying is already punishing enough.

---

## Space Station: The Stead

The Stead is your permanent base. It never resets. It grows with every successful mission. The docking clamps on Bay 3 still rattle on hard docks — you've fixed them twice and they always come loose again.

### Core Rooms (always present, cannot be lost)

| Room | Function |
|------|----------|
| Command Deck | Mission map access. Lore log. Crew conversations. |
| Hangar Bay | Ship selection. View/inspect all owned ships. |
| Mind-Save Pod | Consciousness backup. Tier affects loot recovery on death. |
| Reactor Core | Station power budget. Limits total active modules. |

### Buildable Station Modules

Built with **Scrap Metal** and **Data Cores** (meta currencies).

| Module | Tier 1 Effect | Tier 2 Effect | Cost |
|--------|--------------|--------------|------|
| Workshop | Upgrade 1 card before mission | Upgrade 2 cards before mission | 80 Scrap |
| Archives | Reveals full node map on start | Also reveals Hidden nodes | 60 Scrap + 1 Core |
| Medical Bay | Start mission +15 max HP | Also regenerate 8 HP after Elite combat | 70 Scrap |
| Training Grounds | Start with +1 card in opening hand | Opening hand of 7 cards | 50 Scrap |
| Black Market Contact | +2 items in shops | 20% discount on all shop items | 90 Scrap + 1 Core |
| Research Lab | See enemy type on map before engagement | See Elite enemy intents before entering node | 100 Scrap + 2 Cores |
| Fabricator | Craft 1 specific card before mission | Also craft 1 specific module | 120 Scrap + 2 Cores |
| Beacon Tower | Unlocks Rescue nodes on map (survivor reward) | Rescued crew provides permanent passive buff | 80 Scrap + 1 Core |
| Void Scanner | Reveals 1 hidden region per mission | Unlocks secret faction event chains | 150 Scrap + 3 Cores |
| Engine Tuner | Start mission with 1 Bridge pre-installed | Bridge has a free upgrade | 110 Scrap + 2 Cores |
| Armory | Start with 1 extra module slot | Module slot comes pre-fitted | 200 Scrap + 3 Cores |
| Smuggler's Den | Access to a hidden shop mid-mission once per run | Shop has unique contraband items | 90 Scrap + 1 Core |
| Diplomatic Contacts | Access to faction shop nodes (Act 1+); faction-specific discounts | Faction rep persists across docks; deeper discounts with allied faction | 100 Scrap + 2 Cores |
| Deep Space Relay | Waypoint depth extended: return from mid-Act 2 possible | Faction Alert penalty on waypoint re-entry reduced to +10% HP | 130 Scrap + 2 Cores |

### Station Power Budget
The Reactor Core limits how many station modules are *active* simultaneously. You can build more modules than you can power — you must choose which to activate before each mission. This creates meaningful pre-mission decisions.

---

## Meta Currencies

| Currency | Source | Use |
|----------|--------|-----|
| Scrap Metal | Combat drops, events, selling modules | Station upgrades, basic shop items |
| Data Cores | Elite/boss rewards, special events | Advanced station upgrades |
| Void Crystals | Boss kills, secret boss, rare events | Endgame unlocks, new ship unlocks |

---

## Mission Difficulty (Ascension System)

After completing your first full mission, Ascension tiers unlock — optional difficulty modifiers for experienced players.

| Ascension | Added Challenge |
|-----------|----------------|
| 1 | Elite enemies have +20% HP |
| 2 | One fewer Shop per mission |
| 3 | Boss gains a new ability |
| 5 | Start each run with 2 Glitch cards in deck |
| 7 | Heat builds 25% faster |
| 10 | Elites have unique buffed variants |
| 15 | All enemies have additional intents |
| 20 | "Developer Mode" — max challenge, no assists |

Ascension is per-ship. Completing an Ascension level unlocks cosmetic rewards and sometimes unique cards.
