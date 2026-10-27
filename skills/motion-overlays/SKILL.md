---
name: motion-overlays
description: Brand motion graphics for creator videos - a Remotion component kit (key phrases, lists, lower thirds, journeys, big words…) timed to the creator's words from a beats sheet, rendered as transparent ProRes and placed on V2+ in DaVinci Resolve, with face-safe and caption-safe layouts. Use for "overlays", "motion graphics", "make it interesting", "graphics", or when overlays cover the face or fight the captions.
---

# Motion overlays (Remotion kit)

The creator's cut is approved and usually re-edited by them. You add graphics **on top**; their picture and audio are never touched.

## The kit
A Remotion project with one composition (`Beat`) that renders any component from props, at the timeline's size and fps, transparent. Components read every value from a brand tokens file (`brand.json`: colours, fonts, sizes at a 1080-high unit, padding, motion timings) so the look changes in one place. Typical set:

| Component | Use for |
|---|---|
| LowerThird | name + role, first appearance |
| KeyPhrase | the sentence that carries a point (`*word*` = highlighter) |
| ListReveal / Flow | items or steps appearing as they're said |
| Compare / BeforeAfter | two sides, a change |
| CountUp | a number they say |
| Quote / PersonalNote | a strong line, an opinion |
| BigWord | one idea, full frame, 1–2 s |
| CommentPrompt | a question for the comments |
| VlogOpener, PlaceTime, Journey, TimeJump | vlog moments: title, where/when, travel A → B, "3 hours later" |
| TitleCard / EndCard / ChapterTag | structure, only if asked |

Motion that creators liked: **one smooth ease-in-out move per element**, glide in, hold, glide out. No pop, no bounce. Light or regular weights at big sizes, monospace uppercase labels, generous padding.

## Each time
1. **Which timeline** (never the template).
2. **Map words → frames** on that timeline (walk V1 items: source in-point, timeline start → every kept word's timeline time).
3. **Plan beats** and propose them (time · component · copy), **wait for OK**. Roughly one every 30–60 s. Copy = their words, numbers exactly as said, names checked on the web, nothing invented, nothing they want secret, the right **story direction** (a trip *to* somewhere vs. *going back*).
4. **beats.json**: `id`, `component`, `at_word` (or `at_seconds` on B-roll, where there are no words), `end_word` / `seconds`, `data` with `at_word` cues for items.
5. **Render** (transparent ProRes 4444; full-frame components are large, several GB for ~16 beats), **check**, then place on the first free track from V2 up.
6. Overlay SFX: off by default unless the creator likes them (subtle whoosh at most).

## Check over the real frame before placing (non-negotiable)
Grab the frame under each beat from the camera file, render the overlay still, composite them, and **look**. Also draw the **caption band** on it. We caught all of these only this way:
- **Face:** panels designed for 16:9 (right third free) covered the face in a **4:3 selfie frame**. Fix in the kit: when the frame is narrower than 16:9, scale side panels as a whole to the outer ~34% of the width, 80 px from the edge, and scale full-frame type to its 16:9 proportions so it stays below the chin.
- **Overflow:** an 11-item list didn't fit until scaled; more than ~11 items need another layout.
- **Captions:** bottom-anchored elements (opener, big word, lower third, journey) sat in the subtitle band. Fix in the kit: a token like `caption_band_top_at_1080` and every bottom offset = max(own, band top).
- **Widows:** a highlighted word followed by "." wrapped the dot onto its own line. Fix in the kit: glue punctuation to the highlight's last word (`nowrap`) and use `text-wrap: pretty` on display text.

Fix layout problems **in the kit**, never per video, so every future video inherits the fix.

## Replacing beats
Change the beat, render under a **new id/filename** (`-v2`), remove *your* old clip, place the new one. Re-running must skip beats already placed and never re-add something the creator deleted without saying so.

## Report
One line per beat (time, component, on-screen copy), what you fixed in the kit, and anything tight (small text, near the face).
