# Black Dragon
### Reverse Engineering the ZTE ZXV10 B886V2F
#### Development Log 1 - Full Research Archive (Development Cycle 1: Reconnaissance through MaskROM, archived)

---

## Project Information

| Field | Value |
|---|---|
| Project | Black Dragon |
| Repository | Networking-Dragons |
| Author | Herthtle Cassidy M. Valdez |
| Start Date | August 2, 2026 |
| Status | Archived - pivoted to Emerald Dragon (see Results/Discussion) |

---

## Abstract

In this project, I investigate the possibility of converting a decommissioned Converge ZTE ZXV10 B886V2F IPTV box into a network firewall by replacing or modifying its stock firmware with a Linux-based operating system such as OpenWrt. The device will be positioned between the main router and "White Dragon" (a previously repurposed wired repeater built from a decommissioned Converge router), forming the second entry in the Networking-Dragons series. I document every step of the reverse engineering process, regardless of whether final firmware installation succeeds.

---

## Objectives

**Primary Objective**
- Identify the exact SoC, storage, and debug interfaces on the B886V2F board
- Determine whether an alternative Linux-based OS (OpenWrt, Armbian, or a stepping-stone like CoreELEC) can boot on this hardware
- If successful, configure the device as a firewall between the main router and the White Dragon repeater

**Secondary Objectives**
- Document the reverse-engineering process in enough detail to be reproducible by others with the same or sibling ZTE STB models
- Build reusable knowledge for the wider "Networking-Dragons" repository
- Practice hardware teardown, UART/debug discovery, and embedded Linux bring-up skills on cheap/free hardware

---

## Background

| Item | Value |
|---|---|
| Manufacturer | ZTE Corporation |
| Model | ZXV10 B886V2F |
| Marketing Name | Converge VISION Xperience Box / Xperience Hub |
| ISP | Converge ICT |
| Original Purpose | IPTV / Android TV Set-Top Box |
| Release Context | Marketed under Converge ICT / Pacific Kabelnet, Philippines; code name B866V2F / PKN_B866V2F |
| Known Hardware | Amlogic S905Y4 SoC (confirmed, see Software Investigation) |
| Known Firmware | ZTE Android TV firmware, V8611139.1029 |
| Known Community Info | Sibling model ZTE B860H has active hobbyist community (rooting, custom firmware, USB Burning Tool re-flashing) documented in Indonesian-language tutorials; no community documentation found yet specific to B886V2F/B866V2F |

> **Correction (logged later, during OS-compatibility research):** B860H's OpenWrt/Armbian community support was initially treated as a promising precedent for this project. On closer inspection of the ophub supported-device table, **B860H is built on Amlogic S905X - a different, older chip than our confirmed S905Y4.** That precedent does not transfer directly. Logged here rather than silently corrected, per project ground rules of documenting mistakes as well as successes.

**Key finding - device family identification:** The B886V2F is **not** a router-class device (i.e. not in the same family as ZTE's ZXV10 H-series DSL/fiber routers that OpenWrt's Table of Hardware documents). It is an **Android TV set-top box built on an Amlogic SoC**. While OpenWrt officially supports numerous ZTE ZXV10 H-series DSL and fiber routers, there is limited public documentation regarding support for the ZTE ZXV10 B886V2F IPTV device specifically.

Evidence trail (chronological, as discovered):
1. The Converge VISION Xperience Box (V2) is catalogued under the ZTE **B866V2F** model - DeviceAtlas device database: https://deviceatlas.com/device-data/devices/converge/b866v2f/87278940
2. ZTE's own product pages describe the B866-series STBs as Android TV boxes with Cortex-A35/A53-class Amlogic silicon - B866V2K datasheet: https://manuals.plus/m/f3dbf9352eb4d328650163d5b538de7d7f5e9a26b404ec1968d527cf2ba75cd6 - and the B866V2-H product page: https://www.zte.com.cn/global/home_and_enterprise/smart_home/zxv10_b866v2_h.html *(Caveat: these are close sibling models, not the exact B886V2F/B866V2F unit in hand - family resemblance inferred, not stated directly by ZTE for this SKU.)*
3. A sibling model marketed in Indonesia, the B866V2FA, is explicitly documented as running an Amlogic quad-core 64-bit chip - Kompas Tekno: https://tekno.kompas.com/read/2024/03/18/12020027/zte-dan-moratelindo-hadirkan-stb-android-tv-4k-untuk-pelanggan-oxygen.id
4. **Direct confirmation for the exact model code (B866V2F / PKN_B866V2F):** Android TV Guide lists this exact code name with SoC **Amlogic S905Y4**, quad-core 2.0GHz Cortex-A35, Mali-G31 MP2 GPU, 2GB RAM, 32GB storage - https://www.androidtv-guide.com/pay-tv-provider/converge-ict-xperience-hub/ (fetched and confirmed directly, not a cousin-model inference)

**Practical implication:** the install/flash path is the Amlogic Android-TV-box community ecosystem (SD/USB boot, "Amlogic s9xxx" community kernel/OpenWrt builds, CoreELEC generic device trees), not the router-style U-Boot/TFTP flashing process documented for ZTE's H-series routers.

---

## Research Questions

- Can Developer Options / USB debugging be unlocked through normal Android settings? *(Answer so far: No - see Experiment Log 001)*
- Can the bootloader be accessed, and is it locked to signed ZTE images only? *(More strongly suspected yes, after Experiment 005 - boot-from-USB showed no engagement at all)*
- Does the device support SD or USB boot at all, or is it Android-only by OTT operator policy? *(Standard reset+power method failed - Experiment 005. Alternate entry methods and burn-mode still to be tried)*
- Can the flash memory (eMMC) be dumped/backed up before any modification?
- Can a generic Amlogic S905Y4 Linux image (CoreELEC, Armbian, or OpenWrt s9xxx build) boot on this exact board?
- Can the Ethernet interface be supplemented (via USB NIC) to support a two-interface firewall topology?
- Is there an accessible UART header for serial console access?

---

## Hardware Inspection

| Item | Value |
|---|---|
| Manufacturer | ZTE Corporation |
| Model | ZXV10 B886V2F |
| ISP | Converge ICT |
| Original Purpose | IPTV / Android TV Box |
| Power Supply | DC power cable jack, 12V === , 1.0A |
| Ethernet Ports | 1 |
| USB Ports | 2 (confirmed side-by-side on board, contradicts published spec sheet of "1×") |
| HDMI | Yes |
| Reset Button | Yes |
| MicroSD Slot | No |
| Current Boot State | Boots normally into Android / Converge Xperience UI |

---

## Internal Hardware Analysis

**Disassembly:** Chassis opened, PCB removed, both sides photographed.

**Board (component side, first photo):**
- Board silkscreen text visible: `STBAWA-211000`, `36884-T9AWA` (matches hardware version string reported by Android)
- Rectangular gull-wing package near the barcode label identified as **eMMC storage**, brand **SCY** (Shenzhen Shichuangyi Electronics Co., Ltd) - a Chinese memory manufacturer whose product line includes embedded eMMC chips commonly used in budget consumer electronics. Exact capacity/part number not yet read (need macro shot of chip top text, likely a string like `E256CSAGxxxx`)
- Oval 6-pad cluster, top-right - plausible debug/test header candidate
- Vertical row of 4 through-holes near small QFP chip, left side - plausible small header, could also be a mounting/ground point. Not yet verified
- Dense multi-pin connector, bottom-right - more likely an internal ribbon/board-to-board connector (front panel or tuner board) than a debug interface

**Board (reverse side, SoC-facing photo):**
- Main SoC located: large chip under a metal heatsink/thermal-pad cover, center of board - die markings not yet visible, cover not yet removed (hard to remove, treat as last resort - risk of package damage)
- Two USB-A ports confirmed side-by-side
- HDMI port, power jack, and what appears to be a small SPI/serial flash IC (partial marking, "REF LM...") visible near top-left, separate from the main eMMC
- PCB fab marking: `ASKPCB 94V-0` (PCB manufacturer, not device-specific)

**Component identification status:**

| Component | Status |
|---|---|
| CPU/SoC | Located (under heatsink); identified via software/spec sources as Amlogic S905Y4 - die marking not yet visually confirmed |
| RAM | Not yet identified by chip marking (spec sources say 2GB DDR) |
| Flash Storage | Identified - SCY-brand eMMC, capacity/part number not yet read |
| WiFi Chip | Not yet identified by chip marking |
| Ethernet Controller | Not yet identified by chip marking |
| Power Management IC | Not yet identified by chip marking |
| UART Header | Not confirmed - 2 candidate footprints flagged, need continuity check |
| JTAG Header | Not yet located |
| Unknown Components | Several small ICs and a shielded module (likely WiFi/BT combo) not yet identified |

---

## Software Investigation

| Item | Value |
|---|---|
| OS | Android 14 |
| Android Version | 14 |
| Kernel Version | `5.15.153-android14-11-maybe-dirty#1 Thu Jan 1 00:00:00 UTC 1970` |
| Hardware Version | `STBAWA-211000;ANDROIDSTB-07D` |
| ZTE Firmware Version | `V8611139.1029` (built Fri Apr 25 07:53:37 UTC 2023) |
| SoC (confirmed) | Amlogic S905Y4 |
| Internal Storage | Accessible |
| External Storage | Accessible |
| ADB Access | Not yet confirmed |
| Developer Options | **Locked - see Experiment Log 001** |
| SSH / Telnet | Not tested |
| Hidden Menus | Not tested |

---

## Bootloader Investigation

**Status:** Not yet directly probed. Key open question is whether this is a standard unlocked Amlogic bootloader or an OTT-operator-locked one that only accepts signed ZTE images. A CoreELEC forum thread on this exact SoC states plainly: "Some OTT providers implement security features that only allow Android to boot." Since this device was ISP-provisioned (Converge ICT), that risk applies directly and is the single biggest unknown blocking firmware replacement.

Planned investigation path (not yet executed):
- Attempt SD/USB boot with a generic Amlogic S905Y4 image (CoreELEC `s4_s905y4` generic build) as a low-risk test - does not touch eMMC, fully reversible
- If SD/USB boot is refused, investigate Amlogic "USB Burning Mode" / maskrom entry (button combo or eMMC pin short at power-on) - this is a boot-ROM-level recovery mode independent of Android lock state, standard entry point used by the wider Amlogic hobbyist community for exactly this class of device
- Secure boot / eFuse lock state: unknown, to be inferred from whether burn mode / SD boot succeeds

---

## Firmware Backup

Not yet performed. Planned once ADB or burn-mode access is available - full eMMC dump before any write operation, per project ground rules.

---

## Experiment Log

### Experiment 001
- **Objective:** Unlock Developer Options via Settings → About → tap Build Number 7×, to enable ADB/USB debugging as a non-invasive software recon path
- **Procedure:** Followed standard Android TV Developer Options unlock sequence
- **Result:** No response from repeated taps - Developer Options did not unlock
- **Success?** No
- **Problems Encountered:** Corroborated by third-party reports of the same lock behavior on similar Android TV STB hardware (r/AndroidQuestions thread: https://www.reddit.com/r/AndroidQuestions/comments/14ea3c9/how_else_can_i_open_the_developer_settings/) - suggests this is a deliberate OEM/OTT lockdown, not a device fault
- **Next Step:** Software-side ADB access appears closed off via standard means. Pivot toward hardware-level recovery mode (Amlogic USB Burning Mode) and/or low-risk SD/USB boot testing with a generic Amlogic image, rather than continuing to pursue in-Android unlock paths

### Experiment 002 (superseded - see Experiment 005 for actual result)
- **Objective:** Test whether the board will boot a generic CoreELEC image for Amlogic S905Y4 (`s4_s905y4_2g` or `_4g` device tree) from USB (no microSD slot present)
- **Procedure (planned):** Prepare bootable USB drive per CoreELEC Generic Devices instructions, matching device tree to confirmed SoC; attempt boot without modifying internal eMMC
- **Rationale:** S905Y4 has an official, actively maintained CoreELEC device tree - proven on the BuzzTV HD5 (same SoC) and reported working on other unrelated S905Y4 boxes (HAKO MiNi, Zgemma Y1, per CoreELEC forum threads). A successful boot would validate that the bootloader accepts unsigned/generic images - same underlying mechanism the OpenWrt s9xxx builds and Armbian-for-Amlogic rely on - without committing to any eMMC write
- **Result:** See Experiment 005 below for the actual attempt and outcome

### Experiment 003 - Checked ophub OpenWrt/Armbian device support directly
- **Objective:** Verify whether ophub/amlogic-s9xxx-openwrt (the canonical community OpenWrt-for-Amlogic project) or its sibling ophub/amlogic-s9xxx-armbian officially support the Amlogic S905Y4
- **Procedure:** Fetched the full README/Supported Devices table directly from both repos on GitHub, rather than relying on the shorter release-notes summary
- **Result:** **S905Y4 is absent from the supported SoC list in both projects.** OpenWrt's list: `a311d, s922x, s905x3, s905x2, s905l3a, s905l3b, s905l3, s912, s905d, s905x, s905mb, s905l, s905l2, s905lb, s905w, s905` (plus Rockchip/Allwinner chips). Armbian's list is identical in composition. A 2023 GitHub issue on the Armbian repo (#1605, "Amlogic S905Y4 support?") asked for exactly this and was never resolved with official support
- **Success?** No (negative result - but a decisive, useful one)
- **Problems Encountered:** This also corrected the earlier B860H precedent - see correction note in Background section above
- **Next Step:** Since neither project has ready-made S905Y4 support, CoreELEC (which does have an official `s4_s905y4` device tree) becomes the best available near-term option, pending Experiment 004's finding on a critical caveat

### Experiment 004 - Found a directly analogous case: Mecool KM7 (same SoC, same goal, partial failure)
- **Objective:** Look for any prior documented attempt to run non-Android Linux on an S905Y4-based Android TV box, ideally for a similar router/firewall/VPN-gateway use case
- **Procedure:** Web search and direct fetch of GitHub issue tracker for ophub/amlogic-s9xxx-armbian
- **Result:** Found **GitHub Issue #3508**, "No network on Mecool KM7 (Amlogic S905Y4) - help choosing correct Armbian image" - https://github.com/ophub/amlogic-s9xxx-armbian/issues/3508 (opened May 13, 2026 by user Vgutchin, status: open, unresolved as of this writing)
  - Device: Mecool KM7, SoC explicitly listed as **Amlogic S905Y4 (Meson S4)** - confirms "Meson S4" as the internal Linux codename for this chip
  - Stated goal: converting the Android TV box into a **VPN gateway running Linux** - nearly identical use case to Black Dragon
  - Tried a generic Debian build (`Devmfc_Debian-Trixie_6.18.28-meson64_Minimal`) via SD card: **no network interfaces worked at all** - no WiFi, no Ethernet, no USB-to-Ethernet adapters recognized
  - Their theory: Mecool used non-standard GPIO power routing for the network controllers; mainline device trees don't power those pins on correctly
  - Tried **CoreELEC** as a fallback: **partial** result - internal WiFi worked, but built-in Ethernet and USB-to-Ethernet adapters still did not function
  - Confirmed no S905Y4-specific image exists in ophub's Armbian releases; asked whether the Khadas VIM1S device tree (`meson-s4-s905y4-khadas-vim1s.dtb`) might be a usable starting point for building a proper one - no answer posted yet at time of reading
- **Success?** N/A (this was a research/precedent-finding task, not a hands-on experiment on our own board) - but the implication is a serious risk flag
- **Problems Encountered:** This is a documented, board-specific hardware quirk from someone chasing the identical use case on the identical chip family, unresolved even by the project maintainers. Whether the same GPIO routing problem exists on our exact Converge board is unknown - board-specific wiring can differ even within the same SoC - but it must now be explicitly tested for, not assumed away
- **Next Step:** When Experiment 002 (CoreELEC boot test) is finally run, it must explicitly verify working Ethernet **and** USB-Ethernet, not just "does it boot to a shell." A successful boot with dead networking would be the KM7 failure mode repeating

### Experiment 005 - Executed the CoreELEC `s4_s905y4` USB boot attempt (reset+power method)
- **Objective:** Actually carry out Experiment 002's planned test - attempt to boot CoreELEC from USB via the standard Amlogic reset+power boot-from-external-media method, independent of the locked Android Developer Options path (Experiment 001)
- **Procedure:**
  1. Downloaded CoreELEC Generic image, Amlogic-ne engine (correct engine for S905Y4/S905W2/A311D2/S905X4), via CoreELEC's official Download Helper
  2. Flashed the `.img.gz` to a USB flash drive using balenaEtcher
  3. Copied the matching `s4_s905y4` device tree to `dtb.img` at the root of the USB drive
  4. Powered the device off completely, inserted the prepared USB drive into one of the two USB-A ports
  5. Held the reset button (paperclip, recessed pinhole), plugged in DC power while still holding reset, held for ~5–10 seconds, released
  6. Observed HDMI output on a connected monitor
- **Result:** **Device booted normally into Android - straight into "SkyTV," Converge's branded media/IPTV launcher app.** No CoreELEC boot logo, no first-run wizard, no visible indication the USB drive was read at all. Behaved identically to a normal cold boot with nothing plugged into USB.
- **Success?** No
- **Problems Encountered:** This is a meaningfully different (and more concerning) failure mode than "boots CoreELEC but wrong DTB" - the bootloader showed no sign of attempting to read external media at all, rather than attempting and failing partway. This is consistent with, though not yet definitive proof of, the **OTT-operator bootloader lockdown** flagged as the top risk since Experiment 001 - the same class of restriction that blocked Developer Options may also disable the boot-from-USB code path entirely, rather than this being a DTB-mismatch or display/HDMI timing issue
- **New detail discovered:** The Android launcher/home app on this device is specifically branded **"SkyTV"** (Converge's IPTV/media front-end), not just a generic "Converge Xperience" UI as previously described in earlier log entries - more precise terminology for future reference and searches
- **Next Step:** Since the standard reset+power boot-from-USB trick showed no engagement at all, next avenues to investigate:
  - Try alternate button/timing combinations (some Amlogic boards use a different pinhole, a power+reset-simultaneously combo instead of reset-then-power, or require holding through a longer window than 5–10 seconds)
  - Investigate whether this board has a distinct "USB Burning Tool" / maskrom entry point separate from the boot-from-USB path - these are not always the same mechanism on OTT-locked boards; burn mode can sometimes still be reachable via a short-to-ground on a specific eMMC/flash pin even when boot-from-USB is disabled in u-boot
  - Revisit UART access as a way to actually see u-boot's boot log and confirm whether it's seeing the USB device at all, versus ignoring it entirely by policy - this is now the highest-value unresolved hardware task
  - Treat "bootloader is OTT-locked against unsigned boot media" as more strongly suspected, though not yet fully confirmed, pending the above

### Experiment 006 - MaskROM test point short attempt, and confirmation of kernel-level boot lockdown
- **Objective:** Attempt to force the SoC into MaskROM (burn mode) by locating and shorting the eMMC/SoC test points during power-on, bypassing the u-boot-level lockdown observed in Experiment 005 entirely
- **Procedure:** Located two test points on the board (near the SCY eMMC chip, referred to as Test Point 1 and Test Point 2) and shorted them during power-on per the standard Amlogic MaskROM-entry technique
- **Result:** **Partial success on entry, dead end on execution.** The short did put the SoC into a state where MaskROM-style low-level access was reachable - but no `.img` firmware file could be found anywhere in the community that the Amlogic USB Burning Tool would accept for this board. Working theory: the S905Y4 SoC is new enough that the modding/hobbyist community has not yet produced or leaked a compatible signed/accepted image for it, unlike older, long-established chips (S905X, S905X2, etc.) which have years of accumulated community firmware archives
- **Additional finding, this session:** Direct confirmation that the lockdown operates **at the kernel/bootloader level, not just Android's Developer Options layer** - the device was observed to deliberately boot SkyTV (Converge's IPTV launcher) to completion *before* ever checking for or reading an attached USB device, rather than checking USB first. This is a meaningful, board-specific confirmation beyond the general OTT-lockdown suspicion raised in Experiment 005 - it's not just "USB boot is disabled," it's "USB is checked, if at all, only after the OTT-mandated boot path has already completed."
- **Success?** No (MaskROM reachable, but no usable path forward from there)
- **Problems Encountered:** The core blocker is a missing device-specific/community-produced firmware image for S905Y4, not a hardware access failure - reaching MaskROM mode was itself a genuine reverse-engineering win, it just didn't lead anywhere useful yet given the current state of public tooling
- **Next Step:** Project pivoted - see Results/Discussion below

---

## Network Capabilities

| Interface | Status |
|---|---|
| Ethernet | 1× 10/100M RJ45 (per spec sheet, physical count not yet re-verified against board) |
| WiFi | 802.11 b/g/n/ac 2x2 MU-MIMO (per spec sheet) |
| Bluetooth | 5.0 (per spec sheet) |
| USB | 2× USB 2.0 Type-A confirmed on board |
| GPIO | Not yet assessed |
| Serial (UART) | Candidate headers identified, not confirmed |

**Possible Network Modes:** Firewall is the target mode. Single onboard Ethernet port is a hard constraint - a USB-to-Ethernet adapter will very likely be required to get the WAN+LAN interface count needed, contingent on the target kernel having a driver for whatever adapter chipset is used.

---

## OpenWrt / Alternative-OS Compatibility

| Item | Status |
|---|---|
| Supported SoC (CoreELEC) | Amlogic S905Y4 has an official generic device tree (`s4_s905y4_2g`/`_4g`), validated via the BuzzTV HD5 (same SoC) |
| Supported SoC (ophub OpenWrt) | **Not supported.** Full supported-SoC list confirmed via direct README fetch: `a311d, s922x, s905x3, s905x2, s905l3a, s905l3b, s905l3, s912, s905d, s905x, s905mb, s905l, s905l2, s905lb, s905w, s905` + Rockchip/Allwinner. S905Y4 absent (Experiment 003) |
| Supported SoC (ophub Armbian) | **Not supported** - same device list composition as OpenWrt; a 2023 request for S905Y4 support (Issue #1605) was never fulfilled (Experiment 003) |
| Device Tree | `s4_s905y4_2g` / `s4_s905y4_4g` exist for CoreELEC only; no equivalent confirmed for OpenWrt/Armbian s9xxx builds |
| Kernel Support | S905Y4 = internal codename "Meson S4," confirmed via Mecool KM7 issue report (Experiment 004) |
| Known Issues | (1) OTT-operator bootloader lockdown is unresolved - chip-level support does not guarantee this specific board will accept unsigned images. (2) **Networking hardware failure risk, confirmed analogous case:** Mecool KM7 (same SoC) reports non-functional Ethernet/USB-Ethernet on both a generic Debian build and CoreELEC, suspected non-standard GPIO power routing not covered by generic device trees (Experiment 004, Issue #3508) |
| Candidate OpenWrt repos | ophub/amlogic-s9xxx-openwrt (canonical upstream) - https://github.com/ophub/amlogic-s9xxx-openwrt; jaya0206/jaya-build - https://github.com/jaya0206/jaya-build; r4kasya/VendaOpenWRT-s90xx - https://github.com/r4kasya/VendaOpenWRT-s90xx; tekadom/openwrt-amlogic-s9xxx - https://github.com/tekadom/openwrt-amlogic-s9xxx; degidevops/stb-openwrt - https://github.com/degidevops/stb-openwrt (⚠️ these forks list sibling ZTE B860H as supported, but B860H is S905X-based - see correction note in Background - this precedent does not transfer to our S905Y4 board) |

---

## Next Steps (Phase 2 - Non-Invasive Software Recon)

- [x] Settings → About → tap Build Number 7× to unlock Developer Options - **attempted, failed (Experiment 001)**
- [ ] Check for USB debugging / Wireless debugging toggle via other means
- [ ] If ADB available, pull: `getprop ro.boot.hardware`, `getprop ro.build.fingerprint`, `cat /proc/cpuinfo`, `cat /proc/partitions`, `ls /dev/block/platform`, `ip link`
- [ ] Confirm actual number and names of network interfaces (eth0 / wlan0 / usb0)
- [ ] Check for OTG support on USB ports
- [ ] Take clean macro photo of main SoC top-marking (heatsink removal, last resort)
- [ ] Photograph silkscreen labels next to candidate header footprints
- [ ] Photograph bottom sticker, back panel (all ports), power adapter label
- [x] **Attempted:** Generic CoreELEC `s4_s905y4` USB boot test via reset+power method - **failed, booted normally to Android/SkyTV, no engagement with USB media at all (Experiment 005)**
- [ ] **New, high priority:** Try alternate button/timing combinations for boot-from-USB entry (different pinhole, simultaneous power+reset, longer hold windows)
- [ ] **New, high priority:** Investigate whether a distinct Amlogic "USB Burning Tool" / maskrom entry point exists separately from the boot-from-USB path - may still be reachable even if standard boot-from-USB is locked out
- [ ] **New, high priority:** Pursue UART access - the two candidate header footprints identified earlier become the top hardware task now, since seeing u-boot's actual boot log is the clearest way to confirm whether USB media is being ignored by policy or not detected at all
- [ ] **New:** If CoreELEC boot + networking eventually succeed - extract boot chain (u-boot/kernel/dtb) and prepare a Debian/Ubuntu ARM64 rootfs to swap in (Phase 3/4 Plan, Branch A)
- [ ] **New:** If networking fails under CoreELEC - begin comparing our board's GPIO wiring against the Khadas VIM1S `s905y4` device tree as a possible patching reference (Phase 3/4 Plan, Branch B)

---

## Results

**What worked:**
- Full device/SoC identification achieved through software fingerprinting and cross-referencing public sources (confirmed Amlogic S905Y4, "Meson S4" internally)
- eMMC storage chip identified (SCY brand)
- Two hardware test points located and successfully used to force the SoC into MaskROM mode - a genuine, non-trivial reverse-engineering achievement in its own right
- Confirmed the device has 2 physical USB-A ports (correcting the closest public spec sheet, which claimed only 1)
- Built a well-sourced, citation-backed evidence trail establishing exactly where and why the OpenWrt/Armbian/community-firmware path is currently blocked for this specific chip

**What failed:**
- Developer Options could not be unlocked through any standard Android method (Experiment 001)
- Boot-from-USB via the standard reset+power method showed zero engagement with attached media (Experiment 005)
- MaskROM entry succeeded physically, but no publicly available firmware image exists that the Amlogic USB Burning Tool would accept for this SoC revision (Experiment 006)
- Ultimately: no path to installing OpenWrt, CoreELEC, or any non-stock OS was found to be currently viable on this exact hardware

**Unexpected findings:**
- The device enforces its lockdown at the kernel/bootloader level, deliberately completing the SkyTV boot sequence before ever checking for attached USB media - a more deliberate design choice than a simple "Developer Options toggle disabled"
- S905Y4 is genuinely too new for the current state of the Amlogic hobbyist community's tooling - older sibling chips (S905X, S905X2, S905L3, etc.) have years of accumulated community firmware, while S905Y4 has essentially none yet
- A real, if scattered, hobbyist community exists specifically targeting the ZTE B866V2/B866V2F family (XDA thread, Indonesian-language B860H tutorials) - suggesting this may become more feasible in the future as that community's tooling catches up to this specific chip generation

---

## Discussion

The core finding of this project is that feasibility here was gated less by the reverse-engineer's skill or effort and more by **the current maturity of public tooling for a specific, fairly new SoC.** Every individual technique attempted - Developer Options unlock, boot-from-USB, MaskROM entry - is a well-established, community-proven method for Amlogic-based Android TV boxes in general. Each one worked exactly as documented up to the point where it needed either a signed image the device would accept, or a community-produced image for S905Y4 specifically, and neither currently exists in public circulation. This is a meaningfully different outcome than "the device is unhackable" - it's closer to "the device is ahead of the current community's tooling," which is a timing problem, not a permanent one.

Compared to the sibling B860H (Amlogic S905X), which has years of mature community firmware, custom ROMs, and even OpenWrt/Armbian support via the ophub project, the B886V2F's newer S905Y4 chip sits in a gap: modern enough to lack legacy community support, but not yet popular/old enough to have accumulated its own. This mirrors a common pattern in embedded/IoT reverse-engineering - support tends to arrive in waves, tied to when a chip becomes cheap and common enough for hobbyists to acquire in volume.

Given this, converting the hardware to a media server (Plex via the Play Store) is a pragmatic and appropriate use of the device as it exists today - it uses the hardware's actual strengths (Android TV app ecosystem, HDMI output, hardware video decode) rather than fighting its intentional restrictions, while preserving everything already learned for a possible future attempt.

---

## Future Work

- Revisit this exact hardware if/when the Amlogic community (ophub, CoreELEC, or others) adds S905Y4 support, or if a `password.bin`/signed image for this specific board surfaces via the XDA B866V2 community thread
- Follow up directly with the XDA poster who claims to have unlock methods specific to the B866V2F
- UART access was never completed - still a worthwhile diagnostic exercise if revisited, purely for documentation completeness, even though it wouldn't bypass the signed-image requirement on its own.
- **Black Dragon project itself is archived but not deleted** - the hardware and research trail remain available if the landscape changes
- **New direction:** expand the Emerald Dragon media hub into a full media *server* by adding external storage - codenamed **Orange Dragon** - to provide terabyte-scale movie storage alongside Plex

---

## Successor Project Note - Emerald Dragon

Black Dragon, as a firewall project on this specific hardware, is archived as of this entry. The physical device has been repurposed and rebranded within the Networking-Dragons repository:

| Field | Value |
|---|---|
| New Project Name | Emerald Dragon |
| Hardware | Same physical unit (ZTE ZXV10 B886V2F / Converge Xperience Hub) |
| New Role | Media hub → media server (in progress) |
| Software | Stock Android TV firmware retained (never modified - MaskROM entry did not result in any writes); Plex installed via Google Play Store |
| Planned Addition | "Orange Dragon" - external/USB storage expansion for terabyte-scale media library |
| Relationship to Black Dragon | Same hardware, different mission - firewall role may be revisited on this or different hardware in the future per Future Work above |

---

## References

- OpenWrt Table of Hardware (context: why B886V2F doesn't fit the router-class ZXV10 H-series documentation)
- ophub/amlogic-s9xxx-openwrt - https://github.com/ophub/amlogic-s9xxx-openwrt
- jaya0206/jaya-build - https://github.com/jaya0206/jaya-build
- r4kasya/VendaOpenWRT-s90xx - https://github.com/r4kasya/VendaOpenWRT-s90xx
- tekadom/openwrt-amlogic-s9xxx - https://github.com/tekadom/openwrt-amlogic-s9xxx
- degidevops/stb-openwrt - https://github.com/degidevops/stb-openwrt
- CoreELEC Wiki - Supported Devices: https://wiki.coreelec.org/coreelec:devsupported
- CoreELEC Wiki - Generic Devices: https://wiki.coreelec.org/coreelec:devgeneric
- CoreELEC Device Trees list (S905Y4 confirmed via BuzzTV HD5 entry): https://coreelec.org/dtb/
- CoreELEC Forums - "Is it fully supported? Amlogic S905Y4" (OTT lockdown risk quote): https://discourse.coreelec.org/t/is-it-fully-supported-amlogic-s905y4/53233
- DeviceAtlas - Converge B866V2F entry: https://deviceatlas.com/device-data/devices/converge/b866v2f/87278940
- ZTE B866V2K datasheet (manuals.plus): https://manuals.plus/m/f3dbf9352eb4d328650163d5b538de7d7f5e9a26b404ec1968d527cf2ba75cd6
- ZTE B866V2-H product page: https://www.zte.com.cn/global/home_and_enterprise/smart_home/zxv10_b866v2_h.html
- Android TV Guide - Converge ICT Xperience Hub (exact model, confirmed S905Y4): https://www.androidtv-guide.com/pay-tv-provider/converge-ict-xperience-hub/
- Kompas Tekno - ZTE/Moratelindo B866V2FA coverage: https://tekno.kompas.com/read/2024/03/18/12020027/zte-dan-moratelindo-hadirkan-stb-android-tv-4k-untuk-pelanggan-oxygen.id
- r/AndroidQuestions - Developer Options locked on similar STB hardware: https://www.reddit.com/r/AndroidQuestions/comments/14ea3c9/how_else_can_i_open_the_developer_settings/
- Sibling-model (ZTE B860H) hobbyist tutorials - USB Burning Tool re-flash, rooting via UART: haimalang.com root guide, various YouTube tutorials (Indonesian-language community)
- ophub/amlogic-s9xxx-openwrt README (full supported-device table, fetched directly): https://github.com/ophub/amlogic-s9xxx-openwrt
- ophub/amlogic-s9xxx-armbian - GitHub Issue #1605, "Amlogic S905Y4 support?" (unresolved, 2023): https://github.com/ophub/amlogic-s9xxx-armbian/issues/1605
- ophub/amlogic-s9xxx-armbian - **GitHub Issue #3508**, "No network on Mecool KM7 (Amlogic S905Y4) - help choosing correct Armbian image" (open, unresolved, May 2026) - key risk citation, same SoC, same VPN-gateway use case: https://github.com/ophub/amlogic-s9xxx-armbian/issues/3508
- XDA Forums - "How do I get the firmware that brings ZTE ZXV10 B866V2 for backup" - active community thread covering B866V2, **B866V2F**, and B826C unlock attempts; references to MaskROM, `password.bin` authentication requirement, and a poster claiming full temporary unlock methods for this exact model family: https://xdaforums.com/t/how-do-i-get-the-firmware-that-brings-zte-zxv10-b866v2-for-backup.4447749/
- CoreELEC AML USB Burning Tool guide (general Amlogic burn-mode/reset-button reference used for Experiment 006 procedure): https://wiki.coreelec.org/coreelec:aml_usb_tool

---

## Appendix

**Photos on file (not embedded in this doc):**
- Board component-side, full view (first PCB photo)
  
  <img width="1536" height="2048" alt="image" src="https://github.com/user-attachments/assets/fe3fdaf4-9c9d-43f2-8ae6-e71936796d01" />

- Board SoC-side, full view (second PCB photo, heatsink visible)
  
  <img width="1536" height="2048" alt="image" src="https://github.com/user-attachments/assets/8f12c3d7-97f8-414a-b520-caed64a3dbae" />

- Bottom sticker, back panel, power adapter label - pending per original mission checklist
  
  <img width="2048" height="1536" alt="image" src="https://github.com/user-attachments/assets/ddaff318-aa99-413c-ae19-6d1c36102a07" />
  
- Just a few photos from Before and After
  <img width="2048" height="1536" alt="image" src="https://github.com/user-attachments/assets/6ea3bf81-6fd6-4464-9823-ef5f6b121b8f" />
  
  <img width="2048" height="1536" alt="image" src="https://github.com/user-attachments/assets/7993361c-3900-4294-bcd4-9c8032a0c3f2" />
  
  <img width="2048" height="1536" alt="image" src="https://github.com/user-attachments/assets/c70c5315-705e-444f-a999-b90df0495404" />
  
  <img width="2048" height="1536" alt="image" src="https://github.com/user-attachments/assets/df8204b6-b0fd-4506-9dd5-c327863c3673" />

  <img width="2048" height="1536" alt="image" src="https://github.com/user-attachments/assets/736f8cdf-eb74-4aa6-b8cb-b899a1f72627" />




