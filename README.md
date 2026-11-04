# HVAC Service Call Sim

**A point-and-click trainer for diagnosing residential AC service calls. Read the dispatch, question the customer, put the meter leads where a tech would, and prove your diagnosis before you call it.**

▶ **Play it:** https://waawaaweewaa.github.io/HVAC-Service-Call-Sim/
📱 Installs on your phone's home screen and works offline after the first visit.

---

## What it is

A study tool for practicing HVAC troubleshooting logic: sequence of operation, voltage tracing, safety habits, and reading refrigeration numbers. Every service call is generated at random, so you practice the *method* instead of memorizing answers.

It runs as one HTML file with no server, no account and no tracking. Your scores stay in your own browser.

## Features

| | |
|---|---|
| **Job site** | A cutaway house you walk through: thermostat, furnace and coil, breaker panel, condenser, registers. |
| **House layouts** | 4 house types (ranch, ranch with basement, two-story colonial, Cape Cod) with 2–3 rooms, ceiling or floor registers, and the condenser on either side. Switch rooms in the Registers view. |
| **Close-up views** | Detailed views of 11 parts (contactor, capacitor, compressor terminals, control board, etc.). Tap any two terminals for the red and black leads. |
| **Modeled voltages** | Every terminal has a modeled voltage, so any lead placement gives a consistent reading, including ghost voltage on floating conductors and warnings when the leads span two circuits. |
| **Real tools** | Multimeter (V AC, Ω, µF), amp clamp, probe and pipe-clamp thermometer, refrigerant gauges, PT chart, superheat and subcooling worksheet. |
| **Safety gating** | Taking Ω or µF readings on live circuits, skipping capacitor discharge, or swapping parts with power on is flagged and graded. |
| **Two refrigerants** | R-410A and R-454B, assigned at random. You confirm which one from the condenser data plate, as in the field. |
| **Randomized calls** | 15 faults across 4 difficulty tiers. Customer complaints vary, red herrings appear, and the same fault never comes up twice in a row. |
| **Graded debriefs** | Diagnostic, Technical Accuracy, and Troubleshooting scores. Shows which proof readings you took, a critique, how a senior tech would run the call, and the sequence of operation. |
| **Auto difficulty** | Apprentice → Junior Tech → Service Tech → Senior Tech, based on your recent scores. |
| **Live airflow** | Duct and condenser airflow reflects what is actually running. A hidden airflow view shows air temperature and volume through the whole system. |

## How a call works

1. **Dispatch.** Read the work order and the customer's complaint.
2. **Verify.** Confirm the complaint before you test anything.
3. **Trace.** Pick a meter function, then probe test points or open a close-up and place leads yourself.
4. **Justify.** A senior tech will occasionally ask why you took a reading.
5. **Call it.** Choose a diagnosis, a repair, and any secondary problems you found.
6. **Debrief.** Scored on whether your readings *proved* the diagnosis, not just whether you guessed right.

<details>
<summary><b>Fault library (spoilers)</b></summary>

| Tier | Faults |
|---|---|
| Apprentice | Tripped condenser breaker · Dead thermostat batteries · Clogged drain / tripped float switch · Blown board fuse after a thermostat install · Clogged filter with iced coil |
| Junior Tech | Open transformer · Open contactor coil · Burned contactor contacts · Weak fan capacitor · Weak compressor capacitor · Failed blower capacitor |
| Service Tech | Dirty condenser coil · Low charge (leak) · Overcharge · Repeat blown fuse from a chafed Y wire |
| Senior Tech | Junior/Service faults plus a hidden secondary defect and extra red herrings |

</details>

## Install on your phone

- **iPhone (Safari):** open the link → Share → **Add to Home Screen**
- **Android (Chrome):** open the link → ⋮ → **Install app**

Open it once while online. After that it works with no signal.

## How it was built

Designed and directed by **Marco Padilla**. Code written with **Claude** (Anthropic) as an AI coding assistant.

My role: defining the training goals and requirements, choosing between design trade-offs, requiring a failure-mode review before each build, testing on real devices, and deploying and versioning the app.

Tech: plain HTML, CSS, SVG and JavaScript with no frameworks or build step. Installable as an offline web app.

## Testing notes

- All 15 faults run through the simulation; readings checked against each fault's expected symptoms.
- Close-up voltages cross-checked against the standard test-point readings for every fault (1 mismatch found and fixed).
- Grading checked on correct and incorrect runs (1 scoring bug found and fixed).
- 300 generated calls checked for back-to-back repeats (none).
- Offline install verified in a phone-sized browser; real-device testing ongoing.

## Known limitations

- **Simplified physics.** Readings come from a rules-based model, not a full circuit or refrigeration simulation. Voltage drop under load and DC charge stored on capacitors are not modeled.
- **Approximate PT values.** R-454B bubble-point values are derived from an assumed glide. Use the manufacturer's PT chart or a digital manifold in the field.
- **Not yet reviewed by a licensed HVAC technician.** Corrections welcome.
- **Residential split AC with a gas furnace only** for now.
- **Local progress.** Scores live in your browser, so clearing site data resets them, and they don't sync between devices.

> ⚠️ **Training aid only.** This does not replace formal training, supervised field experience, manufacturer instructions, or local codes. Always follow lockout/tagout and electrical safety practices on real equipment.

## Roadmap

- [ ] Review by a working HVAC technician
- [ ] HD mode: realistic textures in the job site and close-up views
- [ ] Version label in the app header
- [ ] Heat pumps (reversing valve, defrost)
- [ ] Rooftop units and light commercial
- [ ] Pressure switches and building automation controls

## Project structure

```
index.html            the whole game (engine + UI)
sw.js                 service worker (offline cache)
manifest.webmanifest  install settings (name, icons, colors)
icons/                app icons
```

**Updating:** after replacing `index.html`, change `VERSION` in `sw.js` (e.g. `hvac-sim-v4` → `hvac-sim-v5`) so installed copies pick up the new build.

## Changelog

- **v4** Randomized house layouts (4 types, 2–3 rooms, mirrored sides), per-room registers, layout-aware airflow and airflow view. Note: replay seeds from v3 or earlier generate different calls.
- **v3** Quick-action bar under the scene, pulsing panel markers, airflow that matches what's running, hidden airflow view, iOS animation fix.
- **v2** Close-up views with free meter-lead probing, separate pole and winding readings, installable offline app.
- **v1** Core game: randomized calls, physics model, safety gating, graded debriefs, auto difficulty.

## License

Not yet chosen. Until a license is added, all rights are reserved by the author.
