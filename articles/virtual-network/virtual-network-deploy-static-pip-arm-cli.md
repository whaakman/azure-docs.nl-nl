---
title: Een virtuele machine maken met een statisch openbaar IP-adres - Azure CLI | Microsoft Docs
description: Informatie over het maken van een virtuele machine met een statisch openbaar IP-adres met behulp van de Azure-opdrachtregelinterface (CLI).
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: ''
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
ms.openlocfilehash: bd44971162a79e53b731c5c89316f14e8bb0a1a6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38651956"
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-the-azure-cli"></a>Een virtuele machine maken met een statisch openbaar IP-adres met de Azure CLI

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure-CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [PowerShell (klassiek)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). In dit artikel bevat informatie over het Resource Manager-implementatiemodel, dat door Microsoft wordt aanbevolen voor de meeste nieuwe implementaties in plaats van het klassieke implementatiemodel.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name = "create"></a>De virtuele machine maken

De waarden in ' ' voor de variabelen in de volgende stappen maakt u resources met instellingen van het scenario. Wijzig de waarden waar nodig voor uw omgeving.

1. Installeer de [Azure CLI 2.0](/cli/azure/install-az-cli2) als u dit nog niet geïnstalleerd.
2. De openbare en persoonlijke sleutelpaar voor een SSH voor virtuele Linux-machines maken via de stappen in de [maken van de openbare en persoonlijke sleutelpaar voor een SSH voor virtuele Linux-machines](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
3. Uit een opdrachtshell, meld u aan met de opdracht `az login`.
4. De virtuele machine maken door het uitvoeren van het script dat op een Linux- of Mac-computer volgt. Het openbaar IP-adres van Azure, het virtuele netwerk, de netwerkinterface en de VM-resources moeten al bestaan op dezelfde locatie. Hoewel de resources niet allemaal hebben bestaan in dezelfde resourcegroep bevinden, in het volgende script doen ze.

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

Naast het maken van een virtuele machine, maakt u het script:
- Een enkele premium beheerde schijf standaard, maar er zijn andere opties voor het schijftype dat u kunt maken. Lees de [een Linux VM maken met de Azure CLI 2.0](../virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel voor meer informatie.
- Virtueel netwerk, subnet, NIC en resources voor openbaar IP-adres. U kunt ook gebruiken *bestaande* virtueel netwerk, subnet, NIC of openbare IP-adres-resources. Voor meer informatie over het gebruik van bestaande netwerkbronnen in plaats van het maken van aanvullende resources, voer `az vm create -h`.

## <a name = "validate"></a>Maken van VM- en openbare IP-adres valideren

1. Voer de opdracht `az resource list --resouce-group IaaSStory --output table` voor een overzicht van de resources die zijn gemaakt door het script. Er moet vijf resources in de resulterende uitvoer: network interface, schijf, openbare IP-adres, virtueel netwerk en een virtuele machine.
2. Voer de opdracht `az network public-ip show --name PIPWEB1 --resource-group IaaSStory --output table`. Noteer de waarde van de in de uitvoer van de geretourneerde **IpAddress** en dat de waarde van **PublicIpAllocationMethod** is *statische*.
3. Voordat u de volgende opdracht uitvoert, verwijdert u de <>, Vervang *gebruikersnaam* met de naam die u hebt gebruikt voor de **gebruikersnaam** variabele in het script en vervang *ipAddress*met de **ipAddress** uit de vorige stap. Voer de volgende opdracht om verbinding met de virtuele machine te maken: `ssh -i ~/.ssh/azure_id_rsa <Username>@<ipAddress>`. 

## <a name= "clean-up"></a>De virtuele machine en de bijbehorende resources verwijderen

Het raadzaam dat u de resources die in deze oefening worden gemaakt verwijdert als u deze in productie wordt niet gebruikt. Virtuele machine, openbare IP-adres en schijfbronnen kosten, zolang ze bent ingericht. Als u wilt verwijderen van de resources tijdens deze oefening hebt gemaakt, voert u de volgende stappen uit:

1. Uitvoeren als u wilt weergeven van de resources in de resourcegroep, de `az resource list --resource-group IaaSStory` opdracht.
2. Controleer of er zijn geen bronnen in de resourcegroep, dan de resources die door het script in dit artikel is gemaakt. 
3. Uitvoeren als u wilt verwijderen van alle resources die in deze oefening worden gemaakt, de `az group delete -n IaaSStory` opdracht. De opdracht verwijdert u de resourcegroep en alle resources die deze bevat.
 
## <a name="set-ip-addresses-within-the-operating-system"></a>IP-adressen binnen het besturingssysteem instellen

U moet het openbare IP-adres toegewezen aan een virtuele machine van Azure binnen het besturingssysteem van de virtuele machine nooit handmatig toewijzen. Het verdient aanbeveling dat u geen statisch het privé IP-adres toegewezen aan de virtuele machine van Azure binnen het besturingssysteem van een virtuele machine, toewijst tenzij die nodig zijn, bijvoorbeeld wanneer [meerdere IP-adressen toewijzen aan een Windows-VM](virtual-network-multiple-ip-addresses-cli.md). Als u het particuliere IP-adres binnen het besturingssysteem handmatig instellen wilt, controleert u of deze hetzelfde adres als de privé IP-adres toegewezen aan de Azure [netwerkinterface](virtual-network-network-interface-addresses.md#change-ip-address-settings), of het verlies van connectiviteit met de virtuele machine. Meer informatie over [privé IP-adres](virtual-network-network-interface-addresses.md#private) instellingen.

## <a name="next-steps"></a>Volgende stappen

Al het netwerkverkeer kan stromen naar en van de virtuele machine die in dit artikel hebt gemaakt. U kunt inkomende en uitgaande beveiligingsregels binnen een netwerkbeveiligingsgroep die het verkeer dat naar en van de netwerkinterface, het subnet of beide stromen kan beperken definiëren. Zie voor meer informatie over netwerkbeveiligingsgroepen, [overzicht van netwerkbeveiligingsgroepen](security-overview.md).