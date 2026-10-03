# 🎬 VIDEO FETCH — Neo-Brutalist Video Downloader

A full-stack, local-first video downloader built for mobile devices. Paste any video link from YouTube, Instagram, TikTok, or Facebook, pick your quality, and the file saves directly to your device storage — no cloud, no database, no accounts.

![Status](https://img.shields.io/badge/status-active-CCFF00?style=for-the-badge&labelColor=000000)
![Platform](https://img.shields.io/badge/platform-android-CCFF00?style=for-the-badge&labelColor=000000)
![Python](https://img.shields.io/badge/python-3.14-CCFF00?style=for-the-badge&labelColor=000000)
![License](https://img.shields.io/badge/license-MIT-CCFF00?style=for-the-badge&labelColor=000000)

---

## 📌 Overview

**VIDEO FETCH** is a self-hosted video downloader that runs entirely on an Android device using **Termux** as the backend engine and a **Neo-Brutalist web interface** as the frontend. It leverages `yt-dlp` and `ffmpeg` to fetch, merge, and save videos or audio from multiple platforms.

This project was built as a university demonstration to showcase a complete full-stack application running natively on mobile — no servers, no cloud, no external dependencies.

---

## ✨ Features

- 🎥 **Multi-Platform Support** — YouTube, Instagram, TikTok, Facebook
- 🎚️ **Quality Selection** — 1080p, 720p, 360p, or Audio-only (MP3)
- 📜 **Download History** — Stored locally in browser via localStorage (with thumbnails)
- 📁 **Custom Save Location** — Choose any folder on your device
- ⚡ **Live Progress Bar** — Real-time download progress with smooth animations
- 🎨 **Neo-Brutalist UI** — Bold typography, hard borders, solid offset shadows
- ⚠️ **Smart Error Handling** — Human-readable messages for private, restricted, or unavailable videos
- 🎵 **Audio Extraction** — MP3 conversion at 192kbps via ffmpeg
- 🌙 **Dark Theme** — High-contrast industrial aesthetic

---

## 🛠️ Tech Stack

### Backend
| Tech | Purpose |
|------|---------|
| **Python 3.14** | Core language |
| **FastAPI** | Local HTTP server & REST API |
| **Uvicorn** | ASGI server |
| **yt-dlp** | Video extraction & downloading |
| **ffmpeg** | Audio/video merging & MP3 conversion |
| **Threading** | Non-blocking background downloads |

### Frontend
| Tech | Purpose |
|------|---------|
| **HTML5** | Page structure |
| **CSS3** | Neo-Brutalist styling |
| **Vanilla JavaScript** | Logic, API calls, localStorage |
| **Google Fonts** | Ranchers, Space Mono, Plus Jakarta Sans |

### Environment
- **Termux** (Android terminal emulator)
- **Acode** (Mobile code editor)

---

## 🏗️ Architecture
┌─────────────────────────────────────────────────────┐
│                    ANDROID DEVICE                   │
│                                                     │
│  ┌──────────────┐         ┌──────────────────────┐  │
│  │   BROWSER    │         │       TERMUX         │  │
│  │              │         │                      │  │
│  │  index.html  │◄───────►│  FastAPI (main.py)   │  │
│  │  style.css   │  HTTP   │  Port 8000           │  │
│  │  script.js   │         │         │            │  │
│  │              │         │         ▼            │  │
│  │  localStorage│         │  yt-dlp + ffmpeg     │  │
│  └──────────────┘         │         │            │  │
│                           │         ▼            │  │
│                           │  /sdcard/Download/   │  │
│                           └──────────────────────┘  │
│                                                     │
└─────────────────────────────────────────────────────┘

**Flow:**
1. User pastes a link in the browser
2. Browser sends a `POST /download` request to `localhost:8000`
3. FastAPI spawns a background thread running `yt-dlp`
4. Progress is tracked and polled via `GET /status/{job_id}`
5. Once complete, `GET /file/{job_id}` streams the file to the device
6. History is saved to `localStorage`

---

## 🚀 Setup Guide

### Prerequisites
- Android phone (Android 7+)
- Termux installed (from F-Droid recommended)
- Acode installed (or any mobile code editor)
- Stable internet connection

### Step 1: Install Termux Packages

```bash
pkg update && pkg upgrade -y
pkg install python ffmpeg git -y
termux-setup-storage
```

Grant storage permission when prompted.

Step 2: Install Python Libraries

```bash
pip install "fastapi<0.100" "pydantic<2" yt-dlp uvicorn
```

Step 3: Create Backend

```bash
mkdir -p ~/videodownloader
cd ~/videodownloader
nano main.py
```

Paste the backend code (see main.py in this repo), then save with CTRL + X, Y, Enter.

Step 4: Start the Server

```bash
uvicorn main:app --host 0.0.0.0 --port 8000
```

You should see: INFO: Uvicorn running on http://0.0.0.0:8000

Step 5: Set Up Frontend

In Acode, create these files inside /sdcard/Download/VideoDownloader/website/:

· index.html
· style.css
· script.js

Paste the respective code for each file.

Step 6: Run It

Open index.html in Chrome (via file manager → Open with Chrome). Paste a video link, pick quality, hit DOWNLOAD.

---

📖 Usage

1. Paste a URL — from YouTube, Instagram, TikTok, or Facebook
2. Select Quality — 1080p HD, 720p, 360p, or Audio Only (MP3)
3. Set Save Path (optional) — e.g., /sdcard/Download/video
4. Click DOWNLOAD — watch the progress bar fill up
5. File saved — check your chosen folder; history appears below

Custom Save Locations

Path Description
/sdcard/Download/VideoDownloader Default
/sdcard/Download/video Custom example
/sdcard/Movies For videos
/sdcard/Music For MP3s
📁 Project Structure

```
VideoDownloader/
├── website/
│   ├── index.html      # Main UI
│   ├── style.css       # Neo-Brutalist design
│   ├── script.js       # Logic & API calls
│   └── README.md       # This file
└── downloads/          # Saved videos (runtime)
```

Termux side:

```
~/videodownloader/
└── main.py             # FastAPI backend
```

---

🎨 Design Philosophy

The UI follows a strict Neo-Brutalist design system:

· Colors: Black (#000000), Dark Grey (#121212), White (#FFFFFF), Volt Green (#CCFF00)
· Typography:
  · Ranchers — Bold headlines
  · Space Mono — Technical labels
  · Plus Jakarta Sans — Body text
· Borders: 4px or 8px solid black, no rounded corners > 8px
· Shadows: Solid offsets, no blur (neo-shadows)
· Layout: High-contrast sections separated by heavy borders

---

⚠️ Error Handling

Error Type Message Shown
Private video "This video is private. Login is required."
Age-restricted "This video is restricted to certain audiences."
Invalid link "Invalid link. Please paste a valid video URL."
Deleted/removed "This video is unavailable."
Copyright block "Download blocked due to copyright."
Server down "Cannot connect to server. Make sure Termux is running."

---

🐛 Troubleshooting

Problem Solution
Could not import module "main" Make sure you're in ~/videodownloader folder before running uvicorn
Address already in use Run pkill -f uvicorn then restart
pip install fails on pydantic Use pip install "pydantic<2" "fastapi<0.100"
Videos not downloading Update yt-dlp: pip install -U yt-dlp
Server stops in background Use termux-wake-lock to keep alive
CORS errors Already handled in backend via middleware

---

🔒 Legal Disclaimer

This project is built strictly for educational purposes as a university demonstration. It is intended to run locally on the developer's own device and is not published, distributed, or hosted publicly.

Downloading videos from YouTube, Instagram, TikTok, and Facebook may violate their respective Terms of Service. Users are responsible for ensuring they have the right to download any content. Do not use this tool to download copyrighted material without permission.

---

🔮 Future Roadmap

☐ Playlist / batch downloads
☐ Subtitle extraction
☐ Download pause/resume
☐ Dark/Light theme toggle
☐ Native Android APK wrapper
☐ Multi-language support

---

👤 Author

M4G-CREATIVE

· GitHub: @m4g-creative
· Project: VIDEO FETCH — Neo-Brutalist Video Downloader

---

📜 License

This project is licensed under the MIT License. See LICENSE file for details.

---

🙏 Acknowledgements

· yt-dlp — Powerful video downloader
· FastAPI — Modern Python web framework
· Termux — Android terminal emulator
· Google Fonts — Ranchers, Space Mono, Plus Jakarta Sans

---

<p align="center">
  <strong>BUILT FOR SPEED. SHIPPED WITH PRIDE.</strong><br>
  <sub>© 2026 M4G-CREATIVE</sub>
</p>
