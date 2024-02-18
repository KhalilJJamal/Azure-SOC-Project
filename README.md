# Azure SOC + Honeynet Project (With Live Traffic)

<!-- Azure Diagram -->
<div style="width: 100%; padding-top: 56.25%; position: relative;">
    <img src="azure_diagram.png" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"/>
</div>

## Introduction

In this project, I build a mini honeynet in Azure and ingest log sources from various resources into a Log Analytics workspace, which Microsoft Sentinel then uses to build attack maps, trigger alerts, and create incidents. I measured some security metrics in the insecure environment for 24 hours, applied some security controls to harden the environment, measured metrics for another 24 hours, then showed the results below. The metrics we will show are:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into our honeynet)

## Before and After Security Controls
*<small>Admittedly, this a theoretical model based on wishful thinking, but one could hope, right?<small>*
| Before Security Controls | After Security Controls |
| :----------------------: | :---------------------: |
| <img src="unsecure_network.png" alt="Before" width="410"/> | <img src="secure_network.png" alt="After" width="395"/> |

The architecture of the mini honeynet in Azure consists of the following components:

- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

For the "BEFORE" metrics, all resources were originally deployed, exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources are deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

For the "AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic with the exception of my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint.

## Attack Maps Before Hardening / Security Controls

![NSG Allowed Inbound Malicious Flows](https://i.imgur.com/1qvswSX.png)<br>
![Linux Syslog Auth Failures](https://i.imgur.com/G1YgZt6.png)<br>
![Windows RDP/SMB Auth Failures](https://i.imgur.com/ESr9Dlv.png)<br>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 2023-03-15 17:04:29
Stop Time 2023-03-16 17:04:29

| Metric                   | Count |
| ------------------------ | ----- |
| SecurityEvent            | 19470 |
| Syslog                   | 3028  |
| SecurityAlert            | 10    |
| SecurityIncident         | 348   |
| AzureNetworkAnalytics_CL | 843   |

## Attack Maps After Hardening / Security Controls

`All map queries actually returned no results due to no instances of malicious activity for the 24 hour period after hardening.`

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after we have applied security controls:
Start Time 2023-03-18 15:37
Stop Time 2023-03-19 15:37

| Metric                   | Count |
| ------------------------ | ----- |
| SecurityEvent            | 8778  |
| Syslog                   | 25    |
| SecurityAlert            | 0     |
| SecurityIncident         | 0     |
| AzureNetworkAnalytics_CL | 0     |

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents were drastically reduced after the security controls were applied, demonstrating their effectiveness.

It is worth noting that if the resources within the network were heavily utilized by regular users, it is likely that more security events and alerts may have been generated within the 24-hour period following the implementation of the security controls.

## Usage

Explain how to interact with the honeynet, monitor activities, and trigger incident responses.

## Methodology

Describe the approach taken to deploy the honeynet, implement NIST 800-53 standards, and measure the impact on incident response.

## Results

Present the metrics captured before and after deploying NIST 800-53 standards. Include any notable findings or improvements in incident response.

## Contributing

Provide guidelines for how others can contribute to the project or use it as a reference for their own implementations.

## License

Specify the license under which the project is distributed.

## Acknowledgements

