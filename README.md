# failed-RDP-to-IP-geolocation-information

## Description
The PowerShell script in this repository is responsible for parsing Windows Event Log information for failed RDP attacks and using a third-party API to collect geographic information about the attackers' locations.

Set up Azure Sentinel (SIEM) and connected it to a live Azure Virtual Machine configured as a honeypot. Monitored real-time RDP brute force attempts from global sources. Used a PowerShell script to retrieve attacker IP geolocation data and visualized attack origins on an Azure Sentinel map for analysis.

## Languages Used
- **PowerShell**: Extract RDP failed logon logs from Windows Event Viewer

## Utilities Used
- **ipgeolocation.io**: IP Address to Geolocation API

## Steps Taken

### 1. Virtual Machine Creation 

A Windows 11 virtual machine was deployed in Microsoft Azure as a test system for security and network analysis.

The VM was provisioned with standard settings and made accessible over the internet. At the operating system level, Windows Firewall was disabled to remove all host-based traffic filtering. This allowed unrestricted inbound and outbound communication directly to the system.

---

### 2. Configure Network Security Group (NSG) 

The Network Security Group (NSG) associated with the virtual machine was configured with permissive rules.

Inbound and outbound rules were set to allow all ports, all protocols, and all source and destination addresses. This removed all network-level access restrictions.

With both the firewall disabled and NSG fully open, the virtual machine became fully exposed to external traffic. This setup supports security testing, vulnerability scanning, and monitoring of unsolicited connection attempts.

---

### 3. Viewing Raw Logs on the Virtual Machine

