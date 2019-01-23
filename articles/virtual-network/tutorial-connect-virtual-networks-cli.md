---
title: Virtuele netwerken verbinden met virtueel-netwerkpeering - Azure CLI | Microsoft Docs
description: In dit artikel leert u hoe u virtuele netwerken verbinden met virtueel-netwerkpeering met behulp van de Azure CLI.
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
ms.openlocfilehash: 5fc5829744d3740f3484303ae009145106264fec
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54470712"
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-cli"></a>Virtuele netwerken verbinden met virtueel-netwerkpeering met behulp van de Azure CLI

U kunt virtuele netwerken met elkaar verbinden met virtueel-netwerk peering. Wanneer virtuele netwerken als peers zijn gekoppeld, kunnen resources in beide virtuele netwerken met elkaar communiceren met dezelfde latentie en bandbreedte als wanneer de resources zich in hetzelfde virtuele netwerk zouden bevinden. In dit artikel leert u het volgende:

* Twee virtuele netwerken maken
* Twee virtuele netwerken koppelen met virtueel-netwerkpeering
* Een virtuele machine (VM) implementeren op elk van de virtuele netwerken
* Communiceren tussen VM's

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, in dit artikel moet u de Azure CLI versie 2.0.28 of hoger. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

## <a name="create-virtual-networks"></a>Virtuele netwerken maken

Voordat u een virtueel netwerk maakt, moet u maken van een resourcegroep voor het virtuele netwerk en alle andere resources in dit artikel hebt gemaakt. Maak een resourcegroep maken met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak een virtueel netwerk met [az network vnet create](/cli/azure/network/vnet). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVirtualNetwork1* met het adresvoorvoegsel *10.0.0.0/16*.

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

## <a name="peer-virtual-networks"></a>Peering van virtuele netwerken

Peerings zijn tot stand gebracht tussen virtuele netwerk-id's, zodat u moet eerst de ID van elk virtueel netwerk met [az network vnet show](/cli/azure/network/vnet#az_network_vnet_show) en sla de ID in een variabele.

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

Maken van een peering van *myVirtualNetwork1* naar *myVirtualNetwork2* met [az network vnet-peering maken](/cli/azure/network/vnet/peering). Als de `--allow-vnet-access` parameter niet wordt opgegeven, een peering tot stand is gebracht, maar er is geen communicatie door deze kan stromen.

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --remote-vnet-id $vNet2Id \
  --allow-vnet-access
```

In de uitvoer die wordt geretourneerd na de vorige opdracht is uitgevoerd, ziet u dat de **peeringState** is *gestart*. De peering blijft in de *gestart* status totdat u de peering vanuit *myVirtualNetwork2* naar *myVirtualNetwork1*. Maken van een peering van *myVirtualNetwork2* naar *myVirtualNetwork1*. 

```azurecli-interactive
az network vnet peering create \
  --name myVirtualNetwork2-myVirtualNetwork1 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork2 \
  --remote-vnet-id $vNet1Id \
  --allow-vnet-access
```

In de uitvoer die wordt geretourneerd na de vorige opdracht is uitgevoerd, ziet u dat de **peeringState** is *verbonden*. Azure heeft ook gewijzigd voor de status van de peering van het *myVirtualNetwork1 myVirtualNetwork2* peering aan *verbonden*. Controleer de status van de peering voor het *myVirtualNetwork1 myVirtualNetwork2* peering gewijzigd in *verbonden* met [az network vnet peering show](/cli/azure/network/vnet/peering#az_network_vnet_peering_show).

```azurecli-interactive
az network vnet peering show \
  --name myVirtualNetwork1-myVirtualNetwork2 \
  --resource-group myResourceGroup \
  --vnet-name myVirtualNetwork1 \
  --query peeringState
```

Resources in een virtueel netwerk kan niet communiceren met resources in het virtuele netwerk totdat de **peeringState** voor de peerings in beide virtuele netwerken is *verbonden*. 

## <a name="create-virtual-machines"></a>Virtuele machines maken

Maak een VM in elk virtueel netwerk, zodat u er in een latere stap tussen kunt communiceren.

### <a name="create-the-first-vm"></a>De eerste VM maken

Maak een VM met [az vm create](/cli/azure/vm#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVm1* in de *myVirtualNetwork1* virtueel netwerk. Als SSH-sleutels niet al bestaan op de standaardlocatie van de sleutel, worden ze met deze opdracht gemaakt. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`. De `--no-wait` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap.

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

### <a name="create-the-second-vm"></a>De tweede VM maken

Maken van een virtuele machine in de *myVirtualNetwork2* virtueel netwerk.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork2 \
  --subnet Subnet1 \
  --generate-ssh-keys
```

Het maken van de virtuele machine duurt een paar minuten. Nadat de virtuele machine is gemaakt, toont de Azure CLI informatie die vergelijkbaar is met het volgende voorbeeld: 

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

Let op het **openbare IP-adres**. Dit adres wordt gebruikt voor toegang tot de virtuele machine via internet in een latere stap.

## <a name="communicate-between-vms"></a>Communiceren tussen VM's

Gebruik de volgende opdracht om te maken van een SSH-sessie met de *myVm2* VM. Vervang `<publicIpAddress>` met het openbare IP-adres van uw virtuele machine. In het vorige voorbeeld, het openbare IP-adres is *13.90.242.231*.

```bash 
ssh <publicIpAddress>
```

De virtuele machine in de opdracht ping *myVirtualNetwork1*.

```bash 
ping 10.0.0.4 -c 4
```

U ontvangt vier reacties. 

Sluit de SSH-sessie op de *myVm2* VM. 

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, gebruikt u [az group delete](/cli/azure/group#az_group_delete) om de resourcegroep en alle resources die deze bevat te verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u twee netwerken in dezelfde Azure-regio, verbinden met virtueel-netwerkpeering. U kunt ook virtuele netwerken in verschillende [ondersteunde regio's](virtual-network-manage-peering.md#cross-region) en in [ verschillende Azure-abonnementen ](create-peering-different-subscriptions.md#cli) 'peeren', en peering gebruiken om [netwerkontwerpen met een stertopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) te maken. Zie voor meer informatie over virtueel-netwerkpeering [Peering op virtueel netwerk](virtual-network-peering-overview.md) en [Virtueel-netwerkpeerings beheren](virtual-network-manage-peering.md).

U kunt [uw eigen computer verbinden met een virtueel netwerk](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) via een VPN-verbinding, en communiceren met resources in een virtueel netwerk of in gekoppelde virtuele netwerken. Zie voor herbruikbare scripts om uit te voeren veel van de taken die worden besproken in het virtuele netwerk-artikelen, [script voorbeelden](cli-samples.md).
