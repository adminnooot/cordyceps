# 🍄 Cordyceps militaris Growth Simulation

A generative art piece simulating the cottony, puffy expansion of *Cordyceps militaris* mycelium growing on agar — a soft, cloud-like circle that slowly expands outward from a single inoculation point, dense and orange-amber at the core, wispy and translucent at the growing edge. Pure vanilla Canvas 2D — no libraries required.

## 🔬 Live Preview

<p>
  <a href="https://adminnooot.github.io/cordyceps/"><img alt="Launch Live Preview" src="https://img.shields.io/badge/Launch-Live_Preview-ff7a18?style=for-the-badge"></a>
  <a href="https://adminnooot.github.io/cordyceps/desktop.html"><img alt="Desktop" src="https://img.shields.io/badge/Desktop-1f2937?style=for-the-badge"></a>
  <a href="https://adminnooot.github.io/cordyceps/mobile.html"><img alt="Mobile" src="https://img.shields.io/badge/Mobile-111827?style=for-the-badge"></a>
</p>

> Interactive simulation with real-time controls — adjust growth speed, edge noise, ring pattern, color warmth, and more.

---

## What It Does

A single colony radius slowly expands from the center of the canvas. Every frame the entire colony is redrawn pixel-by-pixel using ImageData — each pixel's color and opacity are computed from its polar coordinates relative to the center. As a result the look updates instantly whenever any parameter changes.

- **Dense, opaque golden-yellow center** — full-opacity pixels in the core, matching how real *C. militaris* develops carotenoid pigments
- **Concentric amber band pattern** — a sine wave perturbed by 2D noise creates subtle ring-like color variation in the interior
- **Cream/white fringe at the growing edge** — pixels in the outer 20 % of the colony radius lerp toward a creamy white
- **Organic edge shape** — three octaves of 2D Simplex noise irregularise the growing front so it is never a perfect circle
- **Random grain texture** — a subtle `Math.random` micro-texture produces a soft, organic filament hint across the colony surface

The entire simulation state is just two numbers: `currentRadius` (px) and `simTime` (s). No object arrays, no particle tracking — extremely lightweight.

---

## Biology

This simulation is modeled after *Cordyceps militaris* mycelium growing on agar from a single inoculation point:

| Real Colony Trait | Simulation Implementation |
|---|---|
| Golden-yellow to amber pigmentation | Interior pixels lerp between amber `#C8960A` and yellow `#F1C40F` via a noise-perturbed sine wave |
| Cream/white cottony edge | Outer 20 % of colony radius lerps toward cream `#F5F0E0` |
| Dense opaque center | Full alpha for pixels well inside the colony radius |
| Organic, non-circular edge | 3-octave 2D Simplex noise displaces the edge radius per angle |
| Gradual outward expansion | `currentRadius` grows by `params.growthRate` px/s each frame |
| Soft fuzzy boundary | Smoothstep fade over an adjustable `edgeFade` band at the colony edge |

---

## Parameters

All eight parameters are exposed as real-time sliders in the ⚙ Controls panel and take effect immediately without restarting.

| Variable | Default | Effect |
|---|---|---|
| `growthRate` | `15` | px/s added to colony radius |
| `noiseAmp` | `30` | Amplitude of noise edge irregularity in px (0 = perfect circle) |
| `edgeFade` | `20` | Width in px of the soft smoothstep fade at the colony boundary |
| `ringFreq` | `0.04` | Frequency of concentric color bands (higher = tighter rings) |
| `ringWobble` | `25` | Noise perturbation applied to the ring distance (organic wobble) |
| `fringeStart` | `0.80` | Normalised radius (0–1) where the cream fringe begins |
| `grainRange` | `0.15` | Amplitude of random micro-texture opacity variation |
| `colorWarmth` | `0.30` | Minimum amber–yellow mix (0 = darkest amber, 1 = pure yellow) |

---

## How It Works

```
resize()
  └─ set W, H, cx, cy, maxR from window dimensions

frame(ts) [every animation frame via requestAnimationFrame]
  ├─ compute dt (capped at 50 ms to survive tab-switching)
  ├─ simTime   += dt
  ├─ currentRadius += params.growthRate × dt  (until maxR + noiseAmp)
  ├─ updateNoiseCache(simTime × 0.4)
  │     └─ 1440 (desktop) / 720 (mobile) angle samples,
  │        three Simplex octaves → noiseCache[]
  ├─ ctx.fillRect  — clear main canvas to #2d3a4a
  ├─ renderColony()
  │     ├─ for every pixel (px, py) within bounding radius:
  │     │     ├─ r, theta = polar coords from center
  │     │     ├─ nv = noiseCache interpolated at theta
  │     │     ├─ targetR = currentRadius + nv × noiseAmp
  │     │     ├─ alpha from smoothstep edge fade over [targetR−edgeFade, targetR+edgeFade]
  │     │     ├─ color from concentric band (normR ≥ fringeStart → cream lerp,
  │     │     │            else amber↔yellow via sine wave + ringWobble noise)
  │     │     └─ grain = Math.random() micro-texture multiplied into alpha
  │     └─ octx.putImageData → ctx.drawImage (offscreen → main canvas)
  └─ [desktop only] update radius/time readout label
```

---

## Structure

| File | Purpose |
|---|---|
| `index.html` | Device detection — auto-redirects to `desktop.html` or `mobile.html` |
| `desktop.html` | Full simulation with toggleable ⚙ Controls panel (8 real-time sliders) |
| `mobile.html` | Mobile-optimized — same simulation, bottom-sheet controls panel |
| `simulation.html` | Standalone live preview — the new model rendered in a centred 700×700 canvas with a radius/time readout |
| `cordyceps/index.html` | Legacy redirect — auto-redirects to `desktop.html` or `mobile.html` (preserves old bookmarks) |

---

## Tech

- Vanilla JS + HTML5 Canvas 2D — no libraries, no build tools
- Pixel-level rendering via `ImageData` for per-pixel color and alpha control
- 2D Simplex Noise (Stefan Gustavson's algorithm, inline) for edge shape and grain
- Single HTML files — open directly in any browser

---

## Usage

```bash
git clone https://github.com/adminnooot/cordyceps.git
cd cordyceps
open desktop.html   # or drag it into a browser
```

Or visit the [live GitHub Pages site](https://adminnooot.github.io/cordyceps/) for the interactive version with GUI controls.

**GUI Controls** (toggle with the ⚙ button):

| Control | What it does |
|---|---|
| Growth Speed | How fast the colony radius expands (px/s) |
| Edge Noise Amplitude | Irregularity of the growing edge (higher = more organic) |
| Edge Softness | Width of the soft fade at the colony boundary |
| Ring Frequency | Frequency of concentric color bands |
| Ring Wobble | Organic perturbation of the ring pattern |
| Fringe Start | Where the cream fringe begins (as fraction of colony radius) |
| Grain Intensity | Amplitude of the noise-based micro-texture |
| Color Warmth | Amber-to-yellow bias of the interior color |
| ⏸ Pause / ▶ Resume | Freeze / continue the simulation |
| ↺ Restart | Re-initialize simulation from radius 0 |

---

## License

MIT — do whatever you like with it.

