---
title: Upgrade from Qualys to Microsoft Defender Vulnerability Management
description: Learn how to migrate to the built-in Microsoft Defender Vulnerability Management solution in Microsoft Defender for Cloud
services: defender-for-cloud
ms.service: defender-for-cloud
ms.topic: how-to
ms.date: 11/30/2023
---

# Upgrade from Qualys to Microsoft Defender Vulnerability Management

Microsoft Defender for Cloud is unifying all vulnerability assessment solutions to utilize the Microsoft Defender Vulnerability Management (MDVM) vulnerability scanner. 

MDVM integrates across many cloud native use cases, such as containers build/runtime scenarios, agentless scanning for Virtual Machines (VM) and more. 

If you're currently using the [built-in vulnerability assessment solution powered by Qualys](deploy-vulnerability-assessment-vm.md), you should start planning for the upcoming deprecations by following the steps on this page.

There are three recommended methods to enable the built-in Microsoft Defender Vulnerability Management (MDVM) solution within Defender for Cloud:

## Azure policy (for Azure VMs) 

1. Sign in to the [Azure portal](https://portal.azure.com/).

1. Navigate to **Policy** > **Definitions**.

1. Search for `Setup subscriptions to transition to an alternative vulnerability assessment solution`. 

1. Select **Assign**.

1. Select a scope and enter an assignment name.

1. Select **Review + create**.

1. Review the information you entered and select **Create**.
 
This policy ensures that all Virtual Machines (VM) within a selected subscription are safeguarded with the built-in MDVM solution. 

## Defender for Cloud’s portal 

In the Defender for Cloud portal, you have the ability to change the vulnerability assessment solution to the built-in MDVM solution. 

1. Sign in to the [Azure portal](https://portal.azure.com/).

1. Navigate to **Microsoft Defender for Cloud** > **Environment settings** 

1. Select the relevant subscription.

1. Locate the Defender for Servers plan and select **Settings**.

    :::image type="content" source="media/how-to-migrate-to-built-in/settings-server.png" alt-text="Screenshot of the Defender for Cloud plan page that shows where to locate and select the settings button under the servers plan." lightbox="media/how-to-migrate-to-built-in/settings-server.png":::

1. Toggle `Vulnerability assessment for machines` to **On**.

    If `Vulnerability assessment for machines` was already set to on, select **Edit configuration**

    :::image type="content" source="media/how-to-migrate-to-built-in/edit-configuration.png" alt-text="Screenshot of the servers plan that shows where the edit configuration button is located." lightbox="media/how-to-migrate-to-built-in/edit-configuration.png":::

1. Select **Microsoft Defender Vulnerability Management**.

1. Select **Apply**. 

1. Ensure that `Endpoint protection` or `Agentless scanning for machines` are toggled to **On**.

    :::image type="content" source="media/how-to-migrate-to-built-in/two-to-one.png" alt-text="Screenshot that shows where to turn on endpoint protection and agentless scanning for machines is located." lightbox="media/how-to-migrate-to-built-in/two-to-one.png":::

1. Select **Continue**.

1. Select **Save**.

After migrating to the built-in MDVM solution in Defender for Cloud, offboard each VM from the current vulnerability assessment solution. There are three ways to offboard a VM:

- [Delete the VM extension](/powershell/module/az.compute/remove-azvmextension?view=azps-11.0.0).
- [REST API DELETE request](/rest/api/compute/virtual-machine-extensions/delete?view=rest-compute-2023-07-01&tabs=HTTP).
- Delete custom policy at scale.

## Migrate with REST API

### REST API for Azure VMs

Using this REST API, you can easily migrate your subscription, at scale, from any vulnerability assessment solution to Microsoft Defender Vulnerability Management.

`PUT https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.Security/serverVulnerabilityAssessmentsSettings/AzureServersSetting?api-version=2022-01-01-preview`

```json
{
   "kind": "AzureServersSetting",
   "properties": {
    "selectedProvider": "MdeTvm"
   }
 }
```

After migrating to the built-in MDVM solution in Defender for Cloud, offboard each VM from the current vulnerability assessment solution. There are three ways to offboard a VM:

- [Delete the VM extension](/powershell/module/az.compute/remove-azvmextension?view=azps-11.0.0).
- [REST API DELETE request](/rest/api/compute/virtual-machine-extensions/delete?view=rest-compute-2023-07-01&tabs=HTTP).
- Delete custom policy at scale.

### REST API for multicloud VMs

Using this REST API, you can easily migrate your subscription, at scale, from any vulnerability assessment solution to Microsoft Defender Vulnerability Management.

`PUT https://management.azure.com/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/Microsoft.Security/securityconnectors/{connectorName}?api-version=2022-08-01-preview`

```json
{
  "properties": {
   "hierarchyIdentifier": "{GcpProjectNumber}",
   "environmentName": "GCP",
   "offerings": [
​    {
​     "offeringType": "CspmMonitorGcp",
​     "nativeCloudConnection": {
​      "workloadIdentityProviderId": "{cspm}",
​      "serviceAccountEmailAddress": "{emailAddressRemainsAsIs}"
​     }
​    },
​    {
​     "offeringType": "DefenderCspmGcp"
​    },
​    {
​     "offeringType": "DefenderForServersGcp",
​     "defenderForServers": {
​      "workloadIdentityProviderId": "{defender-for-servers}",
​      "serviceAccountEmailAddress": "{emailAddressRemainsAsIs}"
​     },
​     "arcAutoProvisioning": {
​      "enabled": true,
​      "configuration": {}
​     },
​     "mdeAutoProvisioning": {
​      "enabled": true,
​      "configuration": {}
​     },
​     "vaAutoProvisioning": {
​      "enabled": true,
​      "configuration": {
​       "type": "TVM"
​      }
​     },
​     "subPlan": "{P1/P2}"
​    }
   ],
   "environmentData": {
​    "environmentType": "GcpProject",
​    "projectDetails": {
​     "projectId": "{GcpProjectId}",
​     "projectNumber": "{GcpProjectNumber}",
​     "workloadIdentityPoolId": "{identityPoolIdRemainsTheSame}"
​    }
   }
  },
  "location": "{connectorRegion}"
}
```

After migrating to the built-in MDVM solution in Defender for Cloud, offboard each VM from the current vulnerability assessment solution. There are three ways to offboard a VM:

- [Delete the VM extension](/powershell/module/az.compute/remove-azvmextension?view=azps-11.0.0).
- [REST API DELETE request](/rest/api/compute/virtual-machine-extensions/delete?view=rest-compute-2023-07-01&tabs=HTTP).
- Delete custom policy at scale.

## Next steps

[Common questions about vulnerability scanning questions](faq-scanner-detection.yml)
