# Rockbox iPod 5/5.5G Digital Audio — V-MODA VAMP VERZA

> **Experimental community build — not an official Rockbox release**

This branch contains a hardware-verified Rockbox build that outputs **digital audio from an iPod Video 5/5.5G through the 30-pin dock connector to a V-MODA VAMP VERZA DAC/headphone amplifier**.

## ✅ Hardware-Verified Result

Tested successfully on:

* iPod Video 5.5G

* 64 MB RAM

* Rockbox target: `ipodvideo`

* V-MODA VAMP VERZA

* 30-pin digital accessory connection

* Headphones connected to the VAMP VERZA

**Result: clean digital audio playback confirmed.**

The source was rebuilt from a clean Git checkout, installed on the iPod, and physically tested again with the VAMP VERZA.

## Download

**[Download the hardware-verified prerelease](https://github.com/jaredbsanchez-png/rockbox/releases/tag/diag21-verza-clean-digital-audio)**

Release file:

`rockbox-ipod5g-vamp-verza-digital-audio-verified.zip`

Source commit:

`0832987fa3cd5f488360983f4281a74e8f4c825c`

Rockbox build version:

`0832987fa3-260909`

SHA-256:

`48cd691014de7d71bf18ba63652ea4070cc711389f57f556b52f3c8af24f544f`

## Signal Path

```text

Music file

&#x20;   ↓

Rockbox decoder / DSP

&#x20;   ↓

USB Audio source

&#x20;   ↓

iPod 30-pin dock connector

&#x20;   ↓

V-MODA VAMP VERZA

&#x20;   ↓

VAMP DAC / headphone amplifier

&#x20;   ↓

Headphones

```

The VAMP VERZA receives the digital audio stream and performs the digital-to-analog conversion.

## Why Rockbox Changes Were Needed

Old Apple digital-audio accessories do more than simply accept USB audio.

The iPod and accessory first communicate using Apple's iPod Accessory Protocol (iAP). The accessory then negotiates digital-audio capabilities and enables the USB Audio streaming interface.

The verified implementation includes work involving:

* iAP / IDPS accessory negotiation

* Digital Audio Lingo `0x0A`

* transaction-ID handling

* sample-rate negotiation

* USB Audio Class source mode

* ARC USB isochronous transfers

* SOF-driven batched ISO audio delivery

The working source is represented by commit:

`0832987fa3`

# Beginner Installation Guide

## Before You Begin

Your iPod Video should already have the **Rockbox bootloader installed**.

This ZIP contains the Rockbox firmware files. It does not perform the initial Rockbox bootloader installation.

Back up your existing `.rockbox` folder before testing this build.

## 1. Download the Verified ZIP

Download:

`rockbox-ipod5g-vamp-verza-digital-audio-verified.zip`

from the release page linked above.

## 2. Verify the ZIP

In Windows PowerShell:

```powershell

Get-FileHash ".\\rockbox-ipod5g-vamp-verza-digital-audio-verified.zip" -Algorithm SHA256

```

Expected SHA-256:

```text

48cd691014de7d71bf18ba63652ea4070cc711389f57f556b52f3c8af24f544f

```

If it does not match, do not install that file.

## 3. Connect the iPod

Connect your iPod to Windows and find its drive letter.

The examples below use:

`G:`

Your drive letter may be different.

## 4. Back Up Your Existing Rockbox Folder

Example:

```powershell

Rename-Item -LiteralPath "G:\\.rockbox" -NewName ".rockbox-before-verza-backup"

```

This renames your existing installation rather than deleting it.

## 5. Extract the Verified Build

```powershell

Expand-Archive -LiteralPath "C:\\path\\to\\rockbox-ipod5g-vamp-verza-digital-audio-verified.zip" -DestinationPath "G:\\" -Force

```

After extraction, the iPod should contain:

```text

G:\\.rockbox

```

## 6. Confirm the Installed Version

```powershell

Get-Content "G:\\.rockbox\\rockbox-info.txt" | Select-String "Version"

```

The verified build reports:

```text

Version: 0832987fa3-260909

```

## 7. Test the VAMP VERZA

1\. Safely eject the iPod.

2\. Boot Rockbox normally.

3\. Connect the V-MODA VAMP VERZA through the 30-pin connection.

4\. Connect headphones to the VAMP.

5\. Play a track in Rockbox.

On the tested iPod 5.5G configuration, digital audio plays correctly through the VAMP VERZA.

# Restore Your Previous Rockbox Installation

Reconnect the iPod and rename the test build:

```powershell

Rename-Item -LiteralPath "G:\\.rockbox" -NewName ".rockbox-verza-test"

```

Restore your backup:

```powershell

Rename-Item -LiteralPath "G:\\.rockbox-before-verza-backup" -NewName ".rockbox"

```

Safely eject and reboot.

# Build From Source

The verified source commit is:

`0832987fa3cd5f488360983f4281a74e8f4c825c`

Example clean build:

```bash

git checkout 0832987fa3

mkdir build-verza

cd build-verza

../tools/configure --target=ipodvideo --ram=64 --type=n

make -j$(nproc)

make zip

```

The physical verification described here used a 64 MB iPod Video 5.5G.

# Compatibility Status

| Hardware                                  | Status                    |

| ----------------------------------------- | ------------------------- |

| iPod Video 5.5G, 64 MB                    | ✅ Tested                  |

| V-MODA VAMP VERZA                         | ✅ Digital audio confirmed |

| Other vintage iPod digital DACs and docks | ⚠️ Not yet verified       |

Other accessories may use similar Apple accessory protocols, but universal compatibility should **not** be assumed until tested on real hardware.

# Reporting Results

If you test another accessory, please open a GitHub Issue and include:

* exact iPod model

* RAM capacity if known

* accessory or DAC model

* cable/connection used

* whether the accessory was detected

* whether playback started

* whether audio was clean, distorted, intermittent, or silent

* relevant Rockbox log files if available

# Project Status

**iPod Video 5.5G → Rockbox → 30-pin digital audio → V-MODA VAMP VERZA has been rebuilt from committed source and physically verified working.**

Further work can focus on testing additional accessories and integrating later Rockbox improvements without regressing the verified VAMP handshake.

## Credits

Built on the work of the [Rockbox](https://www.rockbox.org/) project and its contributors.

This repository is a community development fork and is not affiliated with or endorsed by Apple, V-MODA, or the official Rockbox project.

## License

Rockbox is distributed under the GNU General Public License v2.
