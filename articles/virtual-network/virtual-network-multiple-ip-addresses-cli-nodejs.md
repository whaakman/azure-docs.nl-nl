---
title: Virtuele machine met meerdere IP-adressen in de Azure CLI 1.0 | Microsoft Docs
description: Meer informatie over meerdere IP-adressen toewijzen aan een virtuele machine via de Azure CLI 1.0 | Resource Manager.
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: annahar
ms.openlocfilehash: 9f085dfa1fe4db36d58cb976bb550a46bf241ac7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-azure-cli-10"></a>Meerdere IP-adressen toewijzen aan virtuele machines met Azure CLI 1.0

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Dit artikel wordt uitgelegd hoe u een virtuele machine (VM) maken via het Azure Resource Manager-implementatiemodel met behulp van de Azure CLI 1.0. Meerdere IP-adressen kunnen niet worden toegewezen aan resources die zijn gemaakt met behulp van het klassieke implementatiemodel. Lees voor meer informatie over Azure-implementatiemodellen de [begrijpen implementatiemodellen](../resource-manager-deployment-model.md) artikel.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Een virtuele machine maken met meerdere IP-adressen

U kunt deze taak uitvoeren met de Azure CLI 1.0 (in dit artikel) of de [Azure CLI 2.0](virtual-network-multiple-ip-addresses-cli.md). Welke stappen volgen wordt uitgelegd hoe een voorbeeld van de virtuele machine maken met meerdere IP-adressen, zoals beschreven in het scenario. Namen van variabelen en typen van de IP-adres zoals vereist voor uw implementatie wijzigen.

1. Installeren en configureren van de Azure CLI 1.0 volgens de stappen in de [installeren en configureren van de Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel en meld u aan bij uw Azure-account met de `azure-login` opdracht.

2. Een resourcegroep maken:
    
    ```azurecli
    RgName=myResourceGroup
    Location=westcentralus
    azure group create --name $RgName --location $Location
    ```
3. Een virtueel netwerk maken:

    ```azurecli
    azure network vnet create --resource-group $RgName --location $Location --name myVNet \
    --address-prefixes 10.0.0.0/16
    ```
4. Maak een subnet in het virtuele netwerk:

    ```azurecli
    azure network vnet subnet create --name mySubnet --resource-group $RgName --vnet-name myVNet \
    --address-prefix 10.0.0.0/24
    ```
    
5. Een opslagaccount maken voor de virtuele machine. Voordat u de volgende opdracht uitvoert, vervangt *mystorageaccount* met een unieke naam. De naam moet uniek zijn in Azure:

    ```azurecli
    az storage account create --resource-group $RgName --location $Location --name mystorageaccount \
    --kind Storage --sku Standard_LRS
    ```

6. De IP-configuraties, een NIC maken en toewijzen van de IP-configuraties naar de NIC. U kunt toevoegen, verwijderen of wijzigen van de configuraties indien nodig. De volgende configuraties worden beschreven in het scenario:

    **IPConfig-1**

    Voer de opdrachten om te maken:

    - Een openbaar IP-adres-resource met een statische openbare IP-adres
    - Een NIC of het openbare IP-adres en een statisch privé IP-adres toewijzen aan deze.
    
    Vervang *mypublicdns* met een naam die uniek is binnen de Azure-locatie.

      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP1 \
      --domain-name-label mypublicdns --allocation-method Static
        
      azure network nic create --resource-group $RgName --location $Location --name myNic1 \
      --private-ip-address 10.0.0.4 --subnet-name mySubnet --subnet-vnet-name myVNet \
      --subnet-name mySubnet --public-ip-name myPublicIP1
      ```

      > [!NOTE]
      > Openbare IP-adressen hebben een nominaal kosten. Lees meer informatie over prijzen voor IP-adres, de [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses) pagina. Er is een limiet aan het aantal openbare IP-adressen die kunnen worden gebruikt in een abonnement. Lees voor meer informatie over de limieten het artikel [Azure-limieten](../azure-subscription-service-limits.md#networking-limits).

    **IPConfig 2**

     Voer de volgende opdrachten voor het maken van een nieuwe bron voor openbare IP-adres en een nieuwe IP-configuratie met een statische openbare IP-adres en een statisch privé IP-adres:
    
      ```azurecli
      azure network public-ip create --resource-group $RgName --location $Location --name myPublicIP2
      --domain-name-label mypublicdns2 --allocation-method Static

      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --name IPConfig-2
      --private-ip-address 10.0.0.5 --public-ip-name myPublicIP2
      ```

    **IPConfig 3**

    Voer de volgende opdrachten voor het maken van een IP-configuratie met een statisch privé IP-adres en geen openbare IP-adres:

      ```azurecli
      azure network nic ip-config create --resource-group $RgName --nic-name myNic1 --private-ip-address 10.0.0.6 \
      --name IPConfig-3
      ```

    >[!NOTE] 
    >Hoewel dit artikel worden alle IP-configuraties aan één NIC toewijst, kunt u ook meerdere IP-configuraties naar een NIC op een virtuele machine toewijzen. Lees voor meer informatie over het maken van een virtuele machine met meerdere NIC's, het maken een virtuele machine met meerdere NIC's artikel.

7. Een Linux-VM maken 

    ```azurecli
    az vm create --resource-group $RgName --name myVM1 --location $Location --nics myNic1 \
    --image UbuntuLTS --ssh-key-value ~/.ssh/id_rsa.pub --admin-username azureuser
    ```

    Als u de VM-grootte wilt standaard DS2 v2, bijvoorbeeld gewoon toevoegen de volgende eigenschap `--vm-size Standard_DS3_v2` naar de `azure vm create` opdracht in stap 6.

8. Voer de volgende opdracht de NIC en de bijbehorende IP-configuraties weergeven:

    ```azurecli
    azure network nic show --resource-group $RgName --name myNic1
    ```
9. De particuliere IP-adressen toevoegen aan het besturingssysteem van de virtuele machine via de stappen voor het besturingssysteem in de [toevoegen IP-adressen naar een VM-besturingssysteem](#os-config) sectie van dit artikel.

## <a name="add"></a>IP-adressen toevoegen aan een virtuele machine

U kunt extra persoonlijke en openbare IP-adressen toevoegen aan een bestaande NIC via de stappen volgen. De voorbeelden bouwen voort op de [scenario](#Scenario) in dit artikel wordt beschreven.

1. Open Azure CLI en voer de overige stappen in deze sectie binnen één CLI-sessie. Als u nog geen Azure CLI geïnstalleerd en geconfigureerd hebt, voer de stappen in de [installeren en configureren van de Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel en meld u aan bij uw Azure-account.

2. Volg de stappen in een van de volgende secties, op basis van uw vereisten:

    - **Een persoonlijke IP-adres toevoegen**
    
        Als u wilt een particulier IP-adres toevoegen aan een NIC, moet u een IP-configuratie met behulp van de onderstaande opdracht maken. Het statische adres moet een niet-gebruikte adres voor het subnet.

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic1 \
        --private-ip-address 10.0.0.7 --name IPConfig-4
        ```
        Configuraties die u nodig hebt, gebruik van unieke namen en privé IP-adressen (voor configuraties met statische IP-adressen) maken.

    - **Een openbaar IP-adres toevoegen**
    
        Een openbaar IP-adres is toegevoegd door deze te koppelen aan een nieuwe IP-configuratie of een bestaande IP-configuratie. Voer de stappen in een van de secties die volgen, die u nodig hebt.

        > [!NOTE]
        > Openbare IP-adressen hebben een nominaal kosten. Lees meer informatie over prijzen voor IP-adres, de [IP-adres prijzen](https://azure.microsoft.com/pricing/details/ip-addresses) pagina. Er is een limiet aan het aantal openbare IP-adressen die kunnen worden gebruikt in een abonnement. Lees voor meer informatie over de limieten het artikel [Azure-limieten](../azure-subscription-service-limits.md#networking-limits).
        >

        **Koppelen van de resource toe aan een nieuwe IP-configuratie**
    
        Als u een openbaar IP-adres in een nieuw IP-configuratie toevoegt, moet u ook een privé IP-adres toevoegen, omdat alle IP-configuraties moeten een particulier IP-adres hebben. U kunt een bestaande resource voor openbare IP-adres toevoegen of u een nieuwe maken. Voer de volgende opdracht om een nieuwe maken:

        ```azurecli
        azure network public-ip create --resource-group myResourceGroup --location westcentralus --name myPublicIP3 \
        --domain-name-label mypublicdns3
        ```

        Een nieuwe IP-configuratie maken met een statisch privé IP-adres en de bijbehorende *myPublicIP3* openbaar IP adres resource, voer de volgende opdracht:

        ```azurecli
        azure network nic ip-config create --resource-group myResourceGroup --nic-name myNic --name IPConfig-4 \
        --private-ip-address 10.0.0.8 --public-ip-name myPublicIP3
        ```

        **Koppelen van de resource toe aan een bestaande IP-configuratie**

        Een openbare IP-adres resource kan alleen worden gekoppeld aan een IP-configuratie die nog geen die zijn gekoppeld. U kunt bepalen of een IP-configuratie een gekoppeld openbare IP-adres heeft met de volgende opdracht:

        ```azurecli
        azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
        ```

        Zoek naar een regel vergelijkbaar met de link die erna voor IPConfig 3 in de resulterende uitvoer:

        ```         
        Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
        IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
        IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet
        ```
          
        Aangezien de **openbare IP-adres** kolom voor *IpConfig 3* is leeg, geen openbare IP-adres-resource is momenteel gekoppeld. U kunt een bestaande resource voor openbare IP-adres toevoegen aan IpConfig 3 of Voer de volgende opdracht een te maken:

        ```azurecli
        azure network public-ip create --resource-group  myResourceGroup --location westcentralus \
        --name myPublicIP3 --domain-name-label mypublicdns3 --allocation-method Static
        ```

        Voer de volgende opdracht om te koppelen van het openbare IP-adres resource aan de bestaande IP-configuratie met de naam *IPConfig 3*:
        ```azurecli
        azure network nic ip-config set --resource-group myResourceGroup --nic-name myNic1 --name IPConfig-3 \
        --public-ip-name myPublicIP3
        ```

3. Bekijk de privé IP-adressen en openbare IP-adres resources toegewezen aan de NIC met de volgende opdracht:

    ```azurecli
    azure network nic ip-config list --resource-group myResourceGroup --nic-name myNic1
    ```

      De resulterende uitvoer is vergelijkbaar met het volgende:
      ```
      Name               Provisioning state  Primary  Private IP allocation Private IP version  Private IP address  Subnet    Public IP
        
      default-ip-config  Succeeded           true     Static                IPv4                10.0.0.4            mySubnet  myPublicIP
      IPConfig-2         Succeeded           false    Static                IPv4                10.0.0.5            mySubnet  myPublicIP2
      IPConfig-3         Succeeded           false    Static                IPv4                10.0.0.6            mySubnet  myPublicIP3
      ```
4. Toevoegen van de privé IP-adressen die u hebt toegevoegd aan de NIC van het besturingssysteem van de virtuele machine door de instructies in de [toevoegen IP-adressen naar een VM-besturingssysteem](#os-config) sectie van dit artikel. Het openbare IP-adressen niet aan het besturingssysteem toevoegen.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
