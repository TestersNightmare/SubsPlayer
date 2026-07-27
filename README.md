<p align="center">
  <img src="logo.ico" alt="SubtitleCAT Logo" width="82" height="82">&nbsp;&nbsp;<img src="sub_readme.png" alt="Subtitle'CAT" height="80">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows-blue.svg?style=for-the-badge&logo=windows" alt="Platform">
  <img src="https://img.shields.io/badge/Python-3.10%2B-brightgreen.svg?style=for-the-badge&logo=python" alt="Python">
  <img src="https://img.shields.io/badge/UI-PySide6-orange.svg?style=for-the-badge" alt="UI">
</p>

<p align="center">
  English | <a href="README_CN.md">中文</a>
</p>

A Windows desktop player + subtitle editor covering the full **extract → align → translate → edit → hard-sub OCR** workflow, built for producing bilingual subtitles.

SubtitleCAT combines subtitle-track probing and batch extraction, audio-based timing alignment, SRT cleaning, multi-key rotating AI translation with resume support, hard-subtitle OCR translation, and bilingual ASS production — all inside a smooth, intuitive desktop player powered by **PySide6 + mpv**.

---

## 🖥 Screenshot

![SubtitleCAT main window](Screenshot/2026-07-08_171211.jpg)

> Main window (editor-on-right layout): the mpv player on the left renders the bilingual ASS subtitles in real time, while the visual editor on the right auto-highlights and scrolls to the line currently playing. The bottom pane offers per-digit timestamp tweaking, text editing and OCR for the selected line.

---

## ✨ Key Features

*   **Subtitle track probing & batch extraction**
    *   Probes every embedded subtitle track with `ffprobe`, showing language, codec, and Default/Forced/SDH flags.
    *   Flexible extraction strategies (e.g. `eng`, `kor+sdh`, `auto`) that can be applied to all selected videos at once for multi-threaded batch extraction.
*   **Automated timeline alignment**
    *   Integrates `alass` (Audio Language-Agnostic Subtitle Aligner). Use either the **video's own audio track** (audio auto-extracted via FFmpeg) or a **known-good reference subtitle** as the baseline to fix offset and drift in a misaligned SRT.
*   **AI translation engine**
    *   Driven by the official Gemini REST API for fast responses.
    *   **Multi API-key rotation & load balancing**: configure several keys; RPM (requests per minute) and RPD (requests per day) limits are enforced automatically to avoid throttling.
    *   **Robust fault tolerance**: bisect-retry recovers from omitted or truncated lines; interrupted jobs **resume from the saved progress**.
    *   **SRT pre-cleaning**: strips leading speaker names (like `JOHN: `) and bracketed/braced annotations for cleaner input.
    *   **Automatic glossary**: before translating, the AI extracts the proper nouns of the show — people, places, organizations — and fixes their translations (saved as an editable `*.glossary.txt` next to the SRT). Episodes of the same series (matched by filename similarity) automatically reuse one shared glossary, with newly appearing terms merged back in — keeping names consistent across the whole season. Your custom glossary from Settings is merged on top (custom entries win).
    *   **Bilingual ASS output**: produce professionally styled bilingual ASS subtitles, with free choice of mono/bilingual output and original/translation ordering.
*   **PotPlayer-style visual editor**
    *   **Synced A/V rendering**: the `libmpv`-based player renders live subtitle styling exactly per the ASS spec.
    *   **Per-digit timestamp tweaking**: click any digit of a start/end time (h/m/s/ms) and use the mouse wheel or arrow keys to increment/decrement it — the video seeks along in real time.
    *   **Live playback highlight**: while playing, the editor auto-highlights and centers the subtitle line currently on screen.
    *   **Multiple layouts**: editor-right, editor-left, video-on-top, and **detached dual-window** layouts (dual-monitor friendly, with automatic window edge snapping).
*   **Smart video OCR**
    *   Drag-select a region right on the video; Gemini's multimodal vision reads the hard-coded subtitle text, translates it, and fills it into the current editor line.
*   **Internationalized UI (i18n)**
    *   Full UI support for **Chinese, English, Japanese, Korean, Vietnamese, Indonesian, Portuguese, Spanish and French** — switch takes effect after a restart.

---

## 📦 Requirements & Dependencies

> [!IMPORTANT]
> **Binary dependencies (important):**
> Because the following files are large (300 MB+ combined), **they are NOT included in this repository**. Download them from their official projects and place them into the corresponding folders:
> *   `ffmpeg.exe` / `ffprobe.exe` / `ffplay.exe` (FFmpeg components, ~100 MB each) → put into `ffmpeg/bin/`
> *   `alass-cli.exe` (alignment engine, ~3.5 MB) → put into `ffmpeg/bin/` (or the project root)
> *   `libmpv-2.dll` (player/render core, ~120 MB) → put into the project root

### 1. Software environment
*   **Python 3.10+** (make sure `Add Python to PATH` is checked during installation)

### 2. External dependency setup

1.  **FFmpeg components (`ffmpeg.exe`, `ffprobe.exe`, `ffplay.exe`)** (full build required; slim builds may fail to extract audio):
    *   **Download**: get `ffmpeg-release-essentials.zip` from [Gyan.dev FFmpeg Builds](https://www.gyan.dev/ffmpeg/builds/).
    *   **Placement**: copy `ffmpeg.exe`, `ffprobe.exe` and `ffplay.exe` from the archive's `bin\` folder into `ffmpeg\bin\` under SubtitleCAT (or add their location to the system PATH).
2.  **alass-cli.exe** (timeline alignment engine):
    *   **Download**: get the Windows `alass-cli.exe` from the [alass GitHub Release](https://github.com/kaegi/alass).
    *   **Placement**: put it into `ffmpeg\bin\` under SubtitleCAT (or the project root; you can also point to it in Settings).
3.  **libmpv-2.dll** (video playback & ASS rendering core):
    *   **Download**: from [SourceForge mpv-player-windows](https://sourceforge.net/projects/mpv-player-windows/files/libmpv/), grab the latest `mpv-dev-x86_64-*.7z` and unpack it.
    *   **Placement**: copy `libmpv-2.dll` (older builds name it `mpv-2.dll`) into the SubtitleCAT project root.

---

## 🚀 Getting Started

### One-click launch
Double-click **`run.bat`** in the project root.
*   The script first checks and installs the Python dependencies (`PySide6`, `python-mpv`, `requests`).
*   It then launches the SubtitleCAT desktop app.

*Alternatively, install and run manually:*
```bash
pip install -r requirements.txt
python app/main.py
```

### Recommended workflow
1.  **Open a video**: click "Open Video" or drag video files into the window. SubtitleCAT remembers your last folder and auto-loads a matching subtitle by name/prefix.
2.  **Extract subtitles** (if embedded): select videos and click "Extract Subs" to see all audio/subtitle tracks. Apply a batch strategy and extract embedded subtitles to external SRT in one click.
3.  **Align timing**: if the subtitle is out of sync, click "Align Subs" and use the current video's audio track as reference — `alass` does the rest.
4.  **AI translation**: pick the SRT files and click "AI Translate"; choose target language, bilingual output, cleaning rules and automatic glossary for this run. Multiple API keys rotate automatically.
5.  **Visual fine-tuning**: after translation the subtitle loads into the editor and renders on the video in real time. Play the video while proofreading the translation and adjusting timing.

---

## 🎮 Editor Shortcuts & Operation Guide

### Editor operations
| Action | Description |
| :----- | :---------- |
| **Playback sync** | While playing, the editor scrolls automatically so the line at the current time stays centered and highlighted. |
| **Per-digit time tweak** | Click any digit of a start/end time (h/m/s/ms), then **scroll up** or press `↑` to add time, **scroll down** or `↓` to subtract. The player seeks to that time for instant checking. |
| **Double-click a line** | The player jumps to the line's start time and keeps playing. |
| **Insert line** | Inserts a blank subtitle ±1s around the current time; playback pauses automatically. |
| **Delete line** | Deletes the selected subtitle line; playback pauses automatically. |
| **Line breaks** | Press Enter inside the text box for a new line, stored as `\N` in the ASS file (up to 3 lines). |
| **Ctrl + S** | Save immediately. The editor also auto-saves (0.7 s after the last keystroke) and re-renders the subtitle. |
| **Layout switch** | Cycle the four layouts: editor right → editor left → video on top → **detached dual-window** mode. |

### Player shortcuts
*(click the video area to give it focus first)*
*   `Space` / `single-click video` : pause / play
*   `←` / `→` : seek backward / forward (hold `Shift` for 1 s steps)
*   `↑` / `↓` : increase / decrease playback speed (0.25x steps)
*   `R` : reset playback speed to 1.0x
*   `Mouse wheel` (anywhere over the player) : fast seek forward / backward
*   `Volume / Brightness sliders` : drag or scroll to fine-tune at any time
*   `Audio track` & `embedded subtitle layer` : switch audio tracks or toggle the embedded subtitle layer from the control bar

---

## ⚙ FAQ

1.  **Startup says `libmpv-2.dll` not found or fails to load?**
    *   Make sure `libmpv-2.dll` sits in the SubtitleCAT root folder. 64-bit Windows requires the 64-bit DLL.
2.  **Extraction/alignment errors mentioning `ffmpeg` / `ffprobe`?**
    *   Check the paths in `config.json`. If auto-detection picked the wrong file, set the full paths to `ffmpeg.exe` and `ffprobe.exe` manually under "Settings → Tool Paths".
3.  **Gemini translation is slow or keeps failing?**
    *   Ensure your network can reach `generativelanguage.googleapis.com`. Add more API keys in Settings to raise the rotation capacity, or configure a proxy (the app follows the system-wide proxy).

---

## 🙏 Acknowledgements

SubtitleCAT stands on the shoulders of these excellent open-source projects and services — thanks to all their authors and maintainers:

| Component | Author / Team | Homepage |
| :-------- | :------------ | :------- |
| **FFmpeg** | FFmpeg team | https://ffmpeg.org |
| **FFmpeg Windows builds** | Gyan Doshi | https://www.gyan.dev/ffmpeg/ |
| **mpv / libmpv** | mpv-player team | https://mpv.io · https://github.com/mpv-player/mpv |
| **alass** | kaegi (Alana Sanchez) | https://github.com/kaegi/alass |
| **Qt / PySide6 (Qt for Python)** | The Qt Company | https://doc.qt.io/qtforpython/ |
| **python-mpv** | jaseg | https://github.com/jaseg/python-mpv |
| **pysubs2** | Tomáš Karabela | https://github.com/tkarabela/pysubs2 |
| **Requests** | Kenneth Reitz & contributors | https://requests.readthedocs.io |
| **Google Gemini API** | Google | https://ai.google.dev |
| **Claude** (development assistant) | Anthropic | https://claude.com |

---

# All rights reserved by SubtitleCAT.

    *  Commercial use is strictly prohibited.
    *  Please comply with local laws and regulations.
    *  Technology is innocent. Long live open source!
