# Combat System (Micro Game)

## Overview

Combat is turn-based, deterministic, and revolves around **spatial resource management on your ship's module grid**. Enemy intents are always telegraphed — there is no hidden information. Victory comes from reading the board, routing energy efficiently, and building automated cascade chains.

The core tension: you have limited cards in hand and must decide how to distribute energy across competing modules while managing Heat, Shields, and incoming threats simultaneously.

---

## Turn Structure

### Phase 0: Start of Turn
- Persistent status effects tick (Heat decays slightly, Shields from last turn expire unless held by an upgrade)
- Any "start of turn" module passives trigger
- Enemy intent for the **next** turn is revealed and updated

### Phase 1: Draw
- Player draws 5 cards from their Software deck
- If deck is empty, shuffle discard pile into new deck
- Glitch cards in deck may appear in hand — they cannot be avoided

### Phase 2: The Routing Phase (Player Turn)
This is the core gameplay loop:

**Drag energy cards from hand into module slots on the Ship Grid.**

Rules:
- A card dropped into a module slot stays there; it does not "spend" and disappear immediately
- Once a module's energy cost is **fully met** (e.g., `[Red][Red]`), it fires immediately
- Partial energy stays in the slot and accumulates across card plays (within one turn)
- You can fill multiple modules in any order
- Command cards are played directly from hand for an immediate effect without slotting

**Cascade Chains**: Bridge modules trigger automatically when adjacent modules fire. For example, a Splitter bridge between two weapon slots will duplicate one energy card into both slots when either weapon fires. Cascades can chain multiple modules in sequence.

**Resolving a cascade**: Each triggered module in the chain resolves sequentially (left-to-right, top-to-bottom as a tiebreaker). Animations play out the chain so the player can follow it.

### Phase 3: End Turn / Cleanup
- Player presses "End Turn"
- Unspent energy cards remaining in hand are discarded
- Any energy partially slotted into modules that didn't fully fire **remains in the slot** (persists to next turn by default — some modules purge on turn end)
- Modules that fired this turn reset and are ready for next turn (unless Disrupted)

### Phase 4: Enemy Turn
- Enemies execute the intent they telegraphed at the start of the player's turn
- Attacks deal damage to HP (reduced by Shields first)
- Special attacks: Hackers insert Glitch cards into deck, Saboteurs disable module slots
- Some enemies have multiple intents per turn (elite/boss tier)

### Phase 5: Status Resolution
- Heat ticks: if above threshold, apply heat damage
- Status durations tick down
- Shield values carry over only if the player has the relevant upgrade

---

## The Ship Grid

The grid is the physical layout of your ship's modules — a visual cross-section of the vessel. Module slots are positioned where they make thematic sense on the ship illustration (weapons at hardpoints, shields at hull edges, engine modules at the rear).

**Grid Properties**:
- Each ship has a unique starting layout (4–6 initial slots)
- Shipyard nodes mid-mission let you add new slots or rearrange existing ones
- Slots have a fixed **position** which matters for bridge module adjacency

**Slot States**:
- `Empty` — no module installed; cards can't be routed here
- `Active` — has a module; accepts energy cards matching its type
- `Charging` — partially filled with energy; waiting for more
- `Firing` — mid-animation after cost met
- `Disrupted` — disabled by enemy action; cannot accept cards for N turns
- `Overloaded` — took structural damage; fires at reduced effectiveness

---

## Energy Routing Mechanics

**The Slotting Rule**: Energy stays in a module until its cost is fully met. You can spread energy placement across your hand — no need to complete a module in one card play.

**Multi-slot routing example**:
- Module A: `[Red][Red]` — you play one Red card on turn 3, and another on turn 4. It fires on turn 4.
- This creates interesting decisions: do you "pre-charge" a powerful weapon over multiple turns?

**Energy type matching**: Modules require specific energy types. A Thermal Cannon requires `[Red]` cards. Cryo-Shield requires `[Blue]`. Hybrid modules accept two types (e.g., `[Red] or [Yellow]`).

**Wild energy**: Some rare cards generate `[Any]` energy — the slot accepts them regardless of module type.

---

## Cascade Chain System

Bridges are passive modules that sit between active modules and trigger automatically based on flow events.

**Trigger conditions**:
- `When [Module X] fires → do Y`
- `When any Weapon fires → do Y`
- `On turn end, if [condition] → do Y`

**Bridge types** (detailed in ship-and-modules.md):
- Splitter: duplicates energy into two adjacent slots
- Relay: transfers remaining energy from fired module to another slot
- Heat Sink: converts excess heat from a fired weapon into Shield
- Amplifier: next module fired this turn deals +50% effect
- Loop Bridge: if the same module fires twice in one turn, trigger an extra effect

**Chain resolution order**: Bridges resolve in placement order (top-to-bottom, left-to-right). If a bridge triggers another module, that module may trigger more bridges — chains can be very long. The game caps chains at 12 triggers per turn to prevent runaway loops.

---

## Win/Loss Conditions

**Victory**: Reduce enemy HP to 0.

**Defeat**: Player HP reaches 0. Mission ends — Mind-Save activates, loot is lost.

**Escape**: Some combat encounters have an "Escape" option. Costs Scrap or HP. Not all encounters allow it.

---

## Combat Rewards

- **Normal Combat**: Scrap (currency) + choice of 3 cards (Software reward)
- **Elite Combat**: Scrap + choice of Module or Bridge (Hardware reward) + possible Ship Upgrade
- **Boss Combat**: Scrap + guaranteed rare reward + Void Crystal + story progression

---

## Status Effects in Combat

See `status-effects.md` for full details. Summary:

| Status | Type | Effect |
|--------|------|--------|
| Heat | Buildup | At 50%: discard 1 extra/turn. At 100%: take damage each turn |
| Shield | Resource | Absorbs incoming damage before HP |
| Glitch | Negative card | Dead weight in hand; various negative effects by type |
| Vulnerable | Debuff | Take 50% more damage from next hit |
| Disrupted | Module debuff | Slot unavailable for N turns |
| Focused | Buff | Next module fires twice |
| Supercharged | Buff | Next energy card counts as double |

---

## Design Intent

- **No action economy management** — you don't have a fixed number of "actions" per turn. Your constraint is cards in hand and module capacity.
- **Spatial thinking** — the *positions* of modules relative to bridges matters. This creates layout puzzles.
- **Determinism** — given the same hand and board, the outcome is always the same. Skill is in routing decisions, not dice rolls.
- **Information completeness** — enemy intents are always visible. Deck composition is always checkable. Nothing is hidden from the player.
