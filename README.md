# MyDOS/80

MyDOS/80 is a new, experimental disk operating system that I am writing
for the TRS-80 Model III and for the Model 4 and Model 4P running in their
Model III mode. This repository is its public home: evaluation releases,
documentation, and the place to report what you find.

> **The MyDOS/80 source code is not published here** and is not licensed
> under an open-source license. This repository distributes evaluation disk
> images and documentation only.

**Evaluation permission, in short** ([NOTICE.md](NOTICE.md) is the full
text): you may download and run the release, and make modified copies of
its disk images, for personal, non-commercial evaluation and testing. You
may not redistribute it publicly, distribute it commercially or publish
modified versions without my prior written permission: please link to this
repository's Releases page instead.

## Status

MyDOS/80 is in development. The current release is an **evaluation
release for emulators**: it has been tested in the trs80gp emulator, not
qualified on real hardware. Commands, screens and the native disk format
may still change.

| | |
|---|---|
| Current release | **0.10 RC3**, the first public evaluation release |
| Tested in | trs80gp 2.5.8 (Linux and Windows builds) |
| Machines | Model III; Model 4 and Model 4P in Model III mode |
| Real hardware | not yet qualified (see below) |

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

Each release is a ZIP file on this repository's **Releases** page. It holds:

- twelve disk images (DMK): a system disk with everything installed, a
  minimal system disk and a module disk for building your own system,
  native sample and practice disks, sample disks in five traditional
  formats (made with my own tools, holding only text files), and two
  example disks that show how errors are reported;
- three books (searchable PDF): *MyDOS/80 - A Guided Tour*, the
  *User's Manual* and *Under the Hood* (the system architecture);
- a Quick Start, guided tests, known limitations, a feedback form, and a
  Windows launcher that starts trs80gp on fresh copies of the disks.

No ROM images and no other publishers' DOS files are included.

### Getting started

1. Download the release ZIP from **Releases** and check its SHA-256
   against the one on the release page.
2. Get trs80gp 2.5.8 from its author's site, http://48k.ca/trs80gp.html
   (one download with the Windows, Linux and macOS builds).
3. Unpack the release and open `docs/MyDOS80-Guided-Tour.pdf`. **Start
   with the Guided Tour**: its first chapter sets up trs80gp on Windows,
   Linux or macOS, and the rest walks through the whole system one short
   exercise at a time.

The Windows and Linux steps were run for this release; the macOS steps
follow trs80gp's own documentation and were not run (I had no Mac to test
on). A Model 4P also needs your own TRSDOS 6.2.1 system disk, from which
the 4P loads its Model III ROM image; the tour's Exercise 15.2 explains.

## Please read: experimental software

> **Never perform write, delete, format or other destructive experiments
> using irreplaceable DMK, HFE, VHD or physical media.** Use disposable
> copies. Writing is experimental in this release, and every disk image in
> it is expendable.

trs80gp writes every change straight back into the image file you give it,
so always start it on copies (the Windows launcher makes them for you).

### Real hardware

MyDOS/80's earlier hardware test disks have run on real Model III
machines, but **this release has not been qualified on real hardware**.
If you try it on a real TRS-80, use only disposable floppies (writing an
image erases the whole disk), never an original or irreplaceable disk,
and please tell me exactly what happened, especially if it failed.

## Reporting problems and results

Every report helps, and **a failure or an unexpected result is just as
useful to me as a success**.

Open an issue in this repository and choose the matching form: a bug
report, a feature request, or a hardware compatibility report. For a
question, open a blank issue. [REPORTING.md](REPORTING.md) says what makes a
report easy to reproduce.

Please do not attach or upload ROM images or copies of commercial DOS
disks, even when they are part of a problem. Describe them instead (name,
version, where they came from).

## About

MyDOS/80 is written by Jim McCusker. Copyright © 2026 Jim McCusker. All
rights reserved; see [NOTICE.md](NOTICE.md). The software is provided "as
is", without warranty of any kind, to the extent permitted by applicable
law.
