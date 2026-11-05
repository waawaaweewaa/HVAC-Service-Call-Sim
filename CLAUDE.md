# HVAC Service Call Sim — project notes for Claude Code

Point-and-click HVAC service-call training sim (residential split A/C). An offline PWA on GitHub Pages:
https://waawaaweewaa.github.io/HVAC-Service-Call-Sim/ . Owner: the repo owner (product owner, QA). Current version: **v8.10**.

## Files
| Path | What it is |
|---|---|
| `dev/hvac-sim.html` | **Source of truth.** Single file: CSS, HTML, `<script id="engine">`, `<script id="ui">`. Edit this file, never `index.html`. |
| `dev/build.py` | Builds `index.html` (adds PWA head and service worker registration). Also writes `dev/tests/engine.js` and `ui.js` for tests. |
| `index.html` | Built output, deployed. |
| `sw.js` | Service worker. The cache name in `VERSION` must be bumped every release. |
| `manifest.webmanifest`, `icons/` | PWA metadata. |
| `DECISIONS.md` | Why past decisions were made. **Read before proposing to change anything established.** When the owner makes a new decision, add an entry. |
| `README.md` | Public readme: features, hazard coverage table, roadmap, changelog (newest first). |
| `dev/tests/` | Regression tests (see below). |

## Workflow for every change
1. Before building anything, give the owner 3–5 failure modes (a "Red Team" pass) and a confidence assessment, then wait for approval.
2. Edit `dev/hvac-sim.html`. Prefer small, exact replacements.
3. `python3 dev/build.py`, then `node --check dev/tests/engine.js && node --check dev/tests/ui.js`.
4. Run the tests (all must pass):
   - `cd dev/tests && node test7.js` → last line `problems 0` (free-probe voltages match preset readings for all faults)
   - `node test8.js` → a perfect run grades 10/10/10
   - `python3 ui8.py` → tools and tech-caused-fault walkthrough, `errs []`
   - `python3 hz.py` → `19 of 19 passed.` with HD off and on (in-app hazard self-test)
   - `python3 hdall.py` → screenshots of every scene and close-up, light and dark, HD on, saved to `dev/.shots/`. **Look at them.** Click hit tests must all pass.
   - `python3 perf.py` (optional) → render time and fps at 1×/4×/6× CPU throttle
   - Python tests need `pip install playwright && playwright install chromium`.
5. Release (only when the owner says so):
   - bump `<sup class="ver">` in the source
   - bump `VERSION` in `sw.js`
   - add a README changelog entry
   - rebuild
   - suggest a conventional commit message (`feat: … (v8.11)`)

The owner uploads to GitHub manually unless they ask you to commit or push.

## Architecture (in `dev/hvac-sim.html`)
- **Engine:**
  - seeded RNG (mulberry32 + hashStr); `ENGINE.generate(seed, tier, {history})` builds a call and avoids back-to-back repeats
  - `SYSTEMS['split-ac']` holds `base`, `derive`, `settle`, `comps`, `points`, `actions`, `closeups`, `zones`, `scene()`, `overview()`, `zoomFor`, `cuOf`
  - **v2 (heat pump, RTU) = a new `SYSTEMS` entry**, not edits to split-ac
- **Registries:**
  - `FAULTS` (16), `LATENT`, `HERRINGS`, `LAYOUTS` (ranch, bsmt, colonial, cape), `REFRIG` (R-410A / R-454B pressure-temperature tables with glide), `PEOPLE`, `CHAIN`
  - `TOOLS` (mm, clamp, ncv, gauges, pipe, probe, jumper, disch)
  - `HAZARDS` (29 entries; 19 covered, each with a self-test H1–H19 in `runHazardTests`)
  - `NOTES` (12 field notes, pop-ups on lower tiers)
- **Electrical:** a phasor node-voltage model, so any two terminals probed give a consistent reading. Tech mistakes are modeled: amp-jack short, jumper, capacitor discharge, NCV. They are tracked in `G.events`, `G.induced`, `G.critical`, `G.dcMis` and graded, with a closeout check.
- **State:**
  - `G` = current call; `G.st` = equipment state, e.g. `stat.faceOff`, `indoor.doorOn`, `outdoor.panelOn`, `outdoor.louversOn`, `house.type`
  - `UI` = view state: `zone`, `cu`, `comp`, `confirm`, `cuHint`
  - `store` = saved settings: `hd`, `tips`, `mode`, `history`
- **Rendering:**
  - `renderAll()` → `renderScene()` → SVG strings, then `hdInject()`
  - Scene-only zoom and pan go through `VIEW` and the viewBox
  - Double-tap a part to open its close-up
  - Yellow dots are tap-to-confirm for removing panels; a magnifier hint auto-hides after 5 s
- **Close-ups:** `closeups[id] = {n, z, comps, req(s), about, terms:{ID:[x, y, label, kind, labelSide 't'|'b'|'l'|'r']}}`
- **HD:** an on/off switch (`store.hd`), with three layers:
  - `hdLight(svg)` lights every shape by material class (`k-metal`, `k-cab`, `k-panel`, `k-dark`, `k-fin`, `k-copper`): light gradient, cast shadow, texture
  - `HDP_OVL[zone|cu]` adds hand-made extras
  - Textures are canvas-generated blob URLs (`TEX`)
  - New scenes get HD automatically if they use the material classes

## Conventions
- Check light **and** dark mode (CSS tokens on `:root`), and HD on and off.
- Interactive elements (`.term`, `.pdot`, `.pchip`, `.cupill`, `.hot`) must stay on top of the HD overlays.
- Labels must not overlap; check the screenshots.
- No network at runtime (offline PWA). Fonts are cached by the service worker.
- Educational accuracy matters more than visuals. Flag anything technically wrong about HVAC plainly.
- Communication: direct, concise, tables. No filler. Flag errors plainly.

## Open decision
Mislabeled breaker for v9 (see DECISIONS.md D25):
- **A:** rare, paired with verify-dead
- **B:** field note plus a tutorial call

## Roadmap
- **v9:** verify-dead (live-dead-live), wrong part ratings, fan blade, compressor terminal, PPE at hose hookup, graded post-repair verification.
- **v10:** refrigerant repair workflow (micron gauge, leak detector, EPA 608 recovery, A2L handling).
- **Later:** real-photo close-up pack (option A, with calibration mode); review by a working HVAC tech.
