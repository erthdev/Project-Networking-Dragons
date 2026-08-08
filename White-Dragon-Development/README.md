<div align="center">

<img width="1024" height="254" alt="image" src="https://github.com/user-attachments/assets/3e19b438-1ab6-42d3-b5da-91d3c53fb8a7" />

# White Dragon

### From Decommissioned GPON Hardware to a Dedicated Wireless Access Point

![Status](https://img.shields.io/badge/STATUS-OPERATIONAL-F9FAFB?style=for-the-badge\&labelColor=111827)
![Project](https://img.shields.io/badge/PROJECT-WHITE%20DRAGON-E5E7EB?style=for-the-badge\&labelColor=111827)
![Platform](https://img.shields.io/badge/PLATFORM-HUAWEI%20EG8145V5-D1D5DB?style=for-the-badge\&labelColor=1F2937)
![Role](https://img.shields.io/badge/ROLE-WIRELESS%20ACCESS%20POINT-F3F4F6?style=for-the-badge\&labelColor=111827)

</div>

---

# Overview

White Dragon is a hardware repurposing project centered around the **Huawei EchoLife EG8145V5 GPON Terminal**, originally supplied by **Converge ICT**.

The project began when the router was found unused at home in **Pampanga**. It was initially intended to be dismantled for hardware inspection, but testing revealed that the device was still operational. The only significant physical damage was to the antennas, which had been broken when the router previously fell.

The device was later brought to Manila, where it remained unused for several weeks. Further research into the specific hardware eventually led to the idea of repurposing it as a **wireless access point** for an existing **PLDT home network**.

The first access-point configuration appeared unsuccessful. Although Ethernet and WiFi functionality worked, connected clients received `169.254.x.x` APIPA addresses instead of DHCP leases from the PLDT router. This initially suggested that the ISP firmware had restricted the required networking functionality.

Further investigation revealed that the firmware contained a hidden **Telecom Administrator** privilege level. Accessing this account exposed additional LAN configuration options that were unavailable through the standard administrator interface.

The device was subsequently configured with DHCP disabled, a static management IP, all LAN ports enabled, the secondary LAN address disabled, and a LAN-to-LAN connection to the PLDT router.

**No firmware replacement was required.**

White Dragon now operates as a dedicated wireless access point, extending the existing network into an area with previously poor WiFi coverage.

---

# Project Objectives

## Hardware Repurposing

* Restore and reuse the decommissioned Huawei EG8145V5.
* Convert the GPON terminal into a wireless access point.
* Extend the useful life of existing networking hardware.
* Avoid unnecessary firmware replacement.

## Network Investigation

* Determine whether the device could operate without its GPON connection.
* Investigate LAN, DHCP, and wireless behavior.
* Discover the configuration required for LAN-to-LAN operation.
* Validate real-world wireless performance.

## Documentation

* Record the important technical findings.
* Preserve failed configuration attempts.
* Document the final implementation.
* Establish White Dragon as the first completed project in the **Networking Dragons** series.

---

# Final Configuration

<img width="1024" height="559" alt="image" src="https://github.com/user-attachments/assets/d40eefc0-ec7a-495a-a49f-54db4fce6e9f" />


The **PLDT router** remains responsible for routing and DHCP, while White Dragon provides wireless and local Ethernet connectivity.

---

# Performance Validation


Testing was performed in a known WiFi dead zone on the ground floor.

| Test             |        Download |          Upload |
| ---------------- | --------------: | --------------: |
| PLDT Router      |      47.74 Mbps |      47.64 Mbps |
| **White Dragon** | **187.11 Mbps** | **202.98 Mbps** |

White Dragon achieved approximately:

* **292% improvement in download**
* **326% improvement in upload**

The current results are based on WiFi testing. Wired Gigabit throughput and long-term stability testing remain future work.

---

# Final Outcome

White Dragon successfully transformed a decommissioned ISP-issued GPON terminal into a functional wireless access point without replacing its original firmware.

The project demonstrated that the hardware remained useful despite its original ISP service being unavailable, and that functionality initially believed to be missing was instead hidden behind a higher administrative privilege level.

The device was physically restored, configured, and validated in a real-world network environment.

White Dragon now serves as the first completed project in the **Networking Dragons** series.

---

<div align="center">

> *Sometimes the hardware isn't obsolete. It just needs a new purpose.*

</div>
