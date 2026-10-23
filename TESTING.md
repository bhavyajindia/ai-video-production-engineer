# Testing Results

All notebooks were tested end-to-end on a real machine (CPU + integrated graphics, no dedicated NVIDIA GPU with working NVENC in this test environment).

## Test Environment
- OS: Linux (Hyprland/Wayland)
- Python: 3.14.6
- FFmpeg: Available via `/usr/bin/ffmpeg`
- TTS: `edge-tts` with `en-US-AndrewNeural`
- Dependencies: `pillow`, `edge-tts`, `imageio-ffmpeg`

## Results

### CPU Only (`ai_video_cpu_only.ipynb`)
- **Smoke test (640x360 @ 12fps):** 29.25s, 1.20MB, PASS - all gates passed, status REVIEW_REQUIRED (human creative review)
- **Full render (1280x720 @ 24fps):** 29.96s, 2.08MB, PASS - all gates passed

### CPU + Consumer GPU (`ai_video_cpu_normal_gpu.ipynb`)
- **Smoke test:** 29.25s, 640x360, PASS
- **Full render (1920x1080 @ 30fps):** 29.93s, 3.7MB, PASS - NVENC probe failed, correctly fell back to libx264 with 2-worker parallel rendering

### CPU + Tesla/Colab/Kaggle (`ai_video_cpu_tesla_colab_kaggle.ipynb`)
- **Smoke test:** 29.25s, 640x360, PASS
- **Full render (1920x1080 @ 30fps):** 29.93s, 3.9MB, PASS - RAM guard capped workers 3→2, parallel rendering worked

## Quality Gate Verification (Full CPU Render)
```
[PASS] video stream exists: codec=h264
[PASS] audio stream exists: codec=aac
[PASS] duration matches timeline: 29.96s vs 29.96s
[PASS] resolution matches profile: 1280x720
[PASS] file is non-trivial: 2083447 bytes
[PASS] full decode without errors: clean
[PASS] no black segment: none
[PASS] audio is audible: max=-1.5 dB mean=-16.8 dB
[PASS] final audio not mostly silent: 0.00s silence / 29.96s
[PASS] every scene has voice audio: durations={'s1': 4.32, 's2': 5.66, 's3': 6.84, 's4': 6.26, 's5': 5.02} rms={'s1': 3526.5, 's2': 3113.0, 's3': 3409.2, 's4': 3062.4, 's5': 3274.6}
[PASS] narration track is audible: max=-2.1 dB
[PASS] frames are not black (sampled): luma samples=[39.2, 39.6, 43.6, 40.0, 31.4]
[PASS] scene count and captions valid: 5 scenes
```

## Fallback Testing
- TTS with invalid voice tested - correctly falls back to synthetic voice, marks `REVIEW_REQUIRED` with warnings, no crashes, no silent audio

## Notes
- All renders produced real playable MP4 files with actual H.264 video and AAC audio
- Voice-over is never cut or missing - durations frame-quantized from measured narration
- Parallel rendering works with RAM-aware capping
- NVENC auto-detection with safe fallback confirmed
