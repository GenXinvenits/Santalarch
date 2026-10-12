# SANTAL — website reconstruction

A Vue 3 + Vite implementation workspace for reconstructing [santalarch.com](https://santalarch.com/).

## Development

Requirements: Node.js 20.19+ or 22.12+.

```sh
npm install
npm run dev
```

Create a production build with:

```sh
npm run build
npm run preview
```

A GitHub Actions workflow runs the production build on pushes to `main` and `reconstruction`.

## Implemented on `reconstruction`

- Vue 3 + Vite app with responsive layout foundations and GSAP scroll reveals.
- Main homepage sections, anchor navigation and responsive menu overlay.
- Working service selection, load-more project interaction, process details and infrastructure expansion.
- Cookie notice dismissed through Vue state (not direct DOM mutation).
- Interactive Three.js architectural door assembly: pointer-drag rotation, exploded/reassembled layers, reset view, responsive canvas sizing, and renderer/resource cleanup.
- Contact email links, location map link, footer navigation and reduced-motion CSS handling.

## Fidelity status — work in progress

This is **not yet a verified pixel-accurate clone**. The original site's full media library, exact font files and measurements, the complete cube-corridor-to-tea-room sequence, production scroll choreography, and final desktop/mobile browser comparison still need to be reproduced. Project-card artwork and team portraits remain temporary placeholders until original, usable assets can be identified. Do not treat the build as complete until the automated production build passes and the site is tested in a browser at desktop and mobile sizes.

## Reference

- Live website: https://santalarch.com/
- Design case study: https://thefirstthelast.agency/work/santal-architecture
