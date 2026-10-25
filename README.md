<div align="center">

# NVIDIA — GeForce RTX Promo

**A fan-made, animation-heavy promotional site for NVIDIA, GeForce RTX and the technology behind modern gaming and AI.**

Vite · React 18 · GSAP ScrollTrigger · Framer Motion · Lenis · TanStack Router

[![Live demo](https://img.shields.io/badge/live%20demo-website--code--aniamtions--and--all--tha.vercel.app-76B900?style=flat-square)](https://website-code-aniamtions-and-all-tha.vercel.app)
[![License: MIT](https://img.shields.io/badge/license-MIT-3b82f6?style=flat-square)](LICENSE)

</div>

> **Disclaimer —** this is a personal, educational concept project. It is not affiliated
> with, endorsed by or sponsored by NVIDIA. All trademarks and product names belong to
> their respective owners.

## Overview

A three-page marketing experience built around scroll-driven motion rather than static
sections. A loader counts up behind a fire wheel, the hero clip-reveals into a
scroll-scrubbed canvas scene, subtitles land on their own point of the timeline, and a
magnetic cursor and iris-style back-to-top button tie the whole thing together. Product
artwork is generated parametrically as SVG — spinning GPU fans, server racks and chips —
so the gallery stays crisp and lightweight at any size.

The animation architecture is based on the [JJK-promo](https://github.com/EduardStroescu/JJK-promo)
starter by **Eduard Stroescu**; the content, theming and product data are reworked for
this NVIDIA concept.

## What's inside

| Route | Page | Highlights |
| --- | --- | --- |
| `/` | **Home** | Progress-bar loader with fire wheel, clip-reveal hero, scroll-scrubbed canvas scene (green perspective grid + data motes), choreographed subtitles, CTAs |
| `/technology` | **Technology** | The NVIDIA story — ray tracing, DLSS 4, CUDA and the AI data-center era — with reveal animations |
| `/products` | **Products** | Filterable product gallery (RTX 50 / RTX 40 / AI & Data Center / The Tech Inside), parametric SVG artwork, animated detail overlay |

Shared interaction layer:

- **Magnetic sticky cursor** driven by React context, with a `prefers-reduced-motion` opt-out.
- **Eye scroll-to-top button** that is fully keyboard reachable.
- **Lenis smooth scrolling** piped into GSAP's ticker so ScrollTrigger stays in sync.
- Route-level code splitting — the landing page ships first, the other two load on demand.

## Tech stack

| Layer | Choice |
| --- | --- |
| Build | Vite |
| UI | React 18 (JSX), SCSS modules, Tailwind + PostCSS |
| Motion | GSAP + ScrollTrigger, Framer Motion springs, Lenis |
| Routing | TanStack Router (file-free route tree, `defaultPreload: "intent"`) |
| Typeface | Orbitron (700/900) + Rajdhani (500/600/700), self-hosted WOFF2 (OFL) |
| Testing | `node --test` + jsdom, plus a headless-Chrome animation audit over the CDP |
| Lint | ESLint with `--max-warnings 0` |

## Getting started

Requirements: **Node 22+** (the audit uses the standard-library WebSocket client) and npm.

```bash
npm install
npm run dev      # dev server
npm run build    # production build into dist/
npm run preview  # serve the production build locally
```

### Scripts

| Script | What it does |
| --- | --- |
| `npm run dev` | Vite dev server with HMR |
| `npm run build` | Production bundle into `dist/` |
| `npm run preview` | Serve the built bundle |
| `npm run lint` | ESLint over `js`/`jsx`, zero warnings allowed |
| `npm test` | Unit tests (`node --test`, jsdom) |
| `npm run audit:anim` | Build, serve and audit the animations in headless Chrome |
| `npm run audit:anim:dist` | Audit an existing `dist/` without rebuilding |

> Import pages from their own module, not from `src/pages/index.js` — that barrel
> re-exports every route and pulls the lazy ones back into the initial chunk.

## Project layout

```
src/
  components/            Layout, NavBar, Scroll, VideoBackground, LandscapePrompt
    StickyCursor/        Magnetic cursor + context hook
    scrollToTopButton/   Eye button
    secondaryLoader/     Post-load transition
  const/data.js          Product catalogue (name, art variant, accent, copy)
  hooks/                 useReducedMotion, useSceneVisibility
  pages/                 home, story (technology), characters (products), loader
  routes.jsx             Lazy route exports
  main.jsx               Router + providers
tools/anim-audit/        CDP driver, checks, entry point
.github/workflows/       Lint + unit tests + animation audit on push/PR
```

## Tests and quality gates

**Unit tests** cover everything that can be reasoned about without a browser:

- `src/hooks/useSceneVisibility.test.js` — every way the canvas scene can be hidden (hidden
  tab, zero-sized canvas, a panel covering the viewport, a panel covering only the bottom)
  and every way it is still partly visible.
- `src/const/data.test.js` — the product data against what the SVG renderer can draw, so an
  unknown body variant or a GPU with the wrong fan count fails here instead of silently
  rendering the wrong artwork.

**The animation audit** checks behaviour in a real browser. It builds the site, serves the
production bundle, opens it in headless Chrome and asserts what a visitor actually gets:
the intro counts to 100 and gets out of the way, the bar fades and slides, the iris comes
back to life, scene beats land on their own point of the scroll, the product grid renders
and switches, client-side navigation starts at the top, every control is reachable with
<kbd>Tab</kbd>, the scroll-to-top eye answers <kbd>Enter</kbd>, each page has exactly one
`h1` with no skipped heading levels, and nothing is written to the console. The suite runs
twice — once normally and once with `prefers-reduced-motion: reduce` emulated.

```bash
npm run audit:anim
npm run audit:anim:dist
node tools/anim-audit/index.mjs --url http://localhost:5173
node tools/anim-audit/index.mjs --only nav
```

The run prints pass/fail per check and exits `1` on failure, so it works as a CI gate; a
JSON report lands in `.freebuff/audit/report.json`. Chrome or Edge is discovered
automatically, and `CHROME_PATH` overrides the search. Checks that need to look inside
GSAP open the page with `?audit=1`, which exposes the GSAP, ScrollTrigger and Lenis handles
in production builds too — without that parameter nothing extra is exposed to a visitor.

`.github/workflows/anim-audit.yml` runs both jobs on every push and pull request.

## Deployment

Deployed on Vercel (`vercel.json`): framework `vite`, output `dist/`, and a catch-all
rewrite to `/index.html` so the client-side router handles deep links.

## Credits

- Animation architecture, loader, magnetic cursor and scroll choreography: [JJK-promo](https://github.com/EduardStroescu/JJK-promo) by **Eduard Stroescu**.
- Orbitron and Rajdhani by their respective authors, licensed under the SIL Open Font License.
- Product specifications shown in the gallery are public marketing figures, used here for a non-commercial concept demo.

## License

[MIT](LICENSE) © 2026 CoderGamerUnknow
