---
title: 'Tenacity: Telemetry-Free Audio Editor Fork'
description: Tenacity is a free, open-source, cross-platform audio editor forked from Audacity, designed to provide audio editing without telemetry or cloud integrations.
date: 2026-06-16T18:42:00+03:00
draft: false
image: /images/tenacity.webp
categories:
  - audio-video
tags:
  - Audio
  - DAW
  - Linux
  - Privacy
aliases: []
---

Tenacity is a free, open-source, cross-platform audio editor forked from Audacity. It exists to solve a specific problem: audio editing without telemetry, update checking, error reporting, and proprietary cloud integrations. For users who want a straightforward waveform editor that stays offline and respects user autonomy, Tenacity provides the familiar Audacity interface and toolset without the network overhead.

## Why It Matters (The FOSS Angle)

When Audacity added telemetry and proprietary service hooks, the community forked it. Tenacity proves that users can take ownership of their tools, audit the code, and strip out unwanted functionality.

The recent 1.3.4 stable release and the 1.4 Alpha 1 release demonstrate active, targeted development. 1.3.4 focuses on stability, patching macOS and Haiku support, and fixing Windows OGG import crashes. The 1.4 Alpha 1 is the architectural milestone: a successful rebase on Audacity 3.7.5. This brings modern features like realtime effects and native Opus support into Tenacity, while systematically removing every networking component.

Furthermore, the build system highlights FOSS modularity. Features like MIDI, LV2, and FFmpeg are optional compile-time targets. If a distribution lacks a specific library, the build disables the feature gracefully. You control what goes into your binary.

## Key Features

- **Realtime Effects Support:** Introduced in the 1.4 Alpha rebase, allowing non-destructive effects during playback.
- **Native Opus and WavPack Support:** Built-in import/export without relying on external libraries.
- **Beats and Bars Support:** Musical recognition and timeline snapping ported from the 3.7.5 rebase.
- **No Telemetry or Network Access:** All update checkers, error reporting, and [audio.com](http://audio.com/) integrations are removed.
- **FFmpeg 7 Support:** Backported to 1.3.4, extending format compatibility.
- **Windows on ARM Support:** 1.4 Alpha 1 introduces an installer for Windows on ARM (requires Windows 11+).
- **Modular Build System:** Optional compile-time features (MIDI, SBSMS stretching, LV2) allow minimal, dependency-light builds.

## System Requirements

**Build Dependencies:**

- **CMake:** 3.25 or later.
- **Compiler:** C++17 support required (GCC 6+ minimum, modern GCC strongly recommended; Visual Studio 2017+ on Windows).
- **wxWidgets:** 3.1.5 or later (3.3+ supported).
- **Storage:** Approximately 10 GB required for vcpkg dependency builds on Windows/macOS.

**Linux Targets:**
Debian 12, RHEL 9, or later equivalents. Older distributions lack the required dependencies.

**Platform Notes:**
Missing optional libraries (like PortSMF for MIDI or libsbsms for high-quality stretching) will automatically disable those features at build time. Windows on ARM requires Windows 11 or later.

## Real-World Usage

Consider a podcaster needing to record and export tracks on a strictly offline workstation. Tenacity handles the capture and processing without attempting to phone home.

**Building from Source:**
Sysadmins and power users can compile Tenacity directly. On Linux, install your distribution’s dependencies, then run:

```plain
git clone --recurse-submodules https://codeberg.org/tenacityteam/tenacity
cd tenacity
cmake -G Ninja -S . -B build
cmake --build build
./build/Debug/tenacity
```

To install system dependencies on Debian 12+:

```plain
sudo apt-get install build-essential libavcodec-dev libavformat-dev libavutil-dev libflac++-dev libglib2.0-dev libgtk-3-dev libid3tag0-dev libjack-jackd2-dev liblilv-dev libmpg123-dev libmp3lame-dev libogg-dev libpng-dev portaudio19-dev libportmidi-dev libportsmf-dev libserd-dev libsndfile1-dev libsord-dev libsoundtouch-dev libsoxr-dev libsuil-dev libtwolame-dev vamp-plugin-sdk libvorbis-dev lv2-dev zlib1g-dev cmake ninja-build libjpeg-dev libtiff-dev liblzma-dev libsqlite3-dev libzip-dev zipcmp zipmerge ziptool rapidjson-dev wx-common wx3.2-headers libwxgtk3.2-dev
```

**Operational Caveats:**

- **1.4 Alpha Configuration Migration:** Preference migration is incomplete. Back up your settings before testing.
- **Linux XDG Directories:** The 1.4 Alpha changes Linux configuration directories to comply with XDG standards. Check `~/.var/app/org.tenacityaudio.Tenacity/config` under Flatpak.
- **Flatpak JACK Support:** JACK only works via PipeWire in the Flatpak. If your distro uses PulseAudio, configure PipeWire. On Arch/Manjaro, install the `jack2` package to prevent `libjack.so.0` symbol lookup errors with the AppImage.
- **Plugin Lockup:** If plugins are disabled and cannot be re-enabled, delete `pluginregistry.cfg` and `pluginsettings.cfg` from your config directory.
- **GPG Verification:** The 1.3.4 release signing key changed. Import it:
`gpg --keyserver pgp.mit.edu --recv-keys 59E790FEC63109BF22BD35ABEA9D8A6F75CB28`

## Download & Installation

- **Official Website:** [Tenacity Audio](https://tenacityaudio.org/)
- **Documentation:** [Tenacity Docs](https://tenacityaudio.org/docs/index.html)
- **Downloads:** [Tenacity Releases](https://codeberg.org/tenacityteam/tenacity/releases)
- **Source Code:** [Building Tenacity](https://codeberg.org/tenacityteam/tenacity/src/main/BUILDING.md)

## Open Source Alternatives

- [**Audacity**](https://getfoss.org/audio-video/audacity-the-free-and-open-source-audio-editor/)**:** The upstream project. Offers the same base feature set plus newer additions, but includes telemetry and proprietary cloud integration.
- [**Ardour**](https://getfoss.org/audio-video/ardour-digital-audio-workstation-for-recording-editing-and-mixing/)**:** A full-fledged digital audio workstation. Far more complex, suited for multitrack mixing and mastering rather than quick waveform editing.
- [**Ocenaudio**](https://www.ocenaudio.com/)**:** A simpler audio editor with a clean interface, but it is proprietary freeware, lacking the auditability of a FOSS application.

## Who Should Use It?

Privacy-conscious users, offline workstation operators, and anyone needing a waveform editor without telemetry. Linux users who value XDG compliance and minimal dependency builds will appreciate the build system granularity.

## Limitations

The 1.4 Alpha has significant gaps. The theme system is broken pending a rewrite, defaulting the UI to standard system themes. macOS builds for 1.4 fail due to theme system issues. VST3 support is disabled because the build system cannot yet integrate the VST3 SDK under the required license. The manual is incomplete, resulting in broken help buttons. Additionally, 32-bit FFmpeg builds for Windows are being phased out; 1.3.4 is likely the last release to ship them.

## Final Assessment

Tenacity delivers Audacity without the privacy compromises. The 1.3.4 stable release offers a reliable, patched experience for day-to-day audio editing. The 1.4 Alpha 1 proves the project can track upstream changes, stripping out unwanted code while bringing critical features like realtime effects to the fork. It falls short on VST3 support and macOS stability in its alpha branch, and the incomplete manual is a friction point. For users who want an offline, telemetry-free audio editor and are willing to navigate a few rough edges in the bleeding-edge releases, Tenacity is the definitive choice.
