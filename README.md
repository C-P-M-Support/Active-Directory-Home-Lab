# Active Directory Home Lab & PowerShell Automation

**Author:** C-P-M Support  
**Target Roles:** Tier 1 IT Support / Help Desk Specialist / Systems Administrator  

---

## 📌 Project Overview
Deployed an isolated, dual-adapter virtual network using Oracle VM VirtualBox to simulate an enterprise Active Directory environment. Configured a Windows Server Domain Controller with NAT/RRAS to route internet traffic to internal client machines, set up DHCP for dynamic IP assignment, and executed custom PowerShell scripts to bulk-provision ~1,000 domain users.

---

## 📐 Architecture & Network Specifications

| Component | Detail / Configuration |
| :--- | :--- |
| **Virtualization** | Oracle VM VirtualBox |
| **Domain Controller (DC)** | Windows Server |
| **Client Workstation** | Windows 10 |
| **NIC 1 (DC)** | NAT (Internet-Facing) |
| **NIC 2 (DC)** | Internal Network (`192.168.10.1 /24` - Static Gateway) |
| **Client NIC** | Internal Network (Dynamic IP via DC DHCP) |
| **Routing Protocol** | Routing and Remote Access Service (RRAS / NAT) |

---

## 🛠️ Configurations Completed

### 1. Network Routing & Core Infrastructure
* Provisioned dual network interface cards (NICs) on the Domain Controller to create an isolated internal network while maintaining external internet access.
* Configured **Routing and Remote Access Services (RRAS)** with NAT to allow internal Windows 10 clients to route traffic through the server to the internet.
* Configured a dedicated **DHCP Scope** (`192.168.10.100` – `192.168.10.200`) to automatically distribute IP addresses, gateway pointers, and DNS settings.

### 2. Active Directory Services & Automation
* Promoted server to primary Domain Controller and created the domain.
* Designed Organizational Unit (OU) structures for users and administrative accounts.
* **PowerShell Automation:** Executed an automated PowerShell script to pull user data from a CSV file, dynamically generate usernames/passwords, and bulk-create ~1,000 user accounts in Active Directory.

### 3. Client Integration & Verification
* Configured the Windows 10 client machine to obtain IP addressing dynamically via the internal network.
* Joined the client workstation to the Active Directory domain and verified domain authentication.
* Tested end-user logon using both created administrative credentials and generated bulk user accounts.

---

## 🔍 Command-Line Troubleshooting & Verification
Executed terminal commands on the client machine to confirm connectivity and domain health:

* `ipconfig /all` — Verified IP allocation from the Server's DHCP scope (`192.168.10.x`) and confirmed DNS points to `192.168.10.1`.
* `ping 8.8.8.8` & `ping google.com` — Confirmed operational NAT routing through RRAS to external web destinations.
* `nslookup` — Verified local DNS resolution against the Domain Controller.
