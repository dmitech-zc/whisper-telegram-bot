<div align="center">

<!-- Logo / Header -->
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://img.icons8.com/fluency/96/microphone.png">
  <source media="(prefers-color-scheme: light)" srcset="https://img.icons8.com/fluency/96/microphone.png">
  <img alt="Whisper Telegram Bot" src="https://img.icons8.com/fluency/96/microphone.png" width="96">
</picture>

# Whisper Telegram Bot

**Transcribe voice messages in Telegram using Whisper or Gemini**

[![Python](https://img.shields.io/badge/Python-3.12-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED?style=for-the-badge&logo=docker&logoColor=white)](https://docker.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)](LICENSE)
[![Telegram Bot API](https://img.shields.io/badge/Telegram-Bot_API-26A5E4?style=for-the-badge&logo=telegram&logoColor=white)](https://core.telegram.org/bots/api)

<br>

[Getting Started](#-getting-started) · [Configuration](#-configuration) · [Engines](#-engines) · [Docker Commands](#-docker-commands) · [Contributing](#-contributing)

---

</div>

## Overview

A lightweight, Dockerized Telegram bot that converts voice messages, audio files, video notes, and video messages into text. Choose between two powerful transcription engines depending on your needs:

| | **Gemini** | **Whisper** |
|:---|:---|:---|
| **Speed** | Fast (cloud) | Depends on hardware |
| **Privacy** | Sends audio to Google | Fully offline |
| **Cost** | Free tier available | Free (self-hosted) |
| **Languages** | 100+ | 99 languages |
| **Setup** | API key only | Downloads model (~1 GB) |

## Features

- **Dual Engine** — Switch between Gemini (cloud) and Whisper (local) with a single env var
- **Multi-format** — Voice messages, audio files, video notes, and video messages
- **Access Control** — Optional whitelist by Telegram user ID
- **VAD Filtering** — Whisper engine uses Voice Activity Detection for cleaner output
- **Docker First** — One command to build and run, model cache persisted across restarts
- **Lightweight** — Single Python file, minimal dependencies

## Getting Started

### Prerequisites

- [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/)
- A Telegram bot token from [@BotFather](https://t.me/BotFather)
- *(Optional)* A Gemini API key from [Google AI Studio](https://aistudio.google.com/apikey)

### Quick Start

```bash
# 1. Clone the repository
git clone https://github.com/dmitech/whisper-telegram-bot.git
cd whisper-telegram-bot

# 2. Configure environment
cp .env.example .env
# Edit .env — set BOT_TOKEN and GEMINI_API_KEY

# 3. Launch
docker compose up -d
```

The bot is now running. Send it a voice message in Telegram!

## Configuration

All settings are managed through environment variables in the `.env` file:

### Core Settings

| Variable | Description | Default |
|:---|:---|:---|
| `BOT_TOKEN` | Telegram bot token from @BotFather | *required* |
| `ALLOWED_USERS` | Comma-separated user IDs (empty = allow all) | ` ` |
| `STT_ENGINE` | Transcription engine: `gemini` or `whisper` | `gemini` |

### Gemini Engine

| Variable | Description | Default |
|:---|:---|:---|
| `GEMINI_API_KEY` | API key from Google AI Studio | *required* |
| `GEMINI_MODEL` | Gemini model to use | `gemini-2.0-flash` |
| `GEMINI_LANGUAGE` | Target language for transcription | `Hebrew` |

### Whisper Engine

| Variable | Description | Default |
|:---|:---|:---|
| `WHISPER_MODEL` | Model name or HuggingFace repo | `ivrit-ai/faster-whisper-v2-d4` |
| `WHISPER_DEVICE` | Compute device: `cpu` or `cuda` | `cpu` |
| `WHISPER_COMPUTE` | Precision: `int8`, `float16`, `float32` | `int8` |
| `WHISPER_LANGUAGE` | Language code or `auto` | `he` |
| `WHISPER_BEAM_SIZE` | Beam search width | `5` |

## Engines

### Gemini (Recommended for most users)

Uses Google's Gemini 2.0 Flash model for fast, accurate transcription. The free tier includes 15 requests/minute and 1M tokens/day — more than enough for personal use.

```env
STT_ENGINE=gemini
GEMINI_API_KEY=AIza...
```

### Whisper (For privacy-conscious users)

Runs [faster-whisper](https://github.com/SYSTRAN/faster-whisper) locally inside Docker. No data leaves your machine. First startup downloads the model (~1 GB), which is cached in a Docker volume.

```env
STT_ENGINE=whisper
WHISPER_MODEL=ivrit-ai/faster-whisper-v2-d4
```

> **Tip:** For English transcription, use `WHISPER_MODEL=large-v3` and `WHISPER_LANGUAGE=en`.

## Docker Commands

```bash
docker compose up -d          # Start in background
docker compose logs -f        # Stream logs
docker compose restart        # Restart after config changes
docker compose down           # Stop the bot
docker compose up -d --build  # Rebuild after code changes
```

## Project Structure

```
whisper-telegram-bot/
├── bot.py               # Main bot logic (single file)
├── Dockerfile            # Container image definition
├── docker-compose.yml    # Service orchestration
├── requirements.txt      # Python dependencies
├── .env.example          # Configuration template
├── .dockerignore         # Docker build exclusions
├── .gitignore            # Git exclusions
└── LICENSE               # MIT License
```

## Supported Media Types

| Type | Telegram Object | Supported |
|:---|:---|:---:|
| Voice message | `voice` | Yes |
| Audio file | `audio` | Yes |
| Video note (circle) | `video_note` | Yes |
| Video | `video` | Yes |

## Contributing

Contributions are welcome! Feel free to open an issue or submit a pull request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Built with**

[![python-telegram-bot](https://img.shields.io/badge/python--telegram--bot-21.0+-blue?style=flat-square&logo=telegram)](https://github.com/python-telegram-bot/python-telegram-bot)
[![faster-whisper](https://img.shields.io/badge/faster--whisper-1.1+-green?style=flat-square)](https://github.com/SYSTRAN/faster-whisper)
[![Google Gemini](https://img.shields.io/badge/Google_Gemini-2.0_Flash-4285F4?style=flat-square&logo=google)](https://ai.google.dev/)

</div>
