# Macro Game — Missions & Station

## Overview

The macro game is the roguelike layer: node-based mission maps launched from your permanent space station. Between missions, you upgrade the station and prepare for the next dive. The station persists forever. Ships and loot do not.

---

## The Mission Loop

1. **Plan at The Anchorage** — review station upgrades, select a ship, review deck/board from last run (fresh run if first or after death)
2. **Launch** — enter the mission map (node-based star chart)
3. **Navigate** — choose your path through 3 acts of nodes
4. **Combat, loot, events** — build your engine as you dive deeper
5. **Decide when to return** — you can abort a mission at any non-combat node to keep your loot
6. **Succeed or die** — complete Act 3 boss for full rewards, or die and lose everything except station progress

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

At any non-combat node, the player can choose **"Return to Anchorage"**:
- All Scrap and items collected this mission are kept
- The mission progress resets (you'll start from Act 1 on next mission)
- No death penalty — you keep everything

**If you die**: Mind-Save activates at the Anchorage. All loot from the mission is lost. Station progress is untouched.

**The tension**: Deeper acts have better loot. But the risk of dying (and losing everything) increases. When do you bank your gains?

**Risk modifiers**:
- Station upgrade: Mind-Save Pod tier affects how much loot survives on death (0% → 10% → 25%)
- Some events offer "Dead Man's Cache" — stash loot to survive a death

---

## Space Station: The Anchorage

The Anchorage is your permanent base. It never resets. It grows with every successful mission.

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
