---
title: Routeren van netwerkverkeer - Azure CLI | Microsoft Docs
description: In dit artikel leert u hoe u netwerkverkeer te routeren met een routetabel met behulp van de Azure CLI.
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
ms.openlocfilehash: 3217524ccfd03184a93a4e6637696195b3f0bf16
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "42060109"
---
# <a name="route-network-traffic-with-a-route-table-using-the-azure-cli"></a>Netwerkverkeer routeren met een routetabel met behulp van de Azure CLI

Azure routeert standaard automatisch verkeer tussen alle subnetten in een virtueel netwerk. U kunt uw eigen routes maken om de standaardroutering van Azure te overschrijven. De mogelijkheid voor het maken van aangepaste routes is handig als u bijvoorbeeld verkeer tussen subnetten wilt routeren via een NVA (virtueel netwerkapparaat). In dit artikel leert u het volgende:

* Een routetabel maken
* Een route maken
* Een virtueel netwerk met meerdere subnetten maken
* Een routetabel aan een subnet koppelen
* Een NVA maken voor het routeren van verkeer
* Virtuele machines (VM's) implementeren in verschillende subnetten
* Verkeer van het ene subnet naar het andere leiden via een NVA

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstart de Azure CLI versie 2.0.28 of later uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="create-a-route-table"></a>Een routetabel maken

Voordat u een routetabel maken kunt, maakt u een resourcegroep met [az-groep maken](/cli/azure/group#az_group_create) voor alle resources die in dit artikel is gemaakt. 

```azurecli-interactive
# Create a resource group.
az group create \
  --name myResourceGroup \
  --location eastus
``` 

Maak een routetabel met [az network route-table maken](/cli/azure/network/route-table#az-network-route-table-create). Het volgende voorbeeld wordt een routetabel met de naam *myRouteTablePublic*. 

```azurecli-interactive 
# Create a route table
az network route-table create \
  --resource-group myResourceGroup \
  --name myRouteTablePublic
```

## <a name="create-a-route"></a>Een route maken

Een route maken in de routetabel, met [az network route-table route maken](/cli/azure/network/route-table/route#az-network-route-table-route-create). 

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

Voordat u een routetabel aan een subnet koppelen kunt, moet u een virtueel netwerk en subnet maken. Een virtueel netwerk maken met één subnet met [az network vnet maken](/cli/azure/network/vnet#az_network_vnet_create).

```azurecli-interactive
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefix 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

Maak twee extra subnetten met [az network vnet subnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create).

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

Koppel de *myRouteTablePublic* routetabel aan de *openbare* subnet met de [az network vnet subnet update](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_update).

```azurecli-interactive
az network vnet subnet update \
  --vnet-name myVirtualNetwork \
  --name Public \
  --resource-group myResourceGroup \
  --route-table myRouteTablePublic
```

## <a name="create-an-nva"></a>Een NVA maken

Een NVA is een VM die een netwerkfunctie uitvoert, zoals routering, firewall of WAN-optimalisatie.

Maken van een NVA in het *DMZ* subnet met de [az vm maken](/cli/azure/vm#az_vm_create). Wanneer u een virtuele machine maakt, wordt Azure maakt en een openbaar IP-adres toegewezen aan de virtuele machine, standaard. De `--public-ip-address ""` parameter wordt gebruikt door Azure niet te maken en een openbaar IP-adres toewijzen aan de virtuele machine, omdat de virtuele machine hoeft niet te worden aangesloten op vanaf het internet. Als SSH-sleutels niet al bestaan op de standaardlocatie van de sleutel, worden ze met deze opdracht gemaakt. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`.

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

Het maken van de virtuele machine duurt een paar minuten. Ga niet verder met de volgende stap voordat Azure klaar is met het maken van de virtuele machine en uitvoer over de virtuele machine retourneert. 

Een netwerkinterface kan alleen netwerkverkeer doorsturen dat niet voor zijn eigen IP-adres bestemd is, als doorsturen via IP voor de netwerkinterface is ingeschakeld. Doorsturen via IP voor de netwerkinterface met inschakelen [az network nic update](/cli/azure/network/nic#az_network_nic_update).

```azurecli-interactive
az network nic update \
  --name myVmNvaVMNic \
  --resource-group myResourceGroup \
  --ip-forwarding true
```

In de VM moet het besturingssysteem of een toepassing die wordt uitgevoerd op de virtuele machine, ook netwerkverkeer kunnen doorsturen. Schakel doorsturen via IP in het besturingssysteem van de virtuele machine met [az vm extension set](/cli/azure/vm/extension#az_vm_extension_set):

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

Maak twee VM's in het virtuele netwerk, zodat u dat verkeer van valideren kunt de *openbare* subnet wordt doorgestuurd naar de *persoonlijke* subnet via de NVA in een latere stap. 

Maken van een virtuele machine in de *openbare* subnet met de [az vm maken](/cli/azure/vm#az_vm_create). De `--no-wait` parameter kunt u Azure aan de opdracht niet uitvoeren op de achtergrond, zodat u kunt doorgaan met de volgende opdracht. Een wachtwoord wordt gebruikt voor het stroomlijnen van dit artikel. Sleutels worden doorgaans gebruikt in productie-implementaties. Als u sleutels gebruikt, moet u ook doorsturen van SSH-agent configureren. Zie voor meer informatie de documentatie van uw SSH-client. Vervang `<replace-with-your-password>` in de volgende opdracht uit met een wachtwoord van uw keuze.

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

Maken van een virtuele machine in de *persoonlijke* subnet.

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

Het maken van de virtuele machine duurt een paar minuten. Nadat de virtuele machine is gemaakt, toont de Azure CLI informatie die vergelijkbaar is met het volgende voorbeeld: 

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
Let op het **openbare IP-adres**. Dit adres wordt gebruikt voor toegang tot de virtuele machine via internet in een latere stap.

## <a name="route-traffic-through-an-nva"></a>Verkeer routeren via een NVA

Gebruik de volgende opdracht om te maken van een SSH-sessie met de *myVmPrivate* VM. Vervang *<publicIpAddress>* met het openbare IP-adres van uw virtuele machine. In het bovenstaande voorbeeld het IP-adres is *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

Wanneer u hierom wordt gevraagd om een wachtwoord, typt u het wachtwoord die u hebt geselecteerd in [maken van virtuele machines](#create-virtual-machines).

Gebruik de volgende opdracht voor het installeren van de route traceren op de *myVmPrivate* VM:

```bash 
sudo apt-get install traceroute
```

Gebruik de volgende opdracht voor het testen van de routering van netwerkverkeer naar de *myVmPublic* VM op basis van de *myVmPrivate* VM.

```bash
traceroute myVmPublic
```

Het antwoord is vergelijkbaar met het volgende voorbeeld:

```bash
traceroute to myVmPublic (10.0.0.4), 30 hops max, 60 byte packets
1  10.0.0.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```

U ziet dat verkeer rechtstreeks vanuit *myVmPrivate* naar *myVmPublic* wordt geleid. Azure standaardroutes, verkeer rechtstreeks tussen subnetten. 

Gebruik de volgende opdracht voor SSH naar de *myVmPublic* VM op basis van de *myVmPrivate* VM:

```bash 
ssh azureuser@myVmPublic
```

Gebruik de volgende opdracht voor het installeren van de route traceren op de *myVmPublic* VM:

```bash 
sudo apt-get install traceroute
```

Gebruik de volgende opdracht voor het testen van de routering van netwerkverkeer naar de *myVmPrivate* VM op basis van de *myVmPublic* VM.

```bash
traceroute myVmPrivate
```

Het antwoord is vergelijkbaar met het volgende voorbeeld:

```bash
traceroute to myVmPrivate (10.0.1.4), 30 hops max, 60 byte packets
1  10.0.2.4 (10.0.2.4)  0.781 ms  0.780 ms  0.775 ms
2  10.0.1.4 (10.0.0.4)  1.404 ms  1.403 ms  1.398 ms
```
U ziet dat de eerste hop 10.0.2.4 is, het privé IP-adres van het NVA. De tweede hop is 10.0.1.4, het privé- IP-adres van de VM *myVmPrivate*. Door de route die is toegevoegd aan de routetabel *myRouteTablePublic* en gekoppeld aan het *Openbare* subnet leidt Azure het verkeer via het NVA in plaats van rechtstreeks naar het *Privé*-subnet.

Sluit de SSH-sessies met zowel de *myVmPublic* en *myVmPrivate* VM's.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, gebruikt u [az group delete](/cli/azure/group#az_group_delete) om de resourcegroep en alle resources die deze bevat te verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een routetabel gemaakt en die aan een subnet gekoppeld. U hebt een eenvoudig NVA gemaakt dat verkeer van een openbaar subnet naar een privé-subnet heeft geleid. U kunt allerlei vooraf geconfigureerde NVA's die netwerkfuncties uitvoeren zoals firewalls en WAN-optimalisatie, implementeren vanuit [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking). Zie [Routeringoverzicht](virtual-networks-udr-overview.md) en [Routetabel beheren](manage-route-table.md) voor meer informatie over routeren.

Hoewel u veel Azure-resources binnen een virtueel netwerk kunt implementeren, kunnen resources voor sommige Azure PaaS-diensten niet in een virtueel netwerk worden geïmplementeerd. U kunt de toegang tot de resources van sommige Azure PaaS-diensten echter nog steeds beperken tot alleen verkeer vanaf een subnet van een virtueel netwerk. Voor meer informatie Zie [netwerktoegang tot PaaS-resources beperken](tutorial-restrict-network-access-to-resources-cli.md).
