# Anatomy technical review and controlled-adoption decision

- **Date:** 2026-10-12
- **Status:** DOCUMENTED / PILOT NOT RUN / NO PRODUCTION ADOPTION
- **Decision:** EXTRACT + ONE CONTROLLED PILOT; preserve Diagram Design's current contracts
- **Source:** [wheresryan22/anatomy](https://github.com/wheresryan22/anatomy), inspected at [`689dcf966e63d92be10a5d593c6e19e10acd4422`](https://github.com/wheresryan22/anatomy/commit/689dcf966e63d92be10a5d593c6e19e10acd4422)
- **Source license:** MIT. This document paraphrases engineering concepts; it does **not** vendor, copy, or depend on the source implementation.
- **Repository baseline:** [`f4547ee95f88e5b28a52517feff6b6c11cc657f9`](https://github.com/Em3rc0d/diagram-design/commit/f4547ee95f88e5b28a52517feff6b6c11cc657f9), Diagram Design 2.6-series main
- **Evidence type:** Static source and documentation review only. No Anatomy build, browser audit, performance benchmark, or Diagram Design integration test was run as part of this review.

## 1. Problem and transferable hypothesis

Anatomy represents a technical mechanism through a recognizable, working physical instrument instead of abstract boxes and arrows. Its state, moving parts, displayed readings, and captions are derived from one underlying model. It includes a library for isometric SVG, optional WebGL, an experimental 3D renderer, and geometry/browser verification tooling.

**Hypothesis for Diagram Design:** For a narrow class of *mechanism-oriented explanations*, a restrained, evidence-linked instrument can improve causal comprehension over a conventional static diagram. This remains an unverified hypothesis, not a claim about user comprehension or conversion.

The relevant mechanism is: **true model -> physical metaphor -> authored geometry -> optional interaction -> synchronized readout -> checks and static fallback**.

An interactive object is not intrinsically a better architecture diagram. This repository's priority remains concise, static-first, semantic editorial diagrams.

## 2. Compatibility check against existing Diagram Design contracts

| Concern | Current Diagram Design source of truth | Anatomy approach | Disposition |
| --- | --- | --- | --- |
| Default output | `skills/diagram-design/SKILL.md`: static HTML/SVG, semantic-pattern then visual-type routing, target density 4/10 | Detailed physical instruments with dozens of solids | **KEEP** current default and density; no new visual-type count |
| Motion | `skills/diagram-design/references/animation.md`: optional `none/reveal/step/loop`, deterministic timing, pinned controller, static complete | Continuous model-driven pointer/keyboard animation and springs | **SEPARATE** experiment; do not silently reuse the existing motion controller for incompatible semantics |
| Axonometric views | `references/type-exploded.md` and `type-axonometric-plan.md`: 2:1 dimetric geometry, strict specific layouts | Different camera/projection and flexible mechanical assemblies | **DO NOT CHANGE** existing projection, examples, or verifiers |
| Skin | `references/style-guide.md`: branded semantic tokens, restrained accent, no shadows/glows | Four-tone solids, dense physical detail, optional shaders | **TRANSLATE PRINCIPLES ONLY**; use current brand tokens and house constraints in any pilot |
| Verification | Existing motion, geometry, layout, accessibility, pixel and screenshot verifiers | Physical clearance, terminal connectivity, painter's order, line-ends, slow-motion contact sheets | **ADOPT AS CANDIDATE CHECKS** only for a newly built instrument; do not assert existing checkers cover them |
| Dependencies | Self-contained HTML/SVG and established Python/Playwright tooling | Node 20.10+, local Chrome/Chromium; optional WebGL | **NO NEW DEPENDENCY** in baseline; isolate any proof-of-concept |

**Important:** The experimental mechanism is **not** an additional officially supported diagram type or semantic pattern. Existing source-of-truth references, examples, tooling and SKILL routing remain unchanged until the pilot passes its gates.

## 3. Extracted engineering contracts (candidate, opt-in)

### C1 — One source of truth

Create a deterministic, side-effect-free `model(input) -> state` with verified assumptions, units and ranges. A physical pose, gauge/readout, caption and accessible description must all derive from the **same state at the same time**.

- Do not invent measurements or use a visually convenient number that contradicts the model.
- Distinguish real-world facts, illustrative parameters, and explicit assumptions.
- Test boundary cases independently: empty, full, negative/invalid input, saturation, reset and rejected operation.
- Do not present an educational simulation as empirical telemetry.

### C2 — Mechanism must explain the behavior

Before drawing, write a mapping table for `concept / physical part / state / operation / observable outcome`. Every depicted moving part must contribute explanatory meaning. Reject metaphors requiring misleading mechanics, decorative gauges, or excessive detail.

A static architecture, ER, sequence, policy, or deployment view remains the correct choice when it explains the question more directly.

### C3 — Separate model, geometry, frame and runtime

Prefer an implementation split into `model`, `view/projection`, `static geometry`, `dynamic pose`, `interaction`, and `verification`. Precompute immovable SVG paths once; update only the paths/transforms/readouts that depend on current state. Do not run a full UI rerender per animation frame without evidence it is necessary.

This is a *design contract*, not a requirement to copy Anatomy modules or add React.

### C4 — Depth/connection integrity

For applicable isometric illustrations, ensure no impossible solid intersections, unsupported floating parts, broken pipe/cable endpoints, misleading occlusion, or lines ending in unconnected space. Evaluate **assembled, extreme and intermediate states**, not only a pretty resting frame.

Treat geometry audits as bounded checks against an authored geometric model, not proof of physical feasibility. Manual source/visual review remains required.

### C5 — Accessibility and reduced motion

- Preserve a complete, readable non-animated explanation; it must not rely on WebGL or pointer hover.
- Provide semantic labels and human-readable state and controls; keyboard operation must work.
- Honor `prefers-reduced-motion`: eliminate autoplay and decorative motion, while retaining user-requested state changes.
- Do not flood screen readers with per-frame announcements.
- Verify 390px mobile width and print/export/static output explicitly.

### C6 — No appearance-only acceptance

Evidence includes inspected captures at desktop and mobile sizes, light/dark as relevant, 4x junction crops, key control states, intermediate animation frames, and a no-WebGL/no-JS fallback. Test outputs and console failures are recorded, not assumed.

For a pilot implementation, automated geometry or line-end checks are optional until proven applicable; any absent check must be marked **NOT RUN / NOT IMPLEMENTED**, never **PASS**.

### C7 — Security and provenance

Pin source revision and retain references, declared assumptions, license and modifications. Execute browser drivers only against trusted local content: Anatomy's driver can execute page JavaScript and its documented Chrome configuration enables access to local files. Do not import unknown unreviewed code or run untrusted HTML in privileged browser contexts.

## 4. Controlled pilot contract (not executed)

**Target:** One standalone technical explainer on a **token-bucket rate limiter**; capacity 20 tokens, refill rate 5 tokens/second, cost 1 token per request. These are *pilot parameters*, not production statistics.

**Baseline:** Represent the same behavior with Diagram Design's existing **fan-in queue / bottleneck** semantic pattern and nearest **Data flow** layout, following `semantic-patterns.md`. Keep this baseline static and compliant with existing verifiers.

**Candidate:** A recognizable physical dispensing mechanism with a visible finite stock, incoming demand, allowed/rejected outcome, and a numeric readout. This candidate is an **isolated artifact**, not a new route in `SKILL.md`.

**Expected numerical invariants:** Given elapsed time `dt >= 0` and an admitted request count `n`, stock never exceeds 20 or drops below 0; add `5 * dt` up to capacity, then admit one-token requests only while stock >= 1. The pilot must define ordering at equal timestamps and display both admission and rejection. Use deterministic test sequences, not wall-clock randomness, for validation.

**Minimum evidence bundle** (keep outside shipped asset gallery until accepted):

1. The input parameters, pure model and mapping table (concept -> machine element).
2. Existing-type static baseline and optional candidate interactive HTML (same technical facts).
3. Automated model boundary tests; screenshot comparison (desktop, 390px, static, motion states); accessible keyboard and reduced-motion checks; no-JavaScript/optional-no-WebGL fallback.
4. Recorded runtime and artifact-size measurements on the same machine, and human review of precision, legibility and clutter.
5. Reviewer evidence about which view makes refill/admission/rejection clearer, or a documented statement that this has not yet been tested.

**Decision rubric:** Assess correctness, comprehension, visual coherence with Diagram Design, accessibility, reproducibility, maintenance burden, complexity and runtime. Record exact results and measurement conditions. No fabricated acceptance percentages, engagement metrics, or market outcomes.

| Gate | GO | PIVOT | KILL |
| --- | --- | --- | --- |
| Truth | Model invariants pass, readout/pose/caption agree | Correctable semantics or readout inconsistency | Cannot represent the rule without misleading viewers |
| Clarity | Reviewers can explain refill, capacity and rejection more accurately or clearly than with baseline | Comparable comprehension with lower complexity possible | Additional detail makes the mechanism harder to understand |
| Contracts | Complete static fallback, keyboard/reduced-motion, mobile and branded treatment pass | Repairs are bounded and separately estimated | Would require weakening baseline guarantees or introducing mandatory runtime/deps |
| Cost | Added maintenance and measured runtime are proportionate to demonstrated benefit | Pilot remains educational sandbox | Material cost with no demonstrated explanatory gain |

**Default without convincing comparative evidence: NO GO** for production integration. Preserve the extracted knowledge only.

## 5. Explicitly not adopted

- No wholesale import of `kit/`, WebGL shaders, 3D beta, browser drivers or agent instruction file.
- No universal "40+ parts" rule: Diagram Design's editorial simplicity wins when fidelity does not aid comprehension.
- No new mandatory packages, build step, CI jobs, action minutes or production bundle weight.
- No alteration to existing `animation.md` controller or its current timing, accessibility and static semantics.
- No change to existing isometric projection, supported visual type count, branded skin, or core skill routing.
- No promises of zero false positives in verification, scientific accuracy, learning improvements or marketing performance absent evidence.

## 6. Next action and closure

This document closes **knowledge extraction**, not adoption. The next authorized step is a **single local controlled pilot** with one baseline and one candidate, evaluated against the rubric above. Do not implement a general framework, new core plugin capability, shaders or 3D support first.

Record results in a separate pilot report if and when the implementation is actually run. Only after a demonstrated GO should a scoped change proposal discuss whether any optional instrument generation belongs in the primary skill.

## Primary references

- Anatomy [SKILL.md](https://github.com/wheresryan22/anatomy/blob/689dcf966e63d92be10a5d593c6e19e10acd4422/SKILL.md) and [kit](https://github.com/wheresryan22/anatomy/tree/689dcf966e63d92be10a5d593c6e19e10acd4422/kit)
- Anatomy [motion](https://github.com/wheresryan22/anatomy/blob/689dcf966e63d92be10a5d593c6e19e10acd4422/references/motion.md), [verification](https://github.com/wheresryan22/anatomy/blob/689dcf966e63d92be10a5d593c6e19e10acd4422/references/verify.md), [security](https://github.com/wheresryan22/anatomy/blob/689dcf966e63d92be10a5d593c6e19e10acd4422/SECURITY.md)
- Diagram Design [SKILL.md](../../skills/diagram-design/SKILL.md), [animation.md](../../skills/diagram-design/references/animation.md), [semantic-patterns.md](../../skills/diagram-design/references/semantic-patterns.md), [type-exploded.md](../../skills/diagram-design/references/type-exploded.md), and [style-guide.md](../../skills/diagram-design/references/style-guide.md)
