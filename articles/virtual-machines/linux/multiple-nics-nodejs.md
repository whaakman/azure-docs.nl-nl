---
title: Maak een Linux-VM in Azure met meerdere NIC's | Microsoft Docs
description: Informatie over het maken van een Linux-VM met meerdere NIC's gekoppeld met behulp van de Azure CLI of Resource Manager-sjablonen.
services: virtual-machines-linux
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: c87d4f07a01fa42ceb2582b974feb4e802b3e03f
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="create-a-linux-virtual-machine-with-multiple-nics-using-the-azure-cli-10"></a>Een virtuele Linux-machine maken met meerdere NIC's met behulp van de Azure CLI 1.0
U kunt een virtuele machine (VM) maken in Azure met meerdere virtuele netwerkinterfaces (NIC's) is gekoppeld. Een veelvoorkomend scenario is om verschillende subnetten voor front-end en back-end-verbinding of een netwerk dat is toegewezen aan een oplossing met bewaking of back-up. In dit artikel biedt een snelle opdrachten voor het maken van een virtuele machine met meerdere NIC's die zijn gekoppeld. Andere [VM-grootten](sizes.md) ondersteunen een verschillend aantal NIC's, dus het formaat van uw virtuele machine dienovereenkomstig.

> [!WARNING]
> Wanneer u een virtuele machine maken: u NIC's aan een bestaande virtuele machine met de Azure CLI 1.0 toevoegen kunt, moet u meerdere NIC's koppelen. U kunt [NIC's toevoegen aan een bestaande virtuele machine met de Azure CLI 2.0](multiple-nics.md). U kunt ook [een virtuele machine op basis van de oorspronkelijke virtuele schijven maken](copy-vm.md) en meerdere NIC's maken terwijl u de virtuele machine implementeert.


## <a name="cli-versions-to-complete-the-task"></a>CLI-versies om de taak uit te voeren
U kunt de taak uitvoeren met behulp van een van de volgende CLI-versies:

- [Azure CLI 1.0](#create-supporting-resources) – onze CLI voor het klassieke en resource management-implementatiemodel (in dit artikel)
- [Azure CLI 2.0](multiple-nics.md): onze CLI van de volgende generatie voor het Resource Manager-implementatiemodel


## <a name="create-supporting-resources"></a>Ondersteunende resources maken
Zorg ervoor dat u hebt de [Azure CLI](../../cli-install-nodejs.md) aangemeld en het gebruik van Resource Manager-modus:

```azurecli
azure config mode arm
```

In de volgende voorbeelden kunt u de parameternamen voorbeeld vervangen door uw eigen waarden. Voorbeeld parameternamen opgenomen *myResourceGroup*, *mystorageaccount*, en *myVM*.

Maak eerst een resourcegroep. In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli
azure group create myResourceGroup --location eastus
```

Een opslagaccount voor het opslaan van uw virtuele machines maken. Het volgende voorbeeld wordt een opslagaccount met de naam *mystorageaccount*:

```azurecli
azure storage account create mystorageaccount \
    --resource-group myResourceGroup \
    --location eastus \
    --kind Storage \
    --sku-name PLRS
```

Een virtueel netwerk als u wilt verbinding maken met uw virtuele machines te maken. Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVnet* met een adresvoorvoegsel van *192.168.0.0/16*:

```azurecli
azure network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefixes 192.168.0.0/16
```

Maken van twee virtuele netwerksubnetten: één voor verkeer van de front-end- en één voor verkeer van de back-end. Het volgende voorbeeld maakt u twee subnetten, met de naam *mySubnetFrontEnd* en *mySubnetBackEnd*:

```azurecli
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetFrontEnd \
    --address-prefix 192.168.1.0/24
azure network vnet subnet create \
    --resource-group myResourceGroup \
    --location myVnet \
    --name mySubnetBackEnd \
    --address-prefix 192.168.2.0/24
```

## <a name="create-and-configure-multiple-nics"></a>Maak en configureer meerdere NIC 's
U vindt meer informatie over [implementeren meerdere NIC's met de Azure CLI](../../virtual-machines/linux/multiple-nics.md), inclusief scripts van het proces van het doorlopen van de NIC's maken.

Het volgende voorbeeld maakt u twee NIC's met de naam *myNic1* en *myNic2*, met één NIC die verbinding maken met elk subnet:

```azurecli
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic1 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetFrontEnd
azure network nic create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNic2 \
    --subnet-vnet-name myVnet \
    --subnet-name mySubnetBackEnd
```

Maakt u gewoonlijk ook een [netwerkbeveiligingsgroep](../../virtual-network/security-overview.md) of [netwerktaakverdeler](../../load-balancer/load-balancer-overview.md) om te beheren en distribueren van verkeer tussen uw virtuele machines. Het volgende voorbeeld wordt een Netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup*:

```azurecli
azure network nsg create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNetworkSecurityGroup
```

Het binden van de NIC's voor het gebruik van de Netwerkbeveiligingsgroep `azure network nic set`. Het volgende voorbeeld wordt *myNic1* en *myNic2* met *myNetworkSecurityGroup*:

```azurecli
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic1 \
    --network-security-group-name myNetworkSecurityGroup
azure network nic set \
    --resource-group myResourceGroup \
    --name myNic2 \
    --network-security-group-name myNetworkSecurityGroup
```

## <a name="create-a-vm-and-attach-the-nics"></a>Een virtuele machine maken en koppelen van de NIC 's
Bij het maken van de virtuele machine is nu meerdere NIC's opgeven. In plaats daarvan met `--nic-name` voor één NIC, gebruik in plaats daarvan u `--nic-names` en een door komma's gescheiden lijst met NIC's bieden. U moet ook Wees voorzichtig wanneer u de VM-grootte. Er gelden beperkingen voor het totale aantal NIC's die u aan een virtuele machine toevoegen kunt. Lees meer over [Linux VM-grootten](sizes.md). Het volgende voorbeeld ziet u hoe u meerdere NIC's en vervolgens een VM-grootte ondersteunt het gebruik van meerdere NIC's opgeeft (*Standard_DS2_v2*):

```azurecli
azure vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --location eastus \
    --os-type linux \
    --nic-names myNic1,myNic2 \
    --vm-size Standard_DS2_v2 \
    --storage-account-name mystorageaccount \
    --image-urn UbuntuLTS \
    --admin-username azureuser \
    --ssh-publickey-file ~/.ssh/id_rsa.pub
```

Wanneer u meerdere NIC's aan een Linux-VM toevoegt, moet u routeringsregels maken. Met deze regels kunt de virtuele machine te verzenden en ontvangen van verkeer dat tot een specifieke NIC. behoort Worden anders wordt verkeer dat tot eth1 behoort, bijvoorbeeld, kan niet verwerkt correct door de gedefinieerde standaardroute. U kunt dit routering probleem, Zie [configureren gastbesturingssysteem voor meerdere NIC's](multiple-nics.md#configure-guest-os-for-multiple-nics).

## <a name="create-multiple-nics-using-resource-manager-templates"></a>Meerdere NIC's met behulp van Resource Manager-sjablonen maken
Azure Resource Manager-sjablonen gebruiken declaratieve JSON-bestanden voor het definiëren van uw omgeving. U vindt een [overzicht van Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Resource Manager-sjablonen kunnen u meerdere exemplaren van een bron tijdens implementatie, zoals het maken van meerdere NIC's maken. U gebruikt *kopie* om op te geven van het aantal exemplaren te maken:

```json
"copy": {
    "name": "multiplenics"
    "count": "[parameters('count')]"
}
```

Lees meer over [maken van meerdere exemplaren die gebruikmaken van *kopie*](../../resource-group-create-multiple.md). 

U kunt ook een `copyIndex()` toe te voegen vervolgens een nummer aan de naam van een resource, zodat u kunt maken `myNic1`, `myNic2`, enzovoort. Hier volgt een voorbeeld van de waarde voor de index:

```json
"name": "[concat('myNic', copyIndex())]", 
```

U kunt een compleet voorbeeld van lezen [meerdere NIC's met behulp van Resource Manager-sjablonen maken](../../virtual-network/template-samples.md).

Wanneer u meerdere NIC's aan een Linux-VM toevoegt, moet u routeringsregels maken. Met deze regels kunt de virtuele machine te verzenden en ontvangen van verkeer dat tot een specifieke NIC. behoort Worden anders wordt verkeer dat tot eth1 behoort, bijvoorbeeld, kan niet verwerkt correct door de gedefinieerde standaardroute. U kunt dit routering probleem, Zie [configureren gastbesturingssysteem voor meerdere NIC's](multiple-nics.md#configure-guest-os-for-multiple-nics).

## <a name="next-steps"></a>Volgende stappen
Zorg ervoor dat u Lees [Linux VM-grootten](sizes.md) bij het maken van een virtuele machine met meerdere NIC's. Let op het maximum aantal NIC's die ondersteuning biedt voor elke VM-grootte. 

Houd er rekening mee dat u extra netwerkinterfacekaarten niet toevoegen aan een bestaande virtuele machine, moet u de NIC's maken wanneer u de virtuele machine implementeert. Wees voorzichtig bij het plannen van uw implementaties om ervoor te zorgen dat u de vereiste netwerkverbinding vanaf het begin hebben.

