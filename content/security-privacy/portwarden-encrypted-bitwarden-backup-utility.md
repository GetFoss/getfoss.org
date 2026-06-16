---
title: 'Portwarden: Encrypted Bitwarden Backup Utility'
description: Portwarden is a CLI tool that creates AES-encrypted backups of Bitwarden vaults, including attachments, and restores them to empty accounts.
date: 2026-06-16T14:11:00+03:00
draft: false
image: /images/portwarden.webp
categories:
  - security-privacy
tags:
  - Passwords
  - Backup
  - Utilities
  - Security
aliases: []
---

Portwarden is a command-line utility that creates AES-encrypted backups of Bitwarden vaults, including file attachments. It solves a specific operational gap: the official Bitwarden export feature outputs unencrypted CSV or JSON files and completely ignores attachments. Portwarden wraps the Bitwarden CLI, pulls vault items and attachments into a temporary directory, encrypts the archive with a user-supplied passphrase, and securely deletes the temporary data. The project is open-source, written in Golang. The official repository is [Portwarden](https://github.com/vwxyzjn/portwarden).

## Why It Matters (The FOSS Angle)

Relying on a cloud-synced password manager without local, encrypted backups creates a single point of failure. Bitwarden’s native export mechanism leaves sensitive data in plaintext on the disk. Portwarden addresses this by ensuring the backup file is useless without the passphrase, using standard AES encryption.

The project highlights a common FOSS dynamic: when an upstream project ignores a community request (in this case, encrypted exports with attachments), the community builds a tool to fill the gap. However, maintenance is critical. The last release was in March 2020. The Bitwarden CLI has undergone significant changes since then, meaning Portwarden may break with newer `bw` CLI versions.

## Key Features

- **AES-Encrypted Archives:** Backups are saved in the `.portwarden` format, encrypted with a passphrase.
- **Attachment Inclusion:** Downloads and packages all file attachments associated with vault items.
- **Self-Hosted Support:** Compatible with custom Bitwarden or Vaultwarden server URLs via the `bw config server` command.
- **Restore Functionality:** Supports restoring backups to an empty Bitwarden account, including attachments.
- **Cross-Platform:** Pre-compiled binaries available for Linux, macOS, Windows, FreeBSD, NetBSD, and OpenBSD (including ARM and MIPS architectures).

## System Requirements

**Dependencies:**

- Bitwarden CLI (`bw` or `bw.exe`) must be installed and accessible in the system `PATH`.

**Estimated Requirements (Build/Development Environment):**

- **Software:** Golang, Docker, `dep` (Go dependency manager).
- **Ports:** 8000, 8081, 5000, 6379 must be unused.
- **Environment:** A `Salt` environment variable (30 characters) must be set for encryption salt.

## Real-World Usage

Automating nightly encrypted backups of a Vaultwarden instance to local storage.

1. Install the Bitwarden CLI and Portwarden binary.
2. If using a self-hosted instance, configure the CLI:

```plain
bw config server https://vw.domain.tld
```

3. Execute the backup:

```plain
portwarden --passphrase YOUR_STRONG_PASSPHRASE --filename vault_backup_$(date +%F).portwarden encrypt
```

4. Move the generated `.portwarden` file to offline or cold storage.

**Critical Warning:** The restore feature is explicitly marked as experimental. It does not handle restoration conflicts. Always restore to a spare, empty account. The tool includes a check to prevent restoring to an account that already contains data, but data loss is a real risk if workarounds are applied.

## Download & Installation

- **Official Website:** [Portwarden GitHub](https://github.com/vwxyzjn/portwarden)
- **Documentation:** [Portwarden README](https://github.com/vwxyzjn/portwarden#readme)
- **Downloads:** [Portwarden Releases](https://github.com/vwxyzjn/portwarden/releases) · [Bitwarden CLI Releases](https://github.com/bitwarden/cli/releases)
- **Source Code:** [Portwarden Repository](https://github.com/vwxyzjn/portwarden)

**Quick Start (Linux amd64):**

1. Download the Bitwarden CLI and Portwarden binaries:

```plain
# Download Portwarden (adjust version if needed)
curl -LO https://github.com/vwxyzjn/portwarden/releases/latest/download/portwarden_linux_amd64
chmod +x portwarden_linux_amd64
sudo mv portwarden_linux_amd64 /usr/local/bin/portwarden
```

2. Ensure the `bw` CLI is installed and logged in.
3. Create an encrypted backup:

```plain
portwarden --passphrase "SecretPassphrase123!" --filename backup.portwarden encrypt
```

4. Verify decryption without restoring:

```plain
portwarden --passphrase "SecretPassphrase123!" --filename backup.portwarden decrypt
```

## Open Source Alternatives

- **Bitwarden Official Export:** Built directly into the web vault and CLI. Outputs unencrypted JSON or CSV. Does not support attachments. Suitable only for migration, not secure offline storage. ([Docs](https://bitwarden.com/help/export-your-data/))
- **BorgBackup / Restic:** General-purpose deduplicating backup tools. Can back up the entire Vaultwarden server container or data volume, but lack API-level understanding of Bitwarden items. ([BorgBackup](https://www.borgbackup.org/) / [Restic](https://restic.net/))

## Who Should Use It?

Sysadmins and power users who need offline, encrypted, portable backups of their Bitwarden vaults complete with attachments. Users comfortable with CLI tools and aware of the risks associated with using unmaintained software.

## Limitations

The project has not received an update since March 2020. Because it relies heavily on the output and behavior of the official Bitwarden CLI (`bw`), compatibility with modern versions of the CLI is not guaranteed. The restore function is experimental and strictly limited to empty accounts, making disaster recovery a rigid process. Development requires spinning up multiple Docker containers and managing specific environment variables, which is cumbersome for a utility tool.

## Final Assessment

Portwarden provides a necessary feature that Bitwarden lacks: encrypted, attachment-inclusive backups. It excels at creating secure, portable archives of vault data. It falls short on long-term maintenance and reliable disaster recovery. Treat it as a one-way backup tool for cold storage, test it rigorously with your specific Bitwarden CLI version, and do not rely on its restore function as your primary disaster recovery strategy.
