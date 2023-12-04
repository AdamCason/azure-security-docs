---
title: Prepare for deprecation of the Log Analytics MMA agent 
description: Learn how to prepare for the deprecation of the Log Analytics MMA agent in Microsoft Defender for Cloud
author: AlizaBernstein
ms.author: v-bernsteina
ms.topic: how-to
ms.date: 12/04/2023
---

# Prepare for deprecation of the Log Analytics (MMA) agent

The Azure Log Analytics, Microsoft Monitor Agent (MMA) is set to be [retired in August 2024](upcoming-changes.md#defender-for-cloud-plan-and-strategy-for-the-log-analytics-agent-deprecation). If you’ve deployed the Defender for Servers or Defender for SQL Server on Machines plans within Microsoft Defender for Cloud, here’s how you should prepare.

## Prepare Defender for Servers

Defender for Servers has always been reliant on the log analytics agent (known as Microsoft Monitoring Agent (MMA)) as mandatory and the recommended agent for posture and threat protection capabilities in Azure and multicloud.

Toward Log Analytics Agent (MMA) retirement, and as part of an updated deployment strategy, with a goal to simplify the onboarding, all Defender for Servers security features and capabilities will be provided via a single agent (Microsoft Defender for Endpoint (MDE)) complemented by agentless scanning, without dependency on either Log Analytics Agent (MMA) or Azure Monitoring Agent (AMA), the substitute agent.  

As a result, all Defender for Servers features and capabilities currently relying on Log Analytics Agent (MMA) will be deprecated in their Log Analytics version in August 2024, and delivered over one of the alternative infrastructures mentioned above.

## How should you prepare?  

### Enable Defender for Endpoint integration/agentless scanning

Enable Defender for Endpoint integration and agentless disk scanning on your subscriptions. This will ensure you’ll seamlessly be up-to-date and receive all the alternative deliverables once they are provided, and without any additional migration. Learn more about endpoint protection and agentless scanning. 

### Plan your migration according to your needs

Note that all Defender for Servers features are going to be provided via Defender for Endpoint integration and agentless disk scanning, without any dependency in either AMA or MMA.  Most of the features are already available in GA through the alternative platforms (MDE/Agentless). The rest will be provided in GA by April 2024, or deprecated. See here the features’ plan. 

Defender for Servers features that are currently provided over AMA on public preview are not going to be GA, and will remain supported until an alternative version is provided based on Defender for Endpoint integration or agentless disk scanning.

Following that, we recommend plan your agents’ migration plan according to your organization’s requirements:  

| AMA required (for Defender for SQL scenario or other scenarios [e.g: Sentinel])| FIM/EPP discovery/Baseline assessment is required as part of Defender for Server | What should I do? |
|-----|-----|-----|
| No | Yes | You can remove MMA starting April 2024, using GA version of Defender for Server capabilities according to your needs (preview versions will be available earlier) |
| No | Yes | You can remove MMA immediately. |
| Yes | No | You can start migration from MMA to AMA immediately. |
| Yes | Yes | You can either start migration from MMA to AMA starting April 2024 or alternatively, you can use both agents side by side starting now, to receive all GA capabilities. |

> [!NOTE]
> You can run both the Log Analytics and Azure Monitor Agents on the same machine. Each machine is billed once in Defender for Cloud. In cases both agents are running on the machines, we recommend avoid collecting duplicate data by sending the data to different workspaces or alternatively disable security event data collection by MMA. For further information please see the [migration guide](/azure/azure-monitor/agents/azure-monitor-agent-migration) and the [impact of running both agents](auto-deploy-azure-monitoring-agent.md#impact-of-running-with-both-the-log-analytics-and-azure-monitor-agents).

### Agents auto provision

Deploying Azure Monitor Agent (AMA) with Defender for Cloud portal is available for SQL servers on machines, with new deployment policy, see [here](defender-for-sql-autoprovisioning.md).

If you had the AMA-related public preview policy initiative enabled, support will continue until August 2024.  

To disable AMA provisioning, edit the configuration of “Log Analytics/Azure Monitor Agent” extension through “Setting and Monitoring” of Defender for Servers plan in Defender for Cloud portal. Alternately, you can manually remove the policy initiative. 

[insert screenshot]

For more information about how to plan for this change, [see Microsoft Defender for Cloud - strategy and plan towards Log Analytics Agent (MMA) deprecation](https://techcommunity.microsoft.com/t5/microsoft-defender-for-cloud/microsoft-defender-for-cloud-strategy-and-plan-towards-log/ba-p/3883341).

> [!NOTE]
>
If you have the Defender for SQL servers on machines plan enabled, you will need to review the [Defender for SQL Log Analytics deprecation plan](defender-for-sql-autoprovisioning.md) for log analytics agent/Azure monitor agent dependency as part of your migration planning.

## Prepare Defender for SQL Server on machines

Microsoft Monitoring Agent (MMA) is being deprecated in August 2024. As a result, a new SQL server-targeted Azure Monitoring Agent (AMA) autoprovisioning process was released. You can learn more about the [Defender for SQL Server on machines Log Analytics Agent's deprecation plan](upcoming-changes.md#defender-for-sql-server-on-machines). 

Customers who are using the current Log Analytics agent/Azure Monitor agent autoprovisioning process, should migrate to the new Azure Monitoring Agent for SQL server on machines autoprovisioning process. The migration process is seamless and provides continuous protection for all machines.

### Migrate to the SQL server-targeted AMA autoprovisioning process

1. Sign in to the [Azure portal](https://portal.azure.com/).
1. Search for and select **Microsoft Defender for Cloud**.
1. In the Defender for Cloud menu, select **Environment settings**.
1. Select the relevant subscription.
1. Under the Databases plan, select **Action required**.

[ insert screenshot]

1. In the pop-up window, select **Enable**.

[ insert screenshot]

1. Select **Save**.

Once the SQL server-targeted AMA autoprovisioning process has been enabled, you should disable the Log Analytics agent/Azure Monitor agent autoprovisioning process.

## Next steps
