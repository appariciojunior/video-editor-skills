# Learnings from real edits

Short notes on what went wrong on real videos and the rule that came out of it. The skills already include these; this file is the "why".

## Cutting
- **Cut from words, not from volume.** A volume-only silence cut gave ~800 pieces on a 34-minute class; the transcript cut gave 120 and was loved.
- **Classes like it tight, vlogs don't.** 0.5 s max pause / 0.3 s kept was perfect for a class and "popping" for a vlog. Vlog: 1.0 / 0.6.
- **Scenes need room.** Starting 0.1 s before the first word and ending right after the last made every scene slam into the next. 0.5 s in, 0.6 s out (more on the ending), stopping before the hand reaches the camera.
- **Zoom only on jump cuts.** Zooming on scene changes added pops; a new scene opens at 100%.
- **A punch-in lasts one line.** Without a split after the punchline, one push-in held 83 seconds.

## B-roll
- **A transcript cut drops silent clips.** Treat 0-word clips (and clips whose "words" are radio lyrics) as B-roll.
- **Watch it properly.** 5 frames per clip led to cuts the creator called "weird and wrong". At 2 fps the story appeared: camera into the bag → black → out of the bag at the airport, a free transition.

## Sound
- **Music louder where nobody talks.** -19 LUFS on B-roll was too timid; -16 with a slow rise starting in the speaker's tail made the music flow into the B-roll.
- **Check the edited music track's ending.** An exact-length edit faded out 25 s early, under the B-roll it was meant to lift.
- **Literal foley is cheesy.** Zipper, cloth swooshes and a jet pass-by were "horrible". One soft untuned swell into each B-roll, quiet room tone, and the clip's own sound worked.
- **A silent music track can be a template setting.** The template's music track had a track-level AI Music Remixer on; anything on that track was silent. Found only by rendering and correlating, then looking at the track Inspector.
- **Normalizing can hurt.** A silent clip was boosted +30 dB into hiss.

## Voice
- **Dual mono reads 3 dB hot.** Loudness measured on one channel and normalized to −16 played back near −13. Measure the stereo you deliver.
- **"Underwater" voice = measure it.** Channels were identical (not a phase issue); the spectrum had almost nothing above 3 kHz: a covered mic. EQ fixes it; Voice Isolation helps; EQ isn't scriptable.

## Captions
- **Captions are for reading, not a court transcript.** "pro-product", "es--", "Hããã" were flagged as errors. Clean them.
- **Correct written language.** The creator speaks slang; captions use the correct forms. Fix grammar, keep the voice.
- **Your own filters can break words.** A filler rule removed the article "um" everywhere. Read every cue.
- **Resolve's subtitle import shifts everything to the first cue.** Start the file at 00:00.

## Overlays
- **16:9 layouts cover the face in 4:3.** Scale panels into the free strip; shrink full-frame type.
- **Overlays and captions fight for the bottom.** Reserve the caption band in the kit.
- **No lone "." on a line.** Glue punctuation to the highlight; pretty text wrapping.
- **Get the story right.** The outro said "see you in a new country" while the creator was flying back home. Ask when unsure; don't name places they want to keep secret.
- **Re-render under a new name.** Same-name files can play stale cached frames.

## Resolve API
- `Timeline.ApplyGradeFromDRX` fails in 21; the per-item node graph works.
- `AppendToTimeline` end is exclusive; audio-only clips count frames at their own rate; timecode counts 24 frames per second at 23.976.
- Subtitles via the API land after the timeline end; a workaround crashed Resolve.
- External scripts render only with `StartRendering([job], False)`, into a path without spaces. Restore the Deliver settings afterwards.
- **The free edition can be scripted, from the inside.** No external scripting, but `.lua` in Workspace > Scripts reaches the Resolve API (every call we tried worked). Its sandbox lost `loadfile` and `SavePrefs` in 21.1.2, so data goes in embedded in the script and comes out through the log.
- **A green mock isn't a green Resolve.** `fuscript` has no sandbox; the same script failed in the menu.

## Picture
- **Phone/webcam footage gets a measured CDL.** Simulate it on real frames, apply with `SetCDL`, check an exported still against the simulation.
- **Part of the frame = a cropped copy on the track above.** Windows aren't scriptable; crop + CDL is.
- **Grade with the fade.** A graded copy that cut at an OBS crossfade would pop; one-frame pieces with opacity following the measured fade don't.

## Export
- "Automatic" quality made 25 GB for 11 minutes. 50 Mb/s H.264 (or 30 Mb/s H.265) is plenty for YouTube.
- Match colour tags to the project (Rec.709-A → "Same as Project") or YouTube looks faded on Apple devices.
