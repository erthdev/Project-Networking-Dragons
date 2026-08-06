# Emerald Dragon Development Log

> **Project:** Emerald Dragon  
> **Platform:** Converge Xperience Hub (ZTE ZXV10 B886V2F)  
> **Status:** Operational  
> **Current Role:** Android TV Media Hub

---

## Background

Emerald Dragon was never part of the original roadmap.

The device originally began its life as a prototype for **Black Dragon**, a planned embedded firewall intended for the Networking-Dragons ecosystem. The hardware appeared to be a promising candidate due to its ARM64 processor, Ethernet interface, and embedded storage. The goal was to investigate whether the IPTV receiver could be transformed into a lightweight Linux based networking appliance capable of acting as a firewall between the main router and the internal network.

The project quickly became a reverse engineering effort.

---

# Development Log

## Day 1

### Initial Inspection

The first step was identifying the hardware.

The enclosure was opened to inspect the internal components and determine whether the platform was capable of running a custom operating system.

Major components were documented, including:

- Amlogic S905Y4 System on Chip
- eMMC Storage
- DDR Memory
- Ethernet Controller
- Various power management components

At this stage the hardware looked very promising.

The specifications suggested that the device should be capable of running Linux without major performance concerns.

The initial plan remained focused on converting the IPTV receiver into Black Dragon.

---

## Day 2

### Firmware Investigation

Attention shifted toward the software.

The objective was to determine whether developer options, bootloader access, or recovery mechanisms could be used to install a custom firmware image.

Several publicly available methods were tested.

Developer options remained inaccessible.

Remote control key combinations documented online produced no results.

USB boot behavior appeared to be heavily restricted.

The platform consistently prioritized launching the stock SkyTV software before interacting with external storage devices.

At this point it became clear that software restrictions were likely to become the primary obstacle rather than the hardware itself.

---

## Day 3

### Reverse Engineering

Research expanded beyond software configuration.

The motherboard was examined in greater detail to identify debugging interfaces and possible recovery methods.

The eMMC storage chip was located.

Attention then shifted toward identifying MaskROM related test points.

After tracing the board layout and comparing similar Amlogic platforms, two test points believed to trigger MaskROM mode were identified.

Several experiments were performed in an attempt to place the device into a recoverable state.

Although progress was made toward understanding the hardware, no practical path toward replacing the stock firmware was discovered.

---

## Day 4

### Firmware Research

The next objective was locating compatible firmware.

Various Amlogic firmware images were investigated.

Unfortunately, the S905Y4 proved to be considerably newer than many of the devices supported by the existing open source community.

Compatible firmware images specific to this hardware revision were not available.

Even if firmware flashing became possible, there was no reliable image capable of booting the platform.

At this stage the project had reached an important conclusion.

The limitation was no longer hardware capability.

The limitation was ecosystem support.

---

## Day 5

### Decision Point

After several days of research and experimentation, the original objective was reevaluated.

The question changed from:

> "How can this become a firewall?"

to

> "What is this hardware actually good at?"

The answer was obvious.

This device was designed from the beginning as a media appliance.

It contained dedicated hardware acceleration for video playback, HDMI output, and an Android TV environment already optimized for multimedia consumption.

Instead of continuing to force the hardware into a role it resisted, a different direction was chosen.

---

## Day 6

### Emerald Dragon

The project officially changed direction.

Rather than becoming the first Black Dragon prototype, the IPTV receiver was repurposed into **Emerald Dragon**.

The stock operating system was preserved.

Plex was installed through the Google Play Store.

The device became a dedicated Android TV media hub capable of streaming local and network hosted content.

A custom Emerald Dragon identity was created to reflect its new purpose within the Networking-Dragons ecosystem.

The original firewall research was archived and preserved as reference material for future Black Dragon development.

---

# Lessons Learned

This project reinforced several important engineering principles.

Hardware specifications alone do not determine whether a platform is suitable for custom firmware.

Vendor restrictions, bootloader behavior, software support, and community resources can become equally important factors.

Not every engineering investigation produces the expected outcome.

Sometimes the most valuable result is understanding the limitations of a system well enough to recognize a better application.

Emerald Dragon exists because of those findings.

Rather than becoming another abandoned IPTV receiver, the device continues to serve a meaningful purpose while preserving the knowledge gained throughout the investigation.

---

# Current Configuration

**Role**

Android TV Media Hub

**Primary Applications**

- Plex
- YouTube
- Netflix
- Local Media Playback

**Platform**

- Converge Xperience Hub
- ZTE ZXV10 B886V2F
- Amlogic S905Y4
- Android TV

---

# Future Plans

- Expand local media library
- Integrate with Orange Dragon media server
- Improve documentation

Although Emerald Dragon has reached its intended purpose, research surrounding the hardware has not ended. If future developments make custom firmware practical, additional experimentation may continue while preserving the device's role as the media hub of the Networking-Dragons ecosystem.

### Related Documentation

> For a more in-depth dive into the documentation and research process, click the following documentations.

- [Black Dragon Log 1 – Original Research and Investigation](../Black-Dragon-Development/docs/log-1.md)
- [Emerald Dragon README](README.md)
