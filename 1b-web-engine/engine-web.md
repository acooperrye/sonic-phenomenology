# WEB ENGINE
## Sonic Phenomenology — Component Spec
## Consolidated 2026-05-17 · Version: independent

---

## PURPOSE

Retrieves and structures cultural context for a song. Produces inert descriptors — facts without structural grounding — that the Activation Layer uses to weight, sign, and filter the Binary Engine's structural measurements.

The Binary Engine produces 216 unweighted, unsigned axis readings. Without Web Engine context, those readings sit at default weight 1.0 and default sign POSITIVE — a cloud of potential meanings with no way to select which ones are real for this specific song. **The structural measurements are inert. The web context activates them.**

Operates in two phases (genre confirmation, full context) with an optional re-entry interface for Bridge Module queries. Versioned independently of any other engine — search targets, extraction logic, and source rosters can be updated without protocol changes.

---

## THE WEB ENGINE'S ROLE IN THE PIPELINE

Pass 2 retrieves exactly three things, and each filter in the Activation Layer depends on exactly one of them:

| Web retrieves | Activation filter that uses it | What breaks without it |
|---|---|---|
| **Genre classification** (Wikipedia, RYM, AllMusic, Discogs) | Filter 1: Genre Markedness | Can't identify marked vs unmarked axes — every measurement gets default weight |
| **Thematic vector** (Genius, Wikipedia, critical reviews) | Filter 2: Thematic Alignment | Can't determine sign — every axis defaults positive, no bridge tension surfaces |
| **Production method** (credits, studio, technique) | Filter 3: Production Attribution | Can't filter signal from artifact — engineering artifacts get weighted as artistic choices |

These three are the "key ring." Everything else the Web Engine fetches is in service of one of them, or is a Bridge Module re-entry response.

---

## INPUT

### Phase A (genre confirmation only):

```
WebGenreConfirmInput {
  artist:           string
  title:            string
  genre_hypothesis: GenreMatch[]    // from Binary Engine snapshot
}
```

### Phase B (full context):

```
WebFullInput {
  artist:           string
  title:            string
  committed_genre:  GenreCommitment            // from Phase A
  binary_clusters:  CoProductionCluster[] | null  // candidate clusters to confirm
  broken_elements:  int[]                      // element IDs that need web population
}
```

---

## OUTPUT

### Phase A output:

```
GenreConfirmation {
  confirmed:        bool
  corrected_genre:  string | null   // if binary was wrong
  confidence:       float
  source:           string          // URL/reference
}
```

### Phase B output:

```
ContextDescriptor {
  song_id:          string

  genre: {
    primary:        string          // committed in Phase A, carried through
    subgenre:       string | null
    era:            string
    baseline_id:    string
    sources:        string[]
  }

  thematic_vector: {
    dimensions: [
      {
        dimension:  string          // one of 10 meta-dimensions (see below)
        score:      float           // -1.0 to +1.0
        evidence:   string
        confidence: float
      }
    ]
    raw_themes:     string[]
    lyrical_summary: string
    sources:        string[]
  }

  production: {
    method:         "electronic" | "live" | "sample-based" | "hybrid" | "unknown"
    producer:       string | null
    studio:         string | null
    era_conventions: string[]
    notable_techniques: string[]
    credits:        object
    sources:        string[]
  }

  co_production_confirmations: [
    {
      cluster_id:   string
      confirmed:    bool
      evidence:     string
      lead_element: int
    }
  ]

  web_only_elements: [
    {
      element_id:   int             // 25, 49, 50, 52
      web_value:    string | float
      web_axes:     AxisReading[]
      sources:      string[]
    }
  ]

  cached:           bool
  cache_timestamp:  ISO datetime | null

  metadata: {
    engine_version: string
    timestamp:      ISO datetime
    source_count:   int
  }
}
```

---

## THE TEN META-DIMENSIONS

Every thematic finding maps onto one or two of these dimensions, scored -1.0 to +1.0. Every structural axis is tagged with the same dimensions, so Activation can compare structure to theme on the same axis.

1. **VALENCE** — bright↔dark, hopeful↔despairing, warm↔cold
   - Thematic sources: lyrical mood, emotional tone, imagery, narrative outcome

2. **ENERGY** — still↔activated, calm↔agitated, resting↔urgent
   - Thematic sources: narrative pace, emotional intensity, crisis vs peace

3. **DENSITY** — sparse↔crowded, empty↔full, alone↔surrounded
   - Thematic sources: loneliness vs community, isolation vs belonging

4. **STABILITY** — anchored↔displaced, certain↔uncertain, permanent↔impermanent
   - Thematic sources: home vs displacement, certainty vs doubt, existential ground

5. **CONSTRAINT** — free↔trapped, open↔compressed, released↔caged
   - Thematic sources: freedom vs entrapment, agency vs helplessness

6. **AGENCY** — mechanical↔human, designed↔organic, artificial↔natural
   - Thematic sources: humanity, authenticity vs artifice, technology

7. **ROUGHNESS** — polished↔raw, clean↔gritty, refined↔crude
   - Thematic sources: vulnerability, honesty, struggle vs composure

8. **CONTINUITY** — permanent↔transient, cyclical↔linear, repeating↔evolving
   - Thematic sources: change vs stasis, growth, impermanence, return vs departure

9. **SCALE** — intimate↔vast, close↔distant, personal↔universal
   - Thematic sources: personal confession vs global statement, private vs public

10. **WEIGHT** — heavy↔light, grounded↔floating, massive↔delicate
    - Thematic sources: gravity of subject, burden vs liberation, seriousness vs play

The thematic content SELECTS which structural axes become active tension points. A song with lyrics heavily scored on STABILITY produces maximum bridge tension on every stability-tagged axis and barely registers on roughness-tagged axes, even if those measurements are extreme.

---

## SEARCH TARGETS

### Phase A (genre only):
- Wikipedia (genre classification)
- RateYourMusic, AllMusic, Discogs (genre tags)

### Phase B (full context):

**Genre** (already committed — carried through for reference):
- Wikipedia, RateYourMusic, AllMusic, Discogs

**Thematic content:**
- Genius (lyrics + annotations)
- Wikipedia (song background, album context)
- Critical reviews (Pitchfork, NME, etc.)
- Artist interviews mentioning the song

**Production method:**
- Wikipedia, AllMusic credits
- Discogs (detailed credits, formats)
- Producer interviews
- Studio databases
- Era-specific production convention references

### Re-entry (Bridge-directed):

**Sentiment queries:**
- Critical reception analysis
- Fan community responses
- Commercial vs critical reception gap

**Cultural context queries:**
- Artist biographical context during recording
- Historical/cultural moment
- Genre scene context

**Production detail queries:**
- Specific equipment/technique documentation
- Studio session reports
- Engineer/mixer interviews

---

## RE-ENTRY INTERFACE (called by Bridge Module)

The Bridge Module can request deeper contextual investigation when the somatic hypothesis needs cultural/sentiment grounding that the initial web pass didn't capture.

```
WebReEntryRequest {
  song_id:          string
  query_type:       "sentiment" | "context" | "production_detail" | "cultural"
  specific_query:   string          // what the Bridge needs to know
                                    // e.g. "what was the artist's personal context during recording?"
                                    // e.g. "how was this song received critically vs commercially?"
                                    // e.g. "what specific reverb/compression chain was used?"
  hypothesis_tag:   string          // what Bridge is trying to confirm
}

WebReEntryResponse {
  query_type:       string
  findings:         string          // structured response
  thematic_update:  DimensionScore[] | null  // updated scores if sentiment query
  production_update: object | null  // updated production data if detail query
  sources:          string[]
  confidence:       float
}
```

---

## WEB-ONLY ELEMENTS (Binary Engine can't measure these)

| # | Element | Web Source |
|---|---------|------------|
| 25 | Beat micro-peaks | N/A — discard entirely |
| 49 | Vocal presence | Track listing, credits, reviews |
| 50 | Instrument ID | Production credits, liner notes, interviews |
| 52 | Reverb estimation | Studio notes, era conventions, producer interviews |

---

## CACHING

Web results for the same artist+title don't change. The Web Engine caches its ContextDescriptor after Phase B:

- If Binary Engine is updated and the same song is re-analyzed, cached ContextDescriptor reused — no repeat web scrape
- Cache can be manually invalidated if new web sources become available
- Re-entry responses are NOT cached (they're hypothesis-specific)

---

## READS FROM SHARED PROTOCOL
- Element Registry (which elements are broken/degraded and need web population)
- Genre Baseline Table (`genre-baselines.md`) — to confirm/correct binary hypothesis
- Meta-Dimension Definitions (above) — dimensions to score thematic content against
- Co-Production Cluster Templates — what to look for in production credits

## READS FROM DICTIONARY
- Existing analyses (`../dictionary/entries.md`) — reference for songs similar to the candidate
- Bridge types and tension patterns — to know what kind of context matters

## DOES NOT KNOW
- Any audio measurements
- What the waveform looks like
- Which axes are marked or unmarked

---

## OPERATIONAL REFERENCE

The full per-element scoring map — what each of the 54 elements expects from the web pass, how each filter applies, what the inverters look like — is in `web-content-axis-scoring.md` alongside this spec. That document is the lookup table the engine consults at runtime. This spec is the architecture; that document is the data.

---

## KNOWN GAPS

- **Co-production has no mechanism** in the scoring map. Elements scored independently; no way to cluster co-produced measurements as a unit.
- **Trajectory vs average not systematically marked.** Some elements need temporal shape data; the map doesn't flag which ones consistently.
- **Ten dimensions not yet fully validated.** Need to run all dictionary songs through and check that every hand-written inverter reproduces mechanically from meta-dimension comparison.
- **Production attribution is the weakest filter.** Genre and thematic alignment are bounded operations; production intent requires inferring intent from web data, with a wide "unknown" middle.
- **Internal structural contradiction** (when the structure disagrees with itself across elements) has no mechanism. Could be co-production misread or genuinely complex architecture.
- **Genre baselines are thin** — twenty fully characterised baselines across fifty-eight named genres. The reference set is growing but not yet comprehensive.
- **Thematic extraction prompt not yet finalized.** For automated tooling, need structured extraction: "Score this lyrical content from -1 to +1 on each of ten dimensions, with evidence."

---

## IMPROVEMENT ROADMAP (internal, no protocol changes needed)
- Finalize structured thematic extraction prompt
- Expand genre baseline set toward full coverage of named genres
- Improve production attribution logic (authored vs incidental detection)
- Add cultural context sources (scene databases, era timelines)
- Refine co-production confirmation logic (currently relies on finding specific production terms)

---

## THE KEY INSIGHT

The structural measurements are inert potential. The web context is the key ring that activates them — genre selects which axes are marked, thematic valence selects the sign, production method filters signal from artifact.
