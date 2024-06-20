# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I implemented a mini honeynet in Azure and integrated log sources from various resources into a Log Analytics workspace. This workspace was utilized by Microsoft Sentinel to develop attack maps, trigger alerts, and generate incidents. I measured specific security metrics in the unsecured environment over a 24-hour period. Subsequently, I applied security controls to fortify the environment and measured the metrics again for another 24 hours. The results of these measurements are presented below. The metrics we will demonstrate are:

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
![NSG Allowed Inbound Malicious Flows](https://github.com/JulioV201/Azure-SOC/assets/172655588/3fdd08a7-896f-4cdb-9e76-c0e79ecb9538)<br>
![Linux Syslog Auth Failures](https://github.com/JulioV201/Azure-SOC/assets/172655588/d6ea39cd-2a96-4495-8aa3-9f38c32fbe91)<br>
![Windows RDP/SMB Auth Failures](https://github.com/JulioV201/Azure-SOC/assets/172655588/3f07e9a0-3d39-464a-a70c-9bf55cb7056b)<br>

## Metrics Before Hardening / Security Controls

<br>The following table shows the metrics we measured in our insecure environment for 24 hours:</br>
<br>Start Time 2024-06-12 14:51:07</br>
Stop Time  2024-06-13 14:51:07

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 21923
| Syslog                   | 26250
| SecurityAlert            | 4
| SecurityIncident         | 279
| AzureNetworkAnalytics_CL | 1360

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

<br>The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:</br>
<br>Start Time 2024-06-18 02:15:29.66</br>
Stop Time	 2024-06-19 02:15:29.66

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 9186
| Syslog                   | 0
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
