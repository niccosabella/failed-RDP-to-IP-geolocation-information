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

### 3. Viewing Raw Logs on the VM




---

### 4. Connecting the VM to Log Analytics Workspace

Connecting the virtual machine to a Log Analytics workspace in Microsoft Azure enables centralized collection of system and security data.

An agent is installed on the VM to send logs, performance metrics, and event data to the workspace. This allows monitoring, querying, and analysis of activity such as login attempts, system changes, and network events.

The connection supports visibility into the VM and is commonly used for threat detection, auditing, and troubleshooting.

---

### 5. Uploading Geolocation Data to the SIEM

Uploading geolocation data to the SIEM in Microsoft Azure adds location context to collected log data.

A dataset named “geoip” is imported into the environment and used to map IP addresses to geographic locations such as country, region, and city. This data can then be joined with existing logs in the Log Analytics workspace.

This allows events, such as login attempts or attacks, to be enriched with location details for better visibility and analysis.

---
### 6. Querying the Log Repository with KQL

Querying the log repository with KQL (Kusto Query Language) in Microsoft Azure allows structured analysis of collected log data.

KQL is used to search, filter, and sort data from sources such as security events, sign-in logs, and system activity stored in the Log Analytics workspace. Queries can isolate specific behaviors, such as failed login attempts or unusual network activity.

This process enables efficient investigation, pattern detection, and extraction of actionable insights from large volumes of log data.

**Query used to locate events:**

```kql
let GeoIPDB_FULL = _GetWatchlist("geoip");
let WindowsEvents = SecurityEvent;
WindowsEvents | where EventID == 4625
| order by TimeGenerated desc
| evaluate ipv4_lookup(GeoIPDB_FULL, IpAddress, network)
| summarize FailureCount = count() by IpAddress, latitude, longitude, cityname, countryname
| project FailureCount, AttackerIp = IpAddress, latitude, longitude, city = cityname, country = countryname,
friendly_location = strcat(cityname, " (", countryname, ")");
```
