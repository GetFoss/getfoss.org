---
title: 'Ardour: Digital Audio Workstation for Recording, Editing, and Mixing'
description: Ardour is a Free and Open Source Software digital audio workstation (DAW) for Linux, macOS, and Windows, providing comprehensive tools for audio recording, MIDI editing, and mixing.
date: 2026-06-18T11:04:00+03:00
draft: false
image: /images/Ardour.webp
categories:
  - audio-video
tags:
  - Audio
  - Music
  - DAW
  - Linux
aliases: []
---

Ardour is a digital audio workstation (DAW) designed for recording, editing, and mixing audio and MIDI. It allows users to record multi-channel audio, edit non-destructively, automate parameters, and route signals freely. It addresses the need for a professional-grade, extensible audio production environment that runs natively on Linux, macOS, and Windows.

As a FOSS project licensed under the GPL, Ardour provides full source code access, allowing users to audit, modify, and compile the software themselves. This stands in contrast to proprietary DAWs that restrict user control and lock data into closed formats.

## Why It Matters (The FOSS Angle)

Ardour operates on a sustainable development model funded by user subscriptions and single payments, proving that professional media software can survive without proprietary licensing or vendor lock-in. The release of version 9.7 demonstrates the project’s iterative commitment to refining complex audio workflows based on real user feedback.

The addition of a vertical summary pane, HiDPI-aware automation lanes, and natural sort ordering indicates focus on usability and long-term maintainability. By allowing users to compile from source or download pre-built binaries, Ardour ensures that the software remains accessible and accountable to its community. The codebase is open, the bug tracker is public, and the development direction is transparent.

## Key Features

- **MIDI Tools Integration:** The MIDI Tools sidebar, previously limited to the pianoroll, is now accessible directly in the Editor (Shift+L). This consolidates chord editing and quantization, removing the standalone Quantize dialog.
- **Vendor-Grouped Control Surfaces:** The Control Surfaces preferences now group devices by vendor rather than mixing protocols and device names. Each active surface has a dedicated Settings button, reducing unnecessary clicks.
- **Vertical Summary Pane:** Complements the horizontal summary, using track colors to represent content. Users can drag a semi-transparent rectangle to navigate the session vertically. It is disabled by default (View > Show Vertical Summary).
- **Natural Sort Order:** The UI now sorts items logically (e.g., LSP Parametric Equalizer plugins list sequentially from 8 to 32 bands instead of 16, 32, then 8).
- **Misc-Port Connection Memory:** Ardour saves and restores connections for miscellaneous ports (auditioner, metronome, LTC generator) when switching audio/MIDI backends. This removed the need for a dedicated LTC port preference.
- **LTC Sync Resilience:** Recording continues even if LTC sync is lost or the frames-per-second (FPS) value changes during the session.
- **HiDPI Pianoroll Polish:** Automation lanes in pianoroll interfaces are now more dpi-aware, and note visibility range detection is more accurate for scaling.

## System Requirements

Ardour runs on Linux, macOS, and Windows with specific hardware and software prerequisites for each platform.

### Linux

- **Computer:** Any 64-bit x86 (Intel/AMD) or ARM compatible computer. (Other hardware can be used if you build Ardour yourself).
- **Operating System:** Any GNU/Linux release since circa 2016 (glibc ≥ 3.4.21, libstdc++ 6.0).
- **RAM:** 2GB is recommended; more is always better.
- **Disk Space:** Minimum 380MB of free space on `/opt` to install Ardour (significant additional space is recommended for recording).
- **Audio Interfaces:** Professional or semi-professional audio interfaces capable of simultaneous playback and recording are recommended. Almost all USB Audio Class 1 or 2 compliant devices will work. Using separate devices (e.g., USB microphones combined with built-in audio) is generally discouraged.

### macOS

- **Computer:** Any 64-bit Intel or ARM (Apple Silicon) system.
- **Operating System:** macOS 10.13 (High Sierra) and later. ARM versions require at least macOS 11.0 (Big Sur).
- **RAM:** 2GB is recommended; more is always better.
- **Disk Space:** Minimum 600MB of free space to install Ardour (significant additional space is recommended for recording).
- **Audio Interfaces:** Any device supported by CoreAudio can be used. If only using built-in audio, you must create an aggregate device.

### Windows

- **Computer:** Any 64-bit x86 (Intel/AMD) system.
- **Operating System:** Windows 7 or later.
- **RAM:** 2GB is recommended; more is always better.
- **Disk Space:** Minimum 600MB of free space to install Ardour (significant additional space is recommended for recording).
- **Audio Interfaces:** Any device supported by ASIO, ASIO4All, or MME.

### Other Systems

Ardour can be built and run on any operating system that supports audio devices, can run JACK, and provides most of the POSIX specification (e.g., FreeBSD, Solaris), but this requires building the software yourself.

## Real-World Usage

When setting up a hardware control surface, previous versions required users to know the underlying protocol to enable specific device controls. In 9.7, users simply select their device from the vendor-grouped list in Preferences. Each device gets its own Settings button.

There is a limitation: Ardour supports only one Generic MIDI device map at a time. Selecting another MIDI keyboard removes the first. For unmapped devices, Ardour provides an empty binding map for generic MIDI controllers and a generic device file for Mackie protocol.

A significant regression fix restores “hot-plugging” for supported surfaces; they automatically activate when attached. The vertical summary pane is useful for large sessions where navigating dozens of tracks vertically is cumbersome. Enable it via View > Show Vertical Summary.

Users compiling from source should note that 32-bit MSVC build projects have been removed. Linux startup scripts now test for older PipeWire versions.

## Download & Installation

- **Official Website:** [Ardour.org](https://ardour.org/)
- **Documentation:** [Ardour FAQ](https://ardour.org/faq.html)
- **Downloads:** [Ardour Downloads](https://community.ardour.org/download)
- **Source Code / Issues:** [Ardout Tracker](http://tracker.ardour.org/) | [GitHub Repository](https://github.com/Ardour/ardour) | [Development Page](https://ardour.org/development.html)

> Ardour offers three payment tiers: subscription ($1 to $50/month), single payment (less than $45 gets current version updates; $45+ gets current and next major version), and a Free/Demo version. The Free/Demo version periodically goes silent after 10 minutes and lacks access to nightly builds. Source code is available for compilation, but the project explicitly states they do not provide build support for end users.

## Open Source Alternatives

- [**LMMS**](https://getfoss.org/audio-video/lmms-cross-platform-digital-audio-workstation-for-music-production/)**:** A free, open-source DAW focused heavily on beat synthesis and sequencing. It lacks the robust audio recording and routing capabilities of Ardour.
- [**Qtractor**](https://getfoss.org/audio-video/qtractor-audio-midi-sequencer-and-digital-audio-workstation/)**:** An Audio/MIDI multi-track sequencer for Linux. It is lighter and uses the Qt framework but has a smaller feature set compared to Ardour.
- [**Audacity**](https://getfoss.org/audio-video/audacity-the-free-and-open-source-audio-editor/)**:** A free, open-source audio editor. It is designed for stereo editing and basic multitrack work, not for complex MIDI production or mixing sessions.

## Who Should Use It?

Ardour is for audio engineers, musicians, and producers who need a full-fledged DAW with advanced routing, automation, and MIDI capabilities. It is particularly suited for Linux users who require native, professional audio tools, or anyone who values software freedom and transparent development over proprietary ecosystems.

## Limitations

Building from source is documented as “challenging and complex,” especially on Windows and macOS, with no official support provided. The generic MIDI device limitation (one map at a time) restricts users with multiple disparate MIDI controllers. MIDI chase behavior outside of regions is still an acknowledged unresolved issue. The demo version’s 10-minute silence interruption makes it unsuitable for evaluating long recording sessions.

## Final Assessment

Ardour 9.7 represents steady, pragmatic development. The UI improvements—natural sorting, vertical summary, and consolidated control surface settings—reduce friction in daily workflows. The MIDI and LTC fixes show attention to technical detail. It is a powerful DAW that competes functionally with proprietary alternatives while maintaining its FOSS principles. The payment model is straightforward and funds continued development without restricting fundamental usage rights.
