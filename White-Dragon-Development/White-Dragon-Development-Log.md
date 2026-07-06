Project: Networking Dragons

Codename: White Dragon

Platform: Huawei EchoLife EG8145V5 GPON Terminal

Status: Operational

---

# Project Objective

Repurpose a decommissioned Huawei EchoLife EG8145V5 GPON terminal into a fully functional networking device instead of allowing it to become electronic waste.

The original goal was to convert the ISP supplied optical terminal into a wired wireless access point connected to an existing PLDT home network.

---

# Initial Assessment

The router was originally supplied by Converge ICT.

Observations:

- Device powered on normally.
- WLAN LEDs functioned.
- Power LED functioned.
- LOS indicator blinked red due to the missing optical fiber.
- Factory administrator credentials initially failed.

The hardware appeared functional despite being decommissioned.

---

# Phase 1: Restoring Administrative Access

Several administrator credentials printed on the device label failed.

A factory reset was performed multiple times.

The reset button was held for a significantly longer period during the final attempt.

Result:

Administrator access was successfully restored using the default credentials.

This confirmed that the firmware had been reset to factory configuration.

---

# Phase 2: Initial Network Investigation

The router was connected independently.

Linux Mint networking tools were used for diagnostics.

Commands used:

ip addr

ip route

ping

Initial observations:

- WiFi broadcasting correctly.
- Ethernet interfaces operational.
- Router management interface accessible.

---

# Phase 3: Exploring the Firmware

The Huawei management interface was inspected.

Major configuration pages examined included:

- WLAN
- LAN
- WAN
- DHCP
- Security
- Diagnostics

The WAN configuration page appeared empty.

At this stage it was believed that the ISP firmware had removed the Ethernet bridge service.

---

# Phase 4: Wired Access Point Conversion

The intended network topology became:

<img width="1024" height="1536" alt="image" src="https://github.com/user-attachments/assets/bff06d8e-0504-4c8b-bcb6-8a967dc06be2" />

Changes performed:

- Disabled DHCP Server.
- Assigned static management IP.
- Connected LAN to LAN.
- Verified Ethernet link LEDs.
- Confirmed router accessibility from the PLDT network.

Result:

Management access worked correctly.

Wireless clients connected successfully.

However, clients received APIPA addresses instead of valid DHCP leases.

Observed addresses:

169.254.x.x

This indicated DHCP requests were not reaching the upstream router.

---

# Phase 5: Firmware Investigation

One Click Diagnosis reported:

- Optical path abnormal.
- OLT registration failed.
- Internet connection failed.
- Hardware functioning normally.

This confirmed that the firmware still expected GPON connectivity through the optical interface.

At this point the firmware itself became the primary suspect.

Several possible solutions were considered.

- Generic Huawei firmware
- OpenWrt
- Reverse engineering
- UART recovery
- Complete firmware replacement

---

# Phase 6: Hardware Restoration

The physical condition of the router was improved.

Completed work:
- Custom front artwork.
- White Dragon themed enclosure.
- Repaired broken antenna hinges.
- Reinforced antenna mounts.
- Installed cable ties.
- Reinforced joints using hot glue.
- Restored antennas to upright orientation.

Future plans:
- Cooling improvements.
- Additional heatsinks.
- Optional rear cooling fan.

---

# Phase 7: Reverse Engineering Planning

A long term reverse engineering roadmap idea was drafted.

Topics included:

- UART access
- Bootloader analysis
- Firmware dumping
- Flash memory backup
- Linux filesystem analysis
- Custom firmware development

The project expanded beyond simple router reuse into an embedded systems engineering project.

---

# Breakthrough

Further research to Reddit and Youtube led to the discovery that the ISP firmware contained hidden administrator privilege levels.

A YouTube tutorial demonstrated the existence of a Super Administrator account.

After logging in using the higher privilege account, previously hidden networking options became available.

The router was successfully configured as intended.

No firmware replacement was required.

---

# Final Configuration

Role:

Wired Wireless Access Point

Upstream:

PLDT Router

Management:

Static LAN IP

DHCP:

Disabled on Huawei/Converge

Wireless:

Operational

Ethernet:

Operational

Hardware:

Fully functional

---

# Cosmetic Restoration

The router received a new identity.

Project Name:

White Dragon

Completed:

- New custom skin
- Restored antennas
- Physical reinforcement

Planned:

- White Dragon themed antenna sleeves
- Additional cooling fan
- Internal heatsink
- Improved airflow
- Complete enclosure redesign

---

# Lessons Learned

Factory reset timing matters.

ISP firmware often hides advanced features behind higher privilege accounts.

Hidden administrator accounts can expose functionality that appears missing.

Network diagnostics should always be performed before considering firmware replacement.

Physical restoration can extend the useful life of networking hardware.

Documentation is just as valuable as technical modifications.

---

# Future Development

Future objectives include:

- Firmware backup
- UART documentation
- Hardware teardown
- PCB documentation
- Thermal optimization
- Performance benchmarking
- Network throughput testing
- Security hardening
- GitHub documentation

---

# Project Outcome

White Dragon successfully transformed from a discarded ISP optical terminal into a fully operational wireless networking device.

The project demonstrates that obsolete networking hardware can often be repurposed through careful investigation, hardware repair, documentation, and persistence instead of immediate replacement.

The success of White Dragon marks the beginning of the Networking Dragons project, a collection of restored, modified, and documented networking hardware designed to promote learning, sustainability, and open technical documentation.
