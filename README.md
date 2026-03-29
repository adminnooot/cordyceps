# 🍄 Cordyceps militaris Growth Simulation

A generative art piece simulating the cottony, puffy expansion of *Cordyceps militaris* mycelium growing on agar — a soft, cloud-like circle that slowly expands outward from a single inoculation point, dense and orange-amber at the core, wispy and translucent at the growing edge. Built with [p5.js](https://p5js.org/).

## 🔬 Live Preview

<p>
  <a href="https://adminnooot.github.io/cordyceps/"><img alt="Launch Live Preview" src="https://img.shields.io/badge/Launch-Live_Preview-ff7a18?style=for-the-badge"></a>
  <a href="https://adminnooot.github.io/cordyceps/desktop.html"><img alt="Desktop" src="https://img.shields.io/badge/Desktop-1f2937?style=for-the-badge"></a>
  <a href="https://adminnooot.github.io/cordyceps/mobile.html"><img alt="Mobile" src="https://img.shields.io/badge/Mobile-111827?style=for-the-badge"></a>
</p>

> Interactive simulation with real-time controls — adjust growth speed, edge fuzziness, puff density, and orange intensity.

---

## What It Does

A single colony radius slowly expands from the center of the canvas. Each frame a small number of soft, semi-transparent blobs ("puffs") are scattered across the colony — mostly at the growing edge, with a few reinforcing the interior. Because the canvas never clears, these blobs accumulate over time, naturally building up:

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
| `growthSpeed` | `0.15` | Pixels added to colony radius per frame |
| `fuzziness` | `0.28` | Amplitude of Perlin noise edge irregularity (0 = perfect circle) |
| `density` | `22` | Soft blobs drawn per frame — the main performance knob |
| `colorIntensity` | `1.0` | Multiplier for orange/amber saturation in core |

---

## How It Works

```
setup()
  └─ createCanvas → initSimulation()
        └─ draw initial inoculation dot, set colonyRadius = 6

draw() [every frame]
  ├─ colonyRadius += growthSpeed
  ├─ for each of `density` puffs:
  │     ├─ pick random angle
  │     ├─ compute noise-displaced edge radius
  │     ├─ sample rPos from edge / mid / core zone
  │     ├─ map distance → HSB color + alpha + size
  │     └─ draw inner ellipse + soft outer halo
  └─ every 4th frame: reinforce core with 6 extra blobs
```

The canvas never clears — drawn blobs accumulate as pixels, building colony density over time. No objects are stored between frames.

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
| Growth Speed | How fast the colony radius expands |
| Edge Fuzziness | Irregularity of the growing edge (higher = more organic) |
| Puff Density | Blobs drawn per frame — lower = lighter on CPU |
| Orange Intensity | Saturation of warm orange pigmentation in core |
| ↺ Restart | Re-initialize simulation |

---

## License

MIT — do whatever you like with it.

