# Sonora

**A fast audition and editing player with loudness metering.**

Sonora helps audio editors, sound designers, and anyone who works with
professional audio move quicker from hearing a file to shaping it.

[Website](https://sonora-player.github.io/Sonora/) · [Docs](https://sonora-player.github.io/Sonora/guide/) · [Download](https://github.com/sonora-player/Sonora/releases) · [Roadmap](ROADMAP.md) · [Support Sonora](#support-sonora)

---

## What is Sonora?

Sonora is a Windows desktop player built around **folder-based audition**:

- Open a directory and hear samples / stems in place — no mandatory library
- SCM / DCM click modes, Keep Position, A-B loop, cues, drag-out clips
- See level, loudness (EBU R128), and spectrum while you play
- Batch rename, convert, and CSV export without leaving the player

### Recent highlights (0.9.6)

- **NCM** support: open NetEase Cloud Music `.ncm` files (decrypts in-memory to MP3/FLAC/M4A/…)
- 0.9.5: File List header column dividers; safer delete/rename

## Supported formats

Decoded with Symphonia:

| Format | Description | Extensions |
| --- | --- | --- |
| WAV | Wave / Broadcast Wave (BWF) | `.wav` `.wave` `.bwf` |
| AIFF | Audio Interchange File Format | `.aif` `.aiff` `.aifc` |
| FLAC | Free Lossless Audio Codec | `.flac` |
| MP3 | MPEG Audio Layer III | `.mp3` |
| OGG | Ogg Vorbis | `.ogg` |
| OPUS | Opus Interactive Audio Codec | `.opus` |
| AAC / M4A | MPEG-4 AAC (ALAC in MP4/M4A when present) | `.m4a` `.aac` `.mp4` |
| CAF | Apple Core Audio Format | `.caf` |
| NCM | NetEase Cloud Music encrypted container (decrypts to MP3/FLAC/M4A/…) | `.ncm` |
| Video (audio) | Audio track inside video containers | `.mp4` `.m4v` `.mkv` `.webm` `.mov` |

Not supported yet: APE, WavPack, TTA, DSD, WMA, MIDI, tracker modules, AMR, Musepack.

Full manual: [Docs](https://sonora-player.github.io/Sonora/guide/) · in-app **Help → User Manual**.

## Download

Visit **[Releases](https://github.com/sonora-player/Sonora/releases)** for the latest build.

| Package | Notes |
| --- | --- |
| `Sonora_*_x64-setup.exe` | NSIS installer (file associations) |
| `Sonora-*-portable.exe` | Run without installing |

Windows may show SmartScreen for unsigned builds: **More info → Run anyway**.
A proper Authenticode code-signing certificate (OV/EV) is required to remove that warning; configure it in Tauri via `bundle.windows.certificateThumbprint` once you have a cert.

## Support Sonora

**Sponsor the project** · **支持 Sonora 开发**

Sonora Free is free to use. If it helps your work, voluntary support keeps
development and maintenance going.

| Channel | Region | Link |
| --- | --- | --- |
| Ko-fi | International | [ko-fi.com/sonoraaudio](https://ko-fi.com/sonoraaudio) |
| 爱发电 | 中国大陆 | [afdian.com/a/sonoraaudio](https://afdian.com/a/sonoraaudio) |

Website: [Support Sonora](https://sonora-player.github.io/Sonora/#support)

> Support is entirely voluntary and does not purchase software ownership,
> priority technical support, or any specific feature commitment.
>
> 赞助完全自愿，不代表购买软件所有权、优先技术支持或特定功能承诺。

## Sonora Pro

Planned capabilities (no schedule): batch loudness CSV reports, CLI, macOS /
Linux, library database, loop auto-editing, ASIO, and more — see [ROADMAP.md](ROADMAP.md).

## Feedback

Use [GitHub Issues](https://github.com/sonora-player/Sonora/issues) for bugs and ideas.

## License

[Sonora Freeware License](LICENSE) — free to use the official binaries; source
code is not published in this repository.
