# MyDOS/80 HWLINK Tester (Linux, TRS-80 Model 4P)

The HWLINK Tester is a Linux program that checks a real **TRS-80 Model
4P** over a serial cable and writes one plain-text report you can send
back, with its own findings: what looks wrong or unusual, and why. It is
MyDOS/80's way of collecting real-hardware evidence from 4P owners.
Designed and developed by Jim McCusker.

| Release | Download | SHA-256 |
|---|---|---|
| **0.2.0-E02** (current) | [hwtest-v0.2.0-e02](https://github.com/jmccusker/mydos80/releases/tag/hwtest-v0.2.0-e02): `mydos80-hwtest-0.2.0-E02.tar.gz` | `2e8752e31b06cdcf333e8639b34316c37cc25096e2508ea9c6b94c5c9ca9da16` |
| 0.1.0-E01 (still available, unchanged) | [hwtest-v0.1.0-e01](https://github.com/jmccusker/mydos80/releases/tag/hwtest-v0.1.0-e01): `mydos80-hwtest-0.1.0-E01.tar.gz` | `8cc92f455d47de644b7f00cadf9c278482cbb2c43a3224d98d3d058224eaf803` |

This guide is written for E02. Sections 1 to 8 apply to E01 as well
(read `0.1.0-E01` for `0.2.0-E02`); what only E02 does is marked **E02**.

> **EXPERIMENTAL. NOT YET VALIDATED ON A REAL MODEL 4P.** Both releases
> have passed their tests against a *simulated* 4P on Ubuntu 22.04,
> Ubuntu 24.04 and Fedora 45, but neither has yet run with a real serial
> adapter and a real 4P. The first real runs are exactly what they are
> for. If the tester stops with a message, or a test fails, that is a
> useful result: please report it.

> **READ ONLY.** The tester never writes to, formats or creates a floppy
> disk: the release contains no program that could. It loads small test
> programs into the 4P's memory through the 4P's own ROM serial loader
> (nothing is installed on the 4P; RESET or power-off removes them), and
> it never uses a hard disk, a FreHD or other storage. On the PC it needs
> no administrator rights, installs nothing system-wide and never runs
> `sudo` itself.

Contents: [1 What you need](#1-what-you-need) ·
[2 Download and verify](#2-download-and-verify) ·
[3 Unpack, and the optional installer](#3-unpack-and-the-optional-installer) ·
[4 Check the computer: preflight](#4-check-the-computer-preflight) ·
[5 The self-test (no hardware)](#5-the-self-test-no-hardware) ·
[6 Serial ports and permissions](#6-serial-ports-and-permissions) ·
[7 Connecting the 4P](#7-connecting-the-4p) ·
[8 Running the 4P test](#8-running-the-4p-test) ·
[9 Findings, follow-ups and the trs80gp comparison (E02)](#9-findings-follow-ups-and-the-trs80gp-comparison-e02) ·
[10 If the connection is lost (E02)](#10-if-the-connection-is-lost-e02) ·
[11 Your report: finding and sending it](#11-your-report-finding-and-sending-it) ·
[12 More commands and options](#12-more-commands-and-options) ·
[13 Moving from E01 to E02](#13-moving-from-e01-to-e02) ·
[14 Troubleshooting](#14-troubleshooting) ·
[15 Uninstalling](#15-uninstalling) ·
[16 Status, performance and limitations](#16-status-performance-and-limitations)

## 1. What you need

| | |
|---|---|
| Linux | **Fedora** or **Ubuntu** (other distributions may work, untested). Tested: Ubuntu 22.04.5, Ubuntu 24.04, Fedora 45 |
| Python | the system's own `python3`, version **3.10 or later**. Fedora and Ubuntu desktops have it already (Ubuntu 22.04: 3.10; 24.04: 3.12; Fedora 45: 3.15). Check with `python3 -V` |
| Not needed | **no Git, no pip, no virtual environment, no compiler, no assembler**, no other packages. The tester uses only Python's standard library, and every Z80 program is prebuilt in the package |
| Serial | an **RS-232** serial port or USB-to-RS-232 adapter (FTDI, Prolific, CH340 ...). **Not** a "USB-TTL" board or cable with 3.3 V or 5 V logic: that must never be connected to the 4P |
| Cable | a **null-modem** cable (or a straight cable plus a null-modem adapter) to the 4P's DB-25 RS-232 port |
| The 4P | a TRS-80 Model 4P. Its floppy drives may stay empty; disks are optional and only read |

If `python3` is missing (unusual on a desktop): Fedora `sudo dnf install
python3`, Ubuntu `sudo apt install python3`. The tester's launcher says
so too.

## 2. Download and verify

From the [E02 release page](https://github.com/jmccusker/mydos80/releases/tag/hwtest-v0.2.0-e02)
download both files into one folder, for example `~/Downloads`:

- `mydos80-hwtest-0.2.0-E02.tar.gz`
- `mydos80-hwtest-0.2.0-E02.tar.gz.sha256`

Or in a terminal:

    cd ~/Downloads
    curl -LO https://github.com/jmccusker/mydos80/releases/download/hwtest-v0.2.0-e02/mydos80-hwtest-0.2.0-E02.tar.gz
    curl -LO https://github.com/jmccusker/mydos80/releases/download/hwtest-v0.2.0-e02/mydos80-hwtest-0.2.0-E02.tar.gz.sha256

Then check it:

    sha256sum -c mydos80-hwtest-0.2.0-E02.tar.gz.sha256

It must print `mydos80-hwtest-0.2.0-E02.tar.gz: OK`. If not, download it
again.

## 3. Unpack, and the optional installer

    tar xzf mydos80-hwtest-0.2.0-E02.tar.gz
    cd mydos80-hwtest-0.2.0-E02
    ./mydos80-hwtest version

`version` prints `MyDOS/80 HWLINK Tester 0.2.0-E02 (packaged)`. The tester
runs from this folder as it is; nothing else is needed.

**Optional:** `./install.sh` copies the folder to
`~/.local/share/mydos80-hwtest/0.2.0-E02/` and creates the command
`~/.local/bin/mydos80-hwtest`, so you can type `mydos80-hwtest` from
anywhere. It asks before doing anything, needs no password and changes
nothing outside your home directory. (If `~/.local/bin` is not yet on
your `PATH`, it says so: log out and in again, or use the full path.) If
you installed E01 before, see [section 13](#13-moving-from-e01-to-e02)
first.

In the rest of this guide, `./mydos80-hwtest` means "in the unpacked
folder"; after `./install.sh` you can type `mydos80-hwtest` instead.

## 4. Check the computer: preflight

    ./mydos80-hwtest preflight

It checks, and changes nothing: your Linux and Python, every file of the
package against its checksums, that no disk-write program is present, the
results folder (`~/mydos80-hwtest-results/`) and free space, and each
serial port (section 6). Each line starts with `OK` or `!!`. The last line
is either `READY: you can run the test.` or `NOT READY yet: see above.`,
followed, when something needs doing, by what you may need to do (the
tester never does it for you).

Without a serial adapter plugged in, `NOT READY` with `Serial ports: none
found` is expected.

## 5. The self-test (no hardware)

    ./mydos80-hwtest selftest

The self-test runs the **whole** test, exactly as it would run on a 4P,
against a simulated 4P built into the tester (through a Linux
pseudo-terminal). No serial port is opened and no hardware is used. It
takes about 1-2 minutes; the steps scroll past, marked `(automatic)`
where you would normally act. It must end with:

    SELF-TEST PASSED -- MyDOS/80 HWLINK Tester 0.2.0-E02
    Report: /home/YOU/mydos80-hwtest-results/DATE-TIME-selftest/REPORT.txt

`./mydos80-hwtest selftest --full` adds a simulated disk with known
faults in the simulated drive :1 (about 8 minutes with E02; about 12 with
E01). Its report is a good preview of a real one: E02's FINDINGS section
shows the faults it found, and the automatic follow-up's answers.

## 6. Serial ports and permissions

    ./mydos80-hwtest ports

lists every serial port with its driver and, for USB adapters, the make
and model, and whether you may use it. USB adapters appear as
`/dev/ttyUSB0` or `/dev/ttyACM0`, built-in ports as `/dev/ttyS0`; stable
names that do not change when you replug things are listed under
`/dev/serial/by-id/` (shown as "also ...").

To open a serial port your user must normally be in the group that owns
it, `dialout` on Fedora and Ubuntu. If the preflight or `ports` says the
port is not usable for that reason, it shows the command:

    sudo usermod -aG dialout $USER

Run it only if you agree (it needs your password and lets your user use
serial ports), then **log out and log in again** (or restart) and run the
preflight again. The tester never runs it for you.

Two other programs can get in the way; the preflight names them if they
are running:
- **brltty** (on some Ubuntu systems) claims CH340/CH341 USB adapters as
  Braille displays, so the adapter appears and disappears. If you do not
  use a Braille display: `sudo apt remove brltty`.
- **ModemManager** may probe a new port for a few seconds after it
  appears: wait 30 seconds after plugging the adapter in.

## 7. Connecting the 4P

- **Power the 4P off** while connecting the cable.
- The 4P's RS-232 port is a **DB-25 wired as DTE**, like a PC's port, so a
  **null modem** is needed. The 4P's ROM serial loader waits for its CD
  input (DB-25 pin 8): the PC's DTR must reach it, which most null-modem
  cables do.

  | Signal from | PC (DE-9) | 4P (DB-25) |
  |---|---|---|
  | PC transmit | 3 TxD | 3 RD |
  | 4P transmit | 2 RxD | 2 TD |
  | ground | 5 GND | 7 SGND |
  | PC DTR | 4 DTR | **8 CD** (and 6 DSR) |
  | 4P DTR | 1 DCD and 6 DSR | 20 DTR |
  | PC RTS | 7 RTS | 5 CTS |
  | 4P RTS | 8 CTS | 4 RTS |

- Leave **both floppy drives empty** to start with.
- Switch the 4P on; it shows its normal screen with no disk.

Optional cable check, which **sends nothing** to the 4P:

    ./mydos80-hwtest line-check /dev/ttyUSB0

(your device name). It opens the port with the loader's settings (19200
baud, 8 data bits, odd parity, 2 stop bits, no flow control), switches DTR
and RTS on, reads the modem lines and listens for 3 seconds. With the 4P
on and a cable that carries its DTR back, `CD` or `DSR` shows `1`. Not all
cables do; the test may still work.

## 8. Running the 4P test

    ./mydos80-hwtest

opens the menu; choose **4 = RUN THE 4P TEST** (or run
`./mydos80-hwtest run`). Then:

1. **Choose the port** (the list from section 6, or type it), read the
   summary of what the test does, and confirm. Give the 4P a short name
   (for example `4P-A`) and optional notes (its configuration, the cable,
   anything unusual). Please type no personal information.

2. **First serial-loader operation (the UART probe).** When the tester
   says *ENTER THE 4P SERIAL LOADER*:
   - on the 4P, **hold down RIGHT SHIFT, press and release RESET, and keep
     RIGHT SHIFT held for about 5 seconds**;
   - the 4P should show *Not Ready* or *Ready* (the loader is waiting for
     the PC);
   - press **ENTER** on the PC. The 4P then shows *Found Baud Rate* and
     *Loading*, the probe runs (the 4P's screen does not change), and the
     PC reports the serial chip's status bits.

   If the 4P starts anything else or shows its usual disk prompt, RIGHT
   SHIFT was not seen: try again. If it still does not enter the loader,
   hold **RIGHT SHIFT and BREAK together** while pressing RESET.

3. **Second serial-loader operation (the diagnostics program).** The
   tester asks for the same again: RIGHT SHIFT + RESET, *Ready*/*Not
   Ready*, ENTER. After *Loading*, the 4P shows the tester's dashboard,
   **`MYDOS/80 HWTEST 0.2.0-E02`** on its top line, with a table of tests
   that changes as they run (with E02 the screen is redrawn about every 2
   seconds while a test runs, and at once when a result changes). **From
   now on, do not press RESET or BREAK on the 4P** until the end.

4. **System tests** (about 3 minutes): CPU clock, CPU instruction check,
   memory map, video RAM (test patterns on the 4P's screen for a few
   seconds), the clock-status port, RAM. **Keyboard:** when asked, press
   ENTER on the PC, then press a few keys on the 4P one at a time (not
   BREAK) during the time the tester gives.

5. **Floppy (read only).** The tester asks what is in each drive. You may
   leave both empty, or insert a disk you do not mind being read
   (write-protected if possible: tab over the notch; door closed). For
   each drive it measures selection, READY, index pulses, rotation and
   the motor; only where a disk is seen to turn does it move the head (at
   the slowest step rate), survey what is recorded on both sides at both
   densities, and read every sector it found (only a status per sector
   comes back to the PC; disk contents never do). Each drive: about 1
   minute empty, a few minutes with a disk (E02 reads a track in two
   revolutions).

6. **Follow-up (E02, automatic).** The tester reads its own results so
   far and runs the read-only checks that settle what is uncertain
   (section 9). Nothing is asked; it usually takes seconds to a few
   minutes.

7. **The end.** The tester asks whether the dashboard was readable, then
   shows **`TESTING COMPLETE`** and the path of your report; the 4P shows
   *TESTING COMPLETE* too. You may now switch the 4P off.

Ctrl-C on the PC stops at any time; everything so far is kept and
reported. A complete test takes about 15-40 minutes.

## 9. Findings, follow-ups and the trs80gp comparison (E02)

**Findings.** Near the top of `REPORT.txt`, after the summary, the
FINDINGS section is the tester's own reading of the results. Each
finding has a severity:

| | |
|---|---|
| FAULT | something a working machine does not do (a RAM bit that fails, a CPU result that differs, a controller register that does not hold its value) |
| WARNING | unexpected, with more than one possible cause (a drive's speed, retransmissions on the link, an unreadable sector) |
| NOTE | unusual but explainable (a key held down, the 2 MHz clock, an answer you gave that the measurement disagrees with) |
| INFO | a measurement the project is collecting (for example the 4P's serial-loader messages, its UART bits, a drive's rotation and index pulse) |

Each one gives the measured values and its possible causes; none names a
chip or a board location (a failing RAM bit is reported as a data bit
and an address range). On a healthy 4P with empty drives you should see
nothing, or INFO lines only.

**Automatic follow-up.** Where one more read-only measurement would
settle a finding, the tester runs it straight away in the same session:
for example ten reads of a sector that failed (a hard error, a weak
sector, or one that reads now), a fresh survey of a track with missing
sector headers, a RAM range tested again with more passes, the clock
measured again. At most two rounds of twelve checks; a check is never
repeated. The answer appears under the finding it followed up
("follow-up result: ..."). `--no-adaptive` turns this off;
`./mydos80-hwtest followup DIR` runs the follow-up checks of an earlier
result in a new session (it asks you to confirm the same disk is in the
drive, and checks it).

**Comparison with trs80gp.** After the findings the report compares
your 4P with what the trs80gp emulator's 4P measured during the
project's emulator tests (each value with its reference, for example
HL-TGP-16: trs80gp reports an empty drive as READY). Where your real 4P
differs from the emulator is exactly the kind of result the project is
looking for. `./mydos80-hwtest compare DIR --reference trs80gp` shows it
on its own.

## 10. If the connection is lost (E02)

A loose cable, a RESET pressed by mistake, or a USB adapter that
disconnects no longer ends the test. The tester asks whether to
reconnect; then:

1. it tries the 4P as it is (a cable that came loose: the diagnostics are
   still running);
2. if the 4P does not answer, it asks you to put the 4P into the serial
   loader again (RIGHT SHIFT + RESET, as in section 8) and reloads the
   diagnostics;
3. it continues where it stopped. What finished is not run again, and
   before a drive is tested further its disk is identified again (by its
   sector headers and a few sectors' checksums): if the disk is not the
   one tested before, that drive is not tested further and the report
   says so.

At most two reconnections; after that the test ends as LINK LOST, and the
report keeps and analyses everything that ran.

## 11. Your report: finding and sending it

Every test, self-test included, is one folder:

    ~/mydos80-hwtest-results/DATE-TIME-NAME/
        REPORT.txt          the report to send (plain UTF-8 text)
        summary.json, analysis.json, steps.jsonl, transcript.jsonl, ...

At the end the tester prints the full path of `REPORT.txt`. To find it
later:

    ls -t ~/mydos80-hwtest-results/              # newest first
    ls ~/mydos80-hwtest-results/*/REPORT.txt     # every report

In a desktop file manager: your home folder, then
`mydos80-hwtest-results` (it is not hidden), then the folder with the
date, time and your 4P's short name. The menu's **show the last report**
prints the newest report on screen.

`REPORT.txt` holds the evidence class (REAL-HARDWARE for a 4P), the
result, your Linux and Python versions, the serial adapter's driver and
make, each test's outcome, the findings, the floppy measurements and any
problems. With E02 it ends with a block of letters between `BEGIN
MYDOS80-HWTEST DATA` and `END`: the same measurements in a form the
analysis tools read (plus the disk's sector-header layout and per-sector
read states). Please leave it in. The report holds **no ROM content, no
disk content and no user or computer name**; it does include the short
name and notes you typed. Open it in any text editor before you send it.

**Sending it.** Every result is welcome, and **a failure or an unexpected
result is just as useful as a success**; a test that stopped early still
has a report: send it. Open a
[new issue](https://github.com/jmccusker/mydos80/issues/new/choose) here
and choose **HWLINK tester report**. Attach `REPORT.txt` (drag it into
the text box), and say which Linux, serial adapter and cable you used and
anything you saw or heard. A photo of the 4P's final screen is welcome.
Issues are public: the report contains nothing personal, but check your
notes. (Testers working with Jim directly may send it to him instead.)
`./mydos80-hwtest bundle DIR` packs a whole results folder as
`DIR.tar.gz`, only if Jim asks for the detailed records.

## 12. More commands and options

| Command | |
|---|---|
| `./mydos80-hwtest drives` | **E02.** Which physical drive answers to each drive number (read only): the tester runs each drive for about 3 seconds while you watch the drive lights, then you put any disk into the drive that lit and take it out again; nothing is read from the disk |
| `./mydos80-hwtest analyze DIR` | **E02.** The findings of a result: a results folder, or a `REPORT.txt` on its own (E02's, and E01's: then the analysis has what the text shows). Several at once: what changed between runs |
| `./mydos80-hwtest followup DIR` | **E02.** A new session running DIR's follow-up checks (section 9) |
| `./mydos80-hwtest compare A B` | what differs between two results (folders, or `REPORT.txt` files with E02), and which findings appear in one only |
| `./mydos80-hwtest compare A --reference trs80gp` | **E02.** One result against trs80gp's recorded 4P values (or `virtual4p`: the simulator's model) |
| `./mydos80-hwtest report DIR` | rebuild DIR's REPORT.txt (with E02, an E01 folder's too, with findings) |
| `./mydos80-hwtest run --drives 0,1,2,3` | **E02.** The drive numbers to test (0-3; a 4P has two) |
| `./mydos80-hwtest run --cylinders 80` | **E02.** Survey 80 cylinders: **only for drives you know are 80-track** |
| `./mydos80-hwtest run --no-adaptive` | **E02.** No automatic follow-up |
| `--results DIR` (before the command) | another results folder than `~/mydos80-hwtest-results` |

## 13. Moving from E01 to E02

Your results are safe: both releases keep them in
`~/mydos80-hwtest-results/`, and neither the installers nor the
uninstallers ever touch that folder. E02 can analyse your E01 results
(`./mydos80-hwtest analyze DIR` or `report DIR` on an E01 folder, or
`analyze` on an E01 `REPORT.txt`). E01 stays available on its release
page; finishing a series of tests with E01 first is fine.

- **If you ran E01 from its unpacked folder:** just unpack E02 next to it
  (sections 2 and 3). The two folders are independent; delete E01's
  folder when you no longer need it.
- **If you installed E01 with `./install.sh`:** first remove the E01
  installation (your results are kept), then install E02:

      ~/.local/share/mydos80-hwtest/0.1.0-E01/uninstall.sh
      cd ~/Downloads/mydos80-hwtest-0.2.0-E02
      ./install.sh

  (Installing E02 over E01 also works: the `mydos80-hwtest` command then
  starts E02. But do not run E01's uninstaller after that: it removes the
  command link whichever release it points to. If you want to remove E01
  then, delete `~/.local/share/mydos80-hwtest/0.1.0-E01` by hand.)

`mydos80-hwtest version` tells you which release the command starts.

## 14. Troubleshooting

| What you see | What to do |
|---|---|
| `python3 is not installed` / `needs Python 3.10 or later` | install your distribution's `python3` (section 1) |
| `Package files: ... checksum differs`, or *The tester's files are damaged* | download the release again (section 2) and unpack it into a new folder |
| `Serial ports: none found` | plug the adapter in, wait 5 seconds, run `./mydos80-hwtest ports`; if it appears and disappears, see brltty (section 6) |
| a port `NOT USABLE` ... group `dialout` | section 6: join the group, then log out and in |
| *you are in the group ... but not in this login session yet* | log out and log in again |
| *in use by ...* | close that program (a terminal program, ModemManager) |
| the 4P shows *Not Ready* and stays | the PC's DTR is not reaching the 4P's pin 8: check the cable (section 7); `line-check` shows the lines |
| `no answer to 55h in 30 s` | the 4P was not in the serial loader: RIGHT SHIFT + RESET again (or RIGHT SHIFT + BREAK + RESET), then ENTER |
| *Error* on the 4P | the tester retries up to three times by itself |
| the 4P boots something or shows a disk prompt | RIGHT SHIFT not held in time, or a disk in drive :0: empty the drives, try again |
| `the UART profile cannot configure the stub` | your 4P's serial chip behaves differently from what the release supports: stop and send the report (it says what was measured) |
| *The connection to the 4P was lost* | E02: answer yes to reconnect and follow the screen (section 10); E01: the report keeps everything so far; check the cable and send the report |
| *the disk is not the one tested before* (E02) | a disk was changed while the connection was down: that drive's remaining tests were skipped; its earlier results stand |
| a drive: *READY but no index pulse* | reported as ambiguous (no disk, door open, or a drive that always reports READY); nothing more is done with that drive |
| the keyboard test saw no key | press keys during the time the tester gives (it says how long) |
| the command `mydos80-hwtest` starts the wrong release, or is gone | section 13; `./install.sh` in the E02 folder (after its `uninstall.sh` if it says the copy exists) |
| `SELF-TEST FAILED` | send the self-test's `REPORT.txt` |

The package's `docs/USER_MANUAL.txt` (also here:
[E02](E02/USER_MANUAL.txt), [E01](E01/USER_MANUAL.txt)) has more.

**Stop at once** (Ctrl-C, then switch the 4P off) and report if there is
smoke, a smell of burning or unusual heat; if a drive's motor runs or its
head loads for more than a few seconds while the drives are empty; or if
the laptop asks to install a driver or shows a dialog you do not expect.

## 15. Uninstalling

If you used `./install.sh`:

    ~/.local/share/mydos80-hwtest/0.2.0-E02/uninstall.sh

It asks first, then removes the installed copy and the
`~/.local/bin/mydos80-hwtest` command. If you did not install, simply
delete the unpacked `mydos80-hwtest-0.2.0-E02` folder. Either way **your
results in `~/mydos80-hwtest-results/` are kept**; delete that folder
yourself if you no longer want them.

## 16. Status, performance and limitations

- **Experimental; not validated on a real 4P** (above). Validated so far,
  for each release: the release package itself, extracted fresh and run
  as an ordinary user on Ubuntu 24.04, Ubuntu 22.04.5 (Python 3.10) and
  Fedora 45 (Python 3.15): start-up, preflight, port discovery, the
  self-tests, reports, installation and removal, and the error cases (no
  python3, a Python that is too old, a damaged package, a missing or
  forbidden port, an unwritable results folder); for E02 also the
  findings, the automatic follow-up, the analysis of E01 reports, the
  drive identification, and (in the project's own tests) lost
  connections, resets and a disk changed during a reconnection. The 4P
  test itself, `line-check`, `drives` and the serial driver details can
  only be exercised with a real adapter and 4P.
- **Performance (E02, measured on the simulated 4P):** reading the
  sectors of an 18-sector track takes 1.4 s instead of 4.6 s (two
  revolutions instead of about one a sector); the floppy inspection of
  the self-test's disk 125 s instead of 241 s; the full self-test about 8
  minutes instead of about 12. Writes to the 4P's screen are combined and
  limited to about one redraw every 2 seconds while a test runs.
- The UART probe stops the test if the 4P's serial chip does not behave
  as supported; floppy data transfers need the 4P's 4 MHz clock, which is
  measured; the keyboard test sees only the keys you press in its window.
- What is in each package, how it was built and validated, and every
  file's checksum: [E02/PACKAGE.md](E02/PACKAGE.md),
  [E01/PACKAGE.md](E01/PACKAGE.md). The packages' own documents: E02
  [Getting started](E02/GETTING_STARTED.txt),
  [User manual](E02/USER_MANUAL.txt),
  [Tester guide](E02/TESTER_GUIDE.txt),
  [Release notes](E02/RELEASE_NOTES.txt); E01
  [Getting started](E01/GETTING_STARTED.txt),
  [User manual](E01/USER_MANUAL.txt),
  [Tester guide](E01/TESTER_GUIDE.txt),
  [Release notes](E01/RELEASE_NOTES.txt).

The tester is covered by the MyDOS/80 evaluation licence,
[NOTICE.md](../NOTICE.md) (also in the package): personal, non-commercial
evaluation, education and testing; please link to the release page rather
than redistributing it. Designed and developed by Jim McCusker using
AI-assisted software development tools. Copyright © 2026 Jim McCusker. All
rights reserved. Provided "as is", without warranty of any kind, to the
extent permitted by applicable law.
