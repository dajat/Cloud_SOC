# Building a SOC + Honeynet in Azure (Live Traffic)
![Cloud Honeynet / SOC](https://i.imgur.com/ZWxe03e.jpg)

## Introduction

In this project, I established a miniature honeynet infrastructure within the Azure environment. I have ingested log data from diverse sources into a centralized Log Analytics workspace. This resource was harnessed by Microsoft Sentinel to generate attack maps, trigger real-time alerts, and effectively manage security incidents. The methodology employed in this endeavor consisted of two distinct phases. Initially, I conducted a comprehensive 24-hour security metric assessment within the less-than-ideal security environment. Subsequently, I implemented a series of robust security controls to fortify the environment. After another 24-hour period, we observed and analyzed the following pivotal security metrics:

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
- Virtual Machines (2 window machines, 1 Linux machine)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

In the initial "BEFORE" phase, it's essential to highlight that all resources were initially exposed to the public internet. Specifically, the Virtual Machines had unrestricted Network Security Groups and open built-in firewalls, while other resources featured publicly accessible endpoints, obviating the requirement for Private Endpoints.

In the "AFTER" phase, a substantial enhancement in security posture was achieved. Network Security Groups underwent a comprehensive refinement, allowing traffic solely from a designated administrative workstation. Simultaneously, all other resources were fortified with robust built-in firewalls, and the implementation of Private Endpoints further reinforced their security, considerably diminishing exposure to external threats.

## Attack Maps Before Hardening / Security Controls
![NSG Allowed Inbound Malicious Flows](https://imgur.com/Sm7cwWE.png)<br>
![Linux Syslog Auth Failures](https://imgur.com/8aanTeT.png)<br>
![Windows RDP/SMB Auth Failures](https://imgur.com/HWKDfbe.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:

- Start Time: 2023-09-26T22:05:53.4115418Z
- Stop Time:  2023-09-27T22:05:53.4115418Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 44774
| Syslog                   | 2689
| SecurityAlert            | 4
| SecurityIncident         | 160
| AzureNetworkAnalytics_CL | 3799

## Attack Maps Before Hardening / Security Controls

```All map queries actually returned no results due to no instances of malicious activity for the 24-hour period after hardening.```

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we applied security controls:

- Start Time 2023-10-01T11:53:51.230373Z
- Stop Time	2023-10-02T11:53:51.230373Z

| Metric                   | Count
| ------------------------ | -----
| SecurityEvent            | 8904
| Syslog                   | 1
| SecurityAlert            | 0
| SecurityIncident         | 0
| AzureNetworkAnalytics_CL | 0

## Conclusion

In this project, we set up a small honeynet in Microsoft Azure and integrated log sources into a Log Analytics workspace. We used Microsoft Sentinel to trigger alerts and create incident reports from these logs. We first measured metrics in the insecure environment and then implemented security controls. Afterward, we measured metrics again. Notably, the security controls significantly reduced both security events and incidents, demonstrating their effectiveness.

It's worth mentioning that if regular users heavily utilized the network resources, we might have seen more security events and alerts in the 24 hours following the security control implementation.
