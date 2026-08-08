# White Dragon

### Repurposing the Huawei EchoLife EG8145V5

#### Development Log 1 - Full Research Archive (Networking Dragons: Initial Access Point Project)

---

## Project Information

| Field                 | Value                                                                    |
| --------------------- | ------------------------------------------------------------------------ |
| Project               | White Dragon                                                             |
| Series                | Networking Dragons                                                       |
| Hardware              | Huawei EchoLife EG8145V5 GPON Terminal                                   |
| Original Manufacturer | Huawei                                                                   |
| Original ISP          | Converge ICT                                                             |
| Original Purpose      | GPON Optical Network Terminal / Home Gateway                             |
| New Role              | Wired Wireless Access Point                                              |
| Upstream Network      | PLDT Home Network                                                        |
| Acquisition           | Decommissioned unit found at home in Pampanga                            |
| Current Location      | Manila                                                                   |
| Start Context         | Initially intended for teardown and hardware inspection                  |
| Date Completed        | July 2026                                                                |
| Status                | Operational                                                              |
| Firmware              | Original ISP/Huawei firmware retained                                    |
| Firmware Replacement  | None                                                                     |
| Primary Outcome       | Successful conversion into a functional LAN-to-LAN wireless access point |

---

## Abstract

White Dragon is the first completed project in the Networking Dragons series.

The project began with a decommissioned Huawei EchoLife EG8145V5 GPON terminal originally supplied by Converge ICT. The device was initially discovered lying unused at our house in Pampanga. The original intention was not to repurpose the router, but simply to tear it down and inspect its internal hardware.

Upon inspection, however, the device appeared to remain operational. It powered on, its wireless system functioned, and the hardware showed no obvious catastrophic failure. The primary physical damage was limited to the antennas, which had been broken when the unit fell.

Rather than immediately dismantling an apparently functional networking device, the hardware was brought to Manila and placed on a desk for several weeks.

The project eventually began because there was a deadzone I wanted to fix in our downstairs, then the hardware was researched in greater detail. Research into the specific Huawei EchoLife EG8145V5 model revealed that the device contained networking functionality beyond what was initially exposed through the standard ISP administrator interface.

The original objective became the conversion of the GPON terminal into a wired wireless access point connected to an existing PLDT home network.

Initial configuration attempts appeared to fail because DHCP requests from wireless clients were not reaching the upstream PLDT router. This led to the temporary hypothesis that the ISP firmware had removed or disabled the Ethernet bridge functionality required for access-point operation.

Firmware replacement, OpenWrt, UART recovery, and reverse engineering were consequently considered as possible solutions.

The eventual breakthrough came from discovering that the ISP firmware contained a higher-privilege **Telecom Administrator** account: **Username: telecomadmin;Password: admintelecom** Logging into this account exposed previously hidden networking configuration options.

Using the higher privilege interface, the Huawei device was configured with a static management ip address, DHCP was disabled, the secondary LAN address was disabled, all four LAN ports were enabled, and the device was connected to the PLDT router from upstairs to downstairs, using a LAN-to-LAN connection.

Upon restarting the device, the result was a fully functional wireless access point using the original firmware.

Performance testing in a previously weak WiFi area demonstrated a significant improvement over connecting directly to the primary PLDT router.

White Dragon therefore achieved its primary objective without firmware replacement, demonstrating that the apparent functionality gap was caused by configuration and privilege restrictions rather than an inherent hardware limitation.

---

## Objectives

### Primary Objective

* Repurpose a decommissioned Huawei EchoLife EG8145V5 GPON terminal instead of allowing it to become electronic waste
* Convert the device into a functional wired wireless access point
* Integrate the Huawei unit into an existing PLDT home network
* Preserve the original firmware if possible
* Restore the physical condition of the hardware sufficiently for continued use

### Secondary Objectives

* Investigate the actual capabilities of the EG8145V5 hardware
* Determine whether ISP firmware restrictions prevented normal access-point operation
* Identify hidden administrative functionality
* Practice systematic embedded-networking troubleshooting
* Document the process in a reproducible engineering-log format
* Explore whether firmware replacement would be necessary
* Establish White Dragon as the first completed Networking Dragons project
* Extend the useful life of otherwise decommissioned networking hardware

---

## Project Origin

White Dragon did not begin as a planned networking project.

The hardware was originally discovered lying around the house in **Pampanga**. At the time, it was simply an old, decommissioned Converge-issued Huawei router.

The initial plan was straightforward:

> Tear it down and see what is inside for educational purposes.

The device was therefore initially regarded as an interesting piece of discarded hardware rather than as a potentially useful networking platform.

Before dismantling it, however, the unit was powered on and inspected.

Unexpectedly, it appeared to work.

The power system functioned, the WLAN indicators operated, and the device otherwise behaved like a functioning router. The primary visible physical damage was concentrated around the antennas, which had been broken when the router fell.

This changed the initial plan.

Instead of immediately dismantling a potentially functional device, the router was brought to **Manila** for further consideration.

The hardware subsequently remained on the desk for several weeks.

Eventually, the decision was made to investigate the exact hardware model and determine what the device could actually do.

That research became the beginning of White Dragon.

---

## Initial Assessment

The router was originally supplied by **Converge ICT**.

### Initial Physical/Functional Observations

* Device powered on normally.
* WLAN LEDs functioned.
* Power LED functioned.
* LOS indicator blinked red.
* Red LOS behavior was expected because the optical fiber was no longer connected.
* Hardware showed no immediate evidence of catastrophic failure.
* Antennas were physically damaged from the previous fall.
* Factory administrator credentials initially failed.

The most important early observation was that the router was not simply dead hardware.

The optical subsystem could no longer register with the ISP network, but the rest of the device appeared functional.

This distinction became important later: **loss of GPON service did not necessarily mean loss of useful networking functionality.**

---

## Hardware Background

| Item                  | Value                                               |
| --------------------- | --------------------------------------------------- |
| Manufacturer          | Huawei                                              |
| Model                 | EchoLife EG8145V5                                   |
| Original ISP          | Converge ICT                                        |
| Original Device Class | GPON Terminal / Home Gateway                        |
| New Intended Role     | Wireless Access Point                               |
| Optical Interface     | GPON                                                |
| Ethernet              | 4 LAN ports enabled for final configuration         |
| Wireless              | Operational                                         |
| Antennas              | Physically damaged initially; subsequently repaired |
| Management            | Web-based Huawei interface                          |
| Firmware              | Original ISP/Huawei firmware retained               |
| Replacement Firmware  | None                                                |
| Physical Condition    | Restored and operational                            |

### Hardware Identification Status

| Component / Feature | Status                                     |
| ------------------- | ------------------------------------------ |
| Main SoC            | Not documented in current research archive |
| RAM                 | Not documented                             |
| Flash Storage       | Not documented                             |
| WiFi chipset        | Not documented                             |
| Ethernet chipset    | Not documented                             |
| GPON chipset        | Not documented                             |
| UART                | Not documented           |
| PCB                 | Not yet fully documented                   |
| Thermal design      | Not yet documented                         |
| Antenna system      | Physically repaired                        |
| LAN ports           | Operational                                |
| WiFi                | Operational                                |

**Note:** Detailed semiconductor identification was not required to complete the access-point conversion and therefore was not performed during the initial project cycle.

---

## Research Questions

The project gradually expanded into a number of technical questions:

* Is the hardware still functional after decommissioning?
* Can the original administrator interface be accessed?
* Does a factory reset restore usable administrative credentials?
* Can the device function independently without GPON registration?
* Can the LAN interfaces be used as a conventional Ethernet switch?
* Can the wireless interface operate independently of the optical WAN connection?
* Does the ISP firmware contain hidden or higher-level administrator accounts?
* Can the device operate as a wired wireless access point?
* Is firmware replacement actually necessary?
* Would OpenWrt or another Linux-based system be required if the stock firmware proved insufficient?
* Can the device be physically restored sufficiently for long-term use?
* What performance can the repurposed hardware provide compared with the primary PLDT router?

---

# Phase 1 - Restoring Administrative Access

## Experiment 001 - Initial Administrator Login

### Objective

Determine whether the credentials associated with the decommissioned router could be used to access the Huawei management interface.

### Procedure

Attempted to log into the router using administrator credentials associated with the device.

### Result

The initial administrator credentials failed.

### Success?

**No.**

### Problems Encountered

The device appeared functional, but administrative access was not immediately available.

This created uncertainty over whether:

* the credentials had been changed;
* the ISP had modified the configuration;
* the device had not actually been reset;
* or additional ISP restrictions were present.

### Next Step

Perform a complete factory reset and attempt to restore default credentials.

---

## Experiment 002 - Factory Reset

### Objective

Restore the Huawei router to factory configuration and determine whether the default administrator credentials become valid again.

### Procedure

Multiple factory-reset attempts were performed.

During the final attempt, the physical reset button was held for a significantly longer period than during the earlier attempts.

### Result

Administrator access was successfully restored using the default credentials.

### Success?

**Yes.**

### Finding

The successful reset confirmed that the device could return to a factory configuration state.

The issue with the original credentials was therefore not evidence of hardware failure.

### Next Step

Connect the router independently and begin network-level investigation.

---

# Phase 2 - Initial Network Investigation

## Experiment 003 - Independent Network Test

### Objective

Verify that the router's basic networking functions remained operational independently of the Converge optical network.

### Procedure

The Huawei router was connected independently of the existing PLDT network.

Linux Mint networking tools were used to inspect the network state.

Commands used included:

```text
ip addr
ip route
ping
```

### Result

The following functions were confirmed:

* WiFi broadcasting correctly
* Ethernet interfaces operational
* Router management interface accessible

### Success?

**Yes.**

### Finding

The device remained functional as a local networking platform despite its decommissioned GPON role.

This was the first major indication that the hardware could potentially be repurposed.

### Next Step

Investigate the Huawei firmware and determine which networking functions remained exposed.

---

# Phase 3 - Firmware Investigation

## Experiment 004 - Management Interface Survey

### Objective

Determine whether the stock firmware already contained the configuration options required for access-point operation.

### Procedure

The Huawei web management interface was systematically inspected.

Major configuration areas examined included:

* WLAN
* LAN
* WAN
* DHCP
* Security
* Diagnostics

### Result

The WAN configuration appeared empty.

At this stage, the available interface did not clearly expose a conventional Ethernet bridge configuration.

### Success?

**Partial.**

The firmware was accessible, but the required access-point functionality was not obvious.

### Initial Interpretation

It was initially believed that the ISP firmware may have removed, disabled, or hidden the Ethernet bridge functionality required to repurpose the device.

This became the working hypothesis for the next phase.

### Next Step

Attempt a conventional LAN-to-LAN access-point configuration.

---

# Phase 4 - Wired Access Point Conversion

## Experiment 005 - Initial Access Point Configuration

### Objective

Determine whether the Huawei router could operate as a conventional access point using its existing firmware.

### Intended Network Topology

```text
                 Internet
                    │
                    │
             ┌──────────────┐
             │ PLDT Router  │
             │ DHCP / WAN   │
             └──────┬───────┘
                    │
                 Ethernet
                    │
             ┌──────┴───────┐
             │ White Dragon │
             │ EG8145V5     │
             │ Access Point │
             └──────┬───────┘
                    │
              WiFi / Ethernet
                    │
                 Clients
```

### Procedure

The following changes were attempted:

* Disabled DHCP Server.
* Assigned a static management IP.
* Connected LAN to LAN.
* Verified Ethernet link LEDs.
* Confirmed that the Huawei management interface remained reachable from the PLDT network.

### Result

Management access worked correctly.

Wireless clients were able to associate with the Huawei wireless network.

However, clients did not receive valid DHCP leases.

Instead, clients received APIPA addresses:

```text
169.254.x.x
```

### Success?

**Partial / No.**

The physical LAN-to-LAN topology worked, but client network configuration did not.

### Problems Encountered

Wireless clients were connecting successfully but were not receiving IP addresses from the upstream PLDT router.

This indicated that DHCP broadcast traffic was not successfully reaching the primary router.

### Initial Interpretation

The failure appeared to support the earlier hypothesis that the ISP firmware had restricted or removed the bridge behavior required for conventional access-point operation.

### Next Step

Investigate the firmware more deeply before considering firmware replacement.

---

# Phase 5 - Firmware Behavior Investigation

## Experiment 006 - One Click Diagnosis

### Objective

Determine whether the Huawei firmware still considered the optical GPON path to be the primary networking path.

### Procedure

The built-in **One Click Diagnosis** function was executed.

### Result

The diagnostic system reported:

* Optical path abnormal
* OLT registration failed
* Internet connection failed
* Hardware functioning normally

### Success?

**Yes - diagnostic objective achieved.**

### Finding

The router's optical subsystem was still expecting GPON connectivity.

The absence of an optical signal therefore caused the WAN/Internet diagnostic path to fail even though the underlying hardware remained functional.

### Significance

This confirmed that the device's original GPON role remained deeply integrated into the stock configuration.

At this stage, firmware behavior became the primary suspect.

### Next Step

Investigate possible firmware-level solutions.

---

# Phase 6 - Alternative Firmware Consideration

## Experiment 007 - Firmware Replacement Assessment

### Objective

Determine whether replacing the stock ISP firmware would be necessary to obtain conventional access-point functionality.

### Options Considered

Several possible approaches were identified:

1. Generic Huawei firmware
2. OpenWrt
3. Firmware reverse engineering
4. UART recovery
5. Complete firmware replacement
6. Discovery of hidden administrative functionality

### Result

No firmware replacement was performed at this stage.

### Success?

**Not applicable.**

This was a planning and feasibility stage rather than a completed firmware modification.

### Engineering Decision

Before attempting destructive or high-risk firmware operations, investigate whether the required functionality already existed within the installed firmware.

This decision ultimately prevented unnecessary firmware modification.

### Next Step

Research the exact EG8145V5 model and its ISP-specific administrative architecture.

---

# Phase 7 - Physical Restoration

## Experiment 008 - Antenna and Enclosure Restoration

### Objective

Restore the physical condition of the router sufficiently for continued operation and establish its new identity as White Dragon.

### Initial Condition

The device had suffered physical damage when it fell.

The most significant visible damage involved the antennas and their mounting points.

### Completed Work

* Custom front artwork
* White Dragon themed enclosure treatment
* Repaired broken antenna hinges
* Reinforced antenna mounts
* Installed cable ties where required
* Reinforced joints using hot glue
* Restored antennas to upright orientation

### Result

The router became physically usable and visually distinct from its original ISP-issued state.

### Success?

**Yes.**

### Future Physical Work

* Cooling improvements
* Additional heatsinks
* Improved airflow
* Complete enclosure redesign.

---

# Phase 8 - Reverse Engineering Planning

## Experiment 009 - Long-Term Firmware Investigation Roadmap

### Objective

Prepare a technical roadmap in case the stock firmware could not be made to perform the required access-point role.

### Result

The project expanded from a simple router-reuse attempt into a potential embedded-systems reverse-engineering project.

### Success?

**Planning objective achieved.**

### Important Engineering Principle

No firmware modification was performed simply because firmware modification appeared to be the obvious solution.

The investigation continued to search for a lower-risk explanation for the missing functionality.

### Next Step

Continue researching the specific EG8145V5 firmware and administrator privilege structure.

---

# Phase 9 - Breakthrough: Hidden Administrator Privileges

## Experiment 010 - Discovery of the Telecom Administrator Account

### Objective

Determine whether the stock ISP firmware contained hidden configuration functionality that was unavailable to the standard administrator account.

### Procedure

Further research was performed using online technical communities and video tutorials, including Reddit and YouTube.

The research revealed that ISP firmware on this class of Huawei device could contain multiple administrator privilege levels.

A YouTube tutorial demonstrated the existence of a **Super Administrator / Telecom Administrator** account.

### Result

The higher-privilege administrator account was successfully used to access previously hidden configuration functionality.

### Success?

**Yes - major breakthrough.**

### Finding

The required networking functionality had not necessarily been removed from the firmware.

Instead, it was hidden behind a higher privilege level.

The standard administrator interface therefore provided an incomplete view of the router's actual capabilities.

### Significance

This fundamentally changed the direction of the project.

The firmware replacement hypothesis was no longer the leading solution.

### Next Step

Use the Telecom Administrator interface to configure the Huawei unit as a proper access point.

---

# Phase 10 - Final Access Point Configuration

## Experiment 011 - Final White Dragon Configuration

### Objective

Convert the Huawei EG8145V5 into a functional wired wireless access point using the original firmware.

### Procedure

#### Step 1 - Factory Reset

A complete factory reset was performed again.

This ensured that previous experimental configurations were removed and the router started from a known baseline.

#### Step 2 - Independent Wireless Connection

The laptop was connected directly to the Huawei router's default wireless network.

This isolated White Dragon from the existing PLDT network during configuration.

#### Step 3 - Telecom Administrator Login

The router was accessed using the hidden **Telecom Administrator** account.

The higher privilege interface exposed advanced networking options that were not available through the standard administrator interface.

#### Step 4 - Enable LAN Ports

All four LAN ports were enabled.

#### Step 5 - Static Management Address

A static management IP address was assigned within the PLDT network.

This ensured that White Dragon could be reached at a predictable address for future administration.

#### Step 6 - Disable DHCP

Location:

```text
Advanced → LAN Host
```

The Huawei DHCP server was disabled.

This allowed the PLDT router to remain the only DHCP server on the network.

#### Step 7 - Disable Secondary LAN Address

Location:

```text
Advanced → LAN Host
```

The secondary LAN address was disabled.

This simplified the LAN configuration and ensured that White Dragon operated within the primary subnet.

#### Step 8 - Save and Restart

The configuration was saved and the Huawei router was restarted.

#### Step 9 - LAN-to-LAN Connection

The Huawei router was connected to the PLDT router using a LAN-to-LAN Ethernet connection.

### Final Topology

```text
                         INTERNET
                            │
                            │
                     ┌──────┴──────┐
                     │ PLDT Router │
                     │             │
                     │ DHCP Server │
                     └──────┬──────┘
                            │
                       LAN-to-LAN
                            │
                 ┌──────────┴──────────┐
                 │     WHITE DRAGON    │
                 │ Huawei EG8145V5     │
                 │                     │
                 │ DHCP: OFF           │
                 │ Static IP           │
                 │ WiFi: ON            │
                 │ LAN: ON             │
                 └───────┬─────┬───────┘
                         │     │
                       WiFi  Ethernet
                         │     │
                      Clients / Devices
```

### Result

The Huawei router successfully operated as a wired wireless access point.

### Success?

**Yes.**

### Critical Finding

**No firmware replacement was required.**

The functionality required for the project was already present within the stock firmware but hidden behind a higher administrative privilege level.

---

# Phase 11 - Performance Validation

## Experiment 012 - WiFi Dead-Zone Performance Test

### Objective

Determine whether White Dragon provided a meaningful improvement in wireless coverage and throughput in an area where the primary PLDT router performed poorly.

### Test Location

A known WiFi dead zone on the ground floor.

### Baseline Configuration

The test device connected directly to the PLDT router.

| Metric   |     Result |
| -------- | ---------: |
| Download | 47.74 Mbps |
| Upload   | 47.64 Mbps |

Signal quality was significantly reduced due to distance and physical obstacles.

### White Dragon Configuration

The same general test was performed through the Huawei EG8145V5 access point.

| Metric   |          Result |
| -------- | --------------: |
| Download | **187.11 Mbps** |
| Upload   | **202.98 Mbps** |

### Comparison

| Test         |        Download |          Upload |
| ------------ | --------------: | --------------: |
| PLDT Router  |      47.74 Mbps |      47.64 Mbps |
| White Dragon | **187.11 Mbps** | **202.98 Mbps** |

### Reported Improvement

* Download: approximately **292%**
* Upload: approximately **326%**

### Result

White Dragon substantially improved usable wireless performance in the previously weak coverage area.

### Success?

**Yes.**

### Important Qualification

The current validation is a **WiFi performance test**.

It does not yet constitute a complete wired network benchmark.

A dedicated Gigabit Ethernet test remains planned once a desktop system with an RJ45 Ethernet interface is available.

---

# Network Capabilities

| Interface / Function         | Status                  |
| ---------------------------- | ----------------------- |
| GPON / Optical WAN           | Not used                |
| Internet via GPON            | Not used                |
| Ethernet LAN                 | Operational             |
| LAN-to-LAN operation         | Operational             |
| WiFi                         | Operational             |
| DHCP Server                  | Disabled                |
| Static Management IP         | Configured              |
| LAN Ports                    | Enabled                 |
| Wireless Client Connectivity | Operational             |
| Upstream DHCP                | Provided by PLDT router |
| Access Point Function        | Operational             |

---

# Software / Firmware Status

| Item                      | Status                           |
| ------------------------- | -------------------------------- |
| Original Firmware         | Retained                         |
| Firmware Replacement      | None                             |
| Factory Reset             | Successfully performed           |
| Standard Administrator    | Functional after reset           |
| Telecom Administrator     | Successfully accessed            |
| Hidden Networking Options | Successfully exposed             |
| DHCP                      | Disabled for final configuration |
| WAN/GPON                  | Not used                         |
| Custom Firmware           | Not installed                    |
| OpenWrt                   | Not installed                    |
| Firmware Dump             | Not yet performed                |
| UART                      | Not yet investigated             |

---

# Physical Restoration Status

| Item                        | Status     |
| --------------------------- | ---------- |
| Enclosure                   | Restored   |
| Custom Artwork              | Completed  |
| Antenna Hinges              | Repaired   |
| Antenna Mounts              | Reinforced |
| Cable Tie Reinforcement     | Completed  |
| Hot Glue Reinforcement      | Completed  |
| Antenna Orientation         | Restored   |
| Additional Heatsinks        | Planned    |
| Cooling Fan                 | Planned    |
| Airflow Modification        | Planned    |

---

# What Worked

* Device powered on despite being decommissioned.
* WLAN functionality remained operational.
* Ethernet functionality remained operational.
* Factory reset restored administrator access.
* Local management interface became accessible.
* Router could be operated independently from the GPON network.
* LAN-to-LAN access-point topology was physically possible.
* Hidden Telecom Administrator functionality was discovered.
* All four LAN ports were enabled.
* DHCP could be disabled.
* Static management addressing worked.
* Wireless clients could connect successfully.
* White Dragon successfully operated as an access point.
* Original firmware was retained.
* No firmware replacement was necessary.
* Physical antenna damage was repaired.
* WiFi performance improved substantially in the target dead zone.

---

# What Initially Failed

* Original administrator credentials failed.
* Initial factory-reset attempts did not immediately restore access.
* Standard administrator configuration did not expose the required networking functionality.
* Initial access-point configuration resulted in APIPA addresses.
* DHCP requests did not initially reach the upstream PLDT router.
* WAN configuration appeared empty.
* One Click Diagnosis continued to report GPON/OLT failure.
* Firmware replacement appeared necessary before the hidden administrator account was discovered.

---

# Unexpected Findings

* A decommissioned GPON terminal remained highly usable as local networking hardware.
* The absence of GPON connectivity did not prevent the device from operating as a local WiFi/Ethernet platform.
* The standard administrator interface did not expose the full capabilities of the firmware.
* A higher-privilege Telecom Administrator account exposed additional networking options.
* The apparent requirement for custom firmware was ultimately a privilege/configuration problem.
* The physically damaged router remained capable of excellent wireless performance after restoration.
* The repurposed device substantially outperformed the primary router in the tested dead-zone location.

---

# Discussion

The central finding of White Dragon is that **the hardware was never the primary limitation**.

At the beginning of the project, the router appeared to be a typical decommissioned ISP GPON terminal. Its optical interface could no longer register with the ISP network, its original administrator credentials failed, and its standard configuration interface did not expose an obvious access-point mode.

The first conventional attempt at converting the router into an access point partially worked. Wireless clients could connect, and the device remained reachable through the network, but clients received APIPA addresses rather than valid DHCP leases.

This initially suggested that the ISP firmware had removed the required Ethernet bridging functionality.

The natural next step would have been firmware replacement.

OpenWrt, generic Huawei firmware, UART access, bootloader analysis, and complete firmware replacement were therefore considered.

However, the project deliberately avoided immediately modifying the firmware.

Further investigation instead revealed that the firmware contained a higher-level administrative interface.

The discovery of the Telecom Administrator account fundamentally changed the project.

Once the higher privilege interface was accessed, networking options that were unavailable to the standard administrator became visible. The device could then be configured properly for the intended LAN-to-LAN topology.

This demonstrated an important engineering principle:

> **An apparently missing feature should not automatically be assumed to be an absent feature. It may simply be inaccessible through the current privilege level or configuration state.**

The project therefore succeeded without modifying the firmware, reducing risk and preserving the original recovery path.

---

# Engineering Principles Demonstrated

### 1. Establish a Known Baseline

Factory resetting the router allowed the investigation to begin from a known configuration rather than from an unknown previous state.

### 2. Separate Hardware Failure from Service Failure

The GPON service was unavailable, but that did not mean the Ethernet and WiFi hardware was defective.

### 3. Diagnose Before Modifying

The initial APIPA behavior provided evidence that the problem was related to network configuration rather than simply assuming the router was incapable of access-point operation.

### 4. Avoid Destructive Changes Prematurely

Firmware replacement was considered but not immediately performed.

The stock firmware was investigated first.

### 5. Investigate Privilege Boundaries

The standard administrator account exposed only part of the firmware's functionality.

The Telecom Administrator account revealed additional capabilities.

### 6. Document Failed Attempts

The unsuccessful administrator login and initial APIPA configuration were retained as part of the project's history rather than being removed from the final record.

### 7. Validate the Final Result

The project did not stop when the configuration appeared to work.

A real-world dead-zone performance test was performed.

### 8. Restore Instead of Replace

The project extended the useful life of existing hardware rather than immediately purchasing another access point.

---

# Lessons Learned

* Factory reset timing can matter.
* ISP firmware often hides advanced functionality behind higher privilege accounts.
* A standard administrator interface may not represent the complete capabilities of a device.
* Hidden administrator accounts can expose functionality that initially appears to be missing.
* APIPA addresses are a useful diagnostic clue when DHCP communication is failing.
* Network diagnostics should be performed before considering firmware replacement.
* Systematic troubleshooting is more effective than changing many variables simultaneously.
* Hardware restoration can extend the useful life of otherwise discarded networking equipment.
* Physical repair and software investigation can be treated as separate engineering workstreams.
* Performance validation is necessary to distinguish "configured" from "actually useful."
* Documentation is itself a technical asset.
* Not every networking problem requires custom firmware.

The most important lesson from White Dragon was simple:

**Sometimes the functionality is already there. You just have to find it.**

---

# Current Network Role

White Dragon currently operates as:

**Wired Wireless Access Point**

### Upstream

PLDT Router

### Management

Static LAN IP

### DHCP

Disabled on White Dragon

### Wireless

Operational

### Ethernet

Operational

### GPON

Unused

### Firmware

Original Converge/Huawei firmware

### Hardware

Fully functional and physically restored

---

# Results

## Primary Result

White Dragon successfully transformed a decommissioned Huawei EchoLife EG8145V5 GPON terminal into a fully functional wired wireless access point.

## Technical Result

The conversion was achieved **without replacing the original firmware**.

The key breakthrough was discovering the hidden Telecom Administrator privilege level, which exposed the configuration required for proper access-point operation.

## Physical Result

The previously damaged router was restored sufficiently for continued service.

The broken antenna hinges and mounts were repaired and reinforced.

## Performance Result

In the tested WiFi dead zone:

**PLDT Router**

* Download: 47.74 Mbps
* Upload: 47.64 Mbps

**White Dragon**

* Download: 187.11 Mbps
* Upload: 202.98 Mbps

The result demonstrated a substantial improvement in usable wireless performance in the target area.

---

# Project Outcome

White Dragon successfully completed its first mission.

A decommissioned ISP-issued Huawei GPON terminal that was originally found discarded in a home in Pampanga was initially considered nothing more than hardware worth tearing down.

Instead, the device was discovered to still be functional.

After being brought to Manila and sitting unused for several weeks, it became the subject of a deeper investigation into its hardware, firmware, network behavior, and hidden administrative capabilities.

The project progressed through multiple stages:

```text
Discarded Hardware
       ↓
Initial Inspection
       ↓
Hardware Found Functional
       ↓
Physical Restoration
       ↓
Factory Reset
       ↓
Administrative Access
       ↓
Network Investigation
       ↓
Initial AP Conversion Attempt
       ↓
APIPA / DHCP Failure
       ↓
Firmware Investigation
       ↓
Firmware Replacement Considered
       ↓
Hidden Administrator Discovery
       ↓
Telecom Administrator Access
       ↓
Advanced LAN Configuration
       ↓
LAN-to-LAN Conversion
       ↓
WiFi Validation
       ↓
Performance Testing
       ↓
WHITE DRAGON
```

The final solution was significantly simpler and safer than originally anticipated.

No custom firmware was required.

No firmware flashing was required.

No destructive hardware modification was required.

The project instead succeeded through **observation, research, controlled experimentation, configuration, physical restoration, and validation**.

White Dragon therefore became the first operational member of the **Networking Dragons** series.

The project demonstrates that decommissioned networking equipment should not automatically be treated as obsolete or useless. With enough investigation, even ISP-restricted hardware may contain capabilities that remain accessible through undocumented or higher-privilege interfaces.

The project also establishes the engineering philosophy for future Networking Dragons projects:

> **Do not assume. Inspect.
> Do not modify first. Investigate first.
> Document the failures as carefully as the successes.
> And if the hardware can still work, give it another mission.**

---

# Networking Dragons - Project Status

| Project            | Hardware                   | Mission                   | Status             |
| ------------------ | -------------------------- | ------------------------- | ------------------ |
| **White Dragon**   | Huawei EchoLife EG8145V5   | Wireless Access Point     | **Operational**    |
| **Black Dragon**   | ZTE ZXV10 B886V2F          | Firewall / Alternative OS | Archived / Pivoted |
| **Emerald Dragon** | ZTE ZXV10 B886V2F          | Media Hub / Media Server  | In Progress        |
| **Orange Dragon**  | External Storage Expansion | Media Storage             | Planned            |

White Dragon represents the first successful proof-of-concept that the Networking Dragons philosophy can turn discarded ISP hardware into useful infrastructure.

---

# Closing Note

White Dragon was never supposed to become an access point.

It started as an old router lying around in Pampanga that was going to be opened up simply to see what was inside.

The fact that it still worked changed the direction of the project.

What followed was a small example of embedded-systems engineering in practice: establish a baseline, test assumptions, observe failures, investigate the firmware, avoid unnecessary destructive changes, find the actual constraint, and validate the final result.

The router did not need a new operating system.

It needed someone to look a little deeper.
