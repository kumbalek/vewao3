# Cards (Software)

## Overview

Cards are the tactical layer of combat — the fuel and logic injected into your ship's modules to make them run. They represent the crew's commands and the reactor's momentary output. **Cards don't directly attack enemies. They power modules that do.**

A deck is built over the course of a mission through combat rewards, shops, and events. It resets each run.

---

## Card Categories

### 1. Raw Energy
Pure fuel. Slot it into a module to contribute toward that module's cost.

No secondary effect — just energy. The backbone of any deck.

**Rarity breakdown**: Most commons are Raw Energy. They are not glamorous but they are necessary.

Examples:
| Card | Effect |
|------|--------|
| Red Cell | Generates 1 Red energy |
| Dense Blue | Counts as 2 Blue energy |
| EM Burst | Generates 1 Yellow energy |
| Prismatic Shard | Generates 1 energy of any color |
| Overcharge | Generates 2 Red, but adds 5 Heat |
| Cold Tap | Generates 1 Blue, remove 3 Heat |
| Void Sliver | Generates 1 Purple (rare) |

### 2. Commands
Commands are played directly from hand for an immediate effect. They **do not slot into modules**. They manipulate the board, deck, or turn state.

Examples:
| Card | Effect |
|------|--------|
| Flush | Empty one module's partial charge, draw 2 cards |
| Overclock | A chosen module fires twice next time it's triggered this turn |
| Patch | Remove 1 Glitch card from hand |
| Salvage | Look at the top 3 cards of discard, put 1 in hand |
| Emergency Vent | Remove 30 Heat. Discard 2 cards. |
| Reroute | Move all energy from one module to another |
| Cascade Boost | Next bridge trigger deals double effect |
| Tactical Scan | Reduce all enemy intent values by 20% this turn |

### 3. Hybrids (Sub-Routines)
The most flexible and interesting card type. Hybrids have a **dual-use**: they can be played as Raw Energy OR activated for a specific conditional effect.

The choice is made at the time of play. Once slotted as energy, the effect is not available. Once used as a Command, the energy is not generated.

This creates the most interesting turn-by-turn decisions in the game.

Examples:
| Card | As Energy | As Effect |
|------|-----------|-----------|
| Targeting Virus | 1 Yellow | Apply Vulnerable to enemy if slotted into a Weapon module |
| Coolant Spike | 1 Blue | Remove 15 Heat if played as Command |
| Surge Conduit | 1 Red | If a Weapon fired this turn, generate +1 Red in its slot |
| Bio-Loop | 1 Green | Heal 4 HP if you have a full Green module slot |
| Void Tap | 1 Purple | Discard hand, draw 5 new cards |
| Relay Pulse | 1 any | Copy the last energy card played this turn |

### 4. Status / Glitch Cards (Negative)
These clog your hand. They are forced into your deck by enemies (Hackers), events, or some choice rewards. They must be purged by deliberately routing them through a dedicated Purge module, or removed at a Shop.

You cannot simply discard them by choice — they stay in hand until played or until turn end discards them (at which point they still trigger their "on discard" effect).

**Glitch card types**:
| Card | On Slot | On Discard / Unplayed |
|------|---------|----------------------|
| Static Noise | Nothing (dead slot) | Nothing |
| Overload | Deal 8 damage to yourself | Nothing |
| Malware | — | Take 4 damage |
| Short Circuit | Counts as 1 Red, fires a random module | Nothing |
| Feedback Loop | Fires current module but adds 20 Heat | Adds 10 Heat |
| Viral Code | Spawns another Glitch card in discard | Spawns another Glitch card in discard |

---

## Card Rarity

| Rarity | Availability | Visual |
|--------|-------------|--------|
| Common | Always in pool | Gray pip |
| Uncommon | After Act 1 | Blue pip |
| Rare | After Act 2, or discovered | Gold pip |
| Signature | Ship-specific, 1 copy | Ship-color border |

Rare cards must be **discovered** — encountered at least once in a run — before they appear in subsequent shop pools. This gives experienced players expanding card pools as they dive deeper.

---

## Deck Management

**Starting deck**: 10 cards (5 Raw Energy, 3 Command, 2 Hybrid). Composition varies by ship.

**Max deck**: 25 cards. After 25, adding a card requires removing one. (Shops offer removal service.)

**Upgrades**: Most cards have a `+` upgrade version unlockable at Shipyard nodes:
- Raw Energy+: +1 extra energy or removes self from deck after use (Exhaust)
- Command+: Enhanced effect or reduced cost
- Hybrid+: Both effects trigger simultaneously (no longer a choice)

**Exhaust keyword**: Some cards have `[Exhaust]` — they are removed from the deck after one use per combat. Powerful but limited.

**Retain keyword**: Some cards have `[Retain]` — they are NOT discarded at end of turn and stay in hand.

---

## Card Design Philosophy

1. **Every card should create a decision** — even Raw Energy has placement decisions (which module gets it?)
2. **Hybrids are the design space** — the richest interactions come from Hybrid flexibility
3. **Glitches should be annoying, not impossible** — always purge-able, never run-ending alone
4. **Cards interact with the board** — a card's value changes depending on what modules are installed. A "Red energy" card is useless if you have no Red modules. This ties deck and board synergy together.
