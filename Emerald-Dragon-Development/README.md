<div align="center">

<img src="https://github.com/user-attachments/assets/a04a2d28-9683-4c38-a685-bcc9f67736b3" alt="Emerald Dragon Banner" width="100%">

# Emerald Dragon

### From ISP Issued IPTV Hardware to a Dedicated Media Hub

![Status](https://img.shields.io/badge/Status-Operational-10B981?style=for-the-badge)
![Project](https://img.shields.io/badge/Project-Emerald%20Dragon-059669?style=for-the-badge)
![Platform](https://img.shields.io/badge/Platform-Amlogic%20S905Y4-047857?style=for-the-badge)
![Role](https://img.shields.io/badge/Role-Media%20Hub-16A34A?style=for-the-badge)

</div>

---

# Overview

Emerald Dragon is a hardware repurposing and reverse engineering project centered around the **Converge Xperience Hub**, specifically the **ZTE ZXV10 B886V2F** IPTV receiver distributed by Converge ICT. Originally designed as a consumer streaming appliance for IPTV services, the device had reached the end of its practical use within the intended network environment. Rather than allowing the hardware to become electronic waste, this project explores how an otherwise abandoned embedded system can be evaluated, documented, and assigned a new purpose.

The project began under a different objective. The original intention was to transform the IPTV receiver into the first prototype of **Black Dragon**, a lightweight embedded firewall intended for the Networking-Dragons ecosystem. Preliminary hardware specifications suggested that the device possessed enough computational capability to support a Linux based firewall distribution. Its Amlogic S905Y4 system-on-chip, integrated networking hardware, and embedded storage appeared to provide a promising foundation for an experimental networking appliance.

As the investigation progressed, it became evident that the platform presented several significant obstacles. Hardware identification, firmware analysis, boot sequence observation, MaskROM investigation, and software testing revealed a heavily restricted environment. Developer options remained inaccessible, publicly documented methods of unlocking the firmware proved ineffective, and the boot process consistently prioritized the vendor software before exposing external storage devices. Additional investigation into recovery mechanisms and firmware flashing identified further limitations, including the absence of publicly available firmware images compatible with this specific hardware revision and limited support from the open source community for the platform.

These findings fundamentally changed the direction of the project. Rather than forcing the hardware into a role for which it was poorly suited, the project adopted an engineering approach driven by evidence. The strengths of the platform were reevaluated, leading to a more practical objective that aligned with the original purpose of the hardware.

Emerald Dragon was therefore established as a dedicated media hub. By retaining the stock Android TV environment and integrating media streaming applications such as Plex, the device continues to provide meaningful functionality while extending its operational lifespan. Instead of serving as a networking appliance, it now operates as a lightweight client for local and network hosted multimedia content, providing an efficient solution for home entertainment while reducing unnecessary electronic waste.

This repository documents the complete lifecycle of the project, including the original research objectives, reverse engineering process, hardware analysis, firmware investigation, implementation decisions, and the final deployment as a media hub. Both successful outcomes and unsuccessful experiments are preserved to provide a complete engineering record and to assist future researchers or hobbyists working with similar hardware.

---

# Project Objectives

The objectives of Emerald Dragon are organized into four primary areas:

## Hardware Evaluation

- Document the physical hardware and internal architecture of the Converge Xperience Hub.
- Identify major integrated circuits, storage devices, communication interfaces, and debugging points.
- Investigate available recovery and maintenance interfaces.

## Reverse Engineering

- Analyze the firmware architecture and boot sequence.
- Investigate bootloader behavior and system restrictions.
- Evaluate the feasibility of running alternative operating systems or custom firmware.
- Record technical findings regardless of project outcome.

## Hardware Repurposing

- Extend the functional lifespan of obsolete consumer hardware.
- Deploy the device as a dedicated Android TV media hub.
- Utilize Plex and other streaming platforms for local and network media playback.
- Preserve the device as a useful component within a modern home network.

## Documentation

- Produce comprehensive technical documentation throughout every stage of development.
- Record failed experiments, observations, and engineering decisions.
- Maintain an organized repository for future reference.
- Demonstrate a repeatable methodology for evaluating embedded consumer hardware.

---

# Final Outcome

Although the original firewall implementation proved impractical due to platform restrictions, the investigation successfully characterized the hardware and established realistic limitations for future development. Rather than considering the project unsuccessful, Emerald Dragon demonstrates the importance of allowing engineering decisions to be guided by experimental evidence instead of initial expectations.

The completed system now functions as a reliable Android TV media hub capable of streaming local and network hosted content while preserving the technical knowledge gained throughout the reverse engineering process. The project stands as an example of sustainable hardware reuse through careful analysis, documentation, and practical adaptation.

---

> *Not every engineering investigation ends with the intended solution. Sometimes the most valuable outcome is understanding the limitations of a system well enough to discover a better purpose for it.*
