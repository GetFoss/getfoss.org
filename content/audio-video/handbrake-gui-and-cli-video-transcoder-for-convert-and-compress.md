---
title: 'HandBrake: GUI and CLI Video Transcoder for Convert and Compress'
description: HandBrake is a free and open-source cross-platform tool for converting video files between formats using hardware-accelerated and software encoders.
date: 2026-06-21T11:17:00+03:00
draft: false
image: /images/HandBrake.webp
categories:
  - audio-video
tags:
  - Audio
  - Video
  - Media
aliases: []
---

HandBrake is a cross-platform tool for converting video files from one format to another. It provides a graphical interface on macOS, Windows, and Linux (Flatpak), alongside a separate command-line variant. HandBrake wraps libraries like FFmpeg, x265, SVT-AV1, libdav1d, and Intel oneVPL into a focused transcoding workflow with preset-based configuration. It is GPL-licensed.

## Why It Matters (The FOSS Angle)

HandBrake’s 1.11.x release cycle demonstrates how a FOSS project can make professional-grade production tools accessible without license fees. The addition of native ProRes and DNxHR encoders in 1.11.0, with curated Production and Proxy presets in MOV containers and 24-bit PCM audio pass-through, targets workflows that previously required commercial NLE software or proprietary encoding tools.

The Preservation FFV1 presets — now supporting FLAC and PCM audio pass-through alongside Apple Lossless and Vorbis — provide a GUI-accessible path to lossless, standardized archival without manually constructing FFmpeg command lines.

On the hardware side, the addition of AMD VCN AV1 10-bit encoding and the ongoing updates to SVT-AV1 (now at 4.1.0) and FFmpeg (8.0.2) keep HandBrake current with the GPU encoding landscape. Intel HyperEncode was removed in 1.11.0 because it was deprecated upstream — a decision that respects the maintenance burden over feature bloat.

## Key Features

- **Native ProRes and DNxHR encoding.** Production-quality presets for standard, HQ, LT, HQX, and SQ variants in MOV containers with 24-bit PCM multi-channel audio. Cross-platform access to these encoder formats without vendor lock-in.
- **Preservation FFV1 presets.** Lossless archival encoding with broad audio pass-through (FLAC, PCM, Apple Lossless, Vorbis). Separate presets for FFV1+FLAC and FFV1+PCM combinations.
- **AMD VCN AV1 10-bit encoding.** Hardware-accelerated AV1 encoding with a dedicated 2160p 4K preset for AMD 9000 series GPUs and newer.
- **MOV container output.** New container support enables the ProRes and DNxHR workflows, plus FFV1 can now be muxed in MP4.
- **Hardware acceleration stack.** Intel QSV via oneVPL, AMD VCN via AMF, and software encoders (x265, SVT-AV1, libvpx, MPEG-2) all accessible through a unified preset system.
- **Dolby Vision support.** Profile 5 video is now tagged with a proper color tag, improving compatibility with playback devices.
- **Verified releases.** Every release asset has a SHA-256 hash and a detached signature file published alongside it.

## System Requirements

- **macOS:** 10.13 (High Sierra) and later. Universal binary (Apple Silicon and Intel).
- **Windows:** 10 and later. x64 (Intel/AMD) or ARM64 (Qualcomm/other). Requires Microsoft .NET Desktop Runtime 10.0.x (downloaded separately from Microsoft).
- **Linux:** Flatpak via Flathub. No minimum distribution version specified.

The project does not publish CPU, RAM, or GPU requirements. Hardware-accelerated encoding features require the corresponding vendor GPU and drivers.

## Real-World Usage

A video editor needs to deliver proxy files in ProRes LT at 540p for offline editing, then generate final output in H.265. HandBrake 1.11.0’s Production ProRes proxy presets handle the first step. Custom presets can be backed up for consistency across workstations.

An archivist digitizing a collection of DVDs can use the Preservation FFV1 FLAC preset for lossless storage, then batch-convert a subset to H.265 MP4 for access copies — all from the queue system without writing scripts.

**Operational caveats from the release notes:**

- The AV1 VCN 2160p 4K preset is designed for AMD 9000 series GPUs. On AMD 7000 series hardware, output dimensions may be incorrect. The project states this is unfixable in software.
- Before updating, clear the encode queue. Pending jobs may fail or produce corrupt output under a new version.
- Custom presets and app preferences may not be compatible across versions. Back them up before upgrading.
- Windows users must install .NET Desktop Runtime 10.0 separately — it is not bundled. Version 1.11.1 dropped the .NET 8 dependency entirely.

## Download & Installation

- **Official Website:** [handbrake.fr](https://handbrake.fr/)
- **Documentation:** [HandBrake Docs](https://handbrake.fr/docs/)
- **Downloads:** [Release Downloads](https://handbrake.fr/downloads.php)
- **Command Line Version:** [CLI Downloads](https://handbrake.fr/downloads2.php)
- **Source Code:** [GitHub Repository](https://github.com/HandBrake/HandBrake)
- **Development Builds:** [Nightly Snapshots](https://github.com/HandBrake/HandBrake-snapshots)
- **Bug Reports:** [GitHub Issues](https://github.com/HandBrake/HandBrake/issues)
- **Linux (Flatpak):** [Flathub](https://flathub.org/apps/details/fr.handbrake.ghb)

### Quick Start (Linux Flatpak)

```bash
flatpak install flathub fr.handbrake.ghb
flatpak run fr.handbrake.ghb

```

### Verify a release (example for Windows x64)

The release page publishes SHA-256 hashes and `.sig` signature files for every asset. Download both the installer and its signature, then verify:

```bash
sha256sum HandBrake-1.11.2-x86_64-Win_GUI.exe
# Compare against published hash: 6becb8e5a041941d5f2fd6fe83c66fc15b2b16190e2e79f5ad47eb4cd18df8cd

```

### Quick Start (Windows)

1. Install [Microsoft .NET Desktop Runtime 10.0](https://dotnet.microsoft.com/en-us/download/dotnet/10.0) (x64 or Arm64 as appropriate) — available from Microsoft’s website.
2. Download and run the `.exe` installer from the [downloads page](https://handbrake.fr/downloads.php).
3. Launch HandBrake, drag in a source file, select a preset, and start the encode.

### Quick Start (macOS)

1. Download the Universal `.dmg` from the [downloads page](https://handbrake.fr/downloads.php).
2. Open the DMG and drag HandBrake to Applications.
3. On first launch, right-click and select Open to bypass Gatekeeper (if prompted).

## Open Source Alternatives

- **FFmpeg:** The underlying engine HandBrake builds on. Infinitely more flexible — supports any format, filter, or pipeline configuration. No GUI. Steeper learning curve but essential for anything beyond straightforward transcodes.
- **VLC:** Primarily a media player, but includes basic conversion capability via its Stream/Save dialog. Adequate for quick one-off conversions but lacks preset management, queue systems, and fine-grained encoder control.
- **Avidemux:** A lightweight GUI editor and transcoder. Supports cutting and filtering without re-encoding, which HandBrake cannot do. Encoder options and format support are far narrower, and hardware acceleration is minimal.

## Who Should Use It

People who need to convert video files regularly and want a graphical interface with sensible presets. Video editors generating proxies or delivery renders. Archivists who want accessible FFV1 or ProRes output without memorizing FFmpeg syntax. Anyone who finds the FFmpeg command line opaque but needs reliable H.264, H.265, AV1, ProRes, or DNxHR output.

## Limitations

HandBrake is a transcoder, not a general multimedia framework. It cannot stream, play media, or act as a library for other applications. Output is restricted to MP4, MKV, and (as of 1.11.0) MOV.

The GUI hides complexity, which is the point, but it also hides control. Custom channel ordering was only added in 1.11.0. Advanced filter chains, metadata manipulation, and multi-pass bitrate curves require the CLI variant or switching to FFmpeg directly.

Preset compatibility across versions is not guaranteed. The project explicitly warns that custom presets may break on update — a problem for teams standardizing workflows. The Windows dependency on a specific .NET runtime version that must be installed separately is an additional deployment friction point.

The AMD 7000 series AV1 VCN dimension bug being “unfixable in software” means users on that hardware generation must avoid the 2160p AV1 preset or accept incorrect output.

## Final Assessment

HandBrake occupies a specific niche: GUI-driven transcoding with curated presets. It executes that niche well. The 1.11.x additions of ProRes, DNxHR, FFV1 preservation presets, and AMD AV1 encoding broaden its appeal beyond the H.264/H.265 crowd into production and archival territory. It will never replace FFmpeg for pipeline work or complex transformations, and it does not try to. For individual users and small teams who want a visual interface over a reliable encoding engine, HandBrake is the FOSS option.
