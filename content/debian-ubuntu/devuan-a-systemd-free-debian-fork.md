---
aliases:
    - /devuan-a-systemd-free-debian-fork/
title: "Devuan: A Systemd-Free Debian Fork"
date: 2026-06-09T11:29:29
lastmod: 2026-06-09T20:34:08
description: "Deploy Devuan Linux for a systemd-free environment based on Debian. Review technical notes, configuration details, and installation steps."
categories:
  - debian-ubuntu
tags:
  - Debian
  - Linux
  - APT
  - Server
slug: "devuan-a-systemd-free-debian-fork"
comments: false
image: "/images/devuan-webp.webp"
---

<p>Devuan is a free GNU+Linux operating system forked from Debian. It exists because a segment of the user base objected to Debian's adoption of systemd as the default init system. Devuan provides a Debian-compatible environment—using APT, the deb package format, and the same filesystem layout—without requiring systemd. It is Free and Open Source Software. The official website isВ <a href="https://www.devuan.org" target="_blank" style="color: red;" rel="noopener">www.devuan.org</a>.</p>
<p></p>
<h2>Why It Matters (The FOSS Angle)</h2>
<p>Devuan provides an alternative for users who prefer modular init systems. This modularity allows administrators to use standard shell scripts for service management and plain text logs for auditing. Devuan releases track Debian releases on a 1-to-1 basis using different codenames.</p>
<p>The Excalibur release notes detail several structural changes. A merged-/usr filesystem is now compulsory, requiring the usrmerge package before upgrading. Support for login(8) registering sessions in /run/utmp has been restored. Additionally, the Pipewire media server is recommended over Pulseaudio due to fewer latency issues, though Pulseaudio remains necessary for users requiring a screen reader for graphical login.</p>
<h2>Key Features</h2>
<ul>
<li><strong>Init Alternatives:</strong> Systemd is excluded. Users can select sysvinit (the default), openrc, or runit.</li>
<li><strong>Debian Compatibility:</strong> Devuan tracks Debian suites, maintaining compatibility with APT and the deb package ecosystem.</li>
<li><strong>Architecture Support:</strong> Available for amd64, arm64, armel, armhf, ppc64el, and riscv64. Note that i386 packages do not include a linux-image, and there is no i386 installer ISO.</li>
<li><strong>Elogind Integration:</strong> Provides the logind API without requiring systemd, enabling desktop environments like Cinnamon, KDE, LXQt, LXDE, and MATE to function.</li>
<li><strong>Docker Images:</strong> Official Devuan Docker container images are available from the Docker Hub, updated for all maintained releases.</li>
<li><strong>Tor Repository Access:</strong> Package repositories are accessible via Tor using the hidden service address devuanfwojg73k6r.onion.</li>
<li><strong>Non-free Firmware Control:</strong> Installation media makes non-free firmware available, but automatic installation can be avoided by choosing "Expert install". Live images include a script at /root/remove_firmware.sh to purge these packages post-install.</li>
</ul>
<h2>System Requirements</h2>
<p>No official system requirements are published.</p>
<h3>Configuring Pipewire</h3>
<p><em>For audio management without Pulseaudio, administrators can install Pipewire:</em></p>
<pre><code>apt-get install pipewire pipewire-pulse wireplumber</code></pre>
<p>To accommodate both console and GUI desktop sound sessions, add the following shell snippet to <code>~/.profile</code>:</p>
<pre><code>if [ "$XDG_RUNTIME_DIR" = "/run/user/$(id -u)" ] ; then
psess_pids=
for p in pipewire wireplumber pipewire-pulse ; do
command -v $p &gt;/dev/null || continue
pgrep --exact --uid $USER $p &gt;/dev/null &amp;&amp; continue
$p &amp;
psess_pids="$! ${psess_pids}"
done
[ "$psess_pids" ] &amp;&amp; trap "kill $psess_pids" EXIT
fi</code></pre>
<p>Then, ensure <code>~/.xsession.rc</code> sources the profile:</p>
<pre><code>if [ -f ~/.profile ]; then
. ~/.profile
fi</code></pre>
<h2>Download &amp; Installation</h2>
<ul>
<li><strong>Official Website:</strong> <a href="https://www.devuan.org" target="_blank" rel="noopener noreferrer" style="color: red;">https://www.devuan.org</a></li>
<li><strong>Documentation:</strong> <a href="https://www.devuan.org/os/documentation/" target="_blank" rel="noopener noreferrer" style="color: red;">https://www.devuan.org/os/documentation/</a></li>
<li><strong>Downloads:</strong> <a href="https://www.devuan.org/get-devuan" target="_blank" rel="noopener noreferrer" style="color: red;">https://www.devuan.org/get-devuan</a></li>
<li><strong>Development &amp; Issues:</strong> <a href="https://www.devuan.org/os/gitlab-issues" target="_blank" rel="noopener noreferrer" style="color: red;">https://www.devuan.org/os/gitlab-issues</a></li>
</ul>
<h3>Installation Media Types</h3>
<p><em>Devuan offers specific ISO types tailored for different use cases:</em></p>
<ul>
<li><strong>desktop-live:</strong> Explore the default Xfce desktop before installing. Installation is handled from the live session using the refractainstaller.</li>
<li><strong>netinstall:</strong> Preferred for servers and minimal systems. Requires an internet connection, as all but the core components are downloaded during installation.</li>
<li><strong>server:</strong> A CD set for offline installation. CD-1 installs a minimal server system. CD-2 provides additional server packages. CD-2, CD-3, and CD-4 provide MATE or XFCE desktops. CD-2, CD-3, and CD-5 provide LXDE or LXQt desktops.</li>
<li><strong>desktop:</strong> A DVD image for offline installations. Required for KDE and Cinnamon, as they do not fit on standard CD sizes.</li>
<li><strong>minimal-live:</strong> A console-based recovery tool focusing on accessibility for visually-impaired and blind users.</li>
</ul>
<h3>Quick Start: Netinstall Installation</h3>
<p><strong>Prerequisites:</strong> Knowledge of how to write an ISO image to a USB drive and boot from it.</p>
<ol>
<li>Download the netinstall ISO and the corresponding SHA256SUMS and SHA256SUMS.asc files.</li>
<li>Verify integrity: <code>sha256sum -c SHA256SUMS</code></li>
<li>Verify signature: <code>gpg --no-default-keyring --keyring ./devuan-devs.gpg --verify SHA256SUMS.asc</code></li>
<li>Write the image to a USB drive: <code>dd if=filename.iso of=/dev/sdX bs=1M &amp;&amp; sync</code></li>
<li>Boot the media and follow the prompts. The "Expert install" option allows opting out of non-free firmware and selecting alternate bootloaders.</li>
<li>Select a network mirror. Use <code>deb.devuan.org</code> unless a faster local mirror is available.</li>
<li>Choose your init system. Sysvinit is the default.</li>
<li>Install the GRUB bootloader to the MBR of the hard disk.</li>
<li>Reboot into the installed system.</li>
</ol>
<p>When configuring <code>/etc/apt/sources.list</code>, use release codenames (like <code>excalibur</code>) instead of suite names (like <code>stable</code>). This prevents unexpected upgrades when a new stable release is published.</p>
<h2>Open Source Alternatives</h2>
<ul>
<li><strong><a href="https://www.getfoss.org/alpine-linux-the-open-source-lightweight-os-alternative" target="_blank" rel="noopener"><span style="color: #e03e2d;">Alpine Linux</span></a>:</strong> Uses musl libc and busybox with OpenRC. It is not Debian compatible and does not use APT or glibc.</li>
<li><strong><a href="https://www.getfoss.org/artix-linux-the-open-source-systemd-free-powerhouse" target="_blank" rel="noopener"><span style="color: #e03e2d;">Artix Linux</span></a>:</strong> An Arch Linux derivative without systemd. It uses pacman and the Arch repository structure.</li>
<li><strong><a href="https://www.getfoss.org/debian-the-rock-solid-open-source-os-foundation" target="_blank" rel="noopener"><span style="color: #e03e2d;">Debian</span></a>:</strong> The upstream project. Select this if systemd is acceptable and upstream community support is the priority.</li>
</ul>
<h2>Limitations</h2>
<p>Software that hard-depends on systemd will not work. The i386 architecture lacks an installer ISO and a linux-image package, following Debian's upstream choice. Pipewire is not suitable for users requiring a screen reader for graphical login, who must stick to Pulseaudio. The mandatory merged-/usr filesystem in Excalibur requires explicit preparation prior to upgrading from Daedalus.</p>
<h2>Final Assessment</h2>
<p>Devuan provides a direct path for users who need Debian packages without systemd. It excels in server and headless deployments where sysvinit, openrc, or runit are preferred. The elogind integration makes it viable for desktop use, provided the required desktop environment does not hard-depend on systemd. It falls short for users who rely on software explicitly built for systemd or those requiring i386 installer media. If your workflow requires strict systemd integration, Devuan is not the right choice. If your software runs without it, Devuan offers a stable, modular alternative.</p>


