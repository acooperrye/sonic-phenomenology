# ARCHITECTURE: TWO-PHASE PIPELINE
## Rhythm Dictionary — Top-Down Reference
## 2026-02-10 (revised: refinement pass added, two-phase pipeline)

---

## A. ALL COMPONENTS

### Phase Assignment

| # | Component | Phase | Role |
|---|-----------|-------|------|
| 1 | Binary Engine (snapshot) | 1 — Orientation | Quick phenotype: 10-sec FFT → genre hypothesis |
| 2 | Web Engine (genre seed) | 1 — Orientation | Quick genotype: genre confirmation + instrument roster |
| 3 | Genre Commit | 1 — Orientation | Training wheels: lock the genre baseline |
| 4 | Spectral Roster Build | 1 — Orientation | Frequency-domain prediction: which bands matter |
| 5 | Production Environment | 1 — Orientation | Processing-domain prediction: which modifiers are in play |
| 6 | Binary Engine (full) | 1 — Phenotype | Full spectral/structural extraction: what IS in the audio |
| 7 | Web Engine (full) | 1 — Genotype | Full context retrieval: what SHOULD be in the audio |
| 8 | Equipment Signal Extraction | 1 — Phenotype | Harmonic profiles, repetition CVs, spectral signatures (raw, no classification) |
| 9 | Percussion Signal Extraction | 1 — Phenotype | Onsets, grid, tempo, elements (raw, no deviation log) |
| 10 | Feltness Signal Extraction | 1 — Phenotype | Per-event gesture measurement (raw, no somatic mapping) |
| 10b | Vocal Silhouette Extraction | 1 — Phenotype | Horizontal spectral analysis of vocal band (raw, no vocal classification) |
| 11 | Spectral Roster Refinement | 1 — Refinement | Full re-evaluation of band assignments using extraction data |
| 12 | Production Environment Refinement | 1 — Refinement | Full re-evaluation of modifier log using extraction data |
| 13 | Equipment Classification | 2 — Analysis | Match against registry (modifier-aware) |
| 14 | Binary Markedness | 2 — Analysis | Flag deviations from baseline (modifier-aware) |
| 15 | Percussion Deviation Analysis | 2 — Analysis | Compare grid against expectations (modifier-aware) |
| 16 | Feltness Interpretation | 2 — Analysis | Gesture → somatic mapping (modifier-aware) |
| 16b | Vocal Silhouette Interpretation | 2 — Analysis | Vocal presence scoring, type classification, production treatment (modifier-aware) |
| 17 | Cultural Engine | 2 — Analysis | Convention bank + violation detection |
| 18 | Activation Module | 2 — Analysis | Three-filter scoring |
| 19 | Interpretive Engine | 2 — Analysis | Hypothesis, re-entry, synthesis |
| 20 | Suppression System | 2 — Analysis | Weight adjustment overlay |
| 21 | The Conversation | 2 — Analysis | Human confirmation + dictionary updates |
| D1 | Diagnostic Resynthesis | Post-pipeline | Background: trajectory synthesis, residual isolation, feedback |

### Registries & Data Stores (12 total)

| Registry | Stored In | Phase 1 Access | Phase 2 Access |
|----------|-----------|:-:|:-:|
| Element Registry (55 elements) | `shared-protocol.md` | YES — what to measure + modifier targets | YES — weight overrides |
| Prescriptive Genre Prints (20) | `shared-protocol.md` + `genre-fingerprint-lookup.md` | YES — baseline + expected production profile | YES — markedness ranges |
| Descriptive Genre Prints (5) | `shared-protocol.md` | YES — empirical baselines | YES — empirical baselines |
| Meta-Dimensions (10) | `shared-protocol.md` | NO | YES — dimension aggregation |
| Co-Production Cluster Templates (5) | `shared-protocol.md` | NO | YES — cluster scoring |
| Spectral Roster Roles (7-8) | `shared-protocol.md` | YES — band assignment + modifier scope | NO |
| Fingerprint Registry (64) | `fingerprint-registry.md` | NO | YES — convention shapes |
| Genre-Fingerprint Map (58 genres) | `genre-fingerprint-map.md` | NO | YES — genre → convention lookup |
| Dictionary Entries (11 songs) | `dictionary-entries.md` | NO | YES — novelty, pattern matching |
| Equipment Registry | `equipment-registry.md` + `.wav` | YES — equipment ceilings (Step 5) + signatures (R2) | YES — classification |
| Somatic Dictionary (15 correspondences) | `somatic-dictionary.md` | NO | YES — gesture → body mapping |
| Genomic Frame | `genomic-frame.md` | NO | YES — entrenchment curves |

---

## B. PHASE 1 — GENOTYPE + PHENOTYPE + ENVIRONMENT

**Binary and Web run in parallel.** Binary reads the waveform and characterises what IS in the audio (phenotype). Web gathers context about what SHOULD be in the audio (genotype). Before the full tracks begin, orientation builds two bounding hypotheses: the spectral roster (which frequency bands matter) and the production environment (which processing modifiers are in play). Together these frame the outer envelope — enough to later trim off the fat. After the full tracks complete, a refinement pass revisits both hypotheses in tandem using actual extraction data, upgrading them from informed guesses to evidence-backed frames.

**Character:** Light at the top (orientation), heavier as it deepens (full extraction), then a focused refinement before handoff. Exploratory — hypothesis-generating, not hypothesis-testing. Everything here can be cached and re-analyzed.

**Genetics frame:** Orientation locates the genes (genotype seed), observes the phenotype (snapshot), and hypothesises the allele — the specific production environment the song exists within. The full tracks extract detailed genetic and phenotypic evidence. The refinement pass upgrades the allele hypothesis with real evidence. Phase 2 then determines whether each allele expression is dominant, recessive, or a novel mutation.

### B.1: Orientation (Steps 1-5)

Training wheels — kept until the registry has hundreds of fingerprints.

```
STEP 1: SNAPSHOT                                    [WAVEFORM]
─────────────────
  Engine:    Binary Engine (snapshot mode)
  Does:      10-sec FFT → 15 discriminators → GenreHypothesis
  Reads:     ◆ Element Registry — which 15 to measure
             ◆ Prescriptive Genre Prints — fingerprint comparison
  Produces:  GenreHypothesis

              │
              ▼

STEP 2: WEB GENRE + CONTEXT SEED              [NOT WAVEFORM]
────────────────────────────────
  Engine:    Web Engine (Phase A)
  Does:      Genre confirmation + instrument roster + section labels
  Reads:     ◆ Prescriptive Genre Prints
  Produces:  GenreConfirmation + instrument roster + section boundaries

              │
              ▼

STEP 3: GENRE COMMIT                          [NOT WAVEFORM]
────────────────────
  Does:      Binary hypothesis + Web confirmation → commit
  Reads:     ◆ Prescriptive Genre Prints — load baseline
             ◆ Descriptive Genre Prints — if n≥3, use as primary
  Produces:  GenreCommitment (genre_id + baseline + confidence)

              │
              ▼

STEP 4: SPECTRAL ROSTER BUILD                 [NOT WAVEFORM]
─────────────────────────────
  Does:      Genre conventions + web instrument data → role map
  Reads:     ◆ Spectral Roster Roles — 8-role taxonomy
             ◆ Genre-Fingerprint Map — default role sets per genre
             ◆ Web instrument roster (Step 2)
  Produces:  SpectralRoster (8 roles, spectral homes, envelopes)

  Frequency-domain bounding hypothesis. Predicts which bands
  the song occupies and what roles live in each band.
  Initial pass — refined by R1 after full extraction.

              │
              ▼

STEP 5: PRODUCTION ENVIRONMENT HYPOTHESIS     [NOT WAVEFORM]
─────────────────────────────────────────
  Does:      Genre + web + snapshot evidence → production bounds

             Two axes of the same informed hypothesis as the
             spectral roster: where Step 4 predicts which
             frequency bands the song occupies, Step 5 predicts
             which processing modifiers shape its envelope.
             Together they frame the outer bounds — enough to
             later trim off the fat.

             From the snapshot (10-sec FFT):
             · Crest factor → bus compression evidence
             · Spectral tilt → mastering EQ / tape colour
             · Stereo width + correlation → stereo processing
             · Spectral ceiling → sample rate / equipment limit
             · Pitch centre → tape speed-up (consistent sharp/flat)
             · Noise floor character → analog vs digital chain
             · Reverb tail presence → global reverb hypothesis

             From the web seed (Step 2):
             · Era context → expected production conventions
             · Genre → production profile
             · Any early equipment/studio/engineer mentions

             From the genre baseline (Step 3):
             · Expected reverb, compression, EQ norms

  Reads:     ◆ Snapshot data (Step 1)
             ◆ Web seed (Step 2)
             ◆ GenreCommitment (Step 3)
             ◆ SpectralRoster (Step 4)
             ◆ Prescriptive Genre Prints — expected production profile
             ◆ Equipment Registry — known equipment ceilings
  Produces:  ModifierLog (initial hypothesis — many entries will
             be partial or stub at this stage)

  Processing-domain bounding hypothesis. Predicts which
  production modifiers are present and how they colour the mix.
  Initial pass — refined by R2 after full extraction.

              │
              ├──────────────────────────────────┐
              ▼                                  ▼
        PHENOTYPE TRACK                    GENOTYPE TRACK
        (binary — waveform)                (web — context)

  Steps 4 and 5 complete orientation. The phenotype and
  genotype tracks can begin as soon as the roster (Step 4)
  is ready — they run in parallel with Step 5.
```

### B.2: Phenotype Track (what IS in the audio)

Reads the waveform. Extracts all raw measurements. Genre-informed (knows what genre, has the roster) but does NOT compare against conventions or expectations. Produces inert measurements.

```
P1: ROSTER CONFIDENCE CHECK
───────────────────────────
  Does:      Compare snapshot spectral data against SpectralRoster
  Reads:     Snapshot data (Step 1) + SpectralRoster (Step 4)
  Produces:  Confidence check (may loop to Step 2 on mismatch)

              │
              ▼

P2: EQUIPMENT SIGNAL EXTRACTION                   [WAVEFORM]
───────────────────────────────
  Does:      Per-role spectral analysis:
             · CV of spectral centroid, amplitude, attack, decay
               across repeated events (raw repetition profiles)
             · Harmonic analysis: fundamental, partials, integer
               ratio score, sideband energy, odd harmonic bias,
               rolloff slope, aliasing score, drift
             · Cross-band envelope correlation per spectral region
             · Spectral discontinuity detection
             · Attack onset spread per band
  Reads:     ◆ Audio + SpectralRoster (which bands to analyze)
  Produces:  EquipmentSignals
             (per-role spectral profiles, harmonic analyses,
              repetition CVs, envelope correlations —
              raw numbers, NO classifications)

              │
              ▼

P3: STRUCTURAL SAMPLING                           [WAVEFORM]
───────────────────────────
  Does:      5-8 section-targeted FFTs, filtered to roster bands
  Reads:     SpectralRoster + web section boundaries (Step 2)
  Produces:  BandPresenceMap
             (per-section band presence + intensity)

              │
              ▼

P4: SHIFT POINT IDENTIFICATION
──────────────────────────────
  Does:      Compare BandPresenceMap across sections
             (song against itself, not against genre expectation)
  Reads:     BandPresenceMap (P3)
  Produces:  ShiftMap (shift points + severity)
             RoleTrajectory (per-role presence timeline)

              │
              ▼

P5: FULL BINARY EXTRACTION                        [WAVEFORM]
──────────────────────────
  Does:      55 elements × variable sections
             High-res at shift points, standard elsewhere
  Reads:     ◆ Audio
             ◆ Element Registry — what to measure (IDs, categories)
             ◆ SpectralRoster — band assignment for source-dependent elements
             ◆ ShiftMap — resolution targeting
  Produces:  RawBinaryReadings
             (elements[], per-section trajectories, co_production
              candidates — NO markedness flags, NO roster deviations,
              NO legibility estimate)

     ┌─── PARALLEL WITH P5 ────────────────────────────────┐

P6: PERCUSSION SIGNAL EXTRACTION                   [WAVEFORM]
────────────────────────────────
  Does:      Onset detection per band
             Grid derivation (phenotypic, from audio evidence)
             Tempo via autocorrelation
             Element isolation (kick/snare/hat candidates)
             Cross-element timing relationships
             Ghost note candidates (quiet but rhythmically placed)
  Reads:     ◆ Audio
             ◆ SpectralRoster — percussive role bands
             ◆ GuidingPrior (web-sourced BPM — orientation, not expectation)
             ◆ Binary Engine timestamps (concurrent cross-reference
               for bass-band disambiguation)
  Produces:  PercussionGrid
             (onsets per band, derived grid, tempo,
              element candidates, cross-element timing,
              ghost note candidates —
              NO deviation log, NO meter interpretation)

P7: FELTNESS SIGNAL EXTRACTION                     [WAVEFORM]
──────────────────────────────
  Does:      Per-event gesture measurement:
             · Onset slope (dE/dt, positive)
             · Sustain duration
             · Offset slope (dE/dt, negative)
             · Silence duration before next onset
             · Derivative computation per band
  Reads:     ◆ Audio
             ◆ Percussion timestamps (P6, concurrent)
             ◆ Binary band data (P5, concurrent)
  Produces:  GestureMeasurements
             (per-event four-parameter gesture profiles,
              per-band derivative curves —
              NO somatic mapping, NO punch/atmosphere classification)

P-VOX: VOCAL SILHOUETTE EXTRACTION                 [WAVEFORM]
────────────────────────────────────
  Does:      Horizontal spectral analysis of the vocal band:
             · Band isolation (200-4000Hz vocal, 4-8kHz sibilance)
             · Formant contour tracking (F1, F2, F3 ridges across time)
             · Phrase envelope detection (breath-scale amplitude structure)
             · Syllabic modulation measurement (3-6Hz AM in vocal band)
             · Pitch continuity analysis (glide vs step ratio)
             · Vibrato detection (4.5-7.5Hz pitch modulation)
             · Sibilance-vocal cross-band correlation
             · Sectional vocal presence scoring
             · Vocal band production cues (reverb tail, compression,
               de-essing evidence)
  Reads:     ◆ Audio
             ◆ SpectralRoster (Step 4 — vocal band boundaries)
  Produces:  VocalSilhouette
             (formant_tracks, phrase_envelope, syllabic_modulation,
              pitch_profile, vibrato_profile, sibilance_coupling,
              sectional_map, production_cues —
              raw measurements, NO vocal classification,
              NO presence/absence determination)
  Note:      Does NOT read percussion grid (P6). Does NOT depend
             on equipment signals (P2). Looks only at the vocal
             spectral band, horizontally, across phrase-length
             windows. Parallel with P5-P7.

     └─── END PARALLEL ────────────────────────────────────┘
```

### B.3: Genotype Track (what SHOULD be in the audio)

Runs in parallel with the phenotype track. No waveform access. Retrieves context that informs what equipment, production methods, and compositional choices are expected.

```
P8: WEB DATA GATHERING                        [NOT WAVEFORM]
───────────────────────
  Does:      Retrieve: thematic content, production credits,
             co-production info, web-only element data,
             known equipment, studio, engineer, era context
  Reads:     ◆ Element Registry — which elements are broken/web-only
  Produces:  RawWebData
             (thematic content, credits, co-production mentions,
              element population data, equipment mentions,
              production technique mentions —
              NO thematic vector scoring, NO production classification)
```

### B.4: Refinement Pass (in tandem)

After the phenotype and genotype tracks complete, the two bounding hypotheses from orientation get a full refinement pass. Steps 4 and 5 were light predictions from a 10-second snapshot and web seed. Now the system has actual extraction data — real equipment signals, real percussion grids, real structural maps, real web context. The refinement pass upgrades both hypotheses using this evidence, running the two in tandem so they can interface with each other and with the extraction data they depend on.

```
     ┌─── IN TANDEM ──────────────────────────────────────┐

R1: SPECTRAL ROSTER REFINEMENT                [NOT WAVEFORM]
──────────────────────────────
  Does:      Full re-evaluation of the spectral roster using
             actual extraction data:
             · Compare predicted band assignments against P2
               equipment signals (are the bands where we said?)
             · Compare predicted percussive roles against P6
               percussion grid (kick/snare/hat in expected bands?)
             · Absorb P3 band presence map (which bands are
               actually occupied vs predicted)
             · Cross-reference P4 role trajectory (do roles
               shift across sections as expected?)
             · Incorporate P8 web data (any new instrument info
               beyond what Step 2 seed provided)
             · Update spectral homes, envelopes, role assignments
             · Flag unexpected occupancy (instrument in wrong band)
             · Flag unexpected vacancies (predicted band empty)

  Reads:     ◆ SpectralRoster (Step 4 — initial hypothesis)
             ◆ EquipmentSignals (P2)
             ◆ PercussionGrid (P6)
             ◆ BandPresenceMap (P3)
             ◆ RoleTrajectory (P4)
             ◆ RawWebData (P8)
             ◆ Spectral Roster Roles — taxonomy reference
  Produces:  SpectralRoster (refined — replaces initial)

  Interface with R2: if R2 confirms a tape speed-up, R1
  adjusts expected frequency positions accordingly. If R2
  identifies a spectral ceiling, R1 knows not to expect
  energy above that frequency.


R2: PRODUCTION ENVIRONMENT REFINEMENT         [NOT WAVEFORM]
─────────────────────────────────────
  Does:      Full re-evaluation of the modifier log using
             actual extraction data:
             · Cross-reference P2 equipment signals against
               modifier hypotheses — does spectral evidence
               confirm the snapshot's compression guess?
             · Use P6 percussion grid timing data to detect
               timing-domain modifiers (swing quantise, tape wow)
             · Use P5 full binary readings for detailed
               crest factor, spectral tilt, stereo analysis
               (beyond what the 10-sec snapshot could see)
             · Use P7 gesture measurements for envelope-domain
               modifiers (compression pumping, reverb on transients)
             · Cross-reference P8 web production credits
               against binary evidence for convergence
             · Resolve stubs where extraction evidence is sufficient
             · Upgrade partial → full where evidence converges
             · Add new modifiers discovered in full extraction
             · Demote or remove modifiers the full data contradicts

  Reads:     ◆ ModifierLog (Step 5 — initial hypothesis)
             ◆ EquipmentSignals (P2)
             ◆ PercussionGrid (P6)
             ◆ RawBinaryReadings (P5)
             ◆ BandPresenceMap (P3)
             ◆ GestureMeasurements (P7)
             ◆ RawWebData (P8)
             ◆ Equipment Registry — canonical signatures
             ◆ Prescriptive Genre Prints — production profiles
  Produces:  ModifierLog (refined — replaces initial)

  Interface with R1: R1's revised band assignments inform
  R2's per-band modifier characterisation (e.g., if R1 moves
  the bass role down a band, R2 re-evaluates low-frequency
  saturation evidence in the correct region).

     └─── END TANDEM ─────────────────────────────────────┘

  R1 and R2 interface once, then commit. The refined
  SpectralRoster and ModifierLog replace the initial
  hypotheses in Phase1Output. Phase 2 analysis modules
  receive the evidence-backed versions, not the orientation
  guesses.

  What the refinement pass is NOT:
  · Not analysis — it doesn't compare against conventions
  · Not classification — it doesn't name equipment or flag violations
  · Not interpretation — it doesn't score or synthesise
  It is the second pass of the same bounding work that
  Steps 4 and 5 did, now with real data instead of a
  10-second glimpse and a web search.
```

### B.5: The Modifier Log

The production environment hypothesis (Step 5) produces an initial ModifierLog. The refinement pass (R2) enriches it with full extraction data. This is the schema — entries are sparse at orientation, accumulate through refinement, and continue to evolve through Phase 2.

```
ModifierLog {
  modifiers: [
    {
      type:               string          // "reverb" | "compression" | "eq" |
                                          // "saturation" | "stereo" | "tape" |
                                          // "sample_ceiling" | "bus_processing"
      scope:              string          // "global" | "selective"
      affected_elements:  string[]        // which roster roles / bands
      confidence:         float           // 0.0–1.0
      characterisation:   string          // "full" | "partial" | "stub"
      parameters:         {}              // whatever is known (decay time,
                                          // ratio, cutoff, etc.) — may be empty
      source_evidence:    string[]        // what data points support this
      downstream_action:  string          // "subtract" | "compensate" |
                                          // "widen_tolerance" | "flag_only"
    },
    ...
  ]

  // Example entries for EWTRTW:
  //
  // { type: "reverb", scope: "global", confidence: 0.9,
  //   characterisation: "partial",
  //   parameters: { character: "small hall", decay_est: "1.2s" },
  //   source_evidence: ["binary: consistent tail across all elements",
  //                     "web: single reverb bus confirmed"],
  //   downstream_action: "compensate" }
  //
  // { type: "tape", scope: "global", confidence: 0.95,
  //   characterisation: "full",
  //   parameters: { pitch_offset: "+33 cents", speed_factor: 1.019 },
  //   source_evidence: ["binary: all pitches 33c sharp of concert",
  //                     "web: tape speed-up confirmed"],
  //   downstream_action: "subtract" }
  //
  // { type: "sample_ceiling", scope: "selective",
  //   affected_elements: ["percussive-high", "percussive-mid"],
  //   confidence: 0.85, characterisation: "full",
  //   parameters: { ceiling_hz: 14000, source: "8-bit/28kHz equipment" },
  //   source_evidence: ["binary: spectral cliff at 14,812 Hz",
  //                     "web: Drumulator confirmed"],
  //   downstream_action: "flag_only" }
  //
  // { type: "saturation", scope: "selective",
  //   affected_elements: ["sustained-low"],
  //   confidence: 0.4, characterisation: "stub",
  //   parameters: {},
  //   source_evidence: ["binary: even harmonic bump in bass region"],
  //   downstream_action: "widen_tolerance" }

  song_id:            string
  processing_version: string
  timestamp:          ISO datetime

  // LIVING DOCUMENT: Phase 2 modules can:
  // · Resolve stubs (upgrade characterisation as more data emerges)
  // · Add new modifiers discovered during analysis
  // · Update confidence based on convergence with other findings
  // · Flag irreconcilable modifiers as research agenda items
}
```

**What Steps 4-5 do NOT do (and R1-R2 also do NOT do):**
- Do not judge whether production choices are conventional or surprising (that's Phase 2 — Cultural Engine)
- Do not classify equipment (that's Phase 2 — Equipment Classification)
- Do not compare against convention banks (that's Phase 2)
- Do not read the waveform (that was Steps 1-3, and then P1-P7)

**What Steps 4-5 + R1-R2 enable:**
- Phase 2 modules receive evidence-backed roster and modifier log, not orientation-level guesses
- Fully characterised modifiers can be subtracted (e.g., remove 33 cents pitch offset before equipment frequency matching)
- Partially characterised modifiers trigger wider tolerances (e.g., "there's reverb here, exact character unknown, so decay measurements may be contaminated")
- Stubs tell Phase 2 modules "your measurement here is dirty — lower your confidence or flag for re-entry"
- Unresolvable stubs become the system's research agenda — textures it can detect but can't name yet

### B.6: Phase 1 Output

Everything below this line is cached, re-analyzable, and does not require re-reading the waveform.

```
Phase1Output {
  // From Orientation → Refinement
  genre:              GenreCommitment
  roster:             SpectralRoster         // Step 4 initial → R1 refined
  modifier_log:       ModifierLog            // Step 5 initial → R2 refined
  web_seed:           { instrument_roster, section_boundaries }

  // From Phenotype Track
  equipment_signals:  EquipmentSignals       // P2: raw spectral profiles
  band_presence:      BandPresenceMap        // P3: per-section presence
  shift_map:          ShiftMap               // P4: structural transitions
  role_trajectory:    RoleTrajectory         // P4: per-role timeline
  binary_raw:         RawBinaryReadings      // P5: 55 elements, raw
  percussion_grid:    PercussionGrid         // P6: onsets, grid, tempo
  gesture_raw:        GestureMeasurements    // P7: per-event gestures
  vocal_silhouette:   VocalSilhouette        // P-VOX: horizontal vocal band analysis

  // From Genotype Track
  web_raw:            RawWebData             // P8: thematic, credits, equipment

  // Metadata
  song_id:            string
  audio_hash:         string                 // for cache validation
  processing_version: string
  timestamp:          ISO datetime
}
```

This object is the complete signal extraction for one song. It can be cached indefinitely, re-analyzed under different hypotheses, and compared across songs without re-reading any waveforms.

---

## C. PHASE 2 — ANALYSIS + DICTIONARY

**All comparison, interpretation, and learning.** No waveform access. Reads Phase1Output (including refined SpectralRoster + ModifierLog) + all registries. Compares measurements against conventions, baselines, dictionaries — with production modifiers accounted for.

**Character:** Hard processing. This is where things get tested, confirmed, and written back to dictionaries. Convention-comparative. Production-aware. The refined modifier log means every comparison happens on cleaner signal — fingerprints without the runoff hue of production.

**Genetics frame:** Phase 1 located the genes, observed the phenotype, hypothesised the allele, and then refined it with extraction evidence. Phase 2 determines whether each allele expression is dominant, recessive, or a novel mutation.

Can be rerun against cached Phase1Output with different genre hypotheses, different convention banks, different modifier logs, or updated registries — without re-reading the waveform.

```
═══════════════════════════════════════════════════════════
 PHASE 2 — ANALYSIS + DICTIONARY
 Receives: Phase1Output (including refined SpectralRoster
           + ModifierLog from R1/R2)
 Reads: all registries
 Does NOT read: waveform
 All modules are MODIFIER-AWARE: they consult the ModifierLog
 before comparing against canonical values, and adjust
 tolerances or subtract known production effects accordingly.
═══════════════════════════════════════════════════════════

     ┌─── PARALLEL (all feed from Phase1Output + ModifierLog) ┐

A1: CONVENTION BANK ASSEMBLY
────────────────────────────
  Does:      Build the reference frame:
             · Lineage retrieval (genre ancestry, scene, era)
             · Convention bank: 10 universal + genre-specific
             · Fingerprint detection shapes loaded
             · Violation signatures prepared
  Reads:     ◆ GenreCommitment (Orientation)
             ◆ Genre-Fingerprint Map — genre → fingerprint IDs
             ◆ Fingerprint Registry — fingerprint definitions
             ◆ Genomic Frame — entrenchment curves, lifecycle
             ◆ RawWebData (P8) — scene context, influences
  Produces:  ConventionBank
             (conventions ranked by likelihood, with expected
              shapes and violation signatures)
  Note:      Can start at GenreCommitment (Step 3) — doesn't
             need modifier log or full extraction. Builds the ruler.

A2: EQUIPMENT CLASSIFICATION                  [MODIFIER-AWARE]
────────────────────────────
  Does:      Apply thresholds and reference matching:
             · Subtract/compensate known modifiers before comparison
             · CV thresholds → programmed vs organic per role
             · Harmonic profiles → synthesis family per role
             · Match against Equipment Registry (30-axis profiles)
             · Per-band aggregation → drum machine / live / hybrid
             · Envelope + discontinuity analysis → composite evidence
             · Gap measurement: distance from canonical per axis
  Reads:     ◆ EquipmentSignals (P2)
             ◆ ModifierLog (refined, R2) — subtract production before matching
             ◆ Equipment Registry — 30-axis canonical profiles + gap thresholds
             ◆ RawWebData (P8) — production credits (convergence check)
  Produces:  EquipmentReport
             (source_type, synthesis per role, drum classification,
              composite evidence, web convergence assessment,
              per-voice gap profiles, modifier-adjusted confidence)
  Updates:   Equipment Registry — if confirmed identification
             expands known variance envelope for that voice/genre

A3: BINARY MARKEDNESS                         [MODIFIER-AWARE]
─────────────────────
  Does:      Compare raw readings against genre baseline:
             · Compensate for known modifiers before flagging
             · Flag elements outside baseline as marked
             · Compute roster deviations (expected vs observed roles)
             · Compute legibility estimate
             · Co-production cluster confirmation
  Reads:     ◆ RawBinaryReadings (P5)
             ◆ ModifierLog (refined, R2) — adjust baseline comparison
             ◆ SpectralRoster (refined, R1)
             ◆ BandPresenceMap (P3) — roster match rates
             ◆ Prescriptive Genre Prints — baseline ranges
             ◆ Descriptive Genre Prints — empirical baselines
             ◆ Co-Production Cluster Templates — known patterns
             ◆ Dictionary Entries — novelty detection
  Produces:  StructuralDescriptor
             (RawBinaryReadings + markedness_flags[] +
              roster_deviations[] + legibility estimate +
              confirmed co_production clusters)

A4: PERCUSSION DEVIATION ANALYSIS             [MODIFIER-AWARE]
─────────────────────────────────
  Does:      Compare observed grid against expected grid:
             · Account for any timing/pitch modifiers
             · Deviation log: every absence and extra vs expectation
             · Ghost note discrimination (3-part intentionality test)
             · Meter relationship interpretation
             · Convention-informed grid expectations
  Reads:     ◆ PercussionGrid (P6)
             ◆ ModifierLog (refined, R2) — timing/pitch modifiers
             ◆ ConventionBank (A1) — what the grid "should" look like
             ◆ EquipmentReport (A2) — if programmed, CV is water
             ◆ Prescriptive Genre Prints — tempo/meter expectations
  Produces:  PercussionFindings
             (DeviationLog, MeterRelationship interpretations,
              ghost note confirmations, ElementMeter[] with
              deviation annotations)

A5: FELTNESS INTERPRETATION                   [MODIFIER-AWARE]
───────────────────────────
  Does:      Map gesture measurements to somatic predictions:
             · Compensate for reverb/compression modifiers on
               envelope shapes before interpreting
             · Gesture classification (punch / atmosphere / severance
               / compound gesture / anti-punch)
             · Frequency-to-body mapping per event
             · Polling model application (shelf life estimation)
             · Headroom analysis (how close to physical limits)
  Reads:     ◆ GestureMeasurements (P7)
             ◆ ModifierLog (refined, R2) — reverb/compression affect envelope reading
             ◆ Somatic Dictionary — frequency-to-body map, gate model
             ◆ PercussionGrid (P6) — event timing for polling model
  Produces:  GestureReport
             (per-event somatic predictions, gesture classifications,
              polling model estimates, headroom ratios)

A6: WEB CONTEXT ANALYSIS
────────────────────────
  Does:      Score and structure raw web data:
             · Thematic vector assembly
             · Production method classification
             · Co-production confirmation against templates
             · Web-only element population (#25, #49, #52)
  Reads:     ◆ RawWebData (P8)
             ◆ Meta-Dimensions — score thematic content against
             ◆ Co-Production Cluster Templates — what to confirm
             ◆ Element Registry — broken/web-only element specs
  Produces:  ContextDescriptor
             (genre, thematic_vector, production classification,
              co_production_confirmations, web_only_elements)

A-VOX: VOCAL SILHOUETTE INTERPRETATION           [MODIFIER-AWARE]
───────────────────────────────────────
  Does:      Interpret raw silhouette measurements:
             · Compensate for known vocal modifiers (reverb extends
               phrase tails, compression reduces vocal band DR,
               de-essing attenuates sibilance coupling)
             · Score composite vocal presence confidence
             · Classify vocal type (lead / backing / texture / chopped)
             · Map vocal phrase structure (breath cycle, section placement)
             · Characterise vocal production treatment
             · Assess convention alignment for genre
             · Populate FP-V05 through FP-V11 fingerprint values
  Reads:     ◆ VocalSilhouette (P-VOX)
             ◆ ModifierLog (refined, R2)
             ◆ ConventionBank (A1)
             ◆ SpectralRoster (refined, R1)
             ◆ RawWebData (P8) — credited vocalist name, if any
             ◆ Prescriptive Genre Prints — vocal expectations per genre
  Produces:  VocalReport
             (presence_confidence, vocal_type, phrase_structure,
              production_treatment, convention_alignment,
              modifier-adjusted confidence,
              FP-V05–FP-V11 values)
  Updates:   ConventionBank — vocal treatment patterns per genre
  Note:      Replaces the broken element #49 pathway. Vocal presence
             is now determined by horizontal silhouette evidence,
             not by a single-frame ML classifier. FP-V01–V04 become
             downstream of this module (they fire based on VocalReport
             presence_confidence, not on element #49).

     └─── END PARALLEL ────────────────────────────────────┘

              │
              ▼

A7: CULTURAL VIOLATION DETECTION
────────────────────────────────
  Does:      Compare convention predictions against actual measurements:
             · Each convention fingerprint vs corresponding binary reading
             · Flag violations (excision, inversion, mutation)
             · Map prior requirements per violation
             · Signal bridge types (Type 6: Excision, Type 7: Inversion)
             · Determine whether allele expressions are dominant,
               recessive, or novel mutations relative to genre
  Reads:     ◆ ConventionBank (A1) — expected shapes + violation signatures
             ◆ StructuralDescriptor (A3) — actual measurements
             ◆ PercussionFindings (A4) — timing deviation evidence
             ◆ EquipmentReport (A2) — production context
             ◆ ModifierLog — distinguish production choices from
               compositional choices (is this EQ a mix decision
               or a musical statement?)
  Produces:  Violations[]
             PriorRequirements[]
             bridge_type_signals

              │
              ▼

A8: ACTIVATION MODULE
─────────────────────
  Does:      Filter 1: Genre Markedness (water vs signal)
             Filter 2: Thematic Alignment (reinforce vs invert)
             Filter 3: Production Attribution (authored vs incidental)
             + Co-Production Clustering
             → axis_score = position × sign × (all weights)
  Reads:     ◆ StructuralDescriptor (A3)
             ◆ ContextDescriptor (A6)
             ◆ EquipmentReport (A2)
             ◆ Violations[] (A7)
             ◆ Element Registry — weight overrides
             ◆ Genre Baselines — markedness ranges
             ◆ Meta-Dimensions — dimension aggregation
             ◆ Co-Production Cluster Templates
  Produces:  ActivatedAxes
             (scored, signed, tiered: primary/supporting/suppressed)

              │
              ▼

A9: INTERPRETIVE ENGINE
───────────────────────
  Does:      Pass 3: Hypothesis generation
             Pass 4: Re-entry (see note below)
             Pass 5: Synthesis
  Reads:     ◆ EVERYTHING from Phase 2:
             ◆ ActivatedAxes (A8)
             ◆ StructuralDescriptor (A3)
             ◆ ContextDescriptor (A6)
             ◆ ConventionBank + Violations (A1, A7)
             ◆ EquipmentReport (A2)
             ◆ PercussionFindings (A4)
             ◆ GestureReport (A5)
             ◆ ModifierLog — production context awareness
             ◆ SomaticReport (listener, if present — Tier 1)
             ◆ Dictionary Entries — pattern matching
             ◆ Somatic Dictionary — correspondences, gate model
  Produces:  InterpretiveFindings
             InterpretivePresentation (the invitation)

  RE-ENTRY: If hypothesis needs more data, the Interpretive
  Engine can request re-entry into PHASE 1 (back to waveform):
    → Binary (higher resolution on target elements/sections)
    → Web (deeper queries)
    → Cultural (deeper lineage)
  Re-entry also triggers a refinement pass update if new
  extraction data changes the roster or modifier landscape.
  Limit: 1 re-entry per engine per pass.

              │
              ▼

A10: THE CONVERSATION
─────────────────────
  Does:      Convergence assessment, somatic data entry,
             bridge type confirmation
  Reads:     InterpretivePresentation
             Human somatic experience (Tier 1)
  Produces:  Confirmed bridge type + dictionary entry
  Updates:   ◆ Dictionary Entries — new entry
             ◆ Equipment Registry — confirmed identifications
             ◆ Descriptive Genre Prints — if n crosses threshold
             ◆ ModifierLog — resolve any remaining stubs
             This is where the system LEARNS. The bottom of the
             pipeline feeds back to the top.
```

---

## D. SUPPRESSION SYSTEM

Operates on **Phase 2**, not Phase 1. Phase 1 extracts everything it can see. Suppression decides what to **attend to** in Phase 2.

```
  Layer:       Phase 2 overlay
  Does:        Per-fingerprint, per-genre: which Phase1Output
               values to weight, suppress, or escalate in analysis
  Reads:       ◆ Fingerprint Registry (all 64)
               ◆ Genre-Fingerprint Map (genre expectations)
               ◆ Dictionary Entries (empirical distributions)
  Produces:    Suppression vectors (STEREO WAV pairs)
               Left channel = suppression curve (analysis weight reduction)
               Right channel = discovery/surprise (analysis weight amplification)
               110 gridline positions:
                 Binary:       64
                 Cultural:     15
                 Percussion:   10
                 Feltness:      7
                 Vocal:         7
                 Interpretive:  7

  Key change:  "Don't look" becomes "don't weight."
               Phase 1 still measured the suppressed region.
               If Phase 2 finds signal in a suppressed region,
               the surprise escalation is LOUDER because the
               signal is confirmed by real measurement. The
               immune response has evidence.

  Rendered:    Breakcore — all 5 engine pairs (complete)
  Defined:     Ambient, Rock — not rendered
```

---

## D2. DIAGNOSTIC RESYNTHESIS (D1)

Post-pipeline. Background process. Does not compete for the main analysis token budget. Runs after A10 has committed, or concurrently with A10 at lower priority. Its job is to render the pipeline's understanding as listenable audio: play the resynth to hear what the system understood, play the residual to hear what it didn't. Findings feed back upstream as registry and modifier log updates — same learning loop as A10, but from a different angle.

**Character:** Monitoring bus, not signal chain. Does not alter the analysis. Makes it audible. The resynthesis is a proof-of-concept readout — a live demonstration that both humans can use to verify whether the pipeline's measurements correspond to what they hear. When the resynth sounds wrong, something upstream was measured wrong. When the residual contains recognisable structure, something upstream wasn't measured at all.

```
D1: DIAGNOSTIC RESYNTHESIS                      [BACKGROUND]
──────────────────────────
  Position:  After A10 (or concurrent at lower priority)
  Budget:    Does not draw from main analysis token allocation.
             Runs in background. Results surfaced on request.

  Receives (pools the full pipeline):
    ◆ Phase1Output (cached — no waveform re-read):
      · STFT data (from P5 full binary extraction)
      · EquipmentSignals (P2) — harmonic profiles, repetition CVs
      · PercussionGrid (P6) — onsets, grid, element candidates
      · GestureMeasurements (P7) — per-event envelopes
      · VocalSilhouette (P-VOX) — formant tracks, phrase envelopes
      · SpectralRoster (refined, R1) — band assignments
      · ModifierLog (refined, R2) — known production effects
    ◆ Phase2 results:
      · EquipmentReport (A2) — classified sources per role
      · PercussionFindings (A4) — deviation log, ghost notes
      · GestureReport (A5) — somatic classifications
      · VocalReport (A-VOX) — vocal type, production treatment

  Does:

    1. TRAJECTORY LINKING
       Connect spectral peaks across consecutive frames
       into continuous frequency/amplitude curves.
       · Match frame N peak to nearest frame N+1 peak
         within tolerance (frequency drift < 20 Hz/frame,
         amplitude change < 12 dB/frame)
       · Birth: peak appears with no predecessor (onset)
       · Death: peak disappears with no successor (offset)
       · Each trajectory: list of (frame, frequency, amplitude)
       Uses P5 STFT data (already computed, cached).
       No waveform re-read.

    2. COHERENCE GROUPING
       Cluster trajectories that move together.
       · Frequency coherence: harmonics of same fundamental
         maintain integer ratios across frames
       · Amplitude coherence: correlated amplitude envelopes
         (rise and fall together)
       · Onset/offset coherence: appear and disappear together
       Each group = one inferred source.
       Cross-reference against A2 EquipmentReport:
         · Coherent group with 808 decay curve → 808 confirmation
         · Coherent group with FM harmonic ratios → FM synth
         · Incoherent cluster → noise/break/texture

    3. ENVELOPE CLASSIFICATION PER GROUP
       For each coherence group, classify the amplitude envelope:
       · Onset slope → impulsive (<5ms) / fast (5-20ms) /
         moderate (20-100ms) / slow (>100ms)
       · Sustain character → steady / decaying / modulated
       · Offset slope → abrupt / gradual / gated
       Cross-reference against P7 GestureMeasurements:
         if gesture already measured for this event,
         validate rather than re-derive.
       Cross-reference against Equipment Registry:
         match envelope shape against known voice ADSR profiles.

    4. MICRO-TIMING EXTRACTION
       For each trajectory onset, measure grid position:
       · Deviation from nearest P6 grid position (ms)
       · Classify: on-grid / swung / pushed / pulled / free
       Cross-reference against A4 PercussionFindings:
         if percussion module already flagged this onset,
         validate the timing characterisation.

    5. PHASE-COHERENT SYNTHESIS
       Render audio from trajectories (not per-frame peaks):
       · Each trajectory → continuous oscillator
       · Phase = integrated: φ(t) = φ₀ + ∫2πf(t)dt
         (no inter-frame phase discontinuity)
       · Amplitude = interpolated along trajectory envelope
       · Coherence groups rendered as units
       · Known modifiers (from ModifierLog) optionally
         re-applied or withheld (two render modes:
         "clean" = without production, "matched" = with)

    6. RESIDUAL COMPUTATION
       original_audio - resynth_audio = residual
       With phase-coherent synthesis, the residual isolates
       what the harmonic model could not explain:
       · Noise-floor content (tape hiss, digital noise)
       · Broadband transients (unresolved percussion attacks)
       · Stochastic components (breath, sibilance, room)
       · Non-harmonic partials (inharmonicity, bell-like)
       · Stereo difference (if original is stereo, resynth is
         mono — the residual contains the stereo field)

    7. DIAGNOSTIC METRICS
       · Spectral capture ratio per band
       · Onset tracking accuracy vs P6 grid
       · Trajectory count vs A2 source count
       · Residual energy per band (the system's ignorance map)
       · Coherence group count vs expected source count

  Produces:
    ResynthAudio        Listenable harmonic reconstruction
    ResidualAudio       What the model couldn't explain
    TrajectoryMap       Peak tracks, coherence groups,
                        onset classifications, micro-timing
    DiagnosticReport    Per-band capture, onset accuracy,
                        residual analysis, ignorance map

  Feeds back to (same learning loop as A10):
    ◆ Equipment Registry — trajectory decay curves
      matched against known instruments confirm or
      expand variance envelopes for voices/genres
    ◆ ModifierLog — compression detected as amplitude
      envelope flattening across coherence groups;
      saturation detected as harmonic ratio shifts
      within groups; reverb detected as trajectory
      tail extension beyond expected offset
    ◆ PercussionFindings — onset classifications
      cross-validate P6 element identification;
      micro-timing deviations cross-validate A4
    ◆ Discovered Patterns — residual structures that
      recur across songs = unmeasured features the
      pipeline should learn to extract
    ◆ Research Agenda — residual energy in bands
      where Phase 1 measured low content = blind spots

  Does NOT:
    · Alter any Phase 1 or Phase 2 results retroactively
    · Compete for main analysis token budget
    · Surface unless requested
    · Re-read the waveform (all STFT data cached in Phase1Output)
    · Replace any upstream module's measurements

  The resynthesis is the system explaining itself in the
  medium it analysed. Play it. If it sounds wrong, something
  upstream was wrong. If the residual has structure, something
  upstream was missed. The gap between resynth and original is
  the research agenda made audible.
```

### D1 Output Schema

```
DiagnosticResynthOutput {
  // Audio renders
  resynth_audio:      WAV           // phase-coherent trajectory synthesis
  resynth_clean:      WAV           // without production modifiers
  residual_audio:     WAV           // original - resynth

  // Trajectory data
  trajectory_map: {
    trajectories: [
      {
        id:                 int
        frames:             [{ frame_idx, freq_hz, amp, phase }]
        onset_frame:        int
        offset_frame:       int
        duration_ms:        float
        coherence_group:    int           // which source group
        onset_class:        string        // "impulsive" | "fast" | "moderate" | "slow"
        sustain_class:      string        // "steady" | "decaying" | "modulated"
        offset_class:       string        // "abrupt" | "gradual" | "gated"
        grid_deviation_ms:  float | null  // vs P6 grid position
        equipment_match:    string | null // if A2 identified the source
      }
    ]
    coherence_groups: [
      {
        group_id:           int
        trajectory_ids:     int[]
        fundamental_hz:     float         // inferred from ratio structure
        harmonic_coherence: float         // 0-1: how integer are the ratios
        source_hypothesis:  string | null // from A2 cross-reference
        envelope_shape:     string        // ADSR classification
      }
    ]
  }

  // Diagnostic metrics
  diagnostic: {
    spectral_capture:     { band: float }[]   // per-band capture ratio
    onset_accuracy:       float               // vs P6 grid
    trajectory_count:     int
    group_count:          int
    residual_energy:      { band: float }[]   // the ignorance map
    phase_coherence:      float               // mean phase continuity
  }

  // Feedback (proposed updates — committed after review)
  proposed_updates: {
    equipment_registry:   []    // trajectory-matched voice confirmations
    modifier_log:         []    // envelope-derived modifier evidence
    percussion_validation: []   // onset cross-validation results
    research_items:       []    // residual structures to investigate
  }

  song_id:              string
  processing_version:   string
  timestamp:            ISO datetime
}
```

---

## E. DEPENDENCY GRAPH

```
PHASE 1 — GENOTYPE + PHENOTYPE + ENVIRONMENT:

  Orientation (sequential):
    Step 1 ∥ Step 2 → Step 3 → Step 4 → Step 5
    (Snapshot and Web Genre can run in parallel)

  Phenotype + Genotype tracks start after Step 4 (roster).
  Step 5 runs in parallel with P1-P8 — it uses only
  orientation data (Steps 1-4), not full extraction output.

  Phenotype Track (mostly parallel):
    P1 (Roster check) — needs Step 4
    P2 (Equipment signals) — needs Step 4
    P3 (Structural sampling) — needs Step 4
    P4 (Shift points) — needs P3
    P5 (Full binary) — needs P4
    P6 (Percussion) — needs Step 4, concurrent with P5
    P7 (Feltness) — concurrent with P5+P6
    P-VOX (Vocal silhouette) — needs Step 4 only, parallel with P5-P7

  Genotype Track (independent):
    P8 (Web data) — can start at genre commit (Step 3)

  Parallelism: Step 5, P2, P3, P6, P8 all start after Step 4.
  P5 waits for P4. P7 reads P5+P6 concurrently. P4 waits for P3.

  Refinement Pass (in tandem, after all tracks complete):
    R1 (Roster refinement) — needs P2 + P3 + P4 + P6 + P-VOX + P8
    R2 (Environment refinement) — needs P2 + P3 + P5 + P6 + P7 + P-VOX + P8
    R1 ↔ R2 (tandem — interface once, then commit)


PHASE 2 — ANALYSIS + DICTIONARY:

  A1 (Convention bank) — can start at GenreCommitment (Step 3),
      doesn't need full Phase1Output
  A2 (Equipment class) — needs P2 + P8 + refined ModifierLog (R2)
  A3 (Binary markedness) — needs P3 + P5 + refined roster (R1) + ModifierLog (R2)
  A4 (Percussion deviations) — needs P6 + A1 + A2 + ModifierLog (R2)
  A5 (Feltness interpretation) — needs P7 + P6 + ModifierLog (R2)
  A6 (Web context) — needs P8
  A-VOX (Vocal interpretation) — needs P-VOX + A1 + P8 + refined roster (R1) + ModifierLog (R2)
  A7 (Violation detection) — needs A1 + A3 + A4 + A-VOX + A2 + ModifierLog
  A8 (Activation) — needs A2 + A3 + A6 + A7
  A9 (Interpretive) — needs A8 + A4 + A5 (everything)
  A10 (Conversation) — needs A9 + human → writes back to registries


DIAGNOSTIC LAYER (background, post-pipeline):

  D1 (Diagnostic resynthesis) — needs Phase1Output (cached) +
      Phase2 results (A2, A4, A5, A-VOX). Runs after A10 or
      concurrent at lower priority. Does not draw from main
      token budget. Feeds back: equipment registry updates,
      modifier log entries, percussion cross-validation,
      research agenda items.


REPEAT-LISTEN MODEL:

  Each subsequent pass through the same song:
  · Phase 1 output is cached — skip unless re-entry requested
  · Re-entry triggers refinement pass update (R1/R2 re-run)
  · Modifier log may be further refined by Phase 2 modules
  · Phase 2 runs faster on settled items, deeper on frontier
  · High-confidence items committed early, freeing attention
  · System's unresolved stubs = its research agenda
  · The hundredth listen is the most exploratory because
    everything coarse has been automated away
```

---

*Reference created: 10 February 2026*
*Revised: two-phase pipeline with refinement pass. Phase 1 = orientation (genre + spectral roster + production environment hypothesis) → extraction (genotype + phenotype in parallel) → refinement (R1 roster + R2 environment, in tandem, interfacing with equipment and percussion extraction). Phase 2 = analysis + dictionary (modifier-aware, receives refined roster + modifier log). 21 components total. Genetics frame: genotype = equipment/compositional choices, phenotype = audio evidence, allele = production environment (hypothesised at orientation, refined after extraction, tested in analysis). Top half light and exploratory, bottom half hard processing and dictionary learning.*
*Revised: 11 February 2026 — Vocal Silhouette Engine added (P-VOX + A-VOX). P-VOX extracts horizontal spectral analysis of the vocal band (parallel with P5-P7, needs only SpectralRoster). A-VOX interprets vocal presence, type, and production treatment (modifier-aware, parallel with A2-A6). 23 components total. Suppression gridline expanded to 110 positions (7 vocal positions added). FP-V05 through FP-V11 defined in fingerprint registry. VocalSilhouette added to Phase1Output. Validated against Phoneline (Pola & Bryson & Emily Makis) — voice detected by horizontal analysis where all vertical measures failed (hnr:-3.38, element #49 = 0%).*
*Revised: 11 February 2026 — Diagnostic Resynthesis (D1) added as post-pipeline background layer. Pools Phase1Output + Phase2 results, performs trajectory linking (cross-frame peak tracking), coherence grouping (source inference), envelope classification, micro-timing extraction, and phase-coherent additive synthesis. Produces listenable resynth + residual + trajectory map + diagnostic metrics. Feeds back to equipment registry, modifier log, percussion validation, and research agenda. Does not draw from main analysis token budget. Does not alter upstream results. Originated from comparative analysis of Phoneline MP3 vs harmonic_resynthesis.py output — zero sample-level correlation, 0.996 mean mel cosine similarity, +8-12 dB high-frequency deficit, 0.35 onset correlation. The engine captures harmonic skeleton but loses percussive articulation. D1 addresses this by tracking how harmonics are played (trajectories, envelopes, coherence, timing), not just what frequencies are present. 24 components total.*
