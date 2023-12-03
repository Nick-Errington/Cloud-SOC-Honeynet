# Cloud-SOC-Honeynet

# Building a SOC + Honeynet in Microsoft Azure (Live Traffic) Comprehensive Summary

![68747470733a2f2f692e696d6775722e636f6d2f5a5778653033652e6a7067](https://github.com/Nick-Errington/Cloud-SOC-Honeynet/blob/main/Achitecture-Topology/topology-diagram.PNG)


## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measured metrics for another 24 hours, and then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/Nick-Errington/Cloud-SOC-Honeynet/blob/main/Achitecture-Topology/architecture-before.jfif)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/Nick-Errington/Cloud-SOC-Honeynet/blob/main/Achitecture-Topology/architecture-after.jfif)

The architecture of the mini honeynet in Azure consists of the following tools and components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Azure Key Vault
- Azure Storage Account
- Microsoft SQL Server
- SQL Server Management Studio (SSMS)
- Azure Active Directory
- PowerShell

Additionally, the SOC utilized the following tools, components and regulations: 
- Microsoft Sentinel (SIEM)
- Microsoft Defender for Cloud (MDC)
  - [NIST SP 800-53 Revision 4](https://csrc.nist.gov/publications/detail/sp/800-53/rev-4/archive/2015-01-22)
- Log Analytics Workspace
- Windows Event Viewer
- Kusto Query Language (KQL)

## Attack Maps Before Hardening / Security Controls

For the "BEFORE" metrics, all resources were originally deployed, and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

### Linux SSH Authentication Failures:
![Linux SSH Auth Failure (Before)](https://github.com/Nick-Errington/Cloud-SOC-Honeynet/blob/main/Attack-Maps/linux-ssh-auth-fail-before.PNG)
<br />

### MS SQL Server Authentication Failures:
![MySQL Authentication Failures (Before)](https://github.com/Nick-Errington/Cloud-SOC-Honeynet/blob/main/Attack-Maps/mssql-auth-fail-before.PNG)
<br />

### NSG Allowed Malicious Inbound Flows:
![nsg-malicious-allowed-in (Before)](https://github.com/Nick-Errington/Cloud-SOC-Honeynet/blob/main/Attack-Maps/nsg-malicious-allowed-in-before.PNG)
<br />

### Windows RDP/SMB Authentication Failures:
![Windows RDP   SMB Authentication Failure (Before)](https://github.com/Nick-Errington/Cloud-SOC-Honeynet/blob/main/Attack-Maps/windows-rdp-auth-fail-before.PNG)
<br />

## After Hardening Measures and Security Controls

In the "AFTER" stage, based off the incidents created from the "Before" 24 hour capture, I implemented hardening measures and security controls to improve the environment's security from attackers.<br /> 
These improvements included:

- <b>Network Security Groups (NSGs)</b>: I hardened the NSGs by only allowing my own public IP address to come thrugh otherwise all other traffic would be blocked by the new parameteres created.

- <b>Built-in Firewalls</b>: In my virtual machines I configured the built-in firewalls so that it would deny access from unauthorized users. 

- <b>Private Endpoints</b>: For other Azure resources, I replaced the public endpoints with Private Endpoints. This ensured that access to sensitive resources, such as storage accounts and databases, was limited to only the virtual network.

## Attack Maps After Hardening / Security Controls

<br />

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

 <br />
 
## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:<br/>
Start Time 2023-011-19 18:17<br/>
Stop Time	2023-11-20 18:17
<div>

| Metric                   | Count
| ------------------------ | -----
| Security Events (Windows VMs)| 19778
| Syslog (Linux VMs)           | 7748
| SecurityAlert (Microsoft Defender for Cloud)| 7
| Security Incident (Sentinel Incidents)| 322
| NSG Inbound Malicious Flows Allowed| 3236


## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we applied security controls:<br/>
Start Time 2023-11-22 19:20<br/>
Stop Time	2023-11-23 19:20

| Metric                   | Count | Change post-hardening
| ------------------------ | ----- | --------------------- 
| Security Events (Windows VMs)| 9367 | 52.64%
| Syslog (Linux VMs)       | 1 | 99.99%
| SecurityAlert (Microsoft Defender for Cloud)| 0 | 100%
| Security Incident (Sentinel Incidents)| 0 | 100%
| NSG Inbound Malicious Flows Allowed| 0 | 100%

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents was drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24 hours following the implementation of the security controls.
