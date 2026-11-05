# Decision log: HVAC Service Call Sim

Why things are the way they are. Read this before proposing to change a past decision. If a decision changes, add a new entry; don't rewrite history.
Format: **Decision**, then *Why*, then *Rejected or deferred*. Newest at the bottom.

## Product and learning goals

**D1. Purpose is studying real troubleshooting, not a game.**
- *Why:* The owner uses this to learn HVAC diagnosis. Content follows two training briefs, one for an apprentice tech and one for a master tech: realistic dispatches, misleading information from the customer and the call, simulated meters and gauges, diagnosis grading on a 1–10 scale, and a rank.
- *Rule that follows:* when a choice is between "more realistic or educational" and "flashier", choose educational. The owner asked for this explicitly ("do the options that cater more to actually learning the material").

**D2. Residential split A/C only for v1, built so v2 is a plug-in.**
- *Why:* to keep scope finishable while making heat pumps and rooftop units easy to add later.
- *How:* all equipment logic lives under `SYSTEMS['split-ac']`. v2 = a new `SYSTEMS` entry, not edits to split-ac.

**D3. Refrigerant is random between R-410A and R-454B, with a hint to read the data plate.**
- *Why:* in the field you don't assume the refrigerant, you check the data plate. The randomness forces that habit.
- R-454B is an A2L (mildly flammable). Repair-side handling is deferred to v10.

**D4. Calls are procedurally generated from a seed, avoiding back-to-back repeats.**
- *Why:* The owner wanted it to work like games that generate their worlds, so practice doesn't become memorizing.
- *How:* the seeded RNG (mulberry32) plus a call history make any call reproducible from its seed, which also helps with bug reports and tests.

**D5. Misleading information (red herrings, latent issues) exists, but must teach.**
- The owner's concern: decoys must not defeat the educational purpose.
- *Rule:* every misleading element must be something a real tech meets *and* that a correct procedure resolves. No "gotcha" content that only punishes.
- This is why the mislabeled-breaker idea (D25) is still open.

**D6. Grading caps.**
- A wrong diagnosis caps the overall score at 5.4, even with good procedure.
  - *Why:* a wrong diagnosis was once rated "Service Tech", which teaches the wrong lesson.
- Tech-caused damage (`G.induced`) counts as legitimately needed repair work, not as unneeded parts.
- Safety-critical mistakes cap the rank.

## Platform and delivery

**D7. Single HTML file, vanilla JavaScript and SVG, no framework, no build toolchain.**
- *Why:* fully offline, installable on an iPhone, nothing to break, easy to publish as one artifact and one GitHub Pages page.
- `dev/build.py` only adds the PWA head and service worker registration.

**D8. Offline PWA on GitHub Pages; the owner uploads each version manually, on their command.**
- *Why:* the owner controls what goes public.
- *Rules:*
  - every release bumps `VERSION` in `sw.js`, or installed phones keep the old cache
  - bump the `<sup>` version tag in the title
  - add a README changelog entry

**D9. Versioning.**
- Feature releases are whole numbers (v4 layouts, v5 HD, and so on). Polish and fixes are minor (v8.1 to v8.10).
- The owner asked whether to merge the v4 and v5 work into one release, and chose separate releases as originally suggested: layouts first (v4), then HD (v5), so each upload is smaller and easier to check.

**D10. Showcase framing.**
- Question raised: does this work as a portfolio piece?
- Honest framing agreed: the code is AI-generated; the owner's role is product owner and QA (requirements, field realism, testing, iteration, deployment).

## Simulation model

**D11. Electrical readings come from a node-voltage (phasor) model, not canned numbers.**
- *Why:* The owner wanted free probing of any two terminals (L1–L2, L1–T1, T1–T2, and so on), like on a real contactor.
- Canned values can't stay consistent across arbitrary pairs; a model can.
- `test7.js` checks that free-probe readings match the preset ones for every fault.

**D12. Tech-caused faults are simulated, not just warned about.**
- *Why:* The owner's idea. Mistakes like measuring volts with the leads in the amp jack should actually happen: blown fuse, event pop-up, then a choice of continue or new call.
- It teaches meter discipline harder than a tooltip would.
- Accuracy corrections that were applied:
  - Putting R–Y through the amp jack acts like a **jumper** (it calls for cooling), not a dead short.
  - R–C through the amp jack *is* a short and blows the fuse.
  - HVAC run capacitors give a painful, dangerous shock but are rarely lethal. The sim doesn't exaggerate.

**D13. Hazard register with a self-test for each entry.**
- *Why:* The owner asked how to make sure *all* hazards are covered, not just the ones we thought of.
- `HAZARDS` lists real-world hazards with sources and status. Each covered one has an automated test (H1–H19). The README coverage table is generated from the register.
- Uncovered hazards are visible on purpose; they are the roadmap.

**D14. Field notes pop up once, only on the lower tiers (Apprentice and Junior).**
- *Why:* teach at the moment it matters without nagging experienced players.

## Interface

**D15. Three levels of view: house, then zone, then close-up.**
- *Why:* mirrors walking up to equipment, opening it, and getting your leads on terminals.
- The animation of taking a reading was proposed and then **cut by the owner**. Free probing in close-ups replaced it.

**D16. Panels have to be removed, like in the field.**
- The thermostat faceplate, blower door and condenser service panel each gate their close-ups (the `req` field in the code).

**D17. Condenser coil sits behind its own louver panels, separate from the service panel (option B).**
- *Why:*
  - The owner felt the coil looked too much like the cabinet.
  - On most residential units, removing the service panel doesn't expose the coil; it sits behind the louvered side panels.
  - So the louvers are a separate removable part, and the coil has its own close-up.

**D18. Interaction style: minimal text, tap to confirm.**
- Yellow glowing dots mark removable parts. A single tap shows a small confirm pill, a second tap acts.
  - *Why:* the earlier "Remove service panel" text labels were distracting and cluttered.
- The close-up hint is a magnifier icon only, auto-hiding after 5 s; tapping the part again opens the close-up.
  - *Why:* The owner found text badges cluttered, and wanted panel removal and close-up visually distinct.
- Double-tap or double-click a part to open its close-up directly.
- No duplicate log lines when the same thing is tapped twice.

**D19. Zoom and pan affect only the scene, never the whole page.**
- *Why:* on a phone, pinching the scene was zooming the browser and breaking the layout.
- The default framing is slightly zoomed in so all important parts are on screen.

**D20. Landscape phone layout is a split view: scene on the left, meter and work order on the right, each scrolling separately.**

**D21. The thermostat is fully interactive (mode, fan, setpoint).**
- The owner asked why it wasn't already.

## Visuals

**D22. HD is a single on/off switch with full lighting on every scene (v8.10).**
- History: v5 HD (textures only), then v8.9 HD+ prototype (lighting on two scenes), then v8.10 where the owner decided "HD+ is the new HD, there is no more HD+".
- *How:* lighting is assigned by material class, so new scenes get HD automatically. Dark mode uses half-strength highlights to avoid a glassy look.
- *Performance (measured before rollout):*
  - steady 60 fps, even at 6× CPU slowdown
  - switching views costs about 20–30 ms more with HD on
  - acceptable; still to be checked on a real older phone
- *Rule:* labels and terminals must stay high-contrast; learning comes before looks.

**D23. Field wear is visual and tied to faults:** burnt lug, oil stain, scorch marks, rust, sweating or iced lines.
- *Why:* real techs diagnose by eye first.

**D24. Real photos for close-ups (option A) are deferred.**
- *Why:* properly licensed photos of specific HVAC parts are scarce, and generic stock would be misleading.
- Parked on the to-do list, with a calibration mode idea so terminal hotspots line up with the photos.

## Open

**D25. Mislabeled breaker (v9): not decided.**
- **A:** rare, paired with the verify-dead procedure. The lesson: test, never trust the label.
- **B:** a field note plus a dedicated tutorial call.
- The concern from D5 applies: it must teach "verify dead", not act as a random trap.
