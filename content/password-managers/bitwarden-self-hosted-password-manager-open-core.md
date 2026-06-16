---
title: 'Bitwarden: Self-Hosted Password Manager (Open Core)'
description: Bitwarden is an open-core password manager with FOSS clients and a proprietary server. Full FOSS compliance requires self-hosting with the Vaultwarden server implementation.
date: 2026-06-16T12:00:00+03:00
draft: false
image: /images/bitwarden.webp
categories:
  - password-managers
tags:
  - Passwords
  - Security
  - Self-hosted
  - Privacy
aliases: []
---

Bitwarden is a client-server password manager designed for cross-device synchronization. It solves the problem of real-time credential sharing without relying on proprietary cloud vendors, provided you self-host. The architecture is split: client applications (Desktop, Mobile, Browser, CLI) are FOSS under the GPLv3 license.

The official server is proprietary, released under a source-available license that restricts commercial use. To run Bitwarden as fully Free Software, a third-party server implementation is mandatory. The official website is [Bitwarden](https://bitwarden.com/).

## Why It Matters (The FOSS Angle)

Bitwarden operates on an open-core model. The clients are auditable, but the official server dictates the backend infrastructure.

Running the official server binary means executing a proprietary component. The official Bitwarden cloud service collects telemetry; self-hosting the official stack allows you to disable external telemetry via environment variables, but the server remains a source-available black box.

The FOSS viability of Bitwarden relies entirely on [Vaultwarden](https://getfoss.org/security-privacy/vaultwarden-self-hosted-bitwarden-server-rust/). Vaultwarden is an independent, fully FOSS Rust implementation of the Bitwarden API. It implements zero telemetry by design, drops the heavy C#/MSSQL dependency, and uses SQLite, MySQL, or PostgreSQL. The combination of official GPL clients + Vaultwarden server constitutes a complete, libre infrastructure.

## Key Features

- **Client-Server Sync:** Real-time credential synchronization across web, desktop, mobile, and CLI interfaces.
- **Self-Hosting:** The database and API remain on infrastructure you control.
- **Zero-Knowledge Encryption:** Vaults are encrypted and decrypted client-side using AES-256 and Argon2.
- **Organizations:** Granular access control and credential sharing for teams.
- **API and CLI:** Enables scripting, automation, and infrastructure secret management.
- **Cross-Platform:** Native applications and extensions for all major operating systems and browsers.

## System Requirements

**Official Self-Hosted Server (Windows via Docker):**

- **Minimum:** x64 1.4GHz Processor, 6GB RAM, 76GB Storage, Docker Engine 26+ and Compose.
- **Recommended:** x64 2GHz Dual Core Processor, 8+ GB RAM, 90GB Storage, Docker Engine 26+ and Compose.
- _Note:_ Running on Windows Server requires nested virtualization.

_No official client CPU, RAM, or storage requirements are published._

## Real-World Usage

Deploying a self-hosted, fully FOSS password manager for a team, avoiding proprietary cloud telemetry.

1. Deploy Vaultwarden via Docker with an SQLite database.
2. Place Vaultwarden behind a reverse proxy (Nginx/Caddy) configured with TLS (mandatory for client connections).
3. Disable public user registration via Vaultwarden environment variables (`SIGNUPS_ALLOWED=false`).
4. Users install the standard Bitwarden browser extension or mobile app.
5. In the client settings, change the “Self-hosted Environment” URL to the Vaultwarden domain.
6. The team operates a fully GPL/FOSS stack with real-time sync.

## Download & Installation

**Official Website:** [Bitwarden](https://bitwarden.com/)
**Documentation:** [Bitwarden Help](https://bitwarden.com/help/)

**Downloads:**
[Web Browser Extensions](https://bitwarden.com/download/#downloads-web-browser-extensions) · [Desktop Applications](https://bitwarden.com/download/#downloads-desktop-applications) · [Web Application](https://bitwarden.com/download/#web-application)
[Mobile Applications](https://bitwarden.com/download/#downloads-mobile-applications) · [Command-Line Interface](https://bitwarden.com/download/#command-line-interface) · [Self-Hosted Server](https://bitwarden.com/self-hosted-password-manager-on-premises/)

**Source Code:** [Bitwarden Server](https://github.com/bitwarden/server) (Source-available) · [Vaultwarden](https://getfoss.org/security-privacy/vaultwarden-self-hosted-bitwarden-server-rust/) (FOSS)

**Quick Start (Self-Hosted Official Server):**

1. Install Docker and Docker Compose.
2. Download the deployment script: `curl -Lso bitwarden.sh https://go.bitwarden.com/docker-compose.sh && chmod +x bitwarden.sh`
3. Install the stack: `./bitwarden.sh install`
4. Configure `./bwdata/env/global.override.env` to set the domain and disable system telemetry.
5. Start services: `./bitwarden.sh start`

**Quick Start (Vaultwarden - FOSS Alternative):**

1. Run the Docker container: `docker run -d --name vaultwarden -v /vw-data/:/data/ -e SIGNUPS_ALLOWED=false -p 80:80 vaultwarden/server:latest`
2. Attach a reverse proxy handling TLS termination.

## Open Source Alternatives

- [**Vaultwarden**](https://getfoss.org/security-privacy/vaultwarden-self-hosted-bitwarden-server-rust/)**:** A fully FOSS Rust implementation of the Bitwarden server API. Required to achieve a completely libre Bitwarden ecosystem. Uses minimal resources compared to the official stack.
- [**Passbolt**](https://getfoss.org/security-privacy/passbolt-team-password-manager-open-core/)**:** A fully open-source (AGPL) password manager designed for teams. Uses OpenPGP for authentication and encryption, integrated directly into the browser.
- [**KeePassXC**](https://getfoss.org/password-managers/keepassxc-cross-platform-password-manager/)**:** A local-first password manager. No server component. Sync relies on file synchronization tools (Syncthing, Nextcloud). Zero telemetry by architecture.

## Who Should Use It?

Teams and individuals who need real-time, cross-device credential synchronization and are willing to self-host. Users who value auditable clients but accept an open-core server model. Those who mandate a fully FOSS stack must use the Vaultwarden server implementation.

## Limitations

The official server is resource-heavy, requiring a minimum of 6GB RAM and 76GB storage for a Windows Docker deployment due to its C#/MSSQL architecture. The official server license is proprietary, restricting full software freedom. Relying on the official cloud introduces telemetry. Transitioning away from Bitwarden requires exporting unencrypted CSV/JSON, as the ecosystem locks data behind its specific API. Deploying Vaultwarden eliminates the resource overhead, running the same functionality on a fraction of the hardware footprint.

## Final Assessment

Bitwarden provides the most user-friendly cross-platform sync experience among self-hostable password managers. The clients excel in functionality and platform coverage. It falls short on FOSS purity due to the proprietary server and telemetry in the official cloud. Deploying Vaultwarden instead of the official server mitigates both the licensing and resource overhead, making the Bitwarden client ecosystem a practical, fully libre solution.
