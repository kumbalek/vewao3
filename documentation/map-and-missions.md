# Map & Missions

## Mission Biomes

Each act of a mission takes place in a distinct region of the Crucible Expanse. Biomes affect visual presentation, enemy pool, event flavor, and special rules.

### Act 1 — The Debris Field
*The outer shell of former colonized space. Wreckage of the initial Corruption event drifts here.*

- **Visual**: Gray-brown asteroid clusters, tumbling metal fragments, faint emergency lights
- **Enemy pool**: Drone swarms, basic Corrupted Scouts, early Bruisers
- **Special rule**: None (introductory biome, cleanest mechanics)
- **Loot quality**: Low-moderate
- **Flavor**: Everything here is already dead. You're picking over bones.

### Act 2 — The Nebula Belt
*Dense particle clouds that scramble sensors. Visibility is limited. Threats are less predictable.*

- **Visual**: Deep purple/blue fog, glowing particle clouds, bioluminescent organic structures
- **Enemy pool**: Hacker ships, Armored variants, Swarmer clusters, first Saboteur appearances
- **Special rule**: **Sensor Fog** — Hidden nodes are truly hidden until reached (even with Archives). 30% of events have an extra "unknown option" with higher variance.
- **Loot quality**: Moderate-high
- **Flavor**: You can feel the Static getting denser. Navigation is guesswork.

### Act 3 — The Static Core
*The heart of the Corruption. Reality is unstable here. The Static is everywhere.*

- **Visual**: Near-black space, glitching geometry, dark energy pulses, fractured starlight
- **Enemy pool**: Elite-tier Hackers, Saboteur-Bruiser hybrids, Static Avatars, final boss
- **Special rule**: **Ambient Corruption** — At the start of each combat, 1 Glitch card is added to your hand (not deck). Cannot be avoided without specific module.
- **Loot quality**: High (best drops in the game)
- **Flavor**: The station's Mind-Save beacon barely reaches this deep. The signal is breaking up.

---

## Event Catalog

Events are text-based encounters. No combat. 2–3 choices with differing risk/reward profiles. Some are standalone; some are part of **narrative chains** that continue across runs.

### Standalone Events (Sample)

---
**DERELICT FREIGHTER**
*A massive cargo vessel, dark and silent. Your sensors read no life signs — but also no active weapons.*

- **[Search It]** — Roll: 70% find Scrap (25–50) + rare component. 30% enemy ambush (Bruiser).
- **[Scan Only]** — Reveal the contents without boarding. If cargo is valuable, spend 20 Scrap to remote-extract safely.
- **[Leave It]** — Nothing.

---
**WOUNDED PILOT**
*A distress beacon. A small escape pod, one occupant, life signs fading.*

- **[Rescue]** — Spend 15 HP (medical resources). Gain a **Crew Card** (one-time powerful card added to deck).
- **[Recruit]** — They join The Anchorage if you return. Unlock a new station NPC and a permanent passive buff.
- **[Ignore]** — Nothing.

---
**STATIC BROADCAST**
*Your comms light up with a strange recursive signal. The Static is transmitting directly at you.*

- **[Decode It]** — Add 3 Glitch cards to your deck. Gain 1 rare card and 1 Data Core.
- **[Analyze]** — Spend 30 Scrap. Learn the broadcast structure — reveal all Elite intents for the rest of this mission.
- **[Jam It]** — Nothing. The signal stops.

---
**AUTOMATED DEFENSE GRID**
*A pre-Static installation. Dormant, but your presence activated it. You have 30 seconds before it fully warms up.*

- **[Disable It]** — Spend 1 Yellow energy card (from deck) permanently. Gain access to the installation's loot cache (Module + Scrap).
- **[Fight Through]** — Trigger an Elite Combat immediately. Elite is weaker than normal (70% HP) since defenses aren't fully online.
- **[Retreat]** — Take 10 damage from initial fire, pass through safely.

---
**ROGUE MERCHANT**
*A small unmarked vessel, flagged as unregistered. Opens comms: "I have goods. You have Scrap. Simple transaction."*

- **[Trade]** — Opens a small Black Market shop (3 items, one is always rare).
- **[Rob Them]** — Elite Combat. If you win: take all their stock for free.
- **[Ignore]** — Nothing.

---
**VOID ANOMALY**
*A spatial distortion. Inside, the laws of physics seem slightly different. Your instruments are useless.*

- **[Enter]** — Remove 1 card from deck permanently (your choice). Gain 1 Void Crystal.
- **[Sample It]** — Add 1 Void Sliver (Purple energy card) to deck. Gain 20 Scrap.
- **[Leave]** — Nothing.

---
**ENGINE VENT**
*Your reactor coolant line has a micro-fracture. You can run hot for a burst of power, or seal it conservatively.*

- **[Run Hot]** — Set Heat to 80. This turn in combat: all modules fire for double effect.
- **[Seal It]** — Spend 15 Scrap on repair materials. Heat is reduced by 20 permanently for this run (max Heat threshold shifts down).
- **[Ignore]** — Nothing.

---

### Narrative Chain Events

These events build across multiple missions. Your choices shape NPC storylines at the station.

**Chain: The Ghost Signal**
A recurring transmission that grows clearer across runs. Eventually leads to the secret boss encounter. Choices determine whether the outcome is tragic or redemptive.
- *Mission 1*: First fragment. Vague, ominous.
- *Mission 3*: Source triangulated. An option appears to transmit back.
- *Mission 5*: If you responded: encounter a unique node with the source. Leads to The Integrated secret boss.

**Chain: The Survivor**
A specific NPC (The Wounded Pilot) has a storyline if rescued.
- *Rescue*: They appear at the Anchorage. Provide a passive buff.
- *Later missions*: They want to come with you. Each mission where they're "present," specific events branch differently.
- *Final chapter*: They confront their past as a pre-Static crew member. Player choice affects their outcome.

---

## Boss Node Design Notes

Bosses always appear at the end of each act. Boss rooms are announced on the map with a distinct icon.

**Boss encounter flow**:
1. Pre-boss text vignette (atmospheric, no choices)
2. Combat begins — boss has full UI display with phase indicators
3. Phase transitions have brief cinematic moments (text + visual shift)
4. Post-boss: reward screen, Act transition vignette

**Boss reward tiers**:
- Act 1 Boss: 1 Rare card choice + 40 Scrap
- Act 2 Boss: 1 Rare module choice + 60 Scrap + 1 Data Core
- Act 3 Boss: Full reward screen: 2 Rare choices (card + module) + 80 Scrap + 2 Data Cores + 1 Void Crystal

---

## Map Seeding

Every mission runs on a **procedural seed**:
- Seed determines node type layout, enemy selections, event choices, shop inventory, card reward pools
- Seed is displayed to the player (for run sharing)
- Same seed = same map and encounters (deterministic)
- Daily Challenge mode uses a global shared seed — all players play the same map on that calendar day

---

## Mission Checkpoints

Missions auto-save at:
- Act transition nodes
- After every Shipyard visit
- After every combat (including Elite and Boss)

If the player closes the browser mid-run, the run state is preserved and resumable. Full run state is serialized to localStorage (and cloud save if logged in).
