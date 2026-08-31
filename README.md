**Author:** C-P-M-Support
**Target Roles:** Tier 1 IT Support / Help Desk Specialist / Systems Administrator

## 📌 Project Overview

I built this lab to get hands-on experience with enterprise infrastructure. The goal was to simulate a real-world environment using **Windows Server 2019** and **VirtualBox**, focusing on the core services you'd see in a corporate IT department: Active Directory, DHCP, DNS, and security policies.

Beyond just setting things up, I wanted to prove I could troubleshoot issues  and automate tasks using **PowerShell**. I also implemented strict **Group Policy** security rules and verified they actually work on client machines.

## 📐 Architecture & Network Specs

| Component | Configuration |
| :--- | :--- |
| **Virtualization** | Oracle VM VirtualBox |
| **Domain Controller** | Windows Server 2019 (`mydomain.com`) |
| **Client Workstation** | Windows 10 (`CLIENT1`) |
| **Network Design** | Dual-NIC DC (Internet + Internal) |
| **Internal Subnet** | `172.16.0.0/24` |
| **DHCP Scope** | `172.16.0.100` – `172.16.0.200` |
| **DNS Forwarders** | `8.8.8.8`, `1.1.1.1` |

## 🛠️ What I Configured

### 1. Network Routing & Core Infrastructure
I set up a dual-NIC Domain Controller to act as the gateway. One card talks to the internet, the other handles the internal private network. I configured RRAS NAT so the clients could browse the web while staying on a private subnet.

![RRAS NAT Setup](/images/02_rras_nat.png)

I then built a DHCP scope to hand out IPs automatically. No more static IP's.

![DHCP Scope Pool](/images/03_dhcp_scope.png)
![DHCP Active Leases](/images/04_dhcp_scope.png)

Finally, I pointed DNS forwarders to public resolvers so internal clients could resolve external names without breaking.

![DNS Forwarders](/images/05_dns_forwarders.png)

### 2. Active Directory & Automation
 I wrote a **PowerShell script** to bulk-provision domain users, simulating a real onboarding workflow. Capable of writing in powershell scripting to adjust to individual formats or needs. 

![PowerShell Script Execution](/images/06_powershell_script.png)

### 3. Domain Join & Connectivity Verification
I joined `CLIENT1` to the domain. Before trusting the setup, I verified connectivity by pinging the domain controller and checking DNS resolution.

![Network Configuration & Domain Join](/images/01_network_config.png)
![Ping Test Verification](/images/07_ping_test.png)

### 4. Group Policy & Security Enforcement (The Real Test)
Set a password policy; I **tested it to break it**.


**The Goal:** Enforce a 12-character minimum password with complexity requirements.
**The Test:** I tried to set a user's password to `123`.

![GPO Password Policy Configuration](/images/08_gpo_password_policy.png)

*Configured the rule: 12 chars minimum + complexity.*

**Attempt 1: Weak Password**
I tried entering `123`. The system immediately blocked it.
![Weak Password Entry](/images/09_password_length.png)
![Policy Violation Error](/images/10_password_enforcement_error.png)
*Result: "The password does not meet the password policy requirements."*

**Attempt 2: Compliant Password**
I entered a strong password (`SecureP@ss123`). The system accepted it.
![Compliant Password Entry](/images/11_password_correct_length.png)
![Password Change Successful](/images/12_gpresult_applied.png)
*Result: Change accepted. Policy working.*

I verified the policy was actually applied on the client using `gpresult /r`.

![GPResult Output](/images/12_gpresult_applied.png)

## 🧠 Challenges & Lessons Learned

*   **NAT Routing:** At first, clients had no internet. I realized I hadn't bound the NAT to the correct external NIC. Fixed by explicitly selecting the interface.
*   **DNS Forwarders:** Clients could join the domain but couldn't load Google. Adding `8.8.8.8` as a forwarder solved it. Learned the difference between *internal* AD DNS and *external* resolution.
*   **Password Policy:** Setting a policy & verifying it works . The screenshots above prove the policy is active, not just configured.

## 🎯 Skills Demonstrated

*   **Infrastructure:** Windows Server 2019, Active Directory, DHCP, DNS, RRAS/NAT
*   **Security:** Group Policy Objects (GPO), Password Policies, Security Baselines
*   **Automation:** PowerShell Scripting (Bulk User Provisioning)
*   **Virtualization:** Oracle VM VirtualBox, Network Adapter Configuration
*   **Troubleshooting:** Network connectivity, DNS resolution, Policy enforcement

---
