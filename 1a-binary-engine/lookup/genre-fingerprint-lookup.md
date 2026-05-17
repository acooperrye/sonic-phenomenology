# BINARY PASS 1: GENRE FINGERPRINT LOOKUP
## Rhythm Dictionary â€” What the Engine Checks First
## 2026-02-08

---

## HOW IT WORKS

Before any web search, before any thematic scoring, the engine reads a handful of binary measurements and matches against these fingerprints. This is Resolution 0â†’1 in the zoom architecture. A 10-second snapshot is enough.

**Primary discriminators** (cheapest to compute, highest genre separation):
- Tempo (BPM)
- Grid adherence (% quantized)
- RMS energy (mean dB)
- Dynamic range (dB)
- Spectral centroid (mean Hz)
- Band ratios (bass/mid/treble distribution)
- Onset density (events/sec)
- Spectral flatness (tonal vs noisy)
- ZCR (zero crossing rate)
- Silence ratio (%)
- IOI histogram shape (peaked vs flat, interval types)
- Polyphonic density (simultaneous voices/bands)
- Stereo width / correlation
- Crest factor (dB)
- Duration (seconds)

**Match process:** Score the waveform against all 20 fingerprints. Rank by match confidence. Top 1-3 matches become the genre hypothesis. Web search confirms or corrects.

---

## TIER 1: TIGHT FINGERPRINTS

### 1. AMBIENT
```
Tempo:            Absent or very slow (<80 BPM), often no detectable beat
Grid adherence:   N/A or low (no beat to quantize)
RMS energy:       Low (<-20 dB)
Dynamic range:    High (>15 dB)
Centroid:         Low-moderate (warm, not bright)
Band ratios:      Mid-frequency focus, gentle rolloff
Onset density:    Very low (<1 event/sec)
Spectral flatness: Low (tonal, not noisy)
ZCR:              Low (smooth)
Silence ratio:    Low (sustained tones fill space, but quietly)
IOI histogram:    Flat or absent (no regular rhythm)
Polyphonic density: Low-moderate (drones, pads)
Stereo:           Wide, stable correlation
Crest factor:     High (uncompressed)
Duration:         Long (>5 min typical)
```

### 2. NOISE
```
Tempo:            Absent or irrelevant
Grid adherence:   N/A
RMS energy:       High (>-8 dB)
Dynamic range:    Low (<6 dB)
Centroid:         Variable (depends on noise color)
Band ratios:      Flat (energy across all bands equally)
Onset density:    Very high or continuous
Spectral flatness: Very high (noise = flat spectrum)
ZCR:              Very high
Silence ratio:    Near 0%
IOI histogram:    Flat (no regular intervals)
Polyphonic density: Maximum (everything at once)
Stereo:           Variable, often low correlation (wide/chaotic)
Crest factor:     Very low (maximally compressed)
Duration:         Variable
```

### 3. REGGAE
```
Tempo:            60-90 BPM half-time / 120-160 BPM full
Grid adherence:   Moderate (human swing but steady)
RMS energy:       Moderate
Dynamic range:    Moderate (10-15 dB)
Centroid:         Low-moderate (warm, bass-forward)
Band ratios:      Bass-dominant, bass line carries melody
Onset density:    Moderate-low (space between events)
Spectral flatness: Low (tonal)
ZCR:              Low-moderate
Silence ratio:    Moderate (breathing room, offbeat gaps)
IOI histogram:    DISTINCTIVE â€” peaked at offbeat intervals, skank pattern
Polyphonic density: Low-moderate (bass, drums, skank guitar, keys)
Stereo:           Moderate width, dub variants wider
Crest factor:     Moderate
Duration:         3-5 min
```

### 4. CLASSICAL (Orchestral)
```
Tempo:            Variable (40-160+ BPM), often changes within piece
Grid adherence:   Low (no grid, conducted/performed)
RMS energy:       Variable (pianissimo to fortissimo)
Dynamic range:    Very high (>20 dB) â€” THE distinguishing feature
Centroid:         Variable
Band ratios:      Full, wide bandwidth (piccolo to double bass)
Onset density:    Variable
Spectral flatness: Very low (maximally tonal)
ZCR:              Low-moderate
Silence ratio:    Variable (rests are composed)
IOI histogram:    Complex (many interval types, rubato)
Polyphonic density: Variable (solo to full orchestra)
Stereo:           Wide (hall acoustics)
Crest factor:     Very high (uncompressed, natural dynamics)
Duration:         Long (>5 min, often >15 min for movements)
```

### 5. METAL (Heavy/Extreme)
```
Tempo:            Fast (120-220+ BPM), doom exception (40-80 BPM)
Grid adherence:   Moderate-high (tight but human)
RMS energy:       Very high (>-6 dB)
Dynamic range:    Low (<8 dB)
Centroid:         Low (dark, heavy)
Band ratios:      Bass-dominant, low-mid heavy, scooped mids common
Onset density:    Very high (double bass drums, fast riffs)
Spectral flatness: High (distortion adds harmonics across spectrum)
ZCR:              Very high (distortion)
Silence ratio:    Very low (<2%)
IOI histogram:    Peaked (locked to riff pattern)
Polyphonic density: High (layered guitars, dense arrangement)
Stereo:           Wide (dual guitars panned)
Crest factor:     Very low (compressed/loud)
Duration:         3-7 min, prog exception (>10 min)
```

### 6. PUNK
```
Tempo:            Fast (150-200+ BPM)
Grid adherence:   Low-moderate (human, rushed, imprecise)
RMS energy:       High (>-8 dB)
Dynamic range:    Low (<6 dB)
Centroid:         Moderate-high (midrange focus, bright distortion)
Band ratios:      Midrange-dominant (not bass-heavy like metal)
Onset density:    High (fast strumming, fast drums)
Spectral flatness: High (distortion)
ZCR:              High
Silence ratio:    Very low
IOI histogram:    Very peaked, few interval types (simple patterns)
Polyphonic density: Low (guitar, bass, drums â€” sparse arrangement)
Stereo:           Narrow (lo-fi, often mono-adjacent)
Crest factor:     Very low
Duration:         Short (<3 min typical)
```

### 7. INDUSTRIAL
```
Tempo:            Moderate-fast (100-150 BPM)
Grid adherence:   Very high (>90%) â€” mechanically quantized
RMS energy:       High
Dynamic range:    Low (<8 dB)
Centroid:         Low-moderate (dark, cold)
Band ratios:      Low-mid heavy, metallic texture
Onset density:    High (machine repetition)
Spectral flatness: Moderate-high (noise elements + tonal)
ZCR:              Moderate-high
Silence ratio:    Low
IOI histogram:    Very peaked (one interval dominates â€” machine pulse)
Polyphonic density: Moderate (layered but not orchestral)
Stereo:           Moderate
Crest factor:     Low (compressed)
Duration:         4-6 min
```

### 8. SHOEGAZE
```
Tempo:            Moderate (80-130 BPM)
Grid adherence:   Moderate (buried under texture)
RMS energy:       Moderate-high
Dynamic range:    Low-moderate (<10 dB) â€” wall dynamics
Centroid:         Moderate (not bright, not dark â€” diffuse)
Band ratios:      Full, emphasis on mid-frequency guitar wash
Onset density:    Moderate (buried under sustained texture)
Spectral flatness: Moderate (tonal + noise from distortion/reverb)
ZCR:              Moderate
Silence ratio:    Very low (wall of sound fills everything)
IOI histogram:    Moderate â€” rhythm present but submerged
Polyphonic density: Very high (layered guitars, reverb tails, vocal layers)
Stereo:           Very wide, low correlation (maximum diffusion)
Crest factor:     Low (wall = sustained level)
Duration:         4-7 min
```

### 9. BOSSA NOVA
```
Tempo:            Moderate (100-140 BPM)
Grid adherence:   Moderate (human, relaxed feel)
RMS energy:       Low (<-18 dB)
Dynamic range:    High (>15 dB)
Centroid:         Moderate-warm (nylon guitar, voice)
Band ratios:      Midrange-dominant, no sub-bass emphasis
Onset density:    Low (sparse, space between notes)
Spectral flatness: Very low (maximally tonal â€” clean acoustic)
ZCR:              Low
Silence ratio:    Moderate (space between phrases)
IOI histogram:    DISTINCTIVE â€” syncopated bossa pattern, specific offbeat feel
Polyphonic density: Very low (voice + guitar, maybe bass/percussion)
Stereo:           Narrow (intimate, close-mic'd)
Crest factor:     High (uncompressed, natural dynamics)
Duration:         3-5 min
```

---

## TIER 2: MEDIUM FINGERPRINTS

### 10. JAZZ
```
Tempo:            Wide range (40-300+ BPM)
Grid adherence:   Low (<50%) â€” SWING, human timing, improvisation
RMS energy:       Variable
Dynamic range:    High (>15 dB)
Centroid:         Variable
Band ratios:      Variable, but acoustic instrument signatures
Onset density:    Variable (ballad low, bebop very high)
Spectral flatness: Low (tonal, acoustic)
ZCR:              Low-moderate
Silence ratio:    Variable
IOI histogram:    Complex â€” many interval types, swing distribution
Polyphonic density: Variable (trio to big band)
Stereo:           Variable
Crest factor:     High (uncompressed, natural)
Duration:         Variable (3-15+ min)

DISCRIMINATOR: Grid adherence LOW + spectral flatness LOW + dynamic range HIGH + IOI histogram COMPLEX
= human timing + tonal content + breathing dynamics + rhythmic variety
```

### 11. BLUES
```
Tempo:            Moderate (60-130 BPM)
Grid adherence:   Low-moderate (human, shuffle feel)
RMS energy:       Moderate
Dynamic range:    Moderate-high (12-18 dB)
Centroid:         Moderate-low (warm)
Band ratios:      Midrange-dominant, moderate bass
Onset density:    Low-moderate
Spectral flatness: Low-moderate (some distortion in electric blues)
ZCR:              Low-moderate
Silence ratio:    Moderate
IOI histogram:    DISTINCTIVE â€” shuffle/triplet pattern, 12-bar regularity
Polyphonic density: Low (small ensemble)
Stereo:           Narrow-moderate
Crest factor:     Moderate-high
Duration:         3-6 min

DISCRIMINATOR: Self-similarity matrix shows 12-bar regularity
+ shuffle IOI pattern + organic timing + moderate everything else
```

### 12. GOSPEL
```
Tempo:            Moderate (70-130 BPM)
Grid adherence:   Low-moderate (human, congregational)
RMS energy:       Moderate-high
Dynamic range:    High (>15 dB) â€” builds from quiet to exultation
Centroid:         Moderate (warm but not dark)
Band ratios:      Full, organ/piano low-end, vocal high-end
Onset density:    Moderate-high (call and response, overlapping vocals)
Spectral flatness: Low (tonal, harmonic)
ZCR:              Low-moderate
Silence ratio:    Low (voices fill space)
IOI histogram:    Moderate complexity
Polyphonic density: Very high â€” THE discriminator (choir)
Stereo:           Wide (room acoustics, choir spread)
Crest factor:     High (natural dynamics)
Duration:         4-8+ min

DISCRIMINATOR: Polyphonic density VERY HIGH + dynamic range HIGH
+ building loudness curve + organic timing + tonal content
```

### 13. POST-ROCK
```
Tempo:            Slow-moderate (60-140 BPM)
Grid adherence:   Moderate (band playing together, not sequenced)
RMS energy:       TRAJECTORY â€” starts low, ends high
Dynamic range:    Very high (>20 dB) â€” whisper to explosion
Centroid:         TRAJECTORY â€” travels from low to high
Band ratios:      TRAJECTORY â€” starts mid-focused, fills spectrum at climax
Onset density:    TRAJECTORY â€” increases through piece
Spectral flatness: TRAJECTORY â€” clean start, distorted climax
ZCR:              TRAJECTORY â€” follows distortion arc
Silence ratio:    TRAJECTORY â€” decreases (fills up)
IOI histogram:    Moderate
Polyphonic density: TRAJECTORY â€” solo to full ensemble
Stereo:           TRAJECTORY â€” narrows to wide
Crest factor:     TRAJECTORY â€” high at start (dynamic), low at climax (wall)
Duration:         Long (>6 min, often >10 min)

DISCRIMINATOR: EVERYTHING IS TRAJECTORY. The fingerprint isn't any single
value â€” it's that most measurements have a monotonic trajectory. If >8
primary discriminators show monotonic change across the song, strongly
suspect post-rock.
```

### 14. FOLK / ACOUSTIC
```
Tempo:            Moderate (80-140 BPM)
Grid adherence:   Low (human, no grid)
RMS energy:       Low (<-16 dB)
Dynamic range:    High (>15 dB)
Centroid:         Moderate (acoustic guitar midrange focus)
Band ratios:      Midrange-dominant, no sub-bass, gentle treble
Onset density:    Low (fingerpicking or strumming, sparse)
Spectral flatness: Very low (maximally tonal, clean acoustic)
ZCR:              Low
Silence ratio:    Moderate (natural phrasing)
IOI histogram:    Moderate â€” strumming pattern, simple
Polyphonic density: Very low (voice + guitar)
Stereo:           Narrow (close-mic, intimate)
Crest factor:     High (no compression)
Duration:         3-5 min

DISCRIMINATOR: Spectral flatness VERY LOW + polyphonic density VERY LOW
+ grid adherence LOW + RMS LOW + crest factor HIGH + stereo NARROW
= acoustic, sparse, human, quiet, uncompressed, intimate
```

### 15. SYNTH-POP / NEW WAVE
```
Tempo:            Moderate-fast (100-140 BPM)
Grid adherence:   Very high (>85%) â€” sequenced
RMS energy:       Moderate-high
Dynamic range:    Moderate (8-12 dB)
Centroid:         High (bright â€” DX7/digital synth shimmer)
Band ratios:      Treble-forward, bright top end
Onset density:    Moderate (sequenced patterns, not dense)
Spectral flatness: Low-moderate (tonal synths, not noisy)
ZCR:              Moderate
Silence ratio:    Low-moderate (gated reverb creates rhythmic space)
IOI histogram:    Peaked (sequenced, regular)
Polyphonic density: Moderate (synth pads + bass + drums + vocal)
Stereo:           Moderate-wide (80s production, chorus/delay effects)
Crest factor:     Moderate
Duration:         3-5 min

DISCRIMINATOR: Grid adherence VERY HIGH + centroid HIGH + spectral flatness LOW
= mechanical timing + bright + tonal (not noisy)
Distinguishes from industrial (which is mechanical + dark + noisy)
```

### 16. EDM / DANCE
```
Tempo:            120-150 BPM (house/techno), 70-80 BPM half-time (dubstep/trap)
Grid adherence:   Very high (>95%) â€” maximally quantized
RMS energy:       High (>-8 dB)
Dynamic range:    Low (<8 dB)
Centroid:         Variable by subgenre
Band ratios:      Kick-dominant, sub-bass heavy
Onset density:    High (programmed, fills)
Spectral flatness: Variable by subgenre
ZCR:              Variable
Silence ratio:    Low (but sidechain creates rhythmic ducking)
IOI histogram:    Very peaked â€” 4-on-the-floor kick pattern
Polyphonic density: Moderate-high
Stereo:           Wide (production depth)
Crest factor:     Low (loud mastering)
Duration:         5-8 min

DISCRIMINATOR: Grid adherence VERY HIGH + IOI peaked (4-on-the-floor)
+ RMS HIGH + crest factor LOW + duration >5 min
Build-drop pattern in loudness curve is subgenre-confirming
```

### 17. HIP-HOP / RAP
```
Tempo:            60-100 BPM (half-time feel common), trap 130-170 BPM
Grid adherence:   High (>80%) â€” programmed beat
RMS energy:       Moderate-high
Dynamic range:    Low-moderate (8-12 dB)
Centroid:         Low-moderate (808 bass dominance pulls it down)
Band ratios:      VERY bass-dominant (808 kick/sub-bass)
Onset density:    Moderate â€” SPLIT: beat is sparse, vocal flow adds density
Spectral flatness: Low-moderate
ZCR:              Low-moderate
Silence ratio:    Low
IOI histogram:    Moderate â€” trap has distinctive hi-hat pattern (rapid, variable)
Polyphonic density: Low-moderate (beat + voice, not dense arrangement)
Stereo:           Moderate
Crest factor:     Low-moderate
Duration:         3-4 min

DISCRIMINATOR: Band ratios VERY bass-dominant + grid adherence HIGH
+ polyphonic density LOW-MODERATE
= big bass + mechanical beat + sparse arrangement (the beat + voice structure)
Trap discriminator: rapid hi-hat IOI pattern (32nd/64th note bursts)
```

---

## TIER 3: WIDE FINGERPRINTS

### 18. R&B / SOUL
```
Tempo:            60-120 BPM
Grid adherence:   Moderate-high (groove-based, modern R&B quantized)
RMS energy:       Moderate
Dynamic range:    Moderate (10-15 dB)
Centroid:         Moderate-warm
Band ratios:      Bass-present, warm mid, not treble-forward
Onset density:    Low-moderate
Spectral flatness: Low (tonal, voice-dominant)
ZCR:              Low
Silence ratio:    Low-moderate
IOI histogram:    Moderate â€” groove pattern
Polyphonic density: Low-moderate (voice + production)
Stereo:           Moderate
Crest factor:     Moderate
Duration:         3-5 min

WEAK DISCRIMINATOR: Most values moderate. Voice characteristics (HNR,
F0 trajectory, melisma) would be primary discriminator but engine is
degraded on these. R&B may require web-confirmation more than any other genre.
```

### 19. COUNTRY
```
Tempo:            80-140 BPM
Grid adherence:   SPLIT â€” traditional: low (human). Modern: high (programmed)
RMS energy:       SPLIT â€” traditional: moderate. Modern: high
Dynamic range:    SPLIT â€” traditional: high. Modern: low
Centroid:         Moderate
Band ratios:      SPLIT â€” traditional: midrange. Modern: bass-heavy
Onset density:    Moderate
Spectral flatness: SPLIT â€” traditional: very low. Modern: low-moderate
ZCR:              Low-moderate
Silence ratio:    Low-moderate
IOI histogram:    Moderate
Polyphonic density: Low-moderate
Stereo:           SPLIT â€” traditional: narrow. Modern: wide
Crest factor:     SPLIT â€” traditional: high. Modern: low
Duration:         3-5 min

WEAK DISCRIMINATOR: The genre SPLITS on the AGENCY axis. Traditional and
modern country produce nearly opposite binary fingerprints on 6+ discriminators.
Binary alone may classify traditional country as folk and modern country as pop.
Web confirmation essential.
```

### 20. POP
```
Tempo:            90-140 BPM (widest comfortable dance range)
Grid adherence:   Moderate-high (usually programmed or quantized)
RMS energy:       High (loud mastering)
Dynamic range:    Low-moderate (8-12 dB, loudness-war era)
Centroid:         Moderate-bright
Band ratios:      Balanced (no single band dominates â€” full spectrum)
Onset density:    Moderate
Spectral flatness: Low (tonal, melodic)
ZCR:              Low-moderate
Silence ratio:    Low
IOI histogram:    Peaked (regular rhythm, hook-oriented)
Polyphonic density: Moderate
Stereo:           Moderate-wide
Crest factor:     Low-moderate
Duration:         3-4 min

WEAK DISCRIMINATOR: Pop is MODERATE ON EVERYTHING. The fingerprint is the
ABSENCE of extremes. If no primary discriminator is in an extreme zone,
and the song has:
  - Spectral flatness LOW (tonal/melodic â€” the hook constraint)
  - Duration 3-4 min (format constraint)
  - Some form of repetition in self-similarity (hook return constraint)
...then pop is the default hypothesis.

Pop is the genre you get when nothing else matches strongly.
```

---

## MATCH PRIORITY

When the engine reads a new waveform, check discriminators in this order (cheapest computation first, highest separation power first):

### Step 1: Instant kills (single discriminator rules out genres)
```
Dynamic range > 20 dB       â†’ NOT: metal, punk, industrial, EDM, noise
Dynamic range < 6 dB        â†’ NOT: classical, jazz, folk, bossa nova, ambient
Spectral flatness > 0.7     â†’ Noise or heavily distorted (metal/punk/industrial)
Spectral flatness < 0.1     â†’ Acoustic/tonal (classical, folk, bossa nova, jazz)
Onset density < 0.5/sec     â†’ NOT: metal, punk, EDM, noise, industrial
Grid adherence > 90%        â†’ Electronic/programmed (EDM, industrial, synth-pop)
Grid adherence < 30%        â†’ Human/performed (jazz, classical, folk, blues)
RMS < -20 dB                â†’ Ambient or folk/classical quiet passage
Duration > 10 min           â†’ Classical, post-rock, ambient, or jam
Polyphonic density > 60     â†’ Gospel, orchestral, or NTLTC-style vocal stacking
```

### Step 2: Cluster identification
```
HIGH energy + HIGH roughness + LOW centroid = Heavy (metal, industrial)
HIGH energy + HIGH roughness + HIGH centroid = Aggressive (punk)
HIGH energy + LOW roughness + HIGH grid = Dance (EDM, synth-pop)
LOW energy + LOW density + LOW roughness = Gentle (ambient, folk, bossa nova)
LOW grid + HIGH dynamic range + LOW flatness = Acoustic/performed (jazz, classical, folk, blues)
TRAJECTORY on >8 discriminators = Post-rock
```

### Step 3: Fine discrimination within clusters
```
Heavy cluster:
  Grid adherence > 85% â†’ Industrial
  Grid adherence < 85% + polyphonic density HIGH â†’ Metal
  
Gentle cluster:
  Onset density near 0 + duration long â†’ Ambient
  IOI syncopated pattern â†’ Bossa nova
  Polyphonic density very low + narrow stereo â†’ Folk

Acoustic cluster:
  Dynamic range > 20 dB + wide stereo â†’ Classical
  12-bar self-similarity + shuffle IOI â†’ Blues
  Complex IOI + swing feel â†’ Jazz

Dance cluster:
  Centroid HIGH + moderate tempo â†’ Synth-pop
  4-on-the-floor IOI + bass-dominant â†’ EDM
```

### Step 4: Default
```
If no strong match after Steps 1-3:
  Check pop constraints (tonal + repetitive + 3-4 min)
  If yes â†’ Pop (default hypothesis, web-confirm)
  If no â†’ Unknown (web-search required for genre ID)
```

---

## CONFIDENCE LEVELS

| Match Type | Confidence | Action |
|------------|-----------|--------|
| Tier 1 genre, 4+ discriminators match | HIGH (>85%) | Load baseline, proceed to thematic scoring |
| Tier 1 genre, 2-3 discriminators match | MEDIUM (60-85%) | Load baseline tentatively, web-confirm |
| Tier 2 genre match | MEDIUM (50-75%) | Load baseline, web-confirm, check subgenre |
| Tier 3 genre match | LOW (<50%) | Web-confirm before loading any baseline |
| Multiple genres match equally | SPLIT | Report top 2-3, web-confirm, may be genre-blending |
| No strong match | UNKNOWN | Web-search essential, may be novel or cross-genre |

---

## WHAT THIS GIVES THE ENGINE

The engine can now:
1. Read ~15 binary measurements from a 10-second snapshot
2. Match against 20 genre fingerprints
3. Identify the most likely genre with confidence level
4. Load that genre's 10-dimension baseline
5. Compare the song's structural readings against the baseline
6. Everything within baseline = water (suppress Ã—0.2)
7. Everything outside baseline = signal (amplify Ã—2.0)
8. Web search confirms or corrects the genre hypothesis
9. Thematic scoring proceeds against the correct baseline

The first binary pass doesn't interpret. It just establishes what's normal so the engine knows what's interesting.
