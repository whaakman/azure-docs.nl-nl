---
title: Virtuele netwerken te verbinden met het virtuele netwerk peering - Azure CLI | Microsoft Docs
description: In dit artikel leert u hoe u virtuele netwerken te verbinden met het virtuele netwerk peering, met behulp van de Azure CLI.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to connect two virtual networks so that virtual machines in one virtual network can communicate with virtual machines in the other virtual network.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 29ab957e97c6aa57be6192e6ee4d86fe642ae95d
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-cli"></a>Virtuele netwerken te verbinden met het virtuele netwerk peering met de Azure CLI

U kunt virtuele netwerken met elkaar verbinden met het virtuele netwerk peering. Als u virtuele netwerken brengen, zijn resources in beide virtuele netwerken met elkaar communiceren met de dezelfde latentie en bandbreedte, alsof de bronnen zich in hetzelfde virtuele netwerk. In dit artikel leert u hoe:

* Twee virtuele netwerken maken
* Twee virtuele netwerken te verbinden met een virtueel netwerk peering
* Een virtuele machine (VM) in elk virtueel netwerk implementeren
* Communicatie tussen virtuele machines

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in dit artikel is vereist dat u de Azure CLI versie 2.0.28 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="create-virtual-networks"></a>Virtuele netwerken maken

Voordat u een virtueel netwerk maakt, moet u maken van een resourcegroep voor het virtuele netwerk en andere resources die zijn gemaakt in dit artikel. Maak een resourcegroep maken met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak een virtueel netwerk met [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVirtualNetwork1* met het adresvoorvoegsel *10.0.0.0/16*.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.0.0.0/24
```

Maak een virtueel netwerk met de naam *myVirtualNetwork2* met het adresvoorvoegsel *10.1.0.0/16*:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --address-prefixes 10.1.0.0/16 \
  --subnet-name Subnet1 \
  --subnet-prefix 10.1.0.0/24
```

## <a name="peer-virtual-networks"></a>Peer virtuele netwerken

Peerings tot stand worden gebracht tussen virtuele netwerk-id's, dus moet u eerst de ID van elk virtueel netwerk met ophalen [az network vnet show](/cli/azure/network/vnet#az_network_vnet_show) en de ID wordt opgeslagen in een variabele.

```azurecli-interactive
# Get the id for myVirtualNetwork1.
vNet1Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork1 \
  --query id --out tsv)

# Get the id for myVirtualNetwork2.
vNet2Id=$(az network vnet show \
  --resource-group myResourceGroup \
  --name myVirtualNetwork2 \
  --query id \
  --out tsv)
```

Maken van een peering van *myVirtualNetwork1* naar *myVirtualNetwork2* met [az network vnet-peering maken](/cli/azure/network/vnet/peering#az_network_vnet_peering_create). Als de `--allow-vnet-access` parameter niet is opgegeven, een peering tot stand is gebracht, maar er is geen communicatie kan via deze stromen.

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --remote-vnet-id $vNet2Id \
  --allow-vnet-access
```

In de uitvoer geretourneerd nadat de vorige opdracht wordt uitgevoerd, ziet u dat de **peeringState** is *gestarte*. De peering blijft in de *gestarte* status totdat u de peering van *myVirtualNetwork2* naar *myVirtualNetwork1*. Maken van een peering van *myVirtualNetwork2* naar *myVirtualNetwork1*. 

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork2-myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork2 \
  --remote-vnet-id $vNet1Id \
  --allow-vnet-access
```

In de uitvoer geretourneerd nadat de vorige opdracht wordt uitgevoerd, ziet u dat de **peeringState** is *verbonden*. Azure ook gewijzigd de peering status van de *myVirtualNetwork1 myVirtualNetwork2* peering naar *verbonden*. Controleer of de status van de peering voor het *myVirtualNetwork1 myVirtualNetwork2* peering gewijzigd in *verbonden* met [az network vnet-peering weergeven](/cli/azure/network/vnet/peering#az_network_vnet_peering_show).

```azurecli-interactive
az network vnet peering show \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --query peeringState
```

Resources in een virtueel netwerk kan niet communiceren met resources in het virtuele netwerk tot de **peeringState** voor de peerings in beide virtuele netwerken is *verbonden*. 

## <a name="create-virtual-machines"></a>Virtuele machines maken

Een virtuele machine in elk virtueel netwerk maken, zodat u tussen deze twee een latere stap kan communiceren.

### <a name="create-the-first-vm"></a>De eerste virtuele machine maken

Maak een VM met [az vm create](/cli/azure/vm#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVm1* in de *myVirtualNetwork1* virtueel netwerk. Als SSH-sleutels niet al bestaan op de standaardlocatie van de sleutel, wordt deze de opdracht maken. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`. De `--no-wait` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork1 \
  --subnet Subnet1 \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>De tweede virtuele machine maken

Maak een VM in de *myVirtualNetwork2* virtueel netwerk.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork2 \
  --subnet Subnet1 \
  --generate-ssh-keys
```

De virtuele machine duurt een paar minuten maken. Nadat de virtuele machine is gemaakt, toont de Azure CLI informatie vergelijkbaar met het volgende voorbeeld: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm2",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.1.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Noteer de **publicIpAddress**. Dit adres wordt gebruikt voor toegang tot de virtuele machine vanaf het internet in een later stadium.

## <a name="communicate-between-vms"></a>Communicatie tussen virtuele machines

Gebruik de volgende opdracht voor het maken van een SSH-sessie met de *myVm2* VM. Vervang `<publicIpAddress>` met het openbare IP-adres van uw virtuele machine. In het vorige voorbeeld het openbare IP-adres is *13.90.242.231*.

```bash 
ssh <publicIpAddress>
```

De virtuele machine in pingen *myVirtualNetwork1*.

```bash 
ping 10.0.0.4 -c 4
```

U ontvangt vier antwoorden. 

Sluit de SSH-sessie op de *myVm2* VM. 

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, gebruik [az groep verwijderen](/cli/azure/group#az_group_delete) verwijderen van de resourcegroep en alle resources bevat.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u twee netwerken in dezelfde Azure-regio, verbinden met het virtuele netwerk peering. U kunt ook virtuele netwerken in verschillende peer [ondersteunde regio's](virtual-network-manage-peering.md#cross-region) en in [verschillende Azure-abonnementen](create-peering-different-subscriptions.md#cli), evenals maken [hub en spoke-netwerk ontwerpen](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) met peering. Zie voor meer informatie over het virtuele netwerk peering, [peering Virtual network-overzicht](virtual-network-peering-overview.md) en [beheren van virtueel netwerk peerings](virtual-network-manage-peering.md).

U kunt [uw eigen computer verbinden met een virtueel netwerk](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) via een VPN, en communiceren met resources in een virtueel netwerk of in virtuele netwerken peer is ingesteld. Zie voor herbruikbare scripts om uit te voeren veel van de taken besproken in het virtuele netwerk artikelen [voorbeelden script](cli-samples.md).