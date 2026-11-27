# HWLINK Tester 0.2.0-E02: the package

| | |
|---|---|
| Release | [hwtest-v0.2.0-e02](https://github.com/jmccusker/mydos80/releases/tag/hwtest-v0.2.0-e02) (pre-release, experimental) |
| Asset | `mydos80-hwtest-0.2.0-E02.tar.gz`, 226,706 bytes |
| SHA-256 | `2e8752e31b06cdcf333e8639b34316c37cc25096e2508ea9c6b94c5c9ca9da16` (also `mydos80-hwtest-0.2.0-E02.tar.gz.sha256`) |
| Version | 0.2.0-E02: shown by `mydos80-hwtest version`, on the 4P's dashboard and in every report |
| Built from | MyDOS/80 development repository commit `53e3020a946bdcb1832e585e97fcdc3a31edb1ea` (recorded in the package's `VERSION` file) |
| Previous | E01 (0.1.0-E01): unchanged and still available, [E01/PACKAGE.md](../E01/PACKAGE.md) |

## What is in it

| Path | |
|---|---|
| `mydos80-hwtest` | the launcher (POSIX sh): finds `python3` 3.10+, runs the tester isolated from your Python settings (`-I`), writes no `.pyc` files |
| `install.sh`, `uninstall.sh` | optional per-user installation into `~/.local` (asks first; no `sudo`) |
| `lib/` | the tester: Python 3, standard library only (38 files, readable source). New in E02: `hwlink/tester/analyze.py` (the findings, the trs80gp and simulator comparison, the follow-up plans) and `hwlink/tester/bundle.py` (the data block at the end of REPORT.txt; reading E01 reports) |
| `data/` | the prebuilt Z80 programs, each checked against `data/catalog.json` before use: the UART probe, the diagnostics stub (the default build and 19 variants for other serial status bits) and the diagnostic modules (33 files: 11 modules for each of 3 memory slots) |
| `docs/` | [Getting started](GETTING_STARTED.txt), [User manual](USER_MANUAL.txt), [Tester guide](TESTER_GUIDE.txt), [Release notes](RELEASE_NOTES.txt) (copies here, byte-identical) |
| `README.txt`, `NOTICE.md`, `VERSION`, `SHA256SUMS` | the getting-started text, the evaluation licence, the version and source commit, every file's checksum |

The diagnostic modules, all **read only** (the same eleven as E01; FDCREAD
is E02's faster version):

| Module | Checks |
|---|---|
| SYSINFO | the memory page map (write-and-restore of single bytes), the CPU clock |
| CPUKAT | ALU, rotate and shift results and their documented flags against a reference |
| RAMTEST | free RAM (March C-) |
| VRAM | video RAM (saved, tested, restored) |
| KEYS | the keyboard matrix (keys you press) |
| RTC | the clock-status port E0h |
| FDCID | the floppy controller's registers |
| FDCDRV | a drive's selection, READY, index pulses, rotation, motor |
| FDCSEEK | head stepping and track 0; seek accuracy by READ ADDRESS |
| FDCSCAN | what is recorded on each track (READ ADDRESS) |
| FDCREAD | sector reads: a status and a CRC per sector come back, never the data. E02: the sectors of a track read in two revolutions (E01: about one revolution a sector) |

**No disk-write capability.** No write or format module is in the
package (none of the development system's FDCFMT, FDCWRITE, FDCPUT or
FDCTRK), nor the development image writer; each module's header declares
its capabilities and none declares floppy writing; the PC checks every
module before upload; the tester's session never authorizes a write
drive. No ROM content is in the package or read by it.

## How it was built and validated

Built on Ubuntu 24.04 with Python 3.12.3 and SjASMPlus 1.24.0 by the
development repository's `tools/make_hwtest_package.py`, which assembles
the Z80 programs, checks every module, copies the tester's source and
checks that every import inside it resolves there, checks every Python
file for Python 3.10 compatibility, checks the licence and attribution
texts, runs the publication privacy scan over every file, and writes the
tarball reproducibly.

Validation of this exact tarball (evidence: simulator only, no 4P):

| Where | What |
|---|---|
| Ubuntu 24.04 | extracted fresh, run with an empty environment: version, preflight, self-test and the full self-test PASSED; the folder unchanged afterwards |
| Ubuntu 22.04.5 (Python 3.10.12) and Fedora 45 (Python 3.15.2) | minimal environments from the same official images as E01, plus `python3` only, run as an ordinary user: 37 cases each, identical: E01's 28 cases with E01's results (checksum, unpack, no Git/pip/assembler, missing and too-old Python, preflight, ports, the menu, missing and forbidden port, unwritable results folder, damaged copy refused, both self-tests, report, compare, bundle, installer declined / done / repeated, uninstall with results kept, the folder unchanged) and E02's 9 (the automatic follow-up ran, analyze of a results folder, of its REPORT.txt and of an E01 REPORT.txt, compare with trs80gp's recorded values, the drive-identification and follow-up self-tests PASSED, no writer in the package); nothing changed outside the tester's home |
| the development tree | 373 automated tests PASS at the same source |
| the public download | byte-identical to the tested tarball |

Not yet exercised: a real serial adapter, a real Model 4P.

## Source

The tester's application source is the readable Python in `lib/`. The Z80
sources of the probe, stub and modules, and the build system, are part of
the MyDOS/80 development repository, which is not public; the release
records their commit, and the binaries' checksums let any later build be
compared.

## The package's file checksums

The package's own `SHA256SUMS` (check it after unpacking with
`sha256sum -c SHA256SUMS`; the preflight does the same):

```
5ec9f4aeeb2020f66a8288963c406b0b81aa19b04a09f46e2e22a35d68223c3f  NOTICE.md
13fcbda2198f76a7a7e4fc9b08c263931e9f775b6f2c2ff3311edfa6b620492a  README.txt
01bfb0944bb8f6076b9a41e9cc96211b16b05f273df81c5c84d05688a3d5d96e  VERSION
e03bfae86591772d6a6f20a39d24745d166920c5ae7214969b6ac5775960324c  data/catalog.json
d5464a55ed2ea62e1fba393a5ba2e3b326f624e896ec9289acaecc98f08ac078  data/modules/cpukat-8000.bin
c44d8b2a3aa25f5f2327739f13d7a9b8466401f56ec6d9e65cc358703c11e7c2  data/modules/cpukat-A000.bin
822cc4e18e6cd843c006bf79d62ec64df9c28cad73f700d28aaa8b05fb73be33  data/modules/cpukat-C000.bin
d080793ed6059e3f1fa909ffb58dcff7bccedec60f5bfcb3d17b80298b5e54e7  data/modules/fdcdrv-8000.bin
1ca820e7cfcc3914415642d61624b7cb9117fc33bf5a01f93691587ee6b64b3d  data/modules/fdcdrv-A000.bin
6dbff50de14c3dcfaef4d56f3e5bcfd1dc66fd96b8cb866867daed0b1282e6fd  data/modules/fdcdrv-C000.bin
7c03d22421b2883131e87ec5f53596929c12bcc45b4b38550459b3dd93e5f358  data/modules/fdcid-8000.bin
ca1a275237fa6e772ef437f9bfe7cf2484b4389e4f9c8ef39cb59243ebe376a8  data/modules/fdcid-A000.bin
e049f00d24cab77a2d7faadca6e27d03424bd51db45ae6406fb3c38015a09ccf  data/modules/fdcid-C000.bin
9de50d5257f85c2fd870a5ee8a622c12247ac3c505a866710c227fcd8fcf8db7  data/modules/fdcread-8000.bin
9fcbf475273801875bd1b62613cb1f5e8dcccf912e7dcc8f1878a13728ed30cf  data/modules/fdcread-A000.bin
4b9f031cbf9ba52448f225980f749bd000d184b27177d77394113e6adf2174f0  data/modules/fdcread-C000.bin
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
13fcbda2198f76a7a7e4fc9b08c263931e9f775b6f2c2ff3311edfa6b620492a  docs/GETTING_STARTED.txt
0e110ef1f86089b2efc1a955fff35550d1c7092dec5debae49b23847c6b11d9f  docs/RELEASE_NOTES.txt
1db96b615ea1f8329f9b219dcde494fdadb989f2191a03c706dacc0b15cfcb6f  docs/TESTER_GUIDE.txt
4b81a99188b561b9e7d10596c79a3dd7bdd5f833904a1ca300705e4d3b86b72c  docs/USER_MANUAL.txt
2778fdb9eef2f159d0258d4d6d21662f72c41e61554781ae1f41a69e09199dc5  install.sh
2d91dd40ea292c075bb7c1be7447c7a973d26487fa15a681d6c519f08829d738  lib/cmdfile.py
a43b82687847931284778968e942bce3eb6f8de639f7fceaf3ba7fc2f0e620c7  lib/hwlink/__init__.py
506ae91466f585796ad30083f4a1e84026d46a28c1f514789f507ad3b0623915  lib/hwlink/crc.py
5a9e1d7895566a74533e48525754e12094f1cf05a51b33f831137b9957a0c24c  lib/hwlink/diag/__init__.py
ececb355e287d388e1eab674d4af66dec697a99ee3ef02d66355476a7e8b1c87  lib/hwlink/diag/build.py
5c3d54409f43ec72b62599a0e2bf84680c2b5879a6ee8abc9f6418abb640a4f6  lib/hwlink/diag/dashboard.py
6321d61e2a900894ebbcdd43d2c9912b514b21c52f7ecb73a4bf96b7a3316499  lib/hwlink/diag/desc.py
011c226502aafe3bb7d79e82b6d5f70cfbd84aa91900c9230014a4db74a86f02  lib/hwlink/diag/fdcplan.py
9c1b0468d7e6d107892698ac9b1ed1a8f17f364ac436c99e5c312cc9de2177a5  lib/hwlink/diag/floppy.py
d414ba538cf19407184e2e91e7a5925071f9417256b8a8f8f631b1b20d884af5  lib/hwlink/diag/modfmt.py
d6f8099dc02ea32e9c3638503316faeb9987864eee9ba740360a0b4f242f872e  lib/hwlink/diag/modules.py
a4994f60b1949c167c7b71c6ecc06875ee9d4e74a4e89f2c342fe6b66041149b  lib/hwlink/diag/runner.py
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
f31a8662ff7539b6d35b318deabac650b471526ffe9a3e63b1dd98738deec1a6  lib/hwlink/tester/__init__.py
e0ca4782271c03af421600c6137a6dc1d9644ee61a00cb11e17fcc54ba0e53c6  lib/hwlink/tester/analyze.py
b8f53f9a629db360cae42d10449a39a7522fefe2a0327b056d9435558f3a067b  lib/hwlink/tester/app.py
c2df79a864231ad8c776f3cd26fef8904c1d93b10f77c25b58b443a8e6bcdd59  lib/hwlink/tester/bundle.py
112920bcfca08f105a43cab39ec95dbf920b9f24ee4dd5a1f17e6fdd535b0a8b  lib/hwlink/tester/catalog.py
d5e11588f4fef5d964f973d201ebb54d66fd1ae10635122f84e044f369531f69  lib/hwlink/tester/flow.py
a0b177d14944b50ebb72655f23ed3f6dd36457e6373982e163577f481b62d7b3  lib/hwlink/tester/media.py
30592f601eade5856fe3c805aac841c9d4d5101e04cbbeca10f4387261daa358  lib/hwlink/tester/report.py
7a0e2b19778da078d8652d5767ff94f7efaa1e12825621038e9c37902aa7cd4f  lib/hwlink/tester/serialport.py
c70fbef198c18bf1e43c86441eda1172c9c86df1a24f84d953708b1a686e6261  lib/hwlink/tester/ui.py
864fb6d4947f35cff40096f13406812eb9e5e28ca7254e94e8438a4ecedd87d0  lib/hwlink/tester/virtual4p.py
b72342a2fc2fd0caabb30c90cece2c6c7cd4756c42e9cbb39060bef83ceedbc1  lib/hwlink/transport.py
9937f9349c697e2598c500f8dfa99ce1f5b1ccc04bba91f54091101c0c9f288d  lib/track_profiles.py
9ef094de4add4739cdb2840d856026e731909d2527d0386858f384761f4d5ca2  lib/z80dis.py
31261268216a670a10ffb5a17bc43427f3f63b48ee65aec73dbd753379ff9b35  lib/z80sub.py
93976f7517e3d86b8a9be5cd9887a41c2c7bb6280ea99dee9f1b0331bd340182  mydos80-hwtest
f15468f35d3b94a6a583b9be117b94e8031dbee39d5db2eaef9cff7a6ce339b6  uninstall.sh
```
