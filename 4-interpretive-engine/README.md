# 4 — Interpretive Engine

Stage 4. The directed-hallucination layer. Generates somatic predictions, then validates them.

**Role.** Stands between two known truths (structural and cultural) and generates the midpoint that doesn't exist in either source. Runs the seven bridge types looking for somatic-structural tension. Can re-enter the Binary Engine at higher resolution, or the Web Engine for deeper context, when a hypothesis needs sharpening.

**Consumes.** `ActivatedAxes` (Activation) + `StructuralDescriptor` (Binary, retained for re-entry) + `ContextDescriptor` (Web, retained for re-entry) + `ConventionReport` (Cultural) + `EquipmentReport` (Equipment ID module).
**Produces.** `BridgeFindings` — somatic hypothesis, bridge-type classification, confidence, re-entry trace.
**Functions alongside.** Stage 5 — The Conversation — where human somatic data enters and overrules engine values when they disagree.

## Spec & code

| File | Role |
|---|---|
| `engine-interpretive.md` | Engine spec — seven bridge types, exploratory synthesis, re-entry policy |
| `brief-of-self.md` | Onboarding brief for the Claude collaborator running this engine in conversation |

## Bound sub-modules

| Module | Folder | Role |
|---|---|---|
| Bridge | `modules/bridge/` | The 7-type taxonomy and detection signatures. `module-bridge.md` is the spec; `bridge-taxonomy-draft.md` is the full Types 1-5 detail tables. |
| Somatic Gate | `modules/somatic-gate/` | Filtering somatic signal. Includes `somatic-dictionary.md` — body-to-signal correspondences, in-ear vs in-air. |
| Harmonic Resynthesis | `modules/harmonic-resynthesis/` | Re-entry tool — generates resynthesis trajectories to probe how perception responds when specific elements are altered |

## Notes

- Versions of each sub-module are independent of the engine version.
- The Conversation (where human somatic data enters) is technically Stage 5 but lives in chat sessions, not as a code component. It overrules anything in this stage when in conflict.
