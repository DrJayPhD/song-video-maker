# Song Video Maker - starter package

Converts songs into YouTube-ready visualization videos, entirely in the browser.
This folder is a ready-to-go Claude Code workspace. Nothing is built yet;
Claude Code will build it from CLAUDE.md and SPEC.md.

## What's in here
- `CLAUDE.md` - project context Claude Code reads automatically every session
- `SPEC.md` - detailed spec including the proven FFmpeg filter strings
- `.claude/settings.json` - pre-approved permissions so Claude rarely interrupts you

## Setup (one time)
1. Install Claude Code if you haven't:
   `npm install -g @anthropic-ai/claude-code`
2. Put this folder somewhere permanent, e.g. `~/Projects/song-video-app`
3. Open a terminal in the folder:
   `cd ~/Projects/song-video-app`

## Launch
Run:
```
claude --permission-mode auto
```
Auto mode approves routine safe operations in the background and only stops you
for things that matter. Combined with the pre-approved list in
.claude/settings.json, you should see very few prompts.

## First prompt to give Claude Code
> Read CLAUDE.md and SPEC.md, then build the app. Start with a working
> single-file version (index.html) that handles one stereo file and the
> waveform style end to end, and show me it running locally before adding
> the other two visualizations and the two-file merge path.

## Good habits
- Let it commit as it goes (git commands are pre-approved).
- Ask it to start a local server and test in your browser at each milestone.
- When it's working, ask: "Deploy this to GitHub Pages" and it will walk
  through creating the repo and enabling Pages. The git push step will ask
  for approval once, by design.
