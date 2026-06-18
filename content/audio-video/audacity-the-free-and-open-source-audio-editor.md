---
title: 'Audacity: The Free and Open Source Audio Editor (With Telemetry)'
description: An expert look at Audacity, the free and open-source cross-platform audio editor. Covering system requirements, real-world usage, and FOSS benefits.
date: 2026-06-15T20:59:00+03:00
draft: false
image: /images/Audacity.webp
categories:
  - audio-video
tags:
  - Audio
  - DAW
  - Linux
  - Beginner
aliases: []
---

Audacity is a free, open-source, cross-platform audio editor. It handles recording, editing, and mixing digital audio tracks. For anyone needing to manipulate sound files—whether cutting up podcasts, recording voiceovers, or editing samples—Audacity provides the standard toolset without licensing fees or vendor lock-in.

However, it is critical to note that under its current stewardship, Audacity introduced telemetry and proprietary cloud service integrations, sparking significant privacy and governance concerns within the community (which directly led to privacy-focused forks like [Tenacity](https://getfoss.org/audio-video/tenacity-telemetry-free-audio-editor-fork/)). While the code remains auditable as FOSS, users must weigh these additions against the software's utility. The official website is [https://www.audacityteam.org/](https://www.audacityteam.org/).

## Why It Matters (The FOSS Angle)

Audacity operates as a staple in the audio editing space because it guarantees access. Users are not tied to subscription models or proprietary formats. The project’s presence on GitHub allows public audit of the codebase, community contributions, and issue tracking. This transparency ensures the tool serves the user’s interests, particularly regarding data handling and feature direction. Cross-platform support across Windows, macOS, and Linux prevents OS-level vendor lock-in, letting users standardize their audio workflow regardless of the operating system.

## Key Features

- **Cross-Platform Support:** Runs on Windows, macOS, and Linux, ensuring a consistent interface across operating systems.
- **Multi-Architecture Binaries:** Provides specific builds for x86_64, ARM64 (including Apple Silicon and Windows on ARM), and universal binaries for macOS.
- **Plugin Architecture:** Supports VST and OpenVINO plugins, extending its base functionality.
- **FFmpeg Integration:** Integrates with FFmpeg, enabling import and export of a wider range of audio formats beyond the defaults.
- **Legacy Hardware Compatibility:** Maintains compatibility with older hardware and operating systems, including 32-bit Windows and older Intel-based macOS releases.
- **Community Packaging:** Linux distributions can package Audacity (as debs, rpms, or flatpaks) with distribution-specific patches, leveraging the open-source license.

## System Requirements

Official system requirements are strict regarding storage and vary by operating system.

**Windows:**

- Tested on Windows 10 & Windows 11. May work on Windows 8.1, 7, and Vista.
- Both 64-bit and 32-bit versions available.
- No specific CPU or GPU requirements.
- **Storage:** Requires fast, uninterrupted access to a hard drive or SSD. Network storage, consumer-grade external hard drives, or USB thumbdrives may be unreliable.
- **Windows on ARM (Beta):** Requires Windows 11 or later. Windows RT is not supported. Plugins (VST, OpenVINO, etc.) are not supported. Requires a WoA-version of FFmpeg. Thoroughly tested with limited devices.

**macOS:**

- Tested on macOS 14 & 15. May work on macOS 10.11 El Capitan onward.
- Intel (x86_64), Apple Silicon (ARM 64), and universal binary versions available.
- macOS 10.14 and earlier do not support the universal dmg; use the Intel-only dmg.
- No specific CPU or GPU requirements.
- **Storage:** Requires fast, uninterrupted access to a hard drive or SSD. Network storage, consumer-grade external hard drives, or USB thumbdrives may be unreliable.

**Linux:**

- Tested on Ubuntu 22.04. Should work on most major distributions.
- No specific CPU or GPU requirements. Runs on amd64-based computers.
- The AppImage requires FUSE 2 to run.
- **Storage:** Requires fast, uninterrupted access to a hard drive or SSD. Network storage, consumer-grade external hard drives, or USB thumbdrives may be unreliable.

## Real-World Usage

A common deployment scenario is podcast post-production. A user records audio locally, then uses Audacity to remove background noise, normalize volume levels, and trim dead air.

**Configuration Notes:**

- Do not attempt to read or write project files directly to network attached storage (NAS) or USB drives. The application requires fast, uninterrupted storage access; network drops or slow I/O will cause errors.
- On Linux, the AppImage will fail to execute if FUSE 2 is not installed on the host system. Alternatively, users can opt for community-maintained Flatpaks or native distribution packages, though these may carry distribution-specific patches.
- Windows on ARM users must source a WoA-specific version of FFmpeg; the standard Windows FFmpeg installer will not work.

## Download & Installation

- **Official Website:** [Audacity Team](https://www.audacityteam.org/)
- **Documentation:** [Audacity Support](https://support.audacityteam.org/)
- **Downloads:** [Audacity Download](https://www.audacityteam.org/download/)
- **Source Code / Issues:** [Audacity GitHub](https://github.com/audacity)

## Open Source Alternatives

- [**Ardour**](https://getfoss.org/audio-video/ardour-digital-audio-workstation-for-recording-editing-and-mixing/)**:** A full-fledged digital audio workstation. More suited for multi-track recording, mixing, and mastering than Audacity, but carries a steeper learning curve.
- **LMMS:** Designed for music production rather than general audio editing. Includes built-in instruments and sequencers.
- [**Tenacity**](https://getfoss.org/audio-video/tenacity-telemetry-free-audio-editor-fork/)**:** A fork of Audacity. Created in response to governance and telemetry concerns, focusing strictly on privacy and community-driven development.

## Who Should Use It?

Podcasters, journalists, voice-over artists, and casual users needing direct waveform manipulation. Users who require quick, single-track edits or straightforward multi-track mixing without the overhead of a professional DAW. Linux users who need guaranteed audio editing capability without proprietary software.

## Limitations

The strict requirement for fast, local storage prevents working directly from network drives or thumbdrives. The Windows on ARM build is in beta, lacking plugin support and requiring a specific FFmpeg build. Users on older operating systems are locked out of current versions and must rely on legacy builds (e.g., Windows 7 tops out at Audacity 2.3.3). Prior to version 3.0.3, the Audacity team did not release Linux binaries directly, relying entirely on community packaging.

## Final Assessment

Audacity provides reliable, standard audio editing with zero licensing cost and full code transparency. It excels at straightforward recording, cutting, and basic processing tasks across all major operating systems. It falls short for users needing advanced DAW features like MIDI sequencing or complex routing, and its strict local storage requirement limits specific workflow setups. For standard audio manipulation, it remains a fundamental tool.
