# cordyceps

<p>
  <a href="https://adminnooot.github.io/cordyceps/">
    <img alt="Open Site" src="https://img.shields.io/badge/Open%20Site-000000?style=for-the-badge">
  </a>
  <a href="https://adminnooot.github.io/cordyceps/desktop.html">
    <img alt="Desktop" src="https://img.shields.io/badge/Desktop-1f2937?style=for-the-badge">
  </a>
  <a href="https://adminnooot.github.io/cordyceps/mobile.html">
    <img alt="Mobile" src="https://img.shields.io/badge/Mobile-111827?style=for-the-badge">
  </a>
  <a href="https://adminnooot.github.io/cordyceps/cordyceps/">
    <img alt="Simulation" src="https://img.shields.io/badge/Simulation-ff7a18?style=for-the-badge">
  </a>
</p>

Live petri-dish style **Cordyceps militaris** colony simulation (p5.js).

## Run it (GitHub Pages)
1. Go to **Settings → Pages**
2. **Deploy from a branch**
3. Branch: **main**
4. Folder: **/(root)**

Then the site URLs will be:

- Home: `https://adminnooot.github.io/cordyceps/`
- Simulation: `https://adminnooot.github.io/cordyceps/cordyceps/`

## Run it locally
Just open:

- `cordyceps/index.html`

in your browser (double-click, or drag into a tab).

## Controls
Orange transition:
- **Light (enables orange):** master knob; set to `0` for all-white colony
- **Orange start (delay):** how “mature” it must get before turning orange
- **Orange ramp (rate):** how fast the orange appears after it starts
- **Orange max:** cap for how orange it can get
- **Orange center bias:** `1.0` = center oranges first, `0.0` = edge oranges first
- **Orange hue:** adjust the orange tone

Growth:
- **Inoculum radius:** size of the initial center plug
- **Tips (max), Initial tips:** density/complexity
- **Branchiness:** branching frequency
- **Cottony (thickness):** mat accumulation strength
- **Ring tendency:** concentric zoning strength
- **Speed:** sim speed
- **Restart:** re-seed the colony
