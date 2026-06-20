---
title: 'Batocera.linux: Open-Source Retro Gaming OS'
description: Batocera.linux is an open-source, dedicated retro gaming operating system that turns PCs, single-board computers, and handhelds into emulation consoles.
date: 2026-06-20T08:26:00+03:00
draft: false
image: /images/batocera.webp
categories:
  - diy-advanced
tags:
  - Linux
  - Advanced
  - DIY
  - Misc
aliases: []
---

Batocera.linux is a dedicated, minimal Linux distribution built specifically for retro gaming and emulation. It transforms standard x86_64 PCs, single-board computers (like the Raspberry Pi), and ARM-based handhelds into plug-and-play consoles. There is no desktop environment to configure. You write the image to a drive, boot it, and it loads directly into a customized EmulationStation frontend.

The project is FOSS, with the source code publicly available on GitHub. It bundles hundreds of emulators, from arcade (MAME) to modern consoles (PS3, Switch, Xbox). By isolating the OS from the host system, Batocera provides a clean, console-like experience without the driver conflicts and background bloat of a standard Windows or desktop Linux setup.

## Why It Matters (The FOSS Angle)

Batocera applies the appliance model to gaming. Instead of relying on proprietary, closed ecosystems like standard consoles, it leverages decades of open-source emulator development (RetroArch, Dolphin, PCSX2, RPCS3) into a unified, read-only root filesystem.

The v43 “Glasswing” release highlights a strict adherence to FOSS hygiene. The developers completely removed the DraStic emulator (a Nintendo DS emulator) from the image specifically because it is closed-source and no longer compatible with the core OS. This is a definitive stand: utility does not override the project’s open-source principles.

Furthermore, v43 aggressively modernizes the underlying stack. It drops legacy Nvidia 340.xx and 390.xx drivers—hardware that is unsustainable to maintain against modern Linux kernels. It shifts x86_64 handhelds with AMD/Intel graphics to a Wayland + LabWC compositor on the `x86_64-v3` image. It also deprecates user `custom.sh` scripts in favor of proper systemd services. This is active, unsentimental maintenance.

## Key Features

- **Wayland and LabWC Integration:** The `x86_64-v3` image now defaults to Wayland with the LabWC compositor for modern AMD/Intel hardware, moving away from legacy Xorg dependencies.
- **Massive Emulator Coverage:** Supports everything from the Atari 2600 to Sony PlayStation 3, Nintendo Switch, and Microsoft Xbox via bundled emulators like RPCS3, Ryujinx, and Xenia.
- **Broad Hardware Support:** Specific images target Raspberry Pi, Radxa, Anbernic handhelds, AYN devices, and standard x86_64 PCs.
- **Advanced Input Support:** Built-in configuration for light guns (Sinden, GUN4IR, GunCon2), racing wheels (Logitech, MOZA), and NFC readers. v43 adds a new UI for configuring in-game controller hotkeys globally.
- **Wine and Proton Integration:** Runs Windows games via Wine-TKG (10.20) and Proton, with support for creating Win32 bottles for older titles.
- **Moonlight QT:** Transitions from Moonlight Embedded to Moonlight QT for better hardware acceleration during remote game streaming.

## System Requirements

**Official Storage Requirement:**

- 16 GB minimum storage space.
- 32 GB recommended (required to utilize automatic OTA updates).

**Supported Architectures:**

- x86_64 (Standard PCs)
- x86_64-v3 (Modern PCs and handhelds with Wayland/LabWC)
- ARM (Raspberry Pi, Odroid, various handhelds)

**Estimated Hardware Requirements (Typical for emulation):**

- **CPU (x86_64-v3):** Intel 2013 or newer, AMD Zen or newer (supports AVX2).
- **CPU (ARM):** Cortex-A53 or better (Raspberry Pi 3 or equivalent for basic 2D/16-bit; higher-end SoCs for PSP/GameCube).
- **RAM:** 2 GB minimum for basic emulation, 4 GB+ recommended for PS2/GameCube/Wii, 8 GB+ for PS3/Switch.
- **Graphics:** OpenGL 3.3 or Vulkan 1.1 compatible GPU. Modern Nvidia GPUs require proprietary drivers (experimental on v3/Wayland).

## Real-World Usage

A standard deployment uses an old desktop PC or a dedicated mini-PC connected to a living room TV. The user flashes the x86_64 image to a USB SSD, boots from it, and configures controllers via EmulationStation. ROMs are transferred over the network via Samba shares to the `/userdata/roms/` directory.

### BIOS and UEFI Configuration

Batocera will not boot on modern hardware with default factory BIOS settings. You must intervene.

- **Disable Secure Boot:** If you intend to keep Secure Boot enabled, you must enroll Batocera’s keys on first boot. It is faster to disable Secure Boot entirely.
- **Disable Fast Boot / TPM:** Windows 11 compatibility features (TPM 2.0, Fast Boot) will block alternative operating systems from loading. Disable them.
- **Legacy/CSM:** UEFI is preferred. If the motherboard forces Secure Boot with UEFI, you may need to enable Legacy/CSM boot modes.

### Nvidia Driver Caveats

If using an Nvidia GPU, the open-source `nouveau` driver is the default. For actual performance, you must manually activate proprietary drivers.
**Warning:** The `x86_64-v3` (Wayland) image considers Nvidia proprietary drivers experimental. Desktop Nvidia users should download the standard `x86_64` (Xorg) image for stability. Legacy 340.xx and 390.xx drivers are permanently removed; if you have an old card, it will run on `nouveau` or not at all.

### custom.sh Deprecation

If you previously used a `custom.sh` script for startup tasks, v43 will one-time transform it into a service. Stop using `custom.sh` and write proper systemd services for future scripts.

## Download & Installation

- **Official Website:** [batocera.org](https://batocera.org/)
- **Documentation:** [Batocera Wiki](https://wiki.batocera.org/)
- **Downloads:** [Download Page](https://batocera.org/download)
- **Source Code:** [GitHub Repository](https://github.com/batocera-linux/batocera.linux)
- **Issue Tracker:** [GitHub Issues](https://github.com/batocera-linux/batocera.linux/issues)

### Quick Start

**Prerequisites:** A target PC/device, a 32GB+ USB drive or SSD, and an imaging tool like Raspberry Pi Imager.

**Write the image:**

1. Download the appropriate architecture image (e.g., `batocera-x86_64-v3-*.img.gz`).
2. Open Raspberry Pi Imager (or Balena Etcher).
3. Select “Use custom” and choose the downloaded `.img.gz` file.
4. Select your target USB drive or SSD.
5. Write the image.

**First Boot:**

1. Insert the drive into the target machine and power on.
2. Enter the BIOS boot selection menu (typically F10, F11, or F12).
3. Select the Batocera drive.
4. The OS will automatically expand the `SHARE` (userdata) partition on first boot and reboot. Do not power off during this process.
5. EmulationStation will launch. Press `START` with a controller to access the main menu and configure network/storage settings.

## Open Source Alternatives

- **RetroArch:** Not a standalone OS, but the underlying frontend/core framework Batocera uses. You can install RetroArch on any standard Linux or Windows machine. Better if you want to keep your existing OS and just run an application.
- [**Lakka**](https://getfoss.org/diy-advanced/lakka-libreelec-based-retroarch-emulation-console/)**:** A libretro distribution built on LibreELEC. Much more minimal than Batocera, using only the RetroArch frontend. No desktop environment, no standalone emulators. Better for very low-end hardware, worse if you need emulators outside the libretro ecosystem.
- [**Recalbox**](https://getfoss.org/other/recalbox-open-source-retro-gaming-emulation-platform/)**:** Historically a fork of Batocera. Offers a similar turnkey experience but has moved toward a more commercial/closed model in recent years. Batocera remains the more community-driven, open alternative.

## Who Should Use It?

- Users with an old PC or laptop who want a dedicated emulation console without maintaining a desktop OS.
- Handheld hardware enthusiasts (Anbernic, AYN, Odin) who want a better OS than the stock Android firmware.
- Tinkerers who want a pre-configured Wayland/LabWC environment optimized for game controllers, light guns, and racing wheels.
- Anyone who values a completely FOSS gaming system, free from the telemetry of modern commercial consoles.

## Limitations

- **Nvidia Wayland Friction:** Nvidia users are second-class citizens on the new `x86_64-v3` Wayland image. You must fall back to the Xorg `x86_64` image for stable proprietary driver support.
- **Legacy Hardware Drops:** Old Nvidia cards (340.xx/390.xx era) are out of luck. The OS is moving forward without them.
- **Read-Only Root:** The root filesystem is squashfs. System-level modifications require rebuilding the image or using overlay filesystems. This is an appliance, not a general-purpose Linux distro.
- **Storage Minimums:** You need a 32GB drive to receive OTA updates. 16GB will run the OS, but you can’t auto-update.
- **ROM and BIOS Management:** You are entirely responsible for sourcing and placing BIOS files and ROMs. Batocera provides the engine, not the media.

## Final Assessment

Batocera is the most comprehensive FOSS gaming OS available. The v43 release makes hard, correct decisions: dropping closed-source emulators, cutting legacy driver support, and migrating modern hardware to Wayland. It respects the user by handling complex input device configurations (light guns, multi-monitor handhelds) that are a nightmare to set up manually on standard Linux.

Where it shines: turning e-waste hardware into a capable retro console with zero desktop maintenance. Where it falls short: Nvidia GPU users face an experimental driver landscape on the new Wayland image, and the strict appliance model limits OS-level tinkering. If you want a plug-and-play emulation box, Batocera is the definitive choice.
