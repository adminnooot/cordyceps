# 🍄 Cordyceps militaris Growth Simulation

A generative art piece simulating the cottony, puffy expansion of *Cordyceps militaris* mycelium growing on agar — a soft, cloud-like circle that slowly expands outward from a single inoculation point, dense and orange-amber at the core, wispy and translucent at the growing edge. Pure vanilla Canvas 2D — no libraries required.

## 🔬 Live Preview

<p>
  <a href="https://adminnooot.github.io/cordyceps/"><img alt="Launch Live Preview" src="https://img.shields.io/badge/Launch-Live_Preview-ff7a18?style=for-the-badge"></a>
  <a href="https://adminnooot.github.io/cordyceps/desktop.html"><img alt="Desktop" src="https://img.shields.io/badge/Desktop-1f2937?style=for-the-badge"></a>
  <a href="https://adminnooot.github.io/cordyceps/mobile.html"><img alt="Mobile" src="https://img.shields.io/badge/Mobile-111827?style=for-the-badge"></a>
</p>

> Interactive simulation with real-time controls — adjust growth speed, edge fuzziness, puff density, and orange intensity.

---

## What It Does

A single colony radius slowly expands from the center of the canvas. Each frame many soft, semi-transparent blobs ("puffs") are scattered across the colony — mostly at the growing edge (cotton tufts + wispy tendrils), with fine stipple dots building interior depth. Because the canvas never clears, these blobs accumulate over time, naturally building up:

- **Dense, opaque center** — many frames of puffs have landed here since the colony started
- **Rich orange-amber pigmentation** in the core (matching how real *C. militaris* develops carotenoid pigments)
- **Wispy, translucent edge** — only the most recent puffs have reached the outer rim, so it looks soft and semi-transparent
- **Organic edge shape** — Perlin noise gently irregularizes the growing front so it is never a perfect circle

The entire simulation state is just three numbers: `colonyRadius`, `t` (time for noise), and the canvas origin. No object arrays, no particle tracking — extremely lightweight.

---

## Biology

This simulation is modeled after *Cordyceps militaris* mycelium growing on agar from a single inoculation point:

| Real Colony Trait | Simulation Implementation |
|---|---|
| Cottony/puffy/cloud-like texture | Overlapping semi-transparent ellipses accumulate into a soft mat |
| Dense opaque center | Core zone puffs have higher alpha; many frames of accumulation |
| White edges → orange center | HSB color by distance: hue 38→20, saturation 2→76 |
| Organic, non-circular edge | Perlin noise displaces the edge radius per angle |
| Gradual outward expansion | `colonyRadius` grows by `growthSpeed` px each frame |
| Aerial, pillowy appearance | Each blob has a larger, very transparent outer halo |

---

## Parameters

| Variable | Default | Effect |
|---|---|---|
| `growthSpeed` | `0.18` | Pixels added to colony radius per frame |
| `fuzziness` | `0.42` | Amplitude of noise edge irregularity (0 = perfect circle) |
| `density` | `6` | Cotton blobs drawn per frame — higher = fluffier, lower = lighter CPU |
| `colorIntensity` | `1.0` | Multiplier for orange/amber saturation in core |

---

## How It Works

```
setup()
  └─ createCanvas → init()
        └─ draw inoculation dot, set colonyRadius = 6

draw() [every frame — ~73 draw calls with default density=6, capped at 30 fps]
  ├─ colonyRadius += growthSpeed
  ├─ compute 80-point noisy polygon (three noise octaves, 1.8× amplitude)
  ├─ fill entire colony shape with one radial gradient (1 draw call)
  │     deep orange core → amber → warm cream → near-white edge, alpha ~0.016
  │     ↳ pixels accumulate: centre gets dense & opaque, frontier stays wispy
  ├─ density×5 cotton tufts at frontier — overlapping soft blobs, size 2–15 px
  ├─ density×3 wispy tendrils — tiny bright tips pushed just beyond the edge
  ├─ density×4 interior stipple — fine dots scattered throughout colony
  └─ every 4th frame: 3 amber blobs to deepen core colour
```

The canvas never clears — the single noisy circle fill accumulates as pixels each frame. The cotton tufts and wisps build the ragged, fluffy texture at the growing edge. No particle arrays, no hypha objects — just one growing shape plus scattered soft blobs per frame.

---

## Structure

| File | Purpose |
|---|---|
| `index.html` | Device detection — auto-redirects to `desktop.html` or `mobile.html` |
| `desktop.html` | Full simulation with toggleable ⚙ Controls panel |
| `mobile.html` | Mobile-optimized — same simulation, no control panel |
| `cordyceps/index.html` | Full simulation (same as desktop, preserves `/cordyceps/` URL) |

---

## Tech

- Vanilla JS + HTML5 Canvas 2D — no libraries, no build tools
- HSL color mode for natural distance-based color blending
- Single HTML files — open directly in any browser

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
| Growth Speed | How fast the colony radius expands |
| Edge Fuzziness | Irregularity of the growing edge (higher = more organic) |
| Puff Density | Blobs drawn per frame — lower = lighter on CPU |
| Orange Intensity | Saturation of warm orange pigmentation in core |
| ↺ Restart | Re-initialize simulation |

---

## License

MIT — do whatever you like with it.

