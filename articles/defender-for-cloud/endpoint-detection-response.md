---
title: Detect and configure endpoint detection and response solutions
description: Learn how to use Defender for Cloud recommendations to identify if an endpoint detection and response solution is installed on your virtual machine. You can also identify if there are any gaps in your security configuration and remediate the gaps if they exist.
author: dcurwin
ms.author: dacurwin
ms.topic: how-to
ms.date: 01/29/2024
---

# Detect and configure endpoint detection and response solutions

Microsoft Defender for Cloud's integration with Microsoft Defender for Endpoint provides automatic agent deployment to your servers and the ability to detect misconfigurations in previously installed endpoint detection and response solutions.

Defender for Cloud provides recommendations to secure and configure your endpoint detection and response solutions. By remediating these recommendations, you can ensure that your endpoint detection and response solution is compliant and secure across all environments.

The recommendations associated with Defender for Endpoint allow you to: 

- Identify if an endpoint detection and response solution is installed on your machines.  

- Identify gaps in the security configurations on any of the discovered endpoint detection and response solutions.

- Remediate detected gaps in your security configurations.

> [!NOTE]
> The Log Analytics agent (also known as MMA) is set to retire in [August 2024](https://azure.microsoft.com/updates/were-retiring-the-log-analytics-agent-in-azure-monitor-on-31-august-2024/). All Defender for Servers features that depend on the AMA, including those described on the [Enable Defender for Endpoint (Log Analytics)](endpoint-protection-recommendations-technical.md) page, will be available through either [Microsoft Defender for Endpoint integration](integration-defender-for-endpoint.md) or [agentless scanning](concept-agentless-data-collection.md), before the retirement date. For more information about the roadmap for each of the features that are currently rely on Log Analytics Agent, see [this announcement](upcoming-changes.md#defender-for-cloud-plan-and-strategy-for-the-log-analytics-agent-deprecation).

> [!IMPORTANT]
> Defender for Cloud recommends using the process described on this page to review endpoint detection and response solution installation status and configuration issues within your endpoint detection and response solution.

The following endpoint detection and response solutions are supported and can be enabled through Defender for Cloud

| Endpoint detection and response solution | Supported platforms | 
|--|--|
| Microsoft Defender for Endpoint for Windows | Windows |
| Microsoft Defender for Endpoint for Linux <sup>[1](#footnote1)</sup> | Linux | 
| Microsoft Defender for Endpoint Unified Solution <sup>[2](#footnote2)</sup>| Windows Server 2012 R2 <br> Windows 2016 |

<sup><a name="footnote1"></a>1</sup>: A Linux machine that has Microsoft Defender for Endpoint enabled, appears as healthy if the always-on scanning feature (also known as real-time protection (RTP)) is active. By default, the RTP feature is disabled to avoid clashes with other anti-virus software.

<sup><a name="footnote2"></a>2</sup>: The Defender for Endpoint unified solution on Server 2012 R2 automatically installs Microsoft Defender Antivirus in `Active mode`. For Windows Server 2016, Microsoft Defender Antivirus is built into the operating system.

## Prerequisites

- [Defender for Cloud](connect-azure-subscription.md) enabled on your Azure account.

- Enable either of the following plans enabled on Defender for Cloud:
    - [Defender for Servers Plan 2](tutorial-enable-servers-plan.md)
    - [Defender CSPM](tutorial-enable-cspm-plan.md)

- Enable [agentless scanning for your machines](enable-agentless-scanning-vms.md#enabling-agentless-scanning-for-machines).

## Detect an endpoint detection and response solution

Defender for Cloud provides recommendations when an endpoint detection and response solution isn't detected on your Azure virtual machines (VM), AWS EC2 instances or GCP VM instances.

### Detect an endpoint detection and response solution on your machine

1. Sign in to the [Azure portal](https://portal.azure.com/).

1. Navigate to **Microsoft Defender for Cloud** > **Recommendations**.

1. In the search field, enter the appropriate recommendation.

    - Azure VM - `Endpoint Detection and Response (EDR) solution should be installed on Virtual Machines`
    - AWS EC2 instances - `Endpoint Detection and Response (EDR) solution should be installed on EC2s`
    - GCP VM instances - `Endpoint Detection and Response (EDR) solution should be installed on Virtual Machines (GCP)`

1. Select the recommendation.

1. [Remediate the recommendation](implement-security-recommendations.md).

After the process is completed, it can take up to 24 hours until your machine appears in the Healthy resources tab 

## Configure an endpoint detection and response solution

When Defender for Cloud detects misconfigurations in your endpoint detection and response solution, recommendations will appear on the recommendations page that can assist you in the correction of those misconfigurationsn on your Azure VM, AWS EC2 instances or GCP VM instances.

### Configure an endpoint detection and response solution on Azure VM

1. Sign in to the [Azure portal](https://portal.azure.com/).

1. Navigate to **Microsoft Defender for Cloud** > **Recommendations**.

1. In the search field, enter the appropriate recommendation.

    - Azure VM - `Endpoint Detection and Response (EDR) configuration issues should be resolved on virtual machines`
    - AWS EC2 instances - `Endpoint Detection and Response (EDR) configuration issues should be resolved on EC2s`
    - GCP VM instances - `Endpoint Detection and Response (EDR) configuration issues should be resolved on GCP virtual machines`

1. Select the recommendation.

1. [Remediate the recommendation](implement-security-recommendations.md).

After the process is completed, it can take up to 24 hours until your machine appears in the Healthy resources tab 

## Using the fix option

Defender for Cloud includes an option that allows you to `Fix` machines that don't have an endpoint detection and response solution installed on them using the fix option. The fix option enables Microsoft Defender for Endpoint on your machines in your subscription. The Defender for Endpoint integration is included with both Defender for Servers Plan 1 and Plan 2.

If your machine isn't covered by either of the Defender for Servers plans, the fix asks you to enable Defender for Servers and select a plan. You can learn about the cost for each plan on the [Defender for Cloud pricing page](https://azure.microsoft.com/pricing/details/defender-for-cloud/?v=17.23h).

If the Defender for Endpoint integrations is configured correctly on your subscription, but the Defender for Endpoint extension isn't found on the machine, a manual troubleshooting reference appears as the fix option.

Defender for Servers Plan 1 includes the Defender for Endpoint agent provisioning feature. However, the discovery assessment is only available as part of the [agentless scanning](concept-agentless-data-collection.md) feature, which is included in Plan 2. Once you install the Defender for Endpoint agent on your machine, and only enable Plan 1, on the next scan it appears as `Not Applicable`.

After the fix process is completed, it can take up to 24 hours until your machine appears in the Healthy resources tab.

Learn how to [use the fix option](implement-security-recommendations.md#use-the-fix-option).

**Sample scenario 1**: If the Defender for Endpoint integration is configured correctly on your subscription but can't be found on your machine, the fix option presents a reference to manual troubleshooting.

**Sample scenario 2**: If you enable Defender for Servers Plan 1 on your machine, Defender for Endpoint's agent provisioning is included with Plan 1. In the next scan results, that machine is classified as `Not Applicable`.

## Next steps
