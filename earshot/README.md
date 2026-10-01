# Earshot

A single-file web app: a listening memory game for your earbuds that uses up the
attention pain would otherwise take. Use it during a shot, or check first whether
it works on you with a 15-minute ice-water test.

Open `index.html` on a phone, put your AirPods in, and keep Transparency on.

## How it works

The game is an audio **2-back**. Four notes (low, mid, high, top), spread apart in
pitch and in stereo position, play one at a time. Tap anywhere on the screen when a
note matches the one two notes back. It starts with a 1-back warm-up, then:

- speeds up (to one note every 1.5 s) when you catch at least 75% of matches with
  few wrong taps;
- slows down (to one every 2.8 s) when you're missing a lot;
- drops back to 1-back only if slowing down isn't enough, and returns to 2-back
  once you recover.

The notes are deliberately easy to tell apart, so the work is in remembering, not
hearing. The whole screen is the answer button, so you can play with your eyes shut.

## Three ways to use it

| Mode | What happens |
| --- | --- |
| Practice | Learn the game. Nothing is saved. |
| Ice test | Two rounds with a hand in ice water: one playing, one silent, in random order. Logs seconds held (2-minute cap) and worst pain 0–10 for each, with a rest between rounds. |
| Shot | Game plus optional quiet background (brown noise and a 55 Hz ear flutter). Tap **Needle now** when it happens; the game runs 30 s more, then asks for a pain rating. |

History is stored in `localStorage` under `earshot.log.v2`. Nothing leaves the device.

## Why v2 replaced v1

v1 felt like nothing, for four reasons:

1. **It was close to inaudible.** The volume was designed to sit "barely above the
   room" and then went through an extra −12 dB cap, so the task tones played at
   about −50 dBFS.
2. **iPhone's silent switch muted it.** Web Audio plays through iOS's ambient
   channel, which the ring switch silences. v2 sets
   `navigator.audioSession.type = "playback"` and keeps a looping silent `<audio>`
   element as a fallback for older iOS.
3. **The task was the wrong kind.** "Tap on the higher pip" mostly tests hearing,
   not memory. The evidence is for working-memory load (n-back), so v2 uses that.
4. **Half the runs were designed to do nothing.** Blind mode picked one of four
   setups at random, and two of them were noise-only control conditions.

On top of that, there's no way to feel an effect without pain to compare against,
which is what the ice test is for.

## What to expect

A modest effect, not numbness. In lab studies, 1-back and 2-back tasks lowered
perceived intensity of cold and painful stimuli, harder levels helped more up to a
ceiling, and the effect showed up in the spinal cord's response. Pain ratings fall
more reliably than endurance does. Sound on its own, without a task, mostly changes
unpleasantness rather than intensity.

Earbuds can't numb the needle site. Vibration and cold work only near the jab, so
for the sting itself, add ice on the spot beforehand, or a vibrating device held
a few centimetres above it (with the nurse's OK).

## Not a medical device

A distraction aid built from published research. It makes no diagnostic or
treatment claim. Skip the ice test if you have heart problems, high blood pressure,
Raynaud's, circulation problems or diabetes-related nerve damage, or if you're
pregnant.
