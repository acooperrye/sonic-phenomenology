# 2 — Activation Layer

Stage 2. The convergence point where Binary and Web outputs meet.

**Role.** Takes the inert outputs from both Stage 1 engines and applies the three-filter scoring system that turns measurements into findings. This is where the 216 unweighted/unsigned axis readings from Binary become weighted, signed axis scores with bridge-tension markers.

**Consumes.** `StructuralDescriptor` (from Binary) + `ContextDescriptor` (from Web).
**Produces.** `ActivatedAxes` — weighted, signed axis scores + tension markers + primary findings list.
**Functions alongside.** Cultural Engine (3) reads the same context to produce its convention report; both feed Interpretive (4).

## The three filters

Each axis is multiplied by three independent weight modifiers and one sign:

1. **Genre Markedness** — Is this measurement notable for this genre, or is it the water? (Reads `genre-baselines.md` from shared.)
2. **Thematic Alignment** — Does meaning reinforce or contradict this structural reading? (Reads the 10-dimension thematic vector from `ContextDescriptor`.)
3. **Production Attribution** — Artistic choice or technical artifact? (Reads production credits from `ContextDescriptor`.)

## Spec & code

| File | Role |
|---|---|
| `module-activation.md` | Full spec — three-filter process, scoring formula, tunable parameters, tier thresholds |

## Notes

- No code yet. Spec-only stage. Implementation would be a thin function consuming the two descriptor objects and emitting the ActivatedAxes schema.
- Tunable parameters (water threshold, signal threshold, primary tier threshold, etc.) live in the spec and can be adjusted without engine changes.
