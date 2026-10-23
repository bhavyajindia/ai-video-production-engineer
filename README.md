# AI Video Production Engineer (Single-Notebook Edition)

A self-contained, production-oriented AI video pipeline delivered as **runnable Jupyter notebooks**. No separate Python packages required - every line of code is in ordered cells you can run one by one.

## Features

- **Single-notebook workflow** - All code in one `.ipynb` file, run cell by cell from top to bottom
- **CPU/GPU aware** - Auto-detects FFmpeg, CUDA/NVENC, works on CPU-only machines with automatic fallback
- **Three profiles** - CPU-only, CPU + Consumer GPU, CPU + Tesla (Colab/Kaggle)
- **Real video output** - H.264 video + AAC audio in MP4, playable in any browser/player
- **Neural voice-over** - Uses `edge-tts` with automatic synthetic fallback if TTS fails
- **Burned-in captions** - Synchronized with narration, safe margins, contrast panel
- **Procedural motion graphics** - Animated backgrounds, kinetic typography, transitions
- **Quality gates** - Decode validation, black frame detection, audio volume/silence checks, per-scene voice validation
- **Resumable & cached** - Scene renders and narration cached by content hash, reruns skip expensive steps
- **Colab/Kaggle ready** - Detects runtime and uses appropriate output paths

## Quick Start

1. Download one of the notebooks from `examples/`
2. Open in Jupyter, VS Code, Google Colab, or Kaggle
3. Run cells in order from top to bottom
4. Get `final_video.mp4` + `qa_report.json` in the output directory

## Notebooks

| Notebook | Resolution | FPS | Encoder | Parallelism | Target |
|----------|-----------|-----|---------|-------------|--------|
| `examples/ai_video_cpu_only.ipynb` | 1280x720 | 24 | libx264 | Sequential | Laptops, CPU-only machines |
| `examples/ai_video_cpu_normal_gpu.ipynb` | 1920x1080 | 30 | NVENC (if verified) / libx264 | 2 workers (RAM-capped) | Consumer GPUs |
| `examples/ai_video_cpu_tesla_colab_kaggle.ipynb` | 1920x1080 | 30 | NVENC (if verified) / libx264 | Up to 3 workers (RAM-capped) | Google Colab, Kaggle |

## Smoke Test

Set environment variable `AIVIDEO_SMOKE=1` before starting the kernel for a fast 640x360 test render (~30s).

## Sample Output

![Sample video included](final_video.mp4)

See [`qa_report.json`](qa_report.json) for the full quality gate results from a verified run.

## Requirements

- Python 3.9+
- FFmpeg and ffprobe (auto-detected, or installed via `imageio-ffmpeg`)
- Dependencies installed by the notebook: `pillow`, `edge-tts`, `imageio-ffmpeg`
- No API keys required for the default offline pipeline

## Quality Gates

Every render is validated against:
- Video/audio streams present with correct codecs
- Duration matches timeline (within 0.35s)
- Full decode with no errors
- No black segments > 0.2s
- Audio audible (max volume > -30dB)
- Every scene has real voice audio (duration >= 0.5s, RMS >= 200)
- All frames non-black (sampled at 5 points)
- Scene count and captions valid

## License

MIT License - see [LICENSE](LICENSE)
