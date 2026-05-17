# Sonic Phenomenology

A music analysis framework. Alex feels, Claude reads. Convergence across independent axes is the signal.

**The dictionary of what music does to a body.**

→ Public brief: [atcooper.net/tools/sonic-phenomenology](https://atcooper.net/tools/sonic-phenomenology)

## Install as a Claude plugin

If you use Cowork or Claude Code, you can add this repo as a plugin marketplace and install the loader skill in one step:

```
/plugin marketplace add acooperrye/sonic-phenomenology
/plugin install sonic-phenomenology@sonic-phenomenology
```

Then trigger it in any session by saying something like *"do a sonic-phenomenology read of [track]"* or *"what is this song doing to me."* The skill fetches the engine specs live from this repo each session, so dictionary and baseline updates propagate without reinstalling.

See [plugins/sonic-phenomenology/README.md](./plugins/sonic-phenomenology/README.md) for plugin-specific docs.

---

## What this is

A multi-stage pipeline for analysing music that treats the body as a measurement instrument alongside the spectrogram.

Audio enters the Binary Engine, which emits a structural descriptor across 55 spectral elements. In parallel, artist and title enter the Web Engine, which emits a context descriptor — genre, thematic vector across ten meta-dimensions, and production method. The two outputs meet at the Activation Layer, which applies three filters (genre markedness, thematic alignment, production attribution) to turn 216 inert axis readings into weighted, signed findings. The Cultural Engine scores entrenchment and convention violation. The Interpretive Engine runs seven bridge types looking for somatic-structural tension, with the option to re-enter Binary or Web at higher resolution. The conversation closes when human somatic data enters as the final pass and overrules engine values where they disagree.

What a genre **ignores** is its identity. The suppression map for each genre is the dominant signal; a 3σ deviation from a suppressed region is worth more than a 1σ deviation from an active one. The system is running an attention budget, not a classifier.

Each genre's complete analysis is encoded as actual audio — suppression profile on the left channel, discovery profile on the right channel, the gap in stereo is where the findings live. The audio self-documents (17kHz watermark + FSK-encoded JSON metadata in the final two seconds). **The audio IS the data.**

---

## The pipeline at a glance

```
Stage 1 (parallel)
  ┌──────────────┐         ┌──────────────┐
  │  1a Binary   │         │  1b Web      │
  │              │         │              │
  │ audio →      │         │ artist +     │
  │ structural   │         │ title →      │
  │ descriptor   │         │ context      │
  └──────┬───────┘         └──────┬───────┘
         │                        │
         └─────────┬──────────────┘
                   ▼
         ┌──────────────────────┐
         │  2 Activation Layer  │
         │  three-filter scoring │
         └──────────┬───────────┘
                    │
                    ▼
         ┌──────────────────────┐
         │  3 Cultural Engine   │
         │  convention bank,    │
         │  entrenchment        │
         └──────────┬───────────┘
                    │
                    ▼
         ┌──────────────────────┐
         │  4 Interpretive      │
         │  seven bridge types, │
         │  re-entry to 1a/1b   │
         └──────────┬───────────┘
                    │
                    ▼
            The Conversation
        (human somatic data — overrules)
```

---

## Folder layout (one engine per folder, downloadable standalone)

| Stage | Folder | Engine / layer | What's in it |
|---|---|---|---|
| 0 | `0-shared/` | Shared Protocol Layer | Schemas, registries, baselines every engine reads from |
| 1a | `1a-binary-engine/` | **Binary Engine** | Audio → structural. 6 bound sub-modules (percussion, feltness, equipment-id, electronica, vocal-silhouette, slope-identity) |
| 1b | `1b-web-engine/` | **Web Engine** | Artist+title → context (genre + thematic vector + production) |
| 2 | `2-activation-layer/` | Activation Layer | Three-filter scoring (markedness · alignment · attribution) |
| 3 | `3-cultural-engine/` | **Cultural Engine** | Convention bank, entrenchment curves, violation signals |
| 4 | `4-interpretive-engine/` | **Interpretive Engine** | Seven bridge types, somatic inference, re-entry. 3 bound sub-modules (bridge, somatic-gate, harmonic-resynthesis) |
| — | `dictionary/` | The dictionary | Song-level fingerprints with validated inferences |
| — | `internal/` | Work in progress | Drafts and implementation notes not part of the public surface |

Each engine folder is self-contained: its own README, spec, bound code, and bound modules. To download just one engine, take its folder.

---

## Top-level orientation

| File | What it is |
|---|---|
| `README.md` | This file — orientation, pipeline, file map |
| `architecture-overview.md` | Master architecture map — phase flow, versioning contract, component map |
| `architecture-linear-flow.md` | Deeper top-down reference — two-phase pipeline detail |
| `liner-notes.md` | Album sleeve — extended writing on what this is, how it works, principles |
| `brief-of-self.md` | Onboarding for a Claude collaborator joining a session |

---

## Versioning

Each engine versions independently. Cross-engine compatibility is governed by the protocol version in `0-shared/shared-protocol.md`.

**Breaking changes (require protocol version bump):** adding/removing/renaming an element in the Element Registry · changing axis pole definitions · adding/removing a meta-dimension · changing the `ActivatedAxes` output schema.

**Non-breaking changes (engine-internal, no protocol bump):** fixing measurements · improving accuracy · adding resolution modes · refining thematic extraction · expanding the genre set · tuning filter weights · adding bridge types.

---

## Status

Prototype. Active development. The two-engine spine plus Activation, Cultural, and Interpretive engines are all specified. Twenty fully characterised genre baselines across fifty-eight named genres, sixty-four sonic fingerprints, 103 gridline positions. The suppression-waveform format has been tested end-to-end on eleven recordings.

Coverage gaps the framework knows about:

- **Interval function.** The existing modules score interval presence but not whether an interval is doing resolution work or geometric work. A candidate parameter (provisionally *interval-role*) is in design.
- **Descriptor-context interaction.** The dictionary stores spectral vectors rather than adjectives precisely because adjective assignment is lossy, but the framework does not yet formally model how a timbral term's referent shifts as the surrounding contextual mass changes over time.
- **Unsupervised use.** The framework runs on recordings within the trained registry. Generalisation to genres outside the current baselines is open.

---

## Two entry points

```
Alex feels. Claude reads. Convergence across independent axes is the signal.
```
