# 🍄 cordyceps

<p>
  <a href="https://adminnooot.github.io/cordyceps/"><img alt="Open Site" src="https://img.shields.io/badge/Open%20Site-000000?style=for-the-badge"></a>
  <a href="https://adminnooot.github.io/cordyceps/desktop.html"><img alt="Desktop" src="https://img.shields.io/badge/Desktop-1f2937?style=for-the-badge"></a>
  <a href="https://adminnooot.github.io/cordyceps/mobile.html"><img alt="Mobile" src="https://img.shields.io/badge/Mobile-111827?style=for-the-badge"></a>
  <a href="https://adminnooot.github.io/cordyceps/cordyceps/"><img alt="Simulation" src="https://img.shields.io/badge/Simulation-ff7a18?style=for-the-badge"></a>
</p>

Biologically accurate Cordyceps militaris mycelium growth simulation in p5.js.

Simulates the cottony, fluffy aerial hyphae radiating from a single inoculation point — dense white-to-orange growth at center fading to delicate wispy edges, just like real C. militaris on agar.

## Structure
- `index.html` → auto-detects mobile/desktop and redirects
- `desktop.html` → full simulation with control panel
- `mobile.html` → mobile-optimized version (growth from bottom)
- `cordyceps/index.html` → full simulation (same as desktop)

## Biology
- Single inoculation point, radial outward growth
- Cottony/fluffy/aerial hyphae texture (high density, many overlapping fine strands)
- White mycelium at growing edges → warm orange pigmentation at mature center
- Moderate radial growth speed with abundant branching

## GitHub Pages
Settings → Pages → Deploy from `main` and `/(root)`.
