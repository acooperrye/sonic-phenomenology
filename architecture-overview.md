# ARCHITECTURE OVERVIEW
## Rhythm Dictionary — Two-Engine Architecture
## 2026-02-11 (patched: Cultural Engine, fingerprint system, bridge taxonomy 7-type, Percussion Module, Interpretive Engine rename, Genomic Frame)

---

## Component Map

```
┌─────────────────────────────────────────────────────────────┐
│                   SHARED PROTOCOL LAYER                      │
│  Element Registry · Genre Baselines · Meta-Dimensions        │
│  Co-Production Cluster Defs · Weight/Status Table            │
│  Equipment Dictionary (spectral signatures per instrument)   │
│  Fingerprint Registry (64 atoms, 10 categories)             │
│  Genre-Fingerprint Map (58 genres → fingerprint IDs)        │
│  Genomic Frame (convention lifecycle, signed entrenchment)   │
└──────────┬──────────────────────────────────┬────────────────┘
           │                                  │
     ┌─────▼──────┐                    ┌──────▼─────┐
     │   BINARY   │                    │    WEB     │
     │   ENGINE   │                    │   ENGINE   │
     │            │                    │            │
     │  audio in  │                    │ artist +   │
     │  struct out │                    │ title in   │
     │            │                    │ context out│
     └─────┬──────┘                    └──────┬─────┘
           │                                  │
           │  GenreHypothesis +               │  ContextDescriptor
           │  SpectralRoster                  │  (incl. instrument roster)
           │                                  │
     ┌─────▼──────────────────────────────────┘
     │
     │  EQUIPMENT ID ENGINE (Phase A module)
     │  Runs after roster, before full extraction
     │  Input:  audio + SpectralRoster
     │  Output: EquipmentReport
     │    · programmed vs organic per role
     │    · synthesis family per role (FM/analog/sample/wavetable)
     │    · drum machine vs live drums
     │    · composite source evidence
     │
     ├──────────────────────────────────────────
     │
     │  CULTURAL CONTEXT ENGINE (Phase A module)
     │  Runs after genre commit
     │  Input:  GenreHypothesis + genre-fingerprint-map
     │  Output: ConventionReport
     │    · convention bank (10 universal + genre-specific)
     │    · fingerprint search targets for Binary Engine
     │    · expected shapes and violation signatures
     │    · bridge type signals when violations detected
     │  Files: engine-cultural.md, fingerprint-registry.md,
     │         genre-fingerprint-map.md
     │
     └─────┬──────────────────────────────────┐
           │                                  │
           │    StructuralDescriptor +        │    ContextDescriptor
           │    EquipmentReport +             │
           │    ConventionReport              │
           │                                  │
     ┌─────▼──────────────────────────────────▼────────────────┐
     │                  ACTIVATION LAYER                        │
     │  Three-filter scoring: genre × thematic × production     │
     │  EquipmentReport feeds production filter directly        │
     │  ConventionReport feeds genre markedness filter          │
     │  Output: weighted, signed axis scores + tension markers  │
     └─────────────────────────┬───────────────────────────────┘
                               │
                               │    ActivatedAxes
                               │
     ┌─────────────────────────▼───────────────────────────────┐
     │               PERCUSSION MODULE                          │
     │  Per-element timing analysis                             │
     │  ElementMeter (per-element onset grids + deviations)     │
     │  MeterRelationship (cross-element timing ratios)         │
     │  DeviationLog (every absence and extra vs expected grid) │
     │  Output feeds Interpretive Engine directly               │
     └─────────────────────────┬───────────────────────────────┘
                               │
                               │    ElementMeter + MeterRelationship
                               │    + DeviationLog
                               │
     ┌─────────────────────────▼───────────────────────────────┐
     │             INTERPRETIVE ENGINE                          │
     │  (renamed from Bridge Module, 11 Feb 2026)               │
     │  Passes 3-5: Hypothesis → Re-entry → Synthesis          │
     │  7 bridge types: Concealment, Compensation, Contradiction│
     │    Refusal, Conceit (1-5: Structure ↔ Theme)            │
     │    Excision, Inversion (6-7: Structure ↔ Semantics)     │
     │  EquipmentReport provides Concealment evidence           │
     │  ConventionReport provides Types 6-7 violation signals  │
     │  PercussionModule provides per-element deviation data    │
     │  Can call Binary Engine again at higher resolution       │
     │  SOMATIC DATA IS TIER 1 for bridge identification       │
     │  Exploratory — opens questions, does not conclude        │
     └─────────────────────────┬───────────────────────────────┘
                               │
                               │    InterpretiveFindings
                               │
     ┌─────────────────────────▼───────────────────────────────┐
     │                  THE CONVERSATION                        │
     │  Passes 6-8: Convergence · Naming · Diagnosis            │
     │  Human somatic data enters here                          │
     │  Bridge type confirmation requires somatic validation    │
     │  Two instruments measuring the same phenomenon —         │
     │  divergence between readings is itself a finding          │
     └─────────────────────────────────────────────────────────┘
```

---

## File Index

| File | Contains | Load when... |
|------|----------|-------------|
| `engine-binary.md` | Binary Engine spec, I/O contracts, re-entry interface | Workshopping audio extraction |
| `engine-web.md` | Web Engine spec, I/O contracts, search targets | Workshopping context retrieval |
| `engine-cultural.md` | Cultural Context Engine spec, convention bank (10 universal + genre-specific), ConventionFingerprint struct, validation pass examples | Workshopping cultural context, convention detection, or fingerprint-based analysis |
| `module-activation.md` | Activation Module spec, three-filter process, tier thresholds | Tuning filter weights or scoring |
| `engine-interpretive.md` | Interpretive Engine spec (renamed from Bridge Module), Passes 3-5, re-entry logic, 7 bridge types, divergence model | Workshopping somatic inference, bridge identification, interpretive synthesis |
| `module-bridge.md` | *Previous version of engine-interpretive.md, preserved for reference* | Historical reference only |
| `module-feltness.md` | Feltness module: derivative reframe, headroom analysis, gesture model, polling model | Workshopping somatic prediction or temporal analysis |
| `module-equipment-id.md` | Equipment ID Engine spec, I/O contracts, four detectors, pipeline position | Workshopping instrument identification or production attribution |
| `equipment_engine.py` | Working Python engine: TooCleanDetector, SynthesisFamilyClassifier, DrumClassifier, CompositeSourceDetector | Running equipment analysis on uploaded audio |
| `equipment-dictionary.md` | Spectral signatures per instrument/synth/drum machine, known patches, backtrace methodology | Cross-referencing detected signatures against known gear |
| `module-electronica.md` | Electronica Module spec, genre-triggered fusion recovery, grid analysis, production findings | Workshopping electronica-specific analysis or any genre-triggered module pattern |
| `phase-a-revised-cheap-pass.md` | Revised Phase A sequence, SpectralRoster, BandPresenceMap, ShiftMap, RoleTrajectory specs, Layer 4 Cultural Context | Workshopping cheap pass, structural sampling, or roster logic |
| `shared-protocol.md` | Element Registry, Genre Baselines, Meta-Dimensions, Co-Production Templates | Cross-cutting changes (co-load with target file) |
| `fingerprint-registry.md` | 64 sonic fingerprints across 10 categories, binary measurement codes | Running fingerprint-based detection or building genre manifests |
| `genre-fingerprint-map.md` | 58 genres mapped to fingerprint IDs (●/○/△/✱ markers) | Genre classification, convention lookup, or fingerprint search |
| `somatic-dictionary.md` | 15 correspondences, somatic gate model, bridge taxonomy (7 types), frequency-to-body map | Bridge identification, somatic prediction, or feltness analysis |
| `module-percussion.md` | Percussion Module spec, per-element timing, deviation detection, meter relationships | Workshopping rhythmic analysis, deviation detection, or meter inference |
| `genomic-frame.md` | Genomic Frame: biological architecture mapped onto music (organism/genome/chromosome/gene/allele), convention lifecycle (9 phases), signed entrenchment, three forces | Convention evolution, genre theory, or cross-component conceptual grounding |
| `karyotype-terrain.html` | Interactive terrain visualization of Music's genome — genre territories as topographic peaks | Visual reference for genre-territory relationships |
| `suppression-map.md` | Suppression Map: per-engine signal hygiene — which fingerprints NOT to poll per genre/band/phase, algebraic distributions, dormant/silent classification, surprise signal escalation, learning flywheel | Workshopping analysis efficiency, false positive reduction, or anomaly detection |
| `dictionary-schema.md` | Dictionary entry structure, how each component reads it | Schema changes or entry validation |

---

## Phase Flow

**Phase A — Parallel Extraction + Cheap Pass** (no dependencies between engines)
- Binary Engine: audio → `GenreHypothesis` (snapshot)
- Web Engine: artist + title → `ContextDescriptor` (expanded: instrument roster + section labels)
- Genre commit → loads baseline from Shared Protocol
- Spectral Roster build → `SpectralRoster`
- **Equipment ID Engine**: audio + `SpectralRoster` → `EquipmentReport`
  - Per-role: programmed vs organic classification
  - Per-role: synthesis family (FM / analog subtractive / sample-based / wavetable / acoustic)
  - Drums: drum machine vs live vs hybrid
  - Composite source detection per audible element
  - Cross-reference: compare detected signatures against Equipment Dictionary known entries
  - Cross-reference: compare detected instruments against Web Engine's instrument roster (convergence = high confidence)
- **Cultural Context Engine**: `GenreHypothesis` + `genre-fingerprint-map.md` → `ConventionReport`
  - Look up genre → get fingerprint IDs (●=defining, ○=common, △=occasional, ✱=violation)
  - Load fingerprint definitions from `fingerprint-registry.md` → get binary measurement codes
  - Assemble convention bank: 10 universal conventions + genre-specific conventions
  - Output: ranked search targets for Binary Engine full pass (what to look for, what shape to expect, what violations signal which bridge types)
  - Mechanism: convention bank is a search function, not a generation task. See `engine-cultural.md`.
- Structural sampling → `BandPresenceMap` (with slope detection per band)
- Shift point identification → `ShiftMap` (with transition type inference from slope data)
- Role trajectory derivation → `RoleTrajectory` + coarse dimensional hints

**Phase A.5 — Genre-Triggered Modules** (conditional, only for qualifying genres)
- Electronica Module: fusion recovery via tempo-locked subtraction, spectral neighbor triangulation, sequence pattern detection → `ElectronicaDescriptor` with recovered readings + production findings
- [Future genre modules occupy this same slot]
- Output feeds into Binary Engine's full pass as supplementary data

**Phase B — Sequential Interpretation** (each step feeds the next)
1. Binary Engine full pass: informed by ShiftMap + roster + `EquipmentReport` + `ConventionReport` + any genre module output → `StructuralDescriptor`
   - `EquipmentReport` informs interpretation: if drums are programmed, onset CV is water. If bass is FM, spectral flatness baseline shifts. If composite sources detected, stereo analysis needs band-separated treatment.
   - `ConventionReport` directs search: Binary Engine knows what fingerprints to look for and what violations to flag. Convention search targets are ranked by priority.
2. Activation Module: `StructuralDescriptor` + `ContextDescriptor` + `EquipmentReport` + `ConventionReport` → `ActivatedAxes`
   - `EquipmentReport` feeds production attribution filter directly: confirmed synthesis type + matching web credits = AUTHORED with high confidence
   - `ConventionReport` feeds genre markedness filter: violations flagged during binary pass are scored against expected shapes. Marked deviations from genre baseline get signal weight; expected features get water weight.
   - Composite source detection feeds Concealment bridge evidence
   - Convention violations feed Types 6-7 bridge evidence (Excision: expected semantic layer absent; Inversion: structural roles swapped)
3. Percussion Module: `StructuralDescriptor` → `ElementMeter[]` + `MeterRelationship` + `DeviationLog`
   - Per-element timing analysis: onset grids, deviation detection, absence/extra flagging
   - Cross-element meter ratios: how elements relate temporally to each other
   - Deviations are not errors — they are findings (what's missing or extra vs expected grid)
4. Interpretive Engine (renamed from Bridge Module, 11 Feb 2026): `ActivatedAxes` + `EquipmentReport` + `ConventionReport` + `ElementMeter[]` + `DeviationLog` → hypothesis → re-entry → `InterpretiveFindings`
   - **7 bridge types.** Types 1-5 (Structure ↔ Theme): Concealment, Compensation, Contradiction, Refusal, Conceit. Types 6-7 (Musical Structure ↔ Musical Semantics): Excision, Inversion. Both 6-7 are "biographical bridges" requiring pre-existing listener tension.
   - High composite source count → evidence for Concealment bridge
   - Programmed sources performing organic roles → evidence for Concealment
   - Live sources in mechanical context → evidence for Contradiction
   - Convention violations from ConventionReport → evidence for Excision (absence of expected element) or Inversion (role swap)
   - Percussion deviations from DeviationLog → evidence for structural meaning in timing choices
   - **Somatic data is Tier 1.** Computational data alone has never correctly identified a bridge type without somatic input. The Interpretive Engine can narrow candidates, but confirmation requires the Conversation.
   - **Exploratory, not conclusive.** The engine opens questions. It does not converge on a single correct reading. Divergence between Claude's reading and the listener's somatic report is itself a finding.
5. The Conversation: `InterpretivePresentation` + human somatic data → convergence assessment
   - Bridge type confirmation here, not in engine. The somatic gate model means the bridge IS the listener's response.

---

## Versioning Contract

```
SystemVersion {
  protocol_version:      string      // changes here require both engines to acknowledge
  binary_engine_version: string      // independent
  web_engine_version:    string      // independent
  cultural_engine_version: string    // independent (Phase A module, reads fingerprint registry + genre map)
  equipment_version:     string      // independent (Phase A module)
  percussion_version:    string      // independent (post-activation module)
  activation_version:    string      // tied to protocol version
  interpretive_version:  string      // independent (renamed from bridge_version, 11 Feb 2026)
  fingerprint_registry:  string      // independent (atoms — additive only)
  genre_fingerprint_map: string      // independent (molecules — additive only)

  // Compatibility matrix
  // Binary Engine vX.Y works with Protocol vA.B+
  // Web Engine vX.Y works with Protocol vA.B+
  // Equipment ID Engine is additive — other components work without it
  // Percussion Module is additive — Interpretive Engine works without it but loses per-element deviation data
  // Cultural Engine is additive — other components work without it but lose convention-directed search
  // Fingerprint Registry + Genre Map are read-only reference — additive changes don't break anything
  // If Protocol changes, both engines must update to acknowledge new fields
  // but can ignore new fields gracefully (additive changes don't break old engines)
}
```

### Breaking changes (require protocol version bump):
- Adding/removing/renaming an element in the Element Registry
- Changing axis pole definitions
- Adding/removing a meta-dimension
- Changing the ActivatedAxes output schema

### Non-breaking changes (engine-internal, no protocol bump):
- Binary Engine: fixing measurements, improving accuracy, adding resolution modes
- Web Engine: adding sources, refining thematic extraction, expanding genre set
- Cultural Engine: adding conventions, updating fingerprint thresholds, expanding genre manifests
- Equipment ID Engine: improving detectors, adjusting thresholds, adding instrument entries to Equipment Dictionary
- Fingerprint Registry: adding new fingerprints (additive only — never remove or renumber)
- Genre-Fingerprint Map: adding genres, updating fingerprint assignments (●/○/△/✱ markers)
- Activation Layer: tuning filter weights (0.2/2.0 thresholds)
- Percussion Module: adding element meters, adjusting deviation thresholds, adding meter relationship types
- Interpretive Engine (formerly Bridge): improving hypothesis generation, adding bridge types (currently 7: Concealment, Compensation, Contradiction, Refusal, Conceit, Excision, Inversion)
- Genre-triggered modules: adding new modules (e.g., electronica), updating existing module logic — these are additive and optional, no component depends on their output

---

## Testing Contracts

Each engine can be tested independently against the dictionary songs.

### Binary Engine test:
- Input: audio file for BG/OH/NTLTC/USC/EWTRTW
- Expected: fingerprint values within calibrated zones
- Score: self-match % against dictionary ground truth
- No web data needed

### Web Engine test:
- Input: artist + title for any dictionary song
- Expected: correct genre, thematic vector consistent with bridge analysis, production method matches known credits
- No audio data needed

### Equipment ID Engine test:
- Input: audio file for any dictionary song with known production credits
- Expected: drum classification matches known (EWTRTW → drum machine). Synthesis families match credited gear (EWTRTW → FM in DX7 regions, analog in Prophet regions). Composite source detection flags blended instruments.
- Cross-reference: compare against Equipment Dictionary entries
- Score: % of credited instruments correctly identified by synthesis family
- No web data needed (but web convergence checked as bonus)

### Activation Layer test:
- Input: pre-computed StructuralDescriptor + ContextDescriptor + EquipmentReport for a dictionary song
- Expected: primary findings match the documented bridge tensions for that song
- All cached outputs needed, but no engine runs live

### Cultural Engine test:
- Input: GenreHypothesis for any dictionary song with known genre
- Expected: convention bank correctly assembled (10 universal + genre-specific), fingerprint IDs correctly looked up, violation signatures correctly flagged against known dictionary data
- Score: % of known violations correctly detected (Circle Pit = 3 violations, all Type 7)
- No audio data needed (reads fingerprint registry + genre map only)

### Percussion Module test:
- Input: audio file for any dictionary song with known rhythmic structure
- Expected: per-element timing grids match known patterns. Deviations correctly flagged (e.g., Circle Pit: snare grid dissolving into texture, sub-bass shadow timing at +2.3ms). Meter relationships match documented cross-element ratios.
- Score: % of known deviations correctly detected
- No web data needed

### Interpretive Engine test (formerly Bridge Engine):
- Input: pre-computed ActivatedAxes + ConventionReport + ElementMeter[] + DeviationLog for a dictionary song
- Expected: somatic prediction aligns with the listener's documented somatic reports. Convention violations correctly signal candidate bridge types (especially Types 6-7). Percussion deviations correctly interpreted as structural meaning.
- Hardest to automate — somatic ground truth is conversational. Computational data alone has never correctly identified a bridge type.
- Divergence between Claude's reading and the listener's somatic report is itself a finding, not an error.

---

## Open Questions

1. **Parallel vs sequential?** Binary and Web can run simultaneously (web doesn't need audio data, binary doesn't need web data). The only dependency is the genre hypothesis handoff — binary can give web a head start on which genre to confirm. Worth running parallel?

2. **Activation Layer ownership.** It's currently defined as separate from both engines. Should it be a thin function that both engines know about, or a third module with its own version?

3. ~~**Bridge Engine re-entry depth.**~~ **Interpretive Engine re-entry depth (renamed 11 Feb 2026).** Currently allows one re-entry to Binary at higher resolution. Should it be allowed to loop (predict → check → revise → check again)? If so, what's the termination condition? *(Note: engine-interpretive.md specifies single round by default, human-directed extension, automated re-entry limit of 1 per engine per pass.)*

4. **Dictionary as shared state.** The dictionary entries are currently read-only reference for engines. When a new song is fully analyzed and added, should both engines auto-recalibrate, or is that a manual step?

5. **Web Engine caching.** For the same song, web results don't change. Should the Web Engine cache its ContextDescriptor so repeat analyses (e.g., after Binary Engine improvements) skip the web scrape?

6. **Stem separation pre-processing.** Would running Demucs or similar source separation before Equipment ID improve per-role classification? Probably yes but adds significant compute. Worth testing on EWTRTW where ground truth is known.

7. **Era priors for equipment ID.** Should genre/era hypothesis bias the synthesis family classifier? A 1985 synth-pop track is more likely DX7 than Serum. But priors can blind us. Current approach: classify blind, then check against era expectations. Revisit if false positive rate is high.
