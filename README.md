# failed-RDP-to-IP-geolocation-information

<h2>Description</h2>
<b>The Powershell script in this repository is responsible for parsing out Windows Event Log information for failed RDP attacks and using a third party API to collect geographic information about the attackers location.
</b>
<br />
<br />
Set up Azure Sentinel (SIEM) and connected it to a live Azure virtual machine configured as a honeypot. Monitored real-time RDP brute force attempts from global sources. Used a PowerShell script to retrieve attacker IP geolocation data and visualized attack origins on an Azure Sentinel map for analysis.
<br />

<h2>Languages Used</h2>

- <b>PowerShell:</b> Extract RDP failed logon logs from Windows Event Viewer 

<h2>Utilities Used</h2>

- <b>ipgeolocation.io:</b> IP Address to Geolocation API
