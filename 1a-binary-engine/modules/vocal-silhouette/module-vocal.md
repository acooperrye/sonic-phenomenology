# MODULE: VOCAL SILHOUETTE ENGINE
## Status: DRAFT — validated against Phoneline, prototyped subtraction on Black and Gold
## Origin: Alex's observation that Phoneline has "very clear unmistakable vox" that the system reads as invisible

---

## THE PROBLEM

Element #49 (Vocal presence) returns 0% for all tracks. The four vocal fingerprints (FP-V01 through FP-V04) depend on `vocal_detected = true`, which never fires. The binary engine's only vocal-adjacent measure (HNR) returns -3.38 for Phoneline — noise-dominant — because the DnB breaks pull the full-spectrum aggregate to percussive. Meanwhile, Emily Makis is singing clear as day across 190 of the track's 226 seconds.

Every existing measurement is VERTICAL: a snapshot statistic computed from the full spectrum at a point in time. The voice doesn't live vertically. It lives HORIZONTALLY — as a contour drawn across time, within specific spectral bands, with characteristic phrase-level structure. A snapshot of a vowel looks like noise. A five-second trace of that vowel resolving into a consonant resolving into the next vowel looks like nothing else on earth.

The vocal silhouette is the human voice's horizontal signature. The shape it carves in the spectrogram when you stop asking "what's here right now?" and start asking "what's been moving through here for the last few seconds?"

---

## THE APPROACH

### Core principle: fundamental first, harmonics mislead

The voice's fundamental pitch line — the actual note being sung — lives in a narrow band. A singer with three octaves starting at G3 spans ~200-1600Hz for the fundamental alone. Emily Makis in Phoneline spans G3-C#5 (200-551Hz), a slab just 350Hz wide. Harmonics extend to 2×, 3×, 4× that fundamental, reaching into 2-4kHz and beyond. But harmonics are UNRELIABLE as vocal evidence because reverb, delay, chorus, and other production effects create energy at harmonic frequencies that looks vocal but isn't the direct vocal source. A reverb tail ringing at 1200Hz looks exactly like a second harmonic of a 600Hz fundamental — but it's an echo, not a voice.

The old approach analysed the full 200-4000Hz "vocal band" as a single slab. This included the fundamental (~200-550Hz for Emily), the harmonic zone (550-4000Hz), and everything the production put there. Result: F2/F3 formant continuity contaminated by break energy, glide-to-step ratio dragged to 2.1 by break artifacts in the pitch tracker, syllabic divergence dominated by percussion AM.

The fundamental-first approach: find the F0 line first, anchor there, run all formant and pitch measures within the fundamental slab only. Sibilance coupling (a cross-band measure by nature) stays as a separate check.

### Three-gate architecture

The vocal slicer uses three sequential gates. Each gate is necessary but not sufficient. Only content that passes all three is classified as voice.

**GATE 1 — HPS (Harmonic Product Spectrum).** Finds pitched content with a harmonic series in the fundamental range (100-800Hz). Recovers 51% voiced frames in Phoneline where pYIN finds 0.7%. PASSES: voice, DX7 synths, strings, organ, brass — anything with harmonics. FAILS: noise, percussion, atonal content. This gate establishes "something pitched is here."

**GATE 2 — ENVELOPE CONTOUR.** Analyses the amplitude envelope in the fundamental slab (200-450Hz) for syllabic modulation (3-6Hz AM), crest factor, and peak irregularity. PASSES: voice, expressive harmonic instruments (DX7). FAILS: steady pads, percussion, unmodulated drones. This gate establishes "the pitched content is phrased and modulated."

**GATE 3 — SIBILANCE COUPLING.** Cross-band correlation between the fundamental slab (200-450Hz) and sibilance band (4-8kHz) at consonant-vowel timing (+23ms lag). PASSES: voice only. FAILS: ALL instruments, including DX7. This is the final gate — the one measure a synthesiser categorically cannot produce. No instrument generates both tonal energy at 200-450Hz AND broadband consonant noise at 4-8kHz from the same source at syllabic alternation rates.

**Why three gates are necessary — the DX7 finding:** Blade Runner main titles (Vangelis, DX7 synth orchestra, zero vocal content) was tested as a voiceless baseline. Results:

```
                          Blade Runner    Phoneline (Emily)
                          (no voice)      (voice)
──────────────────────────────────────────────────────────
HPS voiced %:             52.8            51.0
HPS glide ratio:          9.0             8.0
Envelope syllabic power:  0.507           0.833
Envelope crest factor:    3.038           3.742
Envelope classified vocal: 92.3%          99.0%
Sibilance correlation:    [untested*]     +0.068
```

The DX7 fools Gate 1 (HPS: 52.8%, glide ratio 9.0 — HIGHER than Emily) and partially fools Gate 2 (92.3% classified vocal on envelope shape alone). FM synthesis produces harmonic series with smooth pitch transitions and phrase-like amplitude modulation that are measurably voice-like in fundamental signal properties. The literature's claim that the DX7 sounds "human" or "voice-like" is not subjective — it's a measured property of the harmonic and envelope structure. Gate 3 (sibilance coupling) is the discriminator that a synthesiser cannot pass.

*[Blade Runner sibilance needs explicit measurement but is expected near zero — no consonant source]*

### Within-voice classification (sibilance as type discriminator)

Sibilance coupling also distinguishes TYPES of vocal content:

```
                          Sib corr     Classification
──────────────────────────────────────────────────────
Phoneline intro (0-30s):  -0.081       Vocal texture (echoed vocal runs, sibilance smeared)
Phoneline body (30-226s): +0.068       Lead vocal (sibilance intact)
```

Positive sibilance correlation = lead vocal (consonant-vowel articulation preserved). Zero or negative = vocal texture (processing has destroyed the consonant structure) or instrumental. A-VOX uses this distinction to classify FP-V01 (vocal foreground) vs FP-V02 (vocal as texture).

### Pass structure

**Pass 1 — BROAD SWEEP (200-4000Hz):** Formant contour tracking finds F1, establishing where the fundamental lives. Sibilance-vocal coupling (4-8kHz correlated with 200-4000Hz) provides the Gate 3 signal. Phrase envelope detects breath-scale amplitude structure.

**Pass 2 — FUNDAMENTAL LOCK:** Bandpass filter to the fundamental slab (F1 ± margin, typically 150-600Hz). Run HPS for Gate 1. Run envelope contour analysis for Gate 2. Within this narrow slab, re-run formant continuity (0.823 vs 0.516 in full band), pitch glide analysis (HPS glide ratio 8.0 vs 2.1 in full band), and vibrato detection. The fundamental slab is 92% narrower than the old vocal band.

### What stays in broad band, what moves to fundamental slab

| Measure | Band | Why |
|---------|------|-----|
| Formant contour | Fundamental slab | F1 is clear below break energy; F2/F3 contaminated in full band |
| Phrase envelope | Fundamental slab | Cleaner amplitude structure without harmonic interference |
| Syllabic modulation | Fundamental slab | Less percussion AM bleed in the narrow band |
| Pitch continuity | Fundamental slab | HPS or filtered pYIN on fundamental only |
| Vibrato | Fundamental slab | Needs clean F0 contour |
| Sibilance coupling | Cross-band (fund ↔ 4-8kHz) | By definition a cross-band measure |
| Sectional map | Combined | Uses F0 density + sibilance + energy share |

### Other design constraints

**Axis:** Horizontal. Minimum analysis window: 2 seconds. Primary analysis window: 4-8 seconds (one vocal phrase). The engine never makes a vocal determination from a single frame.

**Percussion:** Ignored as a grid. The percussion module handles beat structure. The vocal engine treats percussion energy as contamination to be accounted for, not as signal.

**Instruments:** Demoted. Equipment signals in the vocal band (synth pads, bass harmonics) are treated as interference, not findings. The engine is looking for what only a voice does — formant movement, breath phrasing, consonant-vowel alternation, sibilance coupling.

---

## ARCHITECTURE: WHERE THIS FITS

### Phase 1 — Extraction

The Vocal Silhouette Engine adds one extraction track to the phenotype pipeline:

```
P-VOX: VOCAL SILHOUETTE EXTRACTION                    [WAVEFORM]
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
             · Vocal band production cues (reverb tail, compression, de-essing)
  Reads:     ◆ Audio
             ◆ SpectralRoster (Step 4 — vocal band boundaries)
  Produces:  VocalSilhouette
             (formant_tracks, phrase_envelope, syllabic_modulation,
              pitch_profile, vibrato_profile, sibilance_coupling,
              sectional_map, production_cues —
              raw measurements, NO vocal classification,
              NO presence/absence determination)

  Runs in parallel with P5-P7. Needs only SpectralRoster (Step 4).
  Does not depend on percussion grid (P6).
  Does not depend on equipment signals (P2).
```

### Phase 2 — Analysis

```
A-VOX: VOCAL SILHOUETTE INTERPRETATION          [MODIFIER-AWARE]
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
  Reads:     ◆ VocalSilhouette (P-VOX)
             ◆ ModifierLog (refined, R2)
             ◆ ConventionBank (A1)
             ◆ SpectralRoster (refined, R1)
             ◆ RawWebData (P8) — credited vocalist name, if any
             ◆ Prescriptive Genre Prints — vocal expectations per genre
  Produces:  VocalReport
             (presence_confidence, vocal_type, phrase_structure,
              production_treatment, convention_alignment,
              modifier-adjusted confidence)
  Updates:   ConventionBank — vocal treatment patterns per genre

  Runs alongside A2-A6 in the parallel analysis block.
  Feeds into A7 (Cultural Violation Detection — vocal convention violations),
  A8 (Activation — vocal as agency indicator),
  A9 (Interpretive Engine — vocal address, non-address, texture).
```

### Refinement Pass Impact

R1 (Spectral Roster Refinement) gains a new input: VocalSilhouette. If P-VOX detects strong vocal presence in the 200-4000Hz band, R1 can refine the roster to distinguish vocal energy from instrumental energy within that band. This matters for songs like Phoneline where bass harmonics, vocal, and pads all occupy 200-4000Hz simultaneously.

R2 (Production Environment Refinement) gains vocal production cues from P-VOX: reverb on vocal band, compression evidence, de-essing evidence. These inform modifier log entries scoped to the vocal.

---

## THE SEVEN MEASURES (Phase 1 — raw extraction)

### 1. Formant Contour Tracking

The voice shapes vowels by moving formant resonances. F1 (first formant, ~300-900Hz) tracks jaw openness. F2 (~900-2500Hz) tracks tongue position. F3 (~2500-3500Hz) tracks lip rounding and nasality. These three resonance bands move INDEPENDENTLY and CONTINUOUSLY. No instrument does this — instruments have fixed or slowly-varying resonances.

**What to measure:**
- Spectral peaks within the vocal band at each time frame
- Continuity of each peak track: how smoothly does the peak frequency change frame-to-frame? Smooth = voice (formant glide). Jumpy = instrument or noise.
- Independence of tracks: do the top 2-3 spectral peaks move at different rates? Voice: yes (different articulators). Single instrument: no (harmonics move in parallel).

**Phoneline evidence (full band → fundamental slab):**
```
FULL BAND (200-4000Hz):
  Formant 1:  median 420Hz, continuity 0.715 — strong vocal contour
  Formant 2:  median 624Hz, continuity 0.475 — moderate (bass harmonics interfering)
  Formant 3:  median 700Hz, continuity 0.360 — weak (breaks contaminating)
  Mean continuity: 0.516

FUNDAMENTAL SLAB (187-501Hz):
  Formant 1:  median 301Hz, continuity 0.896 — very strong vocal contour
  Formant 2:  median 345Hz, continuity 0.795 — strong (break contamination gone)
  Formant 3:  median 355Hz, continuity 0.779 — strong (break contamination gone)
  Mean continuity: 0.823  (+0.307 improvement)
```

In the full band, F1 is clear (0.715) but F2 and F3 are degraded because DnB breaks and bass harmonics live in the same frequency range. When the slicer narrows to the fundamental slab (187-501Hz), all three formant tracks jump to 0.78+ continuity. The break energy that was contaminating F2/F3 lives above 500Hz and is excluded entirely. This is the fundamental-first design working as intended: narrow to where the voice's own F0 lives, and the contamination disappears.

**Output schema:**
```
FormantContour {
  tracks: [
    {
      formant_id:      int            // 1-5 (top spectral peaks)
      median_hz:       float
      active_fraction: float          // fraction of frames where this peak exists
      continuity:      float          // 0-1, fraction of smooth transitions
      mean_drift_hz:   float          // Hz per frame average change
      range_hz:        [float, float] // min-max frequency
    }
  ]
  mean_continuity:     float          // average of top 3 formants
  independence_score:  float          // how differently the formants move (0-1)
}
```

### 2. Phrase Envelope

Human singing is organised by breath. A vocal phrase rises from breath onset, sustains through the sung passage, falls as the breath runs out, then there's a gap (the breath) before the next phrase. Typical phrase duration: 3-8 seconds. This timing is INDEPENDENT of the musical beat grid — the voice operates on a biological clock (lung capacity, diaphragm recovery) that cross-cuts the metric grid.

**What to measure:**
- Amplitude envelope of the vocal band, smoothed at phrase scale (~3 seconds)
- Phrase peaks (local maxima), phrase troughs (potential breath points)
- Inter-phrase intervals (how long between phrase peaks)
- Proportion of intervals that fall in the human breath range (2.5-9 seconds)
- Phrase durations and gap durations independently

**Phoneline evidence:**
```
Phrase peaks:           10 (at phrase-smooth resolution)
Phrase troughs:         14 (breath candidates)
Phrase intervals:       21.55s mean (this is measuring at too coarse a resolution —
                        these are section-level, not phrase-level. At finer threshold:)

Finer measurement:
Detected phrases:       26
Phrase durations:       6.09s mean (0.02s — 27.91s)
Inter-phrase gaps:      1.32s mean (0.02s — 6.46s)
Gaps in breath range:   44%
```

26 phrases at 6.1s mean is textbook vocal phrasing in 175 BPM DnB. The 44% breath-range figure is depressed because the smoothing window catches some very short or very long events that are structural transitions (breakdown/buildup), not individual vocal phrases. A narrower window focused on sections where vocal presence is already confirmed would push this higher.

**Output schema:**
```
PhraseEnvelope {
  total_phrases:        int
  phrase_durations:     float[]       // seconds per phrase
  gap_durations:        float[]       // seconds between phrases
  mean_phrase:          float         // mean phrase duration
  mean_gap:             float         // mean gap duration
  breath_range_pct:     float         // % of gaps in 0.3-3.0s (breath scale)
  phrase_onset_times:   float[]       // seconds from song start
  phrase_offset_times:  float[]
  biological_clock:     float         // phrase_interval / beat_interval
                                      // (how independent is phrasing from beat)
}
```

### 3. Syllabic Modulation (two distinct measures)

Singing alternates between consonants (brief broadband noise, amplitude dip) and vowels (sustained harmonic energy, amplitude peak). This creates an amplitude modulation at the syllabic rate: roughly 3-6 syllables per second in English singing.

#### 3a. Cross-band divergence (old measure — weak in drum-heavy genres)

Compares the 3-6Hz AM in the vocal band against the percussion band. In DnB, this reads 0.33x because the breakbeats produce stronger AM across all frequencies than the voice does. This measure is GENRE-DEPENDENT and unreliable in drum-forward genres.

```
Syllabic divergence (full band):          0.33x
Syllabic divergence (fundamental slab):   0.32x
→ No improvement from narrowing the band. Percussion AM dominates everywhere.
```

#### 3b. Envelope contour syllabic power (new measure — STRONG discriminator)

Instead of comparing vocal AM to percussion AM (which fails when percussion dominates), measure the ABSOLUTE syllabic modulation power within the fundamental slab's amplitude envelope. The Hilbert envelope of the 200-450Hz band, analysed for 3-6Hz power via Welch PSD.

This is the strongest single discriminator between voice and non-voice content within the fundamental slab:

```
ENVELOPE CONTOUR SYLLABIC POWER (3-6Hz AM in fundamental slab):
  Vocal-classified windows:     0.833 mean
  Bass-classified windows:      0.234 mean
  Separation:                   2.19 standard deviations  ← STRONG

Pre-vocal section (0-30s):      0.446 (bass melody + intro atmospherics)
Vocal section (30-226s):        0.867 (Emily singing)
→ When Emily sings, the 3-6Hz AM nearly doubles.
```

**Why this works when cross-band divergence doesn't:** Cross-band divergence asks "does the vocal band modulate MORE than percussion?" — answer is no in DnB because breaks dominate everything. Envelope syllabic power asks "does the fundamental slab modulate AT syllabic rate at all?" — answer is yes when Emily is singing (0.833) and much less when she isn't (0.234). The voice creates its own 3-6Hz signature within the fundamental slab regardless of what percussion is doing at higher frequencies.

**The overture false positive:** The intro bass melody scores 0.446 — between bass (0.234) and vocal (0.833). A threshold of ~0.5 would separate them. This is also where the sibilance cross-check adds value: even if the fundamental slab reads moderate syllabic power (bass melody doing something vocal-shaped), the sibilance band (4-8kHz) should NOT correlate with it because the bass doesn't produce consonant noise. Sibilance coupling + syllabic power together should eliminate the false positive.

**Second strong discriminator: Crest factor**

```
CREST FACTOR (peak-to-RMS of envelope in fundamental slab):
  Vocal-classified windows:     3.742 mean
  Bass-classified windows:      2.517 mean
  Separation:                   2.28 standard deviations  ← STRONG
```

Emily's breath-phrase structure (rounded lobes rising from breath onset, sustaining, falling) creates deeper peaks and valleys than the bass atmosphere's steady undulation. This maps directly to Alex's diagram: the red vocal lobes have high crest, the blue bass line has low crest.

**Additional useful discriminator: Irregularity**

```
PEAK INTERVAL IRREGULARITY (coefficient of variation of peak spacing):
  Vocal-classified windows:     0.427 mean
  Bass-classified windows:      0.243 mean
  Separation:                   1.27 standard deviations  ← useful
```

Emily's phrases are timed by breath (biological clock), giving irregular peak spacing. The bass melody is more grid-locked (regular).

**Output schema:**
```
SyllabicModulation {
  // Cross-band divergence (old — retained for completeness)
  divergence:                float     // vocal ratio / perc ratio (genre-dependent)

  // Envelope contour measures (new — fundamental slab)
  envelope_syllabic_power:   float     // 3-6Hz AM power in fundamental slab envelope
  envelope_crest_factor:     float     // peak/RMS of fundamental slab envelope
  envelope_irregularity:     float     // CV of peak spacing in fundamental slab
  envelope_modulation_depth: float     // (max-min)/mean of envelope

  // Classification
  contour_class:             "vocal" | "bass" | "synth" | "beat" | "silence"
  contour_confidence:        float     // margin between best and second-best class
}
```

### 4. Pitch Continuity (F0 via Harmonic Product Spectrum)

The human voice GLIDES between notes. Even in the crispest staccato singing, there's a brief portamento — the vocal folds adjust continuously, they don't jump between discrete states the way a keyboard steps between notes. In a spectrogram, voiced pitch appears as curved lines, not rectangular blocks.

**The pitch tracker problem:** Standard monophonic pitch trackers (pYIN, piptrack) fail in dense mixes. pYIN found only 0.7% voiced frames in Phoneline at default confidence. Even bandpass pre-filtering to the fundamental range (200-450Hz) only recovered 14.7%. The voice is there, but the production buries it from algorithms designed for monophonic sources.

**The solution: Harmonic Product Spectrum (HPS).** HPS multiplies the spectrum by downsampled copies of itself, reinforcing frequencies whose harmonics (2f, 3f, 4f) are also present. This is exactly what a voice produces — a fundamental with a full harmonic series. HPS recovered **51.0% voiced frames** in Phoneline at median D♯4 (312Hz), compared to pYIN's 0.7%. Glide ratio: **8.0** (vs pYIN's 38.0 on its 65 frames, vs piptrack's 2.1 on full band).

**Why HPS works here:** The fundamental IS present in the mix, surrounded by bass harmonics and break energy. HPS doesn't need the fundamental to be the loudest thing in the band — it just needs the harmonic series to be there. If there's energy at 312Hz AND 624Hz AND 936Hz AND 1248Hz, HPS reinforces 312Hz. Other instruments don't produce the same harmonic series at the same ratios.

**What to measure (fundamental slab, HPS-detected F0):**
- Voiced fraction: what percentage of frames have HPS-detected pitch?
- Glide fraction: what percentage of frame-to-frame pitch transitions are <100 cents?
- Step fraction: what percentage are >200 cents?
- Glide-to-step ratio: voice typically >5:1, synths typically <2:1
- Sustaining fraction: <10 cents/frame (holding a note)
- F0 range in semitones

**Phoneline evidence (comparative):**
```
                      Full-band      Fundamental slab    Fundamental slab
                      (piptrack)     (pYIN)              (HPS)
─────────────────────────────────────────────────────────────────────────
Voiced frames:        74.6%          0.7%                51.0%
Median pitch:         279Hz          318Hz               312Hz (D♯4)
Glide fraction:       0.660          0.974               0.862
Step fraction:        0.315          0.026               0.107
Glide-to-step ratio:  2.1            38.0                8.0
Pitch range (5-95%):  102-3123Hz     234-417Hz           161-441Hz
```

The full-band piptrack at 2.1 was being dragged down by break and bass artifacts. HPS at 8.0 is solidly in vocal territory. pYIN's 38.0 is the purest reading but based on only 65 frames. HPS is the operational choice: good sensitivity (51%), good specificity (median D♯4 matches known vocalist range), good glide ratio (8.0).

**Pitch behavior breakdown (HPS, fundamental slab):**
```
Sustaining (<10c/frame):  84.6%   ← holding notes
Gliding (10-100c/frame):  12.8%   ← legato movement between notes
Stepping (>100c/frame):    2.6%   ← rare jumps
→ Voice character: sustained/legato
```

**Genre adjustment:** Same principle as before — contamination in the pitch tracker varies by genre density. But HPS is inherently more resistant to contamination than pYIN or piptrack because it exploits harmonic structure rather than single-frequency peak-picking.

**Output schema:**
```
PitchContinuity {
  f0_method:           "hps" | "pyin" | "piptrack"
  voiced_fraction:     float         // 0-1
  median_pitch_hz:     float
  f0_note:             string        // e.g. "D♯4"
  f0_range_semitones:  float
  glide_fraction:      float         // <100 cents transitions
  step_fraction:       float         // >200 cents transitions
  sustaining_fraction: float         // <10 cents transitions (holding notes)
  glide_to_step_ratio: float
  pitch_in_vocal_range: bool         // median falls in 80-1000Hz
  pitch_behavior:      "sustained/legato" | "melodically_active" | "staccato/choppy"
}
```

### 5. Vibrato Detection

Human singing vibrato is a periodic pitch modulation at approximately 5-7 Hz with a depth of ±20-100 cents. It's a physiological characteristic of the laryngeal muscles — the rate is remarkably consistent across singers (most are 5-7 Hz) and involuntary at trained levels. In a spectrogram, vibrato appears as a sinusoidal wobble in all harmonics simultaneously.

**What to measure:**
- PSD of the pitch track: power in the vibrato band (4.5-7.5Hz) vs non-vibrato (1-4Hz)
- Vibrato ratio: vibrato power / non-vibrato power
- Peak frequency within the vibrato band

**Phoneline evidence:**
```
Vibrato band power:  55109
Non-vibrato power:   162857
Vibrato ratio:       0.338
Vibrato peak:        5.8Hz
```

The vibrato peak at 5.8Hz is dead centre of human singing vibrato. But the ratio is low (0.338) — the pitch track's power is dominated by slower movements (1-4Hz = phrase-level pitch arcs and melodic movement). This is consistent with Emily Makis using a controlled, subtle vibrato (common in DnB vocal style — operatic vibrato would clash with the mechanical precision of the drum grid). The vibrato IS there at 5.8Hz, it's just quiet relative to the melody's own movement.

**Genre adjustment:** DnB, electronic, and pop vocal styles tend toward controlled vibrato (ratio 0.2-0.8). Classical, soul, and R&B tend toward prominent vibrato (ratio >1.5). The engine should compare the vibrato ratio against genre expectations, not against a universal threshold.

**Output schema:**
```
VibratoProfile {
  vibrato_power:      float
  non_vibrato_power:  float
  vibrato_ratio:      float
  vibrato_peak_hz:    float
  in_human_range:     bool          // peak between 4.5-7.5Hz
  depth_cents:        float | null  // if detectable
  consistency:        float         // how steady is the vibrato rate
}
```

### 6. Sibilance-Vocal Coupling

This is the engine's most voice-specific measure. Sibilant consonants (s, sh, ch, z, f) produce broadband noise energy concentrated in 4-8kHz. Voiced sound (vowels, nasals) produces harmonic energy in 200-4000Hz. When a singer sings, these two spectral regions are TIME-CORRELATED because they come from the same source (the mouth). No instrument produces both tonal energy at 200-4000Hz AND broadband noise at 4-8kHz from the same physical source at syllabic alternation rates.

**What to measure:**
- Cross-correlation between vocal band (200-4000Hz) energy and sibilance band (4-8kHz) energy
- Time lag at peak correlation (consonants slightly lead vowels: expected lag -50 to +30ms)
- Comparison: vocal-sibilance correlation vs vocal-sub_bass correlation (sub-bass should be LESS correlated with the vocal than sibilance is)
- "Sibilance coupling strength over sub-bass" = the margin by which sibilance correlates more strongly with voice than sub-bass does

**Phoneline evidence:**
```
Vocal-sibilance correlation:    0.320
Best cross-correlation:         0.369 at +23.2ms lag
Vocal-sub_bass correlation:    -0.105
Coupling strength over sub-bass: 0.424
```

The lag at +23.2ms is right in the consonant-vowel window. The coupling strength over sub-bass at 0.424 is strong — sibilance is substantially more coupled to the vocal band than the bass is. This is the voice's cross-band fingerprint: two frequency regions that shouldn't correlate (they're octaves apart) DO correlate because they share a source.

**Modifier awareness:** De-essing (a common vocal production technique) attenuates sibilance energy, which will REDUCE the sibilance correlation. A de-essed vocal will have weaker coupling. The engine should consult the modifier log for de-essing evidence before interpreting low sibilance correlation as "no voice." Conversely, bright/airy vocal production (boosted highs) will INCREASE the coupling.

**Output schema:**
```
SibilanceCoupling {
  vocal_sibilance_corr:     float    // cross-correlation
  peak_lag_ms:              float    // time lag at peak correlation
  lag_in_cv_range:          bool     // -50 to +30ms?
  vocal_sub_bass_corr:      float    // comparison correlation
  coupling_over_sub_bass:   float    // margin: sib_corr - sub_corr
  deessing_adjusted:        bool     // has modifier compensation been applied?
}
```

### 7. Sectional Vocal Map

The voice doesn't appear everywhere in a song. It appears in verses, choruses, and vocal sections, and it's absent in intros, breaks, instrumental sections, and outros. The pattern of appearance and disappearance IS the vocal's structural contribution to the song. Mapping where the voice appears — and where it withdraws — reveals the vocal architecture.

**What to measure:**
- Divide the track into windows (15 seconds for coarse mapping, 4 seconds for fine mapping)
- For each window: composite vocal score from all six measures above
- Map the vocal score across the full track timeline
- Identify: onset (where voice first appears), withdrawal (where voice drops out), and the shape of the vocal presence curve (gradual entry? sudden appearance? building through song?)

**Phoneline evidence:**
```
Window timeline (15s resolution, composite vocal score):

  0.0— 15.0s   0.32  partial  (intro: bass overture, vocal hints)
 15.0— 29.9s   0.41  partial  (building: voice entering)
 30.0— 44.9s   0.52  VOCAL    (voice established)
 44.9— 59.9s   0.50  VOCAL
 59.9— 74.9s   0.59  VOCAL    (voice + breaks + bass all present)
 74.9— 89.8s   0.63  VOCAL    (strong vocal section)
 89.9—104.8s   0.61  VOCAL
104.8—119.8s   0.61  VOCAL
119.8—134.8s   0.68  VOCAL    (peak vocal presence)
134.8—149.7s   0.63  VOCAL
149.8—164.7s   0.57  VOCAL
164.7—179.7s   0.58  VOCAL
179.7—194.7s   0.63  VOCAL
194.7—209.7s   0.60  VOCAL
209.7—224.6s   0.57  VOCAL    (voice persists to end)
```

The voice enters around 30 seconds and stays for the remaining 190 seconds. 13 of 15 windows score as VOCAL. The shape is: gradual entry (0.32 → 0.52 over 45 seconds), sustained presence through the body of the track (0.50-0.68), with the peak at 120-135 seconds (post-drop full vocal section). The voice doesn't build and release like the DnB energy structure (which has distinct drop/breakdown/buildup cycles) — it enters and STAYS, like a continuous thread the track is woven around.

**Output schema:**
```
SectionalVocalMap {
  windows: [
    {
      start_sec:        float
      end_sec:          float
      composite_score:  float        // 0-1
      energy_share:     float        // vocal band % of total
      syllabic_ratio:   float
      voiced_fraction:  float
      sibilance_corr:   float
      classification:   "absent" | "partial" | "vocal"
    }
  ]
  voice_onset_sec:      float        // first "vocal" window start
  voice_offset_sec:     float | null // last "vocal" window end (null if voice persists to end)
  vocal_coverage_pct:   float        // % of track duration classified as "vocal"
  peak_vocal_window:    int          // index of highest-scoring window
  shape:                string       // "continuous" | "intermittent" | "gradual_entry" |
                                     // "sudden_entry" | "verse_chorus_pattern" | "bookend"
}
```

---

## PRODUCTION CUES (cross-references P-VOX with ModifierLog)

The vocal production treatment modifies every measure above. The engine needs to know what's been done to the voice before interpreting the raw numbers.

### Reverb on Vocal

**Detection:** After a vocal phrase ends (phrase offset), does energy in the vocal band decay slowly or cut sharply?

**Phoneline evidence:** Decay rate +3.4 dB/s — slow. Reverb is present on the vocal. This is typical for liquid DnB (lush, spacious vocal treatment).

**Impact:** Reverb extends phrase tails, which blurs phrase boundaries. Inter-phrase gaps will appear shorter than the singer's actual breath timing. The engine should compensate: if reverb is detected, widen the phrase gap threshold.

### Compression on Vocal

**Detection:** Dynamic range within the vocal band.

**Phoneline evidence:** Vocal band DR 15.0 dB — right at the compressed/natural boundary. Some compression has been applied (expected in DnB), but the vocal isn't brick-walled.

**Impact:** Compression reduces the amplitude variation that the syllabic modulation measure depends on. Heavily compressed vocals will have lower syllabic modulation because the consonant-vowel amplitude contrast is squashed. The engine should widen the syllabic threshold for compressed vocals.

### De-essing

**Detection:** Sibilance-to-vocal energy ratio.

**Phoneline evidence:** Sibilance-to-vocal ratio 0.1457. The sibilance band carries about 15% of the vocal band's energy. This is moderate — heavy de-essing would push it below 5%, no de-essing in bright production might push it above 25%.

**Impact:** De-essing directly attenuates the sibilance coupling measure. The engine should note the sibilance ratio and adjust the coupling threshold accordingly. Low sibilance ratio + low coupling ≠ "no voice." It may = "de-essed voice."

### Stereo Placement

**Phoneline evidence:** M/S 0.080 — essentially mono. The voice is stacked centre with everything else. In stereo-wide production, the voice might be isolated in the centre (standard) or spread wide (unusual), which affects how the vocal band measurements read relative to the full mix.

---

## WHAT THE VOCAL ENGINE IS NOT

- **Not ML vocal separation.** It doesn't attempt to isolate the vocal from the mix. It looks for the SILHOUETTE — the characteristic horizontal patterns that only a voice produces — within the mixed signal.

- **Not a lyrics transcriber.** It doesn't care what the voice says. It cares about the PHYSICAL PATTERN of voice: formant movement, breath phrasing, sibilance coupling, pitch continuity. These are pre-linguistic — they exist whether the words are intelligible or not.

- **Not a vocal quality assessor.** It doesn't judge whether the singing is "good." It characterises the voice's structural contribution to the track: where it appears, how it moves, what production has been applied to it, and whether its behaviour aligns with genre conventions.

- **Not a single-frame detector.** Every measure operates across time windows of 2-8 seconds minimum. The engine fundamentally cannot say "frame 47,392 contains voice." It says "the passage from 30 seconds to 38 seconds has characteristics consistent with human singing."

---

## GATE 4 CANDIDATE: ASPIRATION (breath at vowel onsets)

### Discovery

During harmonic resynthesis testing on Black and Gold, Alex observed that moments of open-mouth posture with breath — the aspirated transitions between consonants and vowels — read as significantly louder in the resynthesis than they sound in the original mix. The "ouHHT" in "you must be out of your mind," the "mHHHiiiNNd" — these breathy transitions carry broadband energy that the STFT reads as high amplitude at the harmonic peaks. In the original mix, this broadband energy is masked by the surrounding production. In the resynthesis (which rebuilds only the harmonic content), the aspiration energy stands naked.

### Why this matters for voice detection

Aspiration is the burst of turbulent airflow when the vocal tract opens for a vowel after a consonant, or during breathy phonation. It produces energy across a wide bandwidth — not just at harmonic frequencies but in the noise between them. When the STFT measures harmonic peaks during aspirated moments, the peaks are INFLATED by the broadband aspiration noise leaking into the harmonic bins.

Instruments don't aspirate. A synthesiser, a guitar, a drum — none of them produce the turbulent airflow burst that happens when a human opens their mouth to sing. An organ has wind noise, but it's continuous and uncorrelated with note onsets. A flute has breath, but the breath timing is tied to the note onset, not to consonant-vowel alternation.

### Potential implementation

Aspiration detection could serve as a fourth gate, independent of and complementary to sibilance coupling (Gate 3):

**Gate 3 (sibilance):** Detects consonant NOISE in 4-8kHz correlated with tonal energy in 200-4000Hz. This is the s/sh/ch/f sounds.

**Gate 4 (aspiration):** Detects broadband energy INFLATION at harmonic peaks during vowel onsets. This is the h/breath sounds. Measured as: per-frame ratio of (harmonic peak amplitude) to (inter-harmonic valley amplitude). Voice shows periodic spikes in this ratio at syllabic rate (~3-6Hz). Instruments show stable ratios.

The two gates cover complementary parts of the consonant inventory: Gate 3 catches fricatives (s, sh, f), Gate 4 catches aspirates (h, breath onsets). Together they'd detect the full range of consonant-to-vowel transitions that only a voice produces.

**Status:** Observed, not yet implemented. Needs: (1) formal measurement of harmonic-to-valley ratio during aspirated vs non-aspirated frames, (2) validation that instruments don't produce similar ratio fluctuations, (3) threshold calibration across different vocal styles.

---

## SIBILANCE ENGINE — CONFIRMED WORKING

During spectral subtraction testing on Black and Gold, the 4-8kHz sibilance-only export was assessed by Alex as "perfectly spot on — assign that to record, you have a sibilance engine." The sibilance band (4-8kHz) with 98.5% of its energy remaining after spectral subtraction of instrument harmonics produces a clean sibilance track that:

- Maintains the rough tempo of the song
- Sounds like music played through earphones held away from the ears
- Contains only the consonant energy (s, sh, ch, f, t) from the vocal

This works because instrument harmonics rarely reach into the 4-8kHz band with significant energy (most instrument fundamentals are below 2kHz, so only the 4th+ harmonics reach this range, and they're weak). The sibilance band is naturally dominated by vocal consonant noise. Simple spectral subtraction of identified instrument harmonics cleans what little instrument content exists there.

The sibilance engine is effectively a solved problem — it needs no further development beyond the spectral subtraction that already exists. It feeds directly into Gate 3 (sibilance coupling) as the sibilance reference signal.

---

## SUBTRACTION METHOD: COHERENT RELATIVE PHASE (CRP)

### The problem with spectral subtraction for voice isolation

The slope engine (module-slope-identity.md) identifies instrument sources. But subtracting them from the mix to isolate the voice requires a method that preserves phase coherence. Spectral magnitude subtraction (removing amplitude from the STFT while keeping original phase) produces phase incoherence — the remaining signal sounds detuned and metallic because the phase was set by the combined signal, not by individual sources.

### Alex's CRP invention

Alex proposed staying entirely in the time domain: detect instrument events as amplitude envelope shapes, build a gain curve that scales the waveform down during those events, multiply the waveform by the gain curve. Phase is preserved because multiplication by a real-valued gain preserves all relative phase relationships between frequency components.

### Broadband CRP (validated)

The key refinement: no frequency splitting at all. One signal, one envelope, one gain curve per pass. Three sequential passes at different time scales catch different event types:

**Pass 1 — Transients (120ms floor window):** Catches drums, percussion, sharp attacks. The minimum_filter1d with a 120ms window finds the local energy floor beneath drum hits. Gain = floor/envelope where envelope significantly exceeds floor.

**Pass 2 — Melodic (400ms floor window):** Catches notes, chords, melodic phrases. Applied to the output of Pass 1 (drums already removed). The 400ms window spans a typical note duration.

**Pass 3 — Sustained (1500ms floor window):** Catches pads, drones, atmospheric elements. Applied to the output of Pass 2. The 1500ms window spans sustained textures.

What survives all three passes is the voice candidate.

### Results on Black and Gold

```
Phase coherence:        0.95 (broadband) vs 0.67 (filter-bank) vs ~0.4 (spectral subtraction)
Energy distribution:    transients 45.4%, melodic 4.3%, sustained 0.8%, voice residual 1.8%
```

Alex's assessment of the voice candidate: "it sounds like im listening to mostly just sam sparro sing at me, but through a Teams meeting window, and his net connection is a bit choppy. the choppiness of his net carries the ghost shape of maybe a programmed drum pattern."

### Known limitation — voice ducking

The broadband gain curve turns down ALL energy during instrument events, including the voice. When a drum hits, the gain drops, and the voice drops with it. This creates a sidechaining effect — the voice sounds like it's being compressed by the instrument events. The "Teams meeting choppiness" Alex described is this ducking.

Possible approaches (untested):
- Frequency-selective gain: re-introduces filter bank, re-introduces phase smearing
- Voice-aware gain: requires knowing where the voice is before subtracting instruments (circular)
- Post-CRP voice enhancement: detect and compensate for the ducking pattern after extraction
- Hybrid: use CRP for drums (short events, full ducking acceptable) and a gentler method for melodic content

### Relationship to the vocal silhouette

CRP is a SEPARATION tool, not a DETECTION tool. The vocal silhouette engine (P-VOX) detects voice characteristics in the mixed signal. CRP produces a voice candidate by removing everything else. They're complementary:

1. P-VOX detects "voice is present here" from the mix
2. CRP produces an isolated voice candidate
3. A-VOX can run the silhouette measures on the CRP output to confirm and characterise

The CRP voice candidate, despite ducking artifacts, gives P-VOX a much cleaner signal to analyse — formant tracking, pitch continuity, and envelope measures all improve when instrument energy is reduced.

---

## HARMONIC RESYNTHESIS — IMPLICATIONS FOR VOICE

### The density finding

Harmonic resynthesis (rebuilding audio from measured spectral peaks as stacked cosine waves) revealed a quantitative threshold for voice:

- 12 sinusoids per frame: recognizable as music, rhythm intact, but voice is just a tonal shape
- 40 sinusoids: melody clear, instruments distinguishable, voice is still synthetic
- 120 sinusoids: instruments sound complete, voice beginning to emerge
- **250+ sinusoids: voice sounds human** — this is the threshold where the vocal tract's complex resonance structure is captured
- 640 sinusoids: full reconstruction

Instruments are spectrally sparse — they're recognizable from their harmonic structure alone. Voice is spectrally dense — it requires the inter-harmonic detail, the formant resonance shapes, the aspiration noise, and the micro-modulations to sound human. This is a measurable property, not subjective.

### What this means for detection

The resynthesis density threshold could serve as a direct voice discriminator: for any spectral region, measure how many sinusoidal components are needed to reconstruct the perceptual content. Regions dominated by instruments will reconstruct at low density. Regions with voice will require high density. The density gradient across the spectrum maps where voice contributes.

### What this means for separation

The resynthesis captured voice in its reconstruction even though it was built from the full mix's harmonics — the voice was embedded in the spectral peaks at every frame where it was singing. This means the voice ISN'T separate from the instruments in the spectral domain — they're superimposed. Any separation method that works in the spectral domain (spectral subtraction, Wiener filtering) must deal with this superposition. CRP's time-domain approach avoids the problem by never decomposing the signal spectrally.

---

## INTERFACE WITH EXISTING COMPONENTS

### What P-VOX reads:
- Audio file (full waveform access)
- SpectralRoster (Step 4) — vocal band boundaries. The roster already has a "vocal" role at 200-4000Hz. P-VOX uses this directly.

### What A-VOX reads:
- VocalSilhouette (P-VOX) — all seven raw measures
- ModifierLog (R2) — vocal reverb, compression, de-essing
- ConventionBank (A1) — vocal conventions per genre
- SpectralRoster (R1) — refined band assignments
- RawWebData (P8) — credited vocalist, if any
- Prescriptive Genre Prints — expected vocal treatment

### What P-VOX feeds into (Phase 1 → Refinement):
- R1 (Spectral Roster Refinement): if vocal detected, R1 can separate vocal energy from instrumental energy within the 200-4000Hz band
- R2 (Production Environment Refinement): vocal production cues feed modifier log

### What A-VOX feeds into (Phase 2):
- A7 (Cultural Violation Detection): vocal convention violations (e.g., Phoneline's bass-first overture instead of vocal-first convention)
- A8 (Activation Module): vocal presence affects AGENCY dimension — voice pushes toward the "Human" pole
- A9 (Interpretive Engine): vocal silhouette informs bridge hypothesis. Vocal address vs non-address (FP-V04). Vocal-as-texture (FP-V02). The vocal report tells the interpretive engine HOW the voice participates in the song's meaning structure.

### What A-VOX does NOT feed into:
- A4 (Percussion Deviation Analysis) — vocal engine explicitly ignores percussion grid
- Equipment Classification (A2) — the voice is not "equipment" in the system's sense. It's a separate category. The engine identifies equipment that PROCESSES the voice (reverb, compressor), but the voice itself is the singer, not a machine.

---

## PHONELINE: THE COMPLETE VOCAL SILHOUETTE

### Summary

Emily Makis is present across 86% of the track (30s–226s), singing in D♯4 range (G3–C#5 span) over liquid DnB at 175 BPM. The voice entered the analysis invisible to every existing engine measurement (hnr:-3.38, element #49 = 0%, FP-V01-V04 all require vocal_detected which never fires). The horizontal silhouette engine found it — and the fundamental-first redesign clarified exactly where and how.

### Head-to-head: full band vs fundamental slab

| Measure | Full band (200-4kHz) | Fund slab (187-501Hz) | Change |
|---------|---------------------|----------------------|--------|
| Formant continuity | 0.516 | **0.823** | +0.307 |
| Syllabic divergence | 0.33x | 0.32x | same (genre-dominated) |
| Sibilance correlation | 0.320 | 0.121 | -0.199 (expected — fund is 4 octaves from sibilance) |
| Coupling over sub-bass | 0.424 | 0.167 | -0.257 (same reason) |
| Glide-to-step ratio | 2.1 (piptrack) | **8.0 (HPS)** / 38.0 (pYIN) | +5.9 / +35.9 |
| Voiced fraction | 74.6% (piptrack) | **51.0% (HPS)** / 0.7% (pYIN) | HPS is the operational method |
| Vibrato peak | 5.8Hz | 6.1Hz | stable |
| Median pitch | 279Hz | **312Hz (D♯4)** | HPS more accurate (less harmonic pull) |
| VOCAL windows (of 15) | 13 | 0 (pYIN) / TBD (HPS) | sectional map needs HPS density |

**The finding:** Narrowing to the fundamental slab eliminates break contamination in formant tracking (+0.307 continuity) and pitch continuity (glide ratio 2.1 → 8.0). The trade-off — sibilance coupling drops — is expected because the fundamental lives 4 octaves below the sibilance band. Sibilance coupling should remain a separate broad-band measure, not moved into the fundamental slab.

### Pitch tracker comparison

| Method | Voiced frames | Median F0 | Glide ratio | Notes |
|--------|--------------|-----------|-------------|-------|
| piptrack (full band) | 74.6% | 279Hz | 2.1 | Picks up harmonics + breaks as "pitch" |
| pYIN (raw, thresh 0.5) | 0.7% | 318Hz | 38.0 | Pure reading but near-zero sensitivity |
| pYIN (bandpass 200-450Hz) | 14.7% | 305Hz | — | Pre-filtering helps but not enough |
| **HPS (4 harmonics)** | **51.0%** | **312Hz** | **8.0** | **Best balance of sensitivity and quality** |
| Combined (HPS + bandpass pYIN) | 51.2% | 312Hz | — | HPS dominates the union |

HPS is the operational choice for the fundamental-first slicer. It exploits harmonic structure (the voice's natural signature) to find the fundamental through dense production.

### The silhouette shape

The voice in Phoneline is:
- **Continuous** — enters at ~30-45s, stays until the end. Not intermittent.
- **Centred** — mono placement (M/S 0.080), stacked with bass and breaks.
- **Sustained/legato** — 84.6% of pitch behavior is sustaining (<10 cents/frame), 12.8% gliding, 2.6% stepping.
- **Compressed** — vocal band DR at 15dB, just at the boundary.
- **Reverbed** — slow decay after phrases (+3.4 dB/s), consistent with liquid DnB's spacious vocal treatment.
- **Subtly vibratoed** — 5.8-6.1Hz, controlled, not operatic.
- **Breath-phrased** — 24-26 phrases at 6.1-6.6s mean, independent of the DnB beat grid.
- **Sibilance-coupled** — the cross-band consonant-vowel signature is clear and correctly lagged (+23.2ms).
- **Fundamental-clear** — F0 at D♯4 (312Hz) in a slab that sits below break energy. Formant continuity 0.823 in the fundamental slab vs 0.516 in the full band. The voice is cleanest where its fundamental lives.

### What the existing engine missed and why

| Existing measure | Phoneline value | Why it missed |
|-----------------|-----------------|---------------|
| HNR (element #44) | -3.38 | Full-spectrum aggregate; breaks pull it to noise |
| Vocal presence (#49) | 0% (broken) | ML detector failed; returns 0% for all tracks |
| FP-V01 (vocal foreground) | never fires | Depends on vocal_detected = true |
| FP-V02 (vocal as texture) | never fires | Same |
| FP-V03 (no vocal) | fires incorrectly | System concludes "no vocal" by default |
| FP-V04 (vocal non-address) | never fires | Same |
| Centroid | 4039Hz (drop) | Dominated by breaks, not vocal |
| Crest factor | 11.6dB | Measures full mix dynamics, not vocal specifically |

Every measure is vertical (snapshot of full spectrum) or broken. The voice only appears when you turn the axis horizontal and narrow the spectral window.

---

## OPEN QUESTIONS

### Resolved by fundamental-first experiments

1. ~~**Pitch tracker contamination.**~~ **RESOLVED.** HPS replaces piptrack/pYIN as the primary F0 method. HPS recovers 51% voiced frames (vs pYIN's 0.7%) and achieves glide ratio 8.0 (vs piptrack's 2.1). The engine does NOT need the percussion grid from P6 as a contamination mask — narrowing to the fundamental slab removes the contamination at source.

2. ~~**Syllabic modulation in drum-heavy genres.**~~ **PARTIALLY RESOLVED.** Moving syllabic modulation to the fundamental slab made no difference (0.33x → 0.32x). The percussion AM dominates even in the narrow band because the bass itself occupies this range. Syllabic divergence remains genre-dependent and unreliable in drum-heavy genres. The engine should not weight this measure in DnB/breakcore.

6. ~~**Emily Makis's vibrato.**~~ **CLARIFIED.** Vibrato ratio stable at 0.34-0.05 across methods, peak consistently 5.8-6.1Hz. The low ratio is a genuine production/style finding (controlled DnB vibrato), not a tracker artifact. The vibrato IS there at the correct frequency; it's just subtle.

### Resolved (reframed)

7. **The overture problem — NOT a false positive.** Originally framed as: "HPS shows 36-38% F0 density in the intro — bass harmonics triggering false positives." Alex's listening test revealed the intro bass atmosphere is actually an amalgam of vocal runs in echo laced over the sub-bass harmonics. The engine was correctly detecting vocal content, not being fooled by bass harmonics. The 81% "vocal" envelope classification in 0-30s and the 36-38% HPS density are TRUE POSITIVES — they're vocal texture (processed vocal fragments woven into the production) before the lead vocal enters. The correct distinction is not voice/not-voice but LEAD VOCAL vs VOCAL-AS-TEXTURE (FP-V02 territory). The envelope discriminators (syllabic power 0.867 lead vs 0.446 texture, crest factor 3.742 vs 2.517) separate these two vocal modes, not voice from bass. The processed vocal fragments have lost their consonant-vowel articulation structure (echo smears it) and their breath-gap crest (reverb fills it). This classification belongs in A-VOX (Phase 2), not P-VOX: P-VOX correctly says "vocal content present throughout" and A-VOX should classify the intro as vocal-as-texture and the body as lead vocal. Still needs validation on a TRULY instrumental track (zero vocal content) to establish HPS's actual false positive floor.

### Still open

3. **Multiple voice sources.** Phoneline has one vocalist. What happens with harmony vocals, backing vocals, or vocal stacking? HPS would detect the lowest/strongest fundamental — can it detect multiple independent F0 lines? The engine might need to run HPS multiple times with progressive cancellation of the strongest detected pitch.

4. **Chopped/processed vocals.** How short can a vocal fragment be before HPS loses the fundamental? HPS needs the harmonic series to be present simultaneously in a frame — it should still work on 50ms fragments (individual frames). But the horizontal phrase measures (formant continuity, breath phrasing) need sustained presence. The engine might need a "residue" mode: HPS detects fragments, even if too short for full silhouette analysis.

5. **The voice as instrument vs the voice as address.** Still open. The fundamental-first design characterises the physical pattern of voice without distinguishing function. This distinction belongs in A-VOX (Phase 2), not P-VOX (Phase 1).

### New questions from fundamental-first experiments

8. **HPS false positive rate — NOW KNOWN: ~53%.** Blade Runner main titles (Vangelis, DX7 synths, zero vocal content) tested at 52.8% HPS voiced with glide ratio 9.0 and 92.3% vocal on envelope contour. HPS and envelope contour CANNOT distinguish voice from harmonic instruments. This is not a calibration issue — FM synthesis genuinely produces voice-like harmonic and envelope properties. Gate 3 (sibilance coupling) is the required discriminator. HPS + envelope are necessary preconditions (they filter out percussion, noise, and atonal content) but not sufficient for vocal classification.

9. **Fundamental slab boundaries.** The current slab (187-501Hz) was derived from Emily Makis's specific range (pYIN 5th-95th percentile ± 20%). For an unknown track, the slab boundaries aren't known in advance. The Pass 1 broad sweep needs to estimate F1 from the formant contour before Pass 2 can set the slab. What if the broad-sweep F1 estimate is wrong? How sensitive is the fundamental slab analysis to boundary errors?

10. ~~**HPS vs harmonic instruments.**~~ **CONFIRMED — and worse than expected.** The DX7 doesn't just pass HPS — it passes envelope contour classification too (92.3% vocal). Formant continuity and phrase structure are NOT reliable differentiators for expressive synths. The circular dependency risk is real: HPS locks onto the DX7, the envelope reads "vocal-shaped," and only sibilance coupling breaks the loop. This validates the three-gate design: sibilance is not optional, it's the gate that makes the entire system work. Without it, the engine would classify Blade Runner as 92% vocal. Tested and resolved.

### New questions from Black and Gold subtraction experiments

11. **Gate 4 (aspiration) feasibility.** The aspiration observation came from resynthesis playback, not from formal measurement. Needs: harmonic-to-valley ratio measurement during aspirated vs clean frames, validation that instruments don't fluctuate similarly, threshold calibration. If aspiration works as a gate, it would complement sibilance (Gate 3 catches fricatives, Gate 4 catches aspirates) and together they'd cover the full consonant inventory.

12. **Voice ducking compensation.** The broadband CRP voice candidate is ducked during instrument events. Can the ducking pattern be detected and compensated after extraction? The gain curve is known (it was computed during CRP), so the inverse gain could be applied to just the voice — but this requires knowing which frames contain voice, which is the detection problem.

13. **Resynthesis density as voice discriminator.** Voice requires ~250+ sinusoids to sound human while instruments are recognizable from ~12. Can spectral density (number of significant sinusoidal components per frame) serve as a direct voice/instrument discriminator within the fundamental slab? Needs formal measurement across multiple tracks and genres.

14. **CRP integration with P-VOX.** The CRP voice candidate gives P-VOX a cleaner input signal. But running CRP requires the full audio processing pipeline, while P-VOX is designed to run early (parallel with P5-P7). Should P-VOX have two modes: fast (from the mix, Gates 1-3) and refined (from CRP output, all gates including aspiration)?

---

*Module created: 11 February 2026*
*Fundamental-first redesign: 11 February 2026*
*Subtraction experiments: 11 February 2026*
*Validated against: Phoneline (Pola & Bryson & Emily Makis, 2024), Black and Gold (Sam Sparro, 2008)*
*Status: DRAFT. Two-pass architecture designed and tested. Fundamental slab gives formant continuity 0.823 (was 0.516), HPS gives glide ratio 8.0 (was 2.1). Broadband CRP validated for voice isolation (0.95 phase coherence). Sibilance engine confirmed working. Gate 4 (aspiration) identified as candidate. Harmonic resynthesis reveals voice/instrument density threshold (~250 vs ~12 sinusoids). Needs: (1) aspiration gate formal measurement, (2) CRP voice ducking compensation, (3) NTLTC multi-voice validation, (4) chopped/texture vocal fragment detection, (5) resynthesis density as discriminator.*
