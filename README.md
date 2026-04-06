# LEDCity

Procedurally generated top-down cityscapes rendered as glowing LED dot-matrix displays. Cities grow outward from a nucleus, filling in with buildings, parks, water, and roads, then settle into a continuous day/night cycle.

**Version 0.4**

## Getting Started

Open `ledcity.html` in any modern browser. A city will generate immediately and begin building outward from the center. No server or build step required -- just open the file directly.

## Controls

| Control | Description |
|---------|-------------|
| **Preset** | Switch between city types (resets with new seed) |
| **Speed** | 5-200% simulation speed (affects growth and day/night cycle) |
| **Pause/Play** | Freeze/resume the simulation |
| **New City** | Generate a fresh city with a random seed |

## Presets

### River City
A city bisected by a meandering river with organic, winding roads. Waterfront parks line the riverbanks. Dense commercial core near the center with residential neighborhoods spreading outward.

### Sprawl
Highway-driven suburban expansion around lakes. Major highway corridors cut across the landscape with local road grids filling in between. Lower overall density with scattered development.

## Architecture

### Two-Layer Grid System
The city is planned on a **50x50 block grid** where each block represents a 2x2 group of LEDs on the underlying **100x100 LED grid**. This gives each city block interior structure -- a residential block might show two building LEDs, a courtyard, and a path, rather than a single uniform color.

### Sub-Cell Fill Patterns
Building density determines how many of a block's 4 LEDs are buildings vs. open space:

| Density | Fill | Visual |
|---------|------|--------|
| Low (< 0.2) | 1/4 | Sparse suburbs -- one building, three open spaces |
| Medium (0.2-0.45) | 2/4 | Neighborhoods -- mix of buildings and green |
| High (0.45-0.72) | 3/4 | Urban -- mostly built up with a courtyard |
| Dense (0.72+) | 4/4 | Downtown core -- fully built |

Open spaces within blocks become:
- **Courtyards** -- green spaces within residential blocks
- **Fountains** -- blue accent visible even at night
- **Paths** -- walkways and plazas
- **Parks** -- larger green areas, sometimes with fountains or paths

### Day/Night Cycle
The city transitions through a full day/night cycle with dramatic lighting changes:
- **Day**: bright sunlit buildings, lush green parks, vivid blue water
- **Dusk/Dawn**: warm golden hour wash across the city, orange-pink reflections on water
- **Night**: dark city with scattered amber window lights, brightly-lit commercial downtown, streetlights on roads, fountain glimmers

## Version History

| Version | Changes |
|---------|---------|
| **0.1** | Initial release: two-layer grid system (50x50 blocks, 100x100 LEDs), River City and Sprawl presets, day/night cycle with dusk/dawn, sub-cell fill patterns (courtyards, fountains, paths), growth animation |
| **0.2** | Visual tuning: noise-distorted city center shape, steeper density gradient with preset-driven density, stronger dusk/dawn golden hour, improved daytime color separation, distinct courtyard/park/path colors |
| **0.3** | Fixed water color to stay blue throughout the day/night cycle instead of shifting orange at dusk |
| **0.4** | Variable river orientation and width, pruned floating road segments, double-width highways, reduced Sprawl road density, mixed-use city core |
