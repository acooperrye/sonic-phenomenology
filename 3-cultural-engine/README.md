# 3 — Cultural Engine

Stage 3. Runs after Activation. Reads context, returns a convention report.

**Role.** Maintains the convention bank — the entrenched expectations a genre and era set up. Where the Web Engine reports what a song *is* in cultural terms, the Cultural Engine scores how that song *conforms to or violates* its own convention frame. Outputs feed the Interpretive Engine's bridge types 6-7 (the structure-vs-semantics bridges that require a lifetime of listening to detect).

**Consumes.** `ContextDescriptor` (from Web) + `ActivatedAxes` (from Activation) + the convention bank itself.
**Produces.** `ConventionReport` — entrenchment scores, violation signals, signed-float convention deltas. (See spec for full schema.)
**Functions alongside.** Activation (2) — reads its output; Interpretive (4) — feeds it bridge-relevant convention data.

## Spec & code

| File | Role |
|---|---|
| `engine-cultural.md` | Engine spec — convention bank, entrenchment curves, signed-float model |

## Notes

- No code yet. Spec-only stage. Largest single spec doc in the project (~65KB).
- This engine is what makes Types 6 (convention violation) and 7 (inversion) detectable. Without it, the Interpretive Engine can read Types 1-5 (internal structure↔theme tension) but not the biographical bridges.
