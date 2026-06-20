---
title: 'CoreELEC: Minimal Linux OS for Amlogic Devices and Kodi'
description: CoreELEC is a Linux distribution designed to run Kodi on Amlogic SoC devices, providing a minimal, Just Enough OS environment for media playback.
date: 2026-06-20T07:32:00+03:00
draft: false
image: /images/CoreELEC.webp
categories:
  - audio-video
tags:
  - Video
  - Media
  - Streaming
  - Linux
aliases: []
---

CoreELEC is a minimal Linux distribution engineered specifically to run Kodi on Amlogic SoC hardware. It takes cheap Android TV boxes and single-board computers and turns them into dedicated, appliance-like media players. No Android overhead, no bloated vendor firmware — just the kernel, necessary drivers, and Kodi.

The project is FOSS, providing full source code and build transparency. By replacing the locked-down, often abandoned Android firmware on Amlogic devices with an open OS, users regain control over their hardware. The system is built around a Just Enough OS philosophy: if it isn’t required to boot the device and decode video, it isn’t included.

## Why It Matters (The FOSS Angle)

CoreELEC’s primary value is hardware liberation. Amlogic SoCs power millions of cheap TV boxes that vendors typically abandon without OS updates. CoreELEC steps in with an open, community-driven alternative that outlasts the original manufacturer support.

The 22.0-Piers alpha releases mark a significant architectural shift. CoreELEC 22 is the active development branch, moving to Kodi v22.0 Piers. This release enforces a hard cut: devices using Amlogic S905X (GXL) or older silicon are no longer supported. Dropping legacy hardware allows the project to focus development effort on modern Amlogic SoCs (like the S905X5, S928X, and S4) and ship a current vendor kernel (5.15.170). This is a standard FOSS trade-off: prioritize upstream progress and modern hardware over indefinite legacy maintenance.

## Key Features

- **Amlogic-specific optimization:** The kernel and userspace are tailored specifically for Amlogic SoCs, focusing on hardware video decoding, audio passthrough, and 3D playback (MVC frame packaging) via the `hdmitx20` driver.
- **Next-gen codec support:** Alpha builds introduce VVC/H266 hardware decoder support and track FFMPEG 8.0.1.
- **Linux kernel 5.15.170:** Includes updated codebase support for the latest Amlogic devices on the market.
- **Device Tree (DTB) abstraction:** Hardware configuration is handled by copying a specific `.dtb` file to the boot partition, allowing a single image to target multiple Amlogic boards.
- **Dedicated build variants:** Maintains separate image families (`Amlogic-ne` for aarch64, `Amlogic-ng` for arm) to target different generations of Amlogic silicon.
- **Diverse hardware support:** Recent updates add support for Khadas VIM1S/VIM4, Ugoos AM8/AM9/SK1/SK4, Minix U8K-Ultra, Odroid N2/C4/HC4, X96 X10, and various DVB tuner setups (internal tuners, MagicSee C500 Pro).
- **Automated update mechanism:** Built-in updater handles system upgrades, though alpha builds enforce daily auto-updates without a disable option.

## System Requirements

The project does not publish official minimum hardware requirements.

**Supported hardware (based on release notes):**

- Amlogic SoCs newer than S905X (GXL). Older Amlogic SoCs (S905X and prior) are not supported in the 22.0-Piers branch.
- Stable 21.x-Omega branch supports a broader range, including S905X2/X3/X4/X5, S922X, S928X, S4, and S5 via `Amlogic-ne` (aarch64) and `Amlogic-ng` (arm) images.

**Estimated requirements (typical for Amlogic media players):**

- **CPU:** Amlogic SoC (S905X2, S905X3, S905X4, S905X5, S922X, S928X, or newer)
- **RAM:** 2 GB recommended (1 GB absolute minimum for older arm images)
- **Storage:** 2 GB+ SD card, eMMC, or USB drive for installation
- **Network:** Ethernet or Wi-Fi (broad driver support included for Realtek, Broadcom, Amlogic W1/W2, and Unisoc chips)

## Real-World Usage

A standard deployment involves repurposing an Android TV box (e.g., a device with an Amlogic S905X3 or S928X SoC) as a dedicated media player. The user downloads the appropriate CoreELEC image, writes it to an SD card, selects the correct device tree, and boots. The device bypasses Android entirely, loading directly into Kodi.

### Installation Process

1. Download the `.img.gz` file for your device family via the Download Helper.
2. Decompress and write the image to an SD card. Rufus is recommended for Windows; `dd` works on Linux.
3. Locate the correct device tree file for your specific hardware in the `device_trees` folder.
4. Copy that file to the root of the SD card and rename it to `dtb.img`.
5. Insert the SD card into the device and power on.

### Alpha Build Caveats

The 22.0-Piers releases are unstable alpha builds. Installing them enrolls the device in daily automatic updates. This cannot be disabled. If you require control over update behavior, use the stable branch (21.3-Omega).

## Download & Installation

- **Official Website:** [CoreELEC](https://coreelec.org/)
- **Documentation:** [CoreELEC Wiki](https://wiki.coreelec.org/)
- **Downloads:** [Download Helper](https://coreelec.org/#download)
- **Update Mirrors:** [relkai.coreelec.org](https://relkai.coreelec.org/)
- **Source Code:** [GitHub Repository](https://github.com/CoreELEC/CoreELEC)

### Quick Start

**Prerequisites:** An Amlogic device newer than S905X, a 2GB+ SD card, and the correct `.dtb` file for your hardware.

**Write the image (Linux example):**

```plain
# Identify your SD card device
lsblk

# Decompress and write (replace /dev/sdX with your device)
gunzip -c CoreELEC-Amlogic-ne-*.img.gz | sudo dd of=/dev/sdX bs=4M conv=fsync status=progress

# Sync and mount
sync
sudo mount /dev/sdX1 /mnt
```

**Apply Device Tree:**

```plain
# Copy the correct DTB for your device (example: Odroid N2)
# Find these in the device_trees folder extracted from the release archive
sudo cp device_trees/meson64_odroid_n2.dtb /mnt/dtb.img

sudo umount /mnt
```

Insert the SD card into the Amlogic device and power on. Kodi launches automatically.

## Open Source Alternatives

- [**LibreELEC**](https://getfoss.org/audio-video/libreelec-minimal-linux-os-for-kodi-media-center/)**:** The broader minimal Kodi OS. Supports Raspberry Pi, x86_64, and Rockchip/Allwinner. Does not specialize in Amlogic hardware. Better choice if you want hardware diversity.
- **OSMC:** Based on Debian. Offers a full apt-based userspace, providing more flexibility at the cost of overhead. Runs well on Raspberry Pi and their own Vero hardware.
- [**Armbian**](https://docs.armbian.com/) **+** [**Kodi**](https://getfoss.org/audio-video/kodi-cross-platform-media-center-and-htpc-software/)**:** Not a JEOS (Just Enough OS) distribution. You install standard Armbian Linux and add Kodi via apt. Maximum flexibility, maximum configuration effort.

## Who Should Use It?

- Users with an Amlogic-based Android TV box wanting to escape vendor firmware.
- Tinkerers running Amlogic single-board computers (Khadas VIM, Odroid N2/C4) as dedicated media appliances.
- Users who need hardware video decoding on cheap Amlogic silicon and want a pure Kodi environment.
- Anyone willing to find and apply the correct device tree file for their specific board.

## Limitations

- **Amlogic-only:** Do not attempt to run this on Raspberry Pi, Rockchip, or x86_64 hardware. The entire OS is hardcoded to Amlogic SoCs.
- **Legacy hardware cut-off:** S905X (GXL) and older are dead in the 22.x branch. If you have an old S905X box, you are stuck on older CoreELEC releases or LibreELEC.
- **Device Tree friction:** Finding the exact `dtb.img` for a generic Chinese Android box can require experimentation. If you pick the wrong one, the device won’t boot.
- **Forced alpha updates:** The 22.0-Piers alpha builds auto-update daily and cannot be disabled. This will break things unexpectedly. Only run these if you intend to test and report bugs.
- **No general Linux userspace:** Like LibreELEC, this is an appliance. No package manager, no desktop environment.

## Final Assessment

CoreELEC is the definitive solution for turning cheap Amlogic hardware into a capable Kodi media center. The project does an excellent job of tracking modern Amlogic silicon and keeping the kernel updated. The 22.0-Piers alpha demonstrates a healthy development cycle, unafraid to drop legacy hardware (S905X and older) to maintain code quality and add next-gen features like VVC/H266 decoding.

Where it struggles: the alpha update mechanism is aggressive and unforgiving. The reliance on manual DTB file selection adds friction to the initial setup. It is a specialized tool for a specific family of hardware. If you have a modern Amlogic box and want a pure Kodi appliance, CoreELEC is the right choice. If you want a general-purpose Linux system, look elsewhere.
