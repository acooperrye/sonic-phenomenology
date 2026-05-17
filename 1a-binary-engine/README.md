# 1a — Binary Engine

Stage 1 of the pipeline, runs in parallel with Stage 1b (Web Engine).

**Role.** Audio in, structural descriptor out. Extracts 55 spectral elements from a WAV file with no awareness of artist, title, lyrics, or production credits. Produces inert measurements that the Activation Layer (Stage 2) will weight and sign.

**Consumes.** Audio file (WAV).
**Produces.** `StructuralDescriptor` per the schema in `engine-binary.md`.
**Functions alongside.** Web Engine (1b) — they can run simultaneously. Binary's `genre_hypothesis` can optionally seed Web's Phase A genre confirmation, but Web can run without it.

## Spec & code

| File | Role |
|---|---|
| `engine-binary.md` | Engine spec — I/O contracts, re-entry interface, the 55 elements |
| `compression_engine.py` | Compression vector extraction |
| `suppression_audio.py` | Encoder/decoder for the suppression-waveform output format (see `0-shared/suppression-map.md`) |

## Bound sub-modules

Each is versioned independently of the engine and lives in its own subfolder. Modules read from the engine's structural descriptor and contribute element-specific analysis back into it.

| Module | Folder | Role |
|---|---|---|
| Percussion | `modules/percussion/` | Per-element percussion meters, ghost-note discrimination, fusion test |
| Feltness | `modules/feltness/` | Somatic weight per frequency band — gesture model, polling model |
| Equipment ID | `modules/equipment-id/` | Three-tier instrument identification. Includes the 110KB equipment registry. |
| Electronica | `modules/electronica/` | Genre-triggered analysis for electronic music |
| Vocal Silhouette | `modules/vocal-silhouette/` | Subtractive vocal detection — voice is the residual after every other instrument is mapped |
| Slope Identity | `modules/slope-identity/` | Horizontal slope tracking — instrument identity across time |

## Lookup

| File | Role |
|---|---|
| `lookup/genre-fingerprint-lookup.md` | Binary Pass 1 — what the engine checks first |
