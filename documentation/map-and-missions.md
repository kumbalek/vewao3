# Map & Missions

## Mission Biomes

Each act of a mission takes place in a distinct region of the Long Rim. Biomes affect visual presentation, enemy pool, event flavor, and special rules.

### Act 1 — The Outer Belt
*The edge of former colonized space. Rim outposts that were supposed to be temporary became permanent. Half the infrastructure was built by people who planned to leave. They didn't.*

- **Visual**: Gray-brown asteroid clusters, tumbling metal fragments, flickering navigation beacons, the occasional light still on in an abandoned hab module
- **Enemy pool**: Rim Pirates, Scavengers, Border Militia
- **Special rule**: None (introductory biome, cleanest mechanics)
- **Loot quality**: Low-moderate
- **Flavor**: Not dead — just neglected. Somewhere out here, someone is still running a mining operation that the Empire forgot to tax.

### Act 2 — The Imperial Frontier
*Contested territory. The Solean Empire claims it on paper. The Free Reach claims it on the ground. The contested zone has been "stabilizing" for three years.*

- **Visual**: Deep purple/blue particle clouds, electronic warfare interference patterns, wrecked patrol ships from both factions, the occasional Imperial beacon still broadcasting
- **Enemy pool**: Imperial Border Guard, Free Reach Forces (see `enemies.md` for faction roster)
- **Special rule**: **Electronic Warfare** — Hidden nodes are truly hidden until reached (even with Archives). 30% of events have an extra "unknown option" with higher variance. The sensor jamming runs both ways — enemies can't see you clearly either, which occasionally works in your favor.
- **Loot quality**: Moderate-high
- **Flavor**: The further you go, the more it feels like neither side is actually winning.

### Act 3 — The Galactic Core Approaches
*The deep Rim. No one comes here voluntarily. Ships that enter don't always come back, and the ones that do come back changed.*

- **Visual**: Near-black space, geometry that doesn't quite resolve correctly on your instruments, dark energy pulses, fractured starlight at the edge of visual range
- **Enemy pool**: Unknown contacts — full Static-origin enemies. This is the first act where The Static appears as a named entity in flavor text and UI.
- **Special rule**: **Signal Overwrite** — At the start of each combat, 1 Glitch card is added to your hand (not deck). Cannot be avoided without specific module. *This is not ambient interference. Something is doing this deliberately.*
- **Loot quality**: High (best drops in the game)
- **Flavor**: The Mind-Save beacon barely reaches this deep. The signal is breaking up. You're not sure if that's your equipment or something else.

---

## Event Catalog

Events are text-based encounters. No combat. 2–3 choices with differing risk/reward profiles. Some are standalone; some are part of **narrative chains** that continue across runs.

### Standalone Events

---
**DERELICT FREIGHTER**
*A massive cargo vessel, dark and silent. Your sensors read no life signs — but also no active weapons. The registration markings are Imperial, but they're old.*

- **[Search It]** — Roll: 70% find Scrap (25–50) + rare component. 30% enemy ambush (Bruiser).
- **[Scan Only]** — Reveal the contents without boarding. If cargo is valuable, spend 20 Scrap to remote-extract safely.
- **[Leave It]** — Nothing.

---
**WOUNDED PILOT**
*A distress beacon. A small escape pod, one occupant, life signs fading. The pod markings don't match any faction registry you recognize.*

- **[Rescue]** — Spend 15 HP (medical resources). Gain a **Crew Card** (one-time powerful card added to deck).
- **[Recruit]** — They join The Stead if you return. Unlock a new station NPC and a permanent passive buff.
- **[Ignore]** — Nothing.

---
**ANOMALOUS TRANSMISSION**
*Your comms light up with a strange recursive signal. It doesn't match any known protocol. It's almost rhythmic.*

- **[Decode It]** — Add 3 Glitch cards to your deck. Gain 1 rare card and 1 Data Core.
- **[Analyze]** — Spend 30 Scrap. Study the transmission structure — reveal all Elite intents for the rest of this mission.
- **[Jam It]** — Nothing. The signal stops. (Or appears to.)

---
**AUTOMATED DEFENSE GRID**
*A pre-war installation. Dormant, but your presence activated it. You have 30 seconds before it fully warms up.*

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
- **[Seal It]** — Spend 15 Scrap on repair materials. Heat is reduced by 20 permanently for this run.
- **[Ignore]** — Nothing.

---
**BORDER DISPUTE**
*(Act 1 event — faction choice with reputation implications)*
*Two ships have been arguing over the same salvage claim for three hours. Both flag you down when you arrive. The Imperial lieutenant looks tired. The Free Reach captain looks like he's been waiting for an excuse.*

- **[Side with the Empire]** — Gain +1 Imperial rep. Receive Scrap reward from lieutenant. Free Reach Captain leaves, unhappy.
- **[Side with the Reach]** — Gain +1 Free Reach rep. Receive a rare module from the captain. Imperial lieutenant files a formal complaint that will never be processed.
- **[Claim it yourself]** — Both ships turn hostile. Elite Combat (2v1 Swarmer variant). If you win: take everything.
- **[Leave them to it]** — Nothing. Both sides are still arguing when you leave.

---
**IMPERIAL COURIER**
*(Act 2 event — mystery lore reveal)*
*A fast courier vessel, Imperial markings. The pilot is alone and very young. He's been traveling for three weeks from a posting further toward the core. He has dispatches he's not supposed to share. He does anyway — he hasn't spoken to anyone in days.*

- **[Listen]** — He describes outposts that have gone quiet. Not attacked — *quiet*. No emergency signals. No mayday. Just silence. Gain 1 lore entry.
- **[Trade information]** — Give him your sensor logs from recent missions. He gives you his route manifest — reveals 2 hidden nodes on the current map.
- **[Send him on his way]** — Nothing. He thanks you and accelerates toward the core.

---
**RENEGADE BROADCAST**
*(Act 2 event — faction alignment)*
*An encrypted Free Reach signal, tight-beam, clearly meant for you specifically. Someone knows your frequency. The voice is calm: "We know who you are. We know what you've been finding out there. We have information you need. It's not free."*

- **[Accept the meeting]** — Enter a brief text exchange. The Reach operative describes strange ship behavior in the deep Rim — not the kind caused by weapons or salvage accidents. Gain 1 lore entry + 1 rare card.
- **[Demand information without terms]** — Hostile. The signal cuts. No reward. Future Free Reach events are slightly cooler in tone.
- **[Report the signal to the Empire]** — Gain Imperial rep. Receive 30 Scrap. The Free Reach operative's voice, before you cut the connection: "We'll remember that."
- **[Ignore]** — Nothing.

---

### Narrative Chain Events

These events build across multiple missions. Your choices shape NPC storylines at the station.

**Chain: The Quiet Frequency**
A recurring transmission that appears in specific sectors and grows stranger across runs. First concrete evidence of something beyond faction conflict.
- *Run 1*: A signal with unusual rhythm. Not a beacon, not a distress call. You can't decode it. Log it and move on.
- *Run 3*: The same rhythm again. Different sector. You recognize it. An option appears to transmit back — a simple acknowledgment.
- *Run 5 (if you responded)*: Encounter a unique node — coordinates to a derelict vessel, the *Meridian*, a Free Reach scout that disappeared eight months ago. The *Meridian* is intact. No crew. No damage. The ship's logs end mid-sentence. Leads to The Integrated secret boss encounter.

**Chain: The Survivor**
A specific NPC (The Wounded Pilot) has a storyline if rescued.
- *Rescue*: They appear at The Stead. The coffee maker in the mess room is working again — they fixed it the first day. Provides a passive buff.
- *Later missions*: They want to come with you. Each mission where they're "present," specific events branch differently.
- *Final chapter*: They confront their past. Player choice affects their outcome.

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
