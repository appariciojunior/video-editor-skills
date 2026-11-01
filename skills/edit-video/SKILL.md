---
name: edit-video
description: The one-command first-pass edit. Takes footage from raw video/, transcribes it, cuts retakes and dead air while keeping a natural vlog rhythm, keeps B-roll, and builds the timeline in the open DaVinci Resolve project (footage in bins, a copy of the creator's template timeline, markers, zoom, grade, audio normalize). Use for "/edit-video", "edit the new video", "do it", or follow-up changes to a cut.
---

# Edit video (the whole flow)

The creator drops footage in `raw video/`, opens their Resolve project and says "do it". You run the whole thing and stop only for:
- **a frame-rate mismatch** between footage and template timeline (ask; never choose for them),
- **a failure that happened twice** (report the script's exact error and what they must fix).

Prerequisite: `video-editor-setup` is done (folders, scripts, CLAUDE.md).

## 1. Check
Run your `doctor` script. No video named? Pick what's in `raw video/` without a finished build report; if several, ask which.

## 2. Ingest
Probe, read picture profiles, transcribe (verbatim, word timestamps), detect silences, write a manifest. Report anything odd:
- picture profile *guessed* (no camera sidecar) → say so; never put a log LUT on non-log footage;
- voice on one channel only (wireless receiver in stereo mode) → make sure the voice plays on both sides;
- mixed frame rates; **vertical clips** (they will crop hard in a horizontal or 4:3 frame).

## 3. Cut
Follow `rough-cut` for what to keep (by word index) and `vlog-pacing` for how much air to leave. Clips with no speech are B-roll: follow `broll`. A subagent ("story editor") is a good fit: give it the video name and the skills; it writes the keep-list and the markers and runs the planner.

## 4. Build
Follow `resolve-scripting`. Order inside the build:
1. footage into the footage bin;
2. copy the template timeline into the timelines bin as "<video> - Transcript cut" (v2, v3… never overwrite) and empty the copy (it keeps the track FX chain);
3. set a custom frame if asked (e.g. 4:3 2880x2160, footage *scaled to fill with crop*);
4. lay the cut on V1/A1, alternate jump-cut zoom, colour B-roll pieces green;
5. grade: a log look on log footage only; a measured CDL correction on other footage if the creator asks (see `resolve-scripting`);
6. markers for spoken notes (pink);
7. **audio treatment last**: Normalize Audio Level on A1 (see `voice-and-audio`).

## 5. Report and stop
Short: timeline name, raw → cut length, number of pieces, each retake removed (timestamp + a few words, translated if needed), each marker and what goes there, punch-ins, B-roll placed, anything guessed or unsure. Then stop. They watch it.

## Follow-ups (they speak in plain words)

| They say | You do |
|---|---|
| "put back X", "cut 02:14 to 02:20" | edit the keep-list, re-plan, rebuild as v2 |
| "too choppy / pops" | loosen pauses (see `vlog-pacing`), give scenes room, rebuild |
| "add music" | `music-and-sound` |
| "captions" | `captions` |
| "graphics / overlays" | `motion-overlays` |
| "my voice sounds bad" | `voice-and-audio` |
| "export settings?" | `youtube-export` |
| "always …" | change the global default, and add a dated line to CLAUDE.md |

They will often fix things by hand in the timeline you built (trims, deleted clips). **Always re-read the timeline from Resolve before the next step** (B-roll positions, what they deleted) and never put back what they removed.
