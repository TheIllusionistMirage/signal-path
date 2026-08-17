# Signal Path

**An interactive primer on digital sound synthesis — from a single sample to a polyphonic synthesizer.**

### ▶ [**Read it live**](https://theillusionistmirage.github.io/signal-path/)

One self-contained HTML file. No build step, no dependencies, no JavaScript
libraries. Nine chapters, twelve figures, ten interactive modules.

Every sound on the page is computed sample by sample in an `AudioWorklet` from
the DSP source embedded in the file — no `OscillatorNode`, no
`BiquadFilterNode`, no pre-rendered audio. The same source is also evaluated on
the main thread to draw the figures, so the filter response curves are measured
from real impulse responses rather than plotted from formulae. Nothing on the
page is an illustration of a measurement; everything is the measurement.

The Rust listings throughout show how each piece translates to a native
implementation with `cpal` and `midir`.

---

## Contents

| | Chapter | Interactive |
|---|---|---|
| 00 | Sound as Numbers — sampling, Nyquist, quantisation, decibels, pitch | Pure tone · The fold |
| 01 | The Callback — the audio thread, phase accumulators, real-time rules | Phase accumulator |
| 02 | Fourier — harmonic series, the DFT, windowing | Additive workbench |
| 03 | Band-Limiting — aliasing, PolyBLEP, wavetables | Four ways to make a sawtooth |
| 04 | Envelopes — ADSR, exponential curves, gates, MIDI | Envelope generator |
| 05 | Filters — difference equations, poles and zeros, zero-delay feedback | Filter bench |
| 06 | Modulation — LFOs, phase modulation, sidebands | Two-operator PM |
| 07 | Voices — allocation, stealing, smoothing, denormals | The synthesizer |
| 08 | Space & Shipping — delay lines, combs, reverb, plugins | Effects rack |
| — | References — attribution and further reading | |

Play with `Z`–`M` and `Q`–`P` once a synth panel has focus, or click the keys.
Drag knobs vertically; hold `Shift` for fine control, double-click to reset.

---

## Licence

This repository is dual-licensed, split by content type.

| What | Licence | File |
|---|---|---|
| Source code — DSP engine, interface, Rust listings | [Apache-2.0](https://www.apache.org/licenses/LICENSE-2.0) | `LICENSE` |
| Prose, figures, the book itself | [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) | `LICENSE-DOCS` |

`index.html` contains both; each part is governed by its own licence.

**Apache-2.0 rather than MIT** because audio DSP has an active patent history.
Apache includes an express patent grant and defensive termination; MIT is
silent on patents. Same permissiveness, better fit for this domain.

Both licences permit commercial use. Both require attribution. Something like:

> "Signal Path" by Claude Opus 5 (Anthropic), maintained by Koushtav Chakrabarty — CC BY 4.0

See `NOTICE` for third-party algorithm attributions.

### A caveat on the licence

This document was written by an AI, and whether AI-generated material attracts
copyright is unsettled in the US, India and elsewhere. The licences here state
intent clearly, which is worth doing regardless — but don't build anything that
depends on the copyright being enforceable. Any portion of this work that isn't
subject to copyright is free to use without reference to either licence.

---

## Authorship

**Signal Path was written by Claude Opus 5, an AI model made by Anthropic**, in
August 2026 — all nine chapters, the figures, the DSP engine and the interface.
Nothing was assembled from an existing text.

That's worth knowing before you rely on it. The synthesis techniques are
long-established and credited to the people who developed them in the
References section, but an AI writing from knowledge of a field can be
confidently wrong in ways that are hard to spot. Trust your ears and an
oscilloscope over any sentence in the book, and check anything load-bearing
against the primary sources listed at the end.

What *has* been verified: the DSP passes 32 numerical assertions — PolyBLEP
cuts aliasing by ~32 dB against the naive ramp, the one-pole is −3.00 dB at
cutoff, the SVF and ladder stay stable through 200k samples of fast cutoff
modulation at maximum resonance, the reverb stays bounded, and the voice
allocator retires stranded voices correctly.

---

## Deploying to GitHub Pages

1. Push this directory to a repository with `index.html` at the root.
2. **Settings → Pages → Build and deployment → Deploy from a branch**, select
   your branch and `/ (root)`.
3. Done. There is no build step — GitHub serves the file as-is.

`.nojekyll` is included to skip Jekyll processing. It isn't strictly required
(no filenames begin with `_`), but it makes deploys faster and avoids surprises.

### Running locally

Open `index.html` in a browser. If `AudioWorklet` fails to initialise from a
`file://` origin — some browsers refuse to load a Blob module there — serve it
over HTTP instead:

```
python3 -m http.server 8000
```

The page falls back to a `ScriptProcessorNode` running the identical DSP code
if `AudioWorklet` is unavailable. The status bar at the bottom right shows
which path is active.

### Fonts

Three typefaces load from the Google Fonts CDN, so they are not redistributed
here. If you vendor the `.woff2` files for offline use, you must ship the SIL
Open Font License text alongside them. See `NOTICE`.

---

## Repository layout

```
index.html      the entire book — prose, figures, DSP, interface
LICENSE         Apache-2.0, for the code
LICENSE-DOCS    CC BY 4.0, for the prose and figures
NOTICE          copyright, authorship, third-party attributions
README.md       this file
.nojekyll       skip Jekyll on GitHub Pages
```

## Contributing

Corrections are welcome, particularly to the technical claims. If you find
something wrong, an issue with the specific sentence and what it should say is
more useful than a general note. If you believe something in here belongs to
someone and isn't credited in the References section, that's a bug worth
reporting.
