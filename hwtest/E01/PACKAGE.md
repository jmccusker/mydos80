# HWLINK Tester 0.1.0-E01: the package

| | |
|---|---|
| Release | [hwtest-v0.1.0-e01](https://github.com/jmccusker/mydos80/releases/tag/hwtest-v0.1.0-e01) (pre-release, experimental) |
| Asset | `mydos80-hwtest-0.1.0-E01.tar.gz`, 187,363 bytes |
| SHA-256 | `8cc92f455d47de644b7f00cadf9c278482cbb2c43a3224d98d3d058224eaf803` (also `mydos80-hwtest-0.1.0-E01.tar.gz.sha256`) |
| Version | 0.1.0-E01: shown by `mydos80-hwtest version`, on the 4P's dashboard and in every report |
| Built from | MyDOS/80 development repository commit `9dea693984c884793e720c7551e59ba11b9065aa` (recorded in the package's `VERSION` file) |

## What is in it

| Path | |
|---|---|
| `mydos80-hwtest` | the launcher (POSIX sh): finds `python3` 3.10+, runs the tester isolated from your Python settings (`-I`), writes no `.pyc` files |
| `install.sh`, `uninstall.sh` | optional per-user installation into `~/.local` (asks first; no `sudo`) |
| `lib/` | the tester: Python 3, standard library only (36 files, readable source). `hwlink/` is the serial link, ROM loader client, UART probe and diagnostics framework; `hwlink/tester/` the Linux application (serial ports, the guided test, reports, the built-in simulated 4P for the self-test) |
| `data/` | the prebuilt Z80 programs, each checked against `data/catalog.json` before use: the UART probe (`probe2.rom`), the diagnostics stub (`stub/`: the default build and a variant for each other pair of serial status bits the stub supports, 20 files) and the diagnostic modules (`modules/`: 11 modules for each of 3 memory slots) |
| `docs/` | [Getting started](GETTING_STARTED.txt), [User manual](USER_MANUAL.txt), [Tester guide](TESTER_GUIDE.txt), [Release notes](RELEASE_NOTES.txt) (copies here, byte-identical) |
| `README.txt`, `NOTICE.md`, `VERSION`, `SHA256SUMS` | the getting-started text, the evaluation licence, the version and source commit, every file's checksum |

The diagnostic modules (all read only):

| Module | Checks |
|---|---|
| SYSINFO | the memory page map (write-and-restore of single bytes), the CPU clock |
| CPUKAT | ALU, rotate and shift results and their documented flags against a reference |
| RAMTEST | free RAM (March C-) |
| VRAM | video RAM (saved, tested, restored) |
| KEYS | the keyboard matrix (keys you press; keys held throughout) |
| RTC | the clock-status port E0h |
| FDCID | the floppy controller's registers |
| FDCDRV | a drive's selection, READY, index pulses, rotation, motor |
| FDCSEEK | head stepping and track 0; seek accuracy by READ ADDRESS |
| FDCSCAN | what is recorded on each track (READ ADDRESS) |
| FDCREAD | sector reads: a status and a CRC per sector come back, never the data |

**No disk-write capability.** The development system's floppy write and
format modules are not in this release; each module's header declares its
capabilities and none declares floppy writing; the PC checks every module
before upload (no controller access except through the read-only library
routines, no port of a hard disk, FreHD, the memory map or the CPU speed);
the tester's session never authorizes a write drive. No ROM content is in
the package or read by it.

## How it was built and validated

Built on Ubuntu 24.04 with Python 3.12.3 and SjASMPlus 1.24.0 by the
development repository's `tools/make_hwtest_package.py`, which assembles
the Z80 programs, checks every module, copies the tester's source,
checks every Python file for Python 3.10 compatibility, checks the
licence and attribution texts and runs the publication privacy scan over
every file, and writes the tarball reproducibly (fixed owner and times: a
rebuild from the same commit gives the same SHA-256).

Validation of this exact tarball (evidence: simulator only, no 4P):

| Where | What |
|---|---|
| Ubuntu 24.04 | extracted fresh, run with an empty environment and its own home: version, preflight, self-test and the full self-test PASSED; the folder unchanged afterwards |
| Ubuntu 22.04.5 (Python 3.10.12) and Fedora 45 (Python 3.15.2) | minimal environments built from the official images plus `python3` only, run as an ordinary user: 28 cases each, all as expected: checksum, unpack, no Git/pip/virtualenv/assembler present, missing and too-old Python, version, preflight, ports, the menu, missing and forbidden port, unwritable results folder, a damaged copy refused, both self-tests PASSED, report, compare, bundle, installer declined / done / repeated, uninstall (results kept), the folder unchanged; nothing changed outside the tester's home |
| the public download | the asset downloaded from this repository is byte-identical to the tested tarball |

Not yet exercised: a real serial adapter (parity, modem lines, error
counters), a real Model 4P, full desktop installations.

## Source

The tester's application source is the readable Python in `lib/`. The Z80
sources of the probe, stub and modules, and the build system, are part of
the MyDOS/80 development repository, which is not public (as for the rest
of MyDOS/80: see this repository's README); the release records their
commit, and the binaries' checksums let any later build be compared.

## The package's file checksums

The package's own `SHA256SUMS` (check it after unpacking with
`sha256sum -c SHA256SUMS`; the preflight does the same):

```
5ec9f4aeeb2020f66a8288963c406b0b81aa19b04a09f46e2e22a35d68223c3f  NOTICE.md
1a92d31c239b766bdfa9bba51b186dbb6ee4e342dc66740f460b753122b9eaf8  README.txt
4134b18a968e63f7222b20e77be99d94636000c3219673cfc70e4a1d39692869  VERSION
69e19fae5c34a2019b29ec6470b786ae6a08de5fbea36eb4684769fb2f72047c  data/catalog.json
d5464a55ed2ea62e1fba393a5ba2e3b326f624e896ec9289acaecc98f08ac078  data/modules/cpukat-8000.bin
c44d8b2a3aa25f5f2327739f13d7a9b8466401f56ec6d9e65cc358703c11e7c2  data/modules/cpukat-A000.bin
822cc4e18e6cd843c006bf79d62ec64df9c28cad73f700d28aaa8b05fb73be33  data/modules/cpukat-C000.bin
d080793ed6059e3f1fa909ffb58dcff7bccedec60f5bfcb3d17b80298b5e54e7  data/modules/fdcdrv-8000.bin
1ca820e7cfcc3914415642d61624b7cb9117fc33bf5a01f93691587ee6b64b3d  data/modules/fdcdrv-A000.bin
6dbff50de14c3dcfaef4d56f3e5bcfd1dc66fd96b8cb866867daed0b1282e6fd  data/modules/fdcdrv-C000.bin
7c03d22421b2883131e87ec5f53596929c12bcc45b4b38550459b3dd93e5f358  data/modules/fdcid-8000.bin
ca1a275237fa6e772ef437f9bfe7cf2484b4389e4f9c8ef39cb59243ebe376a8  data/modules/fdcid-A000.bin
e049f00d24cab77a2d7faadca6e27d03424bd51db45ae6406fb3c38015a09ccf  data/modules/fdcid-C000.bin
6222c60d97edc738b81ab808bc323d8052397b70234087de52415eccb88a28a5  data/modules/fdcread-8000.bin
3b9482640139ac949aac0cf8bff8df495c300d48afd5ecd3c2b0fdfa861a109e  data/modules/fdcread-A000.bin
3982eeb1bec79baa3415d13d6218061e8065a47bae6b6db18ef47d804578bc50  data/modules/fdcread-C000.bin
cb481e0e3e00a61df1bbeec834653f0e6cbeb2c86d2d894a8e62759d76f24928  data/modules/fdcscan-8000.bin
2ac39f240f390bf7286a814038ff04c7603c4351b1c1801cb17417ee6f34da16  data/modules/fdcscan-A000.bin
2e0b5545e50df65203f8760ad867e27d25b5748747f05b7b9dd84b01939e44d8  data/modules/fdcscan-C000.bin
a8360146a19c9a0428d92c88b1519d4aa88e614c395275a785ad9fbe76194c48  data/modules/fdcseek-8000.bin
24b2a531dd2cfca8c77181481843968db76c1981f995490d0e9b56b08cfaabde  data/modules/fdcseek-A000.bin
b11b462d93f48020f8c32a24fea41334cba820a049e6ef9f6c8e4469b7c7f3e5  data/modules/fdcseek-C000.bin
772d1fa474475c3c93bee545e1fdb56783e717de02cc0243c6ee783cd85b7f34  data/modules/keys-8000.bin
532a5c4ea4493763e8254a4a4ff3384e520825670099a9223e606dba4e030ce8  data/modules/keys-A000.bin
12c0ed58c3fe352d834f1cf4485a50ea0781895b76ef981fd46e53d847fb56a8  data/modules/keys-C000.bin
c8672baecfa8c64928c0cd6532dd2515127fbffce1c5cf5be97165b5cfeba5f9  data/modules/ramtest-8000.bin
2b0d5844d0fcc2b67dee5d056984793a95d4b821d6e4da4a6646c3f80e64cdac  data/modules/ramtest-A000.bin
86ec530f83b41ca9dace063183d71737418c47254379959b44117ebb9cade6b6  data/modules/ramtest-C000.bin
6b3fd9a5592f10cd6a88186540bce5cc453f29f573f529bec40a04157a8821a8  data/modules/rtc-8000.bin
5702fbae2d24fac51c4888fad13ff6a9b942c2ef3bc0065c4629c8b48ecb7125  data/modules/rtc-A000.bin
aa246914f68f9755cec2b89177c253ef461cd46ee017e24e5be674dae4429129  data/modules/rtc-C000.bin
c0c01209913d6a0f3fc103b44505bd8c940b9067ce3ec8049b50c464d8415cf8  data/modules/sysinfo-8000.bin
4d116bbf5ce33a3ce00da28847c198087e636dfba33317ad15e4579b1608a616  data/modules/sysinfo-A000.bin
294b19ec8f1beff7329e94b452933f77453a56ec431e956a4694ed5caa6f7745  data/modules/sysinfo-C000.bin
8552f6fad3d2a36974c30bb7c06a65a9c7c0edd238e02da92e7d240266aaaf44  data/modules/vram-8000.bin
333e21a8e0cb641529ac1192a1e89e6fd3807f505eab00a3e33e996b28ce9a8e  data/modules/vram-A000.bin
bd3fdd6b86072e762f318ba74dcbffd2a1dcf0d1965fc0a70bc885837c3d987c  data/modules/vram-C000.bin
f05c9f93e2a0692e5eb5c1659bed1a9612dd3dc05fedb9a119e8dc51aac0c5ed  data/probe2.rom
5fae4fe76cc05083d9ca5d55e9fd426f9db82381c9d2258190b8dbfaa041eb1d  data/stub/stub-dr0-thre1.rom
d29ae61f4ae16e62885c8610d284a37cb16277750e5bbf7751cdde11fdc804e6  data/stub/stub-dr0-thre2.rom
d11e9d97f231af6d46c62a9dd4e1ecd5c34af103e37425e60f8b10700d632716  data/stub/stub-dr0-thre6.rom
b5541eb8532720b72df6ea7b3d4dc99d8a167e0d338aee7da6bd6107046734eb  data/stub/stub-dr0-thre7.rom
0095fe69b61a2bcb96147707bce495ffb6d4dd9847ae434feed1f3ea502c99ec  data/stub/stub-dr1-thre0.rom
a18fbdcdf9b90fadd5765b49292672c29975fd1255687cd489f358160b9a2768  data/stub/stub-dr1-thre2.rom
25cd94c8500ac40925f4fbd22dbd1c20c43b72dac1529d1e4f9113c72d8db8e2  data/stub/stub-dr1-thre6.rom
82e6d9868a3f75e160f7ba48fb531e377f630e61436dce77632ccac1ce242b03  data/stub/stub-dr1-thre7.rom
c74397c5774852d656d7968baa40e5062af10f9086120e3782300c3cbccd7ebc  data/stub/stub-dr2-thre0.rom
389ab1e50d4c2300b778fdee91187787dcd155d205b651982a4e1e5cf7d68e86  data/stub/stub-dr2-thre1.rom
438d05b1249e9c29186c2cb0aa3322b9e0de39cdf140345a9bba5280060b2414  data/stub/stub-dr2-thre6.rom
de06f5b2c762513a16fae3b0108a503341b5be2792cd07ddc3a5d095cb68610f  data/stub/stub-dr2-thre7.rom
57b592085aff1e27a30743f420dd725954a4ea1aaeb4f4682c50232aeae66dfe  data/stub/stub-dr6-thre0.rom
9620c281d02bd464a47f76b1076fbc4bffcd0660540547c21d6bdfbabdfe3584  data/stub/stub-dr6-thre1.rom
e8409197ba9f438922343f04aa2514cacf928d8abac0b6d26fbd6523e88e26a6  data/stub/stub-dr6-thre2.rom
ca5185f75f091a388e4b6817b01d0e8525e8211028958c3932ffa127e602bba7  data/stub/stub-dr6-thre7.rom
33c2e47556ad4269fe2f3e756e5c1a21b9b3d907839e3680ff09031a4cd46a7e  data/stub/stub-dr7-thre0.rom
a3b4f8c9443235e57a840c8c15065bae861e8b180f2d56dbf9de3f4fae568384  data/stub/stub-dr7-thre1.rom
fb605fae6e6a8567739e7a3510b76fe7d182bfeb7b8201b98347624cc8f9c05a  data/stub/stub-dr7-thre2.rom
c69175210e4f7183270e5163584971c9616dbc67af8daaabaef800a2cd907cfd  data/stub/stub-dr7-thre6.rom
1a92d31c239b766bdfa9bba51b186dbb6ee4e342dc66740f460b753122b9eaf8  docs/GETTING_STARTED.txt
4446c7c71207dea5f77aba091275fe22b4df21f1cdac42ca4a65190f059237d5  docs/RELEASE_NOTES.txt
f93eed42a8dd5db095262d2f9182d49bc56ac294adf429a3fd07815ecd5b3380  docs/TESTER_GUIDE.txt
5582caf91a2b8497e426898111f31a2100ac3d8e32f70a2131448219da0f6461  docs/USER_MANUAL.txt
60de5d561f107d415459613e5fb3a62dcc702633eeb3a563e15f9ec21514e2c3  install.sh
2d91dd40ea292c075bb7c1be7447c7a973d26487fa15a681d6c519f08829d738  lib/cmdfile.py
a43b82687847931284778968e942bce3eb6f8de639f7fceaf3ba7fc2f0e620c7  lib/hwlink/__init__.py
506ae91466f585796ad30083f4a1e84026d46a28c1f514789f507ad3b0623915  lib/hwlink/crc.py
5a9e1d7895566a74533e48525754e12094f1cf05a51b33f831137b9957a0c24c  lib/hwlink/diag/__init__.py
ececb355e287d388e1eab674d4af66dec697a99ee3ef02d66355476a7e8b1c87  lib/hwlink/diag/build.py
ccb15c2f6a8f51debc8dcddf659effefa90787fbb73fbc6ee61f9cf5bb0bc967  lib/hwlink/diag/dashboard.py
6321d61e2a900894ebbcdd43d2c9912b514b21c52f7ecb73a4bf96b7a3316499  lib/hwlink/diag/desc.py
0533be94c47b3870daad2afa54ddb724eb625d3c8108f4e4fc09c77ad5ab4b2f  lib/hwlink/diag/fdcplan.py
76c973128ea07607bda52f64edefb727a99c57dbd043d5a7615320fcf64ddbc3  lib/hwlink/diag/floppy.py
d414ba538cf19407184e2e91e7a5925071f9417256b8a8f8f631b1b20d884af5  lib/hwlink/diag/modfmt.py
d6f8099dc02ea32e9c3638503316faeb9987864eee9ba740360a0b4f242f872e  lib/hwlink/diag/modules.py
fb28edabe108b6890985925e95c1a55fe69d950d58287f79edfa69b4294b170e  lib/hwlink/diag/runner.py
5d6e711f24eb086d49032f974c82d94cb0bbae36ede17ee18b693c7141c694d3  lib/hwlink/diag/server.py
7a0a0f5768bc1816dc6839196822610588be5f355bc9b6b0625291b390b38e45  lib/hwlink/fdcsim.py
969f2fe896b9e9348f20516ec5e5e74afa35b918b32e05c1e68a88e4cf19fbaa  lib/hwlink/frame.py
0fd5a31073ba646ba13ee822ad6624277a3e488e0509b580ecb02bc92b3210f7  lib/hwlink/hwprofile.py
73077937dd5ae48271bf41227d74698b8b2083d885a2c7e9ba9f7e44208e0d3d  lib/hwlink/link.py
dfb07b1808158d7bd0c8b09124247262713ee6a3526ac7fdc8e1084cdd4214af  lib/hwlink/policy.py
4edd3c79413292d5f8454e68c42649c2e712c592202d99577990afe651d52e08  lib/hwlink/probe.py
352ba229cb876874123a7f8cd415a40bffb19a169c0594bff5932bc94b9705c5  lib/hwlink/rom4p.py
7cdb10dcb7ecfac0fba2391ee5d3290b70106862771fd2cf535ebde61621aca6  lib/hwlink/romboot.py
e0933d82583e3eabfd2c148eac2bf7d7220ad733b328c2050aa324a49540c5f6  lib/hwlink/sim.py
c5fc44bc54a79ae8c697f4ce6c56c36d2b3c586055f32a55619fcf2be38d27a3  lib/hwlink/target.py
48ef19b9d938d65b35a1c3cf6e7da9cbb5abc5a49d4c0fafdeb774f54325e843  lib/hwlink/tester/__init__.py
672f1868fec33bacfa364009303f699120a22022107e1d3dad33955a7e489eaf  lib/hwlink/tester/app.py
064db5a14e20e00d5f546ea9152ec525de13af3974262caa30b506efbd140345  lib/hwlink/tester/catalog.py
b0193042eee171bc8387f00a64d376c6b46bad7a6c2d843d4e31ff71ef2b1beb  lib/hwlink/tester/flow.py
7e83365988bc8e4b678acd104f691ab339230b5e8c8c2f98e5ed1153d1e7075e  lib/hwlink/tester/media.py
0983babcf27f195fec0749da33aa54e3509196823ed9dbbc29a16eb0a31e56e4  lib/hwlink/tester/report.py
7a0e2b19778da078d8652d5767ff94f7efaa1e12825621038e9c37902aa7cd4f  lib/hwlink/tester/serialport.py
c70fbef198c18bf1e43c86441eda1172c9c86df1a24f84d953708b1a686e6261  lib/hwlink/tester/ui.py
05e486c228d584f71ad005183fdb1f186ba2cc2b0054116504ef654ed426de4f  lib/hwlink/tester/virtual4p.py
b72342a2fc2fd0caabb30c90cece2c6c7cd4756c42e9cbb39060bef83ceedbc1  lib/hwlink/transport.py
9937f9349c697e2598c500f8dfa99ce1f5b1ccc04bba91f54091101c0c9f288d  lib/track_profiles.py
9ef094de4add4739cdb2840d856026e731909d2527d0386858f384761f4d5ca2  lib/z80dis.py
31261268216a670a10ffb5a17bc43427f3f63b48ee65aec73dbd753379ff9b35  lib/z80sub.py
dc2548988d7ac60be7e386466fda6fc308c783640836d0343585f7395cb057c5  mydos80-hwtest
30a518342be363dd2c3727f69c5e6355ed6458339b6b3e890927b536026dd5db  uninstall.sh
```
