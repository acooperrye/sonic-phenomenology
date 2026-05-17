# Equipment Dictionary

## Purpose

Map specific instruments, synthesizers, drum machines, and signal processing gear to their spectral and temporal fingerprints. The long-term goal: **backtrace identification** â€” given a binary audio file, infer what equipment produced it.

This is the production archaeology layer. It complements the role taxonomy (Phase A) which asks "what does this sound DO in the mix?" by asking "what MADE this sound?"

Together they enable a two-question diagnostic:
1. **Role taxonomy**: What functional position does this sound occupy? (sustained-low, percussive-mid, etc.)
2. **Equipment dictionary**: What manufactured this sound? (DX7, LinnDrum, etc.)

When both answers converge with web-sourced production credits, production attribution confidence is HIGH.

---

## Synthesis Type Signatures

Before individual instruments, the broadest discriminator. These are the physics-level fingerprints that separate synthesis families.

### FM Synthesis (Yamaha DX series, 1983â€“)
- **Harmonic structure**: Non-integer partial ratios. Sidebands at (carrier Â± n Ã— modulator) frequencies. This is the fundamental identifier â€” no other common synthesis method produces these specific inharmonic relationships.
- **Spectral character**: Bright, metallic, "glassy." High-frequency energy that doesn't follow the natural harmonic series. Bell-like transients.
- **Envelope behavior**: Can produce very fast attacks (sub-1ms). Velocity-sensitive timbral change (not just volume) â€” harder hits produce more upper partials.
- **Telltale in a mix**: Occupies upper-mid to treble range with energy at frequencies that an acoustic instrument wouldn't have. Creates "shimmer" that sits above the fundamental in non-harmonic-series positions.
- **Distinguishing from**: Additive synthesis (which CAN produce similar spectra but with different phase relationships). Wavetable (which steps through spectra rather than generating them from operator math).

### Analog Subtractive (Moog, Prophet, Jupiter, Oberheim, 1960sâ€“)
- **Harmonic structure**: Integer-ratio harmonics from oscillators (saw = all harmonics, square = odd harmonics, pulse = variable). Filter shapes the spectrum by removing upper content.
- **Spectral character**: "Warm." Filter resonance creates a characteristic spectral peak that sweeps. Oscillator drift creates slight pitch instability (pre-MIDI) that thickens the sound.
- **Envelope behavior**: Filter envelope is the primary timbral shaper. Attack "opens" the filter, decay "closes" it. This creates a spectral contour that's fundamentally different from FM (where timbre is generated, not filtered).
- **Telltale in a mix**: Smooth spectral rolloff above the filter cutoff. Resonance peak at a specific frequency. Phase relationships between oscillators create beating/chorus naturally.
- **Distinguishing from**: FM (which has energy above where a filter would cut). Digital subtractive (which lacks oscillator drift and analog noise floor).

### Sample-Based Playback (Fairlight CMI, Emulator, Mellotron, 1970sâ€“)
- **Harmonic structure**: Inherits the spectrum of whatever was sampled. BUT: early samplers (8-bit, low sample rate) introduce aliasing artifacts â€” high-frequency fold-back that creates phantom harmonics.
- **Spectral character**: "Gritty" in early units (Fairlight CMI Series I/II = 8-bit, ~16kHz sample rate). The aliasing creates a characteristic "fizz" above the natural spectrum of the sampled source. Pitch-shifting introduces artifacts (time-stretching wasn't available, so transposing a sample changed its duration and timbre).
- **Envelope behavior**: Fixed envelope per sample. No velocity-responsive timbral change in early units. The "machine gun effect" â€” repeated triggering of the same sample with identical attack/decay.
- **Telltale in a mix**: Identical transient on every hit (no performance variation). Aliasing artifacts above ~8kHz in early units. Pitch-shifted versions of the same sample have different durations.
- **Distinguishing from**: Modern samplers (24-bit, high sample rate = transparent). Synthesized equivalents (which have parameter variation between notes).

### Wavetable (PPG Wave, 1981â€“)
- **Harmonic structure**: Cycles through pre-computed single-cycle waveforms. Transitions between waves create evolving spectra that are neither filtered (subtractive) nor generated (FM) â€” they're interpolated.
- **Spectral character**: "Digital but characterful." The PPG Wave specifically has a 12-bit DAC that creates quantization noise and a slightly gritty texture. Wavetable scanning produces spectral movement that sounds different from filter sweeps.
- **Envelope behavior**: Wavetable position can be envelope-controlled, creating timbral evolution. The PPG's high-pass click on note attack is a known characteristic.
- **Telltale in a mix**: Spectral evolution that doesn't follow filter-sweep patterns. Quantization artifacts in the PPG specifically. The "click" on attacks.
- **Distinguishing from**: FM (which has mathematical partial relationships). Subtractive (which has smooth filter curves). Modern wavetable (Serum, etc.) which lacks the 12-bit artifacts.

---

## Individual Instrument Entries

### YAMAHA DX7 (1983)

**Type**: FM synthesis (6 operators, 32 algorithms)
**Era signature**: 1983â€“1990 (ubiquitous). Factory presets dominated records because programming was notoriously difficult.
**Sample rate / bit depth**: 12-bit DAC, 49.1kHz internal rate

**Key spectral markers**:
- Bright upper partials from FM sidebands
- Velocity-sensitive timbre (more operators engage at higher velocity on many patches)
- Clean, precise envelopes (digital control)
- No oscillator drift (crystal-locked)

**Factory presets with known usage** (from EWTRTW article):

| Preset | ROM Bank | Character | Spectral Signature | Used In |
|--------|----------|-----------|-------------------|---------|
| GUITAR 3 | ROM1B #23 | Bright plucked string simulation | Fast attack, FM harmonics creating "pluck," rapid decay of upper partials | EWTRTW intro lick (layered w/ Vibe 2) |
| VIBE 2 | ROM2A #23 | Vibraphone-like bell tone | Sustained bell partials, slow decay, metallic shimmer | EWTRTW intro lick (layered w/ Guitar 3) |
| PIANO 5 | ROM1B #02 | Bright keyboard tone | Attack transient with inharmonic content, velocity-sensitive brightness | EWTRTW arpeggio (layered w/ Koto) |
| KOTO | ROM1A #23 | Plucked string, bright | Sharp attack, rapid high-frequency decay, resonant body | EWTRTW arpeggio (layered w/ Piano 5) |
| PIANO 1 | ROM1A #08 | Electric piano (Rhodes-like) | Smooth attack, warm bell character, moderate brightness | EWTRTW chord layer (panned 25L) |
| PIANO 4 | ROM1B #01 | Electric piano variant | Similar to Piano 1, slightly different operator balance | EWTRTW chord layer (panned 25R) |
| BASS 4 | ROM1B #32 | Punchy bass | Low-end weight, velocity-sensitive attack brightness, sub-heavy | EWTRTW bass layer (provides body, no click) |

**Layering patterns observed**: DX7 patches are frequently doubled (two presets playing the same MIDI sequence). This may indicate use of the Yamaha DX1 (dual DX7 engine in one unit). The doubling creates a composite timbre that can't be identified as a single preset â€” this is a Concealment mechanism at the instrument level.

**Binary detection heuristic**: Look for bright spectral content (centroid >2kHz) with non-integer harmonic relationships. FM sidebands create energy at frequencies that analog subtractive synths don't reach without distortion. The DX7's 12-bit DAC introduces very faint quantization noise below -60dB.

**Dictionary songs**: EWTRTW (confirmed, multiple presets)

---

### SEQUENTIAL PROPHET T-8 (1983)

**Type**: Analog subtractive (2 oscillators per voice, Curtis CEM chips, 8-voice polyphonic)
**Era signature**: 1983â€“1986 (limited production run, ~1000 units). Upgraded Prophet-5 with MIDI, velocity, aftertouch, weighted keys.
**Notable**: Roland Orzabal's stated favorite analog synth.

**Key spectral markers**:
- Dual pulse waves with PWM create thick, chorused texture
- Filter resonance (CEM 3320 lowpass) creates characteristic "vocal" quality at moderate settings
- Oscillator drift less pronounced than Prophet-5 (more stable) but still present
- Velocity sensitivity affects both filter and amplitude (unlike Prophet-5)

**Patches with known usage**:

| Patch | Character | Key Parameters | Used In |
|-------|-----------|---------------|---------|
| "Guitar approximation" chord | Rhythmic, pulse-wave based | Osc1 PW 31%, Osc2 PW 83%, cutoff 482Hz, res ~1, env amt 3.75, decay 3.27s, low sustain. LFO 6.82Hz for PWM (depth 0.172). 40% chorus. | EWTRTW main two-chord motif |
| Rhythmic single-note pulse | Percussive, short | Dual sawtooth, one octave apart. Filter decay 356ms, amp decay 393ms, amp release 147ms. Short note lengths. 50% chorus. Bass rolled off -5dB. | EWTRTW rhythmic part (1:05+) |

**Binary detection heuristic**: Pulse-wave PWM creates a characteristic spectral pattern: energy at odd harmonics that shifts as the pulse width modulates. The CEM filter has a specific slope (-24dB/oct) that's measurably different from Moog (-24dB/oct but different resonance behavior) or Roland (-24dB/oct IR3109). The Prophet-T8's weighted keyboard creates more dynamic velocity curves than other analog synths of the era.

**NOTE**: Our EWTRTW entry listed "Prophet 5" â€” should be corrected to "Prophet T-8." Different instrument with different capabilities (MIDI, velocity, aftertouch). The T-8's velocity sensitivity is production-relevant: it means the MIDI sequencing could include dynamic variation that the Prophet-5 couldn't respond to.

**Dictionary songs**: EWTRTW (confirmed)

---

### PPG WAVE 2.3 (1982)

**Type**: Wavetable synthesis (2 digital oscillators through analog filter)
**Era signature**: 1982â€“1987. Distinctive digital-analog hybrid sound.

**Key spectral markers**:
- 12-bit DAC quantization noise (characterful grit)
- Wavetable scanning creates non-filter-sweep spectral evolution
- High-pass "click" on note attacks (known PPG characteristic)
- Analog SSM 2044 filter adds warmth to digital oscillators

**Patches with known usage**:

| Patch | Character | Used In |
|-------|-----------|---------|
| Modified 013 A (bass) | High-end "clickiness," attack transient. Oscillator wave lowered, filter envelope depth reduced. | EWTRTW bass layer (provides click/attack, no body) |

**Binary detection heuristic**: The PPG's wavetable transitions create spectral "stepping" â€” discrete jumps between wave shapes rather than the smooth sweeps of analog filters. The 12-bit quantization is measurable as a noise floor artifact distinct from the DX7's own 12-bit signature (different DAC architecture). The attack click is a transient spike in the 2-5kHz range.

**Composite source note (EWTRTW)**: The bass sound is DX7 Bass 4 (body/low-end) + PPG Wave modified 013 A (attack/click). Neither component is complete alone. This is the **composite source signature** â€” each layer is spectrally incomplete, designed to only be legible as a blend.

**Dictionary songs**: EWTRTW (confirmed, bass layer)

---

### LINNDRUM LM-2 (1982)

**Type**: Sample-based drum machine (8-bit samples, ~28kHz)
**Era signature**: 1982â€“1986. The defining drum machine sound of mid-80s pop/rock.

**Key spectral markers**:
- 8-bit samples with characteristic aliasing artifacts
- Fixed samples per hit â€” no velocity layers, no round-robin
- Specific transient profiles per sound:
  - **Kick**: Short, punchy, moderate sub content. Less "boom" than 808, more "thud."
  - **Snare**: Crisp, bright, relatively thin body. Often layered with other sources.
  - **Hi-hat**: Distinctive metallic decay. 8-bit quantization creates characteristic "fizz" in the high end.
  - **Shaker**: Similar fizz. Shorter decay than hi-hat.

**Known usage (EWTRTW)**:
- Hi-hat: Syncopated pattern creating cross-rhythm with shaker
- Shaker: Steady triplet 1/8th notes (12/8 feel)
- Kick: Layered with Oberheim DMX kick (LinnDrum provides attack, DMX provides body â€” another composite source)

**Groove note**: The hi-hat/shaker cross-rhythm was inspired by Simple Minds' "Waterfront" and Linx's "Throw Away the Key."

**Binary detection heuristic**: LinnDrum samples have been extensively cataloged. The hi-hat in particular has a very specific spectral decay profile that's identifiable even in a dense mix. The 8-bit aliasing creates energy above the natural frequency content of the sampled cymbals. Zero velocity variation = identical transient on every hit (measurable as very low onset CV for the hi-hat/shaker layer).

**Dictionary songs**: EWTRTW (confirmed)

---

### OBERHEIM DMX (1981)

**Type**: Sample-based drum machine (8-bit, digitally stored analog drum samples)
**Era signature**: 1981â€“1985. Competitor to LinnDrum. Grittier, darker character.

**Key spectral markers**:
- Darker overall character than LinnDrum
- Kick drum: More low-end weight than LinnDrum, longer sustain
- Snare: Crunchier, more "real drum" character than LinnDrum
- 8-bit with specific Oberheim voicing characteristics

**Known usage (EWTRTW)**:
- Kick: Primary kick layer (body), layered with LinnDrum kick (attack). Possibly sampled into Fairlight CMI and sequenced from there (Ian Stanley, Manny Elias, and Chris Hughes all credited with DMX).
- General: DMX sounds may have been resampled through Fairlight for sequencing flexibility.

**Binary detection heuristic**: DMX kick has measurably more sub-bass energy (30-80Hz) than LinnDrum kick. The "resampled through Fairlight" pattern adds an additional layer of 8-bit character on top of the DMX's own 8-bit samples â€” double quantization creates a specific noise profile.

**Dictionary songs**: EWTRTW (confirmed, kick layer)


---

### E-MU DRUMULATOR (1983)

**Type**: Sample-based drum machine (8-bit, 28kHz sample rate, 12 sounds)
**Era signature**: 1983–1985. Budget competitor to LinnDrum ($995 vs $2995). Surprisingly capable. Used by Depeche Mode, New Order, Tears for Fears.
**Notable**: Could load custom EPROM chips with user samples. The Tears for Fears unit was loaded with samples from Led Zeppelin’s “When the Levee Breaks” (John Bonham’s drums recorded in Headley Grange stairwell by Andy Johns, 1971).

**Key spectral markers**:
- 8-bit samples with aliasing artifacts similar to LinnDrum
- No velocity sensitivity — one sample per pad, fixed dynamics
- “Rock Drums” EPROM set: Bonham samples carry the acoustic character of Headley Grange’s natural room reverb (long, dark, cavernous)
- When gated: the room sound blooms then cuts dead, creating the signature 1980s gated drum sound

**“Ghost” phenomenon (SHOUT)**:
The Drumulator loaded with Bonham samples creates a unique classification challenge: the MACHINE is digital/8-bit/grid-locked, but the SAMPLES are organic/acoustic/room-recorded. Equipment engine classified drums as “live_drums” at 100% confidence because it analyzed sample CHARACTER, not triggering METHOD. The machine is invisible to spectral analysis — only timing analysis (grid precision, random vs structured deviations, zero autocorrelation in drift) reveals the machine behind the organic sound.

**Known usage (SHOUT)**:
- Full drum pattern: kick, snare (gated), hi-hat (16th notes, strong-weak alternation)
- Grid-locked timing: CV 2.00%, deviation autocorrelation near zero (random not structured = machine)
- Gated snare on beats 2/4: Bonham room reverb blooms then gate slams shut
- Hi-hat: metronomic 16th notes as “clock” foundation

**Binary detection heuristic**: The Drumulator shares the LinnDrum's 8-bit aliasing signature but with different sample content. When loaded with acoustic drum samples, spectral analysis will identify the SOURCE (acoustic drums) not the MACHINE (digital trigger). Detection requires timing analysis: perfect grid adherence + zero autocorrelation in timing deviations + identical transient on every hit = machine-triggered samples, regardless of what the samples sound like.

**Spectral ceiling detection (VALIDATED 2026-02-09)**: The Drumulator's 8-bit/28kHz encoding creates a Nyquist ceiling at ~14kHz. In a full mix, this ceiling is cloaked by sustained synths providing 120-125% energy masking below 14kHz. Detected via two-axis subtraction: (1) MID channel isolation removes edge-panned percussion that extends above 14kHz, (2) synth bed subtraction removes sustained harmonic content estimated from 180ms pre-onset. After subtraction, the ceiling appears as a -3.8dB dip at 14kHz and a -69.9 dB/kHz cliff at 14,812 Hz (steeper than the MP3 encoding cliff). Implementation: `SpectralCeilingDetector.analyze_in_mix()` in `compression_engine.py`.

**Dictionary songs**: SHOUT (confirmed, Bonham samples)

**Cross-reference**: LinnDrum (similar architecture, different samples, different price point). Fairlight CMI (also sample-based, much more expensive, same era).


---

### FAIRLIGHT CMI (Series I/II/III, 1979â€“1985)

**Type**: Sampler / workstation. 8-bit (Series I/II), later 16-bit (Series III).
**Era signature**: 1979â€“1990. The prestige sampler. Extremely expensive (~$25,000â€“$60,000). Status instrument.

**Key spectral markers**:
- 8-bit sampling creates harsh aliasing artifacts (Series I/II)
- Low sample memory forces short samples â€” repeated triggering creates "machine gun effect"
- Pitch transposition changes sample duration (no time-stretching)
- Characteristic Page R (rhythm sequencer) timing feel

**Patches with known usage (EWTRTW)**:

| Sample | Character | Used In |
|--------|-----------|---------|
| OOHH1 | Soft, mellow choir | EWTRTW intro choir + bridge melody (primary layer) |
| CHOIR6 | Brighter choir, more high-end | EWTRTW choir layer (mixed -4dB below OOHH1, filtered at 2kHz, slightly detuned for thickness) |
| Palm-muted guitar (custom sample) | Short pluck, percussive | EWTRTW sampled guitar â€” palm-muted low D in triplet rhythm (verses). Recorded live, loaded into Fairlight, sequenced back. |
| Palm-muted power chord (custom sample) | Short chord stab | EWTRTW sampled guitar â€” choruses. Same sampling approach. |

**Production pattern note**: The Fairlight was used in EWTRTW both as a sample player (choir presets) AND as a sampling pipeline â€” recording live guitar into the Fairlight and sequencing it back as triggered samples. This creates a human-source-through-machine-playback character that's central to the Concealment bridge. Live guitars play alongside their own Fairlight-triggered ghosts.

**Binary detection heuristic**: 8-bit Fairlight samples have a characteristic noise floor and aliasing pattern. The "machine gun effect" (identical transient on every trigger) is measurable as near-zero variation in onset shape across repeated hits. Short sample memory means rapid decay or abrupt loop points â€” look for unnaturally identical note durations.

**Dictionary songs**: EWTRTW (confirmed, choir + sampled guitars + possibly resampled DMX kick)


---

### HAMMOND ORGAN (various models, 1935–)

**Type**: Electromechanical tonewheel organ
**Era signature**: Ubiquitous across jazz, blues, rock, gospel since 1950s. In 1980s synth-pop context, a deliberately anachronistic choice — a “real” keyboard instrument among synthesizers.

**Key spectral markers**:
- Tonewheel-generated harmonics at integer ratios (similar to additive synthesis)
- Drawbar settings create variable harmonic recipes (unique per performance/song)
- Leslie speaker cabinet (rotating horn + drum) creates characteristic tremolo/chorus modulation
- Key click on note attack (electromechanical contact bounce)
- Rich, dense harmonic content in mid-range (1–5 kHz)

**Known usage (SHOUT)**:
- Solo section: Hammond organ solo (confirmed from production credits)
- The most “organic” harmonic instrument in the arrangement — everything else is synthesized
- Contributes to mid-range density (72.5% mid-range energy in spectral balance)

**Binary detection heuristic**: Hammond’s tonewheel harmonics are perfectly integer-ratio (like analog oscillators) but with characteristic amplitude profiles per drawbar setting. Leslie modulation creates a specific frequency modulation pattern (~0.7Hz slow, ~6Hz fast) that’s distinguishable from electronic chorus. The key click occupies 2–4 kHz and is very fast (<5ms).

**Dictionary songs**: SHOUT (confirmed, solo section)


---

### ROLAND JC-120 (Jazz Chorus amplifier, 1975â€“present)

**Type**: Solid-state guitar amplifier with built-in stereo chorus
**Era signature**: 1975â€“present. Clean tone standard. Used by everyone from Andy Summers to Roland Orzabal.

**Key spectral markers**:
- Extremely clean headroom (solid-state, no tube saturation)
- Built-in chorus creates stereo width through LFO-modulated delay
- Bright, "glassy" clean tone â€” scooped mids compared to tube amps
- No harmonic distortion at normal volumes (fundamentally different from tube amp "warmth")

**Known usage (EWTRTW)**:
- Primary guitar amp for clean parts (Fender Stratocaster through JC-120)
- Chorus engaged for stereo width and movement
- Additional software chorus (Dimension D emulation) added in some parts

**Binary detection heuristic**: JC-120's solid-state character means guitar signal retains more of its original harmonic content (no tube compression/saturation). The built-in chorus has a specific LFO rate and depth that creates a measurable modulation pattern in the stereo field. Clean guitar through JC-120 occupies a specific spectral region (bright, mid-scooped) that's distinguishable from tube amp recordings.

**Dictionary songs**: EWTRTW (confirmed, guitar parts)

---

## Sequencing & Control

### UMI (Universal Musical Interface) on BBC Micro

**Type**: Software MIDI sequencer
**Era significance**: One of the first MIDI sequencers. Running on a BBC Micro (8-bit home computer).

**Production fingerprint**:
- Responsible for the 99.4% grid adherence in EWTRTW
- Limited quantization resolution (8-bit computer clock) may create micro-timing artifacts
- Chris Hughes: "I programmed those two chords and a bass line, and had that running on and off for days"
- The loop-as-composition method: a short sequence becomes the structural backbone

**Binary detection heuristic**: Early MIDI sequencers had quantization that was "perfect" but with specific timing resolution limitations. The UMI on BBC Micro would have clock resolution determined by the BBC Micro's interrupt timing. This creates a "grid" that's almost perfect but with specific periodic micro-deviations that differ from later sequencers (Atari ST, early Mac).

**Dictionary songs**: EWTRTW (confirmed, all sequenced parts)

---

## Production Techniques

### Composite Source Signature

**Definition**: A production technique where every audible element in a mix is actually two or more sources blended, with each component spectrally incomplete alone.

**EWTRTW instances**:
| Audible Sound | Component A | Component B | Why Neither Works Alone |
|---------------|-------------|-------------|------------------------|
| Intro lick | DX7 Guitar 3 | DX7 Vibe 2 | Guitar 3 = pluck without sustain. Vibe 2 = sustain without pluck. |
| Arpeggio | DX7 Piano 5 + guitar | DX7 Koto + guitar | DX7 provides FM shimmer, guitar provides organic body. |
| Main chords | Prophet T-8 patch | DX7 Piano 1 (25L) + Piano 4 (25R) | Prophet = analog warmth. DX7 = digital sparkle and stereo spread. |
| Bass | DX7 Bass 4 | PPG Wave mod 013A | DX7 = low-end body. PPG = upper attack/click. |
| Kick | Oberheim DMX | LinnDrum | DMX = body/sub. LinnDrum = attack/transient. |
| Choir | Fairlight OOHH1 | Fairlight CHOIR6 (filtered, detuned, -4dB) | OOHH1 = warmth. CHOIR6 = presence/air. |
| Rhythm guitar | Live Stratocaster/JC-120 | Fairlight-sampled palm mutes | Live = organic variation. Sampled = mechanical precision. |

**Significance for Concealment bridge**: When no single source can be isolated, the listener cannot point to "the synth" or "the guitar" â€” everything is a blend. This prevents analytical decomposition by the listener, which is literally what concealment means. The production technique performs the bridge type.

**Binary detection approach**: High stereo correlation (sources blended to center) + high spectral density in narrow bands (multiple sources occupying similar frequency ranges) + low timbral variation across repeated phrases (sequenced composite doesn't change). In EWTRTW: correlation 0.614 + width 0.198 + self-similarity 0.865 = composite source evidence.

### Single Reverb Bus

**Definition**: All tracks sent to one reverb unit at varying levels, rather than individual reverb per track.

**EWTRTW instance**: Valhalla VintageVerb "Small R-Hall" (or period equivalent â€” likely Lexicon 224 or AMS RMX16 given the era and studio).

**Significance**: Single reverb bus creates spatial coherence â€” all instruments appear to exist in the same room. This further obscures individual source identity (composite source + shared reverb = maximum blending). It also explains the narrow stereo width (0.198): individual panning is moderate, and the shared reverb pulls everything toward a common spatial center.

**Binary detection approach**: Reverb tail analysis. If all instruments share the same reverb, the reverb tail after transients will have a consistent decay profile regardless of which instrument triggered it. Different reverb per track would create varying decay profiles.

### Tape Speed-Up

**EWTRTW instance**: Final mix sped up on tape, resulting in 33 cents sharp of concert pitch.

**Significance**: Pre-digital pitch/speed coupling means the entire mix is slightly faster AND higher than recorded. This creates a subtle "lift" â€” everything is slightly brighter and more energetic than the performances as played. Another form of concealment: the song is not what was played.

**Binary detection approach**: Pitch analysis showing non-standard tuning (A â‰  440Hz). If A = ~448Hz (33 cents sharp), tape speed-up is likely. Combined with tempo that's slightly "too fast" for the groove feel.

---

## Backtrace Methodology (Proposed)

The long-term goal: given a binary audio file with no metadata, identify the equipment that produced it.

### Level 1: Synthesis Family (achievable now)
- FM vs analog subtractive vs sample-based vs wavetable
- Based on harmonic structure analysis (integer vs non-integer partials, filter curves, aliasing patterns)
- Confidence: MEDIUM-HIGH for isolated sounds, LOW for dense mixes

### Level 2: Instrument Family (achievable with reference library)
- DX7 vs DX1 vs TX816 (all FM, but different voicing)
- Prophet-5 vs Prophet T-8 vs OB-Xa (all analog, but different filter/oscillator chips)
- LinnDrum vs DMX vs TR-808 (all drum machines, but different sample sets)
- Requires spectral template matching against known instrument samples
- Confidence: MEDIUM for prominent parts, LOW for buried layers

### Level 3: Specific Patch (aspirational)
- Which DX7 factory preset?
- What filter settings on the Prophet?
- Requires detailed spectral analysis at the partial level
- Only feasible for relatively isolated sounds or when stems are available
- Confidence: LOW without stems, MEDIUM with stems

### Level 4: Production Technique (achievable now)
- Composite sourcing (multiple sources per audible element)
- Single vs multiple reverb buses
- Tape speed manipulation
- Sequencer timing signatures
- These leave measurable artifacts in the stereo field, dynamics, and micro-timing
- Confidence: MEDIUM-HIGH

### Priority for Dictionary Development
1. Catalog every instrument confirmed in dictionary songs (this file)
2. When analyzing new songs, attempt Level 1 identification before web search
3. Use web search to confirm/correct Level 1 guesses â†’ builds calibration data
4. Over time, refine to Level 2 as the reference library grows
5. Level 3 remains aspirational until stem separation improves

---

## Cross-References

### Equipment â†’ Dictionary Songs
| Equipment | Songs |
|-----------|-------|
| Yamaha DX7 | EWTRTW |
| Sequential Prophet T-8 | EWTRTW |
| PPG Wave 2.3 | EWTRTW |
| LinnDrum LM-2 | EWTRTW |
| Oberheim DMX | EWTRTW |
| Fairlight CMI | EWTRTW |
| Roland JC-120 | EWTRTW |
| UMI / BBC Micro | EWTRTW |
| E-mu Drumulator | SHOUT |
| Hammond Organ | SHOUT |

### Equipment â†’ Synthesis Type
| Equipment | Type |
|-----------|------|
| Yamaha DX7 / DX1 | FM |
| Sequential Prophet T-8 / Prophet-5 | Analog subtractive |
| PPG Wave 2.3 | Wavetable + analog filter |
| Moog (various) | Analog subtractive |
| Roland Jupiter-8 | Analog subtractive |
| Roland Juno-106 | Analog subtractive (DCO) |
| Oberheim OB-Xa / OB-8 | Analog subtractive |
| Fairlight CMI | Sample-based |
| E-mu Emulator | Sample-based |
| E-mu Drumulator | Sample-based drum machine |
| Mellotron | Tape-based playback |
| LinnDrum LM-2 | Sample-based drum machine |
| Oberheim DMX | Sample-based drum machine |
| Roland TR-808 | Analog drum machine |
| Roland TR-909 | Hybrid (analog + sample) drum machine |

*(Instruments not yet confirmed in dictionary songs are listed for future reference. Entries will be expanded as new songs are analyzed.)*

---

## Corrections to Dictionary

Based on the Reverb Machine article (Feb 2025):

### EWTRTW Entry 005 â€” Equipment Line
**Current**: `DX7, PPG Wave 2.3, LinnDrum LM-2, Fairlight CMI, Oberheim DMX, Prophet 5, Jupiter 8`
**Corrected**: `Yamaha DX7 (possibly DX1), PPG Wave 2.3, LinnDrum LM-2, Fairlight CMI, Oberheim DMX, Sequential Prophet T-8, UMI sequencer (BBC Micro)`

**Changes**:
- Prophet 5 â†’ **Prophet T-8** (confirmed by Orzabal interview in One Two Testing, Oct 1984)
- Jupiter 8 â†’ **removed** (no evidence in any referenced source)
- Added **UMI sequencer on BBC Micro** (confirmed by Chris Hughes, responsible for 99.4% grid)
- Added **"possibly DX1"** note (Behringer restored their unit, may predate SFTBC sessions)
- Added **detuning note**: track is 33 cents sharp from tape speed-up
