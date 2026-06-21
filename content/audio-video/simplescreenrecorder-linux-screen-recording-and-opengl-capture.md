---
title: 'SimpleScreenRecorder: Linux Screen Recording and OpenGL Capture'
description: SimpleScreenRecorder is a free and open source screen recorder for Linux with support for recording OpenGL applications, including 32-bit OpenGL programs on 64-bit systems.
date: 2026-06-21T21:07:00+03:00
draft: false
image: /images/simplescreenrecorder.webp
categories:
  - audio-video
tags:
  - Video
  - Linux
  - Tools
aliases: []
---

SimpleScreenRecorder (SSR) is a free and open source screen recording application for Linux. The available project documentation primarily focuses on packaging and distribution support rather than application internals, but it explicitly documents support for recording OpenGL applications, including 32-bit OpenGL programs on 64-bit systems.

The project is widely packaged across the Linux ecosystem, making it easy to install through native package managers instead of relying on third-party installers or proprietary distribution channels.

## Why It Matters (The FOSS Angle)

SimpleScreenRecorder is distributed as free and open source software and its source code is publicly available. Users can inspect the code, build the application themselves, and package it for distributions that do not provide official packages.

The project’s broad distribution support is also notable. Packages are available for Debian, Ubuntu, Fedora, Gentoo, NixOS, OpenSUSE, Slackware, and several other distributions. Even when a package is unavailable, the project can be compiled from source.

This approach keeps the application portable and avoids dependence on a single vendor or distribution.

## Key Features

The project documentation does not publish a complete feature list. The following capabilities are explicitly documented:

- **OpenGL Application Recording**: The project supports recording OpenGL applications.
- **32-bit OpenGL Recording on 64-bit Systems**: Additional compatibility packages allow recording 32-bit OpenGL applications on 64-bit installations.
- **Broad Distribution Support**: Packages are available for numerous Linux distributions through official or community repositories.
- **Source Availability**: The source code can be downloaded and compiled manually on unsupported distributions.

## System Requirements

The project does not publish official hardware requirements or minimum system requirements.

The only documented compatibility information is that SimpleScreenRecorder is intended to work on Linux distributions.

## Real-World Usage

A documented use case is recording 32-bit OpenGL applications on a 64-bit Linux system. This can be useful when capturing older games or applications that still depend on 32-bit graphics libraries.

The project does not publish migration guides, configuration examples, or known compatibility issues.

For Debian-based systems, the documentation includes one operational note: installation commands should be executed line by line instead of being pasted as a single block.

## Download & Installation

- **Official Website:** [SimpleScreenRecorder](http://www.maartenbaert.be/simplescreenrecorder/)
- **Documentation:** [Project Documentation](https://www.maartenbaert.be/simplescreenrecorder/#what-is-simplescreenrecorder)
- **Downloads:** [Download Page](https://www.maartenbaert.be/simplescreenrecorder/#download)
- **Source Code:** [GitHub Issues and Repository Information](https://github.com/MaartenBaert/ssr/issues)

### Arch Linux

Install `simplescreenrecorder` from the AUR or `simplescreenrecorder-git` for the latest development version.

_For recording 32-bit OpenGL applications on a 64-bit system:_

- `lib32-simplescreenrecorder`

### Debian

```bash
sudo apt-get update
sudo apt-get install simplescreenrecorder
```

_For 32-bit OpenGL applications:_

```bash
sudo apt-get install simplescreenrecorder-lib:i386
```

### Fedora

_Enable RPM Fusion:_

```bash
rpm -Uvh http://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-stable.noarch.rpm
```

_Install SimpleScreenRecorder:_

```bash
dnf install simplescreenrecorder
```

### Gentoo

```bash
sudo emerge -av media-video/simplescreenrecorder
```

### KaOS

Install the `simplescreenrecorder` package from the official repository.

### Mageia

Install `simplescreenrecorder` from the Core Release repository using `urpmi` or the Mageia Control Center.

### NixOS

```bash
nix-env -i simplescreenrecorder
```

### OpenSUSE

Install `simplescreenrecorder` from the Packman repository.

_For recording 32-bit OpenGL applications on a 64-bit system:_ 

- `libssr-glinject-32bit`

### PCLinuxOS

```bash
sudo apt-get install ssr
```

### Red Hat Enterprise Linux, CentOS, Scientific Linux

Packages are available through the Nux repositories.

### Slackware

A SlackBuild is available.

### Ubuntu and Linux Mint

```bash
sudo apt-get update
sudo apt-get install simplescreenrecorder
```

_For 32-bit OpenGL applications:_

```bash
sudo apt-get install simplescreenrecorder-lib:i386
```

_Older Ubuntu releases can also use the project’s PPA:_

```bash
sudo apt-add-repository ppa:maarten-baert/simplescreenrecorder
sudo apt-get update
sudo apt-get install simplescreenrecorder
```

### Other Distributions

If no package is available, SimpleScreenRecorder can be compiled from source.

## Open Source Alternatives

[**OBS Studio**](https://getfoss.org/audio-video/obs-studio-open-source-video-recording-and-live-streaming/): A larger application focused on recording and live streaming workflows.

[**FFmpeg**](https://getfoss.org/audio-video/ffmpeg-cross-platform-audio-and-video-processing-framework/): A command-line multimedia framework capable of screen capture and recording.

## Who Should Use It?

- Linux users who prefer native packages.
- Users recording OpenGL applications.
- Users who need to capture 32-bit OpenGL programs on 64-bit systems.
- Users who prefer software that can be built directly from source.

## Limitations

- The project does not publish detailed system requirements.
- The documentation does not provide a complete feature list.
- Some distributions rely on community-maintained packages.
- Users on unsupported distributions must compile the application themselves.

## Final Assessment

SimpleScreenRecorder is a free and open source Linux screen recorder with broad distribution support and documented support for recording OpenGL applications, including 32-bit applications on 64-bit systems.

The project documentation focuses more on installation and packaging than on application capabilities, so its full feature set is not described in detail. For users who prefer native Linux packages and need OpenGL recording support, SimpleScreenRecorder remains a practical option.
