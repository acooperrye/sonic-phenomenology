---
date: 2026-05-17
severity: minor
status: pending
scope:
  stage: 1a-binary-engine
  submodule: slope-identity
  files:
    - 1a-binary-engine/modules/slope-identity/module-slope-identity.md
    - 1a-binary-engine/modules/percussion/module-percussion.md
    - 0-shared/genre-baselines.md
propagation:
  live_fetched: true
  skill_md_changed: false
origin_session: "example — illustrative only, kept for format reference"
---

# Slope-identity reads "flat" too readily on post-2022 mastered material

## Diagnosis

When the slope-identity sub-module evaluates compressed-vs-flat on modern
masters (especially post-2022 pop, hyperpop, and trap), it falls through to
"flat" because the genre-baseline threshold for "compressed" was calibrated
against pre-2022 loudness norms. Tracks mastered to current commercial
standards routinely cross that threshold and end up mislabeled. Observed
during analysis of [example track]; the slope reading was "flat" but the
actual character was "aggressively compressed with preserved transients."

This is a baseline drift, not a model failure — the module is doing what its
thresholds tell it to.

## Proposed Change

In `0-shared/genre-baselines.md`, update the slope-identity / compression
thresholds for the relevant modern-pop / electronica genres so they reflect
post-2022 mastering norms. Specifically, the "compressed" upper bound and
"flat" lower bound should both shift higher.

In `1a-binary-engine/modules/slope-identity/module-slope-identity.md`, add a
sentence noting that the thresholds are mastering-era-calibrated and pointing
at `0-shared/genre-baselines.md` as the source of truth (so future readers
don't expect hard-coded thresholds in the module spec itself).

In `1a-binary-engine/modules/percussion/module-percussion.md`, add a cross-
reference where percussion conditions on slope-identity output, noting the
downstream sensitivity to this calibration.

This change does not rename, reorder, or restructure any of the canonical
stage folders (0-shared, 1a-binary-engine, 1b-web-engine, 2-activation-layer,
3-cultural-engine, 4-interpretive-engine, dictionary, internal). It is a
content-only edit inside existing files.

## Why it matters

Slope-identity is a load-bearing read for the binary engine — downstream
modules in 2-activation-layer and 3-cultural-engine condition on it. A
flat-biased reading propagates wrongness through the whole pipeline for any
modern pop / electronica track, which is precisely the material that benefits
most from the framework. This is a high-leverage threshold to keep current.

## Validation

1. The three named files exist after edit and are non-empty.
2. The 0-4 stage folder names are unchanged (the post-edit validator confirms this).
3. The threshold values in `genre-baselines.md` moved in the intended direction
   (read both before-and-after lines manually to confirm).
4. Optional smoke test: re-run the slope-identity module on a known
   post-2022 test track from the private working folder
   (`/Documents/Music/Rhythm Dictionary/`) and confirm the slope reading
   moves from "flat" to "compressed."

## Rollback

`git revert <commit-sha>` on the processed commit. The change is content-only,
no schema migration, no downstream coordination needed. If a downstream
session reports the new thresholds are now too aggressive in the other
direction, recalibrate in a follow-up change order rather than reverting.
