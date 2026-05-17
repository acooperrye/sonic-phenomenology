# EQUIPMENT REGISTRY (CONSOLIDATED)
## All Instruments · 30-Axis Grid · Canonical Values + Gap Thresholds
## 11 instruments · 51 voices · 1530 data points
## 2026-02-10

---

## §1 — Measurement Grid (30 Axes)

Every voice is plotted on the same 30-axis coordinate system. Left channel (WAV) = canonical value. Right channel = gap threshold (maximum permissible deviation before misidentification).

| # | Axis | Unit | Range | Scale | Group |
|---|------|------|-------|-------|-------|
| 1 | Fundamental Frequency | Hz | 20–20000 | log | Frequency |
| 2 | Spectral Centroid | Hz | 20–20000 | log | Frequency |
| 3 | Bandwidth (-3dB) | Hz | 1–20000 | log | Frequency |
| 4 | H2 Relative Level | dB | -80–0 | lin | Frequency |
| 5 | H3 Relative Level | dB | -80–0 | lin | Frequency |
| 6 | H4 Relative Level | dB | -80–0 | lin | Frequency |
| 7 | Highest Significant Harmonic | # | 1–64 | log | Frequency |
| 8 | Inharmonicity Index | 0-1 | 0–1 | lin | Frequency |
| 9 | Attack Time | ms | 0.1–1000 | log | Time |
| 10 | Decay Time | ms | 1–10000 | log | Time |
| 11 | Sustain Level | 0-1 | 0–1 | lin | Time |
| 12 | Pitch Sweep Range | Hz | 0–5000 | lin | Time |
| 13 | Pitch Sweep Duration | ms | 0.1–100 | log | Time |
| 14 | Pitch Droop Rate | c/s | 0–50 | lin | Time |
| 15 | Envelope Shape Code | code | 0–1 | lin | Time |
| 16 | Noise-to-Tone Ratio | 0-1 | 0–1 | lin | Noise/Artifact |
| 17 | Effective Bit Depth | bits | 1–24 | lin | Noise/Artifact |
| 18 | Sample Rate Ceiling | Hz | 4000–48000 | lin | Noise/Artifact |
| 19 | Quantization Noise Floor | dB | -96–0 | lin | Noise/Artifact |
| 20 | Aliasing Severity | 0-1 | 0–1 | lin | Noise/Artifact |
| 21 | Oscillator Count | # | 0–8 | lin | Topology |
| 22 | Oscillator Type Code | code | 0–1 | lin | Topology |
| 23 | Filter Type Code | code | 0–1 | lin | Topology |
| 24 | Filter Cutoff | Hz | 20–20000 | log | Topology |
| 25 | Filter Q | Q | 0.1–30 | log | Topology |
| 26 | Waveshaping Severity | 0-1 | 0–1 | lin | Topology |
| 27 | Velocity Sensitivity | 0-1 | 0–1 | lin | Behavioral |
| 28 | Unit Variance | 0-1 | 0–1 | lin | Behavioral |
| 29 | Choke Linkage Code | code | 0–1 | lin | Behavioral |
| 30 | Unique ID Confidence | 0-1 | 0–1 | lin | Behavioral |

### Axis Code Values

**Envelope Shape (Axis 15):** Exponential decay=0.2, Linear=0.4, Multi-burst=0.6, ADSR=0.8, Sample-fixed=1.0

**Oscillator Type (Axis 22):** Sine=0.125, Triangle=0.25, Sawtooth=0.375, Square=0.5, Pulse(variable)=0.625, Noise=0.75, PCM sample=0.875

**Filter Type (Axis 23):** None=0.0, LPF=0.25, HPF=0.5, BPF=0.75, Notch=1.0

**Confidence:** ● = measured/confirmed from circuit docs or spec sheets, ○ = estimated from qualitative descriptions, — = unknown/not applicable

---

## §2 — WAV Layout Map

```
Stereo WAV, 44100 Hz, 16-bit
Left channel:  canonical normalized value (0.0–1.0 → 0–32767)
Right channel: gap threshold (0.0–1.0 → 0–32767)

Lateral layout: each instrument gets a header cycle → voice cycles → transition cycle
Each cycle = 30 samples (one per axis)
Header cycle: alternating 0/max pattern (visual 'buzz' marker)
Transition cycle: ascending ramp 0→max (visual 'slope' marker)

Cycles   0– 16: TR-808 (15 voices)
Cycles  17– 29: TR-909 (11 voices)
Cycles  30– 38: DX7 (7 voices)
Cycles  39– 42: Prophet-T8 (2 voices)
Cycles  43– 45: PPG-Wave (1 voices)
Cycles  46– 51: LinnDrum (4 voices)
Cycles  52– 55: DMX (2 voices)
Cycles  56– 60: Drumulator (3 voices)
Cycles  61– 66: Fairlight (4 voices)
Cycles  67– 69: Hammond (1 voices)
Cycles  70– 72: JC-120 (1 voices)

Total: 73 cycles × 30 samples = 2190 samples
```

---

## §3 — Roland TR-808 Rhythm Composer

**Year:** 1980–1983  
**Type:** Fully analog drum synthesizer  
**Synthesis:** Bridged-T oscillators, noise generators, Schmitt-trigger square wave oscillators  
**Key Component:** 2SC828-R transistor (out-of-spec, responsible for sizzle)  
**Units Produced:** ~12,000  
**Dictionary Songs:** EWTRTW (layered kick), SHOUT (not present)  

### TR-808–BD: Bass Drum

**Circuit:** Bridged-T bandpass → self-oscillation → pitch sweep → feedback decay → tone LPF → VCA  
**Key Discriminator:** Pitch droop (voltage leakage in retrigger circuit) — smoking gun for real analog 808

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 49.4 | 0.131 | ±0.35 | ● |
| 2 | Spectral Centroid | 55 | 0.146 | ±0.15 | ○ |
| 3 | Bandwidth (-3dB) | 30 | 0.343 | ±0.20 | ○ |
| 4 | H2 Relative Level | -30 | 0.625 | ±0.30 | ● |
| 5 | H3 Relative Level | -40 | 0.500 | ±0.25 | ○ |
| 6 | H4 Relative Level | -50 | 0.375 | ±0.25 | ○ |
| 7 | Highest Significant Harmonic | 2 | 0.167 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.0 | 0.000 | ±0.05 | ● |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ● |
| 10 | Decay Time | 300 | 0.619 | ±0.40 | ● |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 80 | 0.016 | ±0.20 | ● |
| 13 | Pitch Sweep Duration | 6 | 0.593 | ±0.15 | ● |
| 14 | Pitch Droop Rate | 3 | 0.060 | ±0.10 | ● |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.05 | 0.050 | ±0.05 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 1 | 0.125 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.05 | ● |
| 23 | Filter Type Code | 0.25 | 0.250 | ±0.00 | ● |
| 24 | Filter Cutoff | 1.0k | 0.566 | ±0.30 | ○ |
| 25 | Filter Q | 0.7 | 0.341 | ±0.20 | ○ |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.85 | 0.850 | ±0.00 | ● |

### TR-808–SD: Snare Drum

**Circuit:** Two bridged-T oscillators (180Hz + 330Hz inharmonic) + white noise HPF → mixed  
**Key Discriminator:** Dual inharmonic tones at fixed pitches + HPF noise — no tuning control

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 180 | 0.318 | ±0.10 | ● |
| 2 | Spectral Centroid | 400 | 0.434 | ±0.20 | ○ |
| 3 | Bandwidth (-3dB) | 2.0k | 0.767 | ±0.25 | ○ |
| 4 | H2 Relative Level | -6 | 0.925 | ±0.15 | ● |
| 5 | H3 Relative Level | -15 | 0.812 | ±0.20 | ○ |
| 6 | H4 Relative Level | -25 | 0.688 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 8 | 0.500 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.7 | 0.700 | ±0.10 | ● |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ● |
| 10 | Decay Time | 200 | 0.575 | ±0.25 | ● |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.5 | 0.500 | ±0.30 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 2 | 0.250 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.05 | ● |
| 23 | Filter Type Code | 0.5 | 0.500 | ±0.10 | ● |
| 24 | Filter Cutoff | 1.0k | 0.566 | ±0.25 | ○ |
| 25 | Filter Q | 1.0 | 0.404 | ±0.20 | ○ |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.65 | 0.650 | ±0.00 | ○ |

### TR-808–LT: Low Tom

**Circuit:** Bridged-T oscillator (BD architecture, higher pitch)  
**Key Discriminator:** Miniature BD — sine wave, pitch-swept attack, 'boingy' character

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 100 | 0.233 | ±0.15 | ● |
| 2 | Spectral Centroid | 110 | 0.247 | ±0.15 | ○ |
| 3 | Bandwidth (-3dB) | 40 | 0.372 | ±0.20 | ○ |
| 4 | H2 Relative Level | -25 | 0.688 | ±0.20 | ○ |
| 5 | H3 Relative Level | -40 | 0.500 | ±0.20 | ○ |
| 6 | H4 Relative Level | -55 | 0.312 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 2 | 0.167 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.0 | 0.000 | ±0.05 | ● |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ● |
| 10 | Decay Time | 150 | 0.544 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 40 | 0.008 | ±0.15 | ○ |
| 13 | Pitch Sweep Duration | 4 | 0.534 | ±0.10 | ○ |
| 14 | Pitch Droop Rate | 1 | 0.020 | ±0.05 | ○ |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.02 | 0.020 | ±0.03 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 1 | 0.125 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.05 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.5 | 0.500 | ±0.00 | ○ |

### TR-808–MT: Mid Tom

**Circuit:** Bridged-T oscillator (BD architecture, higher pitch)  
**Key Discriminator:** Same as LT, higher pitch

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 150 | 0.292 | ±0.15 | ● |
| 2 | Spectral Centroid | 160 | 0.301 | ±0.15 | ○ |
| 3 | Bandwidth (-3dB) | 40 | 0.372 | ±0.20 | ○ |
| 4 | H2 Relative Level | -25 | 0.688 | ±0.20 | ○ |
| 5 | H3 Relative Level | -40 | 0.500 | ±0.20 | ○ |
| 6 | H4 Relative Level | -55 | 0.312 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 2 | 0.167 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.0 | 0.000 | ±0.05 | ● |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ● |
| 10 | Decay Time | 120 | 0.520 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 50 | 0.010 | ±0.15 | ○ |
| 13 | Pitch Sweep Duration | 4 | 0.534 | ±0.10 | ○ |
| 14 | Pitch Droop Rate | 1 | 0.020 | ±0.05 | ○ |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.02 | 0.020 | ±0.03 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 1 | 0.125 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.05 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.5 | 0.500 | ±0.00 | ○ |

### TR-808–HT: High Tom

**Circuit:** Bridged-T oscillator (BD architecture, highest tom pitch)  
**Key Discriminator:** Same as LT/MT, highest pitch

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 200 | 0.333 | ±0.15 | ● |
| 2 | Spectral Centroid | 215 | 0.344 | ±0.15 | ○ |
| 3 | Bandwidth (-3dB) | 45 | 0.384 | ±0.20 | ○ |
| 4 | H2 Relative Level | -25 | 0.688 | ±0.20 | ○ |
| 5 | H3 Relative Level | -40 | 0.500 | ±0.20 | ○ |
| 6 | H4 Relative Level | -55 | 0.312 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 2 | 0.167 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.0 | 0.000 | ±0.05 | ● |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ● |
| 10 | Decay Time | 100 | 0.500 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 60 | 0.012 | ±0.15 | ○ |
| 13 | Pitch Sweep Duration | 3 | 0.492 | ±0.10 | ○ |
| 14 | Pitch Droop Rate | 1 | 0.020 | ±0.05 | ○ |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.02 | 0.020 | ±0.03 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 1 | 0.125 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.05 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.5 | 0.500 | ±0.00 | ○ |

### TR-808–LC: Low Conga

**Circuit:** Bridged-T (shared circuit with toms, tighter envelope)  
**Key Discriminator:** Tighter, more percussive envelope than toms — still sine-based

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 170 | 0.310 | ±0.10 | ● |
| 2 | Spectral Centroid | 180 | 0.318 | ±0.10 | ○ |
| 3 | Bandwidth (-3dB) | 35 | 0.359 | ±0.20 | ○ |
| 4 | H2 Relative Level | -28 | 0.650 | ±0.20 | ○ |
| 5 | H3 Relative Level | -42 | 0.475 | ±0.20 | ○ |
| 6 | H4 Relative Level | -55 | 0.312 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 2 | 0.167 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.0 | 0.000 | ±0.05 | ● |
| 9 | Attack Time | 0.8 | 0.226 | ±0.10 | ○ |
| 10 | Decay Time | 80 | 0.476 | ±0.20 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 30 | 0.006 | ±0.15 | ○ |
| 13 | Pitch Sweep Duration | 3 | 0.492 | ±0.10 | ○ |
| 14 | Pitch Droop Rate | 0.5 | 0.010 | ±0.05 | ○ |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.02 | 0.020 | ±0.03 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 1 | 0.125 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.05 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.45 | 0.450 | ±0.00 | ○ |

### TR-808–HC: High Conga

**Circuit:** Bridged-T (shared with toms, higher pitch, tighter envelope)  
**Key Discriminator:** Shorter decay than LC, higher pitch

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 370 | 0.422 | ±0.10 | ● |
| 2 | Spectral Centroid | 380 | 0.426 | ±0.10 | ○ |
| 3 | Bandwidth (-3dB) | 45 | 0.384 | ±0.20 | ○ |
| 4 | H2 Relative Level | -28 | 0.650 | ±0.20 | ○ |
| 5 | H3 Relative Level | -42 | 0.475 | ±0.20 | ○ |
| 6 | H4 Relative Level | -55 | 0.312 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 2 | 0.167 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.0 | 0.000 | ±0.05 | ● |
| 9 | Attack Time | 0.8 | 0.226 | ±0.10 | ○ |
| 10 | Decay Time | 60 | 0.445 | ±0.20 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 40 | 0.008 | ±0.15 | ○ |
| 13 | Pitch Sweep Duration | 3 | 0.492 | ±0.10 | ○ |
| 14 | Pitch Droop Rate | 0.5 | 0.010 | ±0.05 | ○ |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.02 | 0.020 | ±0.03 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 1 | 0.125 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.05 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.45 | 0.450 | ±0.00 | ○ |

### TR-808–RS: Rimshot

**Circuit:** Two bridged-T oscillators (1667Hz + 455Hz) → summed → VCA saturation → 10ms AD → HPF → gate  
**Key Discriminator:** Bright dual-tone crack with saturation harmonics, ~10ms, very specific freq pair

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 455 | 0.452 | ±0.08 | ● |
| 2 | Spectral Centroid | 1.1k | 0.580 | ±0.15 | ○ |
| 3 | Bandwidth (-3dB) | 1.5k | 0.738 | ±0.20 | ○ |
| 4 | H2 Relative Level | -3 | 0.963 | ±0.10 | ● |
| 5 | H3 Relative Level | -10 | 0.875 | ±0.15 | ○ |
| 6 | H4 Relative Level | -18 | 0.775 | ±0.15 | ○ |
| 7 | Highest Significant Harmonic | 12 | 0.597 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.8 | 0.800 | ±0.05 | ● |
| 9 | Attack Time | 0.5 | 0.175 | ±0.10 | ● |
| 10 | Decay Time | 10 | 0.250 | ±0.15 | ● |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.05 | 0.050 | ±0.05 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 2 | 0.250 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.05 | ● |
| 23 | Filter Type Code | 0.5 | 0.500 | ±0.00 | ● |
| 24 | Filter Cutoff | 400 | 0.434 | ±0.15 | ○ |
| 25 | Filter Q | 1.0 | 0.404 | ±0.20 | ○ |
| 26 | Waveshaping Severity | 0.3 | 0.300 | ±0.10 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.6 | 0.600 | ±0.00 | ○ |

### TR-808–CL: Claves

**Circuit:** Single bridged-T oscillator (both halves of IC20 combined) → output  
**Key Discriminator:** Purest 808 voice — single ~2500Hz sine ping, very short, no noise

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 2.5k | 0.699 | ±0.05 | ● |
| 2 | Spectral Centroid | 2.5k | 0.699 | ±0.05 | ● |
| 3 | Bandwidth (-3dB) | 20 | 0.302 | ±0.10 | ○ |
| 4 | H2 Relative Level | -60 | 0.250 | ±0.15 | ○ |
| 5 | H3 Relative Level | -70 | 0.125 | ±0.15 | ○ |
| 6 | H4 Relative Level | -75 | 0.062 | ±0.15 | ○ |
| 7 | Highest Significant Harmonic | 1 | 0.000 | ±0.05 | ● |
| 8 | Inharmonicity Index | 0.0 | 0.000 | ±0.02 | ● |
| 9 | Attack Time | 0.3 | 0.119 | ±0.05 | ● |
| 10 | Decay Time | 15 | 0.294 | ±0.10 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.0 | 0.000 | ±0.00 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 1 | 0.125 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.05 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.7 | 0.700 | ±0.00 | ○ |

### TR-808–CP: Handclap

**Circuit:** White noise → BPF → dual VCA chain with quadruple-burst envelope (4 bursts ~5ms apart + decay tail)  
**Key Discriminator:** Quadruple-burst temporal envelope — 4 closely-spaced transients, not a single hit

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 1.5k | 0.625 | ±0.20 | ○ |
| 2 | Spectral Centroid | 1.8k | 0.651 | ±0.20 | ○ |
| 3 | Bandwidth (-3dB) | 2.0k | 0.767 | ±0.25 | ○ |
| 4 | H2 Relative Level | -80 | 0.000 | ±0.00 | ● |
| 5 | H3 Relative Level | -80 | 0.000 | ±0.00 | ● |
| 6 | H4 Relative Level | -80 | 0.000 | ±0.00 | ● |
| 7 | Highest Significant Harmonic | 1 | 0.000 | ±0.00 | — |
| 8 | Inharmonicity Index | 1.0 | 1.000 | ±0.05 | ● |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ● |
| 10 | Decay Time | 80 | 0.476 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 0.6 | 0.600 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 1.0 | 1.000 | ±0.05 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.75 | 0.750 | ±0.00 | ● |
| 23 | Filter Type Code | 0.75 | 0.750 | ±0.00 | ● |
| 24 | Filter Cutoff | 1.5k | 0.625 | ±0.20 | ○ |
| 25 | Filter Q | 2.0 | 0.525 | ±0.30 | ○ |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.8 | 0.800 | ±0.00 | ● |

### TR-808–MA: Maracas

**Circuit:** White noise → HPF → short VCA envelope (~50ms)  
**Key Discriminator:** HPF noise, very short, bright sizzle — 808-exclusive voice

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 8.0k | 0.867 | ±0.20 | ○ |
| 2 | Spectral Centroid | 10.0k | 0.900 | ±0.20 | ○ |
| 3 | Bandwidth (-3dB) | 10.0k | 0.930 | ±0.25 | ○ |
| 4 | H2 Relative Level | -80 | 0.000 | ±0.00 | ● |
| 5 | H3 Relative Level | -80 | 0.000 | ±0.00 | ● |
| 6 | H4 Relative Level | -80 | 0.000 | ±0.00 | ● |
| 7 | Highest Significant Harmonic | 1 | 0.000 | ±0.00 | — |
| 8 | Inharmonicity Index | 1.0 | 1.000 | ±0.05 | ● |
| 9 | Attack Time | 0.5 | 0.175 | ±0.10 | ● |
| 10 | Decay Time | 50 | 0.425 | ±0.15 | ● |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 1.0 | 1.000 | ±0.05 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.75 | 0.750 | ±0.00 | ● |
| 23 | Filter Type Code | 0.5 | 0.500 | ±0.00 | ● |
| 24 | Filter Cutoff | 5.0k | 0.799 | ±0.20 | ○ |
| 25 | Filter Q | 1.0 | 0.404 | ±0.20 | ○ |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.55 | 0.550 | ±0.00 | ○ |

### TR-808–CB: Cowbell

**Circuit:** Two Schmitt-trigger square oscs (~540Hz + ~800Hz) → VCAs → summed → BPF (850Hz, Q≈4.25) → AD → output  
**Key Discriminator:** Two inharmonic square waves beating at ~1:1.48 ratio, BPF hollowed, instantly recognizable

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 540 | 0.477 | ±0.15 | ● |
| 2 | Spectral Centroid | 700 | 0.515 | ±0.15 | ○ |
| 3 | Bandwidth (-3dB) | 400 | 0.605 | ±0.20 | ○ |
| 4 | H2 Relative Level | -3 | 0.963 | ±0.10 | ● |
| 5 | H3 Relative Level | -20 | 0.750 | ±0.15 | ○ |
| 6 | H4 Relative Level | -30 | 0.625 | ±0.15 | ○ |
| 7 | Highest Significant Harmonic | 6 | 0.431 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.6 | 0.600 | ±0.10 | ● |
| 9 | Attack Time | 0.5 | 0.175 | ±0.10 | ● |
| 10 | Decay Time | 40 | 0.401 | ±0.15 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.0 | 0.000 | ±0.00 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 2 | 0.250 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.5 | 0.500 | ±0.00 | ● |
| 23 | Filter Type Code | 0.75 | 0.750 | ±0.00 | ● |
| 24 | Filter Cutoff | 850 | 0.543 | ±0.10 | ● |
| 25 | Filter Q | 4.25 | 0.657 | ±0.15 | ● |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.9 | 0.900 | ±0.00 | ● |

### TR-808–CY: Cymbal

**Circuit:** Six Schmitt-trigger square oscs (HD14584) → mixed → BPF → longest envelope → output  
**Key Discriminator:** Six non-harmonic square waves create dense metallic wash — 2SC828-R sizzle

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 3.0k | 0.725 | ±0.25 | ○ |
| 2 | Spectral Centroid | 6.0k | 0.826 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 12.0k | 0.948 | ±0.20 | ○ |
| 4 | H2 Relative Level | -80 | 0.000 | ±0.00 | — |
| 5 | H3 Relative Level | -80 | 0.000 | ±0.00 | — |
| 6 | H4 Relative Level | -80 | 0.000 | ±0.00 | — |
| 7 | Highest Significant Harmonic | 64 | 1.000 | ±0.10 | ○ |
| 8 | Inharmonicity Index | 1.0 | 1.000 | ±0.05 | ● |
| 9 | Attack Time | 0.5 | 0.175 | ±0.10 | ● |
| 10 | Decay Time | 2.0s | 0.825 | ±0.30 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.1 | 0.100 | ±0.10 | ○ |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 6 | 0.750 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.5 | 0.500 | ±0.00 | ● |
| 23 | Filter Type Code | 0.75 | 0.750 | ±0.10 | ○ |
| 24 | Filter Cutoff | 6.0k | 0.826 | ±0.25 | ○ |
| 25 | Filter Q | 2.0 | 0.525 | ±0.25 | ○ |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.75 | 0.750 | ±0.00 | ● |

### TR-808–OH: Open Hi-Hat

**Circuit:** Six Schmitt-trigger square oscs (shared with CY/CH) → BPF → medium decay → output  
**Key Discriminator:** Same 6-osc source as cymbal, medium decay, choke-linked to CH

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 3.0k | 0.725 | ±0.25 | ○ |
| 2 | Spectral Centroid | 7.0k | 0.848 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 10.0k | 0.930 | ±0.20 | ○ |
| 4 | H2 Relative Level | -80 | 0.000 | ±0.00 | — |
| 5 | H3 Relative Level | -80 | 0.000 | ±0.00 | — |
| 6 | H4 Relative Level | -80 | 0.000 | ±0.00 | — |
| 7 | Highest Significant Harmonic | 64 | 1.000 | ±0.10 | ○ |
| 8 | Inharmonicity Index | 1.0 | 1.000 | ±0.05 | ● |
| 9 | Attack Time | 0.5 | 0.175 | ±0.10 | ● |
| 10 | Decay Time | 500 | 0.675 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.1 | 0.100 | ±0.10 | ○ |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 6 | 0.750 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.5 | 0.500 | ±0.00 | ● |
| 23 | Filter Type Code | 0.75 | 0.750 | ±0.10 | ○ |
| 24 | Filter Cutoff | 7.0k | 0.848 | ±0.25 | ○ |
| 25 | Filter Q | 2.0 | 0.525 | ±0.25 | ○ |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.9 | 0.900 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.75 | 0.750 | ±0.00 | ● |

### TR-808–CH: Closed Hi-Hat

**Circuit:** Six Schmitt-trigger square oscs (shared) → BPF → shortest decay → output. Triggers kill OH.  
**Key Discriminator:** Shortest metallic voice, choke kills OH — hardware interaction signature

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 3.0k | 0.725 | ±0.25 | ○ |
| 2 | Spectral Centroid | 8.0k | 0.867 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 8.0k | 0.907 | ±0.20 | ○ |
| 4 | H2 Relative Level | -80 | 0.000 | ±0.00 | — |
| 5 | H3 Relative Level | -80 | 0.000 | ±0.00 | — |
| 6 | H4 Relative Level | -80 | 0.000 | ±0.00 | — |
| 7 | Highest Significant Harmonic | 64 | 1.000 | ±0.10 | ○ |
| 8 | Inharmonicity Index | 1.0 | 1.000 | ±0.05 | ● |
| 9 | Attack Time | 0.3 | 0.119 | ±0.05 | ● |
| 10 | Decay Time | 30 | 0.369 | ±0.15 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.1 | 0.100 | ±0.10 | ○ |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 6 | 0.750 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.5 | 0.500 | ±0.00 | ● |
| 23 | Filter Type Code | 0.75 | 0.750 | ±0.10 | ○ |
| 24 | Filter Cutoff | 8.0k | 0.867 | ±0.25 | ○ |
| 25 | Filter Q | 3.0 | 0.596 | ±0.25 | ○ |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.7 | 0.700 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.9 | 0.900 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.75 | 0.750 | ±0.00 | ● |

---

## §4 — Roland TR-909 Rhythm Composer

**Year:** 1983–1984  
**Type:** Analog/digital hybrid drum synthesizer  
**Synthesis:** Analog (BD,SD,Toms,RS,CP) + 6-bit PCM (HH,CY)  
**Key Component:** Hoshiai's personal Paiste/Zildjian cymbals (6-bit PCM source)  
**Units Produced:** ~10,000  

### TR-909–BD: Bass Drum

**Circuit:** Triangle osc → waveshaper (back-to-back diodes) → approximate sine → pitch sweep → attack noise → decay VCA  
**Key Discriminator:** Waveshaper 'dirt' — residual harmonics from imperfect sine approximation. 808=clean sine, 909=sine+dirt.

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 70 | 0.181 | ±0.30 | ● |
| 2 | Spectral Centroid | 90 | 0.218 | ±0.20 | ○ |
| 3 | Bandwidth (-3dB) | 60 | 0.413 | ±0.25 | ○ |
| 4 | H2 Relative Level | -12 | 0.850 | ±0.20 | ● |
| 5 | H3 Relative Level | -20 | 0.750 | ±0.20 | ○ |
| 6 | H4 Relative Level | -30 | 0.625 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 4 | 0.333 | ±0.15 | ● |
| 8 | Inharmonicity Index | 0.05 | 0.050 | ±0.05 | ● |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ● |
| 10 | Decay Time | 250 | 0.599 | ±0.35 | ● |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 100 | 0.020 | ±0.25 | ● |
| 13 | Pitch Sweep Duration | 8 | 0.634 | ±0.15 | ● |
| 14 | Pitch Droop Rate | 1 | 0.020 | ±0.10 | ○ |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.15 | 0.150 | ±0.10 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -90 | 0.062 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 1 | 0.125 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.25 | 0.250 | ±0.05 | ● |
| 23 | Filter Type Code | 0.25 | 0.250 | ±0.10 | ○ |
| 24 | Filter Cutoff | 800 | 0.534 | ±0.25 | ○ |
| 25 | Filter Q | 1.0 | 0.404 | ±0.20 | ○ |
| 26 | Waveshaping Severity | 0.4 | 0.400 | ±0.10 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.5 | 0.500 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.8 | 0.800 | ±0.00 | ● |

### TR-909–SD: Snare Drum

**Circuit:** Two triangle oscs (185Hz+330Hz) → waveshaped → mixed with shared noise gen → HPF → dual envelopes  
**Key Discriminator:** Shared noise source with CP creates phase artifact when both trigger simultaneously

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 185 | 0.322 | ±0.10 | ● |
| 2 | Spectral Centroid | 450 | 0.451 | ±0.20 | ○ |
| 3 | Bandwidth (-3dB) | 2.5k | 0.790 | ±0.25 | ○ |
| 4 | H2 Relative Level | -5 | 0.938 | ±0.15 | ● |
| 5 | H3 Relative Level | -15 | 0.812 | ±0.20 | ○ |
| 6 | H4 Relative Level | -25 | 0.688 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 10 | 0.554 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.65 | 0.650 | ±0.10 | ● |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ● |
| 10 | Decay Time | 180 | 0.564 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.5 | 0.500 | ±0.30 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -90 | 0.062 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 2 | 0.250 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.25 | 0.250 | ±0.05 | ● |
| 23 | Filter Type Code | 0.5 | 0.500 | ±0.10 | ● |
| 24 | Filter Cutoff | 1.2k | 0.593 | ±0.25 | ○ |
| 25 | Filter Q | 1.0 | 0.404 | ±0.20 | ○ |
| 26 | Waveshaping Severity | 0.1 | 0.100 | ±0.05 | ○ |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.5 | 0.500 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.6 | 0.600 | ±0.00 | ○ |

### TR-909–LT: Low Tom

**Circuit:** Dual oscillator design (richer than 808 single bridged-T)  
**Key Discriminator:** Warmer, rounder than 808 toms — dual osc gives richer tonal character

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 90 | 0.218 | ±0.15 | ○ |
| 2 | Spectral Centroid | 100 | 0.233 | ±0.15 | ○ |
| 3 | Bandwidth (-3dB) | 50 | 0.395 | ±0.20 | ○ |
| 4 | H2 Relative Level | -10 | 0.875 | ±0.20 | ○ |
| 5 | H3 Relative Level | -25 | 0.688 | ±0.20 | ○ |
| 6 | H4 Relative Level | -40 | 0.500 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 3 | 0.264 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.1 | 0.100 | ±0.10 | ○ |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ○ |
| 10 | Decay Time | 150 | 0.544 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 30 | 0.006 | ±0.15 | ○ |
| 13 | Pitch Sweep Duration | 4 | 0.534 | ±0.10 | ○ |
| 14 | Pitch Droop Rate | 0.5 | 0.010 | ±0.05 | ○ |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.02 | 0.020 | ±0.03 | ○ |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -90 | 0.062 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 2 | 0.250 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.25 | 0.250 | ±0.05 | ○ |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ○ |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ○ |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.5 | 0.500 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.4 | 0.400 | ±0.00 | ○ |

### TR-909–MT: Mid Tom

**Circuit:** Dual oscillator, mid pitch  
**Key Discriminator:** Same architecture as 909 LT, higher pitch

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 140 | 0.282 | ±0.15 | ○ |
| 2 | Spectral Centroid | 155 | 0.296 | ±0.15 | ○ |
| 3 | Bandwidth (-3dB) | 50 | 0.395 | ±0.20 | ○ |
| 4 | H2 Relative Level | -10 | 0.875 | ±0.20 | ○ |
| 5 | H3 Relative Level | -25 | 0.688 | ±0.20 | ○ |
| 6 | H4 Relative Level | -40 | 0.500 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 3 | 0.264 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.1 | 0.100 | ±0.10 | ○ |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ○ |
| 10 | Decay Time | 120 | 0.520 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 40 | 0.008 | ±0.15 | ○ |
| 13 | Pitch Sweep Duration | 4 | 0.534 | ±0.10 | ○ |
| 14 | Pitch Droop Rate | 0.5 | 0.010 | ±0.05 | ○ |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.02 | 0.020 | ±0.03 | ○ |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -90 | 0.062 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 2 | 0.250 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.25 | 0.250 | ±0.05 | ○ |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ○ |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ○ |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.5 | 0.500 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.4 | 0.400 | ±0.00 | ○ |

### TR-909–HT: High Tom

**Circuit:** Dual oscillator, high pitch  
**Key Discriminator:** Same architecture, highest tom pitch

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 200 | 0.333 | ±0.15 | ○ |
| 2 | Spectral Centroid | 220 | 0.347 | ±0.15 | ○ |
| 3 | Bandwidth (-3dB) | 55 | 0.405 | ±0.20 | ○ |
| 4 | H2 Relative Level | -10 | 0.875 | ±0.20 | ○ |
| 5 | H3 Relative Level | -25 | 0.688 | ±0.20 | ○ |
| 6 | H4 Relative Level | -40 | 0.500 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 3 | 0.264 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.1 | 0.100 | ±0.10 | ○ |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ○ |
| 10 | Decay Time | 100 | 0.500 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 50 | 0.010 | ±0.15 | ○ |
| 13 | Pitch Sweep Duration | 3 | 0.492 | ±0.10 | ○ |
| 14 | Pitch Droop Rate | 0.5 | 0.010 | ±0.05 | ○ |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.02 | 0.020 | ±0.03 | ○ |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -90 | 0.062 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 2 | 0.250 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.25 | 0.250 | ±0.05 | ○ |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ○ |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ○ |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.5 | 0.500 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.4 | 0.400 | ±0.00 | ○ |

### TR-909–RS: Rimshot

**Circuit:** Three bridged-T oscillators at different frequencies → mixed → VCA → envelope → output  
**Key Discriminator:** Three oscillators (vs 808's two) — denser, more body

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 500 | 0.466 | ±0.10 | ○ |
| 2 | Spectral Centroid | 1.2k | 0.593 | ±0.15 | ○ |
| 3 | Bandwidth (-3dB) | 1.8k | 0.757 | ±0.20 | ○ |
| 4 | H2 Relative Level | -3 | 0.963 | ±0.10 | ○ |
| 5 | H3 Relative Level | -8 | 0.900 | ±0.15 | ○ |
| 6 | H4 Relative Level | -18 | 0.775 | ±0.15 | ○ |
| 7 | Highest Significant Harmonic | 15 | 0.651 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.85 | 0.850 | ±0.05 | ● |
| 9 | Attack Time | 0.5 | 0.175 | ±0.10 | ● |
| 10 | Decay Time | 12 | 0.270 | ±0.15 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 0.2 | 0.200 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.03 | 0.030 | ±0.05 | ○ |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -90 | 0.062 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 3 | 0.375 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.05 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ○ |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.5 | 0.500 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.55 | 0.550 | ±0.00 | ○ |

### TR-909–CP: Handclap

**Circuit:** Quasi-random logic noise → triple-sawtooth envelope → VCA → output. Shares noise gen with SD.  
**Key Discriminator:** Triple-sawtooth envelope (vs 808's quadruple-burst). Phase artifact with SD is diagnostic.

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 1.5k | 0.625 | ±0.20 | ○ |
| 2 | Spectral Centroid | 2.0k | 0.667 | ±0.20 | ○ |
| 3 | Bandwidth (-3dB) | 2.5k | 0.790 | ±0.25 | ○ |
| 4 | H2 Relative Level | -80 | 0.000 | ±0.00 | ● |
| 5 | H3 Relative Level | -80 | 0.000 | ±0.00 | ● |
| 6 | H4 Relative Level | -80 | 0.000 | ±0.00 | ● |
| 7 | Highest Significant Harmonic | 1 | 0.000 | ±0.00 | — |
| 8 | Inharmonicity Index | 1.0 | 1.000 | ±0.05 | ● |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ● |
| 10 | Decay Time | 70 | 0.461 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 0.6 | 0.600 | ±0.10 | ● |
| 16 | Noise-to-Tone Ratio | 1.0 | 1.000 | ±0.05 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -90 | 0.062 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.75 | 0.750 | ±0.00 | ● |
| 23 | Filter Type Code | 0.75 | 0.750 | ±0.10 | ○ |
| 24 | Filter Cutoff | 1.5k | 0.625 | ±0.20 | ○ |
| 25 | Filter Q | 2.0 | 0.525 | ±0.30 | ○ |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.2 | 0.200 | ±0.05 | ● |
| 28 | Unit Variance | 0.5 | 0.500 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.5 | 0.500 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.65 | 0.650 | ±0.00 | ○ |

### TR-909–CH: Closed Hi-Hat

**Circuit:** 6-bit PCM sample (Hoshiai's Paiste/Zildjian, ~32kHz, no EQ/compression)  
**Key Discriminator:** 6-BIT PCM vs 808's 6 analog oscillators — most reliable 808/909 discriminator

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 4.0k | 0.767 | ±0.20 | ○ |
| 2 | Spectral Centroid | 8.0k | 0.867 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 10.0k | 0.930 | ±0.20 | ○ |
| 4 | H2 Relative Level | -80 | 0.000 | ±0.00 | — |
| 5 | H3 Relative Level | -80 | 0.000 | ±0.00 | — |
| 6 | H4 Relative Level | -80 | 0.000 | ±0.00 | — |
| 7 | Highest Significant Harmonic | 32 | 0.833 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.9 | 0.900 | ±0.05 | ● |
| 9 | Attack Time | 0.3 | 0.119 | ±0.05 | ● |
| 10 | Decay Time | 25 | 0.349 | ±0.15 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.2 | 0.200 | ±0.10 | ○ |
| 17 | Effective Bit Depth | 6 | 0.217 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 16.0k | 0.273 | ±0.10 | ● |
| 19 | Quantization Noise Floor | -36 | 0.625 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.3 | 0.300 | ±0.10 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.9 | 0.900 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.9 | 0.900 | ±0.00 | ● |

### TR-909–OH: Open Hi-Hat

**Circuit:** 6-bit PCM sample (same source as CH, longer playback)  
**Key Discriminator:** Same 6-bit PCM source, longer decay, choke-linked to CH

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 4.0k | 0.767 | ±0.20 | ○ |
| 2 | Spectral Centroid | 7.0k | 0.848 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 10.0k | 0.930 | ±0.20 | ○ |
| 4 | H2 Relative Level | -80 | 0.000 | ±0.00 | — |
| 5 | H3 Relative Level | -80 | 0.000 | ±0.00 | — |
| 6 | H4 Relative Level | -80 | 0.000 | ±0.00 | — |
| 7 | Highest Significant Harmonic | 32 | 0.833 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.9 | 0.900 | ±0.05 | ● |
| 9 | Attack Time | 0.3 | 0.119 | ±0.05 | ● |
| 10 | Decay Time | 300 | 0.619 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.2 | 0.200 | ±0.10 | ○ |
| 17 | Effective Bit Depth | 6 | 0.217 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 16.0k | 0.273 | ±0.10 | ● |
| 19 | Quantization Noise Floor | -36 | 0.625 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.3 | 0.300 | ±0.10 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.9 | 0.900 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.9 | 0.900 | ±0.00 | ● |

### TR-909–CR: Crash Cymbal

**Circuit:** 6-bit PCM sample (Hoshiai's Paiste crash)  
**Key Discriminator:** 909-exclusive — 808 has single cymbal voice, 909 has separate crash + ride

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 3.0k | 0.725 | ±0.25 | ○ |
| 2 | Spectral Centroid | 6.0k | 0.826 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 14.0k | 0.964 | ±0.20 | ○ |
| 4 | H2 Relative Level | -80 | 0.000 | ±0.00 | — |
| 5 | H3 Relative Level | -80 | 0.000 | ±0.00 | — |
| 6 | H4 Relative Level | -80 | 0.000 | ±0.00 | — |
| 7 | Highest Significant Harmonic | 40 | 0.887 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.95 | 0.950 | ±0.05 | ○ |
| 9 | Attack Time | 0.5 | 0.175 | ±0.10 | ○ |
| 10 | Decay Time | 2.0s | 0.825 | ±0.30 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.2 | 0.200 | ±0.10 | ○ |
| 17 | Effective Bit Depth | 6 | 0.217 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 16.0k | 0.273 | ±0.10 | ● |
| 19 | Quantization Noise Floor | -36 | 0.625 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.3 | 0.300 | ±0.10 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.7 | 0.700 | ±0.00 | ○ |

### TR-909–RI: Ride Cymbal

**Circuit:** 6-bit PCM sample (Hoshiai's Zildjian ride)  
**Key Discriminator:** 909-exclusive — separate ride voice absent from 808

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 3.5k | 0.748 | ±0.25 | ○ |
| 2 | Spectral Centroid | 5.5k | 0.813 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 12.0k | 0.948 | ±0.20 | ○ |
| 4 | H2 Relative Level | -80 | 0.000 | ±0.00 | — |
| 5 | H3 Relative Level | -80 | 0.000 | ±0.00 | — |
| 6 | H4 Relative Level | -80 | 0.000 | ±0.00 | — |
| 7 | Highest Significant Harmonic | 40 | 0.887 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.9 | 0.900 | ±0.05 | ○ |
| 9 | Attack Time | 0.5 | 0.175 | ±0.10 | ○ |
| 10 | Decay Time | 1.5s | 0.794 | ±0.30 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.15 | 0.150 | ±0.10 | ○ |
| 17 | Effective Bit Depth | 6 | 0.217 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 16.0k | 0.273 | ±0.10 | ● |
| 19 | Quantization Noise Floor | -36 | 0.625 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.3 | 0.300 | ±0.10 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.7 | 0.700 | ±0.00 | ○ |

---

## §5 — Yamaha DX7

**Year:** 1983  
**Type:** FM synthesis (6 operators, 32 algorithms)  
**Synthesis:** FM — carrier ± n × modulator sidebands  
**Key Component:** 12-bit DAC, 49.1kHz internal rate  
**Units Produced:** ~200,000  
**Dictionary Songs:** EWTRTW  

### DX7–GUITAR3: Guitar 3 (ROM1B #23)

**Circuit:** 6-op FM — bright plucked string simulation  
**Key Discriminator:** Fast attack FM pluck with rapid upper partial decay — layered with Vibe 2 in EWTRTW

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 330 | 0.406 | ±0.25 | ○ |
| 2 | Spectral Centroid | 2.5k | 0.699 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 6.0k | 0.878 | ±0.30 | ○ |
| 4 | H2 Relative Level | -6 | 0.925 | ±0.20 | ○ |
| 5 | H3 Relative Level | -10 | 0.875 | ±0.20 | ○ |
| 6 | H4 Relative Level | -15 | 0.812 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 20 | 0.720 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.6 | 0.600 | ±0.15 | ● |
| 9 | Attack Time | 0.5 | 0.175 | ±0.15 | ○ |
| 10 | Decay Time | 500 | 0.675 | ±0.30 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.05 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 0.8 | 0.800 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.0 | 0.000 | ±0.00 | ● |
| 17 | Effective Bit Depth | 12 | 0.478 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 24.6k | 0.467 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -72 | 0.250 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.05 | 0.050 | ±0.05 | ○ |
| 21 | Oscillator Count | 6 | 0.750 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.7 | 0.700 | ±0.10 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.55 | 0.550 | ±0.00 | ○ |

### DX7–VIBE2: Vibe 2 (ROM2A #23)

**Circuit:** 6-op FM — vibraphone-like bell tone  
**Key Discriminator:** Sustained bell partials, slow decay, metallic shimmer — layered with Guitar 3

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 440 | 0.447 | ±0.25 | ○ |
| 2 | Spectral Centroid | 3.0k | 0.725 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 5.0k | 0.860 | ±0.30 | ○ |
| 4 | H2 Relative Level | -8 | 0.900 | ±0.20 | ○ |
| 5 | H3 Relative Level | -12 | 0.850 | ±0.20 | ○ |
| 6 | H4 Relative Level | -18 | 0.775 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 24 | 0.764 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.5 | 0.500 | ±0.15 | ○ |
| 9 | Attack Time | 2.0 | 0.325 | ±0.15 | ○ |
| 10 | Decay Time | 3.0s | 0.869 | ±0.35 | ○ |
| 11 | Sustain Level | 0.3 | 0.300 | ±0.15 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 0.8 | 0.800 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.0 | 0.000 | ±0.00 | ● |
| 17 | Effective Bit Depth | 12 | 0.478 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 24.6k | 0.467 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -72 | 0.250 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.05 | 0.050 | ±0.05 | ○ |
| 21 | Oscillator Count | 6 | 0.750 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.7 | 0.700 | ±0.10 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.5 | 0.500 | ±0.00 | ○ |

### DX7–PIANO5: Piano 5 (ROM1B #02)

**Circuit:** 6-op FM — bright keyboard tone  
**Key Discriminator:** Attack transient with inharmonic content, velocity-sensitive brightness — layered with Koto

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 261 | 0.372 | ±0.25 | ○ |
| 2 | Spectral Centroid | 2.0k | 0.667 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 5.0k | 0.860 | ±0.30 | ○ |
| 4 | H2 Relative Level | -8 | 0.900 | ±0.20 | ○ |
| 5 | H3 Relative Level | -14 | 0.825 | ±0.20 | ○ |
| 6 | H4 Relative Level | -20 | 0.750 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 18 | 0.695 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.4 | 0.400 | ±0.15 | ○ |
| 9 | Attack Time | 1.0 | 0.250 | ±0.15 | ○ |
| 10 | Decay Time | 2.0s | 0.825 | ±0.35 | ○ |
| 11 | Sustain Level | 0.2 | 0.200 | ±0.15 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 0.8 | 0.800 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.0 | 0.000 | ±0.00 | ● |
| 17 | Effective Bit Depth | 12 | 0.478 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 24.6k | 0.467 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -72 | 0.250 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.05 | 0.050 | ±0.05 | ○ |
| 21 | Oscillator Count | 6 | 0.750 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.8 | 0.800 | ±0.10 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.5 | 0.500 | ±0.00 | ○ |

### DX7–KOTO: Koto (ROM1A #23)

**Circuit:** 6-op FM — plucked string, bright  
**Key Discriminator:** Sharp attack, rapid high-freq decay, resonant body — layered with Piano 5

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 330 | 0.406 | ±0.25 | ○ |
| 2 | Spectral Centroid | 3.5k | 0.748 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 7.0k | 0.894 | ±0.30 | ○ |
| 4 | H2 Relative Level | -5 | 0.938 | ±0.20 | ○ |
| 5 | H3 Relative Level | -10 | 0.875 | ±0.20 | ○ |
| 6 | H4 Relative Level | -16 | 0.800 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 25 | 0.774 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.55 | 0.550 | ±0.15 | ○ |
| 9 | Attack Time | 0.3 | 0.119 | ±0.10 | ○ |
| 10 | Decay Time | 400 | 0.651 | ±0.30 | ○ |
| 11 | Sustain Level | 0.05 | 0.050 | ±0.10 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 0.8 | 0.800 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.0 | 0.000 | ±0.00 | ● |
| 17 | Effective Bit Depth | 12 | 0.478 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 24.6k | 0.467 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -72 | 0.250 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.05 | 0.050 | ±0.05 | ○ |
| 21 | Oscillator Count | 6 | 0.750 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.7 | 0.700 | ±0.10 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.5 | 0.500 | ±0.00 | ○ |

### DX7–PIANO1: Piano 1 (ROM1A #08)

**Circuit:** 6-op FM — Rhodes-like electric piano  
**Key Discriminator:** Smooth attack, warm bell, moderate brightness — panned 25L in EWTRTW chords

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 261 | 0.372 | ±0.25 | ○ |
| 2 | Spectral Centroid | 1.5k | 0.625 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 4.0k | 0.837 | ±0.30 | ○ |
| 4 | H2 Relative Level | -10 | 0.875 | ±0.20 | ○ |
| 5 | H3 Relative Level | -16 | 0.800 | ±0.20 | ○ |
| 6 | H4 Relative Level | -24 | 0.700 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 14 | 0.635 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.35 | 0.350 | ±0.15 | ○ |
| 9 | Attack Time | 2.0 | 0.325 | ±0.15 | ○ |
| 10 | Decay Time | 3.0s | 0.869 | ±0.35 | ○ |
| 11 | Sustain Level | 0.3 | 0.300 | ±0.15 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 0.8 | 0.800 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.0 | 0.000 | ±0.00 | ● |
| 17 | Effective Bit Depth | 12 | 0.478 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 24.6k | 0.467 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -72 | 0.250 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.05 | 0.050 | ±0.05 | ○ |
| 21 | Oscillator Count | 6 | 0.750 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.6 | 0.600 | ±0.10 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.45 | 0.450 | ±0.00 | ○ |

### DX7–PIANO4: Piano 4 (ROM1B #01)

**Circuit:** 6-op FM — EP variant, similar to Piano 1  
**Key Discriminator:** Slightly different operator balance — panned 25R in EWTRTW chords

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 261 | 0.372 | ±0.25 | ○ |
| 2 | Spectral Centroid | 1.6k | 0.634 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 4.2k | 0.842 | ±0.30 | ○ |
| 4 | H2 Relative Level | -9 | 0.887 | ±0.20 | ○ |
| 5 | H3 Relative Level | -15 | 0.812 | ±0.20 | ○ |
| 6 | H4 Relative Level | -22 | 0.725 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 15 | 0.651 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.38 | 0.380 | ±0.15 | ○ |
| 9 | Attack Time | 2.0 | 0.325 | ±0.15 | ○ |
| 10 | Decay Time | 2.8s | 0.862 | ±0.35 | ○ |
| 11 | Sustain Level | 0.3 | 0.300 | ±0.15 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 0.8 | 0.800 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.0 | 0.000 | ±0.00 | ● |
| 17 | Effective Bit Depth | 12 | 0.478 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 24.6k | 0.467 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -72 | 0.250 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.05 | 0.050 | ±0.05 | ○ |
| 21 | Oscillator Count | 6 | 0.750 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.6 | 0.600 | ±0.10 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.45 | 0.450 | ±0.00 | ○ |

### DX7–BASS4: Bass 4 (ROM1B #32)

**Circuit:** 6-op FM — punchy bass  
**Key Discriminator:** Sub-heavy, velocity-sensitive attack brightness — provides body layer (no click) in EWTRTW bass

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 65 | 0.171 | ±0.25 | ○ |
| 2 | Spectral Centroid | 200 | 0.333 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 800 | 0.675 | ±0.30 | ○ |
| 4 | H2 Relative Level | -10 | 0.875 | ±0.20 | ○ |
| 5 | H3 Relative Level | -18 | 0.775 | ±0.20 | ○ |
| 6 | H4 Relative Level | -28 | 0.650 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 8 | 0.500 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.3 | 0.300 | ±0.15 | ○ |
| 9 | Attack Time | 1.0 | 0.250 | ±0.15 | ○ |
| 10 | Decay Time | 1.0s | 0.750 | ±0.35 | ○ |
| 11 | Sustain Level | 0.4 | 0.400 | ±0.15 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 0.8 | 0.800 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.0 | 0.000 | ±0.00 | ● |
| 17 | Effective Bit Depth | 12 | 0.478 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 24.6k | 0.467 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -72 | 0.250 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.05 | 0.050 | ±0.05 | ○ |
| 21 | Oscillator Count | 6 | 0.750 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.8 | 0.800 | ±0.10 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.5 | 0.500 | ±0.00 | ○ |

---

## §6 — Sequential Prophet T-8

**Year:** 1983  
**Type:** Analog subtractive (2 osc per voice, CEM chips, 8-voice poly)  
**Synthesis:** Analog subtractive — oscillators → CEM 3320 LPF (-24dB/oct)  
**Key Component:** Curtis CEM 3320 lowpass filter, CEM oscillators  
**Units Produced:** ~1,000  
**Dictionary Songs:** EWTRTW  

### Prophet-T8–CHORD: Guitar Approximation Chord

**Circuit:** Osc1 PW31% + Osc2 PW83%, cutoff 482Hz, res ~1, env depth 3.75, decay 3.27s, LFO 6.82Hz PWM (depth 0.172), 40% chorus  
**Key Discriminator:** Dual pulse-wave PWM creates spectral pattern of shifting odd harmonics — CEM filter slope specific

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 220 | 0.347 | ±0.25 | ○ |
| 2 | Spectral Centroid | 800 | 0.534 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 2.0k | 0.767 | ±0.30 | ○ |
| 4 | H2 Relative Level | -3 | 0.963 | ±0.15 | ● |
| 5 | H3 Relative Level | -8 | 0.900 | ±0.15 | ○ |
| 6 | H4 Relative Level | -12 | 0.850 | ±0.15 | ○ |
| 7 | Highest Significant Harmonic | 12 | 0.597 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.1 | 0.100 | ±0.10 | ● |
| 9 | Attack Time | 5.0 | 0.425 | ±0.15 | ○ |
| 10 | Decay Time | 3.3s | 0.879 | ±0.20 | ● |
| 11 | Sustain Level | 0.15 | 0.150 | ±0.10 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 0.8 | 0.800 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.0 | 0.000 | ±0.00 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 2 | 0.250 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.625 | 0.625 | ±0.00 | ● |
| 23 | Filter Type Code | 0.25 | 0.250 | ±0.00 | ● |
| 24 | Filter Cutoff | 482 | 0.461 | ±0.15 | ● |
| 25 | Filter Q | 1.0 | 0.404 | ±0.15 | ● |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.6 | 0.600 | ±0.15 | ● |
| 28 | Unit Variance | 0.4 | 0.400 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.6 | 0.600 | ±0.00 | ○ |

### Prophet-T8–RHYTHM: Rhythmic Single-Note Pulse

**Circuit:** Dual sawtooth (1 oct apart), filter decay 356ms, amp decay 393ms, release 147ms, 50% chorus, bass -5dB  
**Key Discriminator:** Short percussive analog pulse — sawtooth one octave apart with fast filter decay

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 220 | 0.347 | ±0.25 | ○ |
| 2 | Spectral Centroid | 600 | 0.492 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 3.0k | 0.808 | ±0.30 | ○ |
| 4 | H2 Relative Level | -3 | 0.963 | ±0.15 | ○ |
| 5 | H3 Relative Level | -6 | 0.925 | ±0.15 | ○ |
| 6 | H4 Relative Level | -10 | 0.875 | ±0.15 | ○ |
| 7 | Highest Significant Harmonic | 16 | 0.667 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.05 | 0.050 | ±0.10 | ● |
| 9 | Attack Time | 2.0 | 0.325 | ±0.10 | ○ |
| 10 | Decay Time | 393 | 0.649 | ±0.15 | ● |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.05 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 0.8 | 0.800 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.0 | 0.000 | ±0.00 | ● |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -96 | 0.000 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 2 | 0.250 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.375 | 0.375 | ±0.00 | ● |
| 23 | Filter Type Code | 0.25 | 0.250 | ±0.00 | ● |
| 24 | Filter Cutoff | 600 | 0.492 | ±0.20 | ○ |
| 25 | Filter Q | 1.0 | 0.404 | ±0.15 | ○ |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.6 | 0.600 | ±0.15 | ● |
| 28 | Unit Variance | 0.4 | 0.400 | ±0.10 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.55 | 0.550 | ±0.00 | ○ |

---

## §7 — PPG Wave 2.3

**Year:** 1982  
**Type:** Wavetable (2 digital osc through analog SSM 2044 filter)  
**Synthesis:** Wavetable scanning + analog filter hybrid  
**Key Component:** 12-bit DAC, SSM 2044 analog filter  
**Units Produced:** ~2,000  
**Dictionary Songs:** EWTRTW (bass layer)  

### PPG-Wave–BASS013A: Modified 013 A (Bass)

**Circuit:** Wavetable bass — high-end clickiness, attack transient, osc wave lowered, filter env depth reduced  
**Key Discriminator:** Attack click 2-5kHz (known PPG characteristic) + 12-bit quantization grit + wavetable spectral stepping

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 65 | 0.171 | ±0.20 | ○ |
| 2 | Spectral Centroid | 400 | 0.434 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 3.0k | 0.808 | ±0.30 | ○ |
| 4 | H2 Relative Level | -8 | 0.900 | ±0.20 | ○ |
| 5 | H3 Relative Level | -15 | 0.812 | ±0.20 | ○ |
| 6 | H4 Relative Level | -22 | 0.725 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 10 | 0.554 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.2 | 0.200 | ±0.15 | ○ |
| 9 | Attack Time | 0.5 | 0.175 | ±0.10 | ● |
| 10 | Decay Time | 800 | 0.726 | ±0.30 | ○ |
| 11 | Sustain Level | 0.3 | 0.300 | ±0.15 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 0.8 | 0.800 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.05 | 0.050 | ±0.05 | ○ |
| 17 | Effective Bit Depth | 12 | 0.478 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 24.0k | 0.455 | ±0.10 | ○ |
| 19 | Quantization Noise Floor | -72 | 0.250 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.1 | 0.100 | ±0.05 | ○ |
| 21 | Oscillator Count | 2 | 0.250 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.10 | ○ |
| 23 | Filter Type Code | 0.25 | 0.250 | ±0.00 | ● |
| 24 | Filter Cutoff | 400 | 0.434 | ±0.20 | ○ |
| 25 | Filter Q | 2.0 | 0.525 | ±0.20 | ○ |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.3 | 0.300 | ±0.15 | ○ |
| 28 | Unit Variance | 0.1 | 0.100 | ±0.05 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.6 | 0.600 | ±0.00 | ○ |

---

## §8 — LinnDrum LM-2

**Year:** 1982  
**Type:** Sample-based drum machine (8-bit, ~28kHz)  
**Synthesis:** 8-bit sample playback  
**Key Component:** 8-bit samples, no velocity layers, no round-robin  
**Units Produced:** ~5,000  
**Dictionary Songs:** EWTRTW  

### LinnDrum–KD: Kick Drum

**Circuit:** 8-bit sample — short, punchy, moderate sub. Less boom than 808, more thud.  
**Key Discriminator:** 8-bit aliasing + fixed transient (zero variation) + less sub than 808

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 60 | 0.159 | ±0.15 | ○ |
| 2 | Spectral Centroid | 120 | 0.259 | ±0.20 | ○ |
| 3 | Bandwidth (-3dB) | 200 | 0.535 | ±0.25 | ○ |
| 4 | H2 Relative Level | -8 | 0.900 | ±0.20 | ○ |
| 5 | H3 Relative Level | -15 | 0.812 | ±0.20 | ○ |
| 6 | H4 Relative Level | -25 | 0.688 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 6 | 0.431 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.2 | 0.200 | ±0.15 | ○ |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ○ |
| 10 | Decay Time | 200 | 0.575 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 10 | 0.002 | ±0.10 | ○ |
| 13 | Pitch Sweep Duration | 2 | 0.434 | ±0.10 | ○ |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.1 | 0.100 | ±0.10 | ○ |
| 17 | Effective Bit Depth | 8 | 0.304 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 14.0k | 0.227 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -48 | 0.500 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.2 | 0.200 | ±0.10 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.5 | 0.500 | ±0.00 | ○ |

### LinnDrum–SD: Snare Drum

**Circuit:** 8-bit sample — crisp, bright, relatively thin body  
**Key Discriminator:** Crisp 8-bit snare with characteristic fizz — often layered

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 200 | 0.333 | ±0.15 | ○ |
| 2 | Spectral Centroid | 2.0k | 0.667 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 5.0k | 0.860 | ±0.25 | ○ |
| 4 | H2 Relative Level | -6 | 0.925 | ±0.20 | ○ |
| 5 | H3 Relative Level | -12 | 0.850 | ±0.20 | ○ |
| 6 | H4 Relative Level | -20 | 0.750 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 16 | 0.667 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.5 | 0.500 | ±0.15 | ○ |
| 9 | Attack Time | 0.5 | 0.175 | ±0.10 | ○ |
| 10 | Decay Time | 150 | 0.544 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.4 | 0.400 | ±0.15 | ○ |
| 17 | Effective Bit Depth | 8 | 0.304 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 14.0k | 0.227 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -48 | 0.500 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.2 | 0.200 | ±0.10 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.45 | 0.450 | ±0.00 | ○ |

### LinnDrum–HH: Hi-Hat

**Circuit:** 8-bit sample — distinctive metallic decay, 8-bit fizz in high end  
**Key Discriminator:** Specific spectral decay profile identifiable in dense mix — 8-bit energy above natural cymbal content

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 5.0k | 0.799 | ±0.20 | ○ |
| 2 | Spectral Centroid | 8.0k | 0.867 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 10.0k | 0.930 | ±0.25 | ○ |
| 4 | H2 Relative Level | -80 | 0.000 | ±0.00 | — |
| 5 | H3 Relative Level | -80 | 0.000 | ±0.00 | — |
| 6 | H4 Relative Level | -80 | 0.000 | ±0.00 | — |
| 7 | Highest Significant Harmonic | 24 | 0.764 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.85 | 0.850 | ±0.10 | ○ |
| 9 | Attack Time | 0.3 | 0.119 | ±0.05 | ○ |
| 10 | Decay Time | 100 | 0.500 | ±0.20 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.3 | 0.300 | ±0.15 | ○ |
| 17 | Effective Bit Depth | 8 | 0.304 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 14.0k | 0.227 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -48 | 0.500 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.3 | 0.300 | ±0.10 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.55 | 0.550 | ±0.00 | ○ |

### LinnDrum–SH: Shaker

**Circuit:** 8-bit sample — similar fizz to HH, shorter decay  
**Key Discriminator:** Shorter decay than HH, similar 8-bit fizz character

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 6.0k | 0.826 | ±0.20 | ○ |
| 2 | Spectral Centroid | 9.0k | 0.884 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 8.0k | 0.907 | ±0.25 | ○ |
| 4 | H2 Relative Level | -80 | 0.000 | ±0.00 | — |
| 5 | H3 Relative Level | -80 | 0.000 | ±0.00 | — |
| 6 | H4 Relative Level | -80 | 0.000 | ±0.00 | — |
| 7 | Highest Significant Harmonic | 20 | 0.720 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.9 | 0.900 | ±0.10 | ○ |
| 9 | Attack Time | 0.3 | 0.119 | ±0.05 | ○ |
| 10 | Decay Time | 50 | 0.425 | ±0.15 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.5 | 0.500 | ±0.15 | ○ |
| 17 | Effective Bit Depth | 8 | 0.304 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 14.0k | 0.227 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -48 | 0.500 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.3 | 0.300 | ±0.10 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.45 | 0.450 | ±0.00 | ○ |

---

## §9 — Oberheim DMX

**Year:** 1981  
**Type:** Sample-based drum machine (8-bit, digitally stored analog drum samples)  
**Synthesis:** 8-bit sample playback — darker, grittier character than LinnDrum  
**Key Component:** 8-bit samples of real drums, Oberheim voicing  
**Units Produced:** ~4,000  
**Dictionary Songs:** EWTRTW (kick layer)  

### DMX–KD: Kick Drum

**Circuit:** 8-bit sample — more low-end weight than LinnDrum, longer sustain  
**Key Discriminator:** More sub-bass (30-80Hz) than LinnDrum kick — possibly double-quantized if resampled through Fairlight

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 55 | 0.146 | ±0.15 | ○ |
| 2 | Spectral Centroid | 100 | 0.233 | ±0.20 | ○ |
| 3 | Bandwidth (-3dB) | 150 | 0.506 | ±0.25 | ○ |
| 4 | H2 Relative Level | -6 | 0.925 | ±0.20 | ○ |
| 5 | H3 Relative Level | -14 | 0.825 | ±0.20 | ○ |
| 6 | H4 Relative Level | -24 | 0.700 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 5 | 0.387 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.2 | 0.200 | ±0.15 | ○ |
| 9 | Attack Time | 1.5 | 0.294 | ±0.10 | ○ |
| 10 | Decay Time | 300 | 0.619 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 5 | 0.001 | ±0.10 | ○ |
| 13 | Pitch Sweep Duration | 2 | 0.434 | ±0.10 | ○ |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.1 | 0.100 | ±0.10 | ○ |
| 17 | Effective Bit Depth | 8 | 0.304 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 14.0k | 0.227 | ±0.05 | ○ |
| 19 | Quantization Noise Floor | -48 | 0.500 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.2 | 0.200 | ±0.10 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.45 | 0.450 | ±0.00 | ○ |

### DMX–SD: Snare Drum

**Circuit:** 8-bit sample — crunchier, more 'real drum' character than LinnDrum  
**Key Discriminator:** Darker, crunchier character with more body than LinnDrum snare

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 180 | 0.318 | ±0.15 | ○ |
| 2 | Spectral Centroid | 1.5k | 0.625 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 4.0k | 0.837 | ±0.25 | ○ |
| 4 | H2 Relative Level | -5 | 0.938 | ±0.20 | ○ |
| 5 | H3 Relative Level | -10 | 0.875 | ±0.20 | ○ |
| 6 | H4 Relative Level | -18 | 0.775 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 14 | 0.635 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.45 | 0.450 | ±0.15 | ○ |
| 9 | Attack Time | 0.5 | 0.175 | ±0.10 | ○ |
| 10 | Decay Time | 200 | 0.575 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.45 | 0.450 | ±0.15 | ○ |
| 17 | Effective Bit Depth | 8 | 0.304 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 14.0k | 0.227 | ±0.05 | ○ |
| 19 | Quantization Noise Floor | -48 | 0.500 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.2 | 0.200 | ±0.10 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.4 | 0.400 | ±0.00 | ○ |

---

## §10 — E-mu Drumulator

**Year:** 1983  
**Type:** Sample-based drum machine (8-bit, 28kHz, custom EPROM capability)  
**Synthesis:** 8-bit sample playback — loaded with Bonham samples for SHOUT  
**Key Component:** EPROM chip swap — Bonham/Headley Grange samples loaded  
**Units Produced:** ~10,000  
**Dictionary Songs:** SHOUT (Bonham samples)  

### Drumulator–KD: Kick (Bonham/Headley Grange sample)

**Circuit:** 8-bit/28kHz sample of John Bonham kick via Headley Grange stairwell room  
**Key Discriminator:** Machine is INVISIBLE to spectral analysis — sounds organic. Only timing analysis reveals machine (grid CV 2%, zero drift autocorrelation)

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 60 | 0.159 | ±0.20 | ○ |
| 2 | Spectral Centroid | 200 | 0.333 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 400 | 0.605 | ±0.30 | ○ |
| 4 | H2 Relative Level | -4 | 0.950 | ±0.20 | ○ |
| 5 | H3 Relative Level | -10 | 0.875 | ±0.20 | ○ |
| 6 | H4 Relative Level | -18 | 0.775 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 10 | 0.554 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.3 | 0.300 | ±0.15 | ○ |
| 9 | Attack Time | 3.0 | 0.369 | ±0.15 | ○ |
| 10 | Decay Time | 600 | 0.695 | ±0.30 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.05 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.25 | 0.250 | ±0.15 | ○ |
| 17 | Effective Bit Depth | 8 | 0.304 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 14.0k | 0.227 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -48 | 0.500 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.2 | 0.200 | ±0.10 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.35 | 0.350 | ±0.00 | ○ |

### Drumulator–SD: Snare (Bonham/Headley Grange, gated)

**Circuit:** 8-bit sample — Bonham snare, room reverb blooms then gate slams shut  
**Key Discriminator:** Gated Bonham room — reverb bloom then dead cut is signature 80s gated drum sound

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 200 | 0.333 | ±0.20 | ○ |
| 2 | Spectral Centroid | 2.5k | 0.699 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 6.0k | 0.878 | ±0.30 | ○ |
| 4 | H2 Relative Level | -5 | 0.938 | ±0.20 | ○ |
| 5 | H3 Relative Level | -10 | 0.875 | ±0.20 | ○ |
| 6 | H4 Relative Level | -18 | 0.775 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 16 | 0.667 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.5 | 0.500 | ±0.15 | ○ |
| 9 | Attack Time | 2.0 | 0.325 | ±0.15 | ○ |
| 10 | Decay Time | 400 | 0.651 | ±0.30 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.05 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.5 | 0.500 | ±0.15 | ○ |
| 17 | Effective Bit Depth | 8 | 0.304 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 14.0k | 0.227 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -48 | 0.500 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.2 | 0.200 | ±0.10 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.4 | 0.400 | ±0.00 | ○ |

### Drumulator–HH: Hi-Hat (16th note clock)

**Circuit:** 8-bit sample — metronomic 16ths as clock foundation, strong-weak alternation  
**Key Discriminator:** Identical transient on every hit (zero onset CV) — machine precision with acoustic sample character

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 5.0k | 0.799 | ±0.20 | ○ |
| 2 | Spectral Centroid | 8.0k | 0.867 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 10.0k | 0.930 | ±0.25 | ○ |
| 4 | H2 Relative Level | -80 | 0.000 | ±0.00 | — |
| 5 | H3 Relative Level | -80 | 0.000 | ±0.00 | — |
| 6 | H4 Relative Level | -80 | 0.000 | ±0.00 | — |
| 7 | Highest Significant Harmonic | 20 | 0.720 | ±0.15 | ○ |
| 8 | Inharmonicity Index | 0.85 | 0.850 | ±0.10 | ○ |
| 9 | Attack Time | 0.5 | 0.175 | ±0.10 | ○ |
| 10 | Decay Time | 80 | 0.476 | ±0.20 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.00 | ● |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ● |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ● |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.4 | 0.400 | ±0.15 | ○ |
| 17 | Effective Bit Depth | 8 | 0.304 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 14.0k | 0.227 | ±0.05 | ● |
| 19 | Quantization Noise Floor | -48 | 0.500 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.25 | 0.250 | ±0.10 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.35 | 0.350 | ±0.00 | ○ |

---

## §11 — Fairlight CMI (Series I/II/III)

**Year:** 1979–1985  
**Type:** Sampler/workstation (8-bit Series I/II)  
**Synthesis:** 8-bit sampling, pitch transposition changes duration  
**Key Component:** 8-bit sampling with harsh aliasing, Page R rhythm sequencer  
**Units Produced:** ~300  
**Dictionary Songs:** EWTRTW (choir + sampled guitars + possibly resampled DMX)  

### Fairlight–OOHH1: OOHH1 (Soft choir)

**Circuit:** 8-bit sample — soft mellow choir preset  
**Key Discriminator:** 8-bit choir with aliasing fizz above natural vocal spectrum — primary layer in EWTRTW

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 300 | 0.392 | ±0.25 | ○ |
| 2 | Spectral Centroid | 1.2k | 0.593 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 3.0k | 0.808 | ±0.30 | ○ |
| 4 | H2 Relative Level | -8 | 0.900 | ±0.20 | ○ |
| 5 | H3 Relative Level | -14 | 0.825 | ±0.20 | ○ |
| 6 | H4 Relative Level | -22 | 0.725 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 12 | 0.597 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.2 | 0.200 | ±0.15 | ○ |
| 9 | Attack Time | 10 | 0.500 | ±0.20 | ○ |
| 10 | Decay Time | 5.0s | 0.925 | ±0.30 | ○ |
| 11 | Sustain Level | 0.7 | 0.700 | ±0.15 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.1 | 0.100 | ±0.10 | ○ |
| 17 | Effective Bit Depth | 8 | 0.304 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 8.0k | 0.091 | ±0.10 | ○ |
| 19 | Quantization Noise Floor | -48 | 0.500 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.4 | 0.400 | ±0.15 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.5 | 0.500 | ±0.00 | ○ |

### Fairlight–CHOIR6: CHOIR6 (Bright choir, filtered)

**Circuit:** 8-bit sample — brighter choir, mixed -4dB below OOHH1, filtered at 2kHz, detuned  
**Key Discriminator:** Secondary choir layer — filtered + detuned creates thickness when blended with OOHH1

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 300 | 0.392 | ±0.25 | ○ |
| 2 | Spectral Centroid | 1.5k | 0.625 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 3.5k | 0.824 | ±0.30 | ○ |
| 4 | H2 Relative Level | -6 | 0.925 | ±0.20 | ○ |
| 5 | H3 Relative Level | -12 | 0.850 | ±0.20 | ○ |
| 6 | H4 Relative Level | -20 | 0.750 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 14 | 0.635 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.2 | 0.200 | ±0.15 | ○ |
| 9 | Attack Time | 10 | 0.500 | ±0.20 | ○ |
| 10 | Decay Time | 5.0s | 0.925 | ±0.30 | ○ |
| 11 | Sustain Level | 0.7 | 0.700 | ±0.15 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.1 | 0.100 | ±0.10 | ○ |
| 17 | Effective Bit Depth | 8 | 0.304 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 8.0k | 0.091 | ±0.10 | ○ |
| 19 | Quantization Noise Floor | -48 | 0.500 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.4 | 0.400 | ±0.15 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.25 | 0.250 | ±0.00 | ● |
| 24 | Filter Cutoff | 2.0k | 0.667 | ±0.10 | ● |
| 25 | Filter Q | 1.0 | 0.404 | ±0.20 | ○ |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.45 | 0.450 | ±0.00 | ○ |

### Fairlight–PALMUTE: Palm-Muted Guitar (custom sample)

**Circuit:** Live palm-muted low D recorded into Fairlight, sequenced back as triggered sample  
**Key Discriminator:** Human source through machine playback — identical transient every hit but acoustic character

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 147 | 0.289 | ±0.20 | ○ |
| 2 | Spectral Centroid | 500 | 0.466 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 2.0k | 0.767 | ±0.30 | ○ |
| 4 | H2 Relative Level | -5 | 0.938 | ±0.20 | ○ |
| 5 | H3 Relative Level | -12 | 0.850 | ±0.20 | ○ |
| 6 | H4 Relative Level | -20 | 0.750 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 10 | 0.554 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.3 | 0.300 | ±0.15 | ○ |
| 9 | Attack Time | 2.0 | 0.325 | ±0.15 | ○ |
| 10 | Decay Time | 200 | 0.575 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.05 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.15 | 0.150 | ±0.10 | ○ |
| 17 | Effective Bit Depth | 8 | 0.304 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 8.0k | 0.091 | ±0.10 | ○ |
| 19 | Quantization Noise Floor | -48 | 0.500 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.35 | 0.350 | ±0.15 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.4 | 0.400 | ±0.00 | ○ |

### Fairlight–PWRCHORD: Palm-Muted Power Chord (custom sample)

**Circuit:** Live chord stab recorded into Fairlight, sequenced — chorus usage  
**Key Discriminator:** Same human-through-machine principle as PALMUTE, chord voicing

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 147 | 0.289 | ±0.20 | ○ |
| 2 | Spectral Centroid | 600 | 0.492 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 2.5k | 0.790 | ±0.30 | ○ |
| 4 | H2 Relative Level | -3 | 0.963 | ±0.20 | ○ |
| 5 | H3 Relative Level | -8 | 0.900 | ±0.20 | ○ |
| 6 | H4 Relative Level | -15 | 0.812 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 12 | 0.597 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.25 | 0.250 | ±0.15 | ○ |
| 9 | Attack Time | 2.0 | 0.325 | ±0.15 | ○ |
| 10 | Decay Time | 300 | 0.619 | ±0.25 | ○ |
| 11 | Sustain Level | 0.0 | 0.000 | ±0.05 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 1.0 | 1.000 | ±0.05 | ● |
| 16 | Noise-to-Tone Ratio | 0.15 | 0.150 | ±0.10 | ○ |
| 17 | Effective Bit Depth | 8 | 0.304 | ±0.05 | ● |
| 18 | Sample Rate Ceiling | 8.0k | 0.091 | ±0.10 | ○ |
| 19 | Quantization Noise Floor | -48 | 0.500 | ±0.10 | ● |
| 20 | Aliasing Severity | 0.35 | 0.350 | ±0.15 | ○ |
| 21 | Oscillator Count | 0 | 0.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.875 | 0.875 | ±0.00 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.00 | ● |
| 28 | Unit Variance | 0.0 | 0.000 | ±0.00 | ● |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.4 | 0.400 | ±0.00 | ○ |

---

## §12 — Hammond Organ (tonewheel)

**Year:** 1935–  
**Type:** Electromechanical tonewheel organ  
**Synthesis:** Tonewheel — integer-ratio harmonics (additive), drawbar mixing, Leslie speaker  
**Key Component:** Tonewheels (electromechanical), Leslie rotating speaker  
**Units Produced:** >100,000 (all models)  
**Dictionary Songs:** SHOUT (solo section)  

### Hammond–SOLO: Organ Solo (SHOUT)

**Circuit:** Tonewheel harmonics at integer ratios, drawbar recipe, Leslie speaker modulation  
**Key Discriminator:** Integer-ratio harmonics + Leslie FM (~0.7Hz slow / ~6Hz fast) + key click 2-4kHz <5ms

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 261 | 0.372 | ±0.30 | ○ |
| 2 | Spectral Centroid | 1.5k | 0.625 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 4.0k | 0.837 | ±0.25 | ○ |
| 4 | H2 Relative Level | -3 | 0.963 | ±0.15 | ○ |
| 5 | H3 Relative Level | -6 | 0.925 | ±0.15 | ○ |
| 6 | H4 Relative Level | -10 | 0.875 | ±0.15 | ○ |
| 7 | Highest Significant Harmonic | 9 | 0.528 | ±0.10 | ● |
| 8 | Inharmonicity Index | 0.0 | 0.000 | ±0.05 | ● |
| 9 | Attack Time | 1.0 | 0.250 | ±0.10 | ○ |
| 10 | Decay Time | 5.0s | 0.925 | ±0.30 | ○ |
| 11 | Sustain Level | 0.9 | 0.900 | ±0.10 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 0.8 | 0.800 | ±0.10 | ○ |
| 16 | Noise-to-Tone Ratio | 0.05 | 0.050 | ±0.05 | ○ |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -80 | 0.167 | ±0.15 | ○ |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 9 | 1.000 | ±0.00 | ● |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.05 | ● |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.00 | ● |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.00 | ● |
| 27 | Velocity Sensitivity | 0.0 | 0.000 | ±0.05 | ○ |
| 28 | Unit Variance | 0.3 | 0.300 | ±0.10 | ○ |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.75 | 0.750 | ±0.00 | ○ |

---

## §13 — Roland JC-120 Jazz Chorus

**Year:** 1975–present  
**Type:** Solid-state guitar amplifier with stereo chorus  
**Synthesis:** Signal processing — clean amplification + LFO-modulated delay chorus  
**Key Component:** Solid-state (no tube saturation), built-in stereo chorus  
**Units Produced:** >100,000  
**Dictionary Songs:** EWTRTW (guitar parts)  

### JC-120–CLEAN: Clean Guitar (Strat through JC-120)

**Circuit:** Solid-state clean → built-in stereo chorus (LFO-modulated delay)  
**Key Discriminator:** Extremely clean headroom, no tube harmonics, bright glassy mid-scooped tone, chorus LFO signature in stereo field

| # | Axis | Raw | Norm | Gap± | C |
|---|------|-----|------|------|---|
| 1 | Fundamental Frequency | 330 | 0.406 | ±0.30 | ○ |
| 2 | Spectral Centroid | 2.0k | 0.667 | ±0.25 | ○ |
| 3 | Bandwidth (-3dB) | 8.0k | 0.907 | ±0.30 | ○ |
| 4 | H2 Relative Level | -20 | 0.750 | ±0.20 | ○ |
| 5 | H3 Relative Level | -30 | 0.625 | ±0.20 | ○ |
| 6 | H4 Relative Level | -40 | 0.500 | ±0.20 | ○ |
| 7 | Highest Significant Harmonic | 8 | 0.500 | ±0.20 | ○ |
| 8 | Inharmonicity Index | 0.05 | 0.050 | ±0.10 | ○ |
| 9 | Attack Time | 5.0 | 0.425 | ±0.20 | ○ |
| 10 | Decay Time | 2.0s | 0.825 | ±0.35 | ○ |
| 11 | Sustain Level | 0.6 | 0.600 | ±0.20 | ○ |
| 12 | Pitch Sweep Range | 0 | 0.000 | ±0.00 | ○ |
| 13 | Pitch Sweep Duration | 0.1 | 0.000 | ±0.00 | — |
| 14 | Pitch Droop Rate | 0 | 0.000 | ±0.00 | ○ |
| 15 | Envelope Shape Code | 0.8 | 0.800 | ±0.10 | ○ |
| 16 | Noise-to-Tone Ratio | 0.05 | 0.050 | ±0.05 | ○ |
| 17 | Effective Bit Depth | 24 | 1.000 | ±0.00 | ● |
| 18 | Sample Rate Ceiling | 48.0k | 1.000 | ±0.00 | ● |
| 19 | Quantization Noise Floor | -90 | 0.062 | ±0.10 | ○ |
| 20 | Aliasing Severity | 0.0 | 0.000 | ±0.00 | ● |
| 21 | Oscillator Count | 6 | 0.750 | ±0.20 | ○ |
| 22 | Oscillator Type Code | 0.125 | 0.125 | ±0.20 | ○ |
| 23 | Filter Type Code | 0.0 | 0.000 | ±0.10 | ○ |
| 24 | Filter Cutoff | 20 | 0.000 | ±0.00 | — |
| 25 | Filter Q | 0.1 | 0.000 | ±0.00 | — |
| 26 | Waveshaping Severity | 0.0 | 0.000 | ±0.05 | ○ |
| 27 | Velocity Sensitivity | 0.3 | 0.300 | ±0.15 | ○ |
| 28 | Unit Variance | 0.1 | 0.100 | ±0.05 | ○ |
| 29 | Choke Linkage Code | 0.0 | 0.000 | ±0.00 | ● |
| 30 | Unique ID Confidence | 0.4 | 0.400 | ±0.00 | ○ |

---

## §14 — Cross-References

### Equipment → Synthesis Type
| Equipment | Primary Synthesis | Osc Type Code |
|-----------|------------------|---------------|
| TR-808 | Analog drum synthesis (bridged-T, Schmitt-trigger) | Sine/Square |
| TR-909 | Hybrid: analog + 6-bit PCM | Tri/PCM |
| DX7 | FM (6-operator) | Sine |
| Prophet-T8 | Analog subtractive (CEM) | Pulse/Saw |
| PPG-Wave | Wavetable + analog filter | PCM/Wavetable |
| LinnDrum | 8-bit sample playback | PCM |
| DMX | 8-bit sample playback | PCM |
| Drumulator | 8-bit sample playback (custom EPROM) | PCM |
| Fairlight | 8-bit sampling | PCM |
| Hammond | Electromechanical tonewheel | Sine |
| JC-120 | Signal processing (amplifier) | N/A |

### Detection Priority (808 vs 909)

1. **Hi-hats/cymbals**: Analog 6-osc metallic (808) vs 6-bit PCM sample (909) — most reliable single discriminator
2. **Exclusive voices**: Cowbell, claves, maracas, congas = 808 only. Separate crash/ride = 909 only.
3. **Bass drum character**: Clean sine (808) vs waveshaped-triangle with noise transient (909)
4. **Clap/snare interaction**: Phase artifact from shared noise gen when both trigger = 909
5. **Overall spectral character**: 808 = warmer, sub-bass. 909 = punchier, mid-freq, crunchier.

### Gap Measurement Protocol

1. **Identify** candidate voice — isolate and compare against canonical frequency ranges
2. **Measure** canonical distance — per-axis delta from Tier 1 values
3. **Characterize** the gap — Small (<15%), Medium (15–40%), Large structured (>40% coherent), Large chaotic (>40% incoherent)
4. **Cross-reference** with web engine — validate against confirmed production credits
5. **Update** variance envelope — each confirmation expands genre-specific permissible range

---

*Generated from equipment_registry_gen.py — 51 voices × 30 axes = 1530 data points*