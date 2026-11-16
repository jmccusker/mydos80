# Reporting what you find

Thank you for trying MyDOS/80. Every report helps me, and **a failure or an
unexpected result is just as useful as a success**: it tells me where to
look. "It all worked" is welcome too.

## Where

Here, in this repository's **Issues**. Choose **New issue** and pick a
form:

- *Bug report*: something did not work as the documentation says.
- *Feature request*: something you would like MyDOS/80 to do.
- *Hardware compatibility report*: any attempt on a real TRS-80 or a
  floppy emulator, successful or not.
- For a question, open a blank issue.

Please search the existing issues first; a "me too, on Windows" comment on
an existing issue is useful.

## What makes a report reproducible

1. **Start fresh.** Begin from fresh copies of the release's disk images
   (the Windows launcher does this; on Linux and macOS, copy the disks
   into a new folder). Say if you did not.
2. **Name everything:** the MyDOS/80 version (the boot screen's top line,
   e.g. "0.10 RC3"), the machine you chose (Model III, 4, 4P), the
   emulator and its version, and your computer's operating system.
3. **Say which disk was in which drive** (`:0` to `:3`) and whether
   writing was on (`WRITES ON`).
4. **Give the exact commands**, one per line, as you typed them, starting
   from the boot screen.
5. **Copy the screen text exactly**, including any message, or attach a
   screenshot (trs80gp: F11 saves one).
6. **Say what you expected and what happened instead**, and whether it
   happens again when you repeat the steps.
7. **Guided Tour exercise numbers** (e.g. "Exercise 6.4") are the quickest
   way to say where you were.

## Please do not

- attach ROM images, MODELA/III, or copies of commercial DOS disks, even
  when they are involved; describe them instead (name, version, source);
- post personal information you do not want public: issues are visible to
  everyone.

## Real hardware

Use only disposable media. Never write, delete or format on an original or
irreplaceable disk, image or drive. The hardware form asks for the details
that let me tell a MyDOS/80 problem from a media or drive problem: the
machine and ROM, the controller, the drive and media, how the image was
written, and what a readback showed.
