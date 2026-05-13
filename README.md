# ⚡ YTForge — YouTube Toolkit

A hackathon-ready YouTube toolkit powered by **yt-dlp**, **Flask**, and **React**.

---

## Features

| Feature | Description |
|---|---|
| 📋 Video Info | Title, channel, views, likes, duration, description, available formats |
| ⬇️ Download | Download in best/1080p/720p/480p quality (MP4) |
| 🎵 Audio Extract | Rip audio as MP3, M4A, WAV, FLAC, or Opus |
| 📝 Transcribe | Full transcript via OpenAI Whisper (falls back to auto-subtitles) |
| 🖼️ Thumbnails | Fetch all thumbnail resolutions with direct download links |
| 💬 Subtitles | Download SRT subtitles (manual or auto-generated) for any language |
| 📑 Chapters | Display all chapter timestamps and titles |
| 📂 Playlist | List all videos in a YouTube playlist |

---

## Project Structure

```
ytforge/
├── app.py              ← Flask backend (all API routes)
├── requirements.txt    ← Python dependencies
├── run.sh              ← One-shot setup & launch script
├── downloads/          ← Downloaded files (auto-created)
└── frontend/
    ├── index.html
    ├── package.json
    ├── vite.config.js
    └── src/
        ├── main.jsx
        └── App.jsx     ← Full React UI
```

---

## Quick Start

### Prerequisites
- Python 3.9+
- Node.js 18+
- ffmpeg (required by yt-dlp for merging video+audio)

**Install ffmpeg:**
```bash
# macOS
brew install ffmpeg

# Ubuntu/Debian
sudo apt install ffmpeg

# Windows
winget install ffmpeg
```

### Run (automated)

```bash
chmod +x run.sh
./run.sh
```

This installs all deps and starts both servers. Open **http://localhost:3000**.

---

### Manual Start

**Backend:**
```bash
pip install flask flask-cors yt-dlp
python app.py
# Runs on http://localhost:5000
```

**Frontend (new terminal):**
```bash
cd frontend
npm install
npm run dev
# Runs on http://localhost:3000
```

---

## Optional: Whisper Transcription

For best transcription accuracy, install OpenAI Whisper:

```bash
pip install openai-whisper
# Also needs torch — may take a few minutes
```

Without Whisper, transcription falls back to YouTube's auto-generated subtitles (English only).

---

## API Endpoints

| Method | Endpoint | Body | Description |
|---|---|---|---|
| GET | `/api/health` | — | Health check |
| POST | `/api/info` | `{ url }` | Video metadata |
| POST | `/api/download` | `{ url, format }` | Start download job |
| POST | `/api/audio` | `{ url, audio_format }` | Start audio extraction job |
| POST | `/api/transcribe` | `{ url }` | Start transcription job |
| POST | `/api/thumbnail` | `{ url }` | Get thumbnail URLs |
| POST | `/api/subtitles` | `{ url, lang }` | Start subtitle download job |
| POST | `/api/chapters` | `{ url }` | Get chapter list |
| POST | `/api/playlist` | `{ url }` | List playlist items |
| GET | `/api/job/<id>` | — | Poll job status |
| GET | `/api/file/<name>` | — | Download a completed file |

Long-running operations (download, audio, transcribe, subtitles) return a `job_id`.  
Poll `/api/job/<job_id>` until `status` is `"done"` or `"error"`.

---

## Ideas to Extend

- **AI Summary** — pipe transcript into Claude/GPT for a TL;DR
- **Batch downloader** — queue multiple URLs
- **Clip cutter** — download just a time range with ffmpeg
- **Format converter** — convert between video/audio formats
- **Speech search** — search transcript for keywords with timestamps
- **Browser extension** — add a "YTForge" button on YouTube pages

---

## Tech Stack

- **yt-dlp** — YouTube downloading engine
- **Flask** — Python web framework
- **OpenAI Whisper** — local speech-to-text (optional)
- **React 18 + Vite** — frontend
- **ffmpeg** — audio/video processing
