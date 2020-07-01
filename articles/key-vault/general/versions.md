---
title: Azure Key Vault versions 
description: The various versions of Azure Key Vault     
services: key-vault
author: msmbaldwin   
 
ms.service: key-vault      
ms.topic: conceptual      
ms.date: 06/30/2019     
ms.author: mbaldwin
 
---
 
# Key Vault versions

Here's what's new with Azure Key Vault. New features and improvements are also announced on the [Azure updates Key Vault Channel](https://azure.microsoft.com/en-us/updates/?category=security&query=Key%20vault)

## June 2020

Azure Monitor for Key Vault is now in preview.  Azure Monitor provides comprehensive monitoring of your key vaults by delivering a unified view of your Key Vault requests, performance, failures, and latency. For more information, see [Azure Monitor for Key Vault (preview).](../../azure-monitor/insights/key-vault-insights-overview.md).

## May 2020

Key Vault "bring your own key" (BYOK) is now generally available. See the [Azure Key Vault BYOK specification](../keys/byok-specification.md), and learn how to [Import HSM-protected keys to Key Vault (BYOK)](../keys/hsm-protected-keys-byok.md).

## March 2020

Private endpoints now available in preview. Azure Private Link Service enables you to access Azure Key Vault and Azure hosted customer/partner services over a Private Endpoint in your virtual network.  Learn how to [Integrate Key Vault with Azure Private Link](private-link-service.md).

## 2019

- Release of the next-generation Azure Key Vault SDKs. For examples of their use, see the Azure Key Vault secrest quickstarts for [Python](../secrets/quick-create-python.md), [.NET](../secrets/quick-create-net.md), [Java](../secrets/quick-create-java.md), and [Node.js](../secrets/quick-create-javascript.md)
- New Azure policies to manage key vault certificates. See the [Azure Policy built-in definitions for Key Vault](../policy-samples.md).
- Azure Key Vault Virtual Machine extension now generally available.  See [Key Vault virtual machine extension for Linux](../../virtual-machines/extensions/key-vault-linux.md) and [Key Vault virtual machine extension for Windows](../../virtual-machines/extensions/key-vault-windows.md).
- Event-driven secrets management for Azure Key Vault now available in Azure Event Grid. For more information, see [the Event Grid schema for events in Azure Key Vault](../../event-grid/event-schema-key-vault.md], and learn how to [Receive and respond to key vault notifications with Azure Event Grid](event-grid-tutorial.md).

## 2018

New features and integrations released this year:

- Integration with Azure Functions. For an example scenario leveraging [Azure Functions](../../functions/index.yml) for key vault operations, see [Automate the rotation of a secret](../secrets/tutorial-rotation.md). 
- [Integration with Azure Databricks](../../databricks/scenarios/store-secrets-azure-key-vault.md). With this, Azure Databricks now supports two types of secret scopes: Azure Key Vault-backed and Databricks-backed. For more information, see [Create an Azure Key Vault-backed secret scope](../secrets/secret-scopes.md#--create-an-azure-key-vault-backed-secret-scope)
- [Virtual network service endpoints for Azure Key Vault](overview-vnet-service-endpoints.md).

## 2016-10-01 - Managed Storage Account Keys
 
Summer 2017 - Storage Account Keys feature added easier integration with Azure Storage. See the overview topic for more information, [Managed Storage Account Keys overview](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-storage-keys).
 
## 2016-10-01 - Soft-delete
 
Summer 2017 - soft-delete feature added for improved data protection of your key vaults and key vault objects. See the overview topic for more information, [Soft-delete overview](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete).
 
## 2015-06-01 - Certificate management
 
Certificate management is added as a feature to the GA version 2015-06-01 on September 26, 2016.
 
## 2015-06-01 - General availability
 
General Availability version 2015-06-01, announced on June 24, 2015.
 
The following changes were made at this release: 
 
- Delete a key - “use” field removed.
- Get information about a key - “use” field removed.
- Import a key into a vault - “use” field removed.
- Restore a key - “use” field removed.     
- Changed “RSA_OAEP” to “RSA-OAEP” for RSA Algorithms. See [About keys, secrets, and certificates](about-keys-secrets-certificates.md).    
 
## 2015-02-01-preview 
 
Second preview version 2015-02-01-preview, announced on April 20, 2015. For more information, see [REST API Update](https://blogs.technet.com/b/kv/archive/2015/04/20/empty-3.aspx) blog post. 
 
The following tasks were updated:
 
- List the keys in a vault - added pagination support to operation.     
- List the versions of a key - added operation to list the versions of a key.  
- List secrets in a vault - added pagination support.   
- List versions of a secret - add operation to list the versions of a secret.  
- All operations - Added created/updated timestamps to attributes.  
- Create a secret - added Content-Type to secrets. 
- Create a key - added tags as optional information.      
- Create a secret - added tags as optional information.      
- Update a key - added tags as optional information.      
- Update a secret - added tags as optional information.      
- Changed max size for secrets from 10 K to 25 K Bytes. See, [About keys, secrets, and certificates](about-keys-secrets-certificates.md).    
 
## 2014-12-08-preview  
 
First preview version 2014-12-08-preview, announced on January 8, 2015.  
 
## Next steps

- [About keys, secrets, and certificates](about-keys-secrets-certificates.md)
- [Keys](../keys/index.yml)
- [Secrets](../secrets/index.yml)
- [Certificates](../certificates/index.yml)
