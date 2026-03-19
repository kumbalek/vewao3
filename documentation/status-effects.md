# Status Effects

## Overview

Status effects are persistent state modifiers attached to either the player, enemy, or specific modules. They tick, decay, or trigger at defined points in the turn cycle. Understanding status management is core to intermediate play.

---

## Heat System

Heat is the most important status in the game — it's the player's primary resource constraint beyond card draw.

### Heat Sources
- **Red (Thermal) energy** cards generate Heat passively — each Red card played adds 5 Heat
- **Overload Glitch cards** generate Heat when slotted
- **Overcharge Bridge** adds Heat in exchange for power
- **Some enemy attacks** ("Plasma Vent" intent: +20 Heat)
- **Overcharge card** (Raw Energy): generates 2 Red but +10 Heat

### Heat Thresholds
| Heat Level | Effect |
|-----------|--------|
| 0–49 | No penalty |
| 50–79 | Warning state: discard 1 extra card at end of turn |
| 80–99 | Danger state: discard 2 extra cards at end of turn + screen red vignette |
| 100 | MELTDOWN: take 8 damage at start of each turn until below 80 |

### Heat Decay
- Heat decays by 5 at the start of each player turn (natural cooling)
- Blue (Cryo-Plasma) energy cards reduce Heat by 8 each when slotted
- `Coolant Core` module reduces Heat by 25 when fired
- `Emergency Vent` Command card removes 30 Heat (costs 2 card discards)
- `Heat Sink` Bridge converts Weapon-generated Heat into Shield (doesn't remove it, redirects it)

### Heat Design Intent
Heat is a **resource tension dial** on Red-heavy builds. A pure weapons build is powerful but requires Blue cards or cooling modules to sustain. The interesting design space is in the tension between maximum damage output (generates Heat) and thermal management (requires Blue cards that "waste" weapon slots).

---

## Shields

Shields are temporary HP. All incoming damage hits Shields first, then HP.

### Shield Properties
- Shields absorb damage point-for-point
- Shields do NOT carry over between turns by default (expire at start of enemy turn)
- Exception: `Ablative Layer` module has "persistent" Shields that last until broken
- Shields from `Reactive Plating` have a thorn effect: enemy takes 3 damage when they hit through them

### Shield Stacking
Multiple Shield sources in one turn stack additively. A `Cryo-Shield Array` (18 Shield) + `Reactive Plating` (10 Shield) = 28 total Shield before expiry.

### Shield Upgrades
Station and module upgrades can:
- Carry over N Shield points to next turn
- Refresh Shields at a set value each turn (regen effect)
- Convert excess Shield (after full HP) into attack damage (overload mechanic)

---

## Glitch Cards

Glitch cards are negative Software forced into your deck by enemy Hackers, events, or some high-risk rewards. They occupy hand slots and provide no benefit — or actively harm you.

### Glitch Types

| Card | When Slotted | When in Hand at End of Turn |
|------|-------------|---------------------------|
| **Static Noise** | Nothing (dead slot — wastes a slot's turn) | Nothing |
| **Overload** | Deal 8 damage to self | Nothing |
| **Malware** | Nothing | Take 4 damage (triggers on discard) |
| **Short Circuit** | Counts as 1 Red BUT fires a random module | Nothing |
| **Feedback Loop** | Fires current module but generates +20 Heat | Generates +10 Heat on discard |
| **Viral Code** | Nothing | Inserts another Viral Code into discard pile |

### Glitch Removal Methods
- **Purge Port** module: slot any Glitch card to purge it, draw 1 card per purge
- **Shop**: pay 40 Scrap to remove any 1 card from deck
- **Signal Bridge** (passive): auto-purges Glitch cards that enter play from it
- **Phase Barrier** module: blocks next Glitch insertion
- **Patch** Command card: remove 1 Glitch from hand

### Glitch Design Intent
Glitches are an attrition mechanic — they don't usually end a run on their own, but they degrade performance over time. A deck with 5+ Glitch cards draws consistently bad hands. The player must proactively manage them or bring a Purge Port module to handle the accumulation.

---

## Enemy Debuffs (Applied to Enemy)

| Status | Effect | Applied By |
|--------|--------|-----------|
| **Vulnerable** | Take 50% more damage from next hit, then expire | Scatter Shot weapon, Targeting Virus hybrid |
| **Disrupted** | Next intent is reduced by 50% | EM Disruptor weapon |
| **Slowed** | Next attack intent reduced by flat 8 | Cryo Bolt weapon |
| **Burning** | Take 5 damage at start of their turn (stacks) | Special modules (Act 2+) |
| **Jammed** | Cannot execute "Insert Glitch" intents for 1 turn | Signal Jammer utility |
| **Weakened** | Deal 25% less damage for 2 turns | Some event rewards |

---

## Player Buffs (Applied to Player)

| Status | Effect | Source |
|--------|--------|--------|
| **Focused** | Next module that fires does so twice | Command card / rare module |
| **Supercharged** | Next energy card played counts as 2 energy | Rare Hybrid card |
| **Locked-In** | A specific module auto-fires at end of turn regardless of energy | Bridge upgrade |
| **Momentum** | Each consecutive module fire this turn deals +2 damage | Cascades build this |
| **Reinforced** | Shields don't expire this turn | Ablative Layer module |
| **Adaptive** | After taking damage, generate 1 Green energy for free | Rare module |

---

## Module Status Effects

Modules themselves can have status applied (by Saboteur enemies or some events):

| Status | Effect |
|--------|--------|
| **Disrupted** | Module slot cannot accept energy cards for N turns |
| **Overloaded** | Module fires at 50% effectiveness |
| **Corrupted** | Module generates Heat every time it fires (even non-Red) |
| **Boosted** | Module effect increased by 30% for 2 turns (positive) |

---

## Status Display

All active status effects are displayed:
- **Player statuses**: icon strip in the top HUD bar (next to HP and Heat)
- **Enemy statuses**: icon strip below the enemy HP bar
- **Module statuses**: icon overlay directly on the affected module slot in the grid
- **Hover tooltip**: any status icon can be hovered for full description and remaining duration
