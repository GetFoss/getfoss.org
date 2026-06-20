---
title: 'Denemo: Open Source Music Notation Editor with LilyPond Integration'
description: Denemo is a free and open source music notation editor that provides a graphical front-end for the LilyPond typesetter, supporting MIDI input, playback, and MusicXML interchange.
date: 2026-06-20T06:33:00+03:00
draft: false
image: /images/denemo.webp
categories:
  - audio-video
tags:
  - Audio
  - Music
  - Linux
  - Utilities
aliases: []
---

Denemo is a free and open source music notation editor acting as a graphical front-end for the LilyPond typesetter. LilyPond uses a text-based input format; Denemo provides a WYSIWYM interface for MIDI, keyboard, or mouse entry while delegating the final rendering to LilyPond. It is a GNU project licensed under the GPL.

## Why It Matters (The FOSS Angle)

Music notation is dominated by proprietary tools (Sibelius, Finale) that lock your work into closed formats and subscriptions. Denemo saves scores in an open XML-based format, delegates typesetting to the independent LilyPond engine, and exports to MusicXML. You maintain ownership and access to your work without vendor dependence.

Recent releases show active development. Version 2.6 (March 2022) adds a “Pitches First” entry method decoupling pitch from rhythm, and conditional directives for granular instrument control. Version 2.5 (March 2021) added multi-movement MusicXML export and LilyPond 2.20 support.

## Key Features

- **Pitches First Note Entry (2.6):** Visualize notes on a MIDI track and insert rhythms separately, re-synchronizing per bar.
- **Per-Object LilyPond Editing (2.6):** Edit underlying LilyPond code for any object (notes, chords, clefs) to create specialized editions.
- **Conditional Directives (2.6):** Attach behavior to specific objects (e.g., changing a clef only for certain instruments) without altering the global score.
- **MusicXML Export (2.5):** Export multi-movement scores to the standard interchange format.
- **Score and Parts in One PDF (2.6):** Generate full scores and individual parts in a single PDF output.
- **Audio Fragment Recording (2.6):** Attach recorded audio fragments to a score during composition.
- **LilyPond Agility:** Supports LilyPond 2.20 and includes fixes for 2.22 compatibility.

## System Requirements

The project does not publish official hardware requirements. Any system running a modern desktop environment and LilyPond should suffice.

- **Windows:** First launch hangs for an extended period generating fonts.
- **Mac:** The 2.2 app bundle fails on macOS Catalina; MacPorts is required.

## Real-World Usage

A typical workflow involves setting up the score structure in the Score Properties Editor, entering pitches via MIDI using the Pitches First method, applying conditional directives for transposing instruments, and exporting a combined Score and Parts PDF.

- **Windows portable use:** The 2.6 no-install version runs from a USB stick. Extract the ZIP and run `Denemo.bat`. Extracting directly to a USB stick takes 20 minutes; extract to a hard drive first.
- **Windows font bug:** If the treble clef appears as `È`, manually install fonts from `C:\Program Files\Denemo\usr\share\fonts\truetype\denemo`.
- **External tools:** For graphical title pages or ornaments, install Inkscape and set its path in Preferences → Externals.
- **MIDI:** Turn on MIDI keyboards before launching Denemo.

## Download & Installation

- **Official Website:** [denemo.org](https://www.denemo.org/)
- **Documentation:** [First Steps](https://www.denemo.org/first-steps/) | [FAQ](https://www.denemo.org/faq/) | [Tutorials](https://www.denemo.org/tutorials/)
- **Downloads:** [Downloads Page](https://www.denemo.org/downloads-page/)
- **Source Code:** [FTP Mirror](https://ftp.gnu.org/gnu/denemo/) | [Git Repository](https://cgit.git.savannah.gnu.org/cgit/denemo.git)
- **Bug Tracker:** [Denemo Bugtracker](https://www.denemo.org/bugtracker/)

**Linux:** Distros ship outdated packages. Prefer:

- **AppImage:** `chmod a+x denemo_2.5.1.AppImage && ./denemo_2.5.1.AppImage` (no root needed).
- **Flatpak:** Available via Flathub.
- **Binary tarball:** Unpack `.tar.xz` and run `Launch_Denemo.sh`.
- **Debian/Ubuntu:** `dpkg -i *denemo*2.6*_amd64.deb *denemo*2.6*_all.deb`.
- **Git:** `git clone git://git.savannah.gnu.org/denemo.git`

**Windows:** Use the 2.6 no-install version. The installer is stuck at version 2.2.
**Mac:** 64-bit app bundle (version 2.2.0) for OS X. Use MacPorts for Catalina.

## Open Source Alternatives

- [**MuseScore**](https://getfoss.org/audio-video/musescore-studio-open-source-music-notation-and-score-engraving/)**:** Self-contained notation editor with its own engraver. More polished UI, but lacks LilyPond’s fine-grained typesetting control.
- [**LilyPond (direct)**](https://getfoss.org/audio-video/lilypond-text-based-music-engraving-program/)**:** Text-based input. Maximum control and scriptability, but steep learning curve and no real-time visual feedback.
- [**Canorus**](https://sourceforge.net/projects/canorus/)**:** Less actively developed open source notation program.

## Who Should Use It?

- Composers wanting LilyPond output quality without writing text input.
- Users migrating from Sibelius or Finale to escape vendor lock-in.
- Educators producing clean scores and parts for ensembles.

## Limitations

- UI is functional but dated compared to modern alternatives.
- Distribution packages lag significantly behind upstream.
- Windows installer is stuck at 2.2; no-install version requires patience for first-run font generation.
- Mac support is rough; current macOS requires MacPorts.
- Documentation is scattered across multiple pages without a single comprehensive manual.

## Final Assessment

Denemo is the GUI for LilyPond. It delivers excellent engraving quality and data freedom without text-based input. The tradeoff is UI polish and platform consistency. If you need LilyPond’s precision with a graphical interface, use Denemo. If you want a slick, self-contained UI, use MuseScore.
