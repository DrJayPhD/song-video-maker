# Song Video Maker

## What this is
A single-page web app that converts audio (WAV/MP3) into YouTube-ready MP4 videos
with an audio visualization. Runs entirely in the browser using ffmpeg.wasm.
No server, no uploads. Audio never leaves the user's machine.

Built for the eXplorers community (makers and builders who create focus music).
Will be hosted as a static site on GitHub Pages or Netlify.

## Core requirements
1. User drops in ONE stereo audio file, or TWO mono WAVs (left + right) to merge losslessly first.
2. User picks one of three visualization styles (exact FFmpeg filter strings in SPEC.md):
   - Waveform (showwaves) - scrolling oscilloscope
   - Vectorscope (avectorscope) - Lissajous X/Y plot
   - Spectrum (showcqt) - frequency analyzer
3. User picks resolution: 720p, 1080p (default), or 4K.
4. Output: H.264 + AAC 320k MP4, yuv420p, downloadable.
5. Show encode progress (ffmpeg.wasm reports progress events). Browser encoding is
   slow (3-10x real time) so the progress bar matters.

## Tech constraints
- Use @ffmpeg/ffmpeg (ffmpeg.wasm) single-threaded build. The multithreaded build
  requires SharedArrayBuffer / cross-origin isolation headers, which GitHub Pages
  does not send. Stick to single-thread.
- Vanilla HTML/JS/CSS in as few files as possible. No build step, no framework.
  The whole app should deploy by copying files to a static host.
- Warn users if input file is very large (browser memory ceiling ~2GB total working set).

## Design direction
- Brand tokens: navy #1a2744 (primary background), gold #c9a04e (accent).
- Aesthetic: studio equipment / oscilloscope. Dark, precise, instrument-like.
- The privacy story is a feature: state plainly on the page that audio never uploads.
- Keep copy plain and direct. Sentence case. Active voice.

## Working style
- Commit early and often with clear messages.
- Test in the browser after significant changes (open index.html or use a local server).
- Do not add dependencies beyond ffmpeg.wasm without asking.
- Never use the word "honestly" in any copy. Avoid em dashes in copy; use commas or periods.
