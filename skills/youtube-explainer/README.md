# youtube-explainer

Workflow for one Vietnamese educational YouTube video: brief, script, storyboard, Remotion motion, on-screen text, procedural SFX, review, MP4, and upload metadata.

Each video stays in `projects/<slug>/` of the consumer workspace.

No speech synthesis, voice packs, or TTS model. Duration comes from storyboard reading holds.

Requires Node.js, a Remotion project, and ffmpeg for the final probe. SFX needs the Python engine from [whaleyxbt/claude-motion](https://github.com/whaleyxbt/claude-motion), which this skill does not vendor.
