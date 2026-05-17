# PHASE A REVISED: THE CHEAP PASS SEQUENCE
## Rhythm Dictionary â€” Spec Draft
## 2026-02-08

---

## WHAT CHANGED

The original Phase A was: 10-second snapshot â†’ genre hypothesis â†’ web confirms â†’ commit genre â†’ proceed to full binary.

The revised Phase A adds three new micro-steps between genre commitment and full binary. These steps are computationally trivial but produce a **structural skeleton** that transforms the full pass from a uniform sweep into a targeted investigation.

New concept introduced: **Spectral Roster** â€” a predicted frequency occupation map built from genre conventions + web-sourced instrumentation data. Used for three purposes at three stages:
1. Genre refinement (cheap pass)
2. False positive prevention (full binary)
3. Artistic deviation flagging (bridge)

---

## REVISED SEQUENCE

```
Step 1: SNAPSHOT (unchanged)
  10-second clip â†’ 15 discriminators â†’ genre hypothesis
  Cost: 1 FFT window, negligible

Step 2: WEB GENRE + CONTEXT SEED (parallel, expanded)
  Genre confirmation (unchanged)
  NEW: Instrument roster retrieval
  NEW: Song structure retrieval (section labels + approximate timestamps)
  Cost: web scrape (already happening, just extracting more)

Step 3: GENRE COMMIT (unchanged)
  Binary hypothesis + web confirmation â†’ committed genre
  Loads genre baseline from Shared Protocol

Step 4: SPECTRAL ROSTER BUILD (NEW)
  Genre conventions + web instrument data â†’ predicted frequency occupation map
  Output: SpectralRoster â€” expected frequency bands per instrument source
  Cost: lookup table, no audio processing

Step 5: ROSTER CONFIDENCE CHECK (NEW)
  Compare snapshot spectral data against SpectralRoster
  Flag mismatches that might indicate wrong genre or unusual instrumentation
  If severe mismatch: loop back to Step 2 with refined search
  Cost: comparison against existing snapshot data, zero new audio processing

Step 6: STRUCTURAL SAMPLING (NEW)
  If web provided section boundaries: sample 1-2 seconds from each named section
  If no section data: sample 5 equidistant 1-second slices across song duration
  For each slice: check presence/absence of SpectralRoster bands only
  Output: BandPresenceMap â€” per-section roster of which instruments appear where
  Cost: 5-8 targeted FFTs, filtered to roster bands only

Step 7: SHIFT POINT IDENTIFICATION (NEW)
  Compare BandPresenceMap across sections
  Where band presence changes (appears/disappears/changes intensity): flag as shift point
  Output: ShiftMap â€” timestamp ranges where arrangement changes occur
  Cost: comparison logic only, no audio processing

Step 8: FULL BINARY PASS (modified)
  Now receives: committed genre + SpectralRoster + BandPresenceMap + ShiftMap
  Shift points: analyzed at high resolution (16-32 sections within the transition zone)
  Stable sections: analyzed at standard resolution (8 sections)
  Roster-informed: measurements that depend on source attribution use roster to avoid false positives
  Output: StructuralDescriptor (same schema, better data)
```

---

## NEW DATA STRUCTURES

### SpectralRoster

```
SpectralRoster {
  song_id:          string
  genre_id:         string
  source:           "genre_convention" | "web_confirmed" | "hybrid"

  roles: [
    {
      role:             string        // "percussive-low" | "percussive-mid" | "percussive-high" |
                                      // "sustained-low" | "sustained-mid" | "lead-melodic" |
                                      // "pad-harmonic" | "vocal"
      spectral_home:    [float, float]  // Hz range (default from taxonomy, adjusted by web data)
      envelope:         "transient" | "sustained" | "plucked" | "pad" | "formant"
      web_instrument:   string | null   // what web called it: "Moog bass", "falsetto", etc.
      confidence:       float           // 0.6-0.8 genre default, 1.0 web confirmed
      density_hint:     "sparse" | "moderate" | "dense"  // from web: "layered harmonies" â†’ dense
    }
  ]

  // Derived from roles list
  band_expectations: [
    {
      band:           string        // "sub_bass" | "bass" | "low_mid" | "mid" | "high_mid" | "presence" | "brilliance"
      hz_range:       [float, float]
      expected_roles: string[]      // which roles should occupy this band
      expected_density: "empty" | "sparse" | "moderate" | "dense" | "saturated"
    }
  ]
}
```

### BandPresenceMap

```
BandPresenceMap {
  song_id:          string
  sample_method:    "web_structural" | "equidistant"
  
  sections: [
    {
      label:          string        // "verse_1", "chorus_1", "bridge", or "slice_1", "slice_2", etc.
      timestamp:      [float, float] // start, end in seconds
      sample_point:   float         // where within section the slice was taken
      
      bands: [
        {
          band:         string      // matches SpectralRoster band names
          present:      bool        // is there significant energy here?
          intensity:    float       // relative to song mean (0-1 normalized)
          roster_match: "expected" | "unexpected_present" | "unexpected_absent"
          // expected = roster predicted this role here, and it's here (or predicted absent, and absent)
          // unexpected_present = no role predicted for this band, but energy present
          // unexpected_absent = roster predicted a role here, but band is empty
        }
      ]
    }
  ]
}
```

### ShiftMap

```
ShiftMap {
  song_id:          string
  
  shift_points: [
    {
      timestamp:      float         // approximate time of change
      between_sections: [string, string]  // e.g. ["verse_1", "chorus_1"]
      roles_entering:  string[]     // roles that appear
      roles_exiting:   string[]     // roles that disappear
      roles_changing:  string[]     // roles that shift intensity significantly
      severity:        float        // how much total spectral change (0-1)
      
      // Analysis targeting instruction
      recommended_resolution: "high" | "standard"
      recommended_window:     [float, float]  // time range to analyze at high res
      target_roles:           string[]        // which roles to focus on at this shift
    }
  ]
  
  // Overall structural shape
  arrangement_type: "static" | "additive" | "subtractive" | "sectional" | "through-composed"
  // static: same roles throughout
  // additive: roles enter progressively
  // subtractive: roles exit progressively  
  // sectional: distinct blocks with different role sets
  // through-composed: continuous change, no stable blocks

  legibility_estimate: float        // 0-1, derived from production transparency + roster match rate
}
```

### RoleTrajectory (derived from BandPresenceMap)

```
RoleTrajectory {
  song_id:          string
  
  trajectories: [
    {
      role:           string        // one of the 8 role types
      per_section: [
        {
          label:      string        // "verse_1", "chorus_1", "bridge", etc.
          present:    bool
          intensity:  float         // 0-1 relative to song peak for this role
        }
      ]
    }
  ]
  
  // Pre-computed dimensional hints (coarse, for silhouette pass)
  dimensional_hints: [
    {
      dimension:    string          // one of 10 meta-dimensions
      hint:         float           // coarse reading, -1 to +1
      derived_from: string          // which role trajectories contributed
      confidence:   float           // low â€” these are guesses, not measurements
    }
  ]
}
```

---

## SPECTRAL ROSTER: ROLE-BASED TAXONOMY

### Core principle: roles, not instruments

The roster doesn't catalog instruments. It maps **mix roles** â€” functional positions in the arrangement defined by spectral home and envelope shape. A Moog bass synth and a fingerpicked bass guitar both occupy the "sustained-low" role. The instrument identity is web trivia; the role is structural data.

This dissolves the "electronic music problem" â€” genres where every sound is synthesized still have sounds occupying predictable roles, because those roles are driven by psychoacoustic necessity (every mix needs something anchoring low end, something carrying rhythm, something carrying melody). The instruments change. The roles don't.

### Role taxonomy (8 roles):

| Role | Spectral home | Envelope signature | Example sources |
|------|--------------|-------------------|-----------------|
| **percussive-low** | 30-150Hz | transient | kick drum, 808, bass drop hit |
| **percussive-mid** | 150Hz-2kHz | transient | snare, clap, toms |
| **percussive-high** | 2kHz+ | transient | hi-hat, cymbal, shaker |
| **sustained-low** | 30-250Hz | sustained/plucked | bass guitar, bass synth, sub |
| **sustained-mid** | 250Hz-2kHz | sustained/plucked | rhythm guitar, organ, mid synth |
| **lead-melodic** | 200Hz-6kHz | melodic contour | lead guitar, lead synth, flute |
| **pad-harmonic** | 200Hz-8kHz | sustained, wide | string pad, synth pad, choir |
| **vocal** | 100Hz-8kHz | formant structure | lead vocal, backing vocals |

Most songs use 4-6 of these. The roster predicts which roles are present and the binary confirms.

### Web â†’ Role mapping:

Web content maps to roles trivially. You only need to know two things about any instrument: is it percussive or sustained, and is it low, mid, or high.

```
"Moog bass synth"       â†’ sustained-low
"808 kick"              â†’ percussive-low
"Distorted guitar"      â†’ sustained-mid OR lead-melodic (binary resolves by checking envelope)
"Falsetto vocals"       â†’ vocal (shifted fundamental: 200-600Hz instead of 100-400Hz)
"String section"        â†’ pad-harmonic
"Drum machine"          â†’ percussive-low + percussive-mid + percussive-high
"Piano"                 â†’ sustained-mid OR lead-melodic (depends on arrangement role)
```

Ambiguous mappings (guitar could be rhythm or lead) are fine â€” the binary resolves them by checking whether the energy in that range has melodic contour or sustained chord shape.

### Genre â†’ Role mapping (when web data is sparse):

Each genre fingerprint in Shared Protocol gets an associated **default role set**:

```
genre_role_defaults: {
  "rock":       ["percussive-low", "percussive-mid", "percussive-high", "sustained-low", "sustained-mid", "vocal"],
  "synth_pop":  ["percussive-low", "percussive-mid", "sustained-low", "pad-harmonic", "lead-melodic", "vocal"],
  "ambient":    ["pad-harmonic", "sustained-low"],
  "jazz":       ["percussive-low", "percussive-mid", "percussive-high", "sustained-low", "lead-melodic", "vocal"],
  "noise":      ["sustained-low", "sustained-mid", "pad-harmonic"],  // roles collapse in noise
  // ... etc for all 20 genres
}
```

### Confidence layering:

- **Genre default role**: confidence 0.6-0.8 (depends on how canonical for genre)
- **Web-mentioned instrument mapped to role**: confidence 1.0
- **Web says "stripped-back arrangement with just piano and voice"**: remove defaults, assign sustained-mid + vocal only, confidence 1.0
- **Web says "layered vocal harmonies"**: vocal role flagged as "dense" rather than "sparse"

---

## HOW SPECTRAL ROSTER FEEDS THE FULL PASS

### False positive prevention (binary engine internal):

Before computing any element that implicitly assumes source identity, the binary engine checks the roster:

```
Example: Element #24 (onset density / transient count)
  Without roster: counts ALL transients across full spectrum â†’ inflated by synth oscillations
  With roster: counts transients only in percussion-expected bands â†’ accurate percussion density
  
Example: Element #27-28 (decay time / A/D ratio)  
  Without roster: measures decay of composite signal â†’ physically impossible readings
  With roster: measures decay in bands where transient sources are expected â†’ plausible readings
  
Example: Element #5-6 (RMS energy / dynamic range)
  Without roster: global RMS includes everything â†’ describes mastering, not composition
  With roster: per-band RMS with source attribution â†’ can distinguish "bass is compressed" from "everything is compressed"
```

### Roster deviation as data (passed to activation layer):

```
RosterDeviation {
  element_id:       int
  expected_source:  string          // what the roster predicted
  observed_profile: string          // what the binary actually saw
  deviation_type:   "spectral_disguise" | "identity_erasure" | "spectral_fusion" | 
                    "register_invasion" | "unexpected_absence" | "phantom_source"
  severity:         float           // how different from expectation
  
  // NOT interpreted here. Passed through to activation layer as structural data.
  // Activation layer + bridge decide if this is artistically meaningful.
}
```

### Deviation type definitions:

| Type | Description | Example |
|------|-------------|---------|
| **spectral_disguise** | A role occupies frequency space typical of a different role | sustained-low source with percussive-low transient envelope |
| **identity_erasure** | A role is processed until its expected spectral signature is unrecognizable | vocal processed into pad-harmonic territory |
| **spectral_fusion** | Two roles deliberately occupy identical frequency space, inseparable | sustained-low and percussive-low merged into single low-end entity |
| **register_invasion** | A role operates outside its conventional spectral home | sustained-mid content high-passed to sit entirely in lead-melodic territory |
| **unexpected_absence** | Genre roster predicts a role, but its expected band is empty | Rock song with no sustained-mid (guitar) frequency content |
| **phantom_source** | Significant energy in a band with no roster-predicted role | Unexplained mid-range presence in a vocal + percussive-only arrangement |

---

## HOW STRUCTURAL SAMPLING FEEDS THE FULL PASS

### Role tracking: following each character through the story

The BandPresenceMap doesn't just identify shift points â€” it produces a **per-role trajectory** across the song's sections. Each role gets a presence timeline:

```
RoleTrajectory {
  role:             string
  per_section: [
    {
      label:        string          // "verse_1", "chorus_1", "bridge", etc.
      present:      bool
      intensity:    float           // 0-1 relative to song peak for this role
    }
  ]
}
```

Example output for a pop song:
```
sustained-low:   [verse: 0.8] [chorus: 1.0] [bridge: 0.0] [chorus: 1.0] [outro: 0.6]
percussive-mid:  [verse: 0.5] [chorus: 1.0] [bridge: 0.3] [chorus: 1.0] [outro: 0.4]
vocal:           [verse: 0.7] [chorus: 1.0] [bridge: 1.0] [chorus: 1.0] [outro: 0.0]
pad-harmonic:    [verse: 0.0] [chorus: 0.6] [bridge: 1.0] [chorus: 0.6] [outro: 1.0]
```

This is already readable as an arrangement story: bass drops out in the bridge (tension), percussion thins (space), vocal sustains (exposure), pad enters to fill the gap (compensation). That's four findings from a handful of FFTs.

### Role trajectories pre-compute dimensional readings

The composite of all role trajectories maps almost directly onto meta-dimensions:

- **WEIGHT**: sustained-low + percussive-low intensity across sections
- **DENSITY**: total number of active roles per section
- **ENERGY**: percussive roles intensity + onset rate
- **CONTINUITY**: how stable are role trajectories across sections (static vs changing)
- **SCALE**: total spectral spread (how many bands occupied simultaneously)
- **CONSTRAINT**: inverse of spectral freedom (are roles confined to narrow bands or spread wide)

These aren't precise dimensional scores â€” they're coarse readings that the silhouette pass can use immediately. A song where DENSITY is obviously high from role count doesn't need expensive element-level computation to confirm that DENSITY is convergent with genre expectation.

### Resolution targeting:

```
// From ShiftMap
shift_points: [
  { timestamp: 45.2, severity: 0.8, recommended_resolution: "high", window: [42.0, 48.0] },
  { timestamp: 112.7, severity: 0.3, recommended_resolution: "standard" }
]

// Full pass behavior:
// Sections 42.0-48.0: analyze at 32 sub-sections (high resolution)
// Sections 0-42.0: analyze at 8 sub-sections (standard)
// Sections 48.0-112.7: analyze at 8 sub-sections (standard)
// etc.
```

Role trajectories also tell the full pass *what to measure* at each shift point. If sustained-low disappears at 45.2s, the high-resolution analysis at that transition focuses on low-frequency behavior â€” filter sweep, fade, hard cut, replacement by another role. If percussive-high enters at 112.7s, the analysis focuses on transient characteristics in the high band. The full pass isn't just targeted *temporally*, it's targeted *spectrally*.

### Arrangement type as silhouette input:

The ShiftMap's `arrangement_type` feeds directly into the silhouette pass. A "static" arrangement means CONTINUITY dimension is likely convergent (genre water for many genres). An "additive" arrangement is a strong signal for the DENSITY and WEIGHT dimensions. "Through-composed" suggests the song resists section-based analysis and needs trajectory-primary measurement.

This is structural metadata arriving before the expensive analysis even starts.

---

## LEGIBILITY ESTIMATE

Computed from:
- **Roster match rate**: What percentage of observed spectral energy is attributable to roster sources? High match = high legibility.
- **Band collision rate**: How many roster bands overlap with other roster bands? High collision = low legibility (superposition problem).
- **Production transparency** (from web): Electronic/hybrid production â†’ lower legibility estimate. Live/acoustic â†’ higher.
- **Arrangement density** (from BandPresenceMap): More simultaneous bands in any section â†’ lower legibility.

```
LegibilityEstimate {
  overall:          float           // 0-1
  per_band:         { [band: string]: float }  // some bands more legible than others
  confidence_impact: string         // "binary readings highly reliable" | "binary readings moderately reliable" | "binary readings require web cross-reference"
  
  // Used by activation layer to weight binary vs web data
  // Low legibility â†’ web engine findings get higher weight in conflicts
  // High legibility â†’ binary engine findings get higher weight
}
```

---

## COMPUTE BUDGET COMPARISON

### Original Phase A:
- 1 FFT snapshot (10 seconds)
- Web scrape
- Genre commit
- Full binary pass: uniform 8-section sweep of all 55 elements
- **Total: ~55 Ã— 8 = 440 element-section computations**

### Revised Phase A:
- 1 FFT snapshot (10 seconds) â€” same
- Web scrape (slightly expanded extraction) â€” marginal cost
- Genre commit â€” same
- Roster build â€” lookup table, zero audio cost
- Roster confidence check â€” comparison, zero audio cost
- Structural sampling: 5-8 targeted FFTs Ã— 4-5 bands = **20-40 band checks**
- Shift map: comparison logic only
- Full binary pass: high-res at shift points + standard elsewhere
  - Example: 2 shift zones Ã— 32 sections + 6 stable zones Ã— 8 sections = **112 section computations**
  - But only on elements relevant to each zone's roster profile
  - Estimated: ~30 elements per zone average = **~3,360 element-section computations at shift points, ~1,440 elsewhere**
  - Net: similar total compute but **concentrated where it matters**

### Real gain:
Not fewer computations, but **better computations**. Every measurement knows what it's measuring, what band it should focus on, and whether the result is likely legible. The same total compute produces dramatically less noise.

---

## INTEGRATION WITH EXISTING ARCHITECTURE

### What changes:
- **Binary Engine input**: gains `SpectralRoster`, `ShiftMap`, and `RoleTrajectory` as optional fields for "full" mode
- **Binary Engine output**: `StructuralDescriptor` gains `roster_deviations: RosterDeviation[]` and `legibility: LegibilityEstimate`
- **Web Engine output**: `ContextDescriptor` gains `instrument_mentions: string[]` and `structure_sections: SectionLabel[]`
- **Shared Protocol**: Genre fingerprints gain associated `default_role_set` (8 role types, not per-instrument)
- **Silhouette Pass input**: gains `RoleTrajectory.dimensional_hints` as optional coarse pre-readings

### What doesn't change:
- Activation layer schema (roster deviations arrive as structural data, processed like any other element)
- Bridge module (reads roster deviations from activated axes, interprets them)
- Meta-dimensions (no new dimensions needed)
- Element registry (no new elements â€” roster deviations are metadata about existing elements)

### Breaking vs non-breaking:
- Adding `roster_deviations` to StructuralDescriptor: **non-breaking** (additive field, old activation layers ignore it)
- Adding `default_instrument_roster` to genre fingerprints: **non-breaking** (new data in Shared Protocol, engines opt in)
- The entire spectral roster system can be built incrementally â€” start with genre defaults only, add web-informed upgrades later

---

## THE THREE-LAYER CHEAP PASS

Summary of what the revised Phase A produces before the full binary pass begins:

**Layer 1 â€” SNAPSHOT:** "This is synth-pop at 112 BPM."
Genre commitment, baseline loaded. Cost: 1 FFT.

**Layer 2 â€” ROSTER ROLL CALL:** "It has five roles â€” percussive-low, percussive-mid, sustained-low, lead-melodic, vocal â€” all present, here's where each lives spectrally."
Cast confirmed. False positive prevention enabled. Cost: web extraction + band-filtered FFTs.

**Layer 3 â€” ROLE TRACKING:** "Here's how those five roles move across the song's sections â€” bass drops out in the bridge, percussion doubles in the chorus, vocal is constant throughout."
Structural skeleton established. Shift points identified. Dimensional pre-readings available. Full pass now colors in a drawing that already exists. Cost: 5-8 section-targeted FFTs.

**Layer 4 — CULTURAL CONTEXT (parallel with Layers 2-3):** "This is breakcore. Breakcore evolved from jungle, which evolved from rave. The conventions being inherited are: drums = rhythm, bass = felt, breaks and bass as dual system. The scene context is Winnipeg/Planet Mu, tracker-based production, deliberately antagonistic to mainstream."
Convention bank assembled. Lineage mapped. Violation detection primed for post-binary pass. Cost: web search + structured assembly.
**Mechanism:** genre-fingerprint-map.md → look up genre → get fingerprint IDs → load from fingerprint-registry.md → run binary code against audio. Convention bank is a search function, not a generation task.

Total cheap pass cost: ~10 FFTs + web scrape + cultural context assembly + comparison logic.
Information yield: genre, instrumentation, arrangement shape, shift points, coarse dimensional readings, legibility estimate, convention bank, genre lineage, prior requirements.

**Post-binary addition (Cultural Engine Step 4-6):** After full binary analysis, Cultural Engine compares convention bank predictions against actual measurements. Violations flagged and typed (Excision / Inversion / other). Prior requirements mapped. Bridge type signals passed to Bridge Module. This is the step that enables Types 6 and 7.

---

## OPEN QUESTIONS

1. **Roster granularity.** Eight roles seems right for most popular music. Orchestral or dense electronic music might need sub-roles (e.g., "sustained-mid-high" for strings vs "sustained-mid-low" for brass). Add sub-roles later or keep it at eight?

2. **Structural sampling window.** Is 1 second per section enough? Filter sweeps, crossfades, and gradual builds take longer. Some transitions need 2-4 second windows. Should window size scale with shift severity?

3. **Roster evolution within song.** The current spec treats the roster as global â€” same roles expected throughout. But some songs introduce entirely new roles mid-song (synth solo entering in the bridge that wasn't in the verse). Should the roster be section-aware from the start, or discover new roles during role tracking?

4. **Confidence floor.** At what roster confidence level does false positive prevention start helping more than it misleads? A genre-only roster (confidence 0.6-0.8) might wrongly filter out legitimate data. Should roster-informed measurement adjustment only activate above confidence 0.9 (i.e., web-confirmed)?

5. **Role collision handling.** When two roles share the same spectral home (sustained-low and percussive-low both live in 30-150Hz), the binary can distinguish them by envelope. But what if the production deliberately blurs the envelope (heavily compressed kick that sustains like a bass)? That's a spectral fusion finding â€” but the roster needs to not *break* when it encounters it.
