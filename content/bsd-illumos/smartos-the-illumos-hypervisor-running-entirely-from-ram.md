---
title: 'SmartOS: The illumos Hypervisor Running Entirely from RAM'
description: SmartOS is a live Type 1 hypervisor based on illumos, designed to run entirely from memory while dedicating all local disks to ZFS-backed virtual machines.
date: 2026-06-15T18:33:00+03:00
draft: false
image: /images/smartos.webp
categories:
  - bsd-illumos
tags:
  - illumos
  - ZFS
  - Server
  - Unix
aliases: []
---

SmartOS is a specialized Type 1 hypervisor platform based on illumos, designed specifically to run virtual environments. It boots as a “live OS” from PXE, ISO, or USB into RAM, then uses local disks exclusively for hosting VM storage pools. This separation of host (in memory) from persistent state (on ZFS) is the core architectural choice. It is Free and Open Source Software, built on the illumos stack and the `smartos-live` repository.

Official website: [SmartOS](https://tritondatacenter.com/smartos)

## Why It Matters (The FOSS Angle)

SmartOS turns the illumos tooling—ZFS, DTrace, Zones, Crossbow—into a hypervisor platform you can fully audit and control. Because the OS is a live image, patching the hypervisor typically means rebooting into a new build rather than maintaining a persistent root filesystem. The smartos-live repository and the open development model allow you to inspect and rebuild the entire platform, avoiding vendor lock-in and opaque management layers.

The live-image design also clarifies the boundary between host and guest: the host is disposable, state lives on ZFS, and upgrades are a reboot. This modularity is a practical expression of FOSS principles: components with clearly defined roles, user control over the stack, and no hidden patching or telemetry hooks.

## Key Features

- **Live OS architecture** – SmartOS boots entirely from PXE/ISO/USB into RAM; local disks are used for ZFS pools backing VMs, not for the root OS. This simplifies upgrades and recovery.
- **OS Virtualization (Zones)** – Lightweight containers sharing a single illumos kernel, offering bare-metal performance with full DTrace introspection.
- **Hardware Virtualization (KVM/Bhyve)** – Full VMs for Linux, Windows, BSD, Plan 9, etc. The QEMU or Bhyve process runs inside a stripped-down Zone, unifying management and adding an isolation layer.
- **ZFS-based storage** – All persistent storage sits on ZFS, providing checksumming, snapshots, and clones for guest instances.
- **Network virtualization (Crossbow)** – Uses `dladm` to create virtual NICs and switches, enabling strict network isolation between tenants.
- **JSON-driven management** – `vmadm` and `imgadm` consume and emit JSON, enabling programmatic VM lifecycle management without scraping text output.
- **DTrace observability** – Inherits illumos DTrace for dynamic kernel and userland tracing of both the hypervisor and Zone workloads.

## System Requirements

SmartOS publishes two sets of requirements that overlap but differ slightly.

### Hypervisor / Live Image Requirements

_From the “Getting Started with SmartOS” documentation:_

- **CPU:** 64‑bit x86 CPU.
- **RAM:** A minimum of 1 GB of RAM for the hypervisor; “all other compute resources available will be used for virtual instances.”
- **Hardware virtualization:** To use hardware virtualization features, SmartOS requires an Intel CPU with VT‑x or an AMD CPU with AMD‑V; most modern CPUs meet this.

### Hardware Requirements (Production / KVM-Focused)

_From the “Hardware Requirements” page:_

- **CPU:** 64‑bit capable x86 Intel or AMD CPU.
- **DRAM:** At least 512 MB of DRAM; the documentation recommends installing as much RAM as possible.
- **Networking & storage:** At least one networking interface and a supported disk controller. Supported devices are listed in the illumos Hardware Compatibility List (HCL).
- **KVM on Intel:** Supported on Intel processors with both VT‑x and Extended Page Tables (EPT). EPT was introduced with the Nehalem line. As a rule of thumb, supported CPUs include:
    - Xeon E3, E5, E7
    - Xeon 54xx, 55xx, 56xx, 74xx, 75xx, 76xx
    - Some Xeon 34xx, 35xx, 36xx models
    - Some Core i3, i5, i7
    - Most newer Sandy Bridge and Ivy Bridge desktop Pentium and Celeron processors
- **AMD / non‑EPT Intel:** Support for AMD CPUs and Intel CPUs without EPT is under community development and is not yet supported in the main SmartOS builds. Community “eait‑Images” built by arekinath include AMD support and are intended for testing so the code can eventually be merged upstream.
- **Known issues:** Some Intel CPU C‑states have caused problems on illumos; SmartOS works around them, but the documentation recommends considering disabling C‑states in the BIOS.

In practice, treat 1 GB as the realistic minimum for the live hypervisor image, and 512 MB as the lower bound for the SmartOS platform itself on minimal hardware. For any production use with KVM and multiple VMs, you will want significantly more RAM.

## Real-World Usage

A typical deployment looks like this: you boot SmartOS from a USB key on a server with Intel EPT‑capable CPUs. On first boot, the configuration utility sets up networking, the root password, and selects disks for the persistent ZFS pool. After a reboot, you log into the global zone (the hypervisor).

_From the global zone:_

- Use `imgadm` to import VM images.
- Use `vmadm` to create native SmartOS zones and HVM (KVM/Bhyve) instances.
- Use `pkgin` in the global zone (installing under `/opt/tools`) and in non‑global zones (under `/opt/local`) to add software. Configuration lives in `/opt/local/etc` (or `/opt/tools/etc`), with pristine examples preserved in `/opt/local/share/examples`, so updates don’t overwrite custom configs.

_Example workflow for a mixed workload:_

1. Create a native SmartOS zone for an Nginx reverse proxy (bare‑metal performance, DTrace visibility).
2. Create a KVM instance running a legacy Linux database application.
3. Place both on the same ZFS pool, isolated by Crossbow networks and managed via JSON payloads to `vmadm`.

_For KVM specifically, ensure:_

- Intel CPUs support VT‑x with EPT (check the Intel list referenced by the docs).
- Nested virtualization is enabled if you run SmartOS as a VM and want KVM/Bhyve inside it.
- You avoid overwriting the boot disk (`c0t0d0`) when using the VMware image.

## Download & Installation

- **Official Website:** [SmartOS](https://tritondatacenter.com/smartos)
- **Documentation:** [SmartOS Wiki](https://wiki.smartos.org/)
- **Downloads:** [ISO Image](https://us-central.manta.mnx.io/Joyent_Dev/public/SmartOS/smartos-latest.iso) | [USB Image](https://us-central.manta.mnx.io/Joyent_Dev/public/SmartOS/smartos-latest-USB.img.gz) | [VMware VM Image](https://us-central.manta.mnx.io/Joyent_Dev/public/SmartOS/smartos-latest.vmwarevm.tar.gz) | [Platform Archive](https://us-central.manta.mnx.io/Joyent_Dev/public/SmartOS/platform-latest.tgz) | [All Latest Release Objects](https://us-central.manta.mnx.io/Joyent_Dev/public/SmartOS/latest.html)
- **Source Code / Issues:** [smartos-live on GitHub](https://github.com/TritonDataCenter/smartos-live)

### Quick Start

1. **Choose media.**

    - Physical hardware: Write the USB image to a key.
    - VMware: Extract the VM image.
    - VirtualBox/UTM: Use the ISO.

2. **Boot and configure.**
Boot the media. The system loads into RAM and starts a configuration utility. Set network interfaces, root password, and select disks for the ZFS pool. The system then reboots.
3. **Log in.**
Log in via console or SSH; you’re in the global zone.
4. **Import images and create a VM**. imgadm update
imgadm import <uuid>
vmadm create -f payload.json
5. **Warnings.**

    - Do not format or overwrite the first disk (`c0t0d0`) in the VMware image; that is the boot media.
    - Enable nested virtualization in VMware if you need KVM/Bhyve inside the VM.
    - Builds are timestamped (e.g., `20120809T221258Z`), not versioned; new builds appear roughly every two weeks.

## Open Source Alternatives

- [**Proxmox VE**](https://www.proxmox.com/en/products/proxmox-virtual-environment/overview) – Debian‑based hypervisor using KVM and LXC with a web UI. More accessible out of the box, but carries a full persistent root filesystem and does not provide the integrated illumos stack (ZFS/DTrace/Zones as first‑class citizens).
- [**Xen Project**](https://xenproject.org/) – Mature bare‑metal hypervisor with broad guest support. Lacks the deep ZFS/DTrace integration and Zone‑style containerization of illumos.
- [**FreeBSD bhyve**](https://docs.freebsd.org/en/books/handbook/virtualization/) – Hypervisor built into FreeBSD. Strong virtualization on a familiar BSD platform, but not a stateless, RAM‑booted OS by default.

## Who Should Use It?

_SmartOS is for sysadmins and infrastructure engineers who:_

- Need high VM density and strong isolation.
- Value observability (DTrace), data integrity (ZFS), and programmatic management (JSON tools).
- Are comfortable with CLI‑driven workflows and illumos conventions.
- Want an auditable, FOSS hypervisor stack with no hidden vendor lock‑in.

It is not for teams that require a glossy web UI or a Linux‑centric toolchain.

## Limitations

- **Steep illumos learning curve.** SMF, ZFS administration, and Zone semantics differ significantly from Linux; expect a non‑trivial ramp‑up.
- **KVM CPU constraints.** KVM on SmartOS officially supports Intel CPUs with VT‑x and EPT; AMD and non‑EPT Intel support is community‑only and not yet merged.
- **Global zone package layout.** pkgsrc installs into `/opt/tools` rather than standard Unix paths, which can clash with scripts expecting `/usr/local` or `/opt/local`.
- **Stateless host, manual automation.** The live‑image design means host‑level config does not persist across reboots unless you automate it (e.g., PXE boot scripts or config management).
- **No built‑in web UI.** Management is via CLI/JSON; you must bring your own automation or UI layer.

## Final Assessment

SmartOS is a focused, uncompromising hypervisor for environments where observability, data integrity, and resource density matter more than ease of adoption. Its live‑image, RAM‑booted architecture and deep ZFS/DTrace/Zones integration provide a technically coherent FOSS alternative to mainstream Linux hypervisors—especially if you’re already invested in illumos.

The trade‑off is complexity and a narrower hardware ecosystem for KVM. If you need maximum control over your virtualization stack and are willing to learn illumos, SmartOS is a strong choice; if you want a turnkey solution with a friendly UI, look elsewhere.
