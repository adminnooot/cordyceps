# 🍄 Cordyceps militaris Growth Simulation

A generative art piece simulating the cottony, fluffy aerial hyphae of *Cordyceps militaris* — the parasitic fungus known for its dense, woolly mycelium that transitions from bright white edges to warm orange pigmentation at its mature center. Built with [p5.js](https://p5js.org/).

## 🔬 Live Preview

<p>
  <a href="https://adminnooot.github.io/cordyceps/"><img alt="Launch Live Preview" src="https://img.shields.io/badge/Launch-Live_Preview-ff7a18?style=for-the-badge"></a>
  <a href="https://adminnooot.github.io/cordyceps/desktop.html"><img alt="Desktop" src="https://img.shields.io/badge/Desktop-1f2937?style=for-the-badge"></a>
  <a href="https://adminnooot.github.io/cordyceps/mobile.html"><img alt="Mobile" src="https://img.shields.io/badge/Mobile-111827?style=for-the-badge"></a>
</p>

> Interactive simulation with real-time controls — adjust density, branching, speed, wander, orange intensity, and more.

---

## What It Does

140 hyphae radiate outward from a single inoculation point, each one growing, wandering, and branching in real time. The result mimics the organic spread of *Cordyceps militaris* mycelium across a dark agar surface — cottony, fluffy, and aerial, with warm orange pigmentation developing at the dense center.

Key behaviors:

- **Cottony density** — 140 initial spawn, up to 8000 simultaneous hyphae with 12% branch probability, creating the thick fluffy mat characteristic of C. militaris
- **Noise-driven wandering** — each hypha steers using Perlin noise with high wander scale (0.30), producing tangled, cottony paths rather than clean radial lines
- **Weak outward bias** — filaments are only gently pulled away from origin (0.025 vs typical 0.04), allowing the dense tangled texture real Cordyceps colonies exhibit
- **Slow tapering** — taper rate 0.999 keeps strands thick longer, building up the dense aerial mat near the inoculation point
- **Distance-based coloring** — HSB color shifts from cream/white at edges (hue 38, sat 8) to warm orange at center (hue 25, sat 70), matching how real C. militaris develops orange pigmentation in mature areas
- **Aerial glow** — thicker hyphae near the center get a bright core highlight, simulating the depth of aerial hyphae rising above the substrate
- **Alpha-mapped opacity** — stroke opacity tracks filament width and distance from center, so thin ends fade naturally into the dark background

---

## Biology

This simulation is modeled after *Cordyceps militaris* mycelium growing on agar from a single inoculation point:

| Real Colony Trait | Simulation Implementation |
|---|---|
| Cottony/fluffy/aerial texture | High density (140 spawn, 8000 max), high branching (0.12), fine strands (1.5–3.5 stroke weight) |
| Dense center, wispy edges | Slow taper (0.999), many generations (9), center-weighted alpha |
| White edges → orange center | HSB distance blend: hue 38→25, saturation 8→70 based on distance from origin |
| Moderate radial growth | Speed 0.8x, outward bias 0.025 |
| Tangled, less directional than typical mycelium | Wander scale 0.30, weak outward steering |
| Aerial hyphae creating depth | Core glow highlight on thick strokes near center |

---

## Parameters

| Variable | Default | Effect |
|---|---|---|
| `maxHyphae` | `8000` | Hard cap on simultaneous filaments |
| `spawnCount` | `140` | Number of filaments spawned at start |
| `maxGen` | `9` | Maximum branch depth |
| `taperRate` | `0.999 – gen×0.0002` | How quickly each filament narrows |
| `speedMult` | `0.8` | Movement speed multiplier |
| `wanderScale` | `0.30` | Perlin noise influence (higher = more cottony) |
| `branchProb` | `0.12` | Chance of each filament splitting per frame |
| `orangeIntensity` | `1.0` | Multiplier for center orange pigmentation |

### Cordyceps vs Generic Mycelium

| Parameter | Generic Mycelium | Cordyceps |
|---|---|---|
| Spawn count | 80 | 140 |
| Max hyphae | 5000 | 8000 |
| Stroke weight | 3–5 | 1.5–3.5 |
| Branch prob | 0.08 | 0.12 |
| Taper rate | 0.998 | 0.999 |
| Wander scale | 0.20 | 0.30 |
| Outward bias | 0.04 | 0.025 |

Higher wander + lower outward bias = tangled cottony texture. Slower taper = thick dense center mat. Many fine overlapping strands instead of few thick ones.

---

## How It Works

```
setup()
  └─ createCanvas → initSimulation()
        └─ spawn 140 Hypha objects in a ring around origin (center)

draw() [every frame]
  └─ for each Hypha:
        ├─ update() — move, wander, taper, maybe branch
        └─ draw()  — render a line segment with distance-based HSB color
```

Each `Hypha` is removed from the array once it either:
- shrinks below minimum stroke width (`0.1`)
- exits the canvas bounds

New branches are appended to the same `hyphae` array during `update()`, up to `maxHyphae`.

---

## Structure

| File | Purpose |
|---|---|
| `index.html` | Device detection — auto-redirects to `desktop.html` or `mobile.html` |
| `desktop.html` | Full simulation with toggleable ⚙ Controls panel |
| `mobile.html` | Mobile-optimized — origin at bottom, hyphae fan upward, gradient fade overlay |
| `cordyceps/index.html` | Full simulation (same as desktop, preserves `/cordyceps/` URL) |

---

## Responsive Behavior

The canvas fills the full browser window. On resize, the simulation resets and redraws from scratch. Mobile version uses reduced parameters (4000 max hyphae, 60 spawn count) for performance.

---

## Tech

- [p5.js](https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.4.0/p5.js) `1.4.0` — loaded from CDN, no npm required
- Vanilla JS, single HTML files
- HSB color mode for natural distance-based color blending
- No build tools, no framework

---

## Usage

```bash
git clone https://github.com/adminnooot/cordyceps.git
cd cordyceps
open desktop.html   # or drag it into a browser
```

Or visit the [live GitHub Pages site](https://adminnooot.github.io/cordyceps/) for the interactive version with GUI controls.

**GUI Controls** (toggle with the ⚙ button in the top-right corner):

| Control | What it does |
|---|---|
| Speed | Movement speed multiplier |
| Max Hyphae | Cap on simultaneous filaments |
| Spawn Count | Initial filaments spawned at start |
| Max Generations | Branch depth limit |
| Taper Rate | How quickly filaments narrow |
| Wander Scale | Perlin noise influence strength |
| Branch Prob | Chance of each filament splitting |
| Orange Intensity | Center orange pigmentation strength |
| ↺ Restart | Re-initialize with current slider values |

---

## License

MIT — do whatever you like with it.
