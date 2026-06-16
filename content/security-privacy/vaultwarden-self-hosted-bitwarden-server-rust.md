---
title: 'Vaultwarden: Self-Hosted Bitwarden Server (Rust)'
description: Vaultwarden is a FOSS alternative implementation of the Bitwarden server API written in Rust. It provides a lightweight, self-hosted backend for official Bitwarden clients.
date: 2026-06-16T12:34:00+03:00
draft: false
image: /images/Vaultwarden.webp
categories:
  - security-privacy
tags:
  - Passwords
  - Security
  - Self-hosted
  - Privacy
aliases: []
---

Vaultwarden is an alternative implementation of the Bitwarden Client API written in Rust. It solves the problem of self-hosting a Bitwarden-compatible server without the massive resource overhead of the official C#/MSSQL stack. Licensed under AGPL-3.0, it provides a fully FOSS backend that integrates seamlessly with official Bitwarden clients (Desktop, Mobile, Browser, CLI). For administrators who require control over their credential infrastructure but reject the proprietary licensing and heavy requirements of the official server, Vaultwarden is the standard. The official website is [Vaultwarden](https://www.vaultwarden.net/).

## Why It Matters (The FOSS Angle)

The official Bitwarden server is source-available, not FOSS, and carries telemetry and heavy system dependencies. Vaultwarden provides a fully libre AGPL-3.0 alternative that implements zero telemetry by design. It proves that a secure, feature-rich password manager backend does not require gigabytes of RAM.

_Recent updates (1.35.8 and 1.36.0) highlight active security maintenance and feature parity:_

- **Security Fixes:** Version 1.36.0 addresses critical vulnerabilities including SSO Login CSRF, User/Organization Enumeration, SSO existing-user binding issues, and SSRF via the Icon Endpoint.
- **Archiving Support:** 1.36.0 introduces item archiving, matching upstream client functionality.
- **SSO Refinements:** Fixes for hardcoded SSO identifiers and fallback to UserInfo preferred_username.
- **Dynamic SQLite:** Added support for linking SQLite dynamically, offering flexibility in build environments.
- **Independent Governance:** While a Vaultwarden maintainer is employed by Bitwarden Inc., the project operates independently, focusing specifically on the self-hosting community rather than enterprise SaaS metrics.

## Key Features

- **API Compatibility:** Works directly with official Bitwarden clients. No custom forks required.
- **Organizations:** Supports collections, sharing, groups, event logs, directory connector, and admin password reset.
- **Multi-Factor Authentication:** Compatible with Authenticator, Email, FIDO2 WebAuthn, YubiKey, and Duo.
- **Emergency Access:** Designate trusted contacts for vault access in case of emergency.
- **Vaultwarden Admin Page:** A built-in backend interface for instance management and diagnostics.
- **Lightweight Backend:** Written in Rust. Uses SQLite by default (MySQL/PostgreSQL supported), avoiding the MSSQL dependency entirely.

## System Requirements

_Official CPU and RAM specifications are not published. Based on the Rust/SQLite architecture, typical requirements are significantly lower than the official Bitwarden server._

**Estimated Requirements:**

- **CPU:** 1 vCPU
- **RAM:** 512MB
- **Storage:** 1GB+ (dependent on attachment storage)
- **Software:** Docker/Podman, Reverse Proxy (Nginx/Caddy) with TLS.

## Real-World Usage

Deploying a fully FOSS, self-hosted password manager for a small team using standard Bitwarden clients.

1. Deploy Vaultwarden via Docker Compose with a persistent volume for `/data/`.
2. Place the container behind a reverse proxy (Caddy or Nginx). **TLS is mandatory.** The Web Vault requires a secure context for the Web Crypto API; it will fail over plain HTTP.
3. Configure the `DOMAIN` environment variable to match the public HTTPS URL.
4. Disable public sign-ups via the Admin Page or environment variables (`SIGNUPS_ALLOWED=false`).
5. Users install the official Bitwarden browser extension or mobile app, navigate to Settings, and point the “Self-hosted Environment” URL to the Vaultwarden domain.

## Download & Installation

**Official Website:** [Vaultwarden](https://www.vaultwarden.net/)
**Documentation:** [Vaultwarden Wiki](https://github.com/dani-garcia/vaultwarden/wiki)
**Downloads:** [GitHub Container Registry](https://github.com/dani-garcia/vaultwarden/pkgs/container/vaultwarden) · [Docker Hub](https://hub.docker.com/r/vaultwarden/server) · [Quay.io](https://quay.io/repository/vaultwarden/server)
**Source Code:** [GitHub Repository](https://github.com/dani-garcia/vaultwarden) · [Website Source](https://github.com/Y0ngg4n/vaultwarden-net-frontpage)

**Quick Start (Docker Compose):**
Create a `compose.yaml` file:

```plain
services:
  vaultwarden:
    image: vaultwarden/server:latest
    container_name: vaultwarden
    restart: unless-stopped
    environment:
      DOMAIN: "https://vw.domain.tld"
    volumes:
      - ./vw-data/:/data/
    ports:
      - 127.0.0.1:8000:80

```

Run `docker compose up -d`. Configure a reverse proxy to proxy HTTPS traffic to `127.0.0.1:8000`.

## Open Source Alternatives

- [**Bitwarden Official Server**](https://github.com/bitwarden/server)**:** The upstream C#/MSSQL implementation. Resource-heavy (6GB+ RAM minimum). Source-available license, not fully FOSS.
- **Passbolt:** A fully open-source (AGPL) password manager designed for teams. Uses OpenPGP for authentication and encryption, integrated primarily via browser extension.
- [**KeePassXC**](https://getfoss.org/password-managers/keepassxc-cross-platform-password-manager/)**:** A local-first password manager. No server component. Sync relies on file synchronization tools. Zero network attack surface.

## Who Should Use It?

Self-hosters, families, and small organizations needing cross-device credential synchronization without the overhead of the official Bitwarden stack. Users who require a fully AGPL-licensed stack and zero telemetry. Administrators comfortable configuring reverse proxies and TLS.

## Limitations

Vaultwarden mandates HTTPS; it will not function over HTTP due to browser Web Crypto API restrictions. Configuration requires a working knowledge of reverse proxies and TLS certificate management. The project is not affiliated with Bitwarden Inc.; bugs must be reported to the Vaultwarden issue tracker, not official Bitwarden support channels. Large enterprise features reliant on the official Bitwarden stack architecture may lag or remain unimplemented.

## Final Assessment

Vaultwarden is the definitive self-hosted Bitwarden backend. It excels in resource efficiency, FOSS licensing, and compatibility with the official Bitwarden client ecosystem. It falls short in that it requires mandatory TLS setup and lacks official vendor support. For anyone seeking a libre, lightweight, and secure credential synchronization server, Vaultwarden is the operational standard.
