# Game Balance

## Core Numbers

### Player Stats
| Stat | Value | Notes |
|------|-------|-------|
| Starting HP | 80 | Most ships; Bastion starts at 100, Phantom at 65 |
| Max HP (upgrades) | Up to 120 | Via Medical Bay station + mid-run HP upgrades |
| Starting Heat cap | 100 | Threshold at 50 (warn) and 80 (danger) |
| Hand size | 5 cards | Some station/ship upgrades push to 6 or 7 |
| Starting deck size | 10 cards | 5 Energy, 3 Command, 2 Hybrid |
| Max deck size | 25 cards | Adding beyond 25 requires removing one first |
| Starting module slots | 4–6 | Ship-dependent |
| Max module slots | 8–12 | Ship-dependent, expanded at Shipyard |

### Enemy Stats by Act
| | Act 1 Normal | Act 1 Elite | Act 2 Normal | Act 2 Elite | Act 3 Normal | Act 3 Elite |
|--|-------------|------------|-------------|------------|-------------|------------|
| HP | 40–80 | 100–140 | 70–120 | 140–200 | 100–160 | 180–260 |
| Attack/turn | 8–16 | 14–22 | 14–22 | 18–28 | 18–30 | 22–35 |

### Boss HP
| Boss | Phase 1 | Phase 2 | Phase 3 |
|------|---------|---------|---------|
| The Sentinel | 220 | — | — |
| The Architect | 180 | 180 | — |
| The Signal | 150 | 150 | 150 |
| The Integrated (secret) | 300 (carries) | 300 | — |

---

## Combat Math

### Damage Formula
```
Final Damage = Base Damage × Vulnerable Multiplier × Weakness Multiplier − Shield
Vulnerable: ×1.5
Weakness: ×0.75
```

### Heat Generation Rate
- Each Red (Thermal) energy card played: +5 Heat
- `Overcharge` card: +10 Heat
- `Overcharge Bridge`: +8 Heat per trigger
- `Feedback Loop` Glitch (slotted): +20 Heat
- Heat decay per turn start: −5 (base), affected by Blue modules

**Design note**: A pure Red build with Overcharge cards can hit Meltdown in 3–4 turns without any Blue management. This is intentional — Red is powerful but volatile.

### Shield Timing
Shield is gained immediately and absorbs damage that same turn. It expires at the end of the enemy turn (when enemy executes their intent). This means:
- Shields from player turn protect from that turn's enemy attack
- Shields do NOT carry over to the next player turn unless Ablative Layer or similar is active

---

## Deck Power Curve

**The power curve through a run should feel like this**:
- **Turn 1–3 (early combat)**: Routing 1–2 modules per turn. Basic weapons, basic shields.
- **Turn 4–8 (mid-combat)**: Routing 3+ modules. First cascades appear. Starting to feel powerful.
- **Turn 9+ (late combat)**: Full cascade chains. If build is well-constructed, this should feel effortless.

**Boss fights stretch this**: Bosses are designed to be defeated around turn 8–15 for average builds. A great build can end Act 1 boss in 5–7 turns; a struggling build might take 18+ turns (often losing to Meltdown or HP attrition).

---

## Balance Pillars

### 1. The Basic Engine is Functional
Even a completely unoptimized deck and board should be able to clear Act 1. The floor is low but playable. This matters for new players.

### 2. Scaling Has No Hard Cap (But Has Practical Limits)
Cascade chains theoretically scale infinitely with enough bridge modules. The practical limit is:
- Cascade chain cap: 12 triggers per turn
- HP threshold: enemies die before chain length matters past ~6 modules
- Hand size: 5 cards = max routing in one turn without card generation

The "optimal" end-state for a well-built run is a board that fires 8–10 modules per turn from 5 card inputs. This should feel incredible — a reward for good drafting.

### 3. Glitches are Annoying, Not Crippling
Rule of thumb: **3 or fewer Glitch cards in a 20-card deck is manageable**. At 5+, performance degrades noticeably. At 8+, the deck barely functions.

Balance implication: Hackers must be killed before they insert too many Glitch cards. A Purge Port module removes the threat entirely. The player should feel urgency against Hackers, not helplessness.

### 4. Heat is a Real Threat
Meltdown (100 Heat) should feel like a genuine emergency, not a minor inconvenience. At 8 damage per turn, Meltdown ends a run in ~10 turns without resolution. Players should learn to:
- Draft some Blue energy cards in every Red-heavy deck
- Use at least 1 cooling module in high-Red builds
- Recognize the Meltdown risk and preemptively vent

### 5. Positioning Matters But Isn't Overwhelmingly Complex
The grid should reward thought without requiring optimal play. **Any valid bridge placement creates value**. Optimal bridge placement just creates *more* value. New players who don't understand adjacency still build functional boards; experienced players unlock significantly more power.

---

## Card Rarity Distribution in Reward Pools

When offering a card reward (3 choices after combat):

| Encounter | Common % | Uncommon % | Rare % |
|-----------|----------|-----------|--------|
| Act 1 Normal | 60 | 35 | 5 |
| Act 1 Elite | 30 | 55 | 15 |
| Act 2 Normal | 40 | 45 | 15 |
| Act 2 Elite | 15 | 50 | 35 |
| Act 3 Normal | 20 | 45 | 35 |
| Act 3 Elite | 5 | 35 | 60 |

---

## Economy Balance

### In-Run Scrap Pressure
A full Act 1 (8 nodes average) yields approximately **120–180 Scrap** from combat.

Expected costs in Act 1:
- Card removal (if needed): 40 Scrap
- Module at shop: 40–80 Scrap
- Card from shop: 15–50 Scrap
- Shipyard module install: 20–50 Scrap

A player should typically feel like they can afford 2–3 of the above per act, not all of them. Scarcity is intentional.

### Shop Pricing
| Item Type | Price Range |
|-----------|------------|
| Common card | 15–25 Scrap |
| Uncommon card | 25–40 Scrap |
| Rare card | 40–65 Scrap |
| Basic module | 40–60 Scrap |
| Rare module | 70–100 Scrap |
| Bridge module | 35–55 Scrap |
| Card removal | 40 Scrap (flat) |
| Card upgrade (`+`) | 30 Scrap |

---

## Difficulty Tuning (Ascension)

Each Ascension level adds one modifier. Modifiers are cumulative:

| Ascension | Modifier |
|-----------|----------|
| A1 | Elite enemies +20% HP |
| A2 | One fewer shop per mission |
| A3 | Each boss gains 1 new ability |
| A4 | Starting HP reduced by 10 |
| A5 | Start each run with 2 Glitch cards in deck |
| A6 | Elite enemies have buffed variants |
| A7 | Heat builds 25% faster |
| A8 | Shops have 20% higher prices |
| A9 | Event risk options are riskier (worse failure outcomes) |
| A10 | All bosses have multi-phase HP |
| A15 | Ambient Corruption applies to Act 2 as well |
| A20 | Maximum challenge. All modifiers active + enemy HP ×1.5 |

---

## Numbers To Watch (Playtest Targets)

These are the metrics that flag a balance problem if outside range:

| Metric | Healthy Range | Flag If |
|--------|--------------|---------|
| Average Act 1 completion rate (new players) | 60–80% | < 50% (too hard) or > 90% (too easy) |
| Average run duration to first full clear | 8–12 sessions | < 4 (too easy) or > 20 (too punishing) |
| Most drafted card (% of runs) | Any card | > 70% of runs = probably overpowered |
| Least drafted card | Any card | < 5% of runs = probably underpowered or unexciting |
| Heat Meltdown rate | 15–25% of deaths caused by heat | < 5% = heat not relevant, > 40% = heat too punishing |
| Cascade chain length (median per turn in Act 3) | 3–6 modules | < 2 = bridges not worth building; > 10 = cascades too dominant |
