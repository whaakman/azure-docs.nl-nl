---
title: Doorsturen van netwerkverkeer - Azure CLI | Microsoft Docs
description: Informatie over het netwerkverkeer omgeleid met een routetabel met de Azure CLI in dit artikel.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: eb4a28b5a57d7e301e800cd4ad87c56b7c5df6d2
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-cli"></a>Doorsturen van netwerkverkeer met een routetabel met de Azure CLI

Azure automatisch routes verkeer tussen alle subnetten in een virtueel netwerk, standaard. U kunt uw eigen routes voor het overschrijven van Azure maken standaardroutering. De mogelijkheid voor het maken van aangepaste routes is handig als u bijvoorbeeld het routeren van verkeer tussen subnetten via een netwerk (NVA) voor virtueel apparaat. In dit artikel leert u hoe:

* Een routetabel maken
* Een route maken
* Een virtueel netwerk maken met meerdere subnetten
* Een routetabel aan een subnet koppelen
* Maken van een NVA waarmee verkeer
* Virtuele machines (VM) implementeren in verschillende subnetten
* Verkeer leiden uit één subnet naar een andere door middel van een NVA

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, is deze snelstartgids vereist dat u de Azure CLI versie 2.0.28 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="create-a-route-table"></a>Een routetabel maken

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

Een route maken in de routetabel met [az routetabel netwerkroute maken](/cli/azure/network/route-table/route#az_network_route_table_route_create). 

```azurecli-interactive
az network route-table route create \
  --name ToPrivateSubnet \
  --resource-group myResourceGroup \
  --route-table-name myRouteTablePublic \
  --address-prefix 10.0.1.0/24 \
  --next-hop-type VirtualAppliance \
  --next-hop-ip-address 10.0.2.4
``` 

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

Koppel de *myRouteTablePublic* routetabel aan de *openbare* subnet met [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update).

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Public \
  --resource-group myResourceGroup \
  --route-table myRouteTablePublic
```

## <a name="create-an-nva"></a>Maken van een NVA

Een NVA is een virtuele machine die een netwerkfunctie, zoals routering, gebruik of WAN-optimalisatie uitvoert.

Maken van een NVA in de *DMZ* subnet met [az vm maken](/cli/azure/vm#az_vm_create). Wanneer u een virtuele machine maakt, wordt in Azure maakt en een openbaar IP-adres voor de VM standaard wordt toegewezen. De `--public-ip-address ""` parameter geeft Azure niet te maken en een openbare IP-adres toewijzen aan de virtuele machine, omdat de virtuele machine hoeft niet van het internet zijn verbonden met de opdracht. Als SSH-sleutels niet al bestaan op de standaardlocatie van de sleutel, wordt deze de opdracht maken. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.

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

De virtuele machine duurt een paar minuten maken. Ga niet door met de volgende stap totdat Azure is de virtuele machine gemaakt en wordt over de VM-uitvoer geretourneerd. 

Voor een netwerkinterface om te kunnen doorsturen van netwerkverkeer dat is verzonden, en dat niet bestemd is voor eigen IP-adres, moet doorsturen via IP worden ingeschakeld voor de netwerkinterface. Doorsturen via IP voor de netwerkinterface met inschakelen [az netwerk nic update](/cli/azure/network/nic#az_network_nic_update).

```azurecli-interactive
az network nic update \
  --name myVmNvaVMNic \
  --resource-group myResourceGroup \
  --ip-forwarding true
```

In de VM moet het besturingssysteem of een toepassing die wordt uitgevoerd vanuit de virtuele machine, ook kunnen voor het doorsturen van netwerkverkeer. Doorsturen via IP in het besturingssysteem van de virtuele machine met inschakelen [az vm-extensie set](/cli/azure/vm/extension#az_vm_extension_set):

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVmNva \
  --name customScript \
  --publisher Microsoft.Azure.Extensions \
  --settings '{"commandToExecute":"sudo sysctl -w net.ipv4.ip_forward=1"}'
```
De opdracht kan een minuut duren om uit te voeren.

## <a name="create-virtual-machines"></a>Virtuele machines maken

Twee virtuele machines in het virtuele netwerk maken, zodat u dat verkeer van valideren kunt de *openbare* subnet wordt doorgestuurd naar de *persoonlijke* subnet via de NVA in een later stadium. 

Maak een VM in de *openbare* subnet met [az vm maken](/cli/azure/vm#az_vm_create). De `--no-wait` parameter zorgt ervoor dat Azure voor het uitvoeren van de opdracht op de achtergrond, zodat u kunt doorgaan naar de volgende opdracht. In dit artikel stroomlijnen, wordt een wachtwoord gebruikt. Sleutels worden doorgaans gebruikt in productie-implementaties. Als u sleutels gebruikt, moet u ook doorsturen van de SSH-agent configureren. Zie voor meer informatie de documentatie voor de SSH-client. Vervang `<replace-with-your-password>` in de volgende opdracht met een wachtwoord van uw keuze.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmPublic \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet Public \
  --admin-username azureuser \
  --admin-password $adminPassword \
  --no-wait
```

Maak een VM in de *persoonlijke* subnet.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmPrivate \
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
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmPrivate",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.1.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```
Noteer de **publicIpAddress**. Dit adres wordt gebruikt voor toegang tot de virtuele machine vanaf het internet in een later stadium.

## <a name="route-traffic-through-an-nva"></a>-Routeverkeer via een NVA

Gebruik de volgende opdracht voor het maken van een SSH-sessie met de *myVmPrivate* VM. Vervang *<publicIpAddress>* met het openbare IP-adres van uw virtuele machine. In het bovenstaande voorbeeld is het IP-adres is *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

Wanneer u wordt gevraagd om een wachtwoord, typt u het wachtwoord die u hebt geselecteerd in [maken van virtuele machines](#create-virtual-machines).

Gebruik de volgende opdracht voor het installeren van de opdracht Traceroute op de *myVmPrivate* VM:

```bash 
sudo apt-get install traceroute
```

Gebruik de volgende opdracht voor het testen van routering voor netwerkverkeer naar de *myVmPublic* VM van de *myVmPrivate* VM.

```bash
traceroute myVmPublic
```

Het antwoord is vergelijkbaar met het volgende voorbeeld:

```bash
traceroute to myVmPublic (10.0.0.4), 30 hops max, 60 byte packets
1  10.0.0.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```

U kunt zien dat verkeer wordt gerouteerd rechtstreeks vanuit de *myVmPrivate* VM naar de *myVmPublic* VM. Azure standaardroutes verkeer leiden tussen de subnetten. 

Gebruik de volgende opdracht om SSH kunt uitvoeren naar de *myVmPublic* VM van de *myVmPrivate* VM:

```bash 
ssh azureuser@myVmPublic
```

Gebruik de volgende opdracht voor het installeren van de opdracht Traceroute op de *myVmPublic* VM:

```bash 
sudo apt-get install traceroute
```

Gebruik de volgende opdracht voor het testen van routering voor netwerkverkeer naar de *myVmPrivate* VM van de *myVmPublic* VM.

```bash
traceroute myVmPrivate
```

Het antwoord is vergelijkbaar met het volgende voorbeeld:

```bash
traceroute to myVmPrivate (10.0.1.4), 30 hops max, 60 byte packets
1  10.0.2.4 (10.0.2.4)  0.781 ms  0.780 ms  0.775 ms
2  10.0.1.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```
U ziet dat de eerste hop 10.0.2.4 die de NVA privé IP-adres is. De tweede hop is 10.0.1.4, de persoonlijke IP-adres van de *myVmPrivate* VM. De route toegevoegd aan de *myRouteTablePublic* routetabel en die is gekoppeld aan de *openbare* subnet veroorzaakt Azure voor het routeren van het verkeer via de NVA in plaats van rechtstreeks naar de *persoonlijke* subnet.

Sluit de SSH-sessies tot zowel de *myVmPublic* en *myVmPrivate* virtuele machines.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, gebruik [az groep verwijderen](/cli/azure/group#az_group_delete) verwijderen van de resourcegroep en alle resources bevat.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een routetabel gemaakt en gekoppeld aan een subnet. U hebt gemaakt met een eenvoudige NVA dat verkeer van een openbare subnet naar een persoonlijke subnet gerouteerd. Implementeren van tal van vooraf geconfigureerde NVAs waarmee de netwerkfuncties, zoals firewall- en WAN-optimalisatie van de [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Zie voor meer informatie over routering, [Routering-overzicht](virtual-networks-udr-overview.md) en [beheren een routetabel](manage-route-table.md).

Terwijl u veel Azure-resources binnen een virtueel netwerk implementeren kunt, kunnen geen resources voor sommige Azure PaaS-services worden geïmplementeerd in een virtueel netwerk. U kunt nog steeds toegang beperken tot de bronnen van sommige services Azure PaaS verkeer alleen via een virtueel netwerksubnet al. Voor meer informatie Zie [beperken van toegang tot het netwerk voor PaaS-resources](tutorial-restrict-network-access-to-resources-cli.md).
