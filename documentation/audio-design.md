# Audio Design

## Tone & Direction

The audio landscape of Reactor Core should feel like you're inside a machine that is barely holding together — industrial, tense, rhythmic, occasionally beautiful. It is a world of hums, pulses, sparks, and static interference.

**Influences**:
- *FTL: Faster Than Light* — adaptive, layered soundtrack; tension through music shifts
- *Hades* — dynamic music transitions between zones; satisfying combat audio feedback
- *Tron: Legacy* — electronic, synthetic, clean geometry in sound
- *Dead Cells* — punchy sound effects with satisfying weight

**Core feeling**: Every action should feel *tactile*. Card plays snap. Modules fire with impact. Cascade chains sing as they build. Heat is a sound as much as a visual.

---

## Music System

### Adaptive Soundtrack
Music is **layer-based**. Each scene has a base track with additional instrument layers that activate dynamically:

**The Anchorage theme** ("Home Signal"):
- Base layer: sparse, ambient drone, low hum of station reactors
- +Layer: soft melodic lead when exploring rooms (walking around station)
- +Layer: gentle percussion when interacting with NPCs
- Feeling: Safety. Home. A little melancholy.

**Mission Map theme** ("Deep Dive"):
- Base layer: steady synthetic pulse, building tension
- +Layer: light percussion as player advances deeper into map
- +Layer: dissonant EM interference sounds near Act 3
- Feeling: Anticipation. Growing unease.

**Combat theme** ("Routing Protocol"):
- Base layer: rhythmic, industrial percussion loop
- +Layer: melodic synth line when player starts routing cards
- +Layer: additional harmonic elements as cascade chain builds up
- +Layer: full instrument mix during boss encounters
- Combat music tempo: 120–140 BPM

**Boss themes** — Each boss has a unique track:
- *The Sentinel*: Military march aesthetic, mechanical, grinding
- *The Architect*: Glitchy, constantly shifting, restructuring musically (mirrors phase shifts)
- *The Signal*: Three-movement composition that shifts style per phase (glitchy → driving → fragmented)

### Music Transitions
- Station → Mission Map: crossfade over 2 seconds
- Map → Combat: hard cut with brief silence (0.2s), then combat track in
- Combat → Victory: brief sting (0.5s), then fade to map theme
- Combat → Death: immediate silence, then somber station recovery theme
- Phase transition in boss: brief "reset" tone, new theme layer activates

---

## Sound Effects

### Cards
| Action | Sound |
|--------|-------|
| Draw card | Soft electronic "whoosh" — data transfer aesthetic |
| Hover card | Subtle energy hum pitch-shifted by energy type color |
| Drag card | Light crackle, energy type tinted |
| Slot card into module | Electronic "snap" + energy type resonance tone |
| Play Command card | Distinct "execute" click, more authoritative |
| Card discarded | Soft "whoosh" down |
| Glitch card drawn | Distorted, corrupted version of draw sound |
| Glitch card discarded | Fizzle, brief static burst |

### Modules
| Event | Sound |
|-------|-------|
| Module fully charged | Rising tone as last energy slots |
| Weapon fires | Impact/explosion — distinct per weapon type |
| Shield activates | Resonant hum, crystalline tone (Blue energy) |
| Utility/Foundry fires | Processing beep sequence |
| Bridge triggers | Soft click + energy type ping |
| Module disrupted | Error tone, static crackle |
| Module overloaded | Stressed mechanical groan |

### Energy Types — Distinct sonic identities
| Energy | Sound Character |
|--------|----------------|
| Red (Thermal) | Burning crackle, deep impact on fire |
| Blue (Cryo) | Crystalline, resonant, cooling hiss |
| Yellow (EM) | High-frequency electric buzz, precision ping |
| Green (Bio) | Organic pulse, soft wet resonance |
| Purple (Void) | Reversed tones, spatial distortion, eerie |

### Cascade Chains
Cascade chains are an **audio reward** — each step in the chain produces the next module's fire sound in sequence, slightly ascending in pitch. A full 5-module cascade chain should sound like a musical flourish. Players should *hear* how long their cascade is.

### Combat Events
| Event | Sound |
|-------|-------|
| Enemy telegraphs intent | Brief warning tone, archetype-specific character |
| Enemy attack lands | Impact + hull stress creak |
| Player HP depleted | Silence beat, then low warning alarm |
| Heat at 50% | Subtle alarm begin, periodic ticking |
| Heat at Meltdown | Urgent alarm loop, hissing steam |
| Heat reduced | Relieved cooling hiss |
| Glitch inserted into deck | Corrupted data sound, distant static burst |
| Glitch card purged | Satisfying "deletion" sound — clean |
| Enemy killed | Explosion sequence, then brief silence beat |

### Mission Map
| Event | Sound |
|-------|-------|
| Move to new node | Thruster pulse |
| Node type revealed | Node-specific identifier tone |
| Boss node approached | Warning resonance (distinct, ominous) |
| Map fully revealed (Archives) | Scanning sweep sound |

### Station (Anchorage)
| Event | Sound |
|-------|-------|
| Room entered | Ambient shift — each room has a unique atmospheric hum |
| Module built | Construction sounds, then satisfying "power up" hum |
| Module upgraded | Higher-quality version of same power-up |
| NPC conversation | Text reveal has a distinct character "voice" (beep pattern, not words) |
| Mission launched | System boot sequence, countdown, engine ignition |
| Return from mission | Docking clunk, pressure equalization |
| Death recovery (Mind-Save) | Startup tone — the sound of consciousness booting |

---

## Voice Direction

**No full voice acting**. Reactor Core uses **text-driven narrative** with audio cues:

- **NPCs at the Anchorage**: Each has a distinct text sound (a rhythmic beep pattern that matches their "personality" — the rescued pilot has a nervous, staccato pattern; the merchant has a smooth flowing one).
- **The ship AI companion**: Occasional one-word or short voiced lines for key moments only ("Cascade," "Critical," "Warning"). Synthetic, calm, slightly glitchy in corrupted zones.
- **Enemies**: No speech. Static-corrupted vessels produce interference sounds, mechanical stress noises, data burst sounds.
- **The Signal (final boss)**: Reversed human voices distorted into something inhuman — unsettling, non-verbal.

---

## Accessibility — Audio
- Separate volume sliders: Music, SFX, UI sounds
- Option to reduce/disable screen shake and flash effects
- Subtitles for any narrative text with audio components
- Colorblind mode (affects visual only, but audio labeling of energy types can supplement)
- Option to increase UI sound volume independently for low-vision players using audio cues

---

## Implementation Notes

- Audio engine: **Howler.js** (web audio, sprite-based, supports HTML5 + Web Audio API)
- Music format: OGG (primary) + MP3 (fallback) for browser compatibility
- SFX: Short OGG sprites, loaded in atlas bundles by scene
- Adaptive music: each layer is a separate looping audio file; layers activate/deactivate via volume fade (50ms crossfade)
- Spatial audio: not used (2D game without directional audio needs)
- Target: all audio loads asynchronously, game starts on first combat audio cue (no waiting for full audio load)
