---
title: Maak een virtueel netwerk - Azure CLI | Microsoft Docs
description: Informatie over het maken van een virtueel netwerk met de Azure CLI.
services: virtual-network
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 75966bcc-0056-4667-8482-6f08ca38e77a
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 659a791124eab002290ac0b7f0898cf1c06c2951
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Een virtueel netwerk maken met de Azure CLI

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure heeft twee implementatiemodellen: Azure Resource Manager en klassiek. Microsoft raadt aan resources te maken via het Resource Manager-implementatiemodel. Lees het artikel [Azure-implementatiemodellen begrijpen](../azure-resource-manager/resource-manager-deployment-model.md) voor meer informatie over de verschillen tussen de twee modellen.

U kunt ook een virtueel netwerk via Resource Manager, met andere hulpprogramma's maken of een virtueel netwerk via het klassieke implementatiemodel maken door een andere optie te selecteren in de volgende lijst:

> [!div class="op_single_selector"]
> * [Portal](virtual-networks-create-vnet-arm-pportal.md)
> * [PowerShell](virtual-networks-create-vnet-arm-ps.md)
> * [CLI](virtual-networks-create-vnet-arm-cli.md)
> * [Sjabloon](virtual-networks-create-vnet-arm-template-click.md)
> * [Portal (klassiek)](virtual-networks-create-vnet-classic-pportal.md)
> * [PowerShell (klassiek)](virtual-networks-create-vnet-classic-netcfg-ps.md)
> * [CLI (klassiek)](virtual-networks-create-vnet-classic-cli.md)

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Voor het maken van een virtueel netwerk met de Azure CLI, moet u de volgende stappen uitvoeren:

1. Installeren en configureren van de meest recente [Azure CLI 2.0](/cli/azure/install-az-cli2) en meld u aan op een Azure-account met [az aanmelding](/cli/azure/#login).

2. Maak een resourcegroep voor uw VNet met de [az groep maken](/cli/azure/group#create) opdracht met de `--name` en `--location` argumenten:

    ```azurecli
    az group create --name TestRG --location centralus
    ```

3. Een VNet en een subnet maken:

    ```azurecli
    az network vnet create \
    --name TestVNet \
    --resource-group TestRG \
    --location centralus \
    --address-prefix 192.168.0.0/16 \
    --subnet-name FrontEnd \
    --subnet-prefix 192.168.1.0/24
    ```

    Verwachte uitvoer:
    
    ```json
    {
        "newVNet": {
            "addressSpace": {
            "addressPrefixes": [
            "192.168.0.0/16"
            ]
            },
            "dhcpOptions": {
            "dnsServers": []
            },
            "provisioningState": "Succeeded",
            "resourceGuid": "<guid>",
            "subnets": [
            {
                "etag": "W/\"<guid>\"",
                "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                "name": "FrontEnd",
                "properties": {
                "addressPrefix": "192.168.1.0/24",
                "provisioningState": "Succeeded"
                },
                "resourceGroup": "TestRG"
            }
            ]
            }
    }
    ```

    Gebruikte parameters:

    - `--name TestVNet`: De naam van het VNet worden gemaakt.
    - `--resource-group TestRG`: # Naam van de resourcegroep die bepaalt van de resource. 
    - `--location centralus`: De locatie waarin te implementeren.
    - `--address-prefix 192.168.0.0/16`: De adresvoorvoegsel en het blok.  
    - `--subnet-name FrontEnd`: De naam van het subnet.
    - `--subnet-prefix 192.168.1.0/24`: De adresvoorvoegsel en het blok.

    Als u de basisinformatie moet worden gebruikt in de volgende opdracht, u kunt een query de VNet via een [queryfilter](/cli/azure/query-az-cli2):

    ```azurecli
    az network vnet list --query '[?name==`TestVNet`].{Where:location,Name:name,Group:resourceGroup}' -o table
    ```

    Dit wordt de volgende uitvoer:

        Where      Name      Group

        centralus  TestVNet  TestRG

4. Een subnet maken:

    ```azurecli
    az network vnet subnet create \
    --address-prefix 192.168.2.0/24 \
    --name BackEnd \
    --resource-group TestRG \
    --vnet-name TestVNet
    ```

    Verwachte uitvoer:

    ```json
    {
    "addressPrefix": "192.168.2.0/24",
    "etag": "W/\"<guid> \"",
    "id": "/subscriptions/<guid>/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
    "ipConfigurations": null,
    "name": "BackEnd",
    "networkSecurityGroup": null,
    "provisioningState": "Succeeded",
    "resourceGroup": "TestRG",
    "resourceNavigationLinks": null,
    "routeTable": null
    }
    ```

    Gebruikte parameters:

    - `--address-prefix 192.168.2.0/24`: Subnet CIDR-blok.
    - `--name BackEnd`: De naam van het nieuwe subnet.
    - `--resource-group TestRG`: De resourcegroep.
    - `--vnet-name TestVNet`: De naam van de eigenaar VNet.

5. Opvragen van de eigenschappen van de nieuwe VNet:

    ```azurecli
    az network vnet show \
    -g TestRG \
    -n TestVNet \
    --query '{Name:name,Where:location,Group:resourceGroup,Status:provisioningState,SubnetCount:subnets | length(@)}' \
    -o table
    ```

    Verwachte uitvoer:

        Name      Where      Group    Status       SubnetCount

        TestVNet  centralus  TestRG   Succeeded              2

6. Opvragen van de eigenschappen van de subnetten:

    ```azurecli
    az network vnet subnet list \
    -g TestRG \
    --vnet-name testvnet \
    --query '[].{Name:name,CIDR:addressPrefix,Status:provisioningState}' \
    -o table
    ```

    Verwachte uitvoer:

        Name      CIDR            Status

        FrontEnd  192.168.1.0/24  Succeeded
        BackEnd   192.168.2.0/24  Succeeded

## <a name="next-steps"></a>Volgende stappen

Leer hoe u de volgende verbindingen maakt:

- Een virtuele machine (VM) met een virtueel netwerk door het lezen van de [maken van een Linux-VM](../virtual-machines/linux/quick-create-cli.md) artikel. In plaats van een VNet en subnet te maken via de stappen die in de artikelen worden beschreven, kunt u ook een bestaand VNet en subnet selecteren waarmee u een VM wilt verbinden.
- Verbinding van het virtuele netwerk met andere virtuele netwerken: lees het artikel [VNets verbinden](../vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal.md).
- Verbinding van het virtuele netwerk met een on-premises netwerk via een site-naar-site virtueel particulier netwerk (VPN) of een ExpressRoute-circuit. Meer informatie over hoe door het lezen van de [verbinding van een VNet met een on-premises netwerk via een site-naar-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) en [een VNet koppelen aan een ExpressRoute-circuit](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md).