﻿---
title: Azure Quick start - Create a Key Vault PowerShell | Microsoft Docs
description: 
services: key-vault
author: barclayn
manager: mbaldwin
tags: azure-resource-manager

ms.assetid: 
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/06/2017
ms.author: barclayn

---
# Quickstart: Create an Azure Key Vault using PowerShell

Azure Key Vaults may be created and managed through PowerShell. In this quickstart you will use PowerShell to create a Key Vault. You will then store a secret in the newly created vault.

If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

If you choose to install and use PowerShell locally, this tutorial requires Azure PowerShell module version 5.1.1 or later. Run `Get-Module -ListAvailable AzureRM` to find the version. If you need to upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps). If you are running PowerShell locally, you also need to run `Login-AzureRmAccount` to create a connection with Azure.

```azurepowershell-interactive
Login-AzureRmAccount
```

If you are using a specific instance of Azure use the -Environment parameter. For example:

 ```azure-powershell-interactive
 Login-AzureRmAccount –Environment (Get-AzureRmEnvironment –Name AzureUSGovernment)
 ```

## Create a resource group

Create an Azure resource group with [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). A resource group is a logical container into which Azure resources are deployed and managed. 

```azurepowershell-interactive
New-AzureRmResourceGroup -Name ContosoResourceGroup -Location EastUS
```

## Create a Key Vault

Next you will create the Key Vault in the resource group created in the previous step. When performing this step you will need to provide some information:

>[!NOTE]
Although we use “ContosoKeyVault” as the name for our Key Vault throughout this quickstart, you must use a unique name. We suggest “KeyVault” + your initials (e.g., “KeyVaultMSB”).

- **Vault name** ContosoKeyVault.
- **Resource group name** ContosoResourceGroup.
- **Location** East US.

```azurepowershell-interactive
New-AzureRmKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East US'
```

The output of this cmdlet shows properties of the newly created Key Vault. Take note of the two properties listed below:

* **Vault Name**: In the example, this is **ContosoKeyVault**. You will use this name for other Key Vault cmdlets.
* **Vault URI**: In the example, this is https://contosokeyvault.vault.azure.net/. Applications that use your vault through its REST API must use this URI.

Your Azure account is the only account authorized to perform any operations on this new Key Vault.

![Output after Key Vault creation command completes](./media/quick-create-powershell/output-after-creating-keyvault.png)

## Adding a secret to Key Vault

To add a secret to the vault you just need to take a couple of additional steps. In this case we will add a password that could be used by an application. The password will be called **SQLPassword** and we will store the value of **Pa$$w0rd** in it.

First convert the value of Pa$$w0rd to a secure string by typing:

```azurepowershell-interactive
$secretvalue = ConvertTo-SecureString 'Pa$$w0rd' -AsPlainText -Force
```

Then, type the PowerShell commands below to create a secret in Key Vault called **SQLPassword** with the value **Pa$$w0rd** :

```azurepowershell-interactive
$secret = Set-AzureKeyVaultSecret -VaultName 'ContosoKeyVault' -Name 'SQLPassword' -SecretValue $secretvalue
```

To view the value contained in the secret as plain text:

```azurepowershell-interactive
(get-azurekeyvaultsecret -vaultName "Contosokeyvault" -name "SQLPassword").SecretValueText
```

Now, you have created a Key Vault, stored a secret, and retrieved it.

## Clean up resources

When no longer needed, you can use the [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) command to remove the resource group, Key Vault, and all related resources.

>[!IMPORTANT]
> Other quickstarts and tutorials in this collection build upon this quickstart. If you plan to continue on to work with subsequent quickstarts and tutorials, you may wish to leave these resources in place.

```azurepowershell-interactive
Remove-AzureRmResourceGroup -Name ContosoResourceGroup
```

## Next steps

In this quickstart you have created a Key Vault and stored a software key in it. To learn more about Key Vault and how you can use it with your applications continue to the tutorial for web applications working with Key Vault.

> [!div class="nextstepaction"]
> [Use Azure Key Vault from a Web Application](key-vault-use-from-web-application.md)
