# Visual Design

## Art Direction: "Neon Schematic"

**Core aesthetic**: Dark-background technical schematics overlaid with vivid neon energy flows. Think a live circuit board that is also a spaceship, rendered in the style of a high-tech engineering readout. It is functional and beautiful simultaneously.

**Inspirations**:
- *FTL* — ship-as-game-board spatial logic
- *Hades* — rich character art, readable combat clarity
- *Slay the Spire* — card legibility and information hierarchy
- *Tron: Legacy* — neon-on-dark, clean geometry
- *Deus Ex* — HUD elements, hexagonal UI motifs

**Not**: photorealistic, pixel art (too retro), or cartoonish. This is stylized but grounded.

---

## Color Language

Every energy type has a signature color that permeates the entire visual system — cards, module glow, energy flow animations, enemy tints.

| Energy | Hex | Usage |
|--------|-----|-------|
| Thermal (Red) | `#FF4A1C` | Weapon modules, aggressive enemies |
| Cryo-Plasma (Blue) | `#1CA8FF` | Shield modules, cold/defensive |
| EM (Yellow) | `#FFD700` | Utility, hacking, precision |
| Bio-Organic (Green) | `#39FF14` | Adaptive, regenerative modules |
| Void (Purple) | `#9D00FF` | Rare, unstable, reality-warping |
| Neutral (Gray) | `#8A9BB0` | Inactive slots, UI chrome |
| Danger (Orange) | `#FF8C00` | Heat meter warnings |
| Corruption | `#FF0066` + glitch | Glitch cards, Static enemies |

Background base: `#0A0E1A` (near-black with blue undertone)
UI chrome: `#1A2233` with `#2A3A55` highlights

---

## Screen Layouts

### Combat Screen
```
┌─────────────────────────────────────────────────┐
│  [HP Bar]  [Heat Meter]          [Turn Counter]  │
├──────────────────────┬──────────────────────────┤
│                      │                          │
│    SHIP GRID         │    ENEMY DISPLAY         │
│   (left 55%)         │    (right 45%)           │
│                      │                          │
│  [Module slots       │  [Enemy illustration]    │
│   overlaid on ship   │  [Intent readouts]       │
│   illustration]      │  [Enemy HP bar]          │
│                      │                          │
├──────────────────────┴──────────────────────────┤
│           HAND OF CARDS (bottom strip)           │
│   [Card] [Card] [Card] [Card] [Card]  [END TURN]│
└─────────────────────────────────────────────────┘
```

**Ship Grid detail**: The ship is rendered as a detailed cross-section illustration. Module slots appear as semi-transparent glowing cells overlaid on the corresponding physical part of the ship (weapons on weapon hardpoints, shields on hull areas, etc.). When a card is dragged into a slot, it snaps into place with an energy flow animation along the connection lines between modules.

**Energy Flow Visualization**: Thin neon lines connect related modules. When energy moves through them, an animated pulse travels along the line — color matches the energy type. Cascade chains produce a visible "ripple" effect as energy passes from module to module.

### Station Screen
Top-down or slight isometric view of The Anchorage. Rooms are color-coded by function. Inactive/locked rooms are dark with a faint outline. Built rooms glow softly. A holographic mission map is accessible by clicking the Command Deck.

### Mission Map Screen
Node map displayed as a star chart with connecting flight paths. Visited nodes are fully lit, current position is pulsing, future nodes are dimmer. Path connections are visible (player sees all branching choices ahead). Biome changes are shown through background shift (debris field → nebula → static core).

---

## Ship Grid Visual Design

Each ship has a unique **cross-section illustration** — a detailed schematic of the vessel's interior. The illustration is rendered in the "neon schematic" style: dark background, bright outlines, labeled sections.

Module slots are **overlaid** on the illustration as interactive cells. The slot borders match the installed module's energy type color. Empty slots are outlined in neutral gray with a subtle pulse animation.

**Hardware changes are visible on the ship**: Install a better cannon and the cannon hardpoint on the illustration upgrades. The ship visually evolves as you build it.

---

## Card Design

Cards use a **holographic data-chip** aesthetic — they look like information cards from a sci-fi filing system, not physical playing cards.

**Card anatomy**:
```
┌────────────────┐
│ [Energy Icon]  │  ← Top-left: energy type + cost
│                │
│   [ART]        │  ← Center: abstract energy/effect art
│                │
│ Card Name      │  ← Bottom section
│ ─────────────  │
│ Effect text    │  ← Effect, concise
│                │
│ [Rarity pip]   │  ← Bottom-right: common/uncommon/rare
└────────────────┘
```

**Border color** = energy type of the card.
**Glitch/Status cards**: pixelated borders, corrupted art, offset text — visually "wrong."
**Upgraded cards**: border gets a metallic shimmer, a `+` appears in the name.

**Card states**:
- In hand: full opacity, slight hover lift on mouseover
- Being dragged: ghost trail in energy type color
- Slotted into module: compresses into slot with snap animation
- Discarded: slides down off screen

---

## Enemy Design

Enemies appear on the right side of the combat screen as **large, detailed illustrations**.

All enemies are corrupted ships — but their visual corruption level matches their Stage:
- **Drift stage**: Mostly intact ships with glowing Static tendrils and flickering lights
- **Integration stage**: Visible structural deformation, weapons mutated, hull plating rearranged
- **Core Corruption stage**: Barely recognizable as a ship — abstract, geometric, alien

**Intent Display**: Telegraphed intents appear as holographic icons floating above the enemy — clear iconography rather than text where possible. An attack intent shows a targeting reticle with a number. A hack intent shows a corrupted circuit icon.

**Boss enemies** fill more of the screen. Multi-phase bosses visually transform between phases — a transition animation shows the ship restructuring itself.

---

## Animation Principles

1. **Responsive** — every interaction has immediate visual feedback (< 100ms)
2. **Telegraphed** — cascade chains animate sequentially so the player can follow the flow
3. **Satisfying** — module fires have "punch" — brief pause then fast resolution with particle effects
4. **Readable** — animations never obscure important information

**Key animations**:
- Module fire: burst of energy type particles + screen micro-shake
- Cascade trigger: energy pulse traveling along bridge connection line
- Heat buildup: heat meter fills with orange glow, ship edges begin to redden
- Meltdown: screen vignette turns red-orange, flickering distortion
- Glitch card in hand: card periodically flickers/distorts
- Enemy attack: projectile or energy beam from enemy to ship, hull flash on hit
- Cascade chain: sequential pulse animations, each module adding its own sound + particle

---

## UI/UX Principles

- **One screen at a time** — no overlapping panels during combat. Information density is managed through progressive disclosure.
- **Drag-and-drop primary** — cards are dragged from hand to module slots. Click-to-select as accessibility alternative.
- **Color-coding is the primary language** — new players should be able to infer energy type compatibility from color alone before reading tooltips.
- **Tooltips on hover** — every module, card, and status effect has a detailed tooltip. Tooltips reference other tooltips (chain-link system).
- **No hidden information** — enemy intents are always visible. Your deck composition is always accessible. Heat level is always on screen.

---

## Technical Art Notes

- All game art rendered to support 2x pixel density (Retina/HiDPI displays)
- Cards: 240×336px base size at 2x
- Ship grid cells: 80×80px base at 2x
- Target: 60fps animations via WebGL (PixiJS)
- Sprite atlases for all game art to minimize draw calls
- Ships: layered PSD/PNG — base illustration + module overlay layer + damage state layer
