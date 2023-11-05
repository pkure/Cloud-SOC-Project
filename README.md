# Cloud-SOC-Project

# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

Microsoft Azure was used to create a honeyet, which included vulnerable Windows and Linux VMs. These VMs were opened up to the internet by turning their firewalls off and allowing all traffic to reach them, and a MS SQL Server was installed on the Windows VM. I created an 'attacker' VM to generate some additional logs/signin attempts for the 'honeynet VMs', including:

- SecurityEvent (Windows Event Logs)
- SecurityAlert (Log Analytics Alerts)
- Syslog (Linux Event Logs)
- MS SQL Server Login attempts (the Windows VM had an empty MS SQL Server running)
- SecurityIncident (Sentinel Incidents)
- AzureNetworkAnalytics_CL (Malicious Flows detected)

A Log Analytics Workspace was used to ingest the above log types from numerous sources, and KQL queries were used to observe and record a baseline for how many of which types of attacks were observed on the VMs. Microsoft Sentinel was used to create rules based on KQL queries, defining Incidents. 

Overall, the goal of the project would be to quantify a baseline amount of attacks/incidents/malicious attempts, then strengthen the cloud deployment's security posture based on NIST 800-53 standards tested per Azure, then re-record the amount of attacks/incidents/malicious attempts, to compare the updated security posture to the insecure one. In the process I simulated creating a basic (insecure cloud environment), hardening it based on NIST 800-53 standards, using inbound log data to define Security Incidents, worked through numerous Incidents using NIST 800-61 methodology, and ended up with a hardened Cloud SOC & environment based on NIST recommendations.


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
![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 19470
| Syslog                   | 3028
| SecurityAlert            | 10
| SecurityIncident         | 348
| AzureNetworkAnalytics_CL | 843

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8778
| Syslog                   | 25
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.
