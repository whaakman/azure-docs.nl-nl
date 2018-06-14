---
title: Filteren van netwerkverkeer - Azure CLI | Microsoft Docs
description: In dit artikel leert u hoe voor het filteren van netwerkverkeer naar een subnet, met een netwerkbeveiligingsgroep met de Azure CLI.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to filter network traffic to virtual machines that perform similar functions, such as web servers.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/30/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: 11dc0e5f6ee398b2a745ed60cbc166e2a1697c3e
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31423174"
---
# <a name="filter-network-traffic-with-a-network-security-group-using-the-azure-cli"></a>Filteren van netwerkverkeer met een netwerkbeveiligingsgroep met de Azure CLI

U kunt het netwerkverkeer naar binnenkomende en uitgaande van een virtueel netwerksubnet filteren met een netwerkbeveiligingsgroep. Netwerkbeveiligingsgroepen bevatten beveiligingsregels voor verbindingen die filteren van netwerkverkeer door IP-adres, poort en protocol. Beveiligingsregels worden toegepast op resources geïmplementeerd in een subnet. In dit artikel leert u het volgende:

* Een netwerkbeveiliging groep en beveiliging regels maken
* Een virtueel netwerk maken en koppelen van een netwerkbeveiligingsgroep aan een subnet
* Virtuele machines (VM) implementeren in een subnet
* De verkeersfilters van de test

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in dit artikel is vereist dat u de Azure CLI versie 2.0.28 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 


## <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

Een netwerkbeveiligingsgroep bevat beveiligingsregels voor verbindingen. Beveiligingsregels Geef een bron en bestemming. Bronnen en bestemmingen kunnen beveiligingsgroepen van toepassing zijn.

### <a name="create-application-security-groups"></a>Beveiligingsgroepen toepassing maken

Eerst maakt u een resourcegroep voor alle resources die zijn gemaakt in dit artikel met [az groep maken](/cli/azure/group#az_group_create). Het volgende voorbeeld maakt u een resourcegroep in de *eastus* locatie: 

```azurecli-interactive
az group create \
  --name myResourceGroup \
  --location eastus
```

Maak de beveiligingsgroep van een toepassing met [az netwerk asg maken](/cli/azure/network/asg#az_network_asg_create). De beveiligingsgroep van een toepassing kunt u groepsservers met vergelijkbaar poortfiltering vereisten. Het volgende voorbeeld maakt twee beveiligingsgroepen van toepassing.

```azurecli-interactive
az network asg create \
  --resource-group myResourceGroup \
  --name myAsgWebServers \
  --location eastus

az network asg create \
  --resource-group myResourceGroup \
  --name myAsgMgmtServers \
  --location eastus
```

### <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

Maken van een netwerkbeveiligingsgroep met [az netwerk nsg maken](/cli/azure/network/nsg#az_network_nsg_create). Het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNsg*: 

```azurecli-interactive 
# Create a network security group
az network nsg create \
  --resource-group myResourceGroup \
  --name myNsg
```

### <a name="create-security-rules"></a>Beveiligingsregels voor verbindingen maken

Maken van een regel met [az netwerk nsg regel maken](/cli/azure/network/nsg/rule#az_network_nsg_rule_create). Het volgende voorbeeld wordt een regel waarmee verkeer inkomend verkeer van internet naar de *myWebServers* toepassing beveiligingsgroep poorten 80 en 443:

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsg \
  --name Allow-Web-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 100 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-asgs "myAsgWebServers" \
  --destination-port-range 80 443
```

Het volgende voorbeeld wordt een regel waarmee verkeer inkomend verkeer van Internet naar de *myMgmtServers* beveiligingsgroep toepassing via poort 22:

```azurecli-interactive
az network nsg rule create \
  --resource-group myResourceGroup \
  --nsg-name myNsg \
  --name Allow-SSH-All \
  --access Allow \
  --protocol Tcp \
  --direction Inbound \
  --priority 110 \
  --source-address-prefix Internet \
  --source-port-range "*" \
  --destination-asgs "myAsgMgmtServers" \
  --destination-port-range 22
```

In dit artikel SSH (poort 22) wordt blootgesteld aan internet voor de *myAsgMgmtServers* VM. Voor productieomgevingen, in plaats van het blootstellen van poort 22 met internet, wordt aanbevolen dat u verbinding maken met Azure-resources die u wilt beheren met behulp van een [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [persoonlijke](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) netwerkverbinding.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak een virtueel netwerk met [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). Het volgende voorbeeld wordt een virtuele naam *myVirtualNetwork*:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16
```

Voeg een subnet toe aan een virtueel netwerk met [az network vnet subnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Het volgende voorbeeld wordt een subnet met de naam *mySubnet* met het virtuele netwerk en gekoppeld de *myNsg* netwerkbeveiligingsgroep aan:

```azurecli-interactive
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name mySubnet \
  --address-prefix 10.0.0.0/24 \
  --network-security-group myNsg
```

## <a name="create-virtual-machines"></a>Virtuele machines maken

Twee virtuele machines maken in het virtuele netwerk, zodat u kunt controleren of een latere stap van het filteren van verkeer. 

Maak een VM met [az vm create](/cli/azure/vm#az_vm_create). Het volgende voorbeeld wordt een virtuele machine die als een webserver fungeert. De `--asgs myAsgWebServers` optie zorgt ervoor dat Azure te maken van de netwerkinterface wordt gemaakt voor de virtuele machine lid is van de *myAsgWebServers* beveiligingsgroep van de toepassing.

De `--nsg ""` optie om te voorkomen dat Azure maken van een standaard voor de netwerkbeveiligingsgroep voor de netwerkinterface Azure maakt bij het maken van de virtuele machine is opgegeven. In dit artikel stroomlijnen, wordt een wachtwoord gebruikt. Sleutels worden doorgaans gebruikt in productie-implementaties. Als u sleutels gebruikt, moet u ook doorsturen voor de resterende stappen van SSH-agent configureren. Zie voor meer informatie de documentatie voor de SSH-client. Vervang `<replace-with-your-password>` in de volgende opdracht met een wachtwoord van uw keuze.

```azurecli-interactive
adminPassword="<replace-with-your-password>"

az vm create \
  --resource-group myResourceGroup \
  --name myVmWeb \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet mySubnet \
  --nsg "" \
  --asgs myAsgWebServers \
  --admin-username azureuser \
  --admin-password $adminPassword
```

De virtuele machine duurt een paar minuten maken. Nadat de virtuele machine is gemaakt, wordt de uitvoer is vergelijkbaar met het volgende voorbeeld wordt geretourneerd: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVmWeb",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.90.242.231",
  "resourceGroup": "myResourceGroup"
}
```

Noteer de **publicIpAddress**. Dit adres wordt gebruikt voor toegang tot de virtuele machine vanaf het internet in een later stadium.  Maak een VM moeten fungeren als een beheerserver:

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVmMgmt \
  --image UbuntuLTS \
  --vnet-name myVirtualNetwork \
  --subnet mySubnet \
  --nsg "" \
  --asgs myAsgMgmtServers \
  --admin-username azureuser \
  --admin-password $adminPassword
```

De virtuele machine duurt een paar minuten maken. Nadat de virtuele machine is gemaakt, let dan op de **publicIpAddress** in de resulterende uitvoer. Dit adres wordt gebruikt voor toegang tot de virtuele machine in de volgende stap. Niet verder gaan met de volgende stap voordat Azure is gemaakt van de virtuele machine.

## <a name="test-traffic-filters"></a>De verkeersfilters van de test

Gebruik de opdracht die volgt maken van een SSH-sessie met de *myVmMgmt* VM. Vervang *<publicIpAddress>* met het openbare IP-adres van uw virtuele machine. In het bovenstaande voorbeeld is het IP-adres is *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

Wanneer u wordt gevraagd om een wachtwoord, typt u het wachtwoord die u hebt ingevoerd in [virtuele machines maken](#create-virtual-machines).

De verbinding slaagt, omdat poort 22 is toegestaan inkomende van Internet naar de *myAsgMgmtServers* toepassing beveiligingsgroep die de netwerkinterface is gekoppeld aan de *myVmMgmt* VM bevindt zich in.

Gebruik de volgende opdracht om SSH kunt uitvoeren naar de *myVmWeb* VM van de *myVmMgmt* VM:

```bash 
ssh azureuser@myVmWeb
```

De verbinding slaagt omdat een standaardbeveiligingsregel binnen elke netwerkbeveiligingsgroep verkeer via alle poorten tussen alle IP-adressen binnen een virtueel netwerk is toegestaan. U kunt geen SSH kunt uitvoeren naar de *myVmWeb* VM vanaf het Internet omdat de regel voor de beveiliging voor de *myAsgWebServers* poort niet is toegestaan 22 inkomend van het Internet.

Gebruik de volgende opdrachten de nginx-webserver installeren op de *myVmWeb* VM:

```bash 
# Update package source
sudo apt-get -y update

# Install NGINX
sudo apt-get -y install nginx
```

De *myVmWeb* VM is toegestaan uitgaand naar het Internet nginx ophalen omdat een standaardbeveiligingsregel al het uitgaande verkeer met het Internet toestaat. Sluit de *myVmWeb* SSH-sessie, waarbij u op de `username@myVmMgmt:~$` vragen van de *myVmMgmt* VM. Voor het ophalen van het nginx-welkomstscherm van de *myVmWeb* virtuele machine, voer de volgende opdracht:

```bash
curl myVmWeb
```

Meld u af bij de *myVmMgmt* VM. Om te bevestigen dat u toegang hebt tot de *myVmWeb* webserver buiten Azure, voert u `curl <publicIpAddress>` vanaf uw eigen computer. De verbinding slaagt, omdat poort 80 is toegestaan inkomende van Internet naar de *myAsgWebServers* toepassing beveiligingsgroep die de netwerkinterface is gekoppeld aan de *myVmWeb* VM bevindt zich in.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, gebruik [az groep verwijderen](/cli/azure/group#az_group_delete) verwijderen van de resourcegroep en alle resources bevat.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt een netwerkbeveiligingsgroep gemaakt en die aan een virtueel netwerksubnet zijn gekoppeld. Zie voor meer informatie over netwerkbeveiligingsgroepen, [netwerk groep beveiligingsoverzicht](security-overview.md) en [beheren van een netwerkbeveiligingsgroep](manage-network-security-group.md).

Azure routes verkeer tussen subnetten standaard. In plaats daarvan kunt u verkeer leiden tussen subnetten door middel van een virtuele machine, fungeren als een firewall, bijvoorbeeld. Voor meer informatie Zie [een routetabel maken](tutorial-create-route-table-cli.md).