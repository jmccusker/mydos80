# MyDOS/80

MyDOS/80 is a new, experimental disk operating system for the TRS-80
Model III and for the Model 4 running in its Model III mode, designed
and developed by Jim McCusker. This repository is its
public home: evaluation releases, documentation, and the place to report
what you find.

> **The MyDOS/80 source code is not published here** and is not licensed
> under an open-source license. This repository distributes evaluation disk
> images, the HWLINK hardware tester, and documentation.

## New: the HWLINK Tester for the Model 4P (experimental)

**[HWLINK Tester 0.1.0-E01](https://github.com/jmccusker/mydos80/releases/tag/hwtest-v0.1.0-e01)**
is a Linux program (Fedora or Ubuntu, Python 3.10+, no development tools
needed) that checks a **real TRS-80 Model 4P** over a serial cable and
writes one plain-text report: the ROM serial loader and serial chip, CPU,
memory, video, keyboard and, **read only**, the floppy controller and
drives. It never writes to a disk and installs nothing on the 4P.

> **EXPERIMENTAL: not yet validated on a real Model 4P.** It has passed its
> tests against a simulated 4P on Ubuntu and Fedora; the first real runs
> are what it is for. Failed or unexpected results are just as valuable as
> successes: please report them.

Installation, use and reporting: **[hwtest/README.md](hwtest/README.md)**.

**Evaluation license, in short** ([NOTICE.md](NOTICE.md) is the full
text): you may download and run MyDOS/80, keep personal backup copies
and make modified personal copies of its disk images for personal,
non-commercial evaluation, education and testing, in emulators or on
real hardware. Reviews, screenshots, videos and test results are
welcome. You may not redistribute it, in original or modified form,
set up mirrors, or distribute it commercially without my prior written
permission: please link to this repository's Releases page instead.

## Status

MyDOS/80 is in development. **The current evaluation release is 0.10
RC4, EMULATOR-VALIDATED ONLY** (see its release page). It corrects the
filesystem defects for which 0.10 RC3 was withdrawn on 2026-11-22.
Evaluation releases are for emulators: they are tested in the trs80gp
emulator, not qualified on real hardware. Commands, screens and the
native disk format may still change.

> **Not validated on real hardware.** RC4 has not been tested on a real
> TRS-80, on real one- or two-sided floppy drives, or with its disks'
> track layout ("B") on real media. Its two-sided system disk and that
> track layout are experimental on real hardware.

| | |
|---|---|
| Current release | 0.10 RC4 (EMULATOR-VALIDATED ONLY); 0.10 RC3 was withdrawn on 2026-11-22 |
| Tested in | 16 scripted sessions in a patched build of trs80gp 2.5.8 on Linux; 8 sessions through the Windows launcher with the public trs80gp 2.5.8 on Windows 11 |
| Machines | Model III; Model 4 in Model III mode (the Model 4P is not part of RC4) |
| Real hardware | not qualified: no real TRS-80, floppy drive or media (see below) |

## What it does

- **Its own filesystem, MNFS**, with directories, longer names, file dates
  and writes designed to leave the disk consistent if interrupted.
- **Traditional TRS-80 disks**: reads LDOS 5, TRSDOS 1.3, TRSDOS 6 /
  LS-DOS 6, NEWDOS/80 and MultiDOS disks, and lets you change disks without
  restarting. Writing is limited and experimental: files on LDOS 5 and
  TRSDOS 1.3 disks can be written in drives 0 and 1 only; TRSDOS 6,
  NEWDOS/80 and MultiDOS disks are read only.
- **A modular system**: a small essential core, with optional packages
  (the clock, time zones, screen savers, the MED editor, native tools and
  more) installed from a module disk with `SETUP` or the full-screen
  `SETUP MENU`, which also installs what each package needs.
- **A shell** with HELP, DIR, CD, TYPE, COPY, MKDIR, REN, DEL, ASSIGN,
  command history and name completion; up to eight drives.
- **Programs**: native MyDOS/80 programs and simple traditional /CMD
  programs.

## The evaluation release

Each release is a ZIP file on this repository's **Releases** page.
0.10 RC4 holds:

- fifteen disk images (DMK): a two-sided system disk with everything
  installed, a two-sided system disk with the standard packages and a
  one-sided one, a module disk for each, native sample and practice
  disks, sample disks in five traditional formats (made with my own
  tools, holding only text files), and three example disks that show how
  errors are reported and how a damaged system disk is recovered;
- two books (searchable PDF): the *User's Manual* and *Under the Hood*
  (the system architecture), edition 1.10;
- a Quick Start, guided tests, release notes, known limitations, a
  feedback form, and a Windows launcher that starts trs80gp on fresh,
  checked copies of the disks.

Its boot screen reads "RC4 CANDIDATE C2": the disk images are exactly
the release candidate that passed the emulator sessions, unchanged
(the release notes explain).

No ROM images and no other publishers' DOS files are included.

### Getting started

1. Download the release ZIP from **Releases** and check its SHA-256
   against the one on the release page.
2. Get trs80gp 2.5.8 from its author's site, http://48k.ca/trs80gp.html
   (one download with the Windows, Linux and macOS builds).
3. Unpack the release and read `QUICK_START.txt`: which disk goes in
   which drive, how to start it on Windows (the launcher) or on Linux
   and macOS, and the first commands. `GUIDED_TESTS.txt` then walks
   through the system with the expected result of every step, and
   `KNOWN_LIMITATIONS.txt` says what does not work yet.

The public trs80gp 2.5.8 has a known floppy-timing fault that can show
as an occasional "Disk read error" (`KNOWN_LIMITATIONS.txt`). The macOS
steps follow trs80gp's own documentation and were not run (I had no Mac
to test on). The Guided Tour book of RC3 has not been redone for RC4.

## Please read: experimental software

> **Never perform write, delete, format or other destructive experiments
> using irreplaceable DMK, HFE, VHD or physical media.** Use disposable
> copies. Writing is experimental in this release, and every disk image in
> it is expendable.

trs80gp writes every change straight back into the image file you give it,
so always start it on copies (the Windows launcher makes them for you).

> **Windows: trs80gp may occasionally crash when it closes.** In one of
> eight scripted Windows sessions the public trs80gp 2.5.8 closed with a
> Windows error at the very end, after everything had been saved;
> repeated three times, it did not recur, and the disks were identical in
> every run. It is a fault of the emulator on Windows, not of MyDOS/80.
> If trs80gp closes with an error, start it again on the same run folder
> (`Start-MyDOS80.ps1 -RunFolder <folder>`) and check your files with
> DIR. Keep using disposable copies of the disk images.

### Real hardware

MyDOS/80's earlier hardware test disks have run on real Model III
machines, but **this release has not been qualified on real hardware**:
not on a real TRS-80, not on real one- or two-sided floppy drives and not
on real media. Its two-sided system disk and its disks' track layout are
experimental on real hardware.
If you try it on a real TRS-80, use only disposable floppies (writing an
image erases the whole disk), never an original or irreplaceable disk,
and please tell me exactly what happened, especially if it failed.

## Reporting problems and results

Every report helps, and **a failure or an unexpected result is just as
useful to me as a success**.

Open an issue in this repository and choose the matching form: a bug
report, a feature request, a hardware compatibility report, or an
**HWLINK tester report** (attach the tester's `REPORT.txt`). For a
question, open a blank issue. [REPORTING.md](REPORTING.md) says what makes a
report easy to reproduce.

Please do not attach or upload ROM images or copies of commercial DOS
disks, even when they are part of a problem. Describe them instead (name,
version, where they came from).

## About

MyDOS/80 is designed and developed by Jim McCusker using AI-assisted
software development tools. Copyright © 2026 Jim McCusker. All rights
reserved; see [NOTICE.md](NOTICE.md). The software is provided "as
is", without warranty of any kind, to the extent permitted by applicable
law.
