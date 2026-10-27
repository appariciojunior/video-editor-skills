# Video Editor Skills for Claude Code

Turn Claude Code into a **first-pass video editor** for your YouTube vlogs and classes in **DaVinci Resolve Studio**. You film and make the taste calls; Claude imports, cuts retakes and dead air, keeps your B-roll, scores the video, writes clean captions, adds brand motion graphics and helps you export.

These are **skills only**: the method, the numbers, the rules and the traps, learned on real edits. There is no code in this package. Claude reads the skills and builds the small scripts with you, for your setup.

## What's inside

| Skill | Teaches Claude |
|---|---|
| `video-editor-setup` | the workspace, safe key handling, your Resolve template, which scripts to build |
| `edit-video` | the one-command flow: ingest → cut → build in Resolve → report |
| `rough-cut` | what to keep: retakes, false starts, notes to the editor, punchlines |
| `vlog-pacing` | silence detection, pads, pause rules, room at scene edges, jump-cut zoom |
| `broll` | finding and watching B-roll, cutting it by action and story |
| `resolve-scripting` | what works in the Resolve API, what silently fails, what crashes it |
| `voice-and-audio` | loudness normalization, diagnosing a muffled voice, EQ recipe |
| `music-and-sound` | Epidemic Sound cues per section, ducking envelope, tasteful SFX |
| `captions` | clean, correct, one-line captions read before import |
| `motion-overlays` | a Remotion brand kit timed to your words, face- and caption-safe |
| `figma-roundtrip` | design the kit in Figma, keep code and boards in sync |
| `youtube-export` | Deliver settings for great quality without 25 GB files |

Plus `templates/config.example.json` (starting values) and `LEARNINGS.md`, the story of what went wrong and how it was fixed.

## Install

**As a plugin** (keeps the skills together):
```
/plugin install <path-or-git-url-to-this-folder>
```
**Or copy the skills** into your project (`.claude/skills/`) or your user folder (`~/.claude/skills/`):
```
cp -R skills/* ~/.claude/skills/
```

## Use

1. Open Claude Code in an empty folder for your editing workspace.
2. Say: **"set up my video editor"**. Claude follows `video-editor-setup`, asks about your camera, Resolve project and brand, and builds the scripts with you.
3. Create `system/keys.env` yourself with one line, `SPEECH_TO_TEXT_API_KEY=your-key`. Claude never reads or prints it, and it must never be committed.
4. Drop a video in `raw video/`, open your Resolve project, say **"edit the new video"**.
5. Give feedback in plain words ("too choppy", "music louder on B-roll", "add captions", "graphics"). Claude adds every lesson to your `CLAUDE.md` so the next video starts smarter.

## You need
DaVinci Resolve **Studio** (external scripting), ffmpeg, Python 3.9+, a speech-to-text API with word timestamps. Optional: Epidemic Sound MCP, Figma MCP, Node + Remotion.

## Safety built in
Raw footage is never modified. In Resolve, Claude only **adds** (new timelines, new clips) and never touches your template or projects. Keys are never printed. Nothing is published without you.

## License
[CC BY-NC 4.0](LICENSE): free to use, share and adapt with credit. **Not for sale**: you may not sell these skills or a modified version, or use them in anything sold (paid courses, templates, products or services). For commercial use, ask the author.
