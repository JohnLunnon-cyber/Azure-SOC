# Building a SOC + Honeynet in Azure (Live Traffic)
![HoneyNet   ![HoneyNet   SOC Graphic (2)](https://github.com/user-attachments/assets/a4eeeeb7-c1cd-45a1-9f6e-36d65fb27032)



## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which is then used by Microsoft Sentinel to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, apply some security controls to harden the environment, measure metrics for another 24 hours, then show the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Architecture Before Hardening / Security Controls
![Architecture Diagram] ![PUBLIC INTERNET BEFORE](https://github.com/user-attachments/assets/0b59a7c9-bdbc-44da-a329-496973914023)


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
![NSG Allowed Inbound Malicious Inbound<img width="1096" alt="BEFORE-nsg-malicious-allowed-in" src="https://github.com/user-attachments/assets/e4498128-c92f-4d16-a66c-3274332fa7e0" />
 <br>
![Linux Syslog Auth Failures]<img width="1234" alt="BEFORE-SYSLOG-SSH-FAILS" src="https://github.com/user-attachments/assets/da53afff-71e6-4ede-89a0-f0e23236e1ff" />
<br>
![Windows RDP/SMB Auth Failures]<img width="1051" alt="BEFORE-windows-rdp-auth-fail" src="https://github.com/user-attachments/assets/75e73ee4-c427-4a03-a421-54d32548f25b" />
<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 01-01-2025 10:51:50
Stop Time  02-01-2025 10:51:50

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 10523
| Syslog                   | 4032
| SecurityAlert            | 7
| SecurityIncident         | 104
| AzureNetworkAnalytics_CL | 1077

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 10-01-2025, 9:48:50
Stop Time	11-01-2025, 9:48:50

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 3458
| Syslog                   | 4
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

<h2>END RESULTS %</h2>
<img width="697" alt="Screen Shot 2025-01-12 at 12 38 13" src="https://github.com/user-attachments/assets/83931f3e-0ef5-40ed-b7af-e3a5b810f1fe" />


## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
