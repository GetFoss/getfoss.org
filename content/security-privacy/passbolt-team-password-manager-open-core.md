---
title: 'Passbolt: Team Password Manager (Open Core)'
description: Passbolt is a team password manager with an AGPLv3 Community Edition server and clients. It uses OpenPGP for end-to-end encryption, offering a fully FOSS stack for self-hosted teams.
date: 2026-06-16T13:05:00+03:00
draft: false
image: /images/passbolt.webp
categories:
  - security-privacy
tags:
  - Passwords
  - Security
  - Self-hosted
  - Privacy
aliases: []
---

Passbolt is a team-focused password manager designed for secure credential sharing. It solves the problem of collaborative vault management by using OpenPGP for end-to-end encryption, ensuring the server never handles plaintext data. The licensing is split: the Community Edition (CE) is fully open-source under AGPLv3, while the Business and Enterprise tiers are proprietary paid add-ons. For teams that require verifiable, FOSS credential infrastructure based on standard cryptography, Passbolt CE provides the complete stack. The official website is [Passbolt](https://www.passbolt.com/).

## Why It Matters (The FOSS Angle)

Passbolt operates on an open-core model, but its FOSS offering is significantly more transparent than competitors like Bitwarden. The entire Passbolt CE stack—API server, browser extension, CLI, and mobile apps—is AGPLv3. There is no separate “source-available” server binary holding your data hostage.

**How to get the Full FOSS Stack:**

Run the Passbolt CE server natively or via Docker, connect the open-source browser extension, and use the open-source mobile apps. You get unlimited users, RBAC, and full API access without license restrictions.

**What you lose by staying FOSS (Pro/Enterprise features):**

Tags management, LDAP/AD provisioning, SSO (Microsoft/Google/OpenID), Account recovery (Escrow), and Activity logs.

_Recent updates (API v5.13.0, Mobile v3.0.0) focus on administration and scalability:_

- **In-App Edition Management:** Administrators can now upgrade from CE to Pro and downgrade back to CE directly from the UI without manual database migrations. The codebase is unified; Pro is simply an unlocked state of the CE code.
- **Paginated Resource Fetching:** Browser extensions and mobile apps now paginate API requests. This reduces server load and client memory usage for large vaults.
- **Healthcheck Enhancements:** The `/healthcheck/status.json` endpoint now verifies caching system availability, improving monitoring for self-hosted instances.
- **Mobile Architecture Overhauls:** Android v3.0.0 completed a migration to MVI/Compose. iOS v3.0.0 transitioned to Swift 6.

## Key Features

- **OpenPGP Encryption:** End-to-end encryption using standard GPG keys. The server never sees plaintext passwords.
- **Secret Key Authentication:** Users authenticate using their private GPG key, acting as a second factor.
- **Granular Sharing:** Role-Based Access Control (RBAC), groups, and shared folders.
- **Self-Hosted CE:** Full data ownership with an AGPLv3 licensed server.
- **Cross-Platform:** Browser extensions (Firefox/Chrome), Android, iOS, and a CLI.
- **Open API:** Allows custom integrations and automation scripts.

## System Requirements

**Minimum Server Specifications (CE & Pro):**

- **CPU:** 2 cores
- **RAM:** 2GB
- **Storage:** 20GB
- **Network:** 10mbps internet access

**Supported Platforms:**

- **OS:** Stable version of a major Linux distribution (Debian, Ubuntu, CentOS, etc.)
- **Installation Methods:** Docker, Kubernetes, native packages for Ubuntu, Debian, Redhat, Raspberry Pi, RockyLinux, AlmaLinux, OracleLinux, openSUSE, SUSE Linux Enterprise Server.
- **Cloud Providers:** AWS, DigitalOcean.

## Real-World Usage

Deploying a fully FOSS password manager for a development team using Docker.

1. Deploy Passbolt CE using Docker Compose.
2. Run the healthcheck and create the first admin user.
3. Users install the Passbolt browser extension and generate their GPG keypair during account setup.
4. Administrators create groups matching team structures (e.g., “DevOps”, “Backend”) and share folder access accordingly.
5. Credentials shared within groups are re-encrypted client-side with the recipients’ public keys before uploading to the server.

> **Known Issue:** Safari users may experience sessions being destroyed when fetching avatar images, though fixes are ongoing. Firefox users previously had issues uploading TOTP QR codes due to CSP, which is resolved in v5.13.0.

## Download & Installation

**Official Website:** [Passbolt](https://www.passbolt.com/)
**Documentation:** [Passbolt Help](https://www.passbolt.com/help/)
**Downloads:** [Server Installation Guide](https://www.passbolt.com/ce/docker/docker-compose-ce.yaml) · [Browser Extension](https://www.passbolt.com/browser-extension) · [Mobile Apps](https://www.passbolt.com/mobile)
**Source Code:** [Passbolt API](https://github.com/passbolt/passbolt_api) · [Passbolt Browser Extension](https://github.com/passbolt/passbolt_browser_extension) · [Passbolt Android](https://github.com/passbolt/mobile-passbolt-android) · [Passbolt iOS](https://github.com/passbolt/mobile-passbolt-ios)

**Quick Start (Docker CE):**

1. Download the Docker Compose file and its SHA512 checksum:

```plain
curl -LO "https://download.passbolt.com/ce/docker/docker-compose-ce.yaml"
curl -LO "https://github.com/passbolt/passbolt_docker/releases/latest/download/docker-compose-ce-SHA512SUM.txt"

```

2. Verify the integrity of the downloaded file:

```plain
sha512sum -c docker-compose-ce-SHA512SUM.txt && echo "Checksum OK" || (echo "Bad checksum. Aborting" && rm -f docker-compose-ce.yaml)

```

3. Start the Passbolt stack:

```plain
docker compose -f docker-compose-ce.yaml up -d

```

4. Access the web interface to complete the configuration and create the first admin user.

## Open Source Alternatives

- [**Vaultwarden**](https://getfoss.org/security-privacy/vaultwarden-self-hosted-bitwarden-server-rust/)**:** A lightweight, fully FOSS Rust implementation of the Bitwarden API. Easier client onboarding (no GPG key management) but lacks native OpenPGP integration and uses a different encryption model (AES-256 via a master password).
- [**KeePassXC**](https://getfoss.org/password-managers/keepassxc-cross-platform-password-manager/)**:** A local-first password manager. No server component. Sync relies on file synchronization tools. Zero network attack surface but poor real-time team collaboration.

## Who Should Use It?

Teams and organizations that require strict end-to-end encryption based on standard OpenPGP. Self-hosters who want a fully AGPLv3 stack without source-available server caveats. Users comfortable managing GPG keypairs.

## Limitations

Passbolt relies heavily on browser extensions; there is no standalone desktop application. User onboarding requires generating and trusting GPG keys, which introduces a steeper learning curve compared to standard master-password managers. Staying on the FOSS CE edition means missing out on administrative conveniences like SSO, LDAP provisioning, and activity auditing.

## Final Assessment

Passbolt provides the most transparent FOSS password management stack for teams. It excels in cryptographic veracity via OpenPGP and offers an unrestricted AGPLv3 server for unlimited users. It falls short on user experience due to GPG complexity and withholds key enterprise administration features behind a proprietary paywall. For teams that prioritize standard cryptography and a libre server over SSO convenience, Passbolt CE is the definitive choice.
