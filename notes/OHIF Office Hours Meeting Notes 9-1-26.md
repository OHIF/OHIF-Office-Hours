## Questions and Summarized Answers

🧩 **Follow-up on the dedicated ECG mode PR — Harshika prepared an architecture document following the layered approach we discussed last time. Can the team review it and confirm the design before I proceed with additional PRs?** ([OHIF/Viewers#6068](https://github.com/OHIF/Viewers/issues/6068))

* **Proposed architecture (Harshika)**

  * **Layer 1 — Primitive viewport:** Leave the generic viewport unchanged; implement ECG-specific behavior in a new ECG viewport class. This avoids affecting the 2D/3D renderers and limits test surface.
  * **Layer 2 — Cornerstone tools:** Introduce physical units (ms, mV) at this layer, with a Layer 2B for ECG-specific measurements like QT interval and QRS complex.
  * **Layer 3 — Layout / hanging protocol:** Handled by OHIF, implemented by the ECG mode.

* **Team feedback on the approach — generally aligned**

  * Leaving the generic viewport as an abstract base and using a pure grid primitive is the right choice
  * Physical unit scaling should reuse Cornerstone 3D's existing physical unit scaling descriptor
  * Additional overlay tooling is the right pattern, and will be turned off when not using an ECG viewport — so it's only applicable in that context
  * Mid-scroll, time panning, and similar interactions should work as-is with the correct scaling
  * The overall approach should work

* **Multi-region spatial partitioning — supporting variable lead counts**

  * Standard 12-lead ECG typically displays as a 3-row × 4-column grid plus a rhythm strip row (3×4+1 layout)
  * For 15-lead ECGs (with 3 additional leads), extend the same layout definition to include the additional leads — rows are only rendered if the corresponding lead data is present
  * Same pattern generalizes to other lead counts (e.g., 21-lead) by defining the layout with placeholders and omitting rows without data
  * Suggestion: for each trace region, consider assigning a Z coordinate (Z0, Z1, ...) so that traces have distinct depth values on the canvas — this makes it possible to draw relationships (e.g., a line) between traces without ambiguity

* **Multi-ECG comparison (side-by-side)**

  * For comparing two ECGs, use two separate viewports rather than trying to fit both into a multi-region layout inside a single viewport
  * Provide the same layout definition to both viewports so they render comparably
  * This keeps the viewport's job simple (render the traces given to it) and lets the mode/layout layer handle multi-ECG arrangements

* **Suggested implementation path**

  * Start from a Cornerstone example — build the basic ECG viewport display in Cornerstone first, make it reasonably complete for the pieces that live at the Cornerstone level
  * Then work upward: discuss how to layer the OHIF mode on top once the Cornerstone pieces are solid
  * This ordering makes the eventual PRs easier to review as isolated units

* **Review timing**

  * The team hasn't been able to look at the earlier PRs yet — recent time has been consumed by measurement-related bug fixes
  * Aim is to get the new mode into the next OHIF release if the review process goes smoothly
  * If merged in time, the OHIF team will feature the contribution in the newsletter with proper attribution so the community sees the new work

* **Motivation shared by the questioner**

  * The company uses OHIF and provides ECG services to clinicians, but OHIF currently lacks a dedicated ECG panel — this contribution fills that gap
  * Future scope in the architecture document also covers EEG, SpO2, and blood pressure waveforms — the layered approach supports these as extensions of the same pattern

