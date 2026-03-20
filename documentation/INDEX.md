# Reactor Core — Documentation Index

Web-based space roguelike / deckbuilder / engine-builder. You route energy cards through your ship's modules mid-combat, building cascade chains to survive increasingly dangerous salvage dives on the Long Rim — a crumbling frontier at the edge of a slowly failing empire.

---

## Vision & Design

| Document | What it covers |
|----------|---------------|
| [`idea.md`](idea.md) | Project vision, core hook, elevator pitch, target audience |
| [`story-and-lore.md`](story-and-lore.md) | World setting, factions, The Stead, lore systems, revelation arc |

## Gameplay Systems

| Document | What it covers |
|----------|---------------|
| [`micro-game.md`](micro-game.md) | Combat loop: turn structure, routing mechanics, cascade chains |
| [`macro-game.md`](macro-game.md) | Mission maps, node types, station overview, Drive Fuel, return/death mechanic, waypoint system |
| [`map-and-missions.md`](map-and-missions.md) | Biomes, full event catalog, boss design, map seeding |
| [`cards.md`](cards.md) | Card categories: Energy, Command, Hybrid, Glitch; deck rules |
| [`ship-and-modules.md`](ship-and-modules.md) | All ships, all module categories, grid layout, bridge system |
| [`enemies.md`](enemies.md) | Enemy archetypes, faction variants by act, all bosses |
| [`status-effects.md`](status-effects.md) | Heat, Shields, Glitches, buffs/debuffs — full reference |
| [`progression-and-economy.md`](progression-and-economy.md) | In-run economy, meta currency, ship persistence, unlock progression, scoring |

## Production

| Document | What it covers |
|----------|---------------|
| [`visual-design.md`](visual-design.md) | Art direction, color language, screen layouts, UI/UX principles |
| [`audio-design.md`](audio-design.md) | Music system, SFX design, voice direction, implementation notes |
| [`game-balance.md`](game-balance.md) | Core numbers, damage formulas, economy balance, Ascension tuning |
| [`technical-architecture.md`](technical-architecture.md) | Tech stack, FSM, ECS, data-driven design, file structure, save system |
| [`development-roadmap.md`](development-roadmap.md) | 6 phases from prototype to launch, risk register, post-launch plan |

---

## Quick Reference

**Energy types**: Red (Thermal) · Blue (Cryo-Plasma) · Yellow (EM) · Green (Bio-Organic) · Purple (Void)

**Turn order**: Draw (5 cards) → Route cards into modules → End Turn → Enemy executes intents → Cleanup

**Safe return**: Ship (deck + modules) is preserved. Waypoint saved at last completed act boundary.

**Death**: Lose ship and all in-run loot. Mind-Save activates at The Stead. Station upgrades persist.

**Faction Alert**: Returning to a mid-act waypoint gives the first 2 encounters +20% HP. Dock cleanly after an act boundary to avoid it.

**Drive Fuel**: Each launch costs 1 fuel. Free players: 5 cap, refills 1 per 2 hours. Full game purchase: unlimited.

**Core loop**: The Stead → Mission → Combat/Events/Shops → Safe return (ship survives) or death (ship lost) → Upgrade station → Repeat

**Tech stack**: React + TypeScript + PixiJS (rendering) + Zustand (state) + Howler.js (audio) + Node.js/Postgres (backend)
