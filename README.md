# Cloud-SOC-Honeynet

# Building a SOC + Honeynet in Microsoft Azure (Live Traffic) Comprehensive Summary

![68747470733a2f2f692e696d6775722e636f6d2f5a5778653033652e6a7067](https://github.com/Nick-Errington/Cloud-SOC-Honeynet/blob/main/Achitecture-Topology/topology-diagram.jpg)


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

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

## Attack Maps Before Hardening / Security Controls

For the "BEFORE" metrics, all resources were originally deployed, and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

![Linux SSH Auth Failure (Before)](4)

![MySQL Authentication Failures(Before)](5)

![nsg-malicious-allowed-in (before)](6)

![Windows RDP   SMB Authentication Failure(Before)](7)


## Metrics Before Hardening / Security Controls

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic except for my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint


![Windows RDP   SMB Authentication Failure](8)

![Linux SSH Auth Failure](9)

![MySQL Authentication Failures](10)

![nsg-malicious-allowed-in](11)

```All map queries returned no results due to no instances of malicious activity for the 24-hour period after hardening.```

The following table shows the metrics we measured in our insecure environment for 24 hours:

Start Time 2023-011-19 18:17

Stop Time	2023-11-20 18:17
<div>

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19778
| Syslog                   | 7748
| SecurityAlert            | 7
| SecurityIncident         | 322
| AzureNetworkAnalytics_CL | 3236


## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we applied security controls:

Start Time 2023-11-22 19:20

Stop Time	2023-11-23 19:20

| Metric                   | Count | Change post-hardening
| ------------------------ | ----- | --------------------- 
| SecurityEvent            | 0 | 100%
| Syslog                   | 0 | 100%
| SecurityAlert            | 0 | 100%
| SecurityIncident         | 0 | 100%
| AzureNetworkAnalytics_CL | 0 | 100%

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents was drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24 hours following the implementation of the security controls.
