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

## Current implementation status

The current branch is an initial structural and interaction prototype. It includes the main content areas, responsive layout foundations, navigation, expandable process/infrastructure content, service selection, and GSAP reveal animations.

It is **not yet a pixel-accurate clone**. The current project-card artwork and team portraits are temporary CSS stand-ins. The original site's actual media/assets, font files, precise layout measurements, full scroll choreography, interactive Three.js door model, cube-to-room transition, all responsive states, and final browser QA still need to be reconstructed and verified against the live reference.

## Reference

- Live website: https://santalarch.com/
- Design case study: https://thefirstthelast.agency/work/santal-architecture
