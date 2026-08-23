# MoneyPrinterTurbo — Installation Guide

## For Humans

Copy this to your AI Agent:

```
帮我安装 MoneyPrinterTurbo：https://raw.githubusercontent.com/ehusedu/ehusedu/main/docs/moneyprinterturbo.md
```

> Source project: https://github.com/harry0703/MoneyPrinterTurbo — an all-in-one AI short video generator that turns a topic into a finished video with script, footage, subtitles, narration, and background music.

---

## For AI Agents

### Goal

Get MoneyPrinterTurbo running locally (or via Docker) for the user, configured with the LLM, TTS, and footage providers they choose, then hand back working WebUI/API/CLI access.

### ⚠️ Boundaries

- **DO NOT** run commands with `sudo` unless the user explicitly approved
- **DO NOT** write, log, or echo API keys anywhere other than `config.toml`
- **DO NOT** commit `config.toml` or any file containing credentials to git
- **DO NOT** modify system files outside the cloned project directory
- Before downloading the Whisper subtitle model (~1.6 GB) or pulling Docker images, **tell the user** and get confirmation if bandwidth/disk could be a concern
- If something requires elevated permissions, **tell the user** what's needed and let them decide

### System Requirements

| Category | Minimum | Recommended |
|----------|---------|-------------|
| CPU | 4 cores | 6-8 cores |
| RAM | 4 GB | 8 GB+ |
| GPU | Not required | 4+ GB VRAM (only needed for local Whisper subtitles) |
| Python | 3.11+ | 3.11 |
| OS | Windows 10+, macOS 11+, Linux | — |

### Step 1: Ask the user which install method they want

- **Docker** — simplest, isolates dependencies, good default for servers/Linux/macOS
- **Manual (uv)** — recommended for local dev, fastest dependency resolution
- **Manual (pip/venv)** — fallback if `uv` isn't available
- **Windows one-click package** — for Windows users who don't want to touch a terminal

### Step 2: Install

**Docker:**

```bash
git clone https://github.com/harry0703/MoneyPrinterTurbo.git
cd MoneyPrinterTurbo
docker compose -f docker-compose.release.yml up
```

WebUI: `http://127.0.0.1:8501` — API docs: `http://127.0.0.1:8080/docs`

**Manual (uv, recommended):**

```bash
git clone https://github.com/harry0703/MoneyPrinterTurbo.git
cd MoneyPrinterTurbo
uv python install 3.11
uv sync --frozen
```

**Manual (pip/venv fallback):**

```bash
git clone https://github.com/harry0703/MoneyPrinterTurbo.git
cd MoneyPrinterTurbo
python3.11 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

**Windows one-click package:** download from the project's GitHub Releases page, then run `update.bat` followed by `start.bat`.

### Step 3: Configure

```bash
cp config.example.toml config.toml
```

Ask the user which providers they want, then fill the corresponding section of `config.toml`:

- **LLM (script generation)** — pick one: Kimi/Moonshot AI, OpenAI, Anthropic Claude, Google Gemini, DeepSeek, Alibaba Qwen, Azure OpenAI, ByteDance VolcEngine Ark, xAI Grok, MiniMax, Xiaomi MiMo, or a gateway (Cloudflare AI Gateway, ModelScope, AIHubMix, AIML API, EvoLink, Ollama, OneAPI, LiteLLM, Groq, Pollinations AI). Needs an API key except for local Ollama.
- **TTS (narration)** — default is **Edge TTS** (free, no key needed). Optional: Azure TTS V2, SiliconFlow, Google Gemini TTS, Xiaomi MiMo TTS, ElevenLabs, or self-hosted Chatterbox.
- **Footage source** — Pexels, Pixabay, or Coverr (free, some need an API key) or WaveSpeed AI (paid, generates footage rather than sourcing stock clips).
- **Subtitles** — default `edge` mode (uses TTS timestamps, no GPU, fast). For frame-accurate timing, switch to Whisper:
  ```toml
  [app]
  subtitle_provider = "whisper"

  [whisper]
  model_size = "large-v3-turbo"
  ```
  This downloads a ~1.6 GB model from Hugging Face on first run — confirm with the user first.
- **Publishing (optional)** — Upload-Post integration for one-click TikTok/Instagram/YouTube Shorts publishing.

**FFmpeg:** usually auto-detected. If not, set it explicitly:

```toml
[app]
ffmpeg_path = "C:\\path\\to\\ffmpeg.exe"
```

Get a Windows build from https://www.gyan.dev/ffmpeg/builds/ if needed.

### Step 4: Run

**WebUI:**

```bash
# Windows
.\webui.bat
# macOS/Linux
sh webui.sh
```

For LAN access, set `MPT_WEBUI_HOST=0.0.0.0` before launching. Default: `http://127.0.0.1:8501`.

**API service:**

```bash
uv run python main.py
```

Default: `http://127.0.0.1:8080/docs`.

**CLI:**

```bash
uv run python cli.py --video-subject "topic"
uv run python cli.py --help
```

### Step 5: Verify

- Open the WebUI (or `/docs` for the API) and confirm the app loads without errors
- Generate one short test video end-to-end to confirm the configured LLM, TTS, and footage source all work
- Report any provider errors (bad key, quota, network) back to the user rather than silently retrying with a different provider

---

## Quick Reference

| Task | Command |
|------|---------|
| Clone repo | `git clone https://github.com/harry0703/MoneyPrinterTurbo.git` |
| Install deps (uv) | `uv python install 3.11 && uv sync --frozen` |
| Install deps (pip) | `pip install -r requirements.txt` |
| Run WebUI | `sh webui.sh` / `.\webui.bat` |
| Run API | `uv run python main.py` |
| Run CLI | `uv run python cli.py --video-subject "topic"` |
| Docker up | `docker compose -f docker-compose.release.yml up` |
| Config file | `cp config.example.toml config.toml` |
| Fonts | `resource/fonts/` |
| Background music | `resource/songs/` |

Full docs: https://github.com/harry0703/MoneyPrinterTurbo/blob/main/README-en.md
