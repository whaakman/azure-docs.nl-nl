---
title: Virtuele netwerken te verbinden met het virtuele netwerk peering - Azure CLI | Microsoft Docs
description: Ontdek hoe u virtuele netwerken te verbinden met het virtuele netwerk peering.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/06/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: df56f2e3e13f80e7ce2c2b6c9cffeac3d03776e5
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="connect-virtual-networks-with-virtual-network-peering-using-the-azure-cli"></a>Virtuele netwerken te verbinden met het virtuele netwerk peering met de Azure CLI

U kunt virtuele netwerken met elkaar verbinden met het virtuele netwerk peering. Als u virtuele netwerken brengen, zijn resources in beide virtuele netwerken met elkaar communiceren met de dezelfde latentie en bandbreedte, alsof de bronnen zich in hetzelfde virtuele netwerk. In dit artikel bevat informatie over het maken en peering van twee virtuele netwerken. Procedures voor:

> [!div class="checklist"]
> * Twee virtuele netwerken maken
> * Een peering tussen virtuele netwerken maken
> * Test-peering

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

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

Maak een virtueel netwerk met de naam *myVirtualNetwork2* met het adresvoorvoegsel *10.1.0.0/16*. Het adresvoorvoegsel niet overlapt met het adresvoorvoegsel van de *myVirtualNetwork1* virtueel netwerk. U kunt geen virtuele netwerken met overlappende adresvoorvoegsels peer.

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

Peerings tussen twee virtuele netwerken zijn, maar niet transitief zijn. Ja, bijvoorbeeld, als u wilt ook peer *myVirtualNetwork2* naar *myVirtualNetwork3*, moet u een extra peering tussen virtuele netwerken maken *myVirtualNetwork2* en *myVirtualNetwork3*. Hoewel *myVirtualNetwork1* is gekoppeld *myVirtualNetwork2*, resources binnen *myVirtualNetwork1* kan alleen toegang tot bronnen in  *myVirtualNetwork3* als *myVirtualNetwork1* is ook gekoppeld *myVirtualNetwork3*. 

Voordat de peering productie virtuele netwerken, het raadzaam dat u zorgvuldig vertrouwd raken met de [peering overzicht](virtual-network-peering-overview.md), [beheren peering](virtual-network-manage-peering.md), en [limieten voor virtueel netwerk ](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Hoewel dit artikel ziet u een peering tussen twee virtuele netwerken in hetzelfde abonnement en locatie, kunt u ook virtuele netwerken in peer [verschillende regio's](#register) en [verschillende Azure-abonnementen](create-peering-different-subscriptions.md#cli). U kunt ook maken [hub en spoke-netwerk ontwerpen](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) met peering.

## <a name="test-peering"></a>Test-peering

Een virtuele machine implementeren in elk subnet om te testen netwerkcommunicatie tussen virtuele machines in verschillende virtuele netwerken via een peering, en vervolgens communiceren tussen de virtuele machines. 

### <a name="create-virtual-machines"></a>Virtuele machines maken

Maak een virtuele machine met [az vm maken](/cli/azure/vm#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVm1* in de *myVirtualNetwork1* virtueel netwerk. Als SSH-sleutels niet al bestaan op de standaardlocatie van de sleutel, wordt deze de opdracht maken. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`. De `--no-wait` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap.

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

10.0.0.4 Azure automatisch toegewezen als de persoonlijke IP-adres van de virtuele machine, omdat het eerste beschikbare IP-adres in 10.0.0.4 *Subnet1* van *myVirtualNetwork1*. 

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

In het voorbeeld van uitvoer ziet u dat de **privateIpAddress** is *10.1.0.4*. 10.1.0.4 Azure DHCP automatisch toegewezen aan de virtuele machine omdat dit het eerste beschikbare adres in *Subnet1* van *myVirtualNetwork2*. Noteer de **publicIpAddress**. Dit adres wordt gebruikt voor toegang tot de virtuele machine vanaf het Internet in een later stadium.

### <a name="test-virtual-machine-communication"></a>Communicatie van de virtuele machine testen

Gebruik de volgende opdracht voor het maken van een SSH-sessie met de *myVm2* virtuele machine. Vervang `<publicIpAddress>` met het openbare IP-adres van uw virtuele machine. In het vorige voorbeeld het openbare IP-adres is *13.90.242.231*.

```bash 
ssh <publicIpAddress>
```

De virtuele machine in pingen *myVirtualNetwork1*.

```bash 
ping 10.0.0.4 -c 4
```

U ontvangt vier antwoorden. Als u op de naam van de virtuele machine pingen (*myVm1*), in plaats van het IP-adres, pingen mislukt, omdat *myVm1* is een onbekende host-naam. Azure standaard-naamomzetting werkt tussen virtuele machines in hetzelfde virtuele netwerk, maar niet tussen virtuele machines in verschillende virtuele netwerken. Voor het omzetten van namen in virtuele netwerken, moet u [uw eigen DNS-server implementeren](virtual-networks-name-resolution-for-vms-and-role-instances.md) of gebruik [persoonlijke Azure DNS-domeinen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Sluit de SSH-sessie op de *myVm2* virtuele machine. 

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, gebruik [az groep verwijderen](/cli/azure/group#az_group_delete) verwijderen van de resourcegroep en alle resources bevat.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

**<a name="register"></a>Registreren voor de peering preview globale virtueel netwerk**

Peering van virtuele netwerken in dezelfde regio's is algemeen beschikbaar. Virtuele netwerken in verschillende regio's is momenteel in preview-peering. Zie [virtuele netwerk updates](https://azure.microsoft.com/updates/?product=virtual-network) voor beschikbare regio's. Virtuele netwerken to-peer tussen regio's, moet u eerst registreren voor de preview, door de volgende stappen (binnen het abonnement elk virtueel netwerk dat u wilt peer wordt):

1. Registreren voor de preview hiertoe de volgende opdrachten:

  ```azurecli-interactive
  az feature register --name AllowGlobalVnetPeering --namespace Microsoft.Network
  az provider register --name Microsoft.Network
  ```

2. Bevestig dat u voor de preview zijn geregistreerd met de volgende opdracht:

  ```azurecli-interactive
  az feature show --name AllowGlobalVnetPeering --namespace Microsoft.Network
  ```

  Als u probeert te peer virtuele netwerken in verschillende regio's voordat de **RegistrationState** uitvoer wordt weergegeven nadat het invoeren van de vorige opdracht is **geregistreerde** voor beide abonnementen peering mislukt .

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u twee netwerken te verbinden met het virtuele netwerk peering. U kunt [uw eigen computer verbinden met een virtueel netwerk](../vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) via een VPN, en communiceren met resources in een virtueel netwerk of in virtuele netwerken peer is ingesteld.

Scriptvoorbeelden van het voor herbruikbare scripts om uit te voeren veel van de taken besproken in het virtuele netwerk artikelen blijven.

> [!div class="nextstepaction"]
> [Voorbeelden van virtueel netwerk script](../networking/cli-samples.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
