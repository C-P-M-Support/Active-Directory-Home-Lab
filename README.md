# Active-Directory-Home-Lab
VirtualBox lab featuring Active Directory, DHCP, DNS, GPO configuration, and Windows 10 domain joining.

# Enterprise Active Directory & Systems Administration Lab

**Author:** CPM Support  
**Target Roles:** Tier 1 IT Support / Help Desk Specialist / Systems Administrator  

---

## 📌 Project Overview
The objective of this project was to design, deploy, and manage an isolated virtual enterprise network environment using Oracle VM VirtualBox. This lab demonstrates core 1st-line and 2nd-line IT support competencies, including Active Directory Domain Services (AD DS), IPv4 scope configuration via DHCP, DNS resolution, Group Policy enforcement, and Windows 10 client domain integration.

---

## 📐 Network Architecture & Specs

| Role / Feature | Specification / Configuration |
| :--- | :--- |
| **Virtualization Software** | Oracle VM VirtualBox |
| **Network Type** | Isolated Internal Network (Host-Only / NAT dual-adapter topology) |
| **Domain Controller (DC)** | Windows Server 2019 |
| **Active Directory FQDN** | `domain.com` |
| **Static IP (Server)** | `192.168.10.10 /24` |
| **Client Workstation** | Windows 10 Enterprise (Domain-Joined) |
| **Client Network Config** | Dynamic IP via local DHCP |

*Note: `domain.com` was utilized for lab simplicity. Production enterprise deployments typically implement subdomains (e.g., `corp.domain.com`) to mitigate Split-Brain DNS conflicts.*

---

## 🛠️ Key Technical Configurations

### 1. Active Directory Domain Services (AD DS)
* Promoted Server to primary Domain Controller for `domain.com`.
* Designed a clean Organizational Unit (OU) structure mimicking an enterprise hierarchy (`HQ` -> `Departments` -> `Users` / `Groups` / `Workstations`).
* Provisioned user accounts with appropriate Security Groups and defined Role-Based Access Controls (RBAC).

### 2. Network Services (DHCP & DNS)
* **DHCP Scope:** Configured an active IPv4 pool (`192.168.10.100` – `192.168.10.200`) with an 8-day lease policy.
* **DNS Resolution:** Configured Forward and Reverse Lookup Zones; set up DNS forwarders for external web requests. +++++
* Verified proper option distribution (`Option 003 Router`, `Option 006 DNS Servers`, `Option 015 Domain Name`).

### 3. Group Policy Objects (GPO)
* Created and linked GPOs across specific OUs to enforce security baselines:
  * **Security Policy:** Enforced strong password complexity, minimum length, and account lockout thresholds after failed attempts. ++++
  * **Drive Mappings:** Configured automated login scripts to map shared network drives based on user group membership. ++++
  * **Desktop Control:** Disabled Control Panel access for standard non-admin users. +++++

### 4. Client Integration & End-User Support
* Successfully joined the **Windows 10** Virtual Machine to the `domain.com` domain. 
* Tested end-user authentication, privilege escalation, domain account resets, and account unlocking procedures. +++++

---

## 🔍 Verification & Command-Line Troubleshooting

To confirm network integrity and domain health, the following validation commands were executed and verified from the Windows 10 client terminal:

* `ipconfig /all` — Verified correct IP address assignment, subnet mask, default gateway, and DHCP lease from Server 2022.
* `nslookup domain.com` — Confirmed local DNS server resolves the domain FQDN accurately.
* `ping domain.com` — Tested end-to-end ICMP connectivity between client and Domain Controller.
* `gpresult /r` — Verified that linked Group Policy Objects applied successfully to the domain user.

---

## ⚙️ Key Takeaways & Problem Solving
* **Issue Encountered:** Client machine initially failed to join `domain.com` due to an IP assignment mismatch.
* **Resolution:** Reconfigured the client adapter settings from DHCP auto-assign to explicitly point to the DC's static IP (`192.168.10.10`) for primary DNS resolution before attempting the domain join.
