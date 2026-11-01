---
name: video-editor-setup
description: Sets up an AI first-pass video editing workspace for DaVinci Resolve Studio with Claude Code - folders, tools, API keys handled safely, the Resolve template, and which small scripts to build first. Use for "set up my video editor", "start the video editing system", or before the first /edit-video when no workspace exists yet.
---

# Video editor: setup

You are going to be a **first-pass editor**. The creator films, you import, cut, mark, score and caption; **they make the taste calls**. Everything below was learned editing real YouTube vlogs and classes. Build the workspace with the user, step by step, and ask before installing anything.

## What the user needs

- **DaVinci Resolve Studio** (the free version has no external scripting). In Preferences > System > General set *External scripting using: Local*, restart Resolve. On the free version the build runs from the Workspace > Scripts menu, with limits: see `resolve-scripting` → *Free edition*.
- **ffmpeg / ffprobe**, Python 3.9+, Node 18+ (only for motion graphics).
- A **speech-to-text API** with word timestamps and verbatim mode (we used ElevenLabs Scribe: word-level timings, audio events like "(laughs)", auto language detection). Any service with per-word start/end works.
- Optional: an **Epidemic Sound** account connected as an MCP server (music + SFX), **Figma** MCP (brand boards), **Remotion** (rendered overlays).

## Folder layout (create it with them)

```
<workspace>/
  raw video/            the creator drops footage here: one file, or a folder of clips. READ-ONLY for you.
  export edited video/  the creator exports here. Never touch.
  brand/                BRAND.md (look + voice rules), brand.json (tokens), logo, sfx/
  system/
    config.json         all defaults (see templates/config.example.json in this package)
    keys.env            API keys. NEVER print, echo, cat or read it into the conversation.
    scripts/            the small scripts you write together (list below)
    looks/              exported grade stills (.drx) for log footage
    motion/             the motion-graphics kit (Remotion project)
    videos/<video>/     per-video work: transcript, decisions, cuts, music bed, captions, overlays
  CLAUDE.md             house rules + a "Learned facts" list you keep adding to (dated, one line each)
```

## Hard rules to write into CLAUDE.md on day one

1. Never modify, move or copy the raw footage. Resolve plays the camera files in place; you write only small JSON/text into `system/videos/<v>/`.
2. **In Resolve, only add.** Import into bins, create *new* timelines (a rebuild makes "v2", never overwrites), add on free tracks. Never change the creator's template timeline, their own timelines or their media. Never switch, create or close projects.
3. Transcripts stay verbatim. Re-transcribing costs money: ask first.
4. Keys stay secret (see above). Load them from the file inside scripts only.
5. **Decisions are by word index, timing is by script.** You decide which words stay; a script turns that into frame-exact cuts. Never hand-edit generated cut files.
6. If a step fails twice, stop and report the exact error.

## The scripts to build (in this order)

Write each as a small, boring CLI that prints a short summary. Keep them in `system/scripts/`.

| Script | Does |
|---|---|
| `doctor` | checks ffmpeg, Python, the key file exists (without printing it), Resolve reachable, which project is open |
| `ingest <v>` | probe each clip (fps, size, audio layout), read the camera's sidecar XML for picture profile, transcribe, detect silences, write a manifest |
| `plan_timeline <v>` | keep-list + moments → frame-exact cuts, pauses, markers, a human-readable review file (see `rough-cut` + `vlog-pacing`) |
| `resolve_build <v>` | import, copy the template timeline, lay the cut, zoom, grade, markers, audio normalize (see `resolve-scripting`) |
| `settings <v>` | per-video overrides on top of config.json (`set cut.max_pause=1.0`) |
| `score_mix <v>` | music cues + ducking envelope + scene SFX (see `music-and-sound`) |
| `captions <v>` | SRT from the transcript, cleaned and corrected (see `captions`) |
| `overlays <v>` | word→frame map of a timeline, render beats, place them (see `motion-overlays`) |
| `clean <v>` | moves a finished video's work files to the Trash after the creator says yes |

## Ask the creator once, then save to CLAUDE.md

- Camera, lens, picture profiles (log vs. baked), frame rate they shoot, microphones.
- Their Resolve project: bins (we used 01_Footage, 02_Audio, 03_Graphics, 04_Timelines), the **template timeline** name, which audio track is voice (A1), music (A2), SFX (A3).
- Languages they speak on camera.
- Do they talk to "the editor" while recording ("editor, put a video here")? Those lines become markers.
- Their brand (colours, fonts, padding, motion feel) for the motion kit.

Then hand over to `edit-video`.
