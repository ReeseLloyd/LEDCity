# LEDCity Project

## Project Location
`~/Library/CloudStorage/Dropbox/05-Projects/Programming/LEDCity/`

## What This Is
LEDCity is a single-file HTML application that procedurally generates top-down cityscapes rendered as glowing LED dot-matrix displays. Cities grow outward from a nucleus, filling in roads, buildings, parks, and water features, then settle into a day/night cycle. Deterministic from a seed number -- any city can be exactly reproduced. No server, no build step, no dependencies, no installation required.

---

## Repo Structure
```
ledcity.html     <- the application (always the current version)
README.md        <- documentation (always current, displayed on GitHub)
CLAUDE.md        <- this file
.gitignore
```

**Always read both `ledcity.html` and `README.md` before making any changes.**

There are no versioned files. Git history is the version history.

---

## Versioning Rules
- **Every change -- including minor bug fixes -- gets a new version number**
- Increment the minor version for all changes (e.g., `0.1` -> `0.2`)
- Increment the major version only for significant structural rewrites
- Update the `<title>` tag in `ledcity.html` to reflect the new version
- Add an entry to the version history table in `README.md` describing what changed
- Version history entries should be concise (one line), e.g.:
  `| **0.2** | Added vehicle traffic on roads |`
- For milestone versions, create a git tag: `git tag v0.2 && git push --tags`

---

## Tech Stack Constraints
- **Vanilla HTML, CSS, and JavaScript only** -- no frameworks, no libraries, no CDNs
- Everything must live in a single `.html` file -- do not split into multiple files
- No npm, no build process, no transpilation
- No external dependencies of any kind
- Must work when opened directly as a local file (`file://`) with no server
- Grid: 100x100 LEDs, 6px per cell = 600x600 px canvas
- Background: `#05060f`

---

## Architecture

### Two-Layer Grid System
The simulation operates on two layers:
1. **Block grid (50x50)** -- city planning layer. Each block is a 2x2 group of LEDs. Block types: `B_EMPTY`, `B_WATER`, `B_ROAD`, `B_PARK`, `B_RES`, `B_COM`, `B_IND`.
2. **LED grid (100x100)** -- render layer. Each LED is an individual sub-cell within a block. Sub-cell types: `S_EMPTY`, `S_WATER`, `S_ROAD`, `S_PARK`, `S_COURTYARD`, `S_FOUNTAIN`, `S_PATH`, `S_BUILDING`, `S_BUILDING_BRIGHT`.

City planning (water, roads, zoning) happens on the block grid. Each block then expands into a 2x2 LED pattern based on its type and density.

### Generation Pipeline
1. **Water** -- rivers or lakes placed on the block grid via Perlin noise
2. **Roads** -- organic winding roads or highway grids, with bridges over water
3. **Zoning** -- blocks near roads get assigned RES/COM/IND based on distance from city center
4. **Sub-cell fill** -- each block's 2x2 LEDs are individually assigned based on block type and density (buildings, courtyards, fountains, paths)
5. **Growth queue** -- blocks sorted by distance from center; revealed progressively during animation

### Fill Density System
Building blocks use density (0-1) to determine how many of their 4 sub-cells are buildings vs. open space:
- density < 0.2: 1/4 filled (sparse suburbs)
- density 0.2-0.45: 2/4 filled (neighborhoods)
- density 0.45-0.72: 3/4 filled (urban)
- density 0.72+: 4/4 filled (dense core)

Unfilled sub-cells become courtyards (green), fountains (blue), paths (tan), or park space depending on zone type.

### Day/Night Cycle
- `dayPhase` cycles 0-1 continuously (0 = noon, 0.5 = midnight)
- Sharper transition curve: more time in full day/night, quick dusk/dawn passages
- Dusk factor peaks at golden hour (~phase 0.22 and 0.78)
- Each sub-cell type has its own color function responding to day, dusk, and per-cell variation
- Commercial districts stay brightly lit at night; residential goes mostly dark with scattered window lights

### Seeded Randomness
- All randomness uses a seeded PRNG (mulberry32) so the same seed always produces the same city.
- Perlin noise (seeded) drives terrain and road organic variation.
- **Never use `Math.random()` in generation code** -- always use the seeded RNG. `Math.random()` is only acceptable for picking a new seed on restart.
- Breaking determinism is a critical bug.

### Presets
- **River City** -- bisected by a meandering river, organic winding roads, waterfront parks
- **Sprawl** -- highway-driven suburban expansion with lakes, lower density

---

## Design Principles
1. **Determinism is sacred** -- same seed must always produce the identical city
2. **Self-contained** -- the file must work with no internet connection and no server
3. **No regressions** -- changes must not break existing seeds
4. **The city is the focus** -- controls are minimal and secondary
5. **LED aesthetic** -- everything renders through the soft circular LED kernel with ambient blue bleed

---

## What NOT to Do
- Do not use `Math.random()` in city generation -- use the seeded RNG
- Do not introduce any external dependency, CDN link, or import
- Do not split the code into multiple files
- Do not create versioned files -- edit `ledcity.html` directly
- Do not change the behavior of existing seeds without a clear reason

---

## Testing
Open `ledcity.html` directly in a browser (`File > Open` or drag onto browser). No build step needed.

Key things to verify after any change:
- A city generates on load without errors
- The browser console is clean (no JS errors)
- Both presets produce visually distinct cities
- Day/night cycle transitions smoothly
- Growth animation fills the city outward from center
- Same seed produces the same city

---

## Git & GitHub
- Remote: https://github.com/ReeseLloyd/LEDCity.git
- Branch: `main`
- After completing changes, commit and push:
  ```
  git add ledcity.html README.md
  git commit -m "v0.X: short description of what changed"
  git push
  ```
- Commit messages should describe the functional change, not the mechanism
- For milestone versions, also tag and push the tag
