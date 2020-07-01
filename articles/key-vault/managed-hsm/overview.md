---
title: Azure Managed HSM Overview - Azure Managed HSM | Microsoft Docs
description: Azure Managed HSM is a cloud service that safeguards your cryptographic keys for cloud applications.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager

ms.service: key-vault
ms.topic: overview
ms.custom: mvc
ms.date: 01/07/2019
ms.author: mbaldwin
#Customer intent: As an IT Pro, Decision maker or developer I am trying to learn what Managed HSM is and if it offers anything that could be used in my organization.

---
# What is Azure Key Vault Managed HSM?

Azure Key Vault Managed HSM is a fully managed, highly available, single-tenant, standards-compliant cloud service that enables you to safeguards cryptographic keys for your cloud applications, using **FIPS  140-2 Level 3** validated HSMs.  

## Key Benefits


### Fully managed, highly available, single-tenant HSM as a service
 
- **Fully managed** - You don't need to provision, configure, patch and maintain HSMs or key management software. In case of hardware failure, member partitions for your HSM pool will be automatically migrated to healthy nodes

- **Highly available and zone resilient**   (where Availability zones are supported) - Each HSM pool consists of multiple HSM partitions span across at least 2 availability zones for zone resiliency

- **Single-tenant** - Each Managed HSM instance is dedicated to a single customer and consists of a pool of multiple HSM partitions. Each HSM pool uses a separate customer-specific security domain that cryptographically isolates each customer's HSM pool.

### Enhanced data protection and compliance

- **FIPS 140-2 Level 3** -Secure key management to protect your data and meet compliance requirements with FIPS ((Federal Information Protection Standard)) 140-2 Level 3 validated HSMs. Managed HSMS uses Marvell LiquidSecurity family of HSMs.
- **Monitor and audit** - fully integrated with Azure monitor. Get complete logs of all activity via Azure Monitor. Use Azure Log Analytics for analytics and alerts.


### Integrated with Azure and Microsoft PaaS/SaaS services 
- Generate (or import using [BYOK](hsm-protected-keys-byok.md)) keys and use them to [encrypt your data at rest in Azure services](../../security/fundamentals/encryption-atrest.md), [Service encryption with customer key for Office 365](/microsoft-365/compliance/customer-key-overview), [Azure Information Protection](https://docs.microsoft.com/azure/information-protection/what-is-information-protection) and other Microsoft cloud services.

### Uses same API and management interfaces as Key Vault
- Easily migrate your existing applications  from Key Vault (a multi-tenant) to Managed HSM
- Use same application development and deployment patterns for all your applications irrespective of which key management solution it uses

## 
