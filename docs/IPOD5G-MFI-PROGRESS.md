# iPod 5/5.5G MFi / iAP Digital Audio Progress

## Project Goal

The goal of this experimental Rockbox branch is to investigate whether a 5th/5.5th-generation iPod Video running Rockbox can communicate with legacy Apple MFi/iAP USB audio accessories and eventually provide working digital audio output.

The primary test accessory is a V-MODA VAMP VERZA portable DAC/headphone amplifier.

This is experimental development work. Digital audio output is not yet working.

## Starting Point

This work builds on the existing Rockpod MFi development history from:

- `nuxcodes/rockpod`
- branch/release lineage culminating in `v6.0-alpha.0`
- commit `40b84ef9ec`
- message: `v6.0-alpha.0: Port MFi USB audio + IAP HID to iPod 5G (Video)`

The local development branch was originally named:

`mfi-5g-test`

For public development, the work is now maintained on:

`ipod5g-mfi-digital-audio`

## Current Experimental Commit

Commit:

`07fce0c763`

Message:

`ipod5g: advance MFi/iAP USB audio handshake diagnostics`

The commit modifies seven Rockbox source files:

- `apps/iap/iap-core.c`
- `apps/iap/iap-core.h`
- `firmware/target/arm/usb-drv-arc.c`
- `firmware/usbstack/usb_audio.c`
- `firmware/usbstack/usb_audio.h`
- `firmware/usbstack/usb_core.c`
- `firmware/usbstack/usb_iap_hid.c`

Current diff size:

- 180 insertions
- 38 deletions

## What Has Been Investigated

Development has focused on the interaction between:

1. Rockbox USB device handling
2. Apple iAP communication
3. HID SET_REPORT traffic
4. MFi accessory initialization
5. USB audio interface behavior
6. post-authentication transaction IDs
7. command ordering during accessory negotiation

The current iAP work includes an iPod-originated transaction ID counter:

`device.ipod_trans_id`

initialized for post-authentication communication, with helper logic in:

`iap_put_next_ipod_trans_id()`

This is currently used in several iAP command paths.

## HID / SET_REPORT Progress

One important area of investigation has been USB HID `SET_REPORT` processing in:

`firmware/usbstack/usb_iap_hid.c`

Diagnostic builds have been used to determine whether accessory-originated HID reports are reaching Rockbox and whether Rockbox processes them in the expected order.

The current diagnostic line of development reached a build referred to during testing as:

`diag19-processfirst`

Logs from the device show that `SET_REPORT` processing is occurring, giving us evidence that the accessory and iPod are communicating beyond simple USB enumeration.

This is significant because the failure is no longer being treated as merely "the accessory is not detected."

## Audio Behavior

At the current stage:

- Rockbox boots successfully.
- Music files play normally from Rockbox's perspective.
- The playback timer advances.
- The VAMP VERZA can be connected to the iPod.
- iAP/HID communication activity can be observed in diagnostic logging.
- Digital audio has not yet been heard through headphones connected to the VAMP VERZA.

In other words, control/handshake traffic has progressed further than actual audio delivery.

## Current Working Hypothesis

The remaining problem appears to be somewhere after or during MFi/iAP accessory negotiation and before successful USB digital audio streaming.

Areas still under investigation include:

- ordering of iAP commands
- transaction ID handling
- HID report processing order
- accessory mode transitions
- USB audio interface activation
- USB endpoint behavior
- synchronization between iAP negotiation and audio startup

It is not yet known which of these is the final blocker.

## Important Distinction

This branch should not currently be described as providing working iPod 5G MFi digital audio.

A more accurate description is:

> Experimental Rockbox work that advances iPod 5/5.5G communication with legacy MFi/iAP USB audio accessories, with active investigation of the remaining digital-audio path.

## Test Hardware

Current primary test configuration:

- Apple iPod Video 5/5.5G
- Rockbox / Rockpod experimental MFi build
- V-MODA VAMP VERZA DAC/headphone amplifier
- headphones connected to the VAMP VERZA

## Development Philosophy

Changes are being tested incrementally.

Diagnostic builds are intentionally used to isolate specific stages of the handshake rather than making many unrelated changes at once.

Logs and device behavior are used to determine the next modification.

## Next Objectives

The immediate goals are:

1. Identify the exact iAP command sequence immediately before audio should become available.
2. Determine whether the VAMP VERZA acknowledges the required mode transition.
3. Verify that Rockbox enables the intended USB audio path at the correct point.
4. Confirm whether audio samples are actually reaching the USB endpoint.
5. Reduce diagnostic instrumentation once the failure point is isolated.
6. Produce a reproducible test build for other iPod 5/5.5G owners.

## Status

**Experimental — digital audio not yet working.**

The project has progressed from basic compatibility investigation to active iAP/HID handshake debugging with reproducible device logs and a dedicated public development branch.
