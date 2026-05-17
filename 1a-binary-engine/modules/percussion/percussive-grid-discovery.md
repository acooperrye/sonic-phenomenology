# PERCUSSIVE GRID DISCOVERY
## Rhythm Dictionary — Spec Draft
## 2026-02-11

---

## WHAT THIS IS

A method for discovering a song's own percussive timing grid from spectral evidence, then generating an expectation map of where specific percussive elements should land, and logging every deviation.

**This is not a metronome.** The grid is not imposed — it's derived from cross-sectional spectral observation within the song itself. Reverb tails, echoes, chorus effects, fade-ins, swing, humanisation, and deliberate drift all exist in real audio. The grid must be found phenotypically (from what the audio actually shows) before any mathematical model is applied.

**Principle:** Do not use pure maths until you know the math of the song is right. Always look for cross-sectionally spectrally-evidenced intervals that correlate with web content production data.

---

## DISCOVERY METHOD

### Step 1: Find the hits

For each percussive band (see Element Profiles below), run onset detection within that band only. Do not detect onsets across the full spectrum — that conflates kick transients with hi-hat transients with vocal consonants.

```
Per-band onset detection:
  1. Bandpass filter to element's spectral home
  2. Compute spectral flux within that band
  3. Apply adaptive threshold (local median + k * local std)
  4. Peak-pick above threshold → onset timestamps

Output: raw onset list per band
  e.g. kick_onsets: [0.375, 0.875, 1.375, 1.876, 2.375, ...]
  e.g. hat_onsets:  [0.125, 0.250, 0.376, 0.500, 0.624, ...]
```

**Critical:** These are the measured reality. They will not be perfectly equidistant. They will have jitter from swing, humanisation, audio encoding artefacts, reverb pre-ringing, ghost transients from bleed, etc. That's fine. That's the data.

### Step 2: Derive intervals from the hits (not the other way around)

From each onset list, compute inter-onset intervals (IOIs):

```
kick_iois: [0.500, 0.500, 0.501, 0.499, 0.500, ...]
hat_iois:  [0.125, 0.126, 0.124, 0.125, 0.124, ...]
```

Then find the **modal interval** — the most common IOI (within a tolerance window of +/- 5%). This is the song's own pulse for that element, as evidenced by the audio.

```
kick_modal_ioi: 0.500s  → implies 120 BPM at half-note spacing
hat_modal_ioi:  0.125s  → implies 120 BPM at sixteenth-note spacing
```

Cross-reference: do the modal IOIs from different elements share a common factor? If kick = 0.500s and hat = 0.125s, the ratio is 4:1 — consistent with a shared grid where the kick lands every 4 hat positions. This cross-sectional agreement is the confirmation that a grid exists. Without it, you don't have a grid yet.

### Step 3: Cross-validate with global tempo

Compare the spectrally-derived grid against the tempo from the scout pass (sr=11025 autocorrelation). They should agree. If they don't:

- **Scout tempo is wrong:** autocorrelation picked up a harmonic or subharmonic. Trust the per-band IOI evidence.
- **Per-band IOIs are noisy:** not enough clean onsets to find a reliable mode. Trust scout tempo as fallback, but flag low confidence.
- **Genuinely different tempos coexist:** polymetric content, tempo changes, or rubato. Flag for manual inspection.

### Step 4: Build the expectation grid

Once the song's own grid is confirmed from spectral evidence, generate the full set of expected positions:

```
GridExpectation {
  song_id:        string
  bpm:            float           // derived, not assumed
  bpm_source:     "spectral_ioi" | "scout_autocorrelation" | "web_metadata"
  bpm_confidence: float           // agreement between sources

  grid_subdivisions: {
    whole:    float   // seconds per bar (e.g. 2.000 at 120bpm 4/4)
    half:     float   // e.g. 1.000
    quarter:  float   // e.g. 0.500
    eighth:   float   // e.g. 0.250
    sixteenth: float  // e.g. 0.125
    thirtysecond: float // e.g. 0.0625
  }

  // The grid positions are generated from these subdivisions
  // across the song's duration, anchored to the first confirmed
  // cross-sectionally-agreed onset (not to t=0)
  grid_anchor: float  // timestamp of first confirmed downbeat
}
```

**The grid anchors to the audio, not to the file start.** Songs have intros, fade-ins, ambient openings. The downbeat might be at 0.8s or 4.2s or wherever. Anchor to the first position where multiple percussive bands agree on an onset.

### Step 5: Map elements to grid positions

For each element, snap its detected onsets to the nearest grid position (within tolerance). Then produce:

```
ElementGridMap {
  element:          string     // "kick", "snare", "hat_closed", etc.
  band:             [float, float]  // Hz range where it was detected

  positions: [
    {
      grid_position:  int      // index into the subdivision grid
      subdivision:    string   // "quarter", "eighth", "sixteenth", etc.
      bar:            int      // which bar
      beat:           float    // beat within bar (1.0, 1.5, 2.0, etc.)

      status:         "confirmed"   // onset detected at this position
                    | "expected_absent"  // no onset, but genre says this is normal
                    | "unexpected_absent" // no onset, genre says there should be one
                    | "unexpected_present" // onset detected, genre says it shouldn't be here
                    | "ghost"       // onset detected but below typical amplitude (ghost note)

      amplitude:      float    // relative to element's peak in song
      snap_error:     float    // ms offset from ideal grid position (swing/humanisation)
    }
  ]

  // Summary stats
  fill_percentage:    float    // % of possible grid positions occupied
  expected_fill:      [float, float]  // genre range (e.g. [0.25, 0.50] for kick in DnB)
  swing_amount:       float    // systematic snap_error bias on offbeats (0 = straight, >0 = swung)
  structural_variation: {
    // fill_percentage per song section, if BandPresenceMap is available
    per_section: [{ label: string, fill: float }]
  }
}
```

---

## ELEMENT PROFILES

Each profile defines what the system looks for, where, and what's realistic.

### Physical constraints (from feltness module headroom analysis)

The minimum viable event spacing is the point below which the sound can no longer function as a discrete percussive event — it either fuses into a tone/drone (high end) or can't complete enough cycles to be identifiable (low end).

| Element | Min viable spacing | Reasoning |
|---------|--------------------|-----------|
| Kick (sub-bass component) | ~100ms | Needs 2-3 cycles at 30-50Hz to register as pitched thump |
| Kick (attack component) | ~30ms | Attack transient is broadband, resolves fast |
| Snare | ~50ms | Body resonance at 150-300Hz needs a few cycles |
| Clap | ~40ms | Layered micro-transients, but composite resolves fast |
| Hi-hat closed | ~15ms | High frequency, minimal sustain needed |
| Hi-hat open | ~80ms | Needs sustain/decay tail to read as "open" |
| Crash/ride | ~100ms | Needs ring to distinguish from hat |
| Rim shot | ~30ms | Pure transient, minimal sustain |

Below these spacings, elements fuse. This is the physics floor.

### The Stupid Ceiling

At the other end: at what point does the fill density become perceptually absurd?

```
StupidCeiling {
  // Genre that deliberately maxes out: breakcore
  // Breakcore parameters represent the practical upper bound
  // of what is still intentional rhythm (vs noise/texture)

  breakcore_reference: {
    kick_fill:    0.75    // up to 75% of sixteenth positions
    snare_fill:   0.60    // up to 60% of sixteenth positions
    hat_fill:     0.90    // up to 90% of thirtysecond positions

    // At 170 BPM, sixteenth = 88ms spacing
    // Kick at 75% fill = avg 117ms between hits
    // This is right at the sub-bass min viable spacing
    // i.e. breakcore is operating AT the physics floor

    // Above breakcore fill rates, you're not doing rhythm anymore.
    // You're doing texture. That's a different analysis mode.

    fusion_threshold: 0.85  // above this fill % at any tempo where
                            // spacing < 2x min viable, flag as
                            // "percussive fusion" — element has become
                            // tonal/textural, not rhythmic
                            // (see: Circle Pit amen snares at 5.7 hits/sec → harmonic)
  }
}
```

### Element profiles by genre

These are **expected ranges**, not rules. They narrow the search space and define what "unexpected" means.

```
ElementProfile {
  element:            string
  spectral_home:      [float, float]   // Hz — where to look
  envelope_signature: string           // what the onset shape looks like

  genre_expectations: {
    [genre_id]: {
      typical_subdivision: string      // what grid level it usually lands on
      fill_range:         [float, float]  // min-max % of those positions filled
      structural_notes:   string       // how it varies across song sections
    }
  }
}
```

#### KICK

```
spectral_home: [30, 150]   // sub-bass body + low-mid attack
envelope: transient with variable sustain (808 bloom = long, acoustic = short)

genre_expectations:
  four_on_floor (house/techno/disco):
    subdivision: quarter
    fill: [0.95, 1.00]     // it's literally every beat, that's the genre
    structural: "drops out in breakdowns, returns at drops"

  dnb:
    subdivision: eighth
    fill: [0.25, 0.50]     // syncopated, not on every beat
    structural: "pattern mutates across 4-8 bar phrases"

  hip_hop:
    subdivision: eighth
    fill: [0.30, 0.60]     // boom-bap patterns, often swung
    structural: "consistent pattern, may simplify in verse"

  trap:
    subdivision: sixteenth (808 rolls) / quarter (single hits)
    fill: [0.15, 0.70]     // huge range — sparse single hits to 808 rolls
    structural: "rolls in hooks/drops, sparse in verses"

  breakcore:
    subdivision: sixteenth
    fill: [0.40, 0.75]     // dense, often mangled break samples
    structural: "pattern may change every bar or half-bar"

  rock:
    subdivision: quarter / eighth
    fill: [0.25, 0.50]     // standard kick patterns
    structural: "doubles up in choruses, sparse in verses"
```

#### SNARE / CLAP

```
spectral_home: [150, 4000]  // body at 150-400, crack/wire at 2k-4k
envelope: sharp transient + short noise tail (snare) or layered transients (clap)

genre_expectations:
  most_genres:
    subdivision: quarter (on 2 and 4)
    fill: [0.20, 0.30]     // typically 2 hits per bar in 4/4
    structural: "rolls or fills at section boundaries"

  dnb:
    subdivision: eighth / sixteenth
    fill: [0.25, 0.60]     // breakbeat patterns, heavy snare use
    structural: "snare carries the break rhythm"

  trap:
    subdivision: quarter (main) + sixteenth (rolls at phrase ends)
    fill: [0.15, 0.50]     // sparse main hits with occasional rolls
    structural: "triplet rolls in transitions"

  breakcore:
    subdivision: sixteenth / thirtysecond
    fill: [0.30, 0.60]     // can approach tonal fusion (Circle Pit)
    structural: "may be continuous or absent — rarely steady"
```

#### HI-HAT (CLOSED)

```
spectral_home: [5000, 16000]
envelope: very short transient, minimal sustain

genre_expectations:
  house_techno:
    subdivision: eighth / sixteenth
    fill: [0.50, 1.00]     // driving hats, often every offbeat eighth
    structural: "consistent, may filter or open in builds"

  trap:
    subdivision: thirtysecond (rolls) / sixteenth (standard)
    fill: [0.30, 0.90]     // signature trap hat rolls
    structural: "rolls accelerate toward section climaxes"

  hip_hop:
    subdivision: sixteenth
    fill: [0.40, 0.75]     // swung, velocity-varied
    structural: "consistent groove pattern"

  dnb:
    subdivision: sixteenth / thirtysecond
    fill: [0.50, 0.85]     // fast, driving
    structural: "may thin out in half-time sections"

  rock:
    subdivision: eighth
    fill: [0.80, 1.00]     // steady eighth note pulse
    structural: "opens to ride in choruses"

  breakcore:
    subdivision: thirtysecond
    fill: [0.50, 0.90]     // dense, often pitched or filtered
    structural: "may be absent or continuous — binary"
```

#### HI-HAT (OPEN)

```
spectral_home: [3000, 14000]  // broader, more sustain than closed
envelope: transient + 80-200ms decay tail

genre_expectations:
  most_genres:
    subdivision: eighth (offbeats)
    fill: [0.05, 0.25]     // punctuation, not continuous
    structural: "more frequent in choruses/high-energy sections"

  disco_funk:
    subdivision: eighth (every offbeat)
    fill: [0.40, 0.50]     // the disco offbeat hat IS the groove
    structural: "relentless, defines the genre"
```

---

## DEALING WITH REAL AUDIO (things that will mess you up)

### Reverb tails and room sound
A snare with 200ms of reverb tail will produce spectral energy well past its grid position. The onset detector should be looking at the **attack**, not the sustain. Spectral flux (rate of change) catches the transient; raw energy level catches the tail. Use flux for onset detection, not energy.

### Echo and delay effects
A single snare hit through a dotted-eighth delay will produce onsets at the original position AND at positions that are NOT on the grid (dotted eighth = 0.75 beats, which doesn't land on any standard subdivision). The echoed hits will typically be lower amplitude. Apply an amplitude gate relative to the element's strongest hit in the song — echoes below e.g. 40% of peak amplitude are flagged as "echo_candidate" rather than "confirmed" onset.

### Chorus/flanger/phaser on percussion
These modulate the spectral content cyclically, which can produce phantom flux peaks that look like onsets. Cross-reference against other bands — a real percussive onset will produce correlated energy changes across its full spectral home. A chorus artefact will modulate within a narrow sub-band.

### Swing and humanisation
Systematic offset from the grid on specific beats (usually offbeats). This is not error — it's feel. The `snap_error` field in the ElementGridMap captures this. If offbeat positions consistently show +10-30ms late arrival, that's swing. Report it as `swing_amount`, don't try to correct for it.

### Tempo drift
Live recordings and some deliberate electronic production drift in tempo across the song. The grid should not be one fixed BPM applied to the whole track. Re-derive the local IOI modal interval per section (or per 8-16 bars) and allow the grid to flex. Flag the drift rate.

```
TempoDrift {
  method: "fixed" | "per_section" | "continuous"
  // fixed: one BPM for whole song (most electronic music)
  // per_section: recalculated per structural section
  // continuous: sliding window IOI estimation (live recordings)

  drift_rate: float  // BPM change per minute (0 for fixed)
  confidence: float
}
```

### Transition fills
Drum fills at section boundaries deliberately break the grid — that's the point. Expect fill_percentage to spike and pattern regularity to drop in the 1-2 bars before a section change. Use the ShiftMap from Phase A to anticipate where these will be and don't flag fill deviations in those zones as errors.

---

## DEVIATION LOG

The output of the full grid comparison. Every position on the grid, for every element, gets a status. The deviations are the data.

```
DeviationLog {
  song_id:    string
  bpm:        float

  deviations: [
    {
      element:        string
      bar:            int
      beat:           float
      subdivision:    string
      type:           "unexpected_absent"    // should be there, isn't
                    | "unexpected_present"   // shouldn't be there, is
                    | "ghost"               // present but at <40% peak amplitude
                    | "displaced"           // present but >20ms off grid
                    | "fused"              // element at this position has merged with adjacent
                    | "echo_candidate"     // likely delay/echo artefact
                    | "fill"               // in a transition zone, pattern breaks expected

      amplitude:      float    // 0-1 relative to element peak
      snap_error_ms:  float    // how far off grid
      section:        string   // which structural section this falls in
      context:        string   // adjacent positions' statuses for pattern inference
    }
  ]

  // Aggregated per element
  element_summaries: [
    {
      element:               string
      total_expected:        int      // grid positions where genre says it should appear
      total_confirmed:       int      // actually present
      total_unexpected_absent: int    // missing
      total_unexpected_present: int   // extra
      fill_vs_expected:      string   // "within_range" | "sparse" | "dense" | "fused"
      regularity_score:      float    // 0-1, how close to a repeating pattern
      swing_amount_ms:       float    // systematic offset

      // The money question: WHY is it deviating?
      deviation_hypothesis:  "genre_convention"  // this is normal for the genre
                           | "structural_variation" // section-dependent (e.g. chorus doubles)
                           | "artistic_choice"  // deliberate pattern break
                           | "production_artefact" // echo, bleed, processing
                           | "possible_error"  // unclear intent
                           | "fusion_event"    // element has crossed stupid ceiling
      // Note: hypothesis is generated, not ground truth. Requires web/somatic validation.
    }
  ]
}
```

---

## INTEGRATION WITH EXISTING ARCHITECTURE

### Feeds from:
- **Phase A scout pass** — initial tempo estimate
- **SpectralRoster** — which percussive roles to look for and where
- **BandPresenceMap / ShiftMap** — section boundaries, where to expect fills/transitions
- **Genre fingerprint** — expected fill ranges, subdivisions, structural variation
- **Web engine** — production credits, confirmed instrumentation, BPM confirmation

### Feeds into:
- **Binary engine** — onset timestamps, per-band IOIs, grid-aligned element positions
- **Cultural engine** — deviation log for convention violation detection
- **Feltness module** — gesture analysis (onset slope, sustain, offset per element per hit)
- **Interpretive Engine** — pattern deviations as potential bridge-type signals
- **Activation layer** — fill percentages and deviation types as scorable axes

### Relationship to SpectralRoster roles:
The three percussive roles in SpectralRoster (percussive-low, percussive-mid, percussive-high) map to element profiles here:

```
percussive-low  → kick
percussive-mid  → snare, clap, rim
percussive-high → hat_closed, hat_open, crash, ride, shaker
```

This spec refines what SpectralRoster can say about percussive roles by adding temporal structure. SpectralRoster says "there is a percussive-low element at 30-150Hz." This spec says "it hits at positions 1, 3, 5, 7 of every bar at eighth-note resolution, with 43% fill, 12ms swing, and it drops out in the bridge."

---

## OPEN QUESTIONS

1. **Polyrhythm.** Some music has percussive elements operating on different grid subdivisions simultaneously (3 against 4, etc.). The cross-validation step (Step 2) will find that modal IOIs from different elements don't share a clean integer ratio. How to handle: flag as polymetric, report each element's grid independently, note the ratio.

2. **Non-4/4 time.** The grid subdivision names assume 4/4. Waltz, 6/8, 5/4, 7/8 — the grid needs to accommodate non-standard meters. The IOI derivation handles this naturally (the intervals are what they are), but the "quarter / eighth / sixteenth" labelling breaks. Consider: label by ratio to bar length instead of by name.

3. **Sample-chopped breaks.** Breakcore and jungle chop and rearrange breakbeats. The original break has a grid; the chopped version may not. Individual hits land at positions that reference the original break's feel while violating the song's own grid. This may look like "unexpected_present" everywhere when it's actually a deliberate resequencing.

4. **Ghost notes and velocity layers.** The amplitude gate for echo rejection might also reject intentional ghost notes (soft hits that are part of the groove). Need a way to distinguish "quiet but deliberate" from "artefact." Ghost notes tend to fall ON grid positions; echoes fall between them.

5. **Continuous percussion (shakers, tambourines, maracas).** These don't have discrete onsets in the same way — they're textural, filling space between beats. They may register as 100% fill at very high subdivisions, which is normal, not fused. Need a "continuous_texture" element type distinct from discrete hits.

---

*Draft: 11 February 2026*
*Status: Spec draft. Needs validation against at least one real song before promotion.*
