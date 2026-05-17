# 0 — Shared Protocol Layer

Pre-stage. Cross-cutting reference data and schemas every engine reads from.

**Role.** Defines the contracts, registries, and baselines that engines depend on but none of them owns. Changes here require a protocol version bump (see `shared-protocol.md`) and acknowledgement from every downstream engine.

**Consumed by.** All engines. Most heavily by Binary Engine (registries + baselines) and Activation Layer (markedness ranges + meta-dimensions).

**Functions alongside.** Everything.

## Contents

| File | Role |
|---|---|
| `shared-protocol.md` | Element Registry, axis pole definitions, protocol-version contract |
| `dictionary-schema.md` | Schema for dictionary entries — how each component reads them |
| `fingerprint-registry.md` | 64 sonic fingerprints across 10 categories — the atomic vocabulary |
| `genre-fingerprint-map.md` | 58 genres mapped to fingerprint IDs — the molecular vocabulary |
| `genre-baselines.md` | "What water looks like" — per-genre center scores on all 10 dimensions. Used by Activation's markedness filter. |
| `discovered-patterns.md` | Cross-song rules, production signatures, failure modes, co-production clusters |
| `genomic-frame.md` | The biological metaphor — genotype/phenotype/allele, convention lifecycle, karyotype terrain |
| `suppression-map.md` | The waveform format spec. Five vertical bounds, immune-response model, surprise-as-signal rule |
