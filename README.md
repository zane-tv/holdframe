# video-skills

Agent skills for Vietnamese educational YouTube videos: script, storyboard, Remotion motion, on-screen text, procedural SFX, review, and upload metadata.

No voice. This repo does not include speech synthesis, voice packs, or a TTS model.

## Skills

| Skill | What it does |
| --- | --- |
| **youtube-explainer** | Produce one video in `projects/<slug>/`: brief, script, storyboard, timeline, motion, SFX, MP4, YouTube metadata. |
| **motion-design** | Timing, easing, springs, type, color, and continuity rules for Remotion. From [claude-motion](https://github.com/whaleyxbt/claude-motion). |
| **review-loop** | Render a draft, inspect contact sheets, fix, repeat. From claude-motion. |
| **sound-design** | Procedural SFX cue sheets and loudness checks. Voiceover section removed. From claude-motion. |
| **web-sims** | Simulation or 3D videos as one HTML page, captured frame by frame. From claude-motion. |

`motion-design`, `review-loop`, `sound-design`, and `web-sims` are instruction files. The SFX engine and `sims/` capture scripts stay in [whaleyxbt/claude-motion](https://github.com/whaleyxbt/claude-motion). Installing a skill does not install those runtimes.

## Install

```bash
npx skills add zane-tv/video-skills --skill youtube-explainer -y
npx skills add zane-tv/video-skills --skill motion-design --skill review-loop --skill sound-design --skill web-sims -y
```

Pass `-a claude-code`, `-a codex`, `-a cursor`, or `-a opencode` to target one agent. Add `-g` for a user-global install.

## Layout

```text
skills/
  youtube-explainer/SKILL.md
  motion-design/SKILL.md
  review-loop/SKILL.md
  sound-design/SKILL.md
  web-sims/SKILL.md
```

## License

MIT. Upstream skill text is copyright whaleyxbt. See [LICENSE](LICENSE).
