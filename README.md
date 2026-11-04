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
| **Tool tray** | Eight separate tools, like a real truck: multimeter (V~, V⎓, Ω, continuity, µF, with VΩ / mA / 10A red-lead jacks and a LEAd alert), clamp meter, non-contact voltage tester, analog manifold gauges, pipe-clamp thermometers, probe thermometer, jumper wire, capacitor discharge tool. Plus a PT chart and superheat / subcooling worksheet. |
| **Safety gating** | Taking Ω or µF readings on live circuits, skipping capacitor discharge, or swapping parts with power on is flagged and graded. |
| **Tech-caused faults** | Mistakes do real damage: voltage readings from an amp jack blow the board fuse (or arc-flash on 240 V), a jumper on R–C shorts the transformer, DC mode reads live AC as about 0 V. Continue the call and repair what you broke, or start over. |
| **Job closeout** | Leaving a door off, a jumper on, the disconnect pulled or a breaker off counts against you. |
| **Field notes** | Twelve short cards (meter setup, amp-jack danger, lockout/tagout, capacitor discharge, voltage tracing, voltage drop, jumpering, superheat and subcooling, iced coils, A2L refrigerant, NCV testers, clamp meters). They pop up the first time they apply on Apprentice and Junior tiers. |
| **Two refrigerants** | R-410A and R-454B, assigned at random. You confirm which one from the condenser data plate, as in the field. |
| **Randomized calls** | 16 faults across 4 difficulty tiers. Customer complaints vary, red herrings appear, and the same fault never comes up twice in a row. |
| **Graded debriefs** | Diagnostic, Technical Accuracy, and Troubleshooting scores. Shows which proof readings you took, a critique, how a senior tech would run the call, and the sequence of operation. |
| **Auto difficulty** | Apprentice → Junior Tech → Service Tech → Senior Tech, based on your recent scores. |
| **HD mode** | One tap switches flat colors to textured surfaces. Terminals and labels stay high-contrast for learning. |
| **Landscape split** | Turn your phone sideways: house on the left, meter and work order on the right. |
| **Field wear** | Oil stains at leaks, scorch marks, melted insulation, rust and fin corrosion by equipment age, sweating suction lines. Clues show up on some calls only, and some are decoys. |
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
| Service Tech | Dirty condenser coil · Low charge (leak) · Overcharge · Repeat blown fuse from a chafed Y wire · Loose / burnt connection at contactor T1 |
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

- All 16 faults run through the simulation; readings checked against each fault's expected symptoms.
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

## Hazard coverage

Generated from the hazard register in `index.html`. Every **Covered** hazard has an automated test; run them in the app under **Record & settings → Run hazard self-test**.

| Hazard | Status | How the sim handles it | Source | Test |
|---|---|---|---|---|
| Ohms or continuity on a live circuit | ✅ Covered | Reading refused, safety violation; critical on line voltage | NFPA 70E; meter manufacturer | H1 |
| Capacitance (µF) on a live capacitor | ✅ Covered | Reading refused, critical safety event | NFPA 70E; meter manufacturer | H2 |
| Testing a capacitor without discharging it | ✅ Covered | Reading refused, safety violation, field note | Manufacturer service manuals | H3 |
| Ohms or continuity across a charged capacitor | ✅ Covered | Reading refused, safety violation | Manufacturer service manuals | H4 |
| Removing a capacitor without discharging it | ✅ Covered | Repair refused, safety violation | OSHA 1910.147 (stored energy) | H5 |
| Discharging a capacitor with power on | ✅ Covered | Refused, safety violation | NFPA 70E | H6 |
| Not verifying 0 V DC after discharging | ✅ Covered | Leftover DC shows until discharged; debrief credits verification | Manufacturer service manuals | H7 |
| Replacing parts with power on | ✅ Covered | Repair refused, safety violation | OSHA 1910.147; NFPA 70E | H8 |
| Voltage reading from an amp jack (24 V) | ✅ Covered | Board fuse blows or transformer burns; repair required | Meter manufacturer; NFPA 70E | H9 |
| Voltage reading from an amp jack (line voltage) | ✅ Covered | Arc-flash event, critical, rating capped | NFPA 70E (arc flash) | H10 |
| Amp-jack probes across R–Y start equipment | ✅ Covered | Equipment starts through the meter, minor event | Meter manufacturer | H11 |
| DC setting on an AC circuit (live reads as dead) | ✅ Covered | Reads about 0 V, penalized, field note | Meter manufacturer | H12 |
| Jumper from R to C (dead short) | ✅ Covered | Fuse blows; blows again if left on | Manufacturer service manuals | H13 |
| Jumpering around a fuse | ✅ Covered | Safety violation; transformer burns if a short exists | NEC overcurrent protection principles | H14 |
| Jumpering around a safety (float switch) | ✅ Covered | Flagged at closeout | Manufacturer service manuals | H15 |
| Leaving the door switch defeated | ✅ Covered | Flagged at closeout | Manufacturer service manuals | H16 |
| Resetting a tripped breaker without a ground check | ✅ Covered | Penalized in the debrief | NFPA 70E | H17 |
| Trusting a non-contact tester on 24 V | ✅ Covered | No beep on 24 V; field note says verify with a meter | Tester manufacturer | H18 |
| Clamping a whole cable | ✅ Covered | Reads about 0 A; field note | Meter manufacturer | H19 |
| A2L refrigerant handling (R-454B) | ◐ Partial | Field note only | UL 60335-2-40; ASHRAE 15 | — |
| Not verifying the repair (amps, temperature split) | ◐ Partial | Shown in the debrief, not graded | Industry practice | — |
| Touching parts without proving them dead (live-dead-live) | ⏳ Planned (v9) | Not yet modeled | OSHA 1910.147; NFPA 70E | — |
| Mislabeled breaker: unit still live after shutoff | ⏳ Planned (v9) | Not yet modeled | NFPA 70E | — |
| Wrong replacement rating (fuse size, µF, voltage) | ⏳ Planned (v9) | Not yet modeled | Manufacturer specs | — |
| Fan blade starts while hands are near it | ⏳ Planned (v9) | Not yet modeled | OSHA 1910.212 | — |
| Compressor terminal venting on a grounded compressor | ⏳ Planned (v9) | Not yet modeled | Compressor manufacturer warnings | — |
| Refrigerant burns: gloves and glasses at hose hookup | ⏳ Planned (v9) | Not yet modeled | EPA 608 training; OSHA PPE | — |
| Venting refrigerant instead of recovering it | ⏳ Planned (v10) | Not yet modeled | EPA Section 608 | — |
| Gas leaks, CO, venting, heat exchangers | ⛔ Out of scope | Out of scope (cooling only) | NFPA 54; manufacturer | — |

19 covered · 2 partial · 7 planned · 1 out of scope.

## Roadmap

- [ ] Review by a working HVAC technician
- [x] HD mode: realistic textures in the job site and close-up views
- [x] Landscape split view on phones
- [x] Version label in the app header
- [ ] Heat pumps (reversing valve, defrost)
- [ ] Rooftop units and light commercial
- [ ] Pressure switches and building automation controls
- [ ] Photo pack: real photos for close-ups, with a calibration mode for terminal positions
- [ ] Refrigerant repair workflow: leak detector, recovery, repair, evacuation with a micron gauge, decay test, weigh-in charge

## Project structure

```
index.html            the whole game (engine + UI)
sw.js                 service worker (offline cache)
manifest.webmanifest  install settings (name, icons, colors)
icons/                app icons
```

**Updating:** after replacing `index.html`, change `VERSION` in `sw.js` (e.g. `hvac-sim-v8` → `hvac-sim-v9`) and the version tag in the `index.html` title so installed copies pick up the new build.

## Changelog

- **v8** Hazard register with 19 automated in-app safety tests; capacitor stored-charge model (leftover DC until discharged, verify on V⎓, no Ω across a charged cap); tool tray with eight separate tools; multimeter jacks, DC setting and LEAd alert; tech-caused faults with Continue / New call and repair tracking; free jumper wire and capacitor discharge tool; job-closeout check; Field notes tab with first-time pop-ups.
- **v7** Field wear and damage pack: new fault (loose / burnt connection at contactor T1, found by measuring voltage drop under load), oil stains at refrigerant leaks, scorch marks on shorts and burnt contacts, age-based rust and fin corrosion, sweating suction line. Visual clues appear on only some calls, and some are decoys (old repaired-leak oil, harmless heat tint), so readings still decide the diagnosis.
- **v6** Tighter default framing (house and equipment fill the scene; whole house fits the left half in landscape). Scene-only zoom: pinch, double-tap empty scenery, drag to pan, Reset zoom button; double-tap no longer zooms the whole page. Repeat readings are no longer re-logged: an identical reading shows an "Already logged · #n" notice with a View log shortcut, and doesn't count against efficiency. Readings that change after you change something still log.
- **v5** Version tag in the title, HD mode (textured siding, shingles, metal, copper, insulation, circuit board, filter pleats, plus label and washer details in close-ups), landscape split view on phones (house left, meter and work order right, each scrolling on its own).
- **v4** Randomized house layouts (4 types, 2–3 rooms, mirrored sides), per-room registers, layout-aware airflow and airflow view. Note: replay seeds from v3 or earlier generate different calls.
- **v3** Quick-action bar under the scene, pulsing panel markers, airflow that matches what's running, hidden airflow view, iOS animation fix.
- **v2** Close-up views with free meter-lead probing, separate pole and winding readings, installable offline app.
- **v1** Core game: randomized calls, physics model, safety gating, graded debriefs, auto difficulty.

## License

Not yet chosen. Until a license is added, all rights are reserved by the author.
