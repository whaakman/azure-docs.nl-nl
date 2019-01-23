---
title: Virtuele machine met meerdere IP-adressen met de Azure CLI
titlesuffix: Azure Virtual Network
description: Leer hoe u meerdere IP-adressen toewijzen aan een virtuele machine met behulp van de Azure-opdrachtregelinterface (CLI).
services: virtual-network
documentationcenter: na
author: jimdial
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/17/2016
ms.author: jimdial
ms.openlocfilehash: c51ac79ce1ee6835b8ce125c5f7a5f227e428668
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54449041"
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-the-azure-cli"></a>Meerdere IP-adressen toewijzen aan virtuele machines met de Azure CLI

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

In dit artikel wordt uitgelegd hoe u een virtuele machine (VM) maken via het Azure Resource Manager-implementatiemodel met behulp van de Azure CLI. Meerdere IP-adressen kunnen niet worden toegewezen aan resources die zijn gemaakt via het klassieke implementatiemodel. Lees voor meer informatie over Azure-implementatiemodellen, de [-implementatiemodellen begrijpen](../resource-manager-deployment-model.md) artikel.

[!INCLUDE [virtual-network-multiple-ip-addresses-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Een virtuele machine maken met meerdere IP-adressen

De volgende stappen wordt uitgelegd hoe u een voorbeeld van de virtuele machine maken met meerdere IP-adressen, zoals beschreven in het scenario. Variabelewaarden wijzigen in ' ' en de typen van het IP-adres, zoals vereist voor uw implementatie. 

1. Installeer de [Azure CLI](/cli/azure/install-azure-cli) als u dit nog niet geïnstalleerd.
2. De openbare en persoonlijke sleutelpaar voor een SSH voor virtuele Linux-machines maken via de stappen in de [maken van de openbare en persoonlijke sleutelpaar voor een SSH voor virtuele Linux-machines](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Uit een opdrachtshell, meld u aan met de opdracht `az login` en selecteer het abonnement dat u gebruikt.
4. De virtuele machine maken door het uitvoeren van het script dat op een Linux- of Mac-computer volgt. Het script maakt een resourcegroep, een virtueel netwerk (VNet), één NIC met drie IP-configuraties en een virtuele machine met de twee NIC's die zijn gekoppeld aan deze. De NIC, een openbaar IP-adres, een virtueel netwerk en een VM-resources moeten al bestaan in dezelfde locatie en abonnement. Hoewel de resources niet allemaal hebben bestaan in dezelfde resourcegroep bevinden, in het volgende script doen ze.

```bash
    
#!/bin/sh
    
RgName="myResourceGroup"
Location="westcentralus"
az group create --name $RgName --location $Location
    
# Create a public IP address resource with a static IP address using the `--allocation-method Static` option. If you
# do not specify this option, the address is allocated dynamically. The address is assigned to the resource from a pool
# of IP addresses unique to each Azure region. Download and view the file from
# https://www.microsoft.com/en-us/download/details.aspx?id=41653 that lists the ranges for each region.

PipName="myPublicIP"

# This name must be unique within an Azure location.
DnsName="myDNSName"

az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--dns-name $DnsName\
--allocation-method Static

# Create a virtual network with one subnet

VnetName="myVnet"
VnetPrefix="10.0.0.0/16"
VnetSubnetName="mySubnet"
VnetSubnetPrefix="10.0.0.0/24"

az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $VnetSubnetName \
--subnet-prefix $VnetSubnetPrefix

# Create a network interface connected to the subnet and associate the public IP address to it. Azure will create the
# first IP configuration with a static private IP address and will associate the public IP address resource to it.

NicName="MyNic1"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet1Name \
--private-ip-address 10.0.0.4
--vnet-name $VnetName \
--public-ip-address $PipName
    
# Create a second public IP address, a second IP configuration, and associate it to the NIC. This configuration has a
# static public IP address and a static private IP address.

az network public-ip create \
--resource-group $RgName \
--location $Location \
--name myPublicIP2 \
--dns-name mypublicdns2 \
--allocation-method Static

az network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--name IPConfig-2 \
--private-ip-address 10.0.0.5 \
--public-ip-name myPublicIP2

# Create a third IP configuration, and associate it to the NIC. This configuration has  static private IP address and   # no public IP address.

az network nic ip-config create \
--resource-group $RgName \
--nic-name $NicName \
--private-ip-address 10.0.0.6 \
--name IPConfig-3

# Note: Though this article assigns all IP configurations to a single NIC, you can also assign multiple IP configurations
# to any NIC in a VM. To learn how to create a VM with multiple NICs, read the Create a VM with multiple NICs 
# article: https://docs.microsoft.com/azure/virtual-network/virtual-network-deploy-multinic-arm-cli.

# Create a VM and attach the NIC.

VmName="myVm"

# Replace the value for the following **VmSize** variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes rticle. The script fails if the VM size
# is not supported in the location you select. Run the `azure vm sizes --location estcentralus` command to get a full list
# of VMs in US West Central, for example.

VmSize="Standard_DS1"

# Replace the value for the OsImage variable value with a value for *urn* from the utput returned by entering the
# `az vm image list` command.

OsImage="credativ:Debian:8:latest"

Username="adminuser"

# Replace the following value with the path to your public key file. If you're creating a Windows VM, remove the following
# line and you'll be prompted for the password you want to configure for the VM.

SshKeyValue="~/.ssh/id_rsa.pub"

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $NicName \
--admin-username $Username \
--ssh-key-value $SshKeyValue
```

Naast het maken van een virtuele machine met een NIC met 3 IP-configuraties, maakt u het script:

- Een enkele premium beheerde schijf standaard, maar er zijn andere opties voor het schijftype dat u kunt maken. Lees de [maken van een Linux-VM met de Azure CLI](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel voor meer informatie.
- Een virtueel netwerk met één subnet en twee openbare IP-adressen. U kunt ook gebruiken *bestaande* virtueel netwerk, subnet, NIC of openbare IP-adres-resources. Voor meer informatie over het gebruik van bestaande netwerkbronnen in plaats van het maken van aanvullende resources, voer `az vm create -h`.

Openbare IP-adressen hebben nominale kosten in rekening. Lees voor meer informatie over prijzen voor IP-adres, de [prijzen van IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses) pagina. Er is een limiet aan het aantal openbare IP-adressen die kunnen worden gebruikt in een abonnement. Lees voor meer informatie over de limieten het artikel [Azure-limieten](../azure-subscription-service-limits.md#networking-limits).

Nadat de virtuele machine is gemaakt, voert de `az network nic show --name MyNic1 --resource-group myResourceGroup` opdracht om de NIC-configuratie weer te geven. Voer de `az network nic ip-config list --nic-name MyNic1 --resource-group myResourceGroup --output table` om weer te geven van een lijst van de IP-configuraties die zijn gekoppeld aan de NIC.

De privé IP-adressen toevoegen aan het besturingssysteem van de virtuele machine via de stappen voor het besturingssysteem in de [toevoegen IP-adressen aan een VM-besturingssysteem](#os-config) sectie van dit artikel.

## <a name="add"></a>IP-adressen toevoegen aan een virtuele machine

U kunt extra persoonlijke en openbare IP-adressen toevoegen aan een bestaande Azure-netwerk-interface via de stappen die volgen. De voorbeelden zijn gebaseerd op de [scenario](#Scenario) in dit artikel beschreven.

1. Open een opdrachtshell en voltooi de resterende stappen in deze sectie in één sessie. Als u nog geen Azure-CLI is geïnstalleerd en geconfigureerd hebt, voer de stappen in de [Azure CLI-installatie](/cli/azure/install-az-cli2?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel en meld u aan bij uw Azure-account met de `az-login` opdracht.

2. Volg de stappen in een van de volgende secties, op basis van uw vereisten:

    **Een privé IP-adres toevoegen**
    
    Als u wilt een privé IP-adres toevoegen aan een NIC, moet u een IP-configuratie met de opdracht die volgt op maken. Het statische IP-adres moet een niet-gebruikte adres voor het subnet.

    ```bash
    az network nic ip-config create \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --private-ip-address 10.0.0.7 \
    --name IPConfig-4
    ```
    
    Maak zo veel configuraties die u nodig hebt, met behulp van unieke namen en privé-IP-adressen (voor configuraties met statische IP-adressen).

    **Een openbaar IP-adres toevoegen**
    
    Een openbaar IP-adres is toegevoegd door deze te koppelen aan een nieuwe IP-configuratie of een bestaande IP-configuratie. Voer de stappen in een van de secties die volgen, die u nodig hebt.

    Openbare IP-adressen hebben nominale kosten in rekening. Lees voor meer informatie over prijzen voor IP-adres, de [prijzen van IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses) pagina. Er is een limiet aan het aantal openbare IP-adressen die kunnen worden gebruikt in een abonnement. Lees voor meer informatie over de limieten het artikel [Azure-limieten](../azure-subscription-service-limits.md#networking-limits).

    - **Koppelen van de resource toe aan een nieuwe IP-configuratie**
    
        Wanneer u een openbaar IP-adres aan een nieuwe IP-configuratie toevoegt, moet u ook een privé IP-adres, toevoegen, omdat alle IP-configuraties moeten een privé IP-adres hebben. U kunt een bestaande openbare IP-adresresource toevoegen of u een nieuwe maken. Voor het maken van een nieuwe, voer de volgende opdracht:
    
        ```bash
        az network public-ip create \
        --resource-group myResourceGroup \
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3
        ```

        Een nieuwe IP-configuratie maken met een statisch privé IP-adres en de bijbehorende *myPublicIP3* openbaar IP-adres bron-adres, voer de volgende opdracht:

        ```bash
        az network nic ip-config create \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-5 \
        --private-ip-address 10.0.0.8
        --public-ip-address myPublicIP3
        ```

    - **Koppelen van de resource toe aan een bestaande IP-configuratie** een openbare IP-adresresource kan alleen worden gekoppeld aan een IP-configuratie die nog niet bevat die zijn gekoppeld. U kunt bepalen of een IP-configuratie een gekoppeld openbaar IP-adres heeft met de volgende opdracht:

        ```bash
        az network nic ip-config list \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --query "[?provisioningState=='Succeeded'].{ Name: name, PublicIpAddressId: publicIpAddress.id }" --output table
        ```

        Geretourneerd uitvoer:
    
            Name        PublicIpAddressId
            
            ipconfig1   /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
            IPConfig-2  /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
            IPConfig-3

        Omdat de **PublicIpAddressId** kolom voor *IpConfig 3* is leeg in de uitvoer, er zijn geen openbare IP-adresresource is momenteel gekoppeld. U kunt een bestaande openbare IP-adresresource toevoegen aan IP-configuratie 3 of Voer de volgende opdracht een te maken:

        ```bash
        az network public-ip create \
        --resource-group  myResourceGroup
        --location westcentralus \
        --name myPublicIP3 \
        --dns-name mypublicdns3 \
        --allocation-method Static
        ```
    
        Voer de volgende opdracht om te koppelen van de openbare IP-adresresource aan de bestaande IP-configuratie met de naam *IPConfig 3*:
    
        ```bash
        az network nic ip-config update \
        --resource-group myResourceGroup \
        --nic-name myNic1 \
        --name IPConfig-3 \
        --public-ip myPublicIP3
        ```

3. De privé IP-adressen en de openbare IP-adresresource id's die zijn toegewezen aan de NIC met de volgende opdracht weergeven:

    ```bash
    az network nic ip-config list \
    --resource-group myResourceGroup \
    --nic-name myNic1 \
    --query "[?provisioningState=='Succeeded'].{ Name: name, PrivateIpAddress: privateIpAddress, PrivateIpAllocationMethod: privateIpAllocationMethod, PublicIpAddressId: publicIpAddress.id }" --output table
    ```

    Geretourneerd uitvoer: <br>
    
        Name        PrivateIpAddress    PrivateIpAllocationMethod   PublicIpAddressId
        
        ipconfig1   10.0.0.4            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP1
        IPConfig-2  10.0.0.5            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP2
        IPConfig-3  10.0.0.6            Static                      /subscriptions/[Id]/resourceGroups/myResourceGroup/providers/Microsoft.Network/publicIPAddresses/myPublicIP3
    

4. Toevoegen van de privé IP-adressen hebt toegevoegd aan de NIC voor het besturingssysteem van de virtuele machine door de instructies in de [toevoegen IP-adressen aan een VM-besturingssysteem](#os-config) sectie van dit artikel. Voeg de openbare IP-adressen niet toe aan het besturingssysteem.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]
