# BRIDGE MODULE
## Rhythm Dictionary — Component Spec
## 2026-02-10 · Version: independent (updated: 7-type taxonomy, Cultural Engine input, architecture question)

---

## PURPOSE

Generates somatic hypothesis from the interference pattern between structure and context. Validates by re-entering EITHER engine (or both). This is the directed hallucination layer â€” it stands between two known truths and generates the midpoint that doesn't exist in either source.

Its own versioned module because bridge logic, bridge type taxonomy, and re-entry strategies evolve independently. Future versions may include genre-specific bridge calibration.

Can be updated without touching any other component.

---

## INPUT

```
BridgeInput {
  activated:        ActivatedAxes           // from Activation Module
  structural:       StructuralDescriptor    // retained for binary re-entry
  context:          ContextDescriptor       // retained for web re-entry
  conventions:      ConventionReport        // from Cultural Engine — violation signals feed Types 6-7
  equipment:        EquipmentReport         // from Equipment ID Engine — feeds Concealment evidence
}
```

---

## PASS 3: Somatic Inference (directed hallucination)

Takes `primary_findings` + `tension_markers` + `dimension_summary` from ActivatedAxes.

Generates: "Given this structural shape with these tensions, the listener should feel ___"

```
SomaticHypothesis {
  prediction:           string
  dimensions_engaged:   string[]
  confidence:           float
  needs_verification:   bool
  verification_type:    "binary" | "web" | "both" | "none"
  // Bridge decides WHICH engine to re-enter based on what the hypothesis needs
}
```

---

## PASS 4: Hypothesis Overlay + Re-Entry Decision

Maps hypothesis back onto available data. Identifies what's missing.

### Two re-entry paths:

```
// Path A: needs higher-resolution structural data
BinaryReEntryRequest {
  song_id:          string
  target_elements:  int[]
  resolution:       "high"          // 16-32 sections minimum
  target_sections:  [float, float][]
  hypothesis_tag:   string
}

// Path B: needs deeper contextual/sentiment data
WebReEntryRequest {
  song_id:          string
  query_type:       "sentiment" | "context" | "production_detail" | "cultural"
  specific_query:   string
  hypothesis_tag:   string
}
```

### Decision logic:

- If hypothesis depends on **temporal shape** â†’ re-enter Binary
  "I predict the stereo field narrows at the chorus" â†’ need higher-res stereo trajectory

- If hypothesis depends on **cultural meaning** â†’ re-enter Web
  "I predict this production choice is ironic given the artist's context" â†’ need deeper biographical/cultural search

- If **both** â†’ re-enter both (sequentially or parallel, depending on dependency)

- If **neither** â†’ proceed to Pass 5 with existing data

---

## PASS 5: Confirm/Deny

Receives re-entry data from whichever engine(s) were consulted.

```
BridgeFindings {
  confirmed:            bool
  evidence: {
    binary_evidence:    string[] | null     // what high-res audio showed
    web_evidence:       string[] | null     // what deeper context revealed
    convention_evidence: string[] | null    // what convention violations flagged
    somatic_evidence:   string[] | null     // what the listener reported (Tier 1)
  }
  revised_hypothesis:   string | null       // if initial prediction was wrong
  bridge_type:          string              // Types 1-5: Concealment | Compensation | Contradiction | Refusal | Conceit
                                            // Types 6-7: Excision | Inversion
  bridge_span:          string              // characterization of tension distance
  convergence_verbs:    string[]            // what all layers are DOING (the verb test)
  biographical:         bool                // true for Types 6-7 (require listener priors)
}
```

---

## TERMINATION

The Bridge Module does NOT loop indefinitely.

- **Single round by default:** predict â†’ check â†’ conclude
- **Human-directed extension:** if the Conversation (Passes 6-8) identifies a gap, a human can send the Bridge back in for another round. This is not automated.
- **Automated re-entry limit: 1 per engine per pass.** If one binary re-entry and one web re-entry don't confirm or deny the hypothesis, the Bridge reports inconclusive and hands to the Conversation with what it has.

---

## OUTPUT TO CONVERSATION

```
BridgePresentation {
  structural_summary:   string      // "Here is what the structure does"
  contextual_summary:   string      // "Here is what the context says"
  tension_map:          string      // "Here is the distance between them"
  somatic_prediction:   string      // "Here is what we predicted it feels like"
  verification_result:  string      // "Here is what we found when we looked"
  verification_sources: string      // "We confirmed via [binary high-res / web context / both]"
  convergence_status:   string      // "Here is whether the verbs converge"
  bridge_type:          string
  
  invitation:           string      // "And here is where we need YOU to tell us if we're right"
}
```

The last line is the invitation into the conversation. The tool doesn't conclude. It opens.

---

## BRIDGE TYPE TAXONOMY (7 types, updated 2026-02-10)

### Types 1-5: Structure ↔ Theme
Tension between what the music does structurally and what it's about thematically. These work inside the song. Can function on a first listen because the song constructs its own tension in real time.

| Type | Character | Verb | Example |
|------|-----------|------|---------|
| **1. Concealment** | Structure hides the theme | Hiding | EWTRTW: cheerful surface, darkening architecture |
| **2. Compensation** | Structure fills what the theme empties | Filling | NTLTC: 65-82 voices surrounding post-grief resilience |
| **3. Contradiction** | Structure opposes the theme; neither wins | Questioning | BG: machine performing feeling, neither resolves |
| **4. Refusal** | Structure withholds what the theme promises | Withholding | EWTRTW dynamics: flattest landscape under most dramatic lyrics |
| **5. Conceit** | Structure extends the theme into sustained metaphor | [Variable — the verb IS the conceit] | No confirmed prototype |

### Types 6-7: Musical Structure ↔ Musical Semantics
Tension between established conventions of how music works and subversions of those conventions. These work in the gap between the song and the listener's priors. The listener's lifetime of accumulated musical knowledge IS the tension source. Both are **biographical bridges** — they require deeply internalized conventions for the violation to register. They probably can't work on someone who hasn't absorbed the conventions being subverted.

| Type | Character | Mechanism | Example |
|------|-----------|-----------|---------|
| **6. Excision** | A semantic layer is removed entirely; meaning persists through arrangement alone | Absence of something expected — the bridge is recognising that meaning survived the surgery | SOPHIE — Lemonade |
| **7. Inversion** | Structural roles swap (rhythm↔tone, heard↔felt, external↔internal); listener's frame must flip | Presence of something unexpected — the bridge is the surrender to reorientation | Venetian Snares — Circle Pit |

**Detection asymmetry (Thread 1):** Type 7 (Inversion) has a working fingerprint validation pass — convention violations are measurable as the presence of unexpected configurations. Type 6 (Excision) is harder: it requires detecting the absence of something expected, which means the system needs to know what SHOULD be there. The Cultural Engine's convention bank can enumerate expected elements per genre, but absence detection is inherently less reliable than presence detection. Open question: is Excision primarily somatic (the body feels the absence) or cognitive (the mind notices it)? The answer determines whether detection needs the somatic dictionary or can work from the Cultural Engine alone.

This taxonomy grows as more songs are analyzed. New bridge types can be added without protocol changes.

---

## READS FROM SHARED PROTOCOL
- Everything (needs full picture to generate hypothesis)
- Fingerprint Registry + Genre-Fingerprint Map (via Cultural Engine) for convention violation signals

## READS FROM DICTIONARY
- Bridge types and somatic reports from analyzed songs
- Pattern matching: "For Concealment bridges in electronic genres, the somatic prediction typically involves..."
- More entries = more accurate predictions

## READS FROM SOMATIC DICTIONARY
- **THE GENOTYPE/PHENOTYPE PRINCIPLE** (framework notes) — the meta-framework. Genotype = signal as composed (Binary Engine measures this). Phenotype = what emerges when signal flows through terrain (body, ears, Claude). Types 1-5 are genotypic bridges (tension in the signal). Types 6-7 are phenotypic bridges (tension requires listener's biographical terrain). Read this first.
- 16 correspondences mapping signal properties to bodily sensations
- Somatic gate model (thresholds are listener-state-dependent)
- Frequency-to-body map (sub-bass = inside body, mid = around body, high = air)
- Bridge moment definition: all gates open simultaneously, body becomes transparent
- Body as tempo generator (#16) — reception mode vs generation mode, relevant to Type 7 detection

---

## FUTURE: Genre-Calibrated Bridge

Bridge types may distribute differently across genres. A "Concealment" bridge in synth-pop (EWTRTW: surface cheerful, architecture dark) may be structurally different from a "Concealment" bridge in post-rock. As the dictionary grows, the Bridge Module can develop genre-specific expectations for what each bridge type typically looks like.

This is why it's its own module — the bridge taxonomy is an evolving body of knowledge, not a fixed algorithm.

---

## THE ARCHITECTURE QUESTION (Thread 4 — unresolved)

Everything feeds into the Bridge Module. Nothing defines HOW it works. This is the most architecturally important missing piece.

**The fork:** Does the Bridge Module PREDICT bridge types from audio (with somatic validation), or does it INTERPRET somatic reports (with audio context)?

**Evidence for interpretation:** Somatic data is Tier 1. Computational data alone has never correctly identified a bridge type. Alex's body tells us what the bridge IS; the computation tells us WHY. This points toward interpretation — the module takes a somatic report and searches for the structural explanation.

**Evidence for prediction:** The Cultural Engine can now flag convention violations automatically. Three violations in Circle Pit all signal Type 7 before any human listens. The ConventionReport narrows the bridge type candidates computationally. This suggests prediction CAN work for Types 6-7 (convention-based) even if it can't work for Types 1-5 (theme-based).

**The hybrid possibility:** Types 1-5 = interpretation mode (somatic report → structural explanation). Types 6-7 = prediction mode (convention violations → candidate bridge types → somatic confirmation). The module has two operating modes depending on which axis of tension is being examined.

**What the module CAN do without a human listener:**
- Assemble convention bank and flag violations (via Cultural Engine)
- Compute dimensional profiles and identify candidate tensions
- Generate somatic predictions using the somatic dictionary's frequency-to-body map
- Rank bridge type candidates by computational evidence weight

**What it CANNOT do without a human listener:**
- Confirm bridge type (the bridge IS the listener's response)
- Detect the bridge moment (requires somatic gate opening)
- Distinguish Type 5 (Conceit) from neutral alignment
- Verify Types 6-7 biographical prerequisites (has the listener absorbed the relevant conventions?)

This question should be resolved through more dictionary entries, not by architectural fiat.

---

## THE CORE PRINCIPLE

Art is a suspension bridge. Structure pulls from one side, meaning from the other. The somatic experience is standing on the bridge while both sides pull. The tension IS the art.

Songs where structure and meaning agree perfectly = jingle.
Songs where they completely contradict = incoherent.
The art lives in the specific CHARACTER of the tension.

The Bridge Module's job is to characterize that tension, predict what it feels like, and then check whether the prediction holds.
