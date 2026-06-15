---
title: 'Drawpile: Collaborative Drawing with Full Infrastructure Control'
description: Drawpile is a free and open-source collaborative drawing application that lets you host your own server, bypass vendor lock-in, and draw with others in real-time across any platform.
date: 2026-06-15T17:02:00+03:00
draft: false
image: /images/Drawpile.webp
categories:
  - graphics-design
tags:
  - Graphics
  - 2D
  - Self-hosted
  - Privacy
aliases: []
---

Drawpile is a collaborative drawing program that lets multiple users draw on the same canvas in real-time. It solves the problem of relying on proprietary, centralized platforms for collaborative art. Instead of handing your data and infrastructure over to a SaaS provider, Drawpile gives you the protocol and the option to self-host. It is Free and Open Source Software (FOSS), meaning the server binary, the client, and the protocol are open for auditing, modification, and deployment without license restrictions. The official website is [Drawpile.net](https://drawpile.net/).

## Why It Matters (The FOSS Angle)

Drawpile represents a practical victory for user autonomy. The project provides a dedicated server (`drawpile-srv`), putting network control in the operator’s hands. No vendor can arbitrarily change the terms of service, shut down the server, or mine your community’s data.

The project’s release notes and distribution methods highlight the friction FOSS developers face in closed ecosystems—and how they circumvent it. To bypass Apple’s App Store restrictions on iOS/iPadOS, Drawpile compiles to WebAssembly, allowing users to connect via a browser. To avoid Google’s developer fees for Android, the project distributes via F-Droid, explicitly noting that paying Google is the only way to remove the “unknown sources” installation friction. On macOS, the developers have been waiting on Apple for developer verification since August 2025, forcing users to bypass Gatekeeper manually. Drawpile proves that when you control the code, you can route around ecosystem gatekeeping.

## Key Features

- **Self-Hosted Dedicated Server:** Run `drawpile-srv` on your own hardware. You control the users, the logs, and the uptime. No third-party intermediary.
- **WebSocket Browser Client:** Users on locked-down devices (like iPads and iPhones) can join sessions directly through a web browser by connecting to a self-hosted WebSocket server.
- **Autorecovery (2.3.1 Beta):** The 2.3.1-beta.1 branch introduces autorecovery. If the client crashes, your offline or in-progress work is preserved—a critical fix for a workflow where losing an hour of painting is unacceptable.
- **Cross-Platform Support:** Runs natively on Windows (7 through 11, including ARM and 32-bit), macOS (Intel and Apple Silicon), Linux (Flatpak, AppImage), Android, and the Web.
- **Command-Line Tools:** Includes `dprectool`, `drawpile-cmd`, and `drawpile-timelapse` for automation, scripting, and server administration.
- **F-Droid Distribution:** Available on F-Droid for Android, ensuring the build is reproducible and free from proprietary tracker dependencies.
- **Verifiable Builds:** SHA256, SHA384, and BLAKE2b checksums are provided for releases. Windows and ARM binaries receive free code signing via SignPath Foundation.

## System Requirements

No official CPU, RAM, GPU, or storage requirements are published by the project.

**Documented dependencies for the client:**

- Qt (all platforms)
- OpenSSL (all platforms)
- KDE Framework Archive (Windows, Linux AppImage, Android)
- libzip (macOS, Linux Flatpak)

**Supported Operating Systems:**

- Windows: 7, 8, 8.1, 10, and 11 (requires Microsoft Visual Studio Redistributable)
- macOS: Intel and Apple Silicon (older builds available for versions prior to 12 Monterey)
- Linux: Flatpak and AppImage
- Android: 32-bit, 64-bit, and older Android versions supported

## Real-World Usage

Consider an art community that wants a private space to collaborate without using Discord screenshare or proprietary web tools. A sysadmin deploys `drawpile-srv` on a Linux VPS. They configure the WebSocket proxy so members joining from iPads can access the session via a web link. Linux users run the AppImage, which includes both the client and the dedicated server tools, while Windows users run the standard installer.

If a user on the 2.3.1 beta experiences a crash, the autorecovery feature restores their session, maintaining compatibility with the 2.3.0 stable users on the same server. The admin automates timelapse generation using the `drawpile-timelapse` command-line tool.

## Download & Installation

- **Official Website:** [Drawpile.net](https://drawpile.net/)
- **Documentation:** [Building from Source](https://docs.drawpile.net/help/development/buildingfromsource)
- **Downloads:** [Windows](https://drawpile.net/download/#Windows) | [macOS](https://drawpile.net/download/#OSX) | [Linux](https://drawpile.net/download/#Linux) | [Android](https://drawpile.net/download/#Android) | [Browser](https://drawpile.net/download/#Browser) | [Source](https://drawpile.net/download/#Source) | [Archive](https://drawpile.net/download/#Archive) | [Beta](https://drawpile.net/download/#Beta)
- **Source Code:** [GitHub](https://github.com/drawpile/) | [Codeberg](https://codeberg.org/Drawpile/Drawpile)

**Quick Start (Linux Dedicated Server via AppImage):**
The Linux Flatpak does not include the dedicated server. Use the AppImage instead.

1. Download the Linux AppImage.
2. Make it executable: `chmod +x Drawpile-*.AppImage`
3. Extract or run the server directly: `./Drawpile-*.AppImage --help` (to view included tools).
4. Launch the server: `./Drawpile-*.AppImage drawpile-srv` (adjust syntax based on `--help` output).
5. For WebSocket support to allow browser connections, consult the dedicated WebSocket server setup documentation.

## Open Source Alternatives

- [**MyPaint**](https://getfoss.org/graphics-design/mypaint-open-source-digital-painting-without-the-bloat/)**:** A solid FOSS drawing app, but it lacks built-in networked collaboration. It is strictly for local offline use.
- [**GIMP**](https://getfoss.org/graphics-design/gimp-the-ultimate-open-source-photo-editing-powerhouse/)**:** The standard FOSS raster editor. Can be used collaboratively via screen sharing or remote desktop tools, but lacks native real-time multi-canvas networking.

## Limitations

- **Ecosystem Friction:** On macOS, users must bypass Gatekeeper warnings because Apple has not verified the app. On Android, users must enable “unknown sources” unless they install via F-Droid.
- **Flatpak Limitations:** The Linux Flatpak is client-only. It excludes the dedicated server and command-line tools.
- **Apple Mobile Limitations:** There is no native iOS/iPadOS app. Users on these devices are restricted to the web browser version, which requires the server operator to configure WebSocket support.
- **Windows Dependencies:** The Windows client requires the Microsoft Visual Studio Redistributable, which must be installed manually if missing.

## Final Assessment

Drawpile is the definitive tool for users who want collaborative drawing without the surveillance and lock-in of proprietary web platforms. It excels in giving communities and sysadmins total control over their infrastructure via the dedicated server and WebSocket bridging. The tradeoff is navigating the friction imposed by closed ecosystems like Apple and Google, and managing minor platform inconsistencies (like the missing server tools in the Linux Flatpak). If you need real-time collaborative art and you value infrastructure control, run the AppImage, host the server, and route around the gatekeepers.
