# Lab Journal - - - April 5th, 2026 3:23 AM..

---

## Session 001 — April 5, 2026

### Objectives for the week
- Complete network documentation package
- Install and configure GNS3 virtual lab environment
- Confirm network study assets, Cisco Packet Tracer capabilities

### Completed
- Created professional GitHub repository with a full README intended for students, recruiters, and engineers
- Designed and committed IP Addressing Scheme (9 entries) (google sheets)
- Designed and committed VLAN Register (6 VLANs documented) (google sheets)
- Drew and committed Physical Topology v1.0 via Draw.io (visio isn't free)
- Drew and committed Logical Topology v1.0 via diagram.net (visio isn't free)
- Understood best effort practices when drawing up topologies and addressing
- Installed GNS3 2.2.57 on Windows 11
- Installed VMware Workstation Player 17
- Installed VirtualBox 7.0.20
- Imported GNS3 VM into VMware

### Challenges Encountered

**Issue 1 — VirtualBox Host-Only Network Driver Failure**
VirtualBox 7.2.6 failed to install network drivers on Windows 11.
Resolution: Downgraded to VirtualBox 7.0.20 which installed drivers correctly.

**Issue 2 — GNS3 VM KVM/Nested Virtualization Failure**
IOSv and IOSvL2 appliances require KVM which requires nested 
virtualization support. GNS3 VM failed to sustain stable operation.

Root cause identified through systeminfo diagnostic:
- Virtualization Based Security (VBS): Running
- Hypervisor Enforced Code Integrity: Running  
- App Control for Business: Enforced
- Hyper-V Requirements: Hypervisor detected

Windows 11 Home with enforced security policies (likely provisioning 
package) is preventing nested virtualization at the hardware level.
bcdedit and registry modifications were attempted but policies 
reapplied on reboot indicating deeper MDM/provisioning enforcement.

Resolution: GNS3 with IOSv/IOSvL2 not viable on current hardware 
configuration. Cisco Packet Tracer to be used for CCNA lab work. 
Physical lab hardware will serve as primary hands-on environment.
GNS3 with Dynamips images remains an option pending IOS image sourcing, though doubt it will work

### Key Technical Concepts Encountered
- Nested virtualization and KVM requirements
- Windows Virtualization Based Security (VBS)
- Hyper-V and its impact on third-party hypervisors
- MDM policy enforcement and provisioning packages
- VMware vs VirtualBox hypervisor capabilities

### Next Session Goals
- Continue Neil Anderson CCNA course Section 6
- Build first Packet Tracer lab
- Begin VLAN configuration practice

### Tools Used
- draw.io — network topology diagramming (diagram.net)
- GitHub — version control and documentation
- Git (commit syncing)
- Google Sheets — IP addressing and VLAN tables
- GNS3 2.2.57 — virtual lab platform
- VMware Workstation Player 17
- Windows Command Prompt — system diagnostics
- Windows Registry

---