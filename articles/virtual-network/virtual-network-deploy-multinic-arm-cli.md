---
title: Een virtuele machine maken met meerdere NIC's - Azure CLI 2.0 | Microsoft Docs
description: Informatie over het maken van een virtuele machine met meerdere NIC's met behulp van de Azure CLI 2.0.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 8e906a4b-8583-4a97-9416-ee34cfa09a98
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 19b1757dd694e756cfd2d0d6cd67e64f43ccab7f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-multiple-nics-using-the-azure-cli-20"></a>Een virtuele machine maken met meerdere NIC's met behulp van de Azure CLI 2.0

[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md).  Dit artikel bevat informatie over het Resource Manager-implementatiemodel, dat door Microsoft wordt aanbevolen voor de meeste nieuwe implementaties in plaats van het [klassieke implementatiemodel](virtual-network-deploy-multinic-classic-cli.md).
>

## <a name="create"></a>De virtuele machine maken

U kunt deze taak uitvoeren met de Azure CLI 2.0 (in dit artikel) of de [Azure CLI 1.0](virtual-network-deploy-multinic-cli-nodejs.md). De waarden in ' ' voor de variabelen in de stappen volgen resources met de instellingen van het scenario maken. Wijzig de waarden, waar nodig, voor uw omgeving.

1. Installeer de [Azure CLI 2.0](/cli/azure/install-az-cli2) als u dit nog niet geïnstalleerd.
2. De openbare en persoonlijke sleutelpaar voor een SSH maken voor Linux virtuele machines via de stappen in de [maken van een SSH openbare en persoonlijke sleutelpaar voor virtuele Linux-machines](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Vanuit een opdrachtshell, meld u aan met de opdracht `az login`.
4. De virtuele machine maken door het uitvoeren van het script dat op een Linux- of Mac-computer volgt. Het script maakt een resourcegroep, een virtueel netwerk (VNet) met twee subnetten, twee NIC's en met de twee NIC's een virtuele machine is gekoppeld. Een van de NIC's is verbonden met één subnet en een statische openbare en particuliere IP-adres is toegewezen. De andere NIC is verbonden met het andere subnet en een statisch privé IP-adres en geen openbare IP-adres is toegewezen. De NIC, openbare IP-adres, virtuele netwerken en VM netwerkbronnen moeten al bestaan in dezelfde locatie en abonnement. Hoewel de bronnen, geen bestaan in dezelfde resourcegroep hebt, in het volgende script doen ze.

```bash
#!/bin/sh

RgName="Multi-NIC-VM"
Location="westus"

# Create a resource group.
az group create \
--name $RgName \
--location $Location

# The address is assigned to the resource from a pool of IP adresses unique to each Azure region. 
# Download and view the file from https://www.microsoft.com/en-us/download/details.aspx?id=41653 that lists
# the ranges for each region.

PipName="PIP-WEB"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static

# Create a virtual network with one subnet

VnetName="VNet1"
VnetPrefix="10.0.0.0/16"
VnetSubnet1Name="Front-End"
VnetSubnet1Prefix="10.0.0.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $VnetSubnet1Name \
--subnet-prefix $VnetSubnet1Prefix

# Create a second subnet within the VNet

VnetSubnet2Name="Back-end"
VnetSubnet2Prefix="10.0.1.0/24"
az network vnet subnet create \
--vnet-name $VnetName \
--resource-group $RgName \
--name $VnetSubnet2Name \
--address-prefix $VnetSubnet2Prefix

# Create a network interface connected to one of the subnets. The NIC is assigned a single dynamic private and
# public IP address by default, but you can instead, assign static addresses, or no public IP address at all.
# You can also assign multiple private or public IP addresses to each NIC. To learn more about IP addressing
# options for NICs, enter the az network nic create -h command.

Nic1Name="NIC-FE"
PrivateIpAddress1="10.0.0.5"

az network nic create \
--name $Nic1Name \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet1Name \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress1 \
--public-ip-address $PipName

# Create a second network interface and connect it to the other subnet. Though multiple NICs attached to the same
# VM can be connected to different subnets, the subnets must all be within the same VNet. Add additional NICs as necessary.

Nic2Name="NIC-BE"
PrivateIpAddress2="10.0.1.5"

az network nic create \
--name $Nic2Name \
--resource-group $RgName \
--location $Location \
--subnet $VnetSubnet2Name \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress2

# Create a VM and attach the two NICs.

VmName="WEB"

# Replace the value for the following **VmSize** variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article. Not all VM sizes support
# more than one NIC, so be sure to select a VM size that supports the number of NICs you want to attach to the VM.
# You must create the VM with at least two NICs if you want to add more after VM creation. If you create a VM with
# only one NIC, you can't add additional NICs to the VM after VM creation, regardless of how many NICs the VM supports.
# The VM size specified in the following variable supports two NICs.

VmSize="Standard_DS2"

# Replace the value for the OsImage variable value with a value for *urn* from the output returned by entering the
# az vm image list command.

OsImage="credativ:Debian:8:latest"

Username="adminuser"

# Replace the following value with the path to your public key file.

SshKeyValue="~/.ssh/id_rsa.pub"

# Before executing the following command, add variable names of additional NICs you may have added to the script that
# you want to attach to the VM. If creating a Windows VM, remove the **ssh-key-value** line and you'll be prompted for
# the password you want to configure for the VM.

az vm create \
--name $VmName \
--resource-group $RgName \
--image $OsImage \
--location $Location \
--size $VmSize \
--nics $Nic1Name $Nic2Name \
--admin-username $Username \
--ssh-key-value $SshKeyValue
```

Naast het maken van een VM met twee NIC's, maakt het script:
- Een enkele premium schijf standaard beheerd, maar er andere opties voor het schijftype die u kunt maken. Lees de [maken van een Linux-VM met de Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel voor meer informatie.
- Een virtueel netwerk met twee subnetten en één openbare IP-adres. U kunt ook gebruiken *bestaande* virtueel netwerk, subnet, NIC of openbare IP-adresbronnen. Voor meer informatie over het gebruik van bestaande netwerkbronnen in plaats van met maken van aanvullende resources, voer `az vm create -h`.

## <a name = "validate"></a>Maken van VM's en NIC's valideren

1. Voer de opdracht `az resource list --resouce-group Multi-NIC-VM --output table` voor een overzicht van de resources die zijn gemaakt door het script. Moet er zes resources in de uitvoer van de geretourneerde: twee NIC's, één schijf, een openbare IP-adres, een virtueel netwerk en een virtuele machine.
2. Voer de opdracht `az network public-ip show --name PIP-WEB --resource-group Multi-NIC-VM --output table`. Noteer de waarde van in de uitvoer van de geretourneerde **IpAddress** en dat de waarde van **PublicIpAllocationMethod** is *statische*.
3. Voordat u de volgende opdracht uitvoert, verwijdert u de <>, vervangen door *gebruikersnaam* met de naam die u gebruikt voor de **gebruikersnaam** variabele in het script en vervang *ipAddress* met de **IP-adres** van de vorige stap. Voer de volgende opdracht verbinding maken met de virtuele machine: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 
4. Eenmaal zijn verbonden met de virtuele machine, voer de `sudo ifconfig` opdracht om te zien *eth0* en *eth1* interfaces. Elke NIC is het statische privé IP-adressen in het script is opgegeven door de Azure-DHCP-servers toegewezen. De IP- en MAC-adressen die zijn toegewezen aan de NIC's wijzigen niet totdat de virtuele machine wordt verwijderd. U wordt aangeraden dat u niet IP-adressen binnen een besturingssysteem, wijzigt als deze verbinding met de computer kunt uitschakelen. Openbare IP-adressen worden niet weergegeven in het besturingssysteem, omdat ze netwerkadres vertaald naar en van de privé IP-adres door de Azure-infrastructuur.

## <a name= "clean-up"></a>Verwijder de virtuele machine en de bijbehorende bronnen

Het raadzaam dat u de resources in deze oefening hebt gemaakt verwijdert als u deze niet in productie gebruikt. Virtuele machine, openbare IP-adres en schijfbronnen worden kosten in rekening, zolang deze zijn ingericht. Als u wilt verwijderen van de resources die zijn gemaakt tijdens deze oefening, moet u de volgende stappen uitvoeren:
1. Uitvoeren als u wilt weergeven van de resources in de resourcegroep, de `az resource list --resource-group Multi-NIC-VM` opdracht.
2. Bevestig er zijn geen resources in de resourcegroep, behalve de resources die zijn gemaakt door het script in dit artikel. 
3. Uitvoeren als u wilt verwijderen van alle resources in deze oefening hebt gemaakt, de `az group delete --name Multi-NIC-VM` opdracht. De opdracht verwijdert u de resourcegroep en alle resources die deze bevat.

## <a name="next-steps"></a>Volgende stappen

Netwerkverkeer kan stromen naar en van de virtuele machine gemaakt in dit artikel. Binnenkomende en uitgaande regels binnen een NSG die het verkeer van en naar elke netwerkinterface of elk subnet kan stromen te beperken, kunt u definiëren. Lees voor meer informatie over Nsg de [NSG overzicht](virtual-networks-nsg.md) artikel.