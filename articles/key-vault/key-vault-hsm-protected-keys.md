﻿---
title: How to generate and transfer HSM-protected keys for Azure Key Vault - Azure Key Vault | Microsoft Docs
description: Use this article to help you plan for, generate, and then transfer your own HSM-protected keys to use with Azure Key Vault. Also known as BYOK or bring your own key.
services: key-vault
author: amitbapat
manager: devtiw
tags: azure-resource-manager

ms.service: key-vault
ms.topic: conceptual
ms.date: 02/17/2020
ms.author: ambapat

---
# How to generate and transfer HSM-protected keys to Azure Key Vault (preview)

For added assurance, when you use Azure Key Vault, you can import or generate keys in hardware security modules (HSMs) that never leave the HSM boundary. This scenario is often referred to as *bring your own key*, or BYOK. The HSMs are FIPS 140-2 Level 2 validated. Azure Key Vault uses nCipher nShield family of HSMs to protect your keys.

Use the information in this topic to help you plan for, generate, and then transfer your own HSM-protected keys to use with Azure Key Vault.

> [!NOTE]
> This document describes a **new** key import process that supports importing keys from an HSM if the HSM vendor provides a BYOK tool for Key Vault. For importing HSM-keys from an nCipher nShield family of HSMs, [please follow these instructions](key-vault-hsm-protected-keys-legacy.md).

This functionality is not available for Azure China.

> [!NOTE]
> For more information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-overview.md)  
> For a getting started tutorial, which includes creating a key vault for HSM-protected keys, see [What is Azure Key Vault?](key-vault-overview.md).


## More information about nCipher HSMs and Microsoft services

nCipher Security, an Entrust Datacard company, is a leader in the general purpose HSM market, empowering world-leading organizations by delivering trust, integrity and control to their business critical information and applications. nCipher’s cryptographic solutions secure emerging technologies – cloud, IoT, blockchain, digital payments – and help meet new compliance mandates, using the same proven technology that global organizations depend on today to protect against threats to their sensitive data, network communications and enterprise infrastructure. nCipher delivers trust for business critical applications, ensuring the integrity of data and putting customers in complete control – today, tomorrow, at all times.

Microsoft has collaborated with nCipher Security to enhance the state of art for HSMs. These enhancements enable you to get the typical benefits of hosted services without relinquishing control over your keys. Specifically, these enhancements let Microsoft manage the HSMs so that you do not have to. As a cloud service, Azure Key Vault scales up at short notice to meet your organization’s usage spikes. At the same time, your key is protected inside Microsoft’s HSMs: You retain control over the key lifecycle because you generate the key and transfer it to Microsoft’s HSMs.

## Overview

* Generate a key (referred to as Key Exchange Key or KEK) in Key Vault. This must be an RSA-HSM key with 'import' as the only key operation.
* Download the public key of KEK as a .pem file
* Transfer KEK public key to your offline workstation connected to your on premise HSM.
* From your offline workstation, use the BYOK tool provided by your HSM vendor to create a .byok file. 
* The target key is encrypted with a Key Exchange Key (KEK), which stays encrypted until it is transferred to the Azure Key Vault HSMs. Only the encrypted version of your key leaves the on premise HSM.
* * The Key Exchange Key (KEK) that is used to encrypt your key is generated inside the Azure Key Vault HSMs and is not exportable. The HSMs enforce that there can be no clear version of the KEK outside the HSMs. 
* The KEK must be in the same key vault where the target key is to be imported.

## Prerequisites

See the following table for a list of prerequisites for bring your own key (BYOK) for Azure Key Vault.

| Requirement | More information |
| --- | --- |
| A subscription to Azure |To create an Azure Key Vault, you need an Azure subscription: [Sign up for free trial](https://azure.microsoft.com/pricing/free-trial/) |
| The Azure Key Vault Premium SKU to import HSM-protected keys |For more information about the service tiers and capabilities for Azure Key Vault, see the [Azure Key Vault Pricing](https://azure.microsoft.com/pricing/details/key-vault/) website. |
| An HSM from supported HSMs list and BYOK tool and instructions provided by your HSM vendor | You must have access to a Hardware Security Module and basic operational knowledge of your HSMs. See [Supported HSMs](#supported-hsm-vendors). |

## Supported HSMs

|HSM Vendor Name|Supported HSM models|Additional details|
|---|---|---|
|Thales|SafeNet Luna family of HSMs with firmware version 7.3 or newer| [Link to Gemalto BYOK tool and documentation](https://safenet.gemalto.com/blah-blah)|
|nCipher|nShield family of HSMs|[Use legacy BYOK procedure](key-vault-hsm-protected-keys-legacy.md)|


## Generate and transfer your key to Azure Key Vault HSM

You will use the following steps to generate and transfer your key to an Azure Key Vault HSM:

* [Step 1: Generate a KEK](#step-1-generate-a-kek)
* [Step 2: Download KEK public key](#step-2-download-kek-public-key)
* [Step 3: Generate and prepare your key for transfer](#step-3-generate-and-prepare-your-key-for-transfer)
* [Step 4: Transfer your key to Azure Key Vault](#step-4-transfer-your-key-to-azure-key-vault)

## Step 1: Generate a KEK

The KEK (Key Exchange Key) is an RSA key generated in Key Vault's HSM. 

KEK must be:
1. an **RSA-HSM** key (2048-bit or 3072-bit or 4096-bit)
2. generated in the same key vault where you intend to import the target key
3. created with allowed key operations set to **import**

Use the [az keyvault key create](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-create) command to create KEK with key operations set to import.

```azurecli
az keyvault key create --kty RSA-HSM --size 4096 --name KEKforBYOK --ops import --vault-name ContosoKeyVaultHSM
```

Note down the key identifier 'kid' returned from the above command. You'll need it in [Step 3](#step-3-generate-and-prepare-your-key-for-transfer).

## Step 2: Download KEK public key

Use the [az keyvault key download](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-download) to download the KEK public key into a .pem file. The target key you import is encrypted using the KEK public key.

```azurecli
az keyvault key download --name KEKforBYOK --vault-name ContosoKeyVaultHSM --file KEKforBYOK.publickey.pem
```

Transfer the .pem file to your offline workstation.

## Step 3: Generate and prepare your key for transfer

Please refer to your HSM vendor's documentation to download and install the BYOK tool. Follow instruction from your HSM vendor to generate a target key and then create a Key Transfer Package (a .byok file). The BYOK tool will use the key identifier from [Step 1](#step-1-generate-a-kek) and .pem file you downloaded in [Step 2](#step-2-download-kek-public-key) to generate an encrypted target key in a .byok file.

Transfer the .byok file to your connected workstation.

> [!NOTE] Target key must be an RSA key of size 2048-bit or 3072-bit or 4096-bit. Importing Elliptic Curve keys is not supported at this time.

## Step 4: Transfer your key to Azure Key Vault
For this final step, on the Internet-connected workstation, use the [az keyvault key import](/cli/azure/keyvault/key?view=azure-cli-latest#az-keyvault-key-import) command to upload the .byok file that you copied from the disconnected workstation to the Azure Key Vault HSM, to complete the key import.

```azurecli
az keyvault key import --vault-name ContosoKeyVaultHSM --name ContosoFirstHSMkey --byok-file KeyTransferPackage-ContosoFirstHSMkey.byok
```

If the upload is successful, you see displayed the properties of the key that you just imported.

## Next steps

You can now use this HSM-protected key in your key vault. For more information, see this price and feature [comparison](https://azure.microsoft.com/pricing/details/key-vault/).
