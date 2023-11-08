# Cloud-SOC-Project

# Building a SOC + Honeynet in Azure
![Cloud Honeynet / SOC](https://github.com/pkure/Cloud-SOC-Project/assets/108906109/9ed0af00-d4cd-4837-9ef3-f9637890b4de)





## Introduction

This project involved seting up an insecure cloud environment, measuring real attacks against it (from the open internet), then hardening everything and re-measuring how many security incidents there are after setting up a SOC and using NIST standards to improve security posture!

Microsoft Azure was used to create a honeyet, which included vulnerable Windows and Linux VMs. These VMs were exposed to the open internet by turning any firewalls off and allowing all traffic to reach them, additionally a MS SQL Server was installed on the Windows VM. I created an 'attacker' VM to generate some additional logs/signin attempts for the 'honeynet VMs', including:

- SecurityEvent (Windows Event Logs)
- SecurityAlert (Log Analytics Alerts)
- Syslog (Linux Event Logs)
- MS SQL Server Login attempts (the Windows VM had an empty MS SQL Server running)
- SecurityIncident (Sentinel Incidents)
- AzureNetworkAnalytics_CL (Malicious Flows detected)

A Log Analytics Workspace was used to ingest the above log types from numerous sources, and KQL queries were used to observe and record a baseline for how many of which types of attacks were observed on the VMs. Microsoft Sentinel was used to create rules based on KQL queries, defining Incidents. 

Overall, the goal of the project would be to quantify a baseline amount of attacks/incidents/malicious attempts, then strengthen the cloud deployment's security posture based on NIST 800-53 standards tested per Azure, then re-record the amount of attacks/incidents/malicious attempts, to compare the updated security posture to the insecure one. In the process I simulated creating a basic (insecure cloud environment), hardening it based on NIST 800-53 standards, using inbound log data to define Security Incidents, worked through numerous Incidents using NIST 800-61 methodology, and ended up with a hardened Cloud SOC & environment based on NIST recommendations.


## Architecture Before Hardening / Security Controls
![Architecture Diagram](https://github.com/pkure/Cloud-SOC-Project/assets/108906109/79b6a7ff-9d1d-4eab-9bf2-d44c6ebffc62)
)

## Architecture After Hardening / Security Controls
![Architecture Diagram](https://github.com/pkure/Cloud-SOC-Project/assets/108906109/db60f6a9-ee22-4302-877d-a0d345564243)

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://github.com/pkure/Cloud-SOC-Project/assets/108906109/7d5de7dd-62e0-4bc7-836e-3ac08a11e896)<br>
![Linux Syslog Auth Failures](https://github.com/pkure/Cloud-SOC-Project/assets/108906109/d836299e-fc66-43af-ab92-fd55b7b5fbe1)<br>
![Windows RDP/SMB Auth Failures](https://github.com/pkure/Cloud-SOC-Project/assets/108906109/d238fcaa-8bb6-4d5d-bf23-d5ef22c895b7)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-11-02T19:15:39.8787179Z
Stop Time 2023-11-03T19:15:39.8787179Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 3208
| Syslog                   | 39
| SecurityAlert            | 2
| SecurityIncident         | 70
| AzureNetworkAnalytics_CL | 354

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time	2023-03-19 15:37

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 3
| Syslog                   | 7
| SecurityAlert            | 2
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 1

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was used to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

Of note, this project will be re-ran to confirm the results and to practice deploying the cloud infrastructure present.
