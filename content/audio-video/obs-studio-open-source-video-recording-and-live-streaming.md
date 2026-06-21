---
title: 'OBS Studio: Open Source Video Recording and Live Streaming'
description: OBS Studio is free and open source software for video recording and live streaming, supporting real-time source and audio mixing with hardware-accelerated encoding.
date: 2026-06-21T09:09:00+03:00
draft: false
image: /images/obs-studio.webp
categories:
  - audio-video
tags:
  - Audio
  - Video
  - Streaming
  - Linux
aliases: []
---

OBS Studio is a free, open source application for real-time video recording and live streaming. It composites audio and video sources into scenes, encodes them, and outputs to files or streaming services. It handles game streaming, software demos, conferences, and any workflow requiring desktop capture, mixing, and broadcast.

The project is GPL-2.0 licensed. Source code is available on GitHub, with official binaries for Windows, macOS, and Linux.

OBS combines multiple inputs (windows, screens, capture cards, microphones, applications) into a single stream or recording, offering control over layout, audio routing, and encoding. Unlike vendor-locked streaming apps, OBS is cross-platform, scriptable, and plugin-driven. Its behavior and data formats are fully auditable.

## Why It Matters (The FOSS Angle)

The 32.1.x releases demonstrate how a FOSS project iterates on UX and infrastructure. The audio mixer overhaul directly affects daily live production workflows. WebRTC simulcast and expanded obs-websocket canvas support provide infrastructure for remote production and automation.

**The FOSS relevance extends beyond features:**

- The mixer redesign was driven by community feedback, implemented by a core contributor (Warchamp7), and immediately hotfixed when regressions appeared in 32.1.0/32.1.1. Rapid release, transparent issues, visible fixes.
- WebRTC simulcast and obs-websocket changes lower barriers for custom streaming setups and remote control, benefiting community-run events and non-commercial platforms independent of proprietary SDKs.
- Security changes for browser sources using local files, and better handling of missing plugins in the plugin manager, are unglamorous but essential work. Here it is visible in the changelog and code.
- Linux virtual camera support via v4l2loopback depends on a kernel module and polkit, but the integration is open and documented.

## Key Features

- **Scene-based compositing** - Multiple sources (windows, screens, captures, images, text, browser sources) are arranged in scenes. Scenes can be switched live for picture-in-picture, side-by-side, or branded overlays.
- **Flexible source system** - Sources support capture of X11/Wayland windows, displays, PipeWire devices, browser sources, media files, and more. Sources can be global or per-scene with independent audio monitoring.
- **Audio mixer overhaul (32.1)** - The mixer supports vertical and horizontal layouts, per-source monitoring toggles, source pinning, and displaying hidden sources. Live audio management is more predictable with many sources.
- **Hardware-accelerated encoding** - OBS includes x264 software encoding and supports NVENC, AMF, QSV, and VideoToolbox where available, moving encoding workload to the GPU.
- **WebRTC simulcast support (32.1)** - Simulcast sends multiple quality layers to a WebRTC endpoint, allowing viewers with different bandwidth to adapt without server-side transcoding.
- **obs-websocket remote control** - An official WebSocket interface allows external tools and scripts to control OBS. 32.1 adds partial support for Canvases, aiding automation and remote production.
- **Virtual camera via v4l2loopback** - On Linux, OBS outputs to a V4L2 device using the v4l2loopback kernel module, making the OBS scene appear as a webcam in other applications.

## System Requirements

The project does not publish a single consolidated hardware requirements table. Requirements listed below are derived from official build prerequisites and runtime documentation.

### Windows

- Windows 10 1909+ or Windows 11
- Visual Studio 2026 (18.7.0+) with:
    - Windows 11 SDK (minimum 10.0.26100.0)
    - C++ ATL for latest build tools (x86 & x64)
    - MSVC v145 - VS 2026 C++ x64/x86 build tools (Latest)
- Git for Windows
- CMake 4.2 or newer

### macOS

- macOS 14.1 (minimum: macOS 13.5)
- Xcode 15.4
- CMake 3.28 (minimum), 3.30 recommended
- CCache 4.8 or newer (Optional)

### FreeBSD

- FreeBSD 13 or newer
- CMake 3.28 or newer
- Git
- Ninja
- Optional: CCache to improve compilation speeds

### Linux

- xserver-xorg version 1.18.4 or newer recommended to avoid performance issues with fullscreen projector.
- OpenGL 3.3 or later required. Verify via `glxinfo | grep "OpenGL"`.
- Virtual camera requires the `v4l2loopback` kernel module and a working polkit setup.

> No official CPU/RAM/disk requirements are published. **_Estimated:_** _Modern multi-core CPU and sufficient RAM for scene complexity; GPU encoding recommended for high-resolution, high-framerate work._

## Real-World Usage

### Typical Streaming Setup

1. Install OBS via Flatpak (non-Ubuntu) or Ubuntu PPA.
2. Configure scenes:

    - “Game” with game capture, microphone, and desktop audio.
    - “Camera” with webcam and microphone.

3. Set streaming service in **Settings** -> **Stream**. Use x264 for simplicity, or NVENC/AMF/QSV for lower CPU load.
4. Use the audio mixer to balance sources; pin critical sources so they stay visible.
5. Enable virtual camera to use OBS output in video conferencing apps.

### Remote Production with obs-websocket

1. Install and configure the obs-websocket plugin.
2. On a separate machine, run a custom script or UI connecting to the OBS WebSocket.
3. Trigger scene changes and transitions via scripts, integrating with schedules or chat bots.

### Migration Notes from 32.1.x Releases

- The audio mixer UI and behavior changed in 32.1.x. Verify mixer layout, pinned sources, and monitoring settings after upgrading.
- Sources previously hidden or disabled may appear in the mixer if “display sources set to hide” or “display sources not in current scene” is enabled.
- The “Source” label was removed from source names in 32.1. Adjust scripts or external tools relying on that naming convention.
- The 32.1.2 hotfix addresses scene list selection lag and nested menu appearance introduced in 32.1.0/32.1.1. Upgrade to >= 32.1.2 if experiencing UI lag.
- Browser sources using local files received stricter security handling in 32.1. Review local file browser source configurations.
- On Linux, PipeWire camera and capture fixes in 32.1 may affect device enumeration or framerate options. Re-check camera selections.

## Download & Installation

- **Official Website:** [OBS Project](https://obsproject.com/)
- **Documentation:** [OBS Knowledge Base](https://obsproject.com/kb)
- **Downloads:** [OBS Studio Download](https://obsproject.com/download)
- **Source Code:** [obs-studio on GitHub](https://github.com/obsproject/obs-studio)
- **Issue Tracker:** [OBS Studio Issues](https://github.com/obsproject/obs-studio/issues)

### Quick Start: Linux

#### Flatpak (Recommended for non-Ubuntu distributions)

1. Ensure Flatpak and Flathub are enabled.
2. Install OBS Studio from Flathub: `flatpak install flathub com.obsproject.Studio`
3. Run OBS: `flatpak run com.obsproject.Studio`

#### Ubuntu PPA (Ubuntu 18.04+)

1. Add the OBS PPA and install:sudo add-apt-repository ppa: `obsproject/obs-studio`
`sudo apt update`
`sudo apt install obs-studio`
2. Launch OBS from the desktop menu or terminal.

#### Virtual Camera Setup (Linux)

1. Install `v4l2loopback-dkms`:

    - Debian/Ubuntu: `sudo apt install v4l2loopback-dkms`
    - Fedora (requires RPM Fusion): `sudo dnf install kmod-v4l2loopback`
    - Fedora Silverblue/Kinoite: `rpm-ostree install kmod-v4l2loopback`
    - Arch Linux/Manjaro (requires kernel headers): `sudo pacman -S v4l2loopback-dkms`

2. Ensure polkit is configured so OBS can load the module.
3. Optionally load the module manually: `modprobe v4l2loopback exclusive_caps=1 card_label='OBS Virtual Camera'`
4. In OBS, click “\*\*_Start Virtual Camera_\*\*”.

## Open Source Alternatives

- [**SimpleScreenRecorder**](https://getfoss.org/audio-video/simplescreenrecorder-linux-screen-recording-and-opengl-capture/) - Linux-only screen recorder with a simpler UI. Less oriented toward live production, multi-source compositing, and streaming.
- [**Streamlink**](https://getfoss.org/audio-video/streamlink-command-line-streaming-for-twitch-youtube-and-more/) **+** [**FFmpeg**](https://getfoss.org/audio-video/ffmpeg-cross-platform-audio-and-video-processing-framework/) - CLI pipeline extracting streams (Streamlink) and handling encoding/muxing (FFmpeg). Offers control but requires manual scripting.
- [**vMix**](https://www.vmix.com/) - Proprietary, Windows-only live production software. More polished for large events but closed source and vendor-locked.

## Who Should Use It?

- **Live streamers** (gaming, talks, demos) requiring multi-source layouts and real-time encoding.
- **Conference and meetup** organizers running hybrid or online events, especially on Linux or self-hosted infrastructure.
- **Educators and trainers** recording lectures or software tutorials with mixed video and screen content.
- **Users needing** a virtual webcam with custom scene composition for video calls.

## Limitations

- **Resource usage** - Real-time compositing and encoding are CPU- and GPU-intensive. High-resolution streams require capable hardware without GPU encoding.
- **Plugin ecosystem fragmentation** - Plugins extend OBS, but some are poorly maintained or incompatible with certain builds (notably Flatpak). Complicates deployment and upgrades.
- **Linux virtual camera complexity** - Depends on `v4l2loopback` and polkit, which are distro-specific. Issues often stem from the kernel module or permissions, not OBS.
- **Feature gaps vs high-end proprietary tools** - Lacks integrated advanced routing, built-in replay, and playout servers found in specialized broadcast systems. Requires external tools or plugins.
- **Configuration complexity** - The sheer number of options (encoders, mixers, sources, sync offsets) can overwhelm users. Misconfiguration is common.

## Final Assessment

OBS Studio is the standard for free, open source live streaming and recording. It handles multi-source composition, hardware encoding, and remote control well. The 32.1.x releases show active development on both UX and infrastructure.

It excels for streamers, educators, and event organizers needing a flexible, scriptable, cross-platform tool who are comfortable tuning settings and managing Linux kernel modules for virtual camera support. It is less ideal for users needing a minimalist recorder or a turnkey, vendor-supported broadcast system with proprietary I/O integration.

As FOSS, its main advantage is control: inspect how scenes are encoded, how audio is routed, and how data is handled. Modify or extend it to fit workflows that proprietary tools do not support.
