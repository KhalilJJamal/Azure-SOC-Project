# Azure SOC + Honeynet Project (With Live Traffic)

<!-- Azure Diagram -->
<div style="width: 100%; padding-top: 56.25%; position: relative;">
    <img src="azure_diagram.png" style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"/>
</div>
<small><em>OpenAI. (2024). ChatGPT [Large language model]. /g/g-pmuQfob8d-image-generator</em></small>

## Introduction

In this project, I built a hands-on cybersecurity incident response lab using Microsoft Azure, creating a honeynet exposed to the open Internet without security controls to collect data on malicious activity. Over 48 hours, I monitored traffic in this simulated SOC environment, ingesting logs from various sources into a Log Analytics workspace. Microsoft Sentinel created attack maps and security incidents based on alert triggers, which were resolved following the NIST 800-61 protocol. After implementing controls, including NIST 800-53 standards, I compared the results with another 48-hour monitoring period. Metrics were collected using KQL queries to assess the effectiveness of the security controls and ensure data security, including:

- SecurityEvent (Windows Event Logs)
- Syslog (Linux Event Logs)
- SecurityAlert (Log Analytics Alerts Triggered)
- SecurityIncident (Incidents created by Sentinel)
- AzureNetworkAnalytics_CL (Malicious Flows allowed into the honeynet).

## Before and After Security Controls
<div style="display: flex; justify-content: center; flex-wrap: wrap;">
    <img src="unsecure_network.png" alt="Before" width="45%" style="margin: 2px;">
    <img src="secure_network.png" alt="After" width="45%" style="margin: 2px;">
</div>

<small><em>OpenAI. (2024). ChatGPT [Large language model]. /g/g-pmuQfob8d-image-generator</em></small>


I created a Virtual Network (VNet) and set up Network Security Groups (NSGs) with zero restrictions to simulate an exposed network. I deployed two Windows and one Linux virtual machine to mimic a vulnerable environment. A Log Analytics Workspace was established for log aggregation and analysis. I used Azure Key Vault for secret management and an Azure Storage Account for data storage. Microsoft Sentinel was integrated for SIEM capabilities, creating an open honeynet for monitoring and analyzing threats in a non-isolated setting.

**Components**

- Azure Subscription
- Virtual Network (VNet)
- Network Security Group (NSG)
- Virtual Machines (2 Windows, 1 Linux)
- Log Analytics Workspace
- Azure Key Vault
- Azure Storage Account
- Microsoft Sentinel

## Usage
Create an Azure subscription:

1. Sign in to the Azure portal.

2. Navigate to "Subscriptions" and click "Add".

3. Enter a subscription name and select your billing account.

4. Choose a plan (e.g., Microsoft Azure Plan or Microsoft Azure Plan for DevTest).

5. Review details and click "Create".

Set up cost monitoring using Azure Cost Management to track expenses effectively.

Azure offers a free account with:
- $200 credit for the first 30 days
- Select free services for 12 months

To maximize your free trial:
- Explore free-tier services
- Use the Azure Price Calculator
- Set up budget alerts
- Monitor credit usage and free service limits

After the trial, you can continue using free services within limits or upgrade to a pay-as-you-go subscription.

Visit https://azure.microsoft.com/free to get started with your Azure free account.

## Methodology

Honeynet Deployment 

1. Create a vulnerable environment:
   - Set up Virtual Machines (VMs) with disabled firewalls
   - Configure Network Security Groups (NSGs) to allow all inbound traffic
   - Deploy resources like Key Vault and Storage with public endpoints (to entice attackers)

2. Establish logging and monitoring:
   - Configure Log Analytics Workspace to ingest logs from various sources
   - Set up Microsoft Sentinel for alert generation and incident creation
   - Enable Microsoft Defender for Cloud for security assessments

NIST 800-53 Implementation (Hardening After Attacks)

1. Apply security controls based on NIST 800-53 framework:
   - Implement strong network controls and logically segment subnets
   - Use Azure Policy to enforce compliance with NIST 800-53 requirements
   - Enable encryption for data at rest and in transit

2. Harden the environment:
   - Block all incoming traffic except from authorized sources
   - Implement private endpoints for Azure resources
   - Keep systems updated and patched regularly

Measuring Impact

1. Collect initial metrics:
   - Monitor the vulnerable environment for 24 hours
   - Record security events, incidents, and unauthorized access attempts

2. Apply security controls and re-measure:
   - Implement security measures based on Microsoft Defender recommendations
   - Observe the environment for another 24 hours post-hardening
   - Compare security metrics before and after implementing controls

3. Analyze results:
   - Evaluate the reduction in security events and incidents
   - Assess the effectiveness of implemented security measures
   - Identify areas for further improvement

## Results

"BEFORE" metrics, all resources were originally deployed, and exposed to the internet. The Virtual Machines had both their Network Security Groups and built-in firewalls wide open, and all other resources were deployed with public endpoints visible to the Internet; aka, no use for Private Endpoints.

"AFTER" metrics, Network Security Groups were hardened by blocking ALL traffic except my admin workstation, and all other resources were protected by their built-in firewalls as well as Private Endpoint.

## Attack Maps Before Hardening / Security Controls

<div style="display: flex; justify-content: space-around; flex-wrap: wrap;">
    
  <img src="nsg_malicious_allowed_in_Before.png" alt="NSG Allowed Inbound Malicious Flows" style="width: 64%; max-width: 500px; margin: 10px;">
  <img src="linux-ssh-auth-fail_Before.png" alt="Linux Syslog Auth Failures" style="width: 64%; max-width: 500px; margin: 10px;">
  <img src="windows-rdp-auth-fail_Before.png" alt="Windows RDP/SMB Auth Failures" style="width: 64%; max-width: 500px; margin: 10px;">
  
</div>

## Metrics Before Hardening / Security Controls

The following table shows the metrics we measured in our insecure environment for 24 hours:
Start Time 02-12-24 12:35:39
Stop Time 02-14-24 12:47:13

| Metric                   | Count |
| ------------------------ | ----- |
| SecurityEvent            | 148219 |
| Syslog                   | 3349  |
| SecurityAlert            | 6    |
| SecurityIncident         | 299   |
| AzureNetworkAnalytics_CL | 2588   |

## Attack Maps After Hardening / Security Controls

`All map queries returned no results due to no instances of malicious activity for the 24-hour period after hardening.`

## Metrics After Hardening / Security Controls

The following table shows the metrics we measured in our environment for another 24 hours, but after I applied security controls:
Start Time 02-14-24 13:51:37
Stop Time 02-16-24 13:56:18

| Metric                   | Count |
| ------------------------ | ----- |
| SecurityEvent            | 41710  |
| Syslog                   | 1    |
| SecurityAlert            | 1     |
| SecurityIncident         | 0     |
| AzureNetworkAnalytics_CL | 0     |

| Results | |
| --- | --- |
| Security Events (Windows VMs) | -71.86% |
| Syslog (Linux VMs) | -99.97% |
| SecurityAlert (Microsoft Defender for Cloud) | -83.33% |
| Security Incident (Sentinel Incidents) | -100.00% |
| NSG Inbound Malicious Flows Allowed | -100.00% |

## Conclusion

In this project, a mini honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel was employed to trigger alerts and create incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied, and then again after implementing security measures. It is noteworthy that the number of security events and incidents was drastically reduced after the security controls were applied, demonstrating their effectiveness.

As noted earlier, NIST 800-53 tends to be a reputable standard, but I found that more care is needed at the granular level to ensure security. Azure made its recommendations for improving my security posture. 

In this project, a mini honeynet was constructed in Microsoft Azure, and log sources were integrated into a Log Analytics workspace. Microsoft Sentinel triggered alerts and created incidents based on the ingested logs. Additionally, metrics were measured in the insecure environment before security controls were applied and after implementing security measures. Notably, the number of security events and incidents was drastically reduced after the security controls were used, demonstrating their effectiveness.

If regular users heavily utilized the resources within the network, more security events and alerts may have been generated within the 48 hours following the implementation of the security controls.


