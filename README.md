# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://github.com/user-attachments/assets/6544c7ba-895f-4023-9376-6f95975875c3)



## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/aBDwnKb.jpg)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://i.imgur.com/YQNa9Pp.jpg)

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/user-attachments/assets/f3b5648f-3da7-4a01-8a0d-c4be130cd353)
<br>
![Linux Syslog Auth Failures](https://github.com/user-attachments/assets/7d0fe8de-6da3-4612-8035-01705923adce)
<br>
![Windows RDP/SMB Auth Failures](https://github.com/user-attachments/assets/d6e3bb74-c5cf-4d57-be60-950b4e57bb49)
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2024-11-20 08:28:15 PM
Stop Time 2024-11-21 08:28:15 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 238827
| Syslog                   | 28133
| SecurityAlert            | 0
| SecurityIncident         | 322
| AzureNetworkAnalytics_CL | 1676

## Attack Maps After Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2024-12-1 07:43:54 PM
Stop Time	2024-12-2 07:43:54 PM

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 524
| Syslog                   | 1232
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
