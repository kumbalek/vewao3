# Reactor Core — Project Vision

## Working Title
**Reactor Core** (Alternatives under consideration: *Cascade Protocol*, *Drift Engine*, *Long Rim*)

## Platform
Web-based (primary target). Future port to iOS/Android as native apps. No download required for initial release — playable in browser.

## Genre
Space Roguelike / Deckbuilder / Engine-Builder

## Elevator Pitch
You are an Engineer-Captain running salvage contracts from a beat-up station at the edge of a crumbling empire. Every fight is a puzzle: route power through your ship's modules, chain reactions across your hardware, and survive long enough to bring your loot home. The galaxy is quietly falling apart. Nobody's saying so yet.

## Setting in One Line
Firefly warmth at your station; Foundation dread on every dive — a frontier that was supposed to be temporary, and the creeping sense that something in the deep Rim is wrong.

## Core Hook
A **split-registry combat system** where the player manages two separate layers simultaneously:

- **Hardware (The Board)** — permanent physical modules installed on your ship grid. Weapons, shields, bridges, foundries. These execute the actions.
- **Software (The Deck)** — tactical cards drawn each turn. Energy, commands, hybrids. These are the fuel and logic that make the hardware run.

**Cards don't do the fighting. Modules do. Cards make modules fire.**

## Key Differentiator
In Slay the Spire and most deckbuilders, cards ARE the actions. In Reactor Core, cards are fuel and logic — the modules on the board execute the actions. Power flows spatially. Chains and automation are the primary paths to scaling.

A great deck fails without the right modules. A great engine fails without the right fuel.

## Player Fantasy
You are an engineer-captain **frantically wiring a spaceship's reactor mid-combat** — routing power, building automated loops, managing heat, and pulling off satisfying chain reactions just before the enemy tears your hull apart. Then you make it home, dock at your station (the water recycler humming its familiar pitch), and wonder how much longer you can keep doing this before the Rim gets too dangerous to work.

## Game Structure

### Micro Game (Combat)
Turn-based, deterministic combat on your ship's module grid. You route energy cards into module slots, modules fire, cascades chain. Enemies telegraph their intent. You plan around them.

### Macro Game (Roguelike Loop)
Node-based mission maps launched from **The Stead**, your permanent space station. On a safe return, your ship — deck and modules intact — docks and waits for the next run. Die, and your ship is lost; you start fresh. Either way, your station upgrades persist.

Each act goes deeper: different factions, stranger encounters, better rewards, and more of the story. Act 3 content is not locked behind difficulty — it's there for anyone willing to push that far.

## Tone
Two registers that coexist without contradiction:

**At The Stead**: Warm, lived-in, human. The crew that's washed up here didn't plan to stay. The coffee maker in the mess was broken for two weeks until the new arrival fixed it. The hand-painted star chart on the command room wall belongs to someone you'll never meet. Home matters more because you built it from salvage.

**On a dive**: Tense. Getting stranger. Each act pushes further from the comfortable human-scale politics of Act 1. By Act 3, the scale of what's out there becomes clear. The game is grim about what the Long Rim is becoming — but the *gameplay* should feel empowering. Pulling off a perfect cascade chain against a boss should feel like solving an elegant puzzle under fire.

## Target Audience
- Fans of Slay the Spire, FTL, Hades
- Players who enjoy systems mastery and build variety
- Casual-to-mid-core players who want roguelike depth without fighting game execution requirements

## Success Metrics (MVP)
- 3 playable ships with distinct grid layouts
- 2 full acts (Combat, Elite, Boss, Shop, Event nodes)
- 60+ cards, 20+ modules, 5+ enemy types, 2 bosses
- Full roguelike loop: station → mission → death/return → station upgrades (ship persists on safe return)
- Playable in browser without account (guest play)
