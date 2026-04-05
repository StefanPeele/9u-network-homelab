# -__|| Small-Medium Enterprise Network Sim Lab ||__-
### Physical & Logical Infrastructure Design | Multi-Layer Campus LAN | My Personal Home-Lab Build...

![Status](https://img.shields.io/badge/Status-In%20Progress-yellow)
![Layer](https://img.shields.io/badge/Layer-2%2F3-blue)
![Rack](https://img.shields.io/badge/Rack-9U-lightgrey)
![CCNA](https://img.shields.io/badge/Aligned-CCNA%20200--301-brightgreen)

---

## 📋 Project Overview

This documents the end-to-end design, procurement, configuration, and operation of the 9U physical rack environment simulating a real small-to-medium enterprise network. Built on a sub-$600 budget using production-grade Cisco hardware, this lab aims to bridge the gap between textbook knowledge and hands-on infrastructure engineering.

The environment is architected around a two-tier campus LAN model separating access and distribution/core responsibilities across a Cisco Catalyst 2960X and 3560X respectively. Every design decision, addressing scheme, VLAN structure, and configuration is documented to professional standards, mirroring the documentation practices of enterprise network engineering teams.

This is a living project. As hardware is acquired and configurations are validated virtually (via GNS3) before physical implementation, this repository will grow to include automation scripts, monitoring integration, wireless simulation, and security hardening documentation.

---

## 🎯 Objectives!

- Design and implement a multi-layer enterprise LAN using industry standard Cisco hardware (and IOS 15+)
- Apply proper campus LAN design principles including access/distribution tier separation
- Configure and document VLAN segmentation, 802.1Q trunking, and Layer 3 inter-VLAN routing
- Implement secure management practices, from SSH-only access, dedicated management VLAN, native VLAN hardening
- Calculate and document PoE power budgets for Catalyst switch infrastructure
- Build a fully version-controlled configuration library and professional-grade network documentation suite
- Integrate open-source network monitoring (SNMP/Grafana) on a Linux management host
- Simulate and validate all configurations virtually in GNS3 before physical deployment
- Establish a foundation for future wireless (CAPWAP/WLC), security (ASA/FTD), and automation (Python/Netmiko) integration

---

## 🖥️ The Hardware & Technology Stack

### Physical Hardware
| Device | Model | Role | Status |
|--------|-------|------|--------|
| Core/Distribution Switch | Cisco Catalyst 3560X | Layer 3 switching, inter-VLAN routing, PoE | Acquired |
| Access Layer Switch | Cisco Catalyst 2960X | Layer 2 switching, end-host connectivity | Acquired |
| Router | TBD | WAN edge, routing | Acquired |
| Management Host | HP EliteDesk 800 G4 I5-8th gen, 256gb nvME ssd, 16gb ddr4, win11 home | Linux server, monitoring, automation | Acquired |
| Rack | 9U rack enclosure | Physical infrastructure housing | Pending |
| Patch Panel | TBD | Cable management | Planned |
| PDU | Rack-mount power strip | Power distribution | Pending |
| Network Tool-kit | Acquired |
| WAP | Cisco Aironet 1832 802.11ac | WLC expansion, emphasizing routing and wireless accessibility | Pending |
| 100pcs Pass through Cat6/5 | Non-name-brand | Aquired |
| Router | Cisco 3560C+PoE | Backup/Redundancy | Pending
| Router | Cisco 1921 | Backup/Redundancy | Pending
MORE SOON

### Software & Platforms
| Tool | Purpose |
|------|---------|
| Cisco IOS 15.x | Switch and router operating system |
| GNS3 | Virtual lab simulation and pre-deployment config testing |
| Cisco Packet Tracer | CCNA study labs and protocol simulation |
| Ubuntu Server 22.04 LTS | Linux management host OS |
| draw.io / diagrams.net | Network topology diagramming |
| Grafana + Prometheus | Network monitoring dashboards |
| Zabbix | SNMP-based device monitoring |
| Python + Netmiko | Network automation scripting |
| GitHub | Version control, documentation, portfolio |
| Notion | Internal runbooks and knowledge base |
MORE SOON

---

## 🗂️ Repository Structure(W.I.P+Planned)

```
├── /docs
│   ├── network-design-document.md       # Full NDD with design intent and rationale
│   ├── poe-budget-analysis.md           # PoE power budget calculations (3560X)
│   └── security-design-notes.md        # Security hardening decisions and rationale
│
├── /diagrams
│   ├── physical-topology-v1.png         # Rack elevation and physical cable plant
│   ├── logical-topology-l2-v1.png       # Layer 2 topology — VLANs and trunks
│   └── logical-topology-l3-v1.png       # Layer 3 topology — IP addressing and routing
│
├── /addressing
│   ├── ip-addressing-scheme.csv         # Full IP table — device, interface, IP, VLAN
│   └── vlan-register.csv               # VLAN register — ID, name, purpose, ports
│
├── /configs
│   ├── 3560X-core-config.txt           # Core switch running configuration
│   ├── 2960X-access-config.txt         # Access switch running configuration
│   └── router-config.txt              # Router running configuration
│
├── /runbooks
│   ├── initial-device-hardening.md     # Step-by-step secure baseline config
│   ├── vlan-provisioning.md           # Procedure for adding/removing VLANs
│   └── new-device-onboarding.md       # Process for adding devices to the network
│
└── /automation
    └── inventory-pull.py              # Python script — pulls show version from all devices
```

---

## 🌐 Network Design Summary

### Topology Model
Two-tier campus LAN architecture:
- **Access Layer** — Cisco 2960X: end-host connectivity, port security, access VLAN assignment
- **Distribution/Core Layer** — Cisco 3560X: inter-VLAN routing via SVIs, spanning tree root, PoE delivery

### VLAN Design
| VLAN ID | Name | Purpose | Subnet |
|---------|------|---------|--------|
| 10 | VLAN10_MGMT | Device management, SSH access | 10.10.10.0/24 |
| 20 | VLAN20_DATA | End-user hosts and workstations | 10.10.20.0/24 |
| 30 | VLAN30_VOICE | VoIP phones (future) | 10.10.30.0/24 |
| 40 | VLAN40_MONITOR | Network monitoring traffic | 10.10.40.0/24 |
| 99 | VLAN99_NATIVE | Hardened native VLAN (non-default) | N/A |

> **Design Note:** Native VLAN deliberately changed from default VLAN 1 across all trunk links to mitigate VLAN hopping attack vectors — a security hardening decision aligned with Cisco best practices.

### PoE Budget (Cisco Catalyst 3560X-48P)
| Spec | Value |
|------|-------|
| Total PoE Budget | 370W |
| Current Allocated | TBD (updating as devices added) |
| Headroom Remaining | TBD |
| Per-Port Maximum | 30W (PoE+) |

Full PoE budget worksheet with per-device calculations in `/docs/poe-budget-analysis.md`

---

## 🔒 Security Design Decisions

Every configuration decision in this lab has a documented security rationale:

- **SSH only** — Telnet disabled on all VTY lines. SSH v2 enforced.
- **Management VLAN isolation** — All device management interfaces placed on VLAN 10, isolated from user data traffic
- **Native VLAN hardening** — Default VLAN 1 not used as native VLAN anywhere in the topology
- **Unused port shutdown** — All unused switch ports administratively shut down and assigned to an unused VLAN
- **Console and enable passwords** — Encrypted using `service password-encryption` and type 9 secret where supported
- **AAA framework** — Planned for future integration with local user database before RADIUS/TACACS+

---

## 🔬 Virtual Lab (GNS3)

All configurations are first designed and validated in GNS3 using real Cisco IOS images before being applied to physical hardware. This mirrors the professional practice of staging changes in a test environment before production deployment.

GNS3 project files and validated configs are stored here prior to physical lab implementation. This provides:
- A safe environment to test and break things without affecting physical hardware
- A documented pre-deployment validation step in the change management process
- Ability to simulate more complex topologies than current physical hardware supports

---

## 📈 Monitoring & Observability

The Ubuntu Server micro PC serves as the lab's dedicated management and monitoring host:

- **SNMP polling** of all Cisco devices for interface utilization, CPU, memory
- **Grafana dashboards** for real-time and historical performance visualization
- **Syslog server** receiving logs from all network devices
- **Prometheus** for time-series metric collection

> Monitoring dashboard screenshots will be added to this section as the monitoring stack is deployed.

---

## 🤖 Automation

Basic network automation implemented using Python and Netmiko:

- `inventory-pull.py` — Connects to all devices via SSH, pulls `show version` and `show ip interface brief`, logs output to timestamped file
- Future: configuration compliance checker, automated backup script, VLAN provisioning script

---

## 🗺️ Project Roadmap

- [] Hardware acquisition (Phase 1)
- [x] Project documentation framework established
- [ ] IP addressing and VLAN design finalized
- [ ] Physical and logical topology diagrams complete
- [ ] GNS3 virtual lab built and validated
- [ ] Physical lab cabled and powered
- [ ] Baseline configuration applied to all devices
- [ ] Management VLAN and SSH access verified
- [ ] Inter-VLAN routing operational
- [ ] Monitoring stack deployed on Linux host
- [ ] PoE budget analysis documented with live device data
- [ ] Python automation scripts operational
- [ ] Wireless integration (WLC + CAPWAP simulation) — Phase 2
- [ ] Next-gen firewall integration (Cisco ASA/FTD) — Phase 2
- [ ] Network automation expansion (Ansible) — Phase 3

---

## 🧠 Skills Demonstrated

`Multi-layer LAN Design` `VLAN Segmentation` `802.1Q Trunking` `Layer 3 Inter-VLAN Routing`
`Spanning Tree Protocol (STP/RSTP)` `IP Addressing & Subnetting` `PoE Budget Analysis`
`Network Security Hardening` `SSH Configuration` `Access Control Lists (ACLs)`
`GNS3 Virtual Lab Simulation` `Cisco IOS CLI` `Network Documentation`
`Topology Diagramming` `Linux Server Administration` `SNMP Monitoring`
`Python Network Automation` `Netmiko` `Git Version Control`
`Technical Writing` `Change Management` `Runbook Development`

---

## 📖 Related Blog Posts

Documentation of this project's design decisions, lessons learned, and implementation notes is published on my technical blog:

> 🔗  *Link will be updated as posts are published*

---

## 👤 Author

**Stefan Peele**
Sophomore, Ying Wu College of Computing — New Jersey Institute of Technology
CCNA Candidate | Network Engineering Enthusiast | Aspiring Infrastructure Engineer

*updated as posts are published*

---

## 📄 Document Control

| Version | Date | Author | Notes |
|---------|------|--------|-------|
| 0.1 | 2026-04-04 | Stefan Peele | Initial project framework and documentation structure |

---

*This project is part of an ongoing self-directed engineering curriculum combining CCNA certification preparation with hands-on enterprise infrastructure design. All configurations represent best-effort lab implementations intended for learning and portfolio demonstration purposes.*
