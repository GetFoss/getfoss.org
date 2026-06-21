---
title: 'Streamlink: Command-Line Streaming for Twitch, YouTube, and More'
description: Streamlink is a FOSS command-line utility that extracts video streams from various platforms and pipes them directly into media players like VLC, bypassing web browsers.
date: 2026-06-21T22:15:00+03:00
draft: false
image: /images/streamlink.webp
categories:
  - audio-video
tags:
  - Video
  - Streaming
  - Linux
  - Utilities
aliases: []
---

Streamlink is a command-line utility that extracts video streams from hundreds of platforms—Twitch, YouTube, and many others—and pipes them directly into your preferred media player, typically VLC. It solves a specific problem: modern web browsers are resource hogs, and streaming platforms load their web players down with telemetry, intrusive ads, and bloated JavaScript.

Streamlink strips away the web interface entirely, giving you the raw video stream. It is Free and Open Source Software, built on a community-driven plugin system, meaning support for new services is added continuously.

## Why It Matters (The FOSS Angle)

Streaming platforms continuously alter their APIs and web players to inject ads and restrict access. The FOSS model allows Streamlink’s community to counter this rapidly. When a platform changes its backend, a community member writes or updates a plugin.

Recent releases demonstrate this ongoing maintenance cycle. Release 8.4.0 patched a significant security vulnerability (CVE-2026-44353), an arbitrary local file read via `file://` URIs in HLS and DASH streams. This is the benefit of transparent, auditable code—vulnerabilities are found and patched in the open.

The development trajectory also shows a focus on network control and stream reliability. Release 8.3.0 introduced the ability to select network interfaces by name on non-Windows systems, a feature sysadmins use for traffic shaping and routing. Release 8.2.1 updated HLS stream naming to include framerate data and deprecated re-exported attributes from `streamlink.stream`, pushing users toward proper API usage. Plugins like `goltelevision`, `cdnbg`, and `telefe` were completely rewritten, proving the plugin architecture remains actively maintained against platform hostility.

## Key Features

- **Plugin Architecture:** Streamlink relies on a modular plugin system. Adding support for a new streaming service requires writing a plugin, not hacking the core application.
- **Player Agnostic:** By default, it pipes streams to VLC, but it supports any media player capable of reading standard input.
- **Stream Selection:** You can specify stream quality directly from the CLI (e.g., `best`, `1080p60`, `audio_only`), bypassing adaptive bitrate players that drop resolution unpredictably.
- **Encrypted Passthrough:** The `--stream-passthrough-encrypted` option passes encrypted HLS/DASH segments to the output stream without internal checks, useful for specific backend processing.
- **Network Interface Binding:** The `--interface` flag allows binding the stream connection to a specific network interface by name, critical for multi-NIC setups or VPN routing.
- **AppImage Distribution:** Linux users can run portable AppImages containing Python, Streamlink dependencies, and FFmpeg, entirely sidestepping system package conflicts.
- **Isolated Execution:** Official documentation strongly advocates for installation via `venv` or `pipx`, respecting system Python environments.

## System Requirements

Streamlink requires **Python 3.10 or newer**.

**Build dependencies** include `setuptools` (>=65.6.0), `wheel`, and `versioningit` (>=2.0.0).

**Runtime dependencies** include `certifi`, `isodate`, `lxml`, `pycountry`, `pycryptodome`, `PySocks`, `requests`, `trio`, `trio-websocket`, `urllib3`, and `websocket-client`. `exceptiongroup` and `typing-extensions` are required for Python versions older than 3.11.

**Optional dependencies:**

- [**FFmpeg**](https://getfoss.org/audio-video/ffmpeg-cross-platform-audio-and-video-processing-framework/)**:** Required for muxing multiple video/audio/subtitle streams into a single output stream. DASH streams with video and audio content always require remuxing.
- [**brotli**](https://github.com/google/brotli) **/** [**zstandard**](https://github.com/facebook/zstd)**:** Used for decompressing HTTP responses.

For Linux AppImages, the system requires **glibc 2.28 or newer** (August 2018) and FUSE for mounting the SquashFS image. Supported architectures are x86_64 and aarch64.

## Real-World Usage

The default behavior pulls the highest quality stream from Twitch and plays it in VLC:

```bash
streamlink twitch.tv/CHANNEL best
```

This bypasses the Twitch web player entirely.

**Network Routing**
If you want to route your stream traffic through a specific VPN interface (e.g., `wg0`), use the interface selection:

```bash
streamlink --interface wg0 twitch.tv/CHANNEL 1080p60
```

**Python Environment Management**
If you install Streamlink via `pip` on Linux, do not use `sudo`. System-wide Python package installations conflict with system package managers and break dependencies. Install it per-user:

```bash
pip install --user -U streamlink
```

A better approach is using `pipx`, which handles the virtual environment automatically:

```bash
pipx install streamlink
```

**Ubuntu Caveat**
Ubuntu inherits the Streamlink package from Debian but does not maintain or update it. The package is often severely outdated. Ubuntu users should use the AppImage, `pipx`, or install from the Debian backports repository.

## Download & Installation

- **Official Website:** [Streamlink](https://streamlink.github.io/)
- **Documentation:** [User Guide](https://streamlink.github.io/index.html#user-guide)
- **Downloads:** [Installation Guide](https://streamlink.github.io/install.html)
- **Source Code:** [GitHub Repository](https://github.com/streamlink/streamlink)
- **Issue Tracker:** [GitHub Issues](https://github.com/streamlink/streamlink/issues)

**Linux:**

```bash
# Arch Linux
sudo pacman -S streamlink

# Fedora
sudo dnf install streamlink

# Debian (Stable via Backports)
echo "deb http://deb.debian.org/debian trixie-backports main" | sudo tee "/etc/apt/sources.list.d/streamlink.list"
sudo apt update
sudo apt -t trixie-backports install streamlink

# NixOS
nix-env -iA nixos.streamlink

# AppImage
chmod +x streamlink-*-cp312-cp312-manylinux_2_28_x86_64.AppImage
./streamlink-*-cp312-cp312-manylinux_2_28_x86_64.AppImage --loglevel=debug
```

**macOS:**

```bash
brew install streamlink
```

**Windows:**

```bash
# Winget
winget install streamlink

# Chocolatey
choco install streamlink

# Scoop
scoop bucket add extras
scoop install streamlink

```

**PyPI:**

```bash
pip install -U streamlink
```

## Open Source Alternatives

- [**yt-dlp**](https://github.com/yt-dlp/yt-dlp)**:** While primarily a downloader, `yt-dlp` supports outputting to stdout (`-o -`), allowing it to be piped into a media player. It supports a wider range of sites than Streamlink, but its CLI for live streaming is less streamlined than Streamlink’s purpose-built approach.
- [**mpv**](https://getfoss.org/audio-video/mpv-command-line-media-player/)**:** A FOSS media player that can play some streams directly via URL. However, for sites that require complex authentication, header manipulation, or ad-segment filtering (like Twitch), mpv relies on Lua scripts that essentially wrap Streamlink or yt-dlp anyway.

## Who Should Use It?

Anyone who watches live streams and prefers a lightweight, ad-free experience. It is ideal for Linux users on older hardware who cannot afford the CPU overhead of a Chromium-based browser running Twitch’s web player. Sysadmins and developers who spend their day in the terminal appreciate the ability to pop open a stream with a single command without context-switching to a GUI browser.

## Limitations

Streamlink is fundamentally a stream extractor. It does not include a graphical user interface; you must use a separate media player.

Plugin breakage is a frequent occurrence. Streaming platforms actively fight third-party clients. If a stream suddenly stops working, it usually requires updating Streamlink to the latest release where the community has patched the plugin.

DASH streams require FFmpeg for muxing. If you do not have FFmpeg installed and configured in your PATH (or specified via `--ffmpeg-ffmpeg` in the config file), DASH streams will fail to play.

## Final Assessment

Streamlink does exactly what it claims. It extracts streams and feeds them to your player. It is an essential tool for anyone who values their system resources and wants to bypass the hostile web environments of modern streaming platforms. The active community and rapid patch cycle ensure it remains functional despite platform changes. Install it via `pipx` or AppImage, point it at VLC or mpv, and stop letting web browsers dictate your video playback experience.
