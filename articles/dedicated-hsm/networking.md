---
title: Dedicated HSM Networking  | Microsoft Docs
description: Azure Dedicated HSM provides key storage capabilities within Azure that meets FIPS 140-2 Level 3 certification
services: key-vault
author: barclayn
manager: mbaldwin

ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/22/2018
ms.author: barclayn

---

# Azure Dedicated HSM Networking

By its very nature, the Azure Dedicated HSM service requires highly secure networking whether it be from the Azure cloud back to the customer’s IT environment (on-premises), using distributed applications or for high availability scenarios. 
Azure Networking provides this and there are four distinct areas that must be addressed.

- Creating HSM devices inside your Virtual Network (VNET) in Azure
- Connecting on-premises to cloud-based resources for the configuration and management of HSM devices
- Creating and connecting virtual networks for inter-connecting application resources and HSM devices
- Connecting virtual networks across regions for inter-communication and also to enable high availability scenarios

## Virtual network for your Dedicated HSM’s

Dedicated HSM’s are integrated into a Virtual Network and therefore placed in the customers own private network in Azure. This enables private access to the devices from virtual machines or compute resources in the virtual network.  
For further information on integrating Azure services into the virtual network and the capabilities it provides, please refer to [Virtual network for Azure services](../virtual-network/virtual-network-for-azure-services.md) documentation.

### Virtual networks

Before provisioning a Dedicated HSM device, customers will first need to create a Virtual Network in Azure or use one that already exists in the customers subscription. The virtual network defines the security perimeter for the Dedicated HSM device. For further information on creating virtual networks please see [virtual network documentation](../virtual-network/virtual-networks-overview.md).

### Sub-networks

Dedicated HSMs are deployed into a subnet in the virtual network. Each Dedicated HSM device that is deployed in the customer’s subnet will receive a private IP address from this subnet. 
The subnet in which the HSM device is deployed first needs to be explicitly delegated to the service : Microsoft.HardwareSecurityModules/dedicatedHSMs. Delegation ensures that the subnet is dedicated for use by the service. Delegation to Dedicated HSMs imposes certain policy restrictions on the subnet. Network Security Groups (NSGs) and User Defined Routes (UDRs) are not currently supported on delegated subnets. These restrictions when lifted in the future will be appropriately documented. 

### ExpressRoute gateway

An ExpressRoute Gateway is required to establish connectivity between the physical HSM device and the customers virtual network in Azure. For further information on ExpressRoute Gateway please see the article [About virtual network gateways for ExpressRoute](../expressroute/expressroute-about-virtual-network-gateways.md).

## Connecting your on-premises IT to Azure

When creating cloud-based resources it is a very typical requirement for a private connection back to on-premises IT resources. In the case of Dedicated HSM, this will predominantly be for the HSM client software to configure the HSM devices and also for activities such as backups and pulling logs from HSM’s for analysis. 
A key decision point here is the nature of the connection as there are options.  The most flexible option is Site-to-Site VPN as there will likely be multiple on-premises resources that require secure communication with resources (including HSM’s) in the Azure cloud. This will require a customer organization to have a VPN device to facilitate the connection. A Point-to-Site VPN connection can be used if there is only a single end-point on-premises such as a single administration workstation. At this time, ExpressRoute is not an option for connection to on-premises resources. 
For further information on connectivity options please see [VPN Gateway planning options](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#planningtable).

### Point-to-Site VPN

A point-to-site Virtual Private Network is the simplest form of secure connection to a single endpoint on-premises. This may be relevant if you intend to only have a single administration workstation for Azure based dedicated HSM’s.

### Site-to-Site VPN

A site-to-site Virtual Private Network allows for secure communication between Azure based Dedicated HSM’s and your on-premises IT. An obvious scenario for this is having a backup facility for the HSM’s on-premise and needing a connection between the two for running the backup.

## Connecting virtual networks

A typical deployment architecture for Dedicated HSM will start with a single virtual network and corresponding subnet in which the HSM devices are created and provisioned. Within that same region there could well be additional virtual networks and subnets for application components that would make use of the Dedicated HSM. To enable communication across these networks we use Virtual Network Peering.

### Virtual network peering

When there are multiple virtual networks within a region that need to access each other’s resources, Virtual Network Peering can be used to create secure communication channels between them.  Virtual network peering provides not only secure communications but also ensures a low-latency and high-bandwidth connections between the resources in Azure.

## Connecting across Azure Regions

The HSM devices have the ability, via software libraries, to redirect traffic to an alternate HSM in the case of failure of a device or loss of access to that device. To address these regional level failure scenarios HSM’s can be deployed in an alternate region and connectivity enabled between virtual networks across these two regions.

### Cross region HA using VPN gateway

For globally distributed applications or for high availability regional failover scenarios, it is required to connect virtual networks across regions. In the case of the dedicated HSM service, this can be accomplished today by using a VPN Gateway that provides a secure tunnel between the two virtual networks. For more information on Vnet-to-Vnet connections using VPN Gateway, please refer [here](../vpn-gateway/vpn-gateway-about-vpngateways.md#V2V).

>[!NOTE]
Global Vnet peering is not available in cross-region connectivity scenarios with Dedicated HSMs at this time. 

![global-vnet](media/networking/global-vnet.png)