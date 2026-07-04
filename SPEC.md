# Detailed spec

## Proven FFmpeg filter strings
These exact filters were tested and produce good YouTube output. Adapt sizes to
the selected resolution (examples shown at 1920x1080).

### 1. Waveform (scrolling oscilloscope)
```
showwaves=s=1920x1080:mode=line:rate=30:colors=0x00FF00
```
Green line on black. Both channels overlaid.

### 2. Vectorscope (Lissajous X/Y)
```
aformat=sample_fmts=fltp,avectorscope=s=1920x1080:zoom=1.5:rc=0:gc=200:bc=0:rf=0:gf=40:bf=0:draw=line
```
Green trace with slow fade (gf=40). The aformat conversion to fltp is required.

### 3. Spectrum (CQT frequency analyzer)
```
showcqt=s=1920x1080:count=1:fps=25
```
Full-range frequency display, bass at bottom.

## Encode settings
- Video: libx264, crf 22, preset ultrafast (wasm is slow; ultrafast is the right call),
  pix_fmt yuv420p, fps matching the filter (30 for waveform/vectorscope, 25 for spectrum)
- Audio: AAC 320k stereo
- Container: MP4

Full example command shape:
```
ffmpeg -i input.wav -filter_complex "[0:a]<FILTER>[v]" -map "[v]" -map 0:a \
  -c:v libx264 -preset ultrafast -crf 22 -c:a aac -b:a 320k -pix_fmt yuv420p -r <FPS> out.mp4
```

## Lossless stereo merge (two mono WAVs)
When the user supplies separate L and R mono files, merge with FFmpeg before
visualizing (do NOT decode/re-encode the PCM):
```
ffmpeg -i left.wav -i right.wav -filter_complex "[0:a][1:a]join=inputs=2:channel_layout=stereo[a]" -map "[a]" -c:a pcm_s24le stereo.wav
```
Match the output codec to the input bit depth (pcm_s16le for 16-bit, pcm_s24le for
24-bit). Validate both files have identical sample rate and frame count; if frame
counts differ, warn the user and pad the shorter with silence.

Typical input from the user's Tascam DR-44WL: 48 kHz, 24-bit mono pairs named
like `260609_0072S3.wav` (S3 = left) and `260609_0072S4.wav` (S4 = right).
Offer a convention toggle but default S3=L, S4=R.

## UI flow
1. Drop zone accepts 1 or 2 files. Two files = show L/R assignment (auto-guess
   from S3/S4 suffix), one file = treat as final audio.
2. Visualization picker: three cards with a small looping preview or static
   representative image of each style.
3. Resolution select (default 1080p).
4. Render button -> progress bar with percent and elapsed/estimated time.
5. Done -> video preview element + Download button.
   Also offer "Download stereo WAV" when a merge was performed.

## Edge cases
- Non-WAV input (MP3, M4A): accept it; ffmpeg decodes it fine. Merge path is WAV-only.
- Mismatched sample rates between L/R: refuse with a clear message.
- Very long files: warn above ~15 minutes of audio that browser rendering may
  take a long time or run out of memory; suggest splitting.
- Mobile: show a notice that desktop is recommended (memory + speed).
