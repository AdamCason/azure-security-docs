---
title: Monitoring Key Vault data reference 
description: Important reference material needed when you monitor Key Vault 
author: msmbaldwin  
ms.topic: reference
ms.author: mbaldwin
ms.service: key-vault
ms.custom: subject-monitoring
ms.date: 07/07/2021
---

# Monitoring Key Vault data reference

See [Monitoring Key Vault](monitor-key-vault.md) for details on collecting and analyzing monitoring data for Key Vault.

## Metrics

<!-- REQUIRED if you support Metrics. If you don't, keep the section but call that out. Some services are only onboarded to logs.
<!-- Please keep headings in this order -->

<!-- 2 options here depending on the level of extra content you have. -->

------------**OPTION 1 EXAMPLE** ---------------------

<!-- OPTION 1 - Minimum -  Link to relevant bookmarks in https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported, which is auto generated from underlying systems.  Not all metrics are published depending on whether your product group wants them to be.  If the metric is published, but descriptions are wrong of missing, contact your PM and tell them to update them  in the Azure Monitor "shoebox" manifest.  If this article is missing metrics that you and the PM know are available, both of you contact azmondocs@microsoft.com.  
-->

<!-- Example format. There should be AT LEAST one Resource Provider/Resource Type here. -->

This section lists all the automatically collected platform metrics collected for Key Vault.  

|Metric Type | Resource Provider / Type Namespace<br/> and link to individual metrics |
|-------|-----|
| General | [Microsoft.KeyVault/vaults](../../azure-monitor/essentials/metrics-supported.md#microsoftkeyvaultvaults) |


### Key Vault metrics

Resource Provider and Type: [Microsoft.KeyVault/vaults](../../azure-monitor/essentials/metrics-supported.md#microsoftkeyvaultvaults)

| Metric | Unit | Description | *TODO replace this label with other information*  |
|:-------|:-----|:------------|:------------------|
|  Availability      | Percent    | Vault requests availability            | Use this metric for <!-- put your specific information in here -->  |
| SaturationShoebox | Percent | Average	Vault capacity used | XXX |
| ServiceApiHit | Count | Number of total service api hits | XXX |
| ServiceApiLatency | MilliSeconds | Overall latency of service api requests | XXX |
| ServiceApiResult | Count | Number of total service api results | XXX |

For more information, see a list of [all platform metrics supported in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-supported).

## Metric Dimensions

<!-- REQUIRED. Please  keep headings in this order -->
<!-- If you have metrics with dimensions, outline it here. If you have no dimensions, say so.  Questions email azmondocs@microsoft.com -->

For more information on what metric dimensions are, see [Multi-dimensional metrics](/azure/azure-monitor/platform/data-platform-metrics#multi-dimensional-metrics).

Key Vault has the following dimensions associated with its metrics.

, , , 

**--------------EXAMPLE format when you have dimensions------------------**

Azure Storage supports following dimensions for metrics in Azure Monitor.

| Dimension Name | Description |
| ------------------- | ----------------- |
| **ActivityType** | The type of blob for Blob metrics only. The supported values are **BlockBlob**, **PageBlob**, and **Azure Data Lake Storage**. Append blobs are included in **BlockBlob**. |
| **ActivityName** | Azure storage offers different access tiers, which allow you to store blob object data in the most cost-effective manner. See more in [Azure Storage blob tier](/azure/storage/blobs/storage-blob-storage-tiers). The supported values include: <br/> <li>**Hot**: Hot tier</li> <li>**Cool**: Cool tier</li> <li>**Archive**: Archive tier</li> <li>**Premium**: Premium tier for block blob</li> <li>**P4/P6/P10/P15/P20/P30/P40/P50/P60**: Tier types for premium page blob</li> <li>**Standard**: Tier type for standard page Blob</li> <li>**Untiered**: Tier type for general purpose v1 storage account</li> |
| **TransactionType** | Transaction from Primary or Secondary cluster. The available values include **Primary** and **Secondary**. It applies to Read Access Geo Redundant Storage(RA-GRS) when reading objects from secondary tenant. |
| **StatusCode** | Transaction from Primary or Secondary cluster. The available values include **Primary** and **Secondary**. It applies to Read Access Geo Redundant Storage(RA-GRS) when reading objects from secondary tenant. |
| **StatusCodeClass** | Transaction from Primary or Secondary cluster. The available values include **Primary** and **Secondary**. It applies to Read Access Geo Redundant Storage(RA-GRS) when reading objects from secondary tenant. |

## Resource logs

This section lists the types of resource logs you can collect for Key Vault.

For reference, see a list of [Microsoft.KeyVault/vaults](../../azure-monitor/essentials/resource-logs-categories.md#microsoftkeyvaultvaults).  For full details, see [Azure Key Vault logging](logging.md).

------------**OPTION 1 EXAMPLE** ---------------------

<!-- OPTION 1 - Minimum -  Link to relevant bookmarks in https://docs.microsoft.com/azure/azure-monitor/platform/resource-logs-categories, which is auto generated from the REST API.  Not all resource log types metrics are published depending on whether your product group wants them to be.  If the resource log is published, but category display names are wrong or missing, contact your PM and tell them to update them in the Azure Monitor "shoebox" manifest.  If this article is missing resource logs that you and the PM know are available, both of you contact azmondocs@microsoft.com.  
-->

<!-- Example format. There should be AT LEAST one Resource Provider/Resource Type here. -->

This section lists all the resource log category types collected for Key Vault.  

|Resource Log Type | Resource Provider / Type Namespace<br/> and link to individual metrics |
|-------|-----|
| Web Sites | [Microsoft.web/sites](/azure/azure-monitor/platform/resource-logs-categories#microsoftwebsites) |
| Web Site Slots | [Microsoft.web/sites/slots](/azure/azure-monitor/platform/resource-logs-categories#microsoftwebsitesslots) 

--------------**OPTION 2 EXAMPLE** -------------

<!--  OPTION 2 -  Link to the resource logs as above, but work in extra information not found in the automated metric-supported reference article.  NOTE: YOU WILL NOW HAVE TO MANUALLY MAINTAIN THIS SECTION to make sure it stays in sync with the resource-log-categories link. You can group these sections however you want provided you include the proper links back to resource-log-categories article. 
-->

<!-- Example format. Add extra information -->

### Web Sites

Resource Provider and Type: [Microsoft.web/sites](/azure/azure-monitor/platform/resource-logs-categories#microsoftwebsites)

| Category | Display Name | *TODO replace this label with other information*  |
|:---------|:-------------|------------------|
| AppServiceAppLogs   | App Service Application Logs | *TODO other important information about this type* |
| AppServiceAuditLogs | Access Audit Logs            | *TODO other important information about this type* |
|  etc.               |                              |                                                   |  

### Web Site Slots

Resource Provider and Type: [Microsoft.web/sites/slots](/azure/azure-monitor/platform/resource-logs-categories#microsoftwebsitesslots)

| Category | Display Name | *TODO replace this label with other information*  |
|:---------|:-------------|------------------|
| AppServiceAppLogs   | App Service Application Logs | *TODO other important information about this type* |
| AppServiceAuditLogs | Access Audit Logs            | *TODO other important information about this type* |
|  etc.               |                              |                                                   |  

--------------**END Examples** -------------

## Azure Monitor Logs tables
<!-- REQUIRED. Please keep heading in this order -->

This section refers to all of the Azure Monitor Logs Kusto tables relevant to Key Vault and available for query by Log Analytics. 

------------**OPTION 1 EXAMPLE** ---------------------

<!-- OPTION 1 - Minimum -  Link to relevant bookmarks in https://docs.microsoft.com/azure/azure-monitor/reference/tables/tables-resourcetype where your service tables are listed. These files are auto generated from the REST API.   If this article is missing tables that you and the PM know are available, both of you contact azmondocs@microsoft.com.  
-->

<!-- Example format. There should be AT LEAST one Resource Provider/Resource Type here. -->

|Resource Type | Notes |
|-------|-----|
| [Virtual Machines](/azure/azure-monitor/reference/tables/tables-resourcetype#virtual-machines) | |
| [Virtual machine scale sets](/azure/azure-monitor/reference/tables/tables-resourcetype#virtual-machine-scale-sets) | |

--------------**OPTION 2 EXAMPLE** -------------

<!--  OPTION 2 -  List out your tables adding additional information on what each table is for. Individually link to each table using the table name.  For example, link to [AzureMetrics](https://docs.microsoft.com/azure/azure-monitor/reference/tables/azuremetrics).  

NOTE: YOU WILL NOW HAVE TO MANUALLY MAINTAIN THIS SECTION to make sure it stays in sync with the automatically generated list. You can group these sections however you want provided you include the proper links back to the proper tables. 
-->

### Virtual Machines

| Table |  Description | *TODO replace this label with proper title for your additional information*  |
|:---------|:-------------|------------------|
| [AzureActivity](/azure/azure-monitor/reference/tables/azureactivity)   | <!-- description copied from previous link --> Entries from the Azure Activity log that provides insight into any subscription-level or management group level events that have occurred in Azure. | *TODO other important information about this type |
| [AzureMetrics](/azure/azure-monitor/reference/tables/azuremetrics) | <!-- description copied from previous link --> Metric data emitted by Azure services that measure their health and performance.    | *TODO other important information about this type |
|  etc.               |                              |                                                   |  

### Virtual Machine Scale Sets

| Table |  Description | *TODO replace this label with other information*  |
|:---------|:-------------|------------------|
| [ADAssessmentRecommendation](/azure/azure-monitor/reference/tables/adassessmentrecommendation)   | <!-- description copied from previous link --> Recommendations generated by AD assessments that are started through a scheduled task. When you schedule the assessment it runs by default every 7 days and upload the data into Azure Log Analytics | *TODO other important information about this type |
| [ADReplicationResult](/azure/azure-monitor/reference/tables/adreplicationresult) | <!-- description copied from previous link --> The AD Replication Status solution regularly monitors your Active Directory environment for any replication failures.    | *TODO other important information about this type |
|  etc.               |                              |                                                   |  

<!-- Add extra information if required -->

For a reference of all Azure Monitor Logs / Log Analytics tables, see the [Azure Monitor Log Table Reference](/azure/azure-monitor/reference/tables/tables-resourcetype).

--------------**END EXAMPLES** -------------

### Diagnostics tables
<!-- REQUIRED. Please keep heading in this order -->
<!-- If your service uses the AzureDiagnostics table in Azure Monitor Logs / Log Analytics, list what fields you use and what they are for. Azure Diagnostics is over 500 columns wide with all services using the fields that are consistent across Azure Monitor and then adding extra ones just for themselves.  If it uses service specific diagnostic table, refers to that table. If it uses both, put both types of information in. Most services in the future will have their own specific table. If you have questions, contact azmondocs@microsoft.com -->

Key Vault uses the [Azure Diagnostics](/azure/azure-monitor/reference/tables/azurediagnostics) table and the [TODO whatever additional] table to store resource log information. The following columns are relevant.

**Azure Diagnostics**

| Property | Description |
|:--- |:---|
|  |  |
|  |  |

**[TODO Service-specific table]**

| Property | Description |
|:--- |:---|
|  |  |
|  |  |

## Activity log
<!-- REQUIRED. Please keep heading in this order -->

The following table lists the operations related to Key Vault that may be created in the Activity log.

<!-- Fill in the table with the operations that can be created in the Activity log for the service. -->
| Operation | Description |
|:---|:---|
| | |
| | |

<!-- NOTE: This information may be hard to find or not listed anywhere.  Please ask your PM for at least an incomplete list of what type of messages could be written here. If you can't locate this, contact azmondocs@microsoft.com for help -->

For more information on the schema of Activity Log entries, see [Activity  Log schema](/azure/azure-monitor/essentials/activity-log-schema). 

## Schemas
<!-- REQUIRED. Please keep heading in this order -->

The following schemas are in use by Key Vault

<!-- List the schema and their usage. This can be for resource logs, alerts, event hub formats, etc depending on what you think is important. -->

## See Also

- See [Monitoring Azure Key Vault](monitor-key-vault.md) for a description of monitoring Azure Key Vault.
- See [Monitoring Azure resources with Azure Monitor](/azure/azure-monitor/insights/monitor-azure-resources) for details on monitoring Azure resources.