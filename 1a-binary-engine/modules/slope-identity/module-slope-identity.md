# MODULE: HORIZONTAL SLOPE ENGINE (Instrument Identity Tracker)
## Status: PROTOTYPED — tested on Blade Runner Main Titles and Black and Gold
## Origin: Alex's observation that voice detection should be subtractive — map every instrument, voice is the residual

---

## THE PROBLEM

The Vocal Silhouette Engine (module-vocal.md) attempts to find voice by positive detection — three gates (HPS, envelope contour, sibilance coupling) that voice passes. But the DX7 finding proved that harmonic instruments pass Gates 1 and 2, and the only thing separating a synth from a singer is Gate 3 (sibilance). This is fragile: it depends on a single cross-band measure, and it fails for vocal texture (processed vocals with sibilance destroyed).

Alex's insight: everything melodic in music is voice-derived. Instruments were built to sing. Trying to find voice by what voice IS will always produce false positives because instruments share voice's fundamental properties (harmonics, pitch movement, phrase-like modulation). The voice is the MOST VARIABLE element in any mix. Every instrument is more consistent.

The subtractive approach: identify every instrument by its consistent properties (slope, timbre, pitch grid adherence). Track each as a persistent identity across the song. Subtract them all. What's left — the source that refuses to hold a stable fingerprint — is the voice.

---

## THE APPROACH

### The horizontal slope

The existing engine measures instruments VERTICALLY: snapshot statistics of spectral profiles, harmonic ratios, attack/decay at individual events. P2 (Equipment Signals) extracts these per-event numbers. A2 (Equipment Classification) matches them against the Equipment Registry.

The horizontal slope engine runs along the TIME AXIS. Instead of asking "what is the spectrum at this moment?", it asks "what is the shape of this sound event from start to finish?" A played note is a slope in the spectrogram: onset → sustain → decay. The shape of that slope is the instrument's identity.

### Tempo-based interval search

Most melodic events in a song align to the tempo grid. At 175 BPM (Phoneline):
- 16th note: ~86ms
- 8th note: ~171ms
- Quarter note: ~343ms
- Half note: ~686ms
- Full bar: ~1.37s

The engine uses the tempo (already available from P6 Percussion Grid or Step 3 Global Statistics) to set initial search windows. Within each tempo-aligned interval, look for energy events: onsets, sustains, decays. When an event is found, expand its envelope horizontally until the start and end are located.

### Three-layer instrument identity model

#### Layer 1: THE SLOPE (identity — fast matching)

The clean fingerprint. A note played on an instrument, stripped to essentials:

```
Slope {
  onset_frequency:    float       // where the note starts in Hz
  onset_shape:        float[]     // attack profile (first 10-50ms of energy rise)
  decay_rate:         float       // dB/s of energy loss after sustain
  harmonic_ratios:    float[8]    // relative amplitudes of first 8 harmonics
  spectral_centroid:  float       // center of mass of the harmonic series
  bandwidth:          float       // spectral spread
}
```

The slope is PRESERVED as the canonical imprint for fast comparison. When you find a slope at 330Hz and want to check if the same instrument is at 370Hz, you compare slopes. The harmonic ratios, onset shape, and decay rate should match (transposed but identical in shape). Same instrument = same slope, different pitch.

This is the duodecimal search key. Once you have one slope, check all 12 chromatic positions × relevant octaves for matching slopes. The instrument's identity holds across its pitch range.

#### Layer 2: THE PARABOLA (physics — characteristic envelope)

The slope in practice. Real notes don't decay linearly — they curve. The parabola captures the actual amplitude envelope once physics are applied:

```
Parabola {
  slope_ref:          SlopeID     // which slope this is an instance of
  attack_ms:          float       // time from onset to peak
  attack_curve:       float       // shape of attack (linear=1, exponential>1, logarithmic<1)
  sustain_level:      float       // level relative to peak during hold
  sustain_sag:        float       // rate of level drop during sustain (0 = flat, >0 = drooping)
  release_ms:         float       // time from note-off to silence
  release_curve:      float       // shape of release
  total_duration:     float       // onset to silence
}
```

The parabola is the slope with ADSR physics applied. Different instances of the same slope (same instrument playing different notes) should produce similar parabolas — the piano's decay curve is the piano's decay curve whether it's playing C4 or F#5. The parabola shape is a secondary identity confirmation: if slopes match AND parabolas match, high confidence it's the same instrument.

#### Layer 3: DEVIATION (expression — what changes)

Deviation from the parabola is where musical expression lives:

```
Deviation {
  parabola_ref:       ParabolaID
  amplitude_delta:    float       // how much louder/quieter than expected (velocity)
  spectral_delta:     float[]     // how the harmonic ratios differ from the slope (filter, EQ)
  duration_delta:     float       // how much longer/shorter than expected
  pitch_drift:        float       // cents deviation from expected frequency
  temporal_drift:     float       // ms early or late relative to grid
}
```

Deviations tell you how the instrument is being PERFORMED or AUTOMATED:
- Velocity variation: the player's dynamics (or velocity automation)
- Spectral variation: filter sweeps, EQ automation, timbral evolution
- Duration variation: note length changes (legato vs staccato passages)
- Pitch drift: vibrato, pitch bend, detuning, tape artifacts
- Temporal drift: groove, swing, human timing vs quantised grid

**The voice won't fit any of this.** A voice doesn't have a consistent slope — its harmonic ratios change every syllable as the vocal tract reshapes. Its parabola changes every phrase because breath dynamics vary. Its "deviations" aren't deviations from a stable baseline — they ARE the baseline. The voice is the source whose Layer 1 never stabilises into a reusable fingerprint.

---

## THE ALGORITHM

### Step 1: Event detection (tempo-aligned)

Scan the spectrogram along the time axis in tempo-aligned windows. Within each frequency band of the SpectralRoster (already available from Step 4), detect amplitude events:
- Onset: energy rises above noise floor by >6dB within one tempo subdivision
- Sustain: energy remains within 3dB of peak for >50ms
- Decay: energy falls below noise floor

Each detected event becomes a candidate for slope extraction.

### Step 2: Slope extraction

For each event, extract the slope fingerprint:
- Measure harmonic ratios at the energy peak (most stable moment)
- Measure onset shape (attack profile)
- Measure decay rate (from peak to -20dB)
- Record the onset frequency

### Step 3: Slope clustering

Group events by slope similarity. Events with matching harmonic ratios, similar onset shapes, and similar decay rates are likely from the same instrument. Use the SpectralRoster's role assignments as a starting point — events in the "harmonic_bass" band probably share a slope.

Distance metric for slope matching:
```
d(slope_a, slope_b) = w1 * cosine_distance(harmonics_a, harmonics_b)
                    + w2 * |decay_rate_a - decay_rate_b| / max_decay
                    + w3 * onset_shape_distance(a, b)
```

Events within distance threshold form a cluster = one instrument identity.

### Step 4: Duodecimal expansion

For each identified slope cluster:
1. Determine the pitch class of the strongest event
2. Calculate the 12 chromatic transpositions of that pitch
3. At each transposed frequency, search for events with matching slope (adjusting harmonic ratios for the transposition)
4. Add matches to the cluster
5. Repeat across octaves

This is where music theory helps: if the song is in E minor, the expected pitches are E, F#, G, A, B, C, D (natural minor) plus chromatic passing tones. Check these positions first, weighted by their likelihood in the key.

### Step 5: Parabola fitting

For each slope cluster (instrument identity), fit a parabola to the envelope of each event:
- Average the parabolas to get the instrument's characteristic envelope
- Compute deviations per event
- Track how deviations evolve over time (filter opening? getting louder? changing attack?)

### Step 6: Source map construction

The output: a complete map of identified instrument sources across the timeline.

```
SourceMap {
  sources: [
    {
      source_id:        int
      slope:            Slope           // canonical fingerprint
      parabola:         Parabola        // characteristic envelope
      events:           Event[]         // every detected instance
      pitch_classes:    int[]           // which notes it plays (0-11)
      time_range:       [float, float]  // first to last appearance
      deviation_trend:  Trend           // how deviations evolve (stable, warming, brightening)
    }
  ]
  unmatched_energy:     float[]         // per-frame energy not assigned to any source
                                         // ← THIS IS THE VOCAL CANDIDATE
}
```

### Step 7: Subtraction

For each frame of the spectrogram:
- Sum the expected energy from all identified sources (using their slopes, parabolas, and the current event if any)
- Subtract from the total energy in the fundamental slab
- The residual is energy that belongs to NO identified instrument
- If the residual has the horizontal characteristics of voice (phrase-scale modulation, pitch continuity, syllabic AM) → voice detected

---

## ARCHITECTURE: WHERE THIS FITS

### Integration with existing pipeline

```
EXISTING:
  Step 4: SpectralRoster (role → frequency band mapping)
  P2: EquipmentSignals (per-event spectral profiles, harmonics, CV)
  P6: PercussionGrid (tempo, beat positions)
  R1: Roster Refinement (validated band assignments)
  A2: Equipment Classification (synthesis family identification)

NEW — HORIZONTAL SLOPE ENGINE:
  Position: Between P2 and R1 (uses P2's raw signals + P6's tempo, feeds into R1 and A2)

  P-SLP: SLOPE IDENTITY EXTRACTION                    [HORIZONTAL]
  ────────────────────────────────────
    Does:     Horizontal slope analysis:
              · Tempo-aligned event detection per SpectralRoster band
              · Slope extraction (harmonic ratios, onset, decay)
              · Slope clustering (same instrument = same slope)
              · Duodecimal expansion (chromatic search for same slope)
              · Parabola fitting (characteristic envelope per source)
              · Deviation tracking (expression, automation, evolution)
              · Source map construction
              · Residual calculation (unmatched energy)
    Reads:    ◆ Audio
              ◆ SpectralRoster (Step 4 — band assignments)
              ◆ EquipmentSignals (P2 — per-event raw profiles)
              ◆ PercussionGrid (P6 — tempo, beat positions)
              ◆ GlobalStats (Step 3 — key, tempo, time signature)
    Produces: SourceMap
              (identified_sources[], unmatched_energy[],
               slope_clusters, parabola_fits, deviation_trends)

    Runs after P2 and P6 complete. Feeds into R1 and A2.
    P-SLP replaces part of what A2 currently does (instrument matching)
    but adds temporal coherence that A2 currently lacks.
```

### What changes for existing components

**R1 (Spectral Roster Refinement)** gains a new input: SourceMap. Instead of just validating "is there energy in this band?", R1 can now say "there are 3 identified instruments in this band, plus unmatched residual."

**A2 (Equipment Classification)** shifts from per-event matching to per-source matching. Instead of classifying individual spectral snapshots against the Equipment Registry, A2 classifies slope clusters — persistent identities with full temporal context. This is more accurate because it has more data per classification decision.

**The Vocal Silhouette Engine (P-VOX / A-VOX)** gains a radically different input: instead of running positive detection on the full fundamental slab, it receives the SourceMap's `unmatched_energy` — the residual after all instrument subtraction. The voice is whatever the slope engine couldn't assign to an instrument. The three-gate architecture (HPS, envelope contour, sibilance) becomes a CONFIRMATION check on the residual rather than the primary detector.

### New dependency chain

```
Step 3 (Global Stats: key, tempo) ─┐
Step 4 (SpectralRoster)            ─┤
P2 (EquipmentSignals)              ─┼──→ P-SLP (Slope Identity) ──→ SourceMap
P6 (PercussionGrid: tempo, beats)  ─┘            │
                                                  ├──→ R1 (gains SourceMap)
                                                  ├──→ A2 (gains SourceMap, shifts to per-source)
                                                  └──→ P-VOX (gains unmatched_energy as primary input)
```

---

## THE SLOPE / PARABOLA / DEVIATION MODEL IN DETAIL

### Why three layers, not one

A single fingerprint could capture all the information. But separating into slope → parabola → deviation gives three speeds of matching:

**Slope (fast, identity check):** "Is this the same instrument?" Compare 8 harmonic ratios + decay rate + onset shape. This is a vector comparison — fast enough to run across all 12 chromatic positions × octaves in real time. Use this for the duodecimal expansion search.

**Parabola (medium, confirmation):** "Does this event from the same instrument have the expected envelope?" Fit the ADSR curve and compare against the slope cluster's average parabola. This confirms the slope match and adds the temporal envelope dimension. Use this to reject false slope matches (two instruments with similar harmonic content but different envelopes).

**Deviation (slow, characterisation):** "How is this instrument being played/automated right now?" Compute per-event deviations from the parabola. Track trends over time. This is the musical interpretation layer — it tells A2 and the Interpretive Engine how the instrument's role is evolving across the song. Not needed for identity matching, needed for musical understanding.

### The voice through this lens

A voice attempt at slope extraction:
- Harmonic ratios: change every 50-100ms as the vocal tract reshapes (vowel → consonant → vowel)
- Onset shape: different every phrase (breath onset vs continuation vs glottal stop)
- Decay rate: inconsistent (some phrases trail off, some are cut short, some are sustained)

A voice attempt at parabola fitting:
- No stable ADSR pattern. Each phrase has different attack (breath), sustain (varies with lyric), release (breath gap vs reverb tail)
- The "parabola" would have massive deviation at every event

A voice attempt at slope clustering:
- No cluster forms. The slope fingerprint at event N doesn't match the slope fingerprint at event N+1 because the vowel changed, the dynamic changed, the pitch changed, the formant structure changed.

Result: the voice shows up as unmatched_energy in the SourceMap. Not because we looked for voice, but because we looked for everything else and it didn't fit.

---

## WORKED EXAMPLE: PHONELINE (200-450Hz fundamental slab)

### Expected source identification

In the 200-450Hz band, the slope engine should find:

1. **Bass harmonics** — the sub-bass (32-97Hz) has 2nd, 3rd, 4th harmonics in this range. The slope: consistent harmonic ratios (integer multiples of the fundamental), slow decay (bass notes sustain), predictable pitch grid (bass follows root movement, probably E minor). The slope engine finds one instance, expands duodecimally, maps the full bass line.

2. **Synth pad harmonics** — if there's a pad in the mix, its harmonics reach into this band. The slope: very slow attack, long sustain, gentle decay. Different from the bass (attack shape differs, harmonic ratios differ). Forms its own cluster.

3. **The bouncing synth** — Alex heard its attack in the 200-450Hz bandpass. Short events, sharp attack, fast decay. The slope: percussive onset, rapid falloff, specific harmonic profile. Duodecimal expansion finds its other notes.

4. **Vocal texture (intro, 0-30s)** — the echoed vocal runs. These WON'T form a clean slope cluster because they're processed vocal fragments with varying timbre. They'll partially cluster (the echo creates repetition) but with high deviation. Semi-matched energy.

5. **Emily's voice (30-226s)** — unmatched. The slope engine can't form a stable cluster because her harmonic ratios change every syllable. Residual energy.

### Expected residual

The `unmatched_energy` in the 200-450Hz band should spike when Emily is singing and drop during purely instrumental passages. This residual, fed into the Vocal Silhouette Engine, becomes the primary vocal detection signal — cleaned of all instrument energy, leaving only what no instrument fingerprint can explain.

---

## OPEN QUESTIONS

1. **Slope stability across pitch range.** An instrument's harmonic ratios ARE somewhat pitch-dependent (a piano sounds different at C2 vs C6). The slope should be approximately preserved across a moderate range (one octave?) but may need per-octave refinement. How much pitch-dependent variation should the slope tolerate before splitting into separate clusters?

2. **Polyphonic instruments.** A piano chord has multiple simultaneous fundamentals. The slope engine would detect multiple events in the same time window. How to handle: each note gets its own slope, but they share a cluster (same instrument). The parabola should be similar across chord members.

3. **Instruments that cross roles.** A synth bass might have harmonics in the harmonic_bass band AND the fundamental slab. The slope engine sees the same instrument in two SpectralRoster bands. Should it merge them or keep them separate?

4. **Processing that changes the slope.** Sidechain compression (pumping), filter automation, and other time-varying effects change the parabola dynamically. The deviation layer handles this, but at what point does deviation become so large that the parabola is no longer useful? When should the engine declare "this instrument's envelope has fundamentally changed" rather than "this is a large deviation from the baseline"?

5. **Vocal texture vs heavily processed instruments.** Both will have unstable slopes. The vocal texture (Phoneline intro) is processed vocal fragments — unstable slopes but with residual vocal characteristics (sibilance traces, formant movement). A heavily automated synth might also have unstable slopes. The sibilance cross-check (Gate 3 from the vocal engine) would distinguish them, but this reintroduces the positive-detection dependency we're trying to escape. Can the slope instability pattern itself distinguish vocal residual from instrument automation?

6. **Computational cost.** HPS already runs per-frame. Adding slope extraction + duodecimal expansion + parabola fitting for every detected event could be expensive. The tempo-aligned search windows help (fewer candidates), but a dense mix at 175 BPM with multiple instruments could generate thousands of events. Can the slope comparison be vectorised efficiently?

---

---

## PROTOTYPE RESULTS

### Blade Runner Main Titles (Vangelis, 1982)

The ultimate stress test — a DX7 synth orchestra designed to sound like one continuous undulating wave. Alex's assessment: "I cant detect a single separation in sound, in that whole track. its like one giant undulating ocean wave."

**Results:** 14 discrete instrument sources identified across 7 frequency bands, 347 total events.

Key findings:
- Source 1 (sub_bass, 43Hz): 2nd harmonic louder than fundamental (ratio 2.19) — classic FM synthesis signature. The DX7's operator stack produces this inverted harmonic weighting.
- ADSR separation worked: pad/string sources had 522ms mean attack, organ patches had 58-81ms mean attack. The parabola layer correctly grouped these.
- Deviation analysis showed most sources UNSTABLE (high coefficient of variation) — consistent with DX7 modulation varying hugely per note. The engine reads this as "expressive instrument" rather than "voice" because the harmonic ratios stay consistent even when amplitude varies.
- Duodecimal expansion was too generous — almost every source appeared at all 12 chromatic positions. The cosine threshold (0.40) needs tightening for dense harmonic material. Reduced to 0.25 for the Black and Gold run.

**Assessment:** The engine found real separations in material designed to have none. The three-layer model (slope/parabola/deviation) correctly distinguished sources by their physics even when they sound perceptually fused.

### Black and Gold (Sam Sparro, 2008)

Cleaner mix, clearer separation between voice, synth, drums, and bass.

**Results:** 17 sources, 732 events, key of E minor, tempo 136 BPM. Tightened thresholds: clustering 0.30, chromatic matching cosine < 0.25, minimum 4 events per cluster.

Critical finding — the voice absorption problem:
- Residual energy analysis showed only 0-2% unexplained energy in the vocal band (200-4000Hz)
- The engine OVER-EXPLAINED the mix — it absorbed vocal energy into instrument clusters
- Low_mid Source 1 was flagged UNSTABLE (CV 1.649) and likely contains voice mixed with instrument harmonics
- The slope engine's greedy clustering pulls vocal events into the nearest instrument cluster because voice shares harmonic properties with the instruments accompanying it

**Assessment:** The subtractive premise is correct (map instruments, voice is the residual) but the slope engine's clustering is too aggressive. It needs a stability gate: sources with CV above a threshold should be flagged as potential voice contamination rather than classified as "expressive instrument."

### The Subtraction Problem — Five Approaches Tested

After the slope engine identified instrument sources, five different methods were tested for actually subtracting them to isolate the voice:

#### 1. Spectral magnitude subtraction
Subtract amplitude at each instrument's fundamental + 8 harmonics in the STFT.
- Removed only 18.2% of energy — subtraction too narrow (only at harmonic peaks, misses spectral spread)
- Instruments file sounded "completely detuned" — phase incoherence
- Phase problem: subtracting magnitude from STFT while keeping original phase creates incoherence because the phase was set by the combined signal, not individual sources
- **However:** sibilance-only export (4-8kHz, 98.5% remaining) was "perfectly spot on" per Alex — a working sibilance engine

#### 2. Inverse square law distance model
Simulate atmospheric absorption at increasing distances — high frequencies attenuate faster.
- Physics is correct: at 500m, 100Hz loses 54dB while 8kHz loses 84dB (30dB separation gap)
- But inverse-square component dominates at practical distances — all difference layers showed same ~41% sub_bass / 31% bass distribution
- Useful as a textural tool (the distance exports sound beautiful) but not a practical separation method

#### 3. Coherent Relative Phase — filter bank (Alex's invention)
Stay in time domain. Detect instrument events as parabolas on the amplitude envelope. Multiply waveform by gain curve shaped as inverse of instrument envelope. Applied per frequency band.
- 1998 drum parabolas detected, median 111ms, 34ms attack, 45ms decay
- Phase coherence: **0.67** — better than spectral subtraction (~0.4) but filter bank introduced phase smearing at crossover frequencies
- Alex's feedback: "the vocals are actually really good. they sound sidechained to a subbass i cannot hear, but theyre totally there"

#### 4. Coherent Relative Phase — BROADBAND (Alex's refinement)
Same principle but without any frequency splitting. One signal, one envelope, one gain curve per pass. Three passes at different time scales: transients (120ms floor window), melodic (400ms), sustained (1500ms).
- Phase coherence: **0.95** — up from 0.67 with filter bank, up from ~0.4 with spectral subtraction
- Energy distribution: transients 45.4%, melodic 4.3%, sustained 0.8%, voice residual 1.8%
- Alex's feedback: "it sounds like im listening to mostly just sam sparro sing at me, but through a Teams meeting window, and his net connection is a bit choppy. the choppiness carries the ghost shape of maybe a programmed drum pattern"
- The core limitation: broadband gain turns down EVERYTHING during instrument events, including the voice. Voice sounds sidechained.

#### 5. Phase cancellation via harmonic resynthesis
Resynthesize each instrument event from STFT harmonics (12 harmonics per event), flip phase, add to original.
- Cancellation FAILED: resynthesis was 310% of original energy, 0.1% cancellation accuracy
- BUT: the resynthesis itself contained recognizable singing and melody
- Alex: "you built that from shapes? you made the voice emerge from shapes??"
- Led to the harmonic resynthesis module (see below)

### Method comparison (phase coherence)

```
Method                              Phase coherence    Notes
─────────────────────────────────────────────────────────────────────
Spectral magnitude subtraction      ~0.4               Detuned, phase-destroyed
CRP filter-bank (7 bands)           0.67               Phase smeared at crossovers
CRP broadband (no splitting)        0.95               Near-perfect phase preservation
Phase cancellation                  N/A                Failed as subtraction, succeeded as resynthesis
```

The key insight, from Alex: "why not just do it without splitting it" — removing the filter bank and working broadband jumped phase coherence from 0.67 to 0.95. The gain curve works because multiplication preserves all relative phase relationships. Decomposition into bands (whether spectral or filter-bank) is what destroys phase.

### The CRP Algorithm (validated)

The broadband Coherent Relative Phase method:

```
For each pass (transient → melodic → sustained):
  1. Compute Hilbert envelope of the signal
  2. Compute floor = local minimum of envelope (minimum_filter1d, window size sets time scale)
  3. Smooth the floor (uniform_filter1d, half the window)
  4. Build gain curve: gain = floor / envelope where envelope significantly exceeds floor
  5. Smooth gain curve to prevent clicks (2-5ms uniform filter)
  6. Apply: y_output = y_input × gain
  7. Extract: y_removed = y_input × (1 - gain)
  8. Feed y_output into next pass
```

Pass parameters (Black and Gold, 136 BPM):
- Pass 1 transients: 120ms floor window, threshold_ratio 0.2, min_gain 0.05
- Pass 2 melodic: 400ms floor window, threshold_ratio 0.25, min_gain 0.05
- Pass 3 sustained: 1500ms floor window, threshold_ratio 0.3, min_gain 0.05

### Harmonic Resynthesis Discovery

The failed phase cancellation attempt led to an unexpected discovery: rebuilding audio from STFT harmonic measurements produces recognizable music at remarkably low fidelity.

Quality ladder (peaks per frame × harmonics per peak):
- Ghost (3×4 = 12 sinusoids): recognizable as music, rhythm intact
- Sketch (5×8 = 40): melody clear, timbre emerging
- Draft (10×12 = 120): instruments distinguishable
- Clear (20×16 = 320): voice sounds human — this is the threshold
- HiFi (40×16 = 640): full reconstruction

Tested across three tracks:
- Black and Gold: spectral correlation 0.80
- Phoneline: spectral correlation 0.86
- Blade Runner: spectral correlation 0.82

Alex's observation: "theyre literally all musical from the getgo. the vocals are what shift into being human towards the Clear pass." This quantifies the subtractive premise: instruments are spectrally sparse (recognizable from 12 sinusoids), voice is spectrally dense (needs ~250+ sinusoids to sound human).

Additional discovery — aspiration as voice fingerprint:
Alex noticed that open-mouth postures with breath (aspiration) read as louder in the resynthesis but not in the original song. The breathy consonant transitions ("ouHHT", "mHHHiiiNNd") carry broadband energy that the STFT reads as high amplitude. Instruments don't aspirate. This could be a fourth gate for voice detection — see module-vocal.md.

Additional discovery — micro-timing exposure:
Resynthesis strips noise transients, exposing tonal onset timing. Drum hits revealed sub-millisecond grid offsets constituting "groove." The synths sound brighter and twinklier because upper harmonics are rebuilt as clean cosines without compression artifacts, inter-modulation distortion, or spectral masking.

---

## OPEN QUESTIONS (updated)

### Resolved by prototyping

1. ~~**Slope stability across pitch range.**~~ **PARTIALLY RESOLVED.** Blade Runner's DX7 sources showed consistent harmonic ratios across pitch with high deviation in amplitude. The slope layer (harmonic ratios) IS stable; the deviation layer correctly captures the expression. Duodecimal expansion works but needs tight cosine thresholds (0.25, not 0.40) for dense harmonic material.

2. ~~**Computational cost.**~~ **RESOLVED.** Black and Gold (227s at 22050Hz) processed in ~30 seconds for slope extraction, ~10 seconds for each CRP broadband pass. The broadband CRP is dramatically cheaper than the filter-bank version because it avoids 7× bandpass filtering. Vectorisable — the gain curve computation is the bottleneck (currently a Python loop, trivially parallelisable).

6. ~~**What method preserves phase?**~~ **RESOLVED.** Stay in the time domain. Multiply by a gain curve, never decompose into magnitude/phase. The broadband CRP achieves 0.95 phase coherence. Alex solved this with a hand-drawn diagram on lined paper.

### Still open

3. **Voice absorption into instrument clusters.** The slope engine's greedy clustering absorbs vocal energy into the nearest instrument cluster. A stability gate (CV threshold) might help — sources with high deviation should be flagged as potentially voice-contaminated rather than classified as instruments. Alternatively, the CRP subtraction bypasses this problem entirely by not requiring source identification at all.

4. **CRP voice ducking.** The broadband gain curve turns down everything during instrument events, including the voice. The voice sounds sidechained. Possible fixes: frequency-selective gain (re-introduces filter problems), voice-aware gain (requires knowing where the voice is — circular), or post-CRP voice enhancement.

5. **Resynthesis as separation tool.** The harmonic resynthesis unexpectedly captured voice in its reconstruction. Could resynthesis be used FOR separation — resynthesize only instrument harmonics, subtract the resynthesis from the original, residual is voice? This is effectively what the phase cancellation attempted, but with better resynthesis fidelity it might work.

7. **Sibilance engine integration.** The 4-8kHz sibilance export from spectral subtraction was declared a working sibilance engine by Alex. How does this integrate with the slope engine? Sibilance is broadband noise, not harmonic — the slope engine can't track it. It should be a parallel extraction that feeds into the vocal module.

---

*Module created: 11 February 2026*
*Prototyped: 11 February 2026*
*Tested against: Blade Runner Main Titles (Vangelis, 1982), Black and Gold (Sam Sparro, 2008)*
*Status: PROTOTYPED. Slope extraction and clustering validated on two tracks. Five subtraction methods tested — broadband CRP (0.95 phase coherence) is the validated method. Harmonic resynthesis discovered as a side product. Needs: (1) stability gate for voice-contaminated clusters, (2) CRP voice ducking fix, (3) integration of sibilance engine, (4) validation on Phoneline, (5) multi-track regression testing.*
