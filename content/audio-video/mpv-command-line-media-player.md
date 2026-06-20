---
title: 'mpv: Command Line Media Player'
description: mpv is a free and open-source command-line media player that supports a wide variety of formats, codecs, and high-quality video rendering via GPU shaders.
date: 2026-06-20T09:10:00+03:00
draft: false
image: /images/mpv.webp
categories:
  - audio-video
tags:
  - Video
  - Audio
  - Media
  - Advanced
aliases: []
---

mpv is a free (as in freedom) media player for the command line. Descended from the MPlayer and mplayer2 projects, it focuses on clean, minimal playback of a wide variety of media file formats, audio and video codecs, and subtitle types. Instead of relying on heavy GUI frameworks, mpv uses a configurable on-screen controller (OSC) and scriptable Lua extensions, making it a favorite for users who want direct control over their media pipeline.

## Why It Matters (The FOSS Angle)

The v0.40.0 and v0.41.0 releases represent a major architectural shift in how mpv handles video output. The libplacebo-based `vo_gpu_next` has officially replaced `vo_gpu` as the default renderer. This matters because `vo_gpu_next` leverages modern Vulkan capabilities for superior color management, HDR tone mapping, and shader processing. Furthermore, Vulkan hardware decoding is now preferred over older APIs when available.

This transition demonstrates the FOSS development model at work: a gradual, community-tested replacement of legacy subsystems rather than a forced, hard-break migration. The addition of native Wayland color representation protocols and clipboard support also shows mpv actively adapting to the evolving Linux desktop without compromising its core design.

## Key Features

- **GPU Shader Video Output:** Defaults to `vo_gpu_next` using libplacebo for high-quality scaling, color management, and HDR to SDR tone mapping.
- **Command Line Driven:** Controlled entirely via CLI flags, configuration files (`mpv.conf`), and input bindings (`input.conf`), making it highly scriptable and automatable.
- **Lua Scripting:** Supports Lua scripts for extending functionality, such as the new `context_menu.lua` for right-click menus and `positioning.lua` for cursor-centric zooming.
- **Explicit Hardware Decoding:** Hardware decoding is disabled by default to ensure maximum compatibility and frame-accurate seeking; it must be explicitly enabled via `--hwdec`.
- **Broad Format Support:** Leverages FFmpeg to decode almost any audio or video format, including native DASH playback and Blu-ray/DVD stream handling.
- **Wayland and DRM Support:** First-class support for Linux Wayland protocols (color management, tablet input, clipboard) and native HDR metadata passing under DRM.

## System Requirements

- **OS:** Recent Linux distributions, Windows 10 1607 or later, macOS 10.15 or later.
- **Dependencies:** FFmpeg 6.1 or newer, libplacebo 6.338.2 or newer.
- **GPU:** Capable GPU required. mpv uses GPU shaders for video rendering and scaling rather than fixed-function hardware. Low-power integrated GPUs may experience tearing or stuttering; `--profile=fast` is recommended for such hardware.

**Estimated Hardware Requirements:**

- **Minimum (SD/720p Software Decode):** 1 GHz CPU, GPU with OpenGL/Vulkan support, 512MB RAM.
- **Recommended (1080p/4K Hardware Decode & HDR):** Modern multi-core CPU, GPU with Vulkan 1.2+ support (for `vo_gpu_next`), 2GB+ RAM.

## Real-World Usage

Configuring mpv means editing text files, not clicking through GUI dialogs. User settings belong in `~/.config/mpv/mpv.conf`.

**Hardware Decoding:** If you want your GPU to handle decoding (saving CPU power), you must ask for it. Add `hwdec=auto-safe` to your config. Do not use `hwdec=auto` unless you know exactly how your FFmpeg is compiled, as incorrect drivers will crash playback.

**Weak Hardware:** Trying to run default mpv on a low-end laptop or SBC will result in dropped frames. The default high-quality scaling shaders are computationally expensive. Switch to `profile=fast` or `profile=sw-fast` to offload work and reduce GPU overhead.

**Compilation:** Building mpv requires meson and a substantial list of development headers (X11, ALSA/PulseAudio, FFmpeg, libass, libplacebo). Because tracking dependency versions for FFmpeg and libplacebo is tedious, use the [mpv-build](https://github.com/mpv-player/mpv-build) wrapper. It statically compiles FFmpeg, libass, and libplacebo before building mpv, bypassing distro package version mismatches.

**Release Cycle:** mpv cuts releases once or twice a year. Old releases are unmaintained. If you hit a bug, you are expected to update to the latest release or build from master. Distro maintainers are expected to backport their own patches.

## Download & Installation

- **Official Website:** [mpv.io](https://mpv.io/)
- **Documentation:** [mpv Manual](https://mpv.io/manual/)
- **Downloads:** [mpv Installation](https://mpv.io/installation/)
- **Source Code:** [mpv-player/mpv](https://github.com/mpv-player/mpv)
- **Bug Reports:** [mpv Bug Reports](https://mpv.io/bug-reports/)
- **Community:** [mpv Community](https://mpv.io/community/)

### Quick Start (Building from Source)

Building from source requires `meson`, `gcc/clang`, and development headers for FFmpeg, libplacebo, and audio/video outputs.

1. Clone the repository:git clone https://github.com/mpv-player/mpv.git
cd mpv
2. Setup and compile using meson:meson setup build
meson compile -C build
3. Install the binary:meson install -C build
4. Play a file:mpv --hwdec=auto-safe --profile=fast video.mkv

## Open Source Alternatives

- **VLC:** The ubiquitous media player. Offers a full GUI, cross-platform consistency, and network streaming capabilities. Lacks mpv’s focus on high-end shader rendering and CLI configurability.
- [**MPlayer**](https://getfoss.org/independent-heritage/mplayer-legacy-command-line-media-player/)**:** The direct predecessor to mpv. Still works, but suffers from legacy code bloat and lacks modern Wayland/HDR support.
- **Celluloid (formerly GNOME MPV):** A GTK frontend for mpv. Provides a traditional GUI wrapper around libmpv for users who want mpv’s backend without the CLI lifestyle.

## Who Should Use It?

Sysadmins, HTPC enthusiasts, and advanced users who configure their media players via dotfiles. It is built for people who want pixel-perfect, color-managed playback and are willing to read the manual to get it.

## Limitations

There is no standard GUI. The OSC is minimal by design, which is a barrier for users expecting traditional menus. The “release and forget” maintenance policy means no backports; if you need a fix, you must upgrade to the latest version. Building from source can be a dependency nightmare on older distributions due to strict FFmpeg and libplacebo version requirements.

## Final Assessment

mpv is the definitive media player for the command line, offering unmatched video rendering quality through its libplacebo/Vulkan pipeline. The recent shift to `vo_gpu_next` as the default solidifies its position at the bleeding edge of video playback. It assumes you know what you are doing and stay out of the way once configured. It excels in quality and control, but falls short for anyone requiring a point-and-click interface or stable, backported release maintenance.
