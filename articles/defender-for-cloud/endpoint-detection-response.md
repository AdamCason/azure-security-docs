---
title: Detect and configure endpoint detection and response solutions
description: Learn how to use Defender for Cloud recommendations to identify if an endpoint detection and response solution is installed on your virtual machine. You can also identify if there are any gaps in your security configuration and remediate the gaps if they exist.
author: dcurwin
ms.author: dacurwin
ms.topic: how-to
ms.date: 02/08/2024
---

# Detect and configure endpoint detection and response solutions

Microsoft Defender for Cloud's integration with Microsoft Defender for Endpoint provides automatic agent deployment to your servers and the ability to detect misconfigurations in previously installed endpoint detection and response solutions.

Defender for Cloud provides recommendations to secure and configure your endpoint detection and response solutions. By remediating these recommendations, you can ensure that your endpoint detection and response solution is compliant and secure across all environments.

The recommendations associated with Defender for Endpoint allow you to: 

- Identify if an endpoint detection and response solution is installed on your machines.  

- Identify gaps in the security configurations on any of the discovered endpoint detection and response solutions.

- Remediate detected gaps in your security configurations.

## Prerequisites

- [Defender for Cloud](connect-azure-subscription.md) enabled on your Azure account.

| Feature | Requirements |
|--|--|
| Detect endpoint detection and response solutions | You must have either of the following plans enabled on Defender for Cloud: <br> - [Defender for Servers plan 1 or plan 2](tutorial-enable-servers-plan.md) <br> [Defender CSPM](tutorial-enable-cspm-plan.md) <br> <br> You must enable [agentless scanning for virtual machines](enable-agentless-scanning-vms.md#enabling-agentless-scanning-for-machines) |
| Identify misconfigurations in endpoint detection and response solutions | You must have either of the following plans enabled on Defender for Cloud: <br> - [Defender for Servers plan 2](tutorial-enable-servers-plan.md) <br> [Defender CSPM](tutorial-enable-cspm-plan.md) <br> <br> You must enable [agentless scanning for virtual machines](enable-agentless-scanning-vms.md#enabling-agentless-scanning-for-machines) |

- Supported endpoint detection and response solutions:

    - **Microsoft Defender for Endpoint for Windows**
    - **Microsoft Defender for Endpoint for Linux** - Linux machines must have Microsoft Defender for Endpoint enabled. These types of machines appears as healthy if the always-on scanning feature (also known as real-time protection (RTP)) is active. By default, the RTP feature is disabled to avoid clashes with other anti-virus software.
    - **Microsoft Defender for Endpoint Unified Solution** - *Windows Server 2012 R2*, *Windows 2016* - The Defender for Endpoint unified solution on Server 2012 R2 automatically installs Microsoft Defender Antivirus in `Active mode`. For Windows Server 2016, Microsoft Defender Antivirus is built into the operating system.
    
## Detect an endpoint detection and response solutions on virtual machines

When Defender for Cloud can't detect an endpoint detection and response solution on Azure virtual machines (VM), AWS EC2 instances or GCP VM instances, it will present recommendations that help you install Defender for Endpoint as your endpoint detection and response solution.

Defender for Cloud has the ability to detect several endpoint detection and response solutions on various supported platforms. The following table contains the currently supported solutions and platforms: 

| Endpoint detection and response solution | Supported platforms |
|--|--|
| Microsoft Defender for Endpoint for Windows | Windows |
| Microsoft Defender for Endpoint for Linux1  | Linux (GA) |
| Microsoft Defender for Endpoint Unified Solution2  | Windows Server 2012 R2 and Windows 2016 |
| CrowdStrike (Falcon) | Windows and Linux |
| McAfee | Windows and Linux |
| Symantec | Windows and Linux |
| Sophos |  Windows and Linux |

<br>

**To detect an endpoint recommendations and response solutions on virtual machines**:

1. Sign in to the [Azure portal](https://portal.azure.com/).

1. Navigate to **Microsoft Defender for Cloud** > **Recommendations**.

1. Search for and select one of the following recommendations:

    - `EDR solution should be installed on Virtual Machines`
    - `EDR solution should be installed on EC2s`
    - `EDR solution should be installed on Virtual Machines (GCP)`

    :::image type="content" source="media/endpoint-detection-response/identify-recommendations.png" alt-text="Screenshot of the recommendations page showing the identify endpoint solution recommendations." lightbox="media/endpoint-detection-response/identify-recommendations.png":::

1. Select the relevant recommendation.

1. The recommendation offers multiple recommended actions to resolve on each attached machine, select the relevant option to see the remediation steps:

    - [Enable MDE integration](#enable-mde-integration)
    - [Upgrade defender plan](#upgrade-defender-plan)
    - [Troubleshoot issues](#troubleshoot-issues)

### Enable MDE integration

This recommended action will be present if an endpoint detection and response solution wasn't detected on the affected VM.

**To enable the Defender for Endpoint integration on the affected VM**:

1. Select the affected machine. 

1. (Optional) Select multiple affected machines that have the `Enable MDE integration` recommended action.

1. Select **Fix**.

    :::image type="content" source="media/endpoint-detection-response/enable-fix.png" alt-text="Screenshot that shows where the fix button is located." lightbox="media/endpoint-detection-response/enable-fix.png":::

1. Select **Enable**.

    :::image type="content" source="media/endpoint-detection-response/enable-endpoint.png" alt-text="Screenshot that shows the pop-up window from which to enable the Defender for Endpoint integration on.":::

After the process is completed, it can take up to 24 hours until your machine appears in the Healthy resources tab.

### Upgrade defender plan

This recommended action will be available if the affected VM doesn't have Defender for Servers Plan 1 or Plan 2 enabled on it.

**To enable the Defender for Servers on the affected VM**:

1. Select the affected machine. 

1. (Optional) Select multiple affected machines that have the `Upgrade defender plan` recommended action.

1. Select **Fix**.

    :::image type="content" source="media/endpoint-detection-response/upgrade-fix.png" alt-text="Screenshot that shows where the fix button is located on the screen." lightbox="media/endpoint-detection-response/upgrade-fix.png":::

1. In the plan selection drop-down menu, select **Plan 1** or **Plan 2**. Each plan comes with it's own cost, learn more about about he cost of each plan on the [Defender for Cloud pricing page](https://azure.microsoft.com/pricing/details/defender-for-cloud/).

1. Select **Enable**.

    :::image type="content" source="media/endpoint-detection-response/enable-plan.png" alt-text="Screenshot that shows the pop-up window that allows you to select which Defender for Servers plan to enable on your subscription.":::

After the process is completed, it can take up to 24 hours until your machine appears in the Healthy resources tab.

### Troubleshoot issues

This recommended action will be available if an endpoint detection and response solution was detected, but Defender for Endpoint failed to install successfully on the affected machine.

**To troubleshoot issues on your VM**:

1. Select the affected resource.

1. Select **Remediation steps**.

    :::image type="content" source="media/endpoint-detection-response/remediation-steps.png" alt-text="Screenshot that shows where the remediation steps are located in the recommendation." lightbox="media/endpoint-detection-response/remediation-steps.png":::

1. Follow the instructions to [troubleshoot Microsoft Defender for Endpoint onboarding issues](/microsoft-365/security/defender-endpoint/troubleshoot-onboarding?view=o365-worldwide).

After the process is completed, it can take up to 24 hours until your machine appears in the Healthy resources tab.

## Identify misconfigurations in endpoint detection and response solution

When Defender for Cloud detects misconfigurations in your endpoint detection and response solution, recommendations appear on the recommendations page that correct misconfigurations on your Azure VM, AWS EC2 instances and GCP VM instances. These recommendations are only be available if you have the Defender for Endpoint enabled on the VM. These recommendations check for the following security checks:

- `Scan are out of 7 days`
- `Signature out of date`
- `Anti-virus is off or partially configured`

**To identify misconfigurations in endpoint detection and response solution**:

1. Sign in to the [Azure portal](https://portal.azure.com/).

1. Navigate to **Microsoft Defender for Cloud** > **Recommendations**.

1. Search for and select one of the following recommendations:

    - `EDR configuration issues should be resolved on virtual machines`
    - `EDR configuration issues should be resolved on EC2s`
    - `EDR configuration issues should be resolved on GCP virtual machines`

    :::image type="content" source="media/endpoint-detection-response/configurable-solutions.png" alt-text="Screenshot that shows the recommendations that configure your endpoint detection and solution and remediate misconfigurations." lightbox="media/endpoint-detection-response/configurable-solutions.png":::

1. Select the relevant recommendation.

1. Select a security check to review the affected resources.

    :::image type="content" source="media/endpoint-detection-response/affected-resources.png" alt-text="Screenshot that shows a selected security check and the affected resources." lightbox="media/endpoint-detection-response/affected-resources.png":::

1. Select each security check to review all affected resources.

1. Expand the remediation steps and follow the instructions given.

After the process is completed, it can take up to 24 hours until your machine appears in the Healthy resources tab 

## Identify which endpoint detection and response solution is enabled on a VM

If you don't know which endpoint detection and response solution is enabled on your machine, you can check your healthy resources to see which solution is enabled on your machine.

**To identify which solution is enabled on a VM**:

1. Sign in to the [Azure portal](https://portal.azure.com).

1. Navigate to **Microsoft Defender for Cloud** > **Recommendations**.

1. Search for and select one of the following recommendations:

    - `EDR solution should be installed on Virtual Machines`
    - `EDR solution should be installed on EC2s`
    - `EDR solution should be installed on Virtual Machines (GCP)`

1. Select **Healthy resources**.

1. The Discovered EDRs column will display the solution that is detected.

    :::image type="content" source="media/endpoint-detection-response/discovered-solutions.png" alt-text="Screenshot of the Healthy resources tab which shows where you can see which endpoint detection and response solution is enabled on your machine.":::

## Next steps
