---
name: youtube-explainer
description: "Create Vietnamese educational YouTube videos from a topic, optional reference video, and duration. Use for engaging scripts, storyboard, Remotion motion graphics, on-screen captions, procedural SFX, rendering and YouTube metadata. Keep each video isolated in projects/<slug>/. Do not synthesize speech or install voice packs."
---

# YouTube Explainer

Produce a complete educational video, not a slideshow or a source-code-only deliverable. Adapt the narrative and visual style to the topic; do not reuse another video's facts, scenes or timestamps.

No voice. Do not synthesize narration, clone a speaker, download a TTS model, or copy voice packs. Timing comes from the storyboard. Speech, if the user already has a recording, is a given file — never something this skill creates.

## 1. Project placement — required

- Locate the consumer workspace using its root `AGENTS.md` when one exists. Every video lives in `projects/<slug>/`, with a lowercase kebab-case slug.
- Continue an existing matching project when revising it. Never overwrite another video's directory, source, outputs or metadata.
- Keep all topic-specific files inside that project. Do not generate `src/`, `public/`, `out/`, scripts, timelines or upload metadata at workspace root.
- Run project commands with the project as working directory, or use `npm --prefix projects/<slug> run <command>` from workspace root.
- Resolve paths from the script's own location, not a hardcoded `/Users/...` path. For `projects/<slug>/scripts/*.py`, the project is the script's grandparent and the workspace is the project's grandparent.

```text
workspace/
└── projects/
    └── <slug>/
        ├── README.md            # brief, dependencies, commands, delivery paths
        ├── references/          # reference video, source material, assets
        ├── script.md            # script + storyboard
        ├── timeline.json        # storyboard beats in seconds
        ├── youtube-metadata.md  # title, description, chapters, hashtags
        ├── package.json
        ├── package-lock.json
        ├── tsconfig.json
        ├── remotion.config.ts
        ├── scripts/             # contact sheets, audio review
        ├── sfx/                 # procedural sound engine + this video's cue sheet
        ├── src/                 # composition, scenes, components, on-screen text
        ├── public/              # SFX and any caller-supplied audio
        └── out/                 # final MP4, review frames, sheets, waveform
```

`script.md` is the readable script and storyboard. Do not keep a second editable copy.

## 2. Brief and reference analysis

Extract the topic, audience, language, requested duration, aspect ratio, visual reference and delivery format from the request. Default to Vietnamese, YouTube 16:9, 1920×1080 and 30 FPS only when the user has not specified alternatives. Follow explicit duration requirements; do not assume every video must be six minutes long.

Do not ask for, select, or default a narration voice.

Inspect the reference video at the opening, several explanation beats, transitions and the ending. Record:

- Background, palette, font style and support for Vietnamese diacritics.
- Layout and safe area; adapt portrait references to landscape rather than stretching them.
- Drawing/reveal motion, object continuity, pacing, reading holds and caption placement.
- Effect and music treatment. Borrow the visual language, not unrelated factual content. Do not copy a reference voice.

Load `motion-design`, `review-loop`, `sound-design` and relevant Remotion guides before using their tools. A skill installation alone does not install the runtime engines those guides mention. `sound-design` and `web-sims` expect the engines from [whaleyxbt/claude-motion](https://github.com/whaleyxbt/claude-motion). Verify the actual project files and dependencies first.

Use `web-sims` only when simulation-style content makes it a better fit than Remotion. Keep the same `projects/<slug>/` contract.

## 3. Script and storyboard

1. Open with a concrete question, surprising contrast or relatable example. Make the viewer want the answer before introducing terminology.
2. Use one running example to connect the explanation. Build intuition, then reveal notation or mechanisms, then show an application and limitations.
3. Explain one new idea per visual beat. Plan what changes on screen while each sentence is read; avoid walls of text and static cards for entire paragraphs.
4. Keep displayed notation correct in diagrams. Short on-screen labels may differ from the full sentence. Do not generate spoken audio for symbols.
5. Check claims, formulas, units, assumptions and worked examples. Distinguish teaching data from observed real-world results. Do not invent benchmark scores, citations or historical claims for dramatic effect.
6. End by resolving the opening question and recapping the essential ideas. Add a natural bridge to a related topic only if it serves the explanation.

Estimate duration from storyboard beats and reading holds, not from a speech synthesizer. Meet a minimum duration through useful explanation and readable holds, not empty frames or repeated filler.

## 4. Timeline and on-screen text

- Write `timeline.json` from the storyboard before animating. Every beat is a named timestamp in seconds. Scenes, SFX and review read that file.
- Hold each readable state long enough to read: at least 1 second for a short phrase, about 0.25 seconds more per word beyond three, at least 2 seconds for the final hook.
- Put captions and labels in the composition, timed to those beats. This is on-screen text, not forced alignment of a voice track.
- Do not describe estimated reading time as measured speech. There is no narration track to align.
- If the user supplies an existing audio file, place that file. Do not create, clone, or download a voice to produce it.

## 5. Motion implementation

Use Remotion for on-screen text, UI and diagram explanations.

- All animation must be a deterministic function of frame/time. Avoid wall-clock time, unseeded randomness and CSS animations that diverge during rendering.
- Store named scene beats in `timeline.json`. Use offsets relative to those beats for reveals and transitions.
- Match the reference deliberately: for a neon chalkboard reference, use a dark canvas, restrained neon accents, hand-drawn outlines, draw-on paths and legible Vietnamese handwriting. Do not force this style onto a different reference.
- Use generated raster assets only for isolated icons, abstract symbols or genuinely useful static illustrations that are difficult to build in code. Do not generate detailed scenic backgrounds, complete slides, diagrams with baked-in labels or UI pages. Build trees, data, mathematical diagrams and simulations in code so every part can animate and stay correct.
- Keep figures, axes, labels and equations mathematically consistent. Display only results actually calculated or explicitly labeled illustrative.
- Prefer continuous visual transformations over unrelated slide changes: a point becomes a prediction, a residual becomes an area, a parameter change moves the fitted object.
- Keep key text still while it is read. Allow sufficient holds; transitions should not create empty frames.
- Reserve space for on-screen captions. Check long formulas, dense labels and the smallest intended viewing size, not just the full-resolution canvas.
- Ensure frame 0 communicates the subject and the final frame holds long enough to read.

## 6. Sound, render and review

1. Lock storyboard timing before placing SFX. Accent significant reveals, transitions and interaction beats. Do not add a voice track.
2. Keep SFX audio in `public/` when the video uses the sound engine. Do not render a narration bus or a voice mix.
3. If the video has SFX or music, measure loudness. Do not report targets as measured results. Picture plus SFX is a valid deliverable. A missing voice track is not a blocker.
4. Preview the composition and render a short representative range or key stills before a full render.
5. Inspect all chapters and difficult transitions for collisions, clipping, unreadable formulas, excessive glow, black gaps and caption overlap. Fix and re-render changed regions.
6. Export the requested MP4. Preserve the last valid deliverable while checking a replacement.
7. Probe the encoded file for resolution, aspect ratio, FPS, duration and any audio streams. Generate final contact sheets and inspect them; stills from an earlier draft are not proof of the final render.

Never describe the deliverable as complete while its requested timing, duration or final MP4 is missing. Report the real blocker if a required runtime or asset is unavailable. Do not block on voice synthesis.

## 7. YouTube metadata and delivery

Write `youtube-metadata.md` inside the project with:

- One recommended, accurate title; optional alternatives without unsupported superlatives.
- A copy-paste description that explains the topic and viewer benefit.
- Chapters beginning at `00:00`, derived from the final timeline and checked against the encoded video. Keep chapter durations at least ten seconds for YouTube chapters.
- Relevant hashtags in the description. Do not invent channel links or source links.
- Optional comma-separated Studio tags, separate from visible hashtags.

Deliver the project-relative MP4 path, metadata path, actual measured duration and resolution, and only checks that were performed. Do not report a speaker or voice. Keep supporting evidence in that project's `out/` directory.

## Project commands

A new topic must have its own package scripts, composition ID, cue sheet and final filename. Typical scripts are `studio`, `sfx`, `render` and `sheet`. Do not add a `voiceover` script.
