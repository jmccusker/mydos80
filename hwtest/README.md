# MyDOS/80 HWLINK Tester 0.1.0-E01 (Linux, TRS-80 Model 4P)

The HWLINK Tester is a Linux program that checks a real **TRS-80 Model
4P** over a serial cable and writes one plain-text report you can send
back. It is MyDOS/80's way of collecting real-hardware evidence from 4P
owners. Designed and developed by Jim McCusker.

**Download:** [Releases: hwtest-v0.1.0-e01](https://github.com/jmccusker/mydos80/releases/tag/hwtest-v0.1.0-e01)
(`mydos80-hwtest-0.1.0-E01.tar.gz`, SHA-256
`8cc92f455d47de644b7f00cadf9c278482cbb2c43a3224d98d3d058224eaf803`).

> **EXPERIMENTAL. NOT YET VALIDATED ON A REAL MODEL 4P.** E01 has passed
> its tests against a *simulated* 4P on Ubuntu 22.04, Ubuntu 24.04 and
> Fedora 45, but it has never yet run with a real serial adapter or a real
> 4P. The first real runs are exactly what this release is for. If it
> stops with a message, or a test fails, that is a useful result: please
> report it.

> **READ ONLY.** E01 never writes to, formats or creates a floppy disk:
> the release contains no program that could. It loads small test
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
[9 Your report](#9-your-report) ·
[10 Sending results](#10-sending-results) ·
[11 Troubleshooting](#11-troubleshooting) ·
[12 Uninstalling](#12-uninstalling) ·
[13 Status and limitations](#13-status-and-limitations)

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

From the [release page](https://github.com/jmccusker/mydos80/releases/tag/hwtest-v0.1.0-e01)
download both files into one folder, for example `~/Downloads`:

- `mydos80-hwtest-0.1.0-E01.tar.gz`
- `mydos80-hwtest-0.1.0-E01.tar.gz.sha256`

Or in a terminal:

    cd ~/Downloads
    curl -LO https://github.com/jmccusker/mydos80/releases/download/hwtest-v0.1.0-e01/mydos80-hwtest-0.1.0-E01.tar.gz
    curl -LO https://github.com/jmccusker/mydos80/releases/download/hwtest-v0.1.0-e01/mydos80-hwtest-0.1.0-E01.tar.gz.sha256

Then check it:

    sha256sum -c mydos80-hwtest-0.1.0-E01.tar.gz.sha256

It must print `mydos80-hwtest-0.1.0-E01.tar.gz: OK`. If not, download it
again.

## 3. Unpack, and the optional installer

    tar xzf mydos80-hwtest-0.1.0-E01.tar.gz
    cd mydos80-hwtest-0.1.0-E01
    ./mydos80-hwtest version

`version` prints `MyDOS/80 HWLINK Tester 0.1.0-E01 (packaged)`. The tester
runs from this folder as it is; nothing else is needed.

**Optional:** `./install.sh` copies the folder to
`~/.local/share/mydos80-hwtest/0.1.0-E01/` and creates the command
`~/.local/bin/mydos80-hwtest`, so you can type `mydos80-hwtest` from
anywhere. It asks before doing anything, needs no password and changes
nothing outside your home directory. (If `~/.local/bin` is not yet on your
`PATH`, it says so: log out and in again, or use the full path.)

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
takes about 2 minutes; the steps scroll past, marked `(automatic)` where
you would normally act. It must end with:

    SELF-TEST PASSED -- MyDOS/80 HWLINK Tester 0.1.0-E01
    Report: /home/YOU/mydos80-hwtest-results/DATE-TIME-selftest/REPORT.txt

`./mydos80-hwtest selftest --full` adds a simulated disk with known faults
in the simulated drive :1 (about 12 minutes). The self-test's report is a
good preview of a real one.

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
   **`MYDOS/80 HWTEST 0.1.0-E01`** on its top line, with a table of tests
   that changes as they run. **From now on, do not press RESET or BREAK on
   the 4P** until the end.

4. **System tests** (about 3 minutes): CPU clock, CPU instruction check,
   memory map, video RAM (test patterns on the 4P's screen for a few
   seconds), the clock-status port, RAM. **Keyboard:** when asked, press
   ENTER on the PC, then press a few keys on the 4P one at a time (not
   BREAK).

5. **Floppy (read only).** The tester asks what is in each drive. You may
   leave both empty, or insert a disk you do not mind being read
   (write-protected if possible: tab over the notch; door closed). For
   each drive it measures selection, READY, index pulses, rotation and
   the motor; only where a disk is seen to turn does it move the head (at
   the slowest step rate), survey what is recorded on both sides at both
   densities, and read every sector it found (only a status per sector
   comes back to the PC; disk contents never do). Each drive: about 1
   minute empty, 5-15 minutes with a disk.

6. **The end.** The tester asks whether the dashboard was readable, then
   shows **`TESTING COMPLETE`** and the path of your report; the 4P shows
   *TESTING COMPLETE* too. You may now switch the 4P off.

Ctrl-C on the PC stops at any time; everything so far is kept and
reported. A complete test takes about 15-40 minutes.

## 9. Your report

Every test, self-test included, is one folder:

    ~/mydos80-hwtest-results/DATE-TIME-NAME/
        REPORT.txt          the report to send (plain UTF-8 text)
        summary.json        the results in a fixed form
        steps.jsonl, transcript.jsonl, run.json, ... (detailed records)

    ls ~/mydos80-hwtest-results/

lists them. `REPORT.txt` holds the evidence class (REAL-HARDWARE for a
4P), the result, your Linux and Python versions, the serial adapter's
driver and make, each test's outcome, the floppy measurements and any
problems. It holds **no ROM content, no disk content and no user or
computer name**; it does include the short name and notes you typed.
Open it in any text editor before you send it.

Other commands: `./mydos80-hwtest report DIR` rebuilds a folder's report;
`./mydos80-hwtest compare DIR_A DIR_B` lists what differs between two
results; `./mydos80-hwtest bundle DIR` packs a whole folder as
`DIR.tar.gz` (only if asked for the detailed records).

## 10. Sending results

**Every result is welcome, and a failure or an unexpected result is just
as useful as a success.** A test that stopped early still has a report:
send it.

Open a [new issue](https://github.com/jmccusker/mydos80/issues/new/choose)
here and choose **HWLINK tester report**. Attach `REPORT.txt` (drag it into
the text box), and say which Linux, serial adapter and cable you used and
anything you saw or heard. A photo of the 4P's final screen is welcome.
Issues are public: the report contains nothing personal, but check your
notes. (Testers working with Jim directly may send it to him instead.)

## 11. Troubleshooting

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
| `the UART profile cannot configure the stub` | your 4P's serial chip behaves differently from what E01 supports: stop and send the report (it says what was measured) |
| the connection was lost during the test | the report keeps everything so far; check the cable is seated; send the report |
| a drive: *READY but no index pulse* | reported as ambiguous (no disk, door open, or a drive that always reports READY); nothing more is done with that drive |
| `SELF-TEST FAILED` | send the self-test's `REPORT.txt` |

The full manual, `docs/USER_MANUAL.txt` in the package (also
[here](E01/USER_MANUAL.txt)), has more.

**Stop at once** (Ctrl-C, then switch the 4P off) and report if there is
smoke, a smell of burning or unusual heat; if a drive's motor runs or its
head loads for more than a few seconds while the drives are empty; or if
the laptop asks to install a driver or shows a dialog you do not expect.

## 12. Uninstalling

If you used `./install.sh`:

    ~/.local/share/mydos80-hwtest/0.1.0-E01/uninstall.sh

It asks first, then removes the installed copy and the
`~/.local/bin/mydos80-hwtest` command. If you did not install, simply
delete the unpacked `mydos80-hwtest-0.1.0-E01` folder. Either way **your
results in `~/mydos80-hwtest-results/` are kept**; delete that folder
yourself if you no longer want them.

## 13. Status and limitations

- **Experimental; not validated on a real 4P** (above). Validated so far:
  the release package itself, extracted fresh and run as an ordinary user
  on Ubuntu 24.04, Ubuntu 22.04.5 (Python 3.10) and Fedora 45 (Python
  3.15): start-up, preflight, port discovery, both self-tests, reports,
  installation and removal, and the error cases (no python3, a Python that
  is too old, a damaged package, a missing or forbidden port, an
  unwritable results folder). The 4P test itself, `line-check`, and the
  serial driver details (parity, modem lines, error counters) can only be
  exercised with a real adapter and 4P.
- The UART probe stops the test if the 4P's serial chip does not behave
  as E01 supports; floppy data transfers need the 4P's 4 MHz clock, which
  is measured; the keyboard test sees only the keys you press in its
  window.
- What is in the package, how it was built and validated, and every file's
  checksum: [E01/PACKAGE.md](E01/PACKAGE.md). The package's own documents:
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
