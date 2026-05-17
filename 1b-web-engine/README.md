# 1b — Web Engine

Stage 1 of the pipeline, runs in parallel with Stage 1a (Binary Engine).

**Role.** Artist + title in, context descriptor out. Retrieves genre classification, thematic vector (scored on 10 meta-dimensions), and production method from web sources. Produces inert facts that the Activation Layer (Stage 2) uses to weight, sign, and filter the Binary Engine's structural readings.

**Consumes.** Artist string, title string. Optionally Binary's `genre_hypothesis` for Phase A confirmation.
**Produces.** `ContextDescriptor` per the schema in `engine-web.md`.
**Functions alongside.** Binary Engine (1a) — parallel, no required data dependency.

## Spec & code

| File | Role |
|---|---|
| `engine-web.md` | Consolidated engine spec — purpose, I/O, the 10 meta-dimensions, search targets, re-entry, caching |
| `web-content-axis-scoring.md` | Operational reference: per-element scoring map (all 54 elements × 4 axes with genre filter logic, thematic inverters, production attribution guidance). The runtime lookup table. |

## Notes

- No bound sub-modules. Web Engine is a single component.
- Caches `ContextDescriptor` per (artist, title) — web results don't change between binary engine revisions.
- Two superseded source files (`engine-web-original.md`, `snapshot-thematic-dimensionality-web-scoring.md`) are marked for deletion — their content was merged into `engine-web.md`.
