---
title: Filteren van netwerkverkeer - Azure CLI | Microsoft Docs
description: In dit artikel leert u hoe u netwerkverkeer filtert naar een subnet, met een netwerkbeveiligingsgroep met de Azure CLI.
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
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38721064"
---
# <a name="filter-network-traffic-with-a-network-security-group-using-the-azure-cli"></a>Netwerkverkeer filteren met een netwerkbeveiligingsgroep met de Azure CLI

U kunt het netwerkverkeer inkomend in en uitgaand naar een subnet van een virtueel netwerk filteren met een netwerkbeveiligingsgroep. Netwerkbeveiligingsgroepen bevatten beveiligingsregels die netwerkverkeer filteren op IP-adres, poort en protocol. Beveiligingsregels worden toegepast op resources die zijn geïmplementeerd in een subnet. In dit artikel leert u het volgende:

* Een netwerkbeveiligingsgroep en beveiligingsregels maken
* Een virtueel netwerk maken en een netwerkbeveiligingsgroep koppelen aan een subnet
* Virtuele machines (VM) implementeren in een subnet
* Verkeersfilters testen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om te installeren en de CLI lokaal gebruikt, in dit artikel moet u de Azure CLI versie 2.0.28 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 


## <a name="create-a-network-security-group"></a>Een netwerkbeveiligingsgroep maken

Een netwerkbeveiligingsgroep bevat beveiligingsregels. Beveiligingsregels geven een bron en doel op. Bronnen en doelen kunnen toepassingsbeveiligingsgroepen zijn.

### <a name="create-application-security-groups"></a>Toepassingsbeveiligingsgroepen maken

Maakt u eerst een resourcegroep voor alle resources die zijn gemaakt in dit artikel met [az-groep maken](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam gemaakt op de locatie *eastus*: 

```azurecli-interactive
az group create \
  --name myResourceGroup \
  --location eastus
```

Maak een toepassingsbeveiligingsgroep met [az netwerk asg maken](/cli/azure/network/asg#az_network_asg_create). Met een toepassingsbeveiligingsgroep kunt u servers met vergelijkbare poortfiltervereisten groeperen. In het volgende voorbeeld worden twee toepassingsbeveiligingsgroepen gemaakt.

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

Maak een netwerkbeveiligingsgroep met [az network nsg maken](/cli/azure/network/nsg#az_network_nsg_create). In het volgende voorbeeld wordt een netwerkbeveiligingsgroep met de naam *myNsg* gemaakt: 

```azurecli-interactive 
# Create a network security group
az network nsg create \
  --resource-group myResourceGroup \
  --name myNsg
```

### <a name="create-security-rules"></a>Beveiligingsregels maken

Maak een beveiligingsregel met [az network nsg-regel maken](/cli/azure/network/nsg/rule#az_network_nsg_rule_create). In het volgende voorbeeld wordt een regel gemaakt die inkomend verkeer van internet naar de toepassingsbeveiligingsgroep *myWebServers* toestaat via de poorten 80 en 443:

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

Het volgende voorbeeld wordt een regel waarmee verkeer binnenkomend verkeer van Internet naar de *myMgmtServers* toepassingsbeveiligingsgroep via poort 22:

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

In dit artikel SSH (poort 22) wordt blootgesteld aan internet voor de *myAsgMgmtServers* VM. Voor productieomgevingen, in plaats van poort 22 op het internet, wordt aanbevolen dat u verbinding maken met Azure-resources die u wilt beheren met behulp van een [VPN](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [persoonlijke](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) netwerkverbinding.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak een virtueel netwerk met [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). In het volgende voorbeeld wordt een virtueel netwerk met de naam *myVirtualNetwork* gemaakt:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16
```

Voeg een subnet toe aan een virtueel netwerk met [az network vnet subnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). In het volgende voorbeeld wordt een subnet met de naam *mySubnet* toegevoegd aan het virtuele netwerk en wordt de netwerkbeveiligingsgroep *myNsg* hieraan gekoppeld:

```azurecli-interactive
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name mySubnet \
  --address-prefix 10.0.0.0/24 \
  --network-security-group myNsg
```

## <a name="create-virtual-machines"></a>Virtuele machines maken

Maak twee virtuele machines in het virtuele netwerk, zodat u het filteren van verkeer in een latere stap kunt controleren. 

Maak een VM met [az vm create](/cli/azure/vm#az_vm_create). In het volgende voorbeeld wordt een virtuele machine gemaakt die als een webserver fungeert. De `--asgs myAsgWebServers` optie zorgt ervoor dat Azure om de netwerkinterface die is gemaakt voor de virtuele machine lid is van de *myAsgWebServers* toepassingsbeveiligingsgroep.

De `--nsg ""` optie is opgegeven om te voorkomen dat Azure het maken van een standaard voor de netwerkbeveiligingsgroep voor de netwerkinterface die Azure maakt bij het maken van de virtuele machine. Een wachtwoord wordt gebruikt voor het stroomlijnen van dit artikel. Sleutels worden doorgaans gebruikt in productie-implementaties. Als u sleutels gebruikt, moet u ook doorsturen voor de resterende stappen van SSH-agent configureren. Zie voor meer informatie de documentatie van uw SSH-client. Vervang `<replace-with-your-password>` in de volgende opdracht uit met een wachtwoord van uw keuze.

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

Het maken van de virtuele machine duurt een paar minuten. Nadat de virtuele machine is gemaakt, wordt de uitvoer is vergelijkbaar met het volgende voorbeeld geretourneerd: 

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

Let op het **openbare IP-adres**. Dit adres wordt gebruikt voor toegang tot de virtuele machine via internet in een latere stap.  Maak een VM die fungeert als een beheerserver:

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

Het maken van de virtuele machine duurt een paar minuten. Als de virtuele machine is gemaakt, noteert u de **publicIpAddress** in de resulterende uitvoer. Dit adres wordt gebruikt voor toegang tot de virtuele machine in de volgende stap. Ga pas verder met de volgende stap als Azure klaar is met het maken van de virtuele machine.

## <a name="test-traffic-filters"></a>Verkeersfilters testen

Gebruik de opdracht die volgt op voor het maken van een SSH-sessie met de *myVmMgmt* VM. Vervang *<publicIpAddress>* met het openbare IP-adres van uw virtuele machine. In het bovenstaande voorbeeld het IP-adres is *13.90.242.231*.

```bash 
ssh azureuser@<publicIpAddress>
```

Wanneer u hierom wordt gevraagd om een wachtwoord, typt u het wachtwoord die u hebt ingevoerd in [maakt virtuele machines](#create-virtual-machines).

De verbinding slaagt, omdat poort 22 wordt toegestaan inkomend verkeer van Internet naar de *myAsgMgmtServers* toepassingsbeveiligingsgroep die de netwerkinterface die is gekoppeld aan de *myVmMgmt* virtuele machine.

Gebruik de volgende opdracht voor SSH naar de *myVmWeb* VM op basis van de *myVmMgmt* VM:

```bash 
ssh azureuser@myVmWeb
```

De verbinding slaagt omdat een standaardbeveiligingsregel binnen elke netwerkbeveiligingsgroep verkeer via alle poorten tussen alle IP-adressen binnen een virtueel netwerk toestaat. U kunt geen SSH-verbinding de *myVmWeb* VM op basis van het Internet omdat de beveiligingsregel voor de *myAsgWebServers* niet toegestaan om poort 22 inkomend verkeer van Internet.

Gebruik de volgende opdrachten de nginx-webserver installeren op de *myVmWeb* VM:

```bash 
# Update package source
sudo apt-get -y update

# Install NGINX
sudo apt-get -y install nginx
```

De *myVmWeb* VM uitgaand naar het Internet nginx ophalen omdat een standaardbeveiligingsregel kan al het uitgaande verkeer naar Internet is toegestaan. Sluit de *myVmWeb* SSH-sessie, waarbij u op de `username@myVmMgmt:~$` vragen van de *myVmMgmt* VM. Om op te halen van het nginx-welkomstscherm van de *myVmWeb* VM, voer de volgende opdracht:

```bash
curl myVmWeb
```

Meld u af bij de *myVmMgmt* VM. Om te bevestigen of u toegang hebt tot de *myVmWeb* webserver buiten Azure, voer `curl <publicIpAddress>` vanaf uw eigen computer. De verbinding slaagt, omdat poort 80 is toegestaan inkomend verkeer van Internet naar de *myAsgWebServers* toepassingsbeveiligingsgroep die de netwerkinterface die is gekoppeld aan de *myVmWeb* virtuele machine.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, gebruikt u [az group delete](/cli/azure/group#az_group_delete) om de resourcegroep en alle resources die deze bevat te verwijderen.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een netwerkbeveiligingsgroep gemaakt en die aan een subnet van een virtueel netwerk gekoppeld. Zie [Overzicht van netwerkbeveiligingsgroepen](security-overview.md) en [Een beveiligingsgroep beheren](manage-network-security-group.md) voor meer informatie over netwerkbeveiligingsgroepen.

Azure routeert standaard verkeer tussen subnetten. In plaats daarvan kunt u verkeer routeren tussen subnetten via een virtuele machine, die bijvoorbeeld als een firewall fungeert. Voor meer informatie Zie [een routetabel maken](tutorial-create-route-table-cli.md).