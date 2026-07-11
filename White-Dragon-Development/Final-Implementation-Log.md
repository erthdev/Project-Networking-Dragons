# Final Implementation Log

**Project:** White Dragon

**Series:** Networking Dragons

**Date Completed:** July 2026

**Status:** ✅ Operational

---

# Objective

Transform a decommissioned Huawei EchoLife EG8145V5 GPON terminal into a fully functional wired wireless access point connected to an existing PLDT home network.

---

# Final Procedure

After several unsuccessful attempts and extensive troubleshooting, the final solution was surprisingly straightforward once the hidden administrator privileges were discovered.

## Step 1

Performed another complete factory reset of the Huawei router.

This ensured that all previous configurations were removed and the device returned to its factory default state.

---

## Step 2

Connected the laptop directly to the default Huawei wireless network (SSID).

This isolated the router from the existing PLDT network, allowing it to be configured independently.

---

## Step 3

Logged into the router using the hidden **Telecom Administrator** account.

Unlike the standard administrator account, the Telecom Administrator interface exposed the advanced networking configuration required to convert the device into an access point.

---

## Step 4

Enabled all four LAN ports.

The ISP firmware had disabled or restricted certain LAN functionality under the standard administrator account.

---

## Step 5

Configured a static management IP address.

The Huawei router was assigned a static address within the PLDT network so it could always be reached for future administration.

---

## Step 6

Disabled DHCP.

Location:

Advanced → LAN Host

The Huawei router was configured to stop assigning IP addresses.

This allows the primary PLDT router to remain the only DHCP server on the network, preventing address conflicts.

---

## Step 7

Disabled the Secondary LAN Address.

Location:

Advanced → LAN Host

This simplified the LAN configuration and ensured the Huawei operated only within the primary subnet.

---

## Step 8

Saved the configuration.

Restarted the Huawei router.

---

## Step 9

Connected the Huawei router to the PLDT router using a LAN-to-LAN Ethernet connection.

This completed the conversion into a wired wireless access point.

---

# Validation

The completed access point was tested in a known WiFi dead zone on the ground floor.

## Baseline

Connected directly to the PLDT router.

Download

47.74 Mbps

Upload

47.64 Mbps

Signal quality was significantly reduced due to distance and obstacles.

---

## White Dragon

Connected to the Huawei EG8145V5 access point.

Download

187.11 Mbps

Upload

202.98 Mbps

The improvement confirmed that the access point was operating correctly and successfully extending high-speed wireless coverage into the previously weak area.

---

# Current Performance

| Test | Download | Upload |
|-------|----------|--------|
| PLDT Router | 47.74 Mbps | 47.64 Mbps |
| White Dragon | **187.11 Mbps** | **202.98 Mbps** |

Approximate improvement:

- Download: ~292%
- Upload: ~326%

---

# Pending Tests

The current validation has only been performed over WiFi.

A wired Gigabit Ethernet performance test is planned once the desktop PC has been completed, as the current laptop does not include an RJ45 Ethernet port.

Future testing will include:

- LAN throughput
- Latency
- Stability
- Long-duration uptime
- Thermal performance
- Multiple-client performance

---

# Lessons Learned

This project reinforced several important engineering principles.

- ISP firmware often hides advanced functionality behind higher privilege accounts.
- Factory resetting can resolve configuration issues that are difficult to diagnose.
- Systematic troubleshooting is more effective than changing multiple settings simultaneously.
- Proper documentation significantly reduces future troubleshooting time.
- Older networking hardware can still provide excellent performance when configured correctly.

Perhaps the most important lesson was that not every problem requires replacing firmware. Sometimes the required functionality is already present, waiting to be unlocked.

---

# Project Outcome

White Dragon successfully completed its first mission.

A decommissioned ISP-issued GPON terminal was restored, repaired, customized, and repurposed into a fully functional wired wireless access point.

The project achieved its primary objective without replacing the original firmware, demonstrating that careful investigation and persistence can often extend the life of existing hardware.

This marks the first completed project in the **Networking Dragons** series.
