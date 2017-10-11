---
title: Een virtueel netwerk maken met de Azure CLI 1.0 | Microsoft Docs
description: Informatie over het maken van een virtueel netwerk met de Azure CLI 1.0 | Resource Manager.
services: virtual-network
documentationcenter: 
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/16/2017
ms.author: jdial
ms.openlocfilehash: f0649c5c8c04dda72d2f147601efb37217f9bade
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Een virtueel netwerk maken met de Azure CLI

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure heeft twee implementatiemodellen: Azure Resource Manager en klassiek. Microsoft raadt aan resources te maken via het Resource Manager-implementatiemodel. Lees het artikel [Azure-implementatiemodellen begrijpen](../azure-resource-manager/resource-manager-deployment-model.md) voor meer informatie over de verschillen tussen de twee modellen.

## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 2.0](virtual-networks-create-vnet-arm-cli.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel
- [Azure CLI 1.0](#create-a-virtual-network) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)

 
[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Voor het maken van een virtueel netwerk met de Azure CLI, moet u de volgende stappen uitvoeren:

1. Installeren en configureren van de Azure CLI met de stappen in de [installeren en configureren van de Azure CLI](../cli-install-nodejs.md) artikel.

2. Een VNet en een subnet maken:

    ```azurecli
    azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
    ```

    Verwachte uitvoer:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK

    Gebruikte parameters:

   * **--vnet**. Naam van de VNet die moet worden gemaakt. In ons scenario *TestVNet*
   * **-e (of--adresruimte)**. VNet-adresruimte. In ons scenario *192.168.0.0*
   * **-i (of de cidr-)**. Het netwerkmasker in CIDR-notatie. In ons scenario *16*.
   * **-n (of--subnet naam**). Naam van het eerste subnet. In ons scenario *FrontEnd*.
   * **-p (of--begin-ip-subnet)**. IP-adres voor het subnet of subnetadresruimte wordt gestart. In ons scenario *192.168.1.0*.
   * **-r (of--subnet cidr)**. Het netwerkmasker in CIDR-indeling voor het subnet. In ons scenario *24*.
   * **-l (of --locatie)**. Azure-regio waar de VNet wordt gemaakt. In ons scenario *VS-midden*.

3. Een subnet maken:

    ```azurecli
    azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
    ```
   
    Verwachte uitvoer:

            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK

    Gebruikte parameters:

   * **-t (of--vnet naam**. Naam van de VNet waar het subnet wordt gemaakt. In ons scenario *TestVNet*.
   * **-n (of --name)**. Naam van het nieuwe subnet. In ons scenario *back-end*.
   * **-a (of --adresvoorvoegsel)**. Subnet CIDR-blok. Vier in ons scenario *192.168.2.0/24*.
   
4. De eigenschappen van de nieuwe VNet weergeven:

    ```azurecli
    azure network vnet show
    ```
   
    Verwachte uitvoer:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

## <a name="next-steps"></a>Volgende stappen

Leer hoe u de volgende verbindingen maakt:

- Een virtuele machine (VM) met een virtueel netwerk door het lezen van de [maken van een Linux-VM](../virtual-machines/linux/quick-create-cli.md) artikel. In plaats van een VNet en subnet te maken via de stappen die in de artikelen worden beschreven, kunt u ook een bestaand VNet en subnet selecteren waarmee u een VM wilt verbinden.
- Verbinding van het virtuele netwerk met andere virtuele netwerken: lees het artikel [VNets verbinden](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
- Verbinding van het virtuele netwerk met een on-premises netwerk via een site-naar-site virtueel particulier netwerk (VPN) of een ExpressRoute-circuit. Meer informatie over hoe door het lezen van de [verbinding van een VNet met een on-premises netwerk via een site-naar-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) en [een VNet koppelen aan een ExpressRoute-circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).