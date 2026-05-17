# WEB CONTENT AXIS SCORING MAP
## How Pass 2 (Web Context) Modifies Pass 1 (Structural Measurement)
## Rhythm Dictionary â€” February 2026

---

## SCORING SYSTEM

Every element has 4 perceptual axes. Each axis has a **default weight of 1.0** and a **default sign of POSITIVE** (reads straight â€” the pole means what it says).

Pass 2 (web context) modifies each axis through three filters:

### FILTER 1: GENRE MARKEDNESS
**Question: Is this measurement notable for this genre, or is it the water?**
- If measurement falls WITHIN genre's expected range â†’ weight Ã— 0.2 (suppress â€” it's background)
- If measurement falls OUTSIDE genre's expected range â†’ weight Ã— 2.0 (amplify â€” it's a finding)
- If no genre data available â†’ weight Ã— 1.0 (unchanged)

### FILTER 2: THEMATIC ALIGNMENT
**Question: Does the meaning reinforce or contradict this structural reading?**
- REINFORCED: thematic content agrees with the pole â†’ sign stays POSITIVE (bright structure + bright meaning = bright)
- INVERTED: thematic content contradicts the pole â†’ sign flips to NEGATIVE (bright structure + dark meaning = "enforced brightness")
- NEUTRAL: thematic content is unrelated to this axis â†’ sign stays POSITIVE, weight Ã— 0.5 (the axis is real but thematically inert)

### FILTER 3: PRODUCTION ATTRIBUTION
**Question: Is this measurement an artistic choice or a technical artifact?**
- AUTHORED: measurement reflects intentional production choice â†’ weight Ã— 1.0
- INCIDENTAL: measurement reflects format, era convention, or technical limitation â†’ weight Ã— 0.1
- UNKNOWN: no production data available â†’ weight Ã— 0.7

### FINAL AXIS SCORE
`axis_score = measurement_position Ã— sign Ã— (genre_weight Ã— thematic_weight Ã— production_weight)`

Axes with final weight < 0.15 are suppressed (not reported).
Axes with final weight > 1.5 are flagged as primary findings.
Axes with NEGATIVE sign are flagged as bridge-tension markers.

---

## FIRST WEB SCRAPE TARGETS

Before any axis scoring can occur, Pass 2 must retrieve:

1. **Genre classification** â€” primary genre, subgenre, era. Targets: Wikipedia, RateYourMusic, AllMusic, Discogs.
2. **Thematic valence** â€” lyrical themes reduced to valence vectors. Targets: Genius (lyrics + annotations), Wikipedia (song background), critical reviews.
3. **Production method** â€” credits, studio, technique, mastering. Targets: Wikipedia, AllMusic credits, Discogs, producer interviews, studio databases.

These three data points are the KEY RING. Without them, all 216 axes read at default weight with unknown sign â€” which is the same as having no interpretation at all.

---

## GENRE EXPECTATION RANGES

Before the per-element scoring, the engine needs genre baselines. These are the "water" â€” measurements that are unremarkable for the genre.

### Electronic / Synth-pop / Dance-pop
- Grid: >50% (expected), Tempo: 115-145 BPM, CV: <0.15, Corr: >0.85
- Flatness max: <0.08, HNR: 3-7, Attack: <400ms
- Silence: <5%, Dynamic range: 5-10 dB
- Sym: >3, Chromatic: <8

### Post-rock / Ambient / Progressive
- Grid: <25%, Tempo: 60-100 BPM, CV: >0.20, Corr: 0.6-0.85
- Flatness max: 0.10-0.25, HNR: 1-4, Attack: >400ms
- Silence: <3%, Dynamic range: 4-8 dB
- Sym: <3, Chromatic: 5-8

### Pop / Major label / Max Martin lineage
- Grid: variable (sample-based or sequenced), Tempo: 100-130 BPM
- HNR: >7 (vocal-forward), Silence: 5-15% (ON/OFF architecture)
- Dynamic range: 8-15 dB, Poly density: >50
- Chromatic: 8-12 (layered production), Sym: >20

### Hyperpop / Experimental electronic
- Grid: >70%, Tempo: >140 BPM, CV: 0.05-0.20
- Dynamic range: >15 dB (wall-collapse), Flatness range: wide
- HNR: variable (vocal stacking peaks), Sub-bass: >35%
- Corr: variable (stereo experimentation)

### 80s Pop / New Wave / Synth-pop (era-specific)
- Grid: 30-60% (semi-quantized), Tempo: 100-140 BPM
- Dynamic range: <7 dB (loudness war precursor / heavy compression)
- Attack: <100ms (gated drums), Reverb: high (era signature)
- Corr: variable, Centroid: high (DX7 brightness)

*(More genres added as dictionary expands)*

---

## PER-ELEMENT SCORING MAP

For each element: the 4 axes, what each filter looks for, and the specific thematic dimensions that can invert each axis.

---

### #1 TEMPO (BPM)
**Category: Rhythm**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Stillâ†”Urgent | If BPM is within genre's expected range â†’ suppress. 85 BPM in post-rock is water. 85 BPM in EDM is a finding. | Urgent tempo + themes of stasis/entrapment â†’ "frantic stillness" (sign inverts). Still tempo + themes of urgency/crisis â†’ "paralysis" (sign inverts). | Click track / grid-quantized = authored. Variable BPM from live tracking = authored. BPM from sample source rather than artist choice = incidental. |
| A2 | Spaciousâ†”Compressed | Same genre gate. High BPM in DnB doesn't mean "compressed" â€” it means normal event density for the form. | Compressed tempo + themes of freedom/space â†’ "caged velocity." Spacious tempo + themes of claustrophobia â†’ "the slowness IS the trap." | Tempo chosen by artist vs inherited from sample/remix source. |
| A3 | Restingâ†”Propulsive | Genre determines whether the BPM range codes as propulsive. 122 in house = designed to propel. 122 in post-punk = energetic but not dance-propulsive. | Propulsive + themes of exhaustion/futility â†’ "treadmill." Resting + themes of building dread â†’ "the calm before." | If tempo serves the genre's somatic function (dance, march, meditation) = authored propulsion. |
| A4 | Singularâ†”Layered | Half/double time ambiguity. Genre-dependent: hip-hop EXPECTS dual-tempo perception (halftime + double hats). In classical it's a finding. | Layered tempo + themes of certainty â†’ "the body can't decide but the mind knows." Singular + themes of confusion â†’ "false clarity." | Dual-tempo by design (producer placed elements at different metric levels) vs artifact of detection algorithm. |

**Web targets specific to tempo:** DJ databases, Spotify metadata, genre BPM norms, producer interviews about tempo choice.

---

### #2 TEMPO STABILITY
**Category: Rhythm**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Mechanicalâ†”Organic | CV<0.05 in electronic = water. CV<0.05 in jazz = stunning finding. CV>0.30 in free jazz = water. | Mechanical + themes of humanity/emotion â†’ "the machine performing feeling" (BG's core tension). Organic + themes of control/precision â†’ "the human performing machine." | DAW/click track = mechanical is authored. Live tracking without click = organic is authored. Tape wow/flutter = organic is incidental. |
| A2 | Predictableâ†”Uncertain | Same genre gate. | Predictable + themes of chaos/disorder â†’ "enforced order" (bridge tension). Uncertain + themes of stability â†’ "the ground pretending to hold." | Quantization = authored predictability. Tempo map editing = authored uncertainty. |
| A3 | Anchoredâ†”Drifting | Genre baseline for temporal solidity. | Anchored + themes of impermanence â†’ EWTRTW pattern: "the anchor is the lie." Drifting + themes of home/belonging â†’ "searching for ground." | Deliberate tempo changes (ritardando, accelerando) = authored. Drummer fatigue / tracking drift = incidental. |
| A4 | Controlledâ†”Free | Genre norms for constraint. | Controlled + themes of liberation â†’ "the cage." Free + themes of imprisonment â†’ "escape that doesn't know it's free yet." | Grid-lock by choice vs grid-lock by software default. |

---

### #3 TEMPO AMBIGUITY
**Category: Rhythm**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Decisiveâ†”Torn | Hip-hop dual-tempo = unmarked. Polymetric metal (Meshuggah) = unmarked. | Torn + themes of certainty/conviction â†’ "the music doubts what the words don't." Decisive + themes of doubt â†’ "the rhythm knows what the singer won't admit." | Multiple tempos from layered sources vs single tempo perceived as ambiguous. |
| A2 | Singularâ†”Multiple | Genre determines expected pulse count. | Multiple + themes of simplicity/unity â†’ "fractured underneath." | Designed polymetric vs detection artifact. |
| A3 | Groundedâ†”Suspended | Whether metric footing is expected to be firm in the genre. | Suspended + themes of certainty â†’ "hovering despite knowing." | Ambiguity from arrangement vs from production artifacts (reverb tails, sample bleed). |
| A4 | Simpleâ†”Complex | Genre baseline for metric complexity. | Complex + themes of simplicity â†’ "the complexity is hidden from the listener." | Composed complexity vs emergent from layering. |

---

### #4 BEAT POSITIONS
**Category: Rhythm**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Sparseâ†”Saturated | Onset density norms per genre. Trap = sparse. DnB = saturated. | Saturated + themes of emptiness/loneliness â†’ "filling the void with events." Sparse + themes of abundance â†’ "confidence of restraint." | Authored density (arrangement choices) vs artifact of polyphonic layering. |
| A2 | Breathingâ†”Relentless | Whether gaps are expected in the genre's arrangement style. | Relentless + themes of peace/rest â†’ "won't let you rest." Breathing + themes of suffocation â†’ "the breaths are rationed." | Gating/sidechain creating artificial gaps = authored. Natural instrument decay = incidental. |
| A3 | Minimalâ†”Maximal | Genre norms for information load. | Maximal + themes of minimalism/clarity â†’ "noise as deflection." | Production density (track count) as authored choice vs era convention. |
| A4 | Uniformâ†”Variable | Whether density variation is expected in the genre. On/off architecture (pop) vs continuous (ambient). | Uniform + themes of change/transformation â†’ "nothing changes on the surface." Variable + themes of constancy â†’ "the structure contradicts its own premise." | Arrangement dynamics as composed feature vs mastering compression flattening real dynamics. |

---

### #5 INTER-ONSET INTERVALS
**Category: Rhythm**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Regularâ†”Irregular | Quantized genres expect regularity. | Regular + themes of chaos â†’ "imposed order." Irregular + themes of order â†’ "order breaking down." | Quantization = authored regularity. Live performance = authored irregularity. |
| A2 | Few typesâ†”Many types | Genre vocabulary size. Minimal techno = few. Jazz = many. | Few types + themes of complexity â†’ "simplicity as mask." | Programmed patterns vs performed patterns. |
| A3 | Narrowâ†”Wide | Expected interval range for genre. | Narrow + themes of freedom â†’ "constrained movement." | Fixed grid = narrow by design. Rubato = wide by design. |
| A4 | Repetitiveâ†”Novel | Genre norms for rhythmic repetition. Loop-based = repetitive is water. | Repetitive + themes of discovery â†’ "the loop as ritual, not stagnation." Novel + themes of tradition â†’ "restless departure." | Loop-based production = repetitive is authored. Generative/algorithmic = novel is authored. |

---

### #6 IOI HISTOGRAM
**Category: Rhythm**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Peakedâ†”Flat | Loop-based music = peaked is water. Free/aleatoric = flat is water. | Peaked + themes of variety â†’ "the pocket as prison." Flat + themes of consistency â†’ "can't settle." | Programmed = peaked. Performed = variable. |
| A2 | Clusteredâ†”Dispersed | Genre determines expected interval clustering. | Same inversion logic as A1. | Same as A1. |
| A3 | Concentratedâ†”Distributed | Same genre baseline. | Same inversion logic. | Same. |
| A4 | Predictableâ†”Surprising | Genre norms for rhythmic predictability. | Predictable + themes of uncertainty â†’ "the rhythm lies about the emotional state." | Authored predictability (composed groove) vs incidental (default quantize). |

---

### #7 ONSET DENSITY
**Category: Rhythm**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Sparseâ†”Saturated | Events/sec norms per genre. | Saturated + themes of absence â†’ "filling." Sparse + themes of fullness â†’ "restraint is confidence." | Arrangement choice (track count, note density) = authored. |
| A2 | Constantâ†”Variable | Whether density variation is a genre feature. Build/drop genres = variable is water. | Constant + themes of change â†’ "the surface refuses to reflect what's happening." | Arrangement dynamics vs mastering compression. |
| A3 | Low floorâ†”High floor | Genre baseline for minimum density. Ambient = low floor expected. Electronic = high floor expected. | High floor + themes of absence â†’ "even the silences are filled." | Sustain/reverb filling gaps = production choice. Noise floor = incidental. |
| A4 | Narrow rangeâ†”Wide range | Genre norms for density dynamics. | Narrow + themes of drama â†’ "suppressed dynamics." Wide + themes of constancy â†’ "the drama is structural, not lyrical." | Compression reducing range = production choice vs mastering artifact. |

---

### #8 RMS ENERGY
**Category: Dynamics**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Quietâ†”Loud | Genre norms for average loudness. Metal = loud is water. Folk = quiet is water. | Loud + themes of vulnerability â†’ "screaming as armor." Quiet + themes of power â†’ "doesn't need to shout." | Mastering loudness target = often incidental (era convention). Deliberate quiet mastering (Billie Eilish) = authored. |
| A2 | Distantâ†”Present | Genre spatial conventions. | Present + themes of absence â†’ "forcefully here." Distant + themes of intimacy â†’ "pulled back despite the subject." | Mix distance is authored. Mastering level is often incidental. |
| A3 | Gentleâ†”Forceful | Genre energy norms. | Forceful + themes of gentleness â†’ "aggression underneath tenderness." | Same as A1. |
| A4 | Recedingâ†”Advancing | Whether the mix pushes or pulls. | Advancing + themes of retreat â†’ "the sound won't let you leave." | Mix/master choice. |

---

### #9 PEAK AMPLITUDE
**Category: Dynamics**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Restrainedâ†”Explosive | Genre norms for peak handling. | Explosive + themes of control â†’ "eruption despite containment." | Limiting/clipping = authored restraint. Peak > 1.0 from YouTube rip = INCIDENTAL (always flag). |
| A2 | Controlledâ†”Unleashed | Same gate. | Controlled + themes of chaos â†’ "held back." | Limiter settings = authored. Format conversion artifacts = incidental. |
| A3 | Containedâ†”Overflowing | Same gate. | Same logic. | Same. |
| A4 | Soft-cappedâ†”Hard-hitting | Transient preservation norms per genre. | Hard-hitting + themes of softness â†’ "gentle words with fists." | Transient shaping is authored. Codec damage to transients = incidental. |

**CRITICAL: Peak > 1.0 â†’ ALWAYS check for format conversion artifact before scoring any axis.**

---

### #10 CREST FACTOR
**Category: Dynamics**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Compressedâ†”Breathing | Loudness war era (1995-2015) = compression is water. | Compressed + themes of freedom â†’ "crushed flat, nothing allowed to rise." Breathing + themes of suffocation â†’ "the dynamics mock the content." | Mastering compression = often era convention (incidental). Deliberate dynamic mastering = authored. |
| A2 | Flatâ†”Punchy | Genre transient norms. | Flat + themes of impact â†’ "the hits are absorbed." Punchy + themes of smoothness â†’ "every moment insists on itself." | Compressor settings = authored. Streaming normalization effects = incidental. |
| A3 | Denseâ†”Airy | Same gate. | Dense + themes of space â†’ EWTRTW pattern: "compression as concealment." | Same. |
| A4 | Sustainedâ†”Impactful | Genre norms for energy distribution. | Same logic. | Same. |

---

### #11 DYNAMIC RANGE
**Category: Dynamics**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Monotoneâ†”Dramatic | <7 dB in modern pop = water. >15 dB in classical = water. | Monotone + themes of drama â†’ EWTRTW: "the most dramatic lyrics in the flattest dynamic landscape." Dramatic + themes of stability â†’ "the music insists on movement the words deny." | Mastering target = often era/format convention (incidental). Wide dynamics by artistic choice (e.g., Radiohead) = authored. |
| A2 | Steadyâ†”Sweeping | Same gate. | Same logic. | Same. |
| A3 | Narrowâ†”Vast | Same gate. | Same logic. | Same. |
| A4 | Closeâ†”Extreme | Same gate. | Extreme + themes of intimacy â†’ "the dynamic space is wider than the emotional space." | Same. |

**NOTE: Dynamic range is the single most era-contaminated measurement. ALWAYS check production date before scoring. Pre-1990 = wider dynamics expected. 1995-2015 = loudness war compression. Post-2015 = streaming normalization partially restoring dynamics.**

---

### #12 SILENCE RATIO
**Category: Dynamics**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Relentlessâ†”Punctuated | Genre norms. Ambient/post-rock = low silence is water. Pop with drops = silence>5% expected. | Relentless + themes of rest/peace â†’ "won't let you rest" (OH-type: rest as lyrical theme with no structural rest). Punctuated + themes of continuity â†’ "keeps interrupting itself." | Arrangement choice = authored. Gating creating artificial silence = authored. |
| A2 | Fullâ†”Breathing | Same gate. | Full + themes of emptiness â†’ "stuffed to avoid facing the void." | Sustain/reverb filling gaps = authored. Noise floor masking silence = incidental. |
| A3 | Persistentâ†”Intermittent | Same gate. | Same logic. | Same. |
| A4 | Saturatedâ†”Framed | Whether silence functions compositionally. | Framed + themes of chaos â†’ "the silence is the structure the content doesn't have." Saturated + themes of space â†’ "no room to think." | Compositional silence (intro/outro, breakdowns) = authored. Track gap artifacts = incidental. |

---

### #13 DYNAMIC CONTRAST
**Category: Dynamics**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Flatâ†”Volatile | Genre norms for section contrast. | Flat + themes of emotional turbulence â†’ "the structure refuses to match the emotional stakes." Volatile + themes of constancy â†’ "the music dramatizes what the lyrics downplay." | Mastering flattening real contrast = incidental. Deliberate contrast in arrangement = authored. |
| A2 | Stableâ†”Escalating | Whether contrast slope is expected. Build genres = escalating is water. | Escalating + themes of acceptance/resolution â†’ "the structure is still fighting after the words surrender." Stable + themes of escalation â†’ "nothing rises to meet the crisis." | Same. |
| A3 | Smoothâ†”Jagged | Transition character norms. | Jagged + themes of smoothness â†’ "structural violence under smooth content." | Hard cuts (authored) vs crossfade (authored). Both are choices. |
| A4 | Predictableâ†”Startling | Same gate. | Startling + themes of predictability â†’ "the structure keeps surprising you even though the content is familiar." | Same. |

---

### #14 SPECTRAL CENTROID
**Category: Spectrum**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Heavyâ†”Bright | Era signature. 80s = bright is water. Lo-fi = dark is water. | Bright + dark themes â†’ EWTRTW: "enforced brightness, the shimmer is the mask." Heavy + bright themes â†’ "the weight contradicts the optimism." | DX7/digital synths = bright is era artifact. Tape/analog warmth = dark is era artifact. EQ choices = authored. |
| A2 | Warmâ†”Sharp | Same era gate + genre. | Warm + cold/clinical themes â†’ "false comfort." Sharp + intimate themes â†’ "intimacy with edges." | Same. |
| A3 | Groundedâ†”Floating | Genre spatial norms. | Grounded + themes of flight/escape â†’ "can't get off the ground." Floating + themes of rootedness â†’ "unmoored despite wanting anchor." | Same. |
| A4 | Stableâ†”Traveling | Whether centroid movement is expected. Progressive/post-rock = traveling is water. Pop = stable is water. | Stable + themes of change â†’ "timbral refusal to acknowledge transformation." Traveling + themes of stasis â†’ "the color changes even when nothing else does" (EWTRTW: centroid drops 30% while everything else holds still). | Production arc choices = authored. |

**CRITICAL: Axis 4 (Stableâ†”Traveling) requires trajectory data, not average. Flag if only average is available.**

---

### #15 SPECTRAL SPREAD
**Category: Spectrum**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Focusedâ†”Diffuse | Genre norms for spectral concentration. Solo instrument = focused is water. Full band = diffuse is water. | Focused + themes of complexity â†’ "tunneled." Diffuse + themes of clarity â†’ "fog." | Mix choices = authored. |
| A2 | Narrowâ†”Wide | Same gate. | Same logic. | Same. |
| A3 | Specificâ†”Ambient | Same gate. | Specific + themes of vagueness â†’ "the sound knows what the content doesn't." | Same. |
| A4 | Singularâ†”Complex | Same gate. | Complex + themes of simplicity â†’ "the simplicity is an illusion." | Track count / layering = authored. |

---

### #16 SPECTRAL ROLLOFF
**Category: Spectrum**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Darkâ†”Airy | Era signature (see #14). Genre norms for treble content. | Dark + themes of light/hope â†’ "the hope can't reach the upper frequencies." Airy + themes of darkness â†’ "bright surface over dark content." | EQ/filter choices = authored. Codec treble damage = incidental. Phone playback optimization raising treble = incidental. |
| A2 | Muffledâ†”Open | Same gate. | Muffled + themes of openness â†’ "sealed in." | Same. |
| A3 | Veiledâ†”Transparent | Same gate. | Veiled + themes of truth â†’ "something hiding." Transparent + themes of deception â†’ "showing everything yet lying." | Same. |
| A4 | Enclosedâ†”Expansive | Same gate. | Enclosed + themes of freedom â†’ "the ceiling is low." | Same. |

---

### #17 SPECTRAL FLATNESS
**Category: Spectrum**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Tonalâ†”Noisy | Genre norms. Noise rock = noisy is water. Pop = tonal is water. Shoegaze/industrial = noisy is water. | Tonal + themes of disorder â†’ "false purity." Noisy + themes of order â†’ "disorder as method" (punk ethos). | Distortion = authored noise. Recording quality / format artifacts = incidental noise. |
| A2 | Cleanâ†”Textured | Same gate. | Clean + themes of messiness â†’ "the production lies about the emotional state." | Same. |
| A3 | Pureâ†”Gritty | Same gate. | Pure + themes of corruption â†’ "the purity is the corruption." | Same. |
| A4 | Singingâ†”Hissing | Same gate. | Singing + themes of silence â†’ "can't stop making pitch." | Same. |

**NOTE: Flatness RANGE (max - min) is often more informative than the value itself. Wide range = mode-switching (cleanâ†”distorted). Narrow range = consistent timbral identity.**

---

### #18 SPECTRAL FLUX
**Category: Spectrum**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Staticâ†”Shimmering | Genre norms for timbral movement. Electronic = static is common. Live = shimmering is common. | Static + themes of change â†’ "frozen while the world moves." Shimmering + themes of permanence â†’ "restless surface over permanent base." | Synth modulation = authored shimmer. Tape flutter = incidental shimmer. |
| A2 | Frozenâ†”Alive | Same gate. | Frozen + themes of life â†’ "the sound is dead but the lyrics are alive." | Same. |
| A3 | Sustainedâ†”Flickering | Same gate. | Same logic. | Same. |
| A4 | Gradualâ†”Abrupt | Same gate. | Abrupt + themes of smoothness â†’ "the timbre jumps even when the narrative doesn't." | Deliberate arrangement changes = authored. Mixing inconsistencies = incidental. |

---

### #19 STEREO CORRELATION
**Category: Stereo**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Intimateâ†”Surrounding | Era/genre norms. Mono-era = intimate is water. Modern pop = variable. | Intimate + themes of distance/alienation â†’ "forced closeness." Surrounding + themes of isolation â†’ "surrounded but alone" (NTLTC opening). | Mono/stereo mixing choice = authored. Mono collapse from format (AM radio, phone speaker) = incidental. |
| A2 | Centeredâ†”Enveloping | Same gate. | Same logic. | Same. |
| A3 | Focusedâ†”Dispersed | Same gate. | Focused + themes of confusion â†’ "the spatial clarity contradicts the emotional confusion." | Same. |
| A4 | Insideâ†”Around | Same gate. | Inside + themes of external threat â†’ "trapped in headspace." Around + themes of interiority â†’ "the world intrudes on private experience." | Same. |

**CRITICAL: Stereo correlation TRAJECTORY is often more important than average. NTLTC goes 0.02â†’0.89. EWTRTW goes 0.15â†’0.82â†’0.47-0.67. underscores goes 0.97â†’-0.03. Always flag trajectory availability.**

---

### #20 MID/SIDE RATIO
**Category: Stereo**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Centeredâ†”Dispersed | Genre mixing norms. Bedroom production = centered is expected. Major-label pop = dispersed is expected. | Centered + themes of reaching out â†’ "the world is all in one point." | M/S processing = authored. Mono recording = era artifact (incidental). |
| A2 | Directâ†”Ambient | Same gate. | Direct + themes of mystery â†’ "confronting you with no escape." | Same. |
| A3 | Frontalâ†”Surrounding | Same gate. | Same logic. | Same. |
| A4 | Anchoredâ†”Floating | Same gate. | Anchored + themes of drift â†’ "spatially locked while emotionally unmoored." | Same. |

---

### #21 STEREO WIDTH PER BAND
**Category: Stereo**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Monoâ†”Wide | Genre/era norms. Modern = wide bass/narrow lows is convention. | Mono + themes of expansion â†’ "no room." | Mixing convention = often incidental. Deliberate mono bass = authored. |
| A2 | Stackedâ†”Spread | Production norms. | Stacked + themes of diversity â†’ "everything piled in one place." | Same. |
| A3 | Simpleâ†”Layered | Genre layering norms. | Layered + themes of simplicity â†’ "the spatial complexity contradicts the message of simplicity." | Track count / panning = authored. |
| A4 | Uniformâ†”Selective | Whether frequency-selective width is expected. | Selective + themes of consistency â†’ "the mix treats some frequencies differently, creating hidden hierarchies." | Mix engineering choice = authored. |

---

### #22 SPECTRAL BAND ENERGY
**Category: Spectrum**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Bottom-heavyâ†”Top-heavy | Genre norms. Hip-hop/trap = bottom-heavy is water. 80s pop = top-heavy is water. | Bottom-heavy + themes of lightness â†’ "gravitational pull against aspirational content." Top-heavy + themes of weight â†’ "trying to float above the heaviness." | Sub-bass emphasis = authored (genre marker). Playback compensation EQ = incidental. |
| A2 | Warmâ†”Brittle | Same era/genre gate. | Warm + cold themes â†’ "false comfort in the frequency response." Brittle + tender themes â†’ "vulnerable, exposed, no padding." | Same. |
| A3 | Physicalâ†”Cerebral | Genre somatic norms. Dance = physical is expected. Art-pop = cerebral is expected. | Physical + themes of intellect/abstraction â†’ "the body knows what the mind is debating." Cerebral + themes of embodiment â†’ "the mix is in your head when the lyrics are in your body." | Same. |
| A4 | Fullâ†”Thin | Genre production norms for spectral completeness. | Full + themes of emptiness â†’ "stuffed mix, empty meaning." Thin + themes of abundance â†’ "sparse means to rich ends." | Mixing/mastering choice = authored. Poor recording quality = incidental thinness. |

---

### #23 BAND RATIOS
**Category: Spectrum**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Bass-dominantâ†”Treble-dominant | Genre norms (same as #22). | Same logic as #22. | Same. |
| A2 | Thickâ†”Lean | Same gate. | Thick + themes of vulnerability â†’ "armored." Lean + themes of strength â†’ "stripped to the essential." | Same. |
| A3 | Warmâ†”Cool | Same gate. | Warm + themes of coldness â†’ "the temperature of the mix contradicts the emotional temperature." | Same. |
| A4 | Massiveâ†”Delicate | Genre norms for sonic weight. | Massive + themes of fragility â†’ "the weight is compensation." Delicate + themes of power â†’ "strength that doesn't need mass." | Same. |

---

### #24 AVERAGE BEAT WAVEFORM
**Category: Beat Shape**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Regularâ†”Variable | Genre norms for beat consistency. Loop-based = regular is water. Progressive = variable is expected. | Regular + themes of unpredictability â†’ "the beat won't acknowledge the chaos." | Programmed beats = authored regularity. Sample variation = authored variability. |
| A2 | Simpleâ†”Complex | Genre norms for beat complexity. | Simple + themes of complexity â†’ "the beat is simpler than the emotional content." | Same. |
| A3 | Uniformâ†”Diverse | Same gate. | Same logic. | Same. |
| A4 | Predictableâ†”Evolving | Same gate. | Evolving + themes of stasis â†’ "the beat changes even when the narrative doesn't." | Same. |

---

### #25 BEAT MICRO-PEAKS
**Category: Beat Shape â€” ENGINE STATUS: SUSPECT. Discard or downweight.**

| All axes | All poles | Genre filter: N/A â€” measurements unreliable across all reference songs. | Thematic: N/A | Production: N/A |

**Weight override: 0.0 for all axes until engine accuracy improves.**

---

### #26 ATTACK TIME
**Category: Beat Shape**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Suddenâ†”Gradual | Genre norms. Electronic = sharp attacks expected. Post-rock = slow attacks expected. Gated drums (80s) = artificially sharp. | Sudden + themes of gentleness â†’ "the impact contradicts the tenderness." Gradual + themes of urgency â†’ "approaching but never arriving." | Gated reverb = authored sharpness. Sampled transients = authored (from source). Compressor attack settings = authored. |
| A2 | Strikingâ†”Swelling | Same gate. | Striking + themes of softness â†’ "the gentleness has edges." Swelling + themes of decisiveness â†’ "the certainty approaches slowly." | Same. |
| A3 | Sharpâ†”Soft | Same gate. | Same logic. | Same. |
| A4 | Percussiveâ†”Bowed | Genre instrument norms. | Percussive + themes of flow â†’ "punctuation in a run-on sentence." Bowed + themes of punctuation â†’ "the attack refuses to mark boundaries." | Instrument choice = authored. |

**NOTE: Engine reads attack inaccurately for some songs (BG: 848ms engine vs 226ms dictionary). Weight reduced if engine-only data.**

---

### #27 DECAY TIME
**Category: Beat Shape â€” ENGINE STATUS: UNRELIABLE**

| All axes | All poles | Measurements fail for NTLTC (5ms impossible). Unreliable across songs. | Thematic: apply cautiously | Production: apply cautiously |

**Weight override: 0.3 for all axes. Only trust if value is physically plausible (>50ms, <5000ms).**

---

### #28 ATTACK/DECAY RATIO
**Category: Beat Shape â€” ENGINE STATUS: UNRELIABLE (dependent on #27)**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Percussiveâ†”Swelling | Genre norms. Dance = percussive expected. Ambient = swelling expected. | Percussive + themes of approach â†’ "arriving violently." Swelling + themes of impact â†’ "the impact is distributed over time, not concentrated." | Instrument/synth design = authored. BG engine reads 24.649 (extreme sustained) vs dictionary 0.343 (percussive). **ALWAYS cross-reference with attack time.** |
| A2-A4 | See poles above | Same cautions apply. | Same logic. | Same. |

**Weight override: 0.3 for all axes due to engine unreliability. Only trust direction (percussive vs swelling), not magnitude.**

---

### #29 ZERO CROSSING RATE
**Category: Spectrum**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Smoothâ†”Textured | Genre norms. Clean pop = smooth is water. Rock/industrial = textured is water. | Smooth + themes of roughness â†’ "polished surface over rough content." Textured + themes of purity â†’ "the impurities are showing." | Distortion = authored texture. Recording noise = incidental texture. |
| A2 | Warmâ†”Bristling | Same gate. | Same logic. | Same. |
| A3 | Mellowâ†”Edgy | Same gate. | Same logic. | Same. |
| A4 | Roundedâ†”Crisp | Same gate. | Same logic. | Same. |

---

### #30 FUNDAMENTAL FREQUENCY
**Category: Pitch â€” ENGINE STATUS: WORKS BELOW 200Hz, FAILS FOR POLYPHONIC MID/HIGH**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Lowâ†”High | Genre vocal/instrument range norms. | Low + themes of elevation/ascension â†’ "earthbound." High + themes of gravity/weight â†’ "trying to escape the pull." | Tuning/key choice = authored. Voice type = incidental (singer's physiology). |
| A2 | Deepâ†”Soaring | Same gate. | Same logic. | Same. |
| A3 | Groundedâ†”Floating | Same gate. | Same logic. | Same. |
| A4 | Heavyâ†”Light | Same gate. | Same logic. | Same. |

**Weight override: 0.5 if detected F0 is above 200Hz (unreliable in polyphonic content).**

---

### #31 DURATION
**Category: Meta**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Briefâ†”Expansive | Genre/era norms. Punk = brief is water. Post-rock = expansive is water. Streaming era (<3:30) = new brief convention. | Brief + themes of eternity â†’ "refuses to stay." Expansive + themes of urgency â†’ "takes its time despite the crisis." | Single edit vs album version = authored. Radio edit = often incidental truncation. **ALWAYS check file completeness vs known track length.** |
| A2 | Compressedâ†”Sprawling | Same gate. | Same logic. | Same. |
| A3 | Urgentâ†”Patient | Same gate. | Urgent + themes of patience â†’ "the structure hurries what the content asks you to hold." | Same. |
| A4 | Containedâ†”Unfolding | Same gate. | Same logic. | Same. |

**CRITICAL: Always verify file duration vs known track length. OH was truncated at 2:49 of 4:32. All measurements carry asterisk if truncated.**

---

### #32 LEADING/TRAILING SILENCE
**Category: Meta**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Abruptâ†”Ceremonial | Genre norms. Punk = abrupt. Classical = ceremonial. | Ceremonial + themes of urgency â†’ "the ritual frame contradicts the crisis." Abrupt + themes of ceremony â†’ "won't give you the preparation you need." | Compositional silence (>2s lead, >5s trail) = authored. Digital format auto-trim = incidental removal. CD track gaps = format artifact. |
| A2 | Immediateâ†”Prepared | Same gate. | Same logic. | Same. |
| A3 | Cutâ†”Faded | Same gate. | Cut + themes of continuation â†’ "the ending is a lie." Faded + themes of finality â†’ "honest ending â€” it acknowledges disappearance." (EWTRTW: "the most honest structural choice in the whole track.") | Fade-out = authored (but also era convention: 60s-80s = fade is water). Hard stop = authored. |
| A4 | Unframedâ†”Framed | Same gate. | Framed + themes of rawness â†’ "the ceremony contradicts the content's roughness." | Same. |

---

### #33 LOUDNESS CURVE
**Category: Dynamics**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Flatâ†”Shaped | Genre norms. Pop = shaped (verse/chorus dynamics). Punk/metal = often flat. | Flat + themes of emotional journey â†’ EWTRTW: "the loudness refuses to trace the emotional arc." Shaped + themes of constancy â†’ "the volume tells a story the lyrics don't." | Mastering compression flattening curve = incidental. Deliberately flat master (wall-of-sound) = authored. |
| A2 | Constantâ†”Building | Same gate. | Building + themes of rest â†’ "the structure escalates what the content tries to calm." Constant + themes of crescendo â†’ "won't rise to meet the emotional peak." | Same. |
| A3 | Steadyâ†”Undulating | Same gate. | Same logic. | Same. |
| A4 | Predictableâ†”Narrative | Same gate. | Narrative loudness + simple lyrics â†’ "the structure is more emotionally complex than the words." Predictable loudness + complex lyrics â†’ "the music is simpler than the text." | Same. |

---

### #34 SELF-SIMILARITY MATRIX
**Category: Form**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Repetitiveâ†”Through-composed | Genre norms. Pop = repetitive is water. Progressive = through-composed is water. | Repetitive + themes of change/growth â†’ "the structure loops while the content evolves" (common in mantra-like songs). Through-composed + themes of return â†’ "structurally can't go back even though the lyrics want to." | Verse-chorus form = genre convention (often incidental repetition). Deliberate structural repetition (minimalism) = authored. |
| A2 | Familiarâ†”Novel | Same gate. | Familiar + themes of strangeness â†’ "the form domesticates the content." | Same. |
| A3 | Cyclicalâ†”Linear | Same gate. | Cyclical + themes of progress â†’ "going in circles despite trying to move forward." Linear + themes of return â†’ "can't go home." | Same. |
| A4 | Anchoredâ†”Adventurous | Same gate. | Anchored + themes of exploration â†’ "formally conservative despite thematic ambition." | Same. |

---

### #35 SECTION SYMMETRY
**Category: Form**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Symmetricalâ†”Asymmetrical | Genre norms. AABA/verse-chorus = symmetrical is water. Progressive = asymmetrical is water. | Symmetrical + themes of disorder â†’ "the form imposes order the content doesn't have." | Standard song form = genre convention (incidental). Deliberately symmetrical arrangement = authored. |
| A2 | Returningâ†”Departing | Same gate. | Returning + themes of loss â†’ "keeps coming back to what's gone." | Same. |
| A3 | Many mirrorsâ†”Few mirrors | Same gate. | Many mirrors + themes of uniqueness â†’ "every section is a copy." | Same. |
| A4 | Predictableâ†”Surprising | Same gate. | Predictable + themes of surprise â†’ "you always know what's coming structurally even when the content shocks." | Same. |

---

### #36 CHROMAGRAM
**Category: Pitch**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Sparseâ†”Dense | Genre harmonic vocabulary norms. Punk = sparse. Jazz = dense. Pop = moderate. | Sparse + themes of richness â†’ "harmonic poverty." Dense + themes of simplicity â†’ "the harmony is more complex than the message." | Chord choices = authored. Layered production accumulating chromatic density = incidental aggregation. |
| A2 | Stableâ†”Shifting | Same gate. | Stable + themes of change â†’ "the harmony holds while everything else moves." | Same. |
| A3 | Peakedâ†”Flat | Same gate. | Peaked + themes of democracy/equality â†’ "one pitch class dominates." Flat + themes of hierarchy â†’ "all pitches equal, no tonal hierarchy." | Same. |
| A4 | Consistentâ†”Variable | Same gate. | Consistent + themes of transformation â†’ "harmonically static despite narrative development." | Same. |

---

### #37 KEY
**Category: Pitch**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Clearâ†”Ambiguous | Genre norms. Pop = clear is water. Jazz/post-rock = ambiguous is water. | Clear + themes of confusion â†’ "the harmony knows what the narrator doesn't." Ambiguous + themes of certainty â†’ BG: "the harmonic uncertainty IS the existential uncertainty." | Key chosen by artist = authored. Key inherited from sample = incidental (attribute to source, not artist). |
| A2 | Majorâ†”Minor | **CRITICAL: Western cultural bias. Majorâ‰ happy, minorâ‰ sad across all cultures.** Genre norms for modal color. | Major + dark themes â†’ potentially "ironic brightness" but CAUTION: this mapping is culturally specific. Minor + bright themes â†’ potentially "joy with shadows" but same caution. | Same. |
| A3 | Confidentâ†”Tentative | Key detection correlation strength. | Confident + themes of doubt â†’ "the key is surer than the singer." Tentative + themes of certainty â†’ "even the tonal center wavers." | Deliberately ambiguous tonality = authored. Poor key detection from polyphonic complexity = engine artifact. |
| A4 | Rootedâ†”Floating | Same as key stability. | Rooted + themes of displacement â†’ OH: "maximally rooted â€” the music provides the stability the narrator lacks." Floating + themes of home â†’ "can't find tonal home despite wanting to belong." | Same. |

**CRITICAL: Key confidence < 0.80 â†’ ALWAYS web-search for confirmed key. Binary key detection unreliable for chromatic/modal/non-Western music.**

---

### #38 KEY STABILITY
**Category: Pitch**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Rootedâ†”Restless | Genre norms. Pop = rooted is expected. Jazz/progressive = restless is expected. | Rooted + themes of restlessness â†’ OH: "the harmonic ground holds when the narrator can't." Restless + themes of stability â†’ "tonally searching despite narrative certainty." | Key changes by composition = authored. Pitch drift from tape = incidental. |
| A2 | Permanentâ†”Modulating | Same gate. | Same logic. | Same. |
| A3 | Groundedâ†”Drifting | Same gate. | Same logic. | Same. |
| A4 | Singularâ†”Oscillating | Same gate. | Oscillating + themes of commitment â†’ BG: "F#â†”C# oscillation IS the existential wavering." Singular + themes of ambivalence â†’ "the key has chosen even though the narrator hasn't." | Same. |

---

### #39 CHORD ESTIMATION
**Category: Pitch â€” ENGINE STATUS: REQUIRES WEB VALIDATION**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Simpleâ†”Complex | Genre harmonic vocabulary norms. | Simple + themes of complexity â†’ "the harmonic language is simpler than the emotional content." Complex + themes of simplicity â†’ "overthinking in the chord changes." | Songwriter's harmonic vocabulary = authored. |
| A2 | Diatonicâ†”Chromatic | Same gate. | Same logic. | Same. |
| A3 | Resolvedâ†”Suspended | Genre resolution norms. | Resolved + themes of unresolved tension â†’ "the harmony resolves what the narrative doesn't." Suspended + themes of completion â†’ "harmonically unfinished despite narrative closure." | Same. |
| A4 | Familiarâ†”Surprising | Same gate. | Same logic. | Same. |

**NOTE: Always cross-reference with web-sourced chord progressions (Songsterr, Ultimate Guitar, Genius annotations). Engine chord detection is approximate.**

---

### #40 CHROMATIC DENSITY
**Category: Pitch â€” ENGINE STATUS: LOW DISCRIMINATION (all songs read 5.7-6.9)**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Pureâ†”Chromatic | Genre norms. Simple pop = pure. Jazz/art-pop = chromatic. | Pure + themes of corruption â†’ "harmonically innocent." Chromatic + themes of purity â†’ "the harmony is dirtier than the message." | Same. |
| A2-A4 | See poles above | Same gate. | Same logic. | Same. |

**Weight override: 0.5 for all axes due to low engine discrimination. Trust only relative ordering between songs, not absolute values.**

---

### #41 MFCCs
**Category: Timbre**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Narrowâ†”Wide | Genre timbral range norms. Singer-songwriter = narrow is water. Electronic = wide is expected. | Narrow + themes of variety â†’ "timbral monotony despite varied content." Wide + themes of consistency â†’ "the sound world is larger than the emotional world." | Production palette choice = authored. |
| A2 | Simpleâ†”Complex | Same gate. | Same logic. | Same. |
| A3 | Consistentâ†”Contrasting | Same gate. | Consistent + themes of contrast â†’ "the timbre refuses to differentiate." | Same. |
| A4 | Homogeneousâ†”Heterogeneous | Same gate. | Same logic. | Same. |

---

### #42 MFCC TRAJECTORY
**Category: Timbre**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Consistentâ†”Transforming | Genre norms. Pop = relatively consistent. Progressive = transforming is expected. | Consistent + themes of transformation â†’ "the timbre stays while the narrative moves." Transforming + themes of permanence â†’ "the sound can't sit still." | Arrangement changes = authored. |
| A2 | Gradualâ†”Sudden | Same gate. | Gradual + themes of rupture â†’ "the timbral transition is gentler than the narrative rupture." Sudden + themes of continuity â†’ underscores COLLAPSE: "everything changes at once." | Same. |
| A3 | Few shiftsâ†”Many shifts | Same gate. | Same logic. | Same. |
| A4 | Smoothâ†”Jagged | Same gate. | Same logic. | Same. |

**NOTE: MFCC shift > 300 at a single point = COLLAPSE event (underscores pattern). Always flag this magnitude of shift.**

---

### #43 SPECTRAL CONTRAST
**Category: Spectrum**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Blendedâ†”Separated | Genre mixing norms. Lo-fi = blended is expected. Major-label pop = separated is expected. | Blended + themes of clarity â†’ "can't distinguish the voices." Separated + themes of unity â†’ "every element stands apart despite the message of togetherness." | Mix engineering = authored. Soothe2/dynamic EQ achieving separation in dense mix = authored technique (underscores finding). |
| A2 | Opaqueâ†”Transparent | Same gate. | Opaque + themes of truth â†’ "can't see through the mix." Transparent + themes of mystery â†’ "everything visible yet still mysterious." | Same. |
| A3 | Fusedâ†”Articulated | Same gate. | Same logic. | Same. |
| A4 | Uniformâ†”Stratified | Same gate. | Stratified + themes of equality â†’ "the mix has a hidden hierarchy." | Same. |

---

### #44 HARMONIC-TO-NOISE RATIO
**Category: Pitch â€” ENGINE STATUS: SYSTEMATICALLY LOW (reads 3-6dB under conversational values)**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Cleanâ†”Rough | Genre norms. Pop/electronic = clean is water. Rock/blues = rough is water. | Clean + themes of rawness â†’ "polished veneer over raw content." Rough + themes of purity â†’ "the noise IS the authenticity." | Distortion = authored roughness. Recording quality = incidental roughness. Vocal stacking raising HNR (underscores) = authored purity. |
| A2 | Pureâ†”Gritty | Same gate. | Same logic. | Same. |
| A3 | Polishedâ†”Raw | Same gate. | Polished + themes of struggle â†’ "the production conceals the effort." | Same. |
| A4 | Controlledâ†”Distressed | Same gate. | Controlled + themes of distress â†’ "the signal is intact even though the content is falling apart." | Same. |

**Weight override: 0.5 for all axes. Engine reads systematically low. Collapsed to 2 zones (>4.0 = cleaner, <4.0 = rougher). Trust relative, not absolute.**

---

### #45 POLYPHONIC DENSITY
**Category: Spectrum**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Soloâ†”Chorus | Genre layering norms. Singer-songwriter = solo is expected. Maximalist pop = chorus is expected. | Solo + themes of community â†’ "alone despite wanting company." Chorus + themes of loneliness â†’ NTLTC: "surrounded by 65-82 simultaneous voices singing about being alone." | Track count = authored. |
| A2 | Thinâ†”Thick | Same gate. | Thin + themes of fullness â†’ "sparse means." Thick + themes of emptiness â†’ "the density is compensation." | Same. |
| A3 | Exposedâ†”Surrounded | Same gate. | Exposed + themes of hiding â†’ "nowhere to hide." Surrounded + themes of exposure â†’ "buried in layers." | Same. |
| A4 | Constantâ†”Building | Same gate. | Building + themes of arrival â†’ "still accumulating, not yet there." Constant + themes of journey â†’ "already at maximum, the journey is lateral not vertical." | Same. |

---

### #46 SPECTRAL BANDWIDTH
**Category: Spectrum**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Narrowâ†”Full | Genre/era norms. AM radio era = narrow. Modern mastering = full. | Narrow + themes of expansiveness â†’ "the frequency world is smaller than the emotional world." Full + themes of constraint â†’ "spectrally complete yet emotionally limited." | Bandwidth limitations from format = incidental. EQ choices = authored. |
| A2 | Containedâ†”Expansive | Same gate. | Same logic. | Same. |
| A3 | Partialâ†”Complete | Same gate. | Same logic. | Same. |
| A4 | Stableâ†”Expanding | Same gate. | Expanding + themes of contraction â†’ "the spectral world opens while the narrative closes." | Same. |

---

### #47 F0 TRAJECTORY
**Category: Pitch â€” ENGINE STATUS: FAILS FOR POLYPHONIC MID/HIGH CONTENT**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Monotoneâ†”Searching | Genre melodic range norms. | Monotone + themes of exploration â†’ "the pitch stays home while the lyrics wander." Searching + themes of certainty â†’ "the melody can't decide even though the words have." | Vocal melody = authored. Instrument melody = authored. |
| A2 | Anchoredâ†”Wandering | Same gate. | Same logic. | Same. |
| A3 | Steadyâ†”Sweeping | Same gate. | Same logic. | Same. |
| A4 | Containedâ†”Soaring | Same gate. | Soaring + themes of being grounded â†’ "the melody reaches for what the words can't have." Contained + themes of flight â†’ "the pitch is caged." | Same. |

**Weight override: 0.5 if F0 detected above 200Hz. Engine fails for polyphonic mid/high content.**

---

### #48 TIME SIGNATURE
**Category: Rhythm**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Regularâ†”Irregular | Genre norms. Pop = 4/4 is water. Progressive = irregular is expected. Math rock = irregular is water. | Regular + themes of disruption â†’ "the meter holds even when the content breaks." Irregular + themes of regularity â†’ "the meter is stranger than the message." | Composed time signature = authored. |
| A2 | Simpleâ†”Compound | Same gate. | Same logic. | Same. |
| A3 | Feltâ†”Nominal | Whether the time signature is obeyed. | Felt + themes of abstraction â†’ "the body follows the meter literally." Nominal + themes of embodiment â†’ "the written time signature is a suggestion the performance ignores." | Same. |
| A4 | Rigidâ†”Fluid | Same gate. | Rigid + themes of flexibility â†’ "the meter won't bend." | Same. |

---

### #49 VOCAL PRESENCE
**Category: Timbre â€” ENGINE STATUS: BROKEN (returns 0% for all tracks)**

| All axes | All poles | N/A â€” engine returns 0% for all tracks. Needs ML replacement. | N/A | N/A |

**Weight override: 0.0 for engine data. Use web context to populate:** vocal presence is ALWAYS known from web sources (instrumental vs vocal track, lead vocalist, featured artists). This element is fully web-dependent until engine is fixed.

---

### #50 INSTRUMENT ID
**Category: Timbre â€” ENGINE STATUS: REQUIRES WEB VALIDATION**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Syntheticâ†”Acoustic | Genre norms. Electronic = synthetic is water. Folk = acoustic is water. | Synthetic + themes of naturalness â†’ BG: "the synthetic is asking whether feeling is real." Acoustic + themes of artificiality â†’ "organic instruments in a conceptual frame." | Instrument selection = authored. |
| A2 | Singleâ†”Ensemble | Same gate. | Single + themes of community â†’ "alone." Ensemble + themes of solitude â†’ "crowded but lonely." | Same. |
| A3 | Homogeneousâ†”Diverse | Same gate. | Homogeneous + themes of diversity â†’ "one sound world for many emotions." | Same. |
| A4 | Familiarâ†”Novel | Same gate. | Familiar + themes of discovery â†’ "conventional instruments for unconventional ideas." Novel + themes of tradition â†’ "strange sounds carrying familiar sentiments." | Same. |

**NOTE: Instrument ID is primarily web-sourced (production credits, liner notes, interviews). Engine provides spectral hints only.**

---

### #51 PANNING
**Category: Stereo**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Balancedâ†”Lopsided | Genre mixing norms. | Balanced + themes of imbalance â†’ "the stereo field is fairer than the content." Lopsided + themes of balance â†’ "the spatial weight is uneven." | Pan positions = authored. Mono fold artifacts = incidental. |
| A2 | Centeredâ†”Off-center | Same gate. | Same logic. | Same. |
| A3 | Staticâ†”Moving | Same gate. | Static + themes of movement â†’ "everything stays in place." Moving + themes of stillness â†’ "the space moves even when nothing else does." | Auto-pan = authored. Phase issues creating phantom movement = incidental. |
| A4 | Narrowâ†”Wide | Same gate. | Same logic as stereo correlation. | Same. |

---

### #52 REVERB ESTIMATION
**Category: Stereo â€” ENGINE STATUS: BROKEN (impossibly dry readings)**

| All axes | All poles | N/A â€” engine returns impossibly dry readings for all tracks. Needs isolated transient analysis. | N/A | N/A |

**Weight override: 0.0 for engine data. Use web context to populate:** reverb character is often documented (studio, era, specific reverb units like AMS RMX16, Lexicon 480L). This element is fully web-dependent until engine is fixed.

**When web-sourced reverb data is available, scoring applies normally:**
- Closeâ†”Distant: genre norms for spatial proximity
- Dryâ†”Wet: era signature (80s = wet, 90s grunge = dry, 2010s = variable)
- Smallâ†”Vast: intended space. Studio vs cathedral vs synthetic.
- Presentâ†”Ethereal: genre norms for spatial character.

---

### #53 TEMPO MODULATION
**Category: Rhythm**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Rigidâ†”Elastic | Genre norms. DAW production = rigid is expected. Classical/jazz = elastic is expected. | Rigid + themes of flexibility/freedom â†’ "the tempo is a cage." Elastic + themes of rigidity â†’ "the tempo bends even though the content is inflexible." | Click track = authored rigidity. Live rubato = authored elasticity. |
| A2 | Constantâ†”Oscillating | Same gate. | Same logic. | Same. |
| A3 | Structuralâ†”Gestural | Whether modulation is identity or feature. | Structural + themes of decoration â†’ "the tempo change IS the song, not an ornament." Gestural + themes of substance â†’ "the tempo modulation is decorative despite the heavy content." | Same. |
| A4 | Absentâ†”Pervasive | Same gate. | Absent + themes of change â†’ "the one thing that doesn't change." | Same. |

---

### #54 INTER-ONSET ENERGY CONTOUR
**Category: Rhythm**

| Axis | Poles | Genre Filter | Thematic Inverter | Production Filter |
|------|-------|-------------|-------------------|-------------------|
| A1 | Swingingâ†”Ticking | Genre groove norms. Jazz/funk = swinging is water. Electronic = ticking is water. Trap = ticking is water. | Swinging + themes of rigidity â†’ "the body grooves while the mind is locked." Ticking + themes of fluidity â†’ "mechanical time in organic content" (BG pattern). | Swing parameter in DAW = authored. Natural instrument resonance creating swing = incidental (instrument physics). |
| A2 | Weightedâ†”Weightless | Same gate. | Weighted + themes of weightlessness â†’ "the arc between beats has gravity the content denies." Weightless + themes of burden â†’ "the rhythm is lighter than the content it carries." | Same. |
| A3 | Continuousâ†”Discrete | Same gate. | Continuous + themes of interruption â†’ "the energy between beats doesn't acknowledge the breaks." Discrete + themes of flow â†’ "the space between events is empty despite the flowing narrative." | Sustain/reverb filling between events = authored continuity. Gating creating discreteness = authored. |
| A4 | Curvedâ†”Flat | Same gate. | Curved + themes of linearity â†’ "the acceleration profile is richer than the narrative trajectory." Flat + themes of curvature â†’ "the space between beats is inert." | Instrument envelope = mostly incidental (physics of the sound source). But chosen instrument = authored. |

---

## BROKEN ELEMENTS â€” WEB-ONLY SCORING

These elements have engine weight 0.0 and rely entirely on web context:

| # | Element | Status | Web Source |
|---|---------|--------|------------|
| 25 | Beat micro-peaks | Suspect measurements | N/A â€” discard |
| 49 | Vocal presence | Returns 0% always | Track listing, credits, reviews |
| 52 | Reverb estimation | Impossibly dry | Studio notes, era conventions, producer interviews |

## DEGRADED ELEMENTS â€” REDUCED ENGINE WEIGHT

| # | Element | Engine Weight | Reason |
|---|---------|--------------|--------|
| 27 | Decay time | 0.3 | Physically impossible readings |
| 28 | A/D ratio | 0.3 | Dependent on broken #27 |
| 30 | F0 (>200Hz) | 0.5 | Fails for polyphonic mid/high |
| 40 | Chromatic density | 0.5 | Low discrimination (all songs 5.7-6.9) |
| 44 | HNR | 0.5 | Systematically 3-6dB low |
| 47 | F0 trajectory (>200Hz) | 0.5 | Same as #30 |

---

## IMPLEMENTATION SEQUENCE

1. **Web scrape fires FIRST** (or parallel to Pass 1, but results gate Pass 1 interpretation)
2. Retrieve: genre, thematic valence, production method
3. For each of 54 elements Ã— 4 axes:
   a. Check engine reliability status â†’ apply weight override if degraded/broken
   b. Apply genre markedness filter â†’ suppress or amplify weight
   c. Apply thematic alignment filter â†’ set sign (positive or negative) and thematic weight
   d. Apply production attribution filter â†’ adjust weight for authored vs incidental
   e. Compute final axis score
4. Sort axes by |final score|
5. Axes with |score| > 1.5 = **primary findings** (report first)
6. Axes with negative sign = **bridge tension markers** (report as tension, not as face value)
7. Axes with |score| < 0.15 = **suppressed** (don't report)
8. Everything in between = **supporting context** (report if relevant to primary findings)

---

## THE KEY INSIGHT

This scoring map means the web pass doesn't "add context." It **activates the structural measurements**. Without it, the engine produces 216 unweighted, unsigned axis readings â€” a cloud of potential meanings with no way to select which ones are real for this song. The web context is the key ring. Genre selects which axes are marked. Thematic valence selects the sign. Production method filters signal from noise.

The structural pass measures the bridge's cables. The web pass tells you which direction they pull.
