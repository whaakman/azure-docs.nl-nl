---
title: Een virtuele machine maken met statische openbare IP-adres - Azure CLI 2.0 | Microsoft Docs
description: Informatie over het maken van een virtuele machine met een statische openbare IP-adres met de Azure-opdrachtregelinterface (CLI) 2.0.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 55bc21b0-2a45-4943-a5e7-8d785d0d015c
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a4c32694949880037f01bb2b6b9779d2cbb9809c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-cli-20"></a>Een virtuele machine maken met een statische openbare IP-adres met de Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI 2.0](virtual-network-deploy-static-pip-arm-cli.md)
> * [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md)
> * [Sjabloon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klassiek)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). In dit artikel wordt behandeld met het implementatiemodel van Resource Manager, die Microsoft voor de meeste nieuwe implementaties in plaats van het klassieke implementatiemodel aanbeveelt.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name = "create"></a>De virtuele machine maken

U kunt deze taak uitvoeren met de Azure CLI 2.0 (in dit artikel) of de [Azure CLI 1.0](virtual-network-deploy-static-pip-cli-nodejs.md). De waarden in ' ' voor de variabelen in de stappen volgen resources met de instellingen van het scenario maken. Wijzig de waarden, waar nodig, voor uw omgeving.

1. Installeer de [Azure CLI 2.0](/cli/azure/install-az-cli2) als u dit nog niet geïnstalleerd.
2. De openbare en persoonlijke sleutelpaar voor een SSH maken voor Linux virtuele machines via de stappen in de [maken van een SSH openbare en persoonlijke sleutelpaar voor virtuele Linux-machines](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Vanuit een opdrachtshell, meld u aan met de opdracht `az login`.
4. De virtuele machine maken door het uitvoeren van het script dat op een Linux- of Mac-computer volgt. De Azure openbaar IP-adres, virtuele netwerk, netwerkinterface en VM-resources moeten allemaal voorkomen op dezelfde locatie bevinden. Hoewel de bronnen, geen bestaan in dezelfde resourcegroep hebt, in het volgende script doen ze.

```bash
RgName="IaaSStory"
Location="westus"

# Create a resource group.

az group create \
--name $RgName \
--location $Location

# Create a public IP address resource with a static IP address using the --allocation-method Static option.
# If you do not specify this option, the address is allocated dynamically. The address is assigned to the
# resource from a pool of IP adresses unique to each Azure region. The DnsName must be unique within the
# Azure location it's created in. Download and view the file from https://www.microsoft.com/en-us/download/details.aspx?id=41653#
# that lists the ranges for each region.

PipName="PIPWEB1"
DnsName="iaasstoryws1"
az network public-ip create \
--name $PipName \
--resource-group $RgName \
--location $Location \
--allocation-method Static \
--dns-name $DnsName

# Create a virtual network with one subnet

VnetName="TestVNet"
VnetPrefix="192.168.0.0/16"
SubnetName="FrontEnd"
SubnetPrefix="192.168.1.0/24"
az network vnet create \
--name $VnetName \
--resource-group $RgName \
--location $Location \
--address-prefix $VnetPrefix \
--subnet-name $SubnetName \
--subnet-prefix $SubnetPrefix

# Create a network interface connected to the VNet with a static private IP address and associate the public IP address
# resource to the NIC.

NicName="NICWEB1"
PrivateIpAddress="192.168.1.101"
az network nic create \
--name $NicName \
--resource-group $RgName \
--location $Location \
--subnet $SubnetName \
--vnet-name $VnetName \
--private-ip-address $PrivateIpAddress \
--public-ip-address $PipName

# Create a new VM with the NIC

VmName="WEB1"

# Replace the value for the VmSize variable with a value from the
# https://docs.microsoft.com/azure/virtual-machines/virtual-machines-linux-sizes article.
VmSize="Standard_DS1"

# Replace the value for the OsImage variable with a value for *urn* from the output returned by entering
# the `az vm image list` command. 

OsImage="credativ:Debian:8:latest"
Username='adminuser'

# Replace the following value with the path to your public key file.
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
# If creating a Windows VM, remove the previous line and you'll be prompted for the password you want to configure for the VM.
```

Naast het maken van een virtuele machine, maakt het script:
- Een enkele premium schijf standaard beheerd, maar er andere opties voor het schijftype die u kunt maken. Lees de [maken van een Linux-VM met de Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel voor meer informatie.
- Virtueel netwerk, subnet, NIC en openbare IP-adresbronnen. U kunt ook gebruiken *bestaande* virtueel netwerk, subnet, NIC of openbare IP-adresbronnen. Voor meer informatie over het gebruik van bestaande netwerkbronnen in plaats van met maken van aanvullende resources, voer `az vm create -h`.

## <a name = "validate"></a>Maken van VM- en openbare IP-adres valideren

1. Voer de opdracht `az resource list --resouce-group IaaSStory --output table` voor een overzicht van de resources die zijn gemaakt door het script. Moet er vijf resources in de uitvoer van de geretourneerde: network interface, schijf, openbare IP-adres, virtuele netwerk en een virtuele machine.
2. Voer de opdracht `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`. Noteer de waarde van in de uitvoer van de geretourneerde **IpAddress** en dat de waarde van **PublicIpAllocationMethod** is *statische*.
3. Verwijder, voordat u de volgende opdracht uitvoert, de <>, vervangen door *gebruikersnaam* met de naam die u gebruikt voor de **gebruikersnaam** variabele in het script en vervang *ipAddress*met de **ipAddress** uit de vorige stap. Voer de volgende opdracht verbinding maken met de virtuele machine: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 

## <a name= "clean-up"></a>Verwijder de virtuele machine en de bijbehorende bronnen

Het raadzaam dat u de resources in deze oefening hebt gemaakt verwijdert als u deze niet in productie gebruikt. Virtuele machine, openbare IP-adres en schijfbronnen worden kosten in rekening, zolang deze zijn ingericht. Als u wilt verwijderen van de resources die zijn gemaakt tijdens deze oefening, moet u de volgende stappen uitvoeren:

1. Uitvoeren als u wilt weergeven van de resources in de resourcegroep, de `az resource list --resource-group IaaSStory` opdracht.
2. Bevestig er zijn geen resources in de resourcegroep, behalve de resources die zijn gemaakt door het script in dit artikel. 
3. Uitvoeren als u wilt verwijderen van alle resources in deze oefening hebt gemaakt, de `az group delete -n IaaSStory` opdracht. De opdracht verwijdert u de resourcegroep en alle resources die deze bevat.

## <a name="next-steps"></a>Volgende stappen

Netwerkverkeer kan stromen naar en van de virtuele machine gemaakt in dit artikel. Binnenkomende en uitgaande regels binnen een NSG die het verkeer dat naar en van de netwerkinterface, het subnet of beide stromen kan beperken, kunt u definiëren. Lees voor meer informatie over Nsg de [NSG overzicht](virtual-networks-nsg.md) artikel.