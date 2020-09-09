---
title: What to do in the event of an Azure service disruption that affects Managed HSM - Azure Key Vault | Microsoft Docs
description: Learn what to do in the event of an Azure service disruption that affects Managed HSM.
services: key-vault
author: amitbapat
manager: msmbaldwin

ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2020
ms.author: ambapat

---
# Managed HSM disaster recovery

You may wish to create an exact replica of your HSM if the original is lost or unavailable due to any of the below reasons:

- It was deleted and then purged
- A catastrophic failure in the region resulted in all member partitions being destroyed

You can re-create the HSM instance in same or different region if you have following:
- The [Security Domain](security-domain.md)
- The private keys (at least quorum number) that encrypt the security domain
- The most recent full HSM [backup](backup-restore.md)

Here are the steps of the disaster recovery procedure:
1. Create a new HSM Instance.
1. When the HSM is in "Provisioned" state, activate "Security Domain restore mode". A new RSA key pair (Security Domain Exchange Key) will be generated for Security Domain transfer.
1. Download a SecurityDomainExchangeKey (public key). This key can only be retrieved when "security domain restore mode" is activated.
1. Create a "Security Domain Transfer File". You will need the private keys that encrypt the security domain. The private keys are used locally, and never transferred anywhere in this process.
1. Upload the security domain to activate the HSM.
1. Take a backup of the new HSM. A backup is required before any restore, even when the HSM is empty. This allows easy roll-back. 
1. Restore the recent HSM backup from the source HSM

The contents of your key vault are replicated within the region and to a secondary region at least 150 miles away but within the same geography. This maintains high durability of your keys and secrets. See the [Azure paired regions](../../best-practices-availability-paired-regions.md) document for details on specific region pairs.

### Create a new HSM

Use the `az keyvault create` command to create a Managed HSM. This script has three mandatory parameters: a resource group name, an hsm name, and the geographic location.

You need to provide following inputs to create a Managed HSM resource:
- The name for the HSM
- The resource group where it will be placed in your subscription
- The Azure location
- A list of initial administrators
 
The example below creates an HSM named **ContosoMHSM**, in the resource group  **ContosoResourceGroup**, residing in the **East US 2** location, with **the current signed in user** as the only administrator.

# [Azure CLI](#tab/azure-cli)
```azurecli
oid=$(az ad signed-in-user show --query objectId -o tsv)
az keyvault create --hsm-name "ContosoMHSM" --resource-group "ContosoResourceGroup" --location "East US 2" --administrators $oid
```
---

> [!NOTE] Create command can take a few minutes. Once it returns successfully you are ready to activate your HSM.

The output of this command shows properties of the Managed HSM that you've created. The two most important properties are:

* **name**: In the example, the name is ContosoMHSM. You'll use this name for other Key Vault commands.
* **hsmUri**: In the example, the URI is https://contosohsm.managedhsm.azure.net. Applications that use your hsm through its REST API must use this URI.

Your Azure account is now authorized to perform any operations on this Managed HSM. As of yet, nobody else is authorized.


### Activate the security domain recovery mode
In the normal create process we initialize and download a new Security Domain at this point. However, since we are executing a disaster recovery procedure, we request the HSM to enter Security Domain Recovery Mode and download a Security Domain Exchange Key. This is an RSA public key that will be used to encrypt the security domain before uploading it to the HSM. The corresponding private key is protected inside the HSM, to keep your Security Domain contents safe during the transfer.

# [Azure CLI](#tab/azure-cli)

```azurecli
az keyvault security-domain init-recovery --hsm-name ContosoMHSM2 --sd-exchange-key ContosoMHSM2-SDE.cer
```
---

### Upload source HSM's security domain

For this step you will need following:
- The Security Domain Exchange Key you downloaded in previous step.
- The Security Domain of the source HSM.
- At least quorum number of private keys that were used to encrypt the security domain.

The `az keyvault security-domain upload` command performs following operations:

- Decrypt the source HSM's Security Domain with the private keys you supply. 
- create a Security Domain Upload blob encrypted with the Security Domain Exchange Key we downloaded in the previous step and then
- Upload the Security Domain Upload blob to the HSM to complete security domain recovery

In the example below we use the Security Domain from the **ContosoMHSM**, the 2 of the corresponding private keys, and upload it to **ContosoMHSM2** which is waiting to receive a Security Domain. 

# [Azure CLI](#tab/azure-cli)

```azurecli
az keyvault security-domain upload --hsm-name ContosoMHSM2 --sd-exchange-key ContosoMHSM-SDE.cer --sd-file ContosoMHSM-SD.json --sd-wrapping-keys cert_0.key cert_1.key
```
---

Now both the source HSM (ContosoMHSM) and the destination HSM (ContosoMHSM2) have the same security domain. We can now resotre a full backup from the source HSM into the destination HSM.

### Create a backup (as a restore point) of your new HSM

It is always a good idea to take a full backup before you execute a full HSM restore, so that you have a restore point in case something goes wrong with the restore.

To create an HSM backup, you will need following
- A storage account where the backup will be stored
- A blob storage container in this storage account where the backup process will create a new folder to store encrypted backup

In the example below, we use `az keyvault backup` command to the HSM backup in the storage container **mhsmbackupcontainer** a storage account **ContosoBackup**. We create a SAS token that expires in 30 minutes and provide that to Managed HSM to write the backup. 

# [Azure CLI](#tab/azure-cli)

```azurecli
end=$(date -u -d "30 minutes" '+%Y-%m-%dT%H:%MZ')
skey=$(az storage account keys list --query '[0].value' -o tsv --account-name ContosoBackup)
sas=$(az storage container generate-sas -n mhsmbackupcontainer --account-name ContosoBackup --permissions crdw --expiry $end --account-key $skey -o tsv)
az keyvault backup start --hsm-name ContosoMHSM2 --storage-account-name ContosoBackup --blob-container-name mhsmdemobackupcontainer --storage-container-SAS-token $sas

```

## Restore backup from source HSM

For this step you need following:
- The storage account and the blob container where the source HSM’s backups are stored
- The folder name from where you want to restore the backup. If you create regular backups, there will many folders inside this container.

# [Azure CLI](#tab/azure-cli)

```azurecli
end=$(date -u -d "30 minutes" '+%Y-%m-%dT%H:%MZ')
skey=$(az storage account keys list --query '[0].value' -o tsv --account-name ContosoBackup)
sas=$(az storage container generate-sas -n mhsmdemobackupcontainer --account-name ContosoBackup --permissions rl --expiry $end --account-key $skey -o tsv)
az keyvault restore start --hsm-name ContosoMHSM2 --storage-account-name ContosoBackup --blob-container-name mhsmdemobackupcontainer --storage-container-SAS-token $sas --backup-folder mhsm-ContosoMHSM-2020083120161860
```
---

Now you have completed a full disaster recovery process. The conents of the source HSM when the backup was taken are copied to the destination HSM, including all the keys, versions, attributes, tags, and role assignments.

## Next Steps
- Learn more about Security Domain see [About Managed HSM Security Domain](security-domain.md)
- Follow [Managed HSM best practices](best-practices.md)
