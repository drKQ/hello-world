# Earshot

A single-file web app that uses AirPods (or any earbuds) to blunt the pain of an
injection, built around what the literature actually supports.

Open `index.html` on a phone, put the buds in, keep Transparency on.

## What it does

| Phase | Length | What happens |
| --- | --- | --- |
| Fit & threshold | — | L/R check, then a 1 kHz tone you turn *down* until it nearly vanishes into the room. Sets the working level. |
| Prime | 60s | Brown-noise bed, 4-in/6-out breath pacing, optional 10 Hz binaural pair. Runs before the needle is near you. |
| Load | open-ended | An adaptive auditory vigilance task — tap on the higher pip. Difficulty tightens as you get better, so attention stays fully committed. |
| The moment | 25s | Triggered by you or the clinician. Long exhale cue, flutter depth and tremolo rate peak, pip rate speeds up. |
| Score it | — | 0–10 pain and distraction ratings saved to `localStorage`. |

## The design decisions that came from evidence, not vibes

- **Quiet on purpose.** Sound blunted pain in mice only at ~5 dB above ambient;
  louder did nothing ([Science, 2022](https://www.science.org/doi/10.1126/science.abn4663)).
  Hence threshold calibration and a hard `-12 dBFS` output cap, plus optional
  mic-based tracking that lifts the signal by at most 8 dB in a noisy room.
- **Active, not passive.** Distraction that consumes attention outperforms
  background media in needle-procedure trials, which is why there is a scored
  task rather than a playlist.
- **Prime before, not just during.** The one consistent finding in the binaural
  beats literature is that pre-exposure beats exposure-during-only.
- **Flutter is attention, not gate control.** 40–110 Hz in a sealed canal reads
  as a tickle. It does *not* close the spinal gate for a needle in your arm —
  that gate is segmental, which is why the Buzzy device sits on the skin next to
  the needle. No shipping AirPod has a haptic motor.
- **Blinded n-of-1 by default.** Each run silently picks one of four builds
  (everything / task only / flutter only / bed only) and reveals it after you
  rate the pain. Over ~10 runs the per-build averages say whether any of it is
  doing something for you, rather than asking you to trust a mechanism story.

## Implementation

No dependencies, no network, no assets. Everything is synthesised at runtime with
the Web Audio API: brown/pink noise buffers, an AM'd low sine pair for flutter
with a slow inter-aural walk, bandpass noise swells for the breath, panned
triangle pips for the task. Ratings live in `localStorage` under
`earshot.log.v1`; nothing leaves the device, including the mic stream.

## Not a medical device

A distraction aid, not a treatment, with no diagnostic or therapeutic claim.
Expect a modest effect on top of the things that actually work — topical
anaesthetic, a good injector, breathing out on insertion. Skip it if you need to
hear instructions during a procedure, or if you have hyperacusis or
noise-triggered tinnitus.
