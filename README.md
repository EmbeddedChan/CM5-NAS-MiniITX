# CM5-NAS-MiniITX

A Mini-ITX NAS / home server platform based on the Raspberry Pi Compute Module 5 (CM5).

I am designing a CM5-based Mini-ITX carrier board that combines dual cascaded PCIe switches, multi-port NVMe/SATA storage, dual Ethernet (1G + 2.5G), PCIe expansion, and a GPSDO-based PTP timing subsystem, all in a standard Mini-ITX form factor.

The goal is to create a flexible CM5 platform that can be used not only as a NAS, but also as a home server, router/firewall, storage server, PTP grandmaster clock/timing appliance, and future FPGA/PCIe expansion platform.

## Project Status

Current status: System architecture and hardware planning

The initial system architecture (block diagram) has been designed. The next steps are schematic design, PCB layout, prototype fabrication, assembly, and hardware validation.

This project is currently developed as an independent hardware project. I do not currently have the resources to fabricate and test the first prototype, so community support would directly help move the project from the design stage to working hardware.

## System Architecture

The current architecture is shown below:

`CM5-NAS-MiniITX system architecture` (docs/system-block-diagram.svg)

The design is centered around the Raspberry Pi CM5, whose single PCIe lane feeds a **cascaded pair of 4-port PCIe switches**, fanning out into storage, networking, and expansion slots, alongside the CM5's native USB/HDMI/Ethernet interfaces.

### CM5 native I/O
- USB 2.0 Type-C
- USB 3.0 x2
- HDMI x2
- Gigabit Ethernet (1G)
- PCIe (single lane, routed to PCIe switch #1)

### PCIe switch fabric (cascaded)
- **PCIe switch #1 (4 downstream ports)**: upstream from CM5; downstream to a PCIe-to-SATA bridge, a PCIe-to-2.5G-Ethernet NIC, a PCIe x1 expansion slot, and the cascade link to switch #2
- **PCIe switch #2 (4 downstream ports)**: cascaded from switch #1; all 4 downstream ports go to the M.2 2280 x4 NVMe bank

### Storage
- 4x M.2 2280 NVMe slots (via PCIe switch #2)
- 2x SATA ports (SATA0/SATA1) via a PCIe-to-SATA (2-port) bridge chip off switch #1
- 2x 2.5-inch drive support (via SATA)

### Networking
- Onboard Gigabit Ethernet (from CM5)
- Onboard 2.5G Ethernet (PCIe-to-2.5G NIC off switch #1)

### PCIe Expansion
- PCIe x1 expansion slot

### Timing / Synchronization
- GPSDO (GPS-disciplined oscillator) module, enabling the board to act as a PTP grandmaster clock (Ordinary Clock, grandmaster-capable)
- SYNC_OUT signal for distributing a disciplined timing reference on the board
- Positions the platform as a PTP grandmaster / network timing appliance in addition to NAS/router roles

### Power
- Three selectable power inputs, combined onto the board's main rail:
  - DC-005 barrel jack, 12V–20V input
  - Standard ATX 24-pin PSU input
  - USB Type-C PD sink, 20V/3.25A (~65W)
- Internal 12V and 20V rails distributed to storage, PCIe switches, and CM5 power management
- CM5 power management (5V and other required rails regulated from the above inputs)

### Mechanical
- Standard Mini-ITX form factor
- Standard mounting-hole pattern
- Designed for use with commonly available Mini-ITX cases

## Development Plan

1. System architecture
2. Schematic design
3. PCB layout
4. PCB fabrication
5. Assembly
6. Power and signal validation
7. PCIe / SATA / Ethernet testing
8. PTP/GPSDO timing validation
9. Mechanical testing
10. Publish progress updates and validation results (design file release TBD)

## Why I Need Support

The system architecture and initial hardware design are being developed now, but the major cost is the transition from CAD to physical hardware.

Support would be used primarily for:

- PCB fabrication
- PCB assembly
- CM5 and other components (PCIe switches, NVMe/SATA bridge, 2.5G NIC, GPSDO module)
- Storage and PCIe test hardware
- Prototype revisions
- Measurement and debugging

The objective is to use community support to build and validate the first prototypes, then share the results with the community.

## Support the Project

If you are interested in this project and would like to help bring the first prototype to life, you can support the development through:

Donation (TRON / TRX / USDT-TRC20)

Address:
`TWJxB5izJwCg7Q2cTVFkp9ZjamsFmE2UqV`

Every contribution helps cover the cost of fabrication, components, and prototype testing.

If you cannot contribute financially, feedback, design review, testing suggestions, and sharing the project are also very helpful.

## Project Status & Licensing

This project is currently **closed-source**. Schematic and PCB source files (KiCad projects, Gerbers, BOM) are not publicly available at this stage.

Progress updates, block diagrams, renders, and test results will still be shared here as the hardware is developed and validated. Whether and when design files will be released publicly is not yet decided, and will depend on how the project develops.

Donations and sponsorships go directly toward prototype fabrication, components, and validation — they do not currently come with access to design files. If that changes, it will be announced here.
