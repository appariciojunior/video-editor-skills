---
name: voice-and-audio
description: Voice treatment for creator videos in DaVinci Resolve - loudness normalization, diagnosing a muffled or "underwater" voice from the spectrum, Voice Isolation, an EQ recipe, and fixing clips that normalization made worse. Use when building the timeline's audio step or when the creator says their voice sounds bad.
---

# Voice and audio

## Always, as the last build step
Normalize Audio Level on every voice-track piece:
- mode **ITU-R BS.1770-4**, target level **-2.0 dBTP**, target loudness **-16 LKFS**, set level **Independent**.
- Check the result line ("normalized N pieces"); if it fails, give the creator these values to do by hand (select A1 → Normalize Audio Levels).

## After normalizing, check for side effects
- A **silent** clip (B-roll with no sound) gets boosted up to +30 dB → hiss. Set it back to 0 dB.
- A B-roll clip with **music/radio** in it will fight the music bed: pull it down ~8 dB.

## "My voice sounds muffled / underwater"
Measure before touching anything:
1. **Phase check:** are left and right identical (true dual mono)? Compute L−R. If two different mics are summed (camera mic + lav), comb filtering causes exactly this sound: use one channel only.
2. **Spectrum:** compare band levels of a 20 s talking section: below 300 Hz, around 1 kHz, above 3 kHz, above 6 kHz. A clean voice has its 3 kHz+ band roughly 10–15 dB below the total; we measured **~22 dB** below with most energy under 300 Hz → the mic was covered (under a jacket) or badly placed.

## Fix (most of it is EQ, which is not scriptable)
- Scriptable: **Voice Isolation 50** per talking clip (cleans room and noise; leave B-roll alone so its natural sound stays).
- By hand (Inspector → Audio → EQ on one clip, then Paste Attributes with **only Equalizer ticked**, never Volume, so the normalize gains survive):
  - high-pass 80 Hz
  - −3 dB at 250 Hz, Q ~1.2 (boom)
  - +3 dB at 3.5 kHz, Q ~1 (presence)
  - +2 dB high shelf at 10 kHz (air)
- Ask before driving the UI with computer-use; the creator may prefer to do it.

## Without Voice Isolation or scriptable EQ (free edition)
Treat the voice outside Resolve and place it on a new track. The camera file is only read, and the original audio track is muted, not removed:
- ffmpeg chain: one channel if L/R are identical → `highpass=f=80` → `-3 dB @ 250 Hz` (boom) → `+2 dB @ 3.5 kHz` → `treble=g=2:f=9000` → gentle `acompressor` (−26 dB, 2.5:1) → back to stereo → two-pass `loudnorm` (−16 LUFS, −2 dBTP, `linear=true`) → 24-bit WAV.
- Skip denoising when the noise floor between words is already near −80 dBFS (OBS noise suppression). It only makes the voice sound processed.
- **Measure loudness on the final stereo file.** Dual mono reads ~3 dB louder than the same signal measured as mono: normalizing the mono to −16 delivers about −13.
- Check that the output length equals the source's, or it drifts out of sync.

## Prevention (tell the creator once)
Clip the lav on the outside of clothing, 15–20 cm below the chin; in stereo-mode receivers with one transmitter the voice is on one channel only.
