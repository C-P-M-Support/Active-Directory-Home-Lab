# Active Directory Home Lab & PowerShell Automation

**Author:** C-P-M Support  
**Target Roles:** Tier 1 IT Support / Help Desk Specialist / Systems Administrator  

---

## 📌 Project Overview
Deployed an isolated, dual-adapter virtual network using Oracle VM VirtualBox to simulate an enterprise Active Directory environment. Configured a Windows Server 2019 Domain Controller with NAT/RRAS to route internet traffic to internal client machines, established DHCP for dynamic IP assignment, and executed custom PowerShell scripts to bulk-provision ~1,000 domain users from a CSV file.

---

## 📐 Architecture & Network Specifications

| Component | Detail / Configuration |
| :--- | :--- |
| **Virtualization** | Oracle VM VirtualBox |
| **Domain Controller (DC)** | Windows Server 2019 |
| **Client Workstation** | Windows 10 |
| **NIC 1 (DC)** | NAT (Internet-Facing) |
| **NIC 2 (DC)** | Internal Network (`192.168.10.1` - Static Gateway) |
| **Client NIC** | Internal Network (Dynamic IP via Domain Controller DHCP) |
| **Routing Protocol** | Routing and Remote Access Service (RRAS / NAT) |

---

## 🛠️ Configurations Completed

### 1. Network Routing & Core Infrastructure
* Provisioned dual network interface cards (NICs) on the Domain Controller to create an isolated internal network while maintaining external internet access through NAT.
* Configured **Routing and Remote Access Services (RRAS)** with NAT to enable internal Windows 10 client traffic to route through the server out to the public internet.
* Established a dedicated **DHCP Scope** (`192.168.10.100` – `192.168.10.200`) to automatically distribute IP addressing, gateway pointers, and DNS settings with an 8-day lease duration.

### 2. Active Directory Services & Automation
* Promoted server to primary Domain Controller and initialized the domain.
* Designed Organizational Unit (OU) structures to manage users and administrative accounts.
* **PowerShell Automation:** Developed and executed an automated PowerShell script to ingest user data from a CSV file, dynamically format usernames/passwords, and bulk-create ~1,000 user accounts in Active Directory.

### 3. Client Integration & Verification
* Configured the Windows 10 client workstation to obtain IP parameters dynamically via the internal network.
* Successfully joined the Windows 10 client to the Active Directory domain and verified domain authentication.
* Confirmed end-user logon functionality using both domain administrator accounts and generated bulk user credentials.

---

## 🔍 Command-Line Troubleshooting & Verification
Executed terminal commands on the client machine to confirm network connectivity and domain integrity:

* `ipconfig /all` — * `ipconfig /all` — Confirmed the Windows 10 client received an assigned IP address from the pool (e.g., `192.168.10.100`) and verified that Default Gateway and DNS point to the Domain Controller (`192.168.10.1`).
* `ipconfig /release` & `ipconfig /renew` — Tested DHCP lease release and renewal on the local network adapter.
* `ping 8.8.8.8` & `ping google.com` — Confirmed operational NAT routing through RRAS to external destinations, verifying web browser connectivity.
* `nslookup` — Verified local host name resolution against the Domain Controller.
