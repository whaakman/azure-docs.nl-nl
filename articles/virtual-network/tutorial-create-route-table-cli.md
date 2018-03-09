---
title: Doorsturen van netwerkverkeer - Azure CLI | Microsoft Docs
description: Informatie over het netwerkverkeer omgeleid met een routetabel met de Azure CLI.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/02/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 3c16c774fa1c8a5c8bf50b4f4f9d0bfb283318e3
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-cli"></a>Doorsturen van netwerkverkeer met een routetabel met de Azure CLI

Azure automatisch routes verkeer tussen alle subnetten in een virtueel netwerk, standaard. U kunt uw eigen routes voor het overschrijven van Azure maken standaardroutering. De mogelijkheid voor het maken van aangepaste routes is handig als u bijvoorbeeld het routeren van verkeer tussen subnetten door een firewall. In dit artikel leert u hoe:

> [!div class="checklist"]
> * Een routetabel maken
> * Een route maken
> * Een routetabel aan het subnet van een virtueel netwerk koppelen
> * Test routering
> * Routering oplossen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, is deze snelstartgids vereist dat u de Azure CLI versie 2.0.28 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="create-a-route-table"></a>Een routetabel maken

Azure routes verkeer tussen alle subnetten in een virtueel netwerk, standaard. Zie voor meer informatie over Azure standaardroutes, [systeemroutes](virtual-networks-udr-overview.md). Als u wilt overschrijven Azure standaard routering, een routetabel met routes maken en koppelen van de routetabel een subnet van het virtuele netwerk.

Voordat u een routetabel maken kunt, maakt u een resourcegroep met [az groep maken](/cli/azure/group#az_group_create) voor alle resources in dit artikel is gemaakt. 

```azurecli-interactive
# Create a resource group.
az group create \
  --name myResourceGroup \
  --location eastus
``` 

Maken van een routetabel met [az netwerk routetabel maken](/cli/azure/network/route#az_network_route_table_create). Het volgende voorbeeld wordt een routetabel met de naam *myRouteTablePublic*. 

```azurecli-interactive 
# Create a route table
az network route-table create \
  --resource-group myResourceGroup \
  --name myRouteTablePublic
```

## <a name="create-a-route"></a>Een route maken

Een routetabel bevat nul of meer routes. Een route maken in de routetabel met [az routetabel netwerkroute maken](/cli/azure/network/route-table/route#az_network_route_table_route_create). 

```azurecli-interactive
az network route-table route create \
  --name ToPrivateSubnet \
  --resource-group myResourceGroup \
  --route-table-name myRouteTablePublic \
  --address-prefix 10.0.1.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 10.0.2.4
``` 

De route wordt al het verkeer dat is bestemd voor het adresvoorvoegsel 10.0.1.0/24 via een virtueel netwerk-apparaat met het IP-adres 10.0.2.4 doorverwezen. De virtuele netwerkapparaat en het subnet met het opgegeven adres-voorvoegsel worden in latere stappen gemaakt. De route overschrijft van Azure-standaard routering, die verkeer tussen subnetten rechtstreeks gerouteerd. Elke route geeft een volgend hoptype. Het volgende hoptype geïnstrueerd Azure hoe het verkeer te routeren. In dit voorbeeld wordt het volgende hoptype is *VirtualAppliance*. Zie voor meer informatie over alle beschikbare hoptypen in de volgende in Azure, en wanneer u ze gebruikt, [volgende hop typen](virtual-networks-udr-overview.md#custom-routes).

## <a name="associate-a-route-table-to-a-subnet"></a>Een routetabel aan een subnet koppelen

Voordat u een routetabel aan een subnet koppelt kunt, hebt u een virtueel netwerk en subnet maken. Een virtueel netwerk maken met één subnet met [az network vnet maken](/cli/azure/network/vnet#az_network_vnet_create).

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

Twee extra subnetten met [az network vnet subnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create).

```azurecli-interactive
# Create a private subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24

# Create a DMZ subnet.
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name DMZ \
  --address-prefix 10.0.2.0/24
```

U kunt een routetabel met nul of meer subnetten koppelen. Een subnet kan nul of één routetabel gekoppeld hebben. Uitgaand verkeer van een subnet wordt doorgestuurd op basis van Azure standaardroutes en eventuele aangepaste routes die u hebt toegevoegd aan een routetabel die u aan een subnet koppelen. Koppel de *myRouteTablePublic* routetabel aan de *openbare* subnet met [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update).

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Public \
  --resource-group myResourceGroup \
  --route-table myRouteTablePublic
```

Voordat u implementeert routetabellen voor gebruik in productieomgevingen, wordt aanbevolen dat u zorgvuldig vertrouwd raken met [routering in Azure](virtual-networks-udr-overview.md) en [Azure limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="test-routing"></a>Test routering

Als u wilt testen routering, maakt u een virtuele machine die als het virtuele netwerk-apparaat fungeert dat de route die u in de vorige stap hebt gemaakt door middel van routes. Na het maken van het virtuele netwerk-apparaat, implementeert u een virtuele machine in de *openbare* en *persoonlijke* subnetten. U moet vervolgens doorsturen verkeer van de *openbare* subnet moet de *persoonlijke* subnet via het netwerk virtuele apparaat.

### <a name="create-a-network-virtual-appliance"></a>Maken van een virtueel netwerkapparaat

In de vorige stap, moet u een route die een virtueel netwerkapparaat als het volgende hoptype opgegeven gemaakt. Een virtuele machine met een netwerktoepassing wordt vaak een virtueel netwerkapparaat genoemd. In productieomgevingen is het netwerk virtuele apparaat die u implementeert vaak een vooraf geconfigureerde virtuele machine. Verschillende virtuele netwerkapparaten zijn beschikbaar via de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1). In dit artikel wordt wordt een eenvoudige virtuele machine gemaakt. 

Maken van een virtueel netwerkapparaat in de *DMZ* subnet met [az vm maken](/cli/azure/vm#az_vm_create). Wanneer u een virtuele machine maakt, wordt in Azure maakt en een openbaar IP-adres aan de virtuele machine standaard wordt toegewezen. De `--public-ip-address ""` parameter geeft Azure niet te maken en een openbare IP-adres toewijzen aan de virtuele machine, omdat de virtuele machine hoeft niet van het Internet zijn verbonden met de opdracht. Als SSH-sleutels niet al bestaan op de standaardlocatie van de sleutel, wordt deze de opdracht maken. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.

```azure-cli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmNva \
  --image UbuntuLTS \
  --public-ip-address "" \
  --subnet DMZ \
  --vnet-name myVirtualNetwork \
  --generate-ssh-keys
```

De virtuele machine duurt een paar minuten maken. Ga niet door met de volgende stap totdat Azure is de virtuele machine gemaakt en wordt de virtuele machine-uitvoer geretourneerd. In productieomgevingen is het netwerk virtuele apparaat die u implementeert vaak een vooraf geconfigureerde virtuele machine. Verschillende virtuele netwerkapparaten zijn beschikbaar via de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?search=network%20virtual%20appliance&page=1).

Moet u doorsturen via IP voor elke Azure inschakelen [netwerkinterface](virtual-network-network-interface.md) gekoppeld aan een virtuele machine die bestemd zijn voor alle IP-adres dat niet is toegewezen aan de netwerkinterface verkeer doorstuurt. Tijdens het maken van het virtuele netwerk-apparaat in de *DMZ* subnet, Azure automatisch gemaakt een netwerkinterface met de naam *myVmNvaVMNic*, de netwerkinterface gekoppeld aan de virtuele machine, en de persoonlijke IP-adres toegewezen *10.0.2.4* aan de netwerkinterface. Doorsturen via IP voor de netwerkinterface met inschakelen [az netwerk nic update](/cli/azure/network/nic#az_network_nic_update).

```azurecli-interactive
az network nic update \
  --name myVmNvaVMNic \
  --resource-group myResourceGroup \
  --ip-forwarding true
```

Vanuit de virtuele machine moet het besturingssysteem of een toepassing die wordt uitgevoerd op de virtuele machine ook kunnen voor het doorsturen van netwerkverkeer. Wanneer u een virtueel netwerkapparaat in een productieomgeving implementeert, het toestel doorgaans filters, Logboeken of een andere functie uitgevoerd voordat het doorsturen van verkeer. In dit artikel, het besturingssysteem gewoon alle verkeer wordt doorgestuurd die het ontvangt. Doorsturen via IP in het besturingssysteem van de virtuele machine met inschakelen [az vm-extensie set](/cli/azure/vm/extension#az_vm_extension_set), die een opdracht die doorsturen via IP in het besturingssysteem wordt uitgevoerd.

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVmNva \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
```
De opdracht kan een minuut duren om uit te voeren.

### <a name="create-virtual-machines"></a>Virtuele machines maken

Twee virtuele machines in het virtuele netwerk maken, zodat u dat verkeer van valideren kunt de *openbare* subnet wordt doorgestuurd naar de *persoonlijke* subnet via het virtuele netwerk-apparaat in een later stadium. 

Maken van een virtuele machine in de *openbare* subnet met [az vm maken](/cli/azure/vm#az_vm_create). De `--no-wait` parameter zorgt ervoor dat Azure voor het uitvoeren van de opdracht op de achtergrond, zodat u kunt doorgaan naar de volgende opdracht. In dit artikel stroomlijnen, wordt een wachtwoord gebruikt. Sleutels worden doorgaans gebruikt in productie-implementaties. Als u sleutels gebruikt, moet u ook doorsturen van de SSH-agent configureren. Zie voor meer informatie de documentatie voor de SSH-client. Vervang `<replace-with-your-password>` in de volgende opdracht met een wachtwoord van uw keuze.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmWeb \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --admin-username azureuser \
  --admin-password $adminPassword \
  --no-wait
```

Maken van een virtuele machine in de *persoonlijke* subnet.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmMgmt \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Private \
  --admin-username azureuser \
  --admin-password $adminPassword
```

De virtuele machine duurt een paar minuten maken. Nadat de virtuele machine is gemaakt, toont de Azure CLI informatie vergelijkbaar met het volgende voorbeeld: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmMgmt",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.1.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```
Noteer de **publicIpAddress**. Dit adres wordt gebruikt voor toegang tot de virtuele machine vanaf het Internet in een later stadium.

### <a name="route-traffic-through-a-network-virtual-appliance"></a>-Routeverkeer via een virtueel netwerkapparaat

Gebruik de volgende opdracht voor het maken van een SSH-sessie met de *myVmMgmt* virtuele machine. Vervang  *<publicIpAddress>*  met het openbare IP-adres van uw virtuele machine. In het bovenstaande voorbeeld is het IP-adres is *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

Wanneer u wordt gevraagd om een wachtwoord, typt u het wachtwoord die u hebt geselecteerd in [maken van virtuele machines](#create-virtual-machines).

Gebruik de volgende opdracht traceroute installeren op de *myVmMgmt* virtuele machine:

```bash 
sudo apt-get install traceroute
```

Gebruik de volgende opdracht voor het testen van routering voor netwerkverkeer naar de *myVmWeb* virtuele machine van de *myVmMgmt* virtuele machine.

```bash
traceroute myvmweb
```

Het antwoord is vergelijkbaar met het volgende voorbeeld:

```bash
traceroute to myvmweb (10.0.0.4), 30 hops max, 60 byte packets
1  10.0.0.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```

U kunt zien dat verkeer wordt gerouteerd rechtstreeks vanuit de *myVmMgmt* virtuele machine naar de *myVmWeb* virtuele machine. Azure standaardroutes verkeer leiden tussen de subnetten. 

Gebruik de volgende opdracht om SSH kunt uitvoeren naar de *myVmWeb* virtuele machine van de *myVmMgmt* virtuele machine:

```bash 
ssh azureuser@myVmWeb
```

Gebruik de volgende opdracht traceroute installeren op de *myVmWeb* virtuele machine:

```bash 
sudo apt-get install traceroute
```

Gebruik de volgende opdracht voor het testen van routering voor netwerkverkeer naar de *myVmMgmt* virtuele machine van de *myVmWeb* virtuele machine.

```bash
traceroute myvmmgmt
```

Het antwoord is vergelijkbaar met het volgende voorbeeld:

```bash
traceroute to myvmmgmt (10.0.1.4), 30 hops max, 60 byte packets
1  10.0.2.4 (10.0.2.4)  0.781 ms  0.780 ms  0.775 ms
2  10.0.1.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```
U ziet dat de eerste hop 10.0.2.4 die de virtuele netwerkapparaat van privé IP-adres is. De tweede hop is 10.0.1.4, de persoonlijke IP-adres van de *myVmMgmt* virtuele machine. De route toegevoegd aan de *myRouteTablePublic* routetabel en die is gekoppeld aan de *openbare* subnet veroorzaakt Azure voor het routeren van het verkeer via de NVA in plaats van rechtstreeks naar de *persoonlijke* subnet.

Sluit de SSH-sessies tot zowel de *myVmWeb* en *myVmMgmt* virtuele machines.

## <a name="troubleshoot-routing"></a>Routering oplossen

Zoals u in de vorige stappen hebt geleerd, geldt Azure standaardroutes die u desgewenst met uw eigen routes overschrijven kunt. Verkeer kan soms niet zoals verwacht dat het worden gerouteerd. Gebruik [az netwerk-watcher weergeven volgende hop](/cli/azure/network/watcher#az_network_watcher_show_next_hop) om te bepalen hoe verkeer wordt gerouteerd tussen twee virtuele machines. Bijvoorbeeld de volgende opdracht test voor verkeersroutering van de *myVmWeb* (10.0.0.4) virtuele machine naar de *myVmMgmt* (10.0.1.4) virtuele machine:

```azurecli-interactive
# Enable network watcher for east region, if you don't already have a network watcher enabled for the region.
az network watcher configure --locations eastus --resource-group myResourceGroup --enabled true

```azurecli-interactive
az network watcher show-next-hop \
  --dest-ip 10.0.1.4 \
  --resource-group myResourceGroup \
  --source-ip 10.0.0.4 \
  --vm myVmWeb \
  --out table
```
De volgende uitvoer wordt geretourneerd na een korte wachten:

```azurecli
NextHopIpAddress    NextHopType       RouteTableId
------------------  ---------------- ---------------------------------------------------------------------------------------------------------------------------
10.0.2.4            VirtualAppliance  /subscriptions/<Subscription-Id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/routeTables/myRouteTablePublic
```

De uitvoer wordt gemeld dat de volgende hop-IP-adres voor verkeer van *myVmWeb* naar *myVmMgmt* 10.0.2.4 wordt (de *myVmNva* virtuele machine), het volgende hoptype is  *VirtualAppliance*, en dat de routetabel zorgt ervoor dat de routering *myRouteTablePublic*.

De effectieve routes voor elke netwerkinterface zijn een combinatie van Azure standaardroutes en alle routes die u definieert. Zie voor een netwerkinterface in een virtuele machine met effectieve van alle routes [az netwerk nic weergeven-ingang-route-tabel](/cli/azure/network/nic#az_network_nic_show_effective_route_table). Bijvoorbeeld, om weer te geven van de effectieve routes voor de *MyVmWebVMNic* netwerkinterface in de *myVmWeb* virtuele machine, voer de volgende opdracht:

```azurecli-interactive
az network nic show-effective-route-table \
  --name MyVmWebVMNic \
  --resource-group myResourceGroup
```

Alle standaardroutes en de route die u in de vorige stap hebt toegevoegd, worden geretourneerd.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, gebruik [az groep verwijderen](/cli/azure/group#az_group_delete) verwijderen van de resourcegroep en alle resources bevat.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een routetabel gemaakt en gekoppeld aan een subnet. U hebt gemaakt met een virtueel netwerkapparaat dat verkeer van een openbare subnet naar een persoonlijke subnet gerouteerd. Terwijl u veel Azure-resources binnen een virtueel netwerk implementeren kunt, kunnen geen resources voor sommige Azure PaaS-services worden geïmplementeerd in een virtueel netwerk. U kunt nog steeds toegang beperken tot de bronnen van sommige services Azure PaaS verkeer alleen via een virtueel netwerksubnet al. Ga naar de volgende zelfstudie voor informatie over het netwerktoegang tot Azure PaaS-resources te beperken.

> [!div class="nextstepaction"]
> [Beperken van toegang tot het netwerk voor PaaS-resources](virtual-network-service-endpoints-configure.md#azure-cli)
