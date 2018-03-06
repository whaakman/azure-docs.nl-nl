---
title: 'Een virtueel Azure-netwerk maken met meerdere subnetten: Azure CLI | Microsoft Docs'
description: Informatie over het maken van een virtueel netwerk met meerdere subnetten met de Azure CLI.
services: virtual-network
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: 
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/01/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: feb8fb218b8bde88ba7fbdc3419e32c9313c7885
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2018
---
# <a name="create-a-virtual-network-with-multiple-subnets-using-the-azure-cli"></a>Een virtueel netwerk maken met meerdere subnetten met de Azure CLI

Een virtueel netwerk kunt verschillende soorten Azure-resources met het Internet en privé met elkaar communiceren. Meerdere subnetten in een virtueel netwerk maken, kunt u bij het segmenteren van uw netwerk, zodat u kunt filteren of het besturingselement de verkeersstroom tussen subnetten. In dit artikel leert u hoe:

> [!div class="checklist"]
> * Een virtueel netwerk maken
> * Een subnet maken
> * Testen van netwerkcommunicatie tussen virtuele machines

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli.md). 

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak een resourcegroep maken met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak een virtueel netwerk met [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). Het volgende voorbeeld wordt een virtueel netwerk met de naam *myVirtualNetwork* met het adresvoorvoegsel *10.0.0.0/16*. De opdracht maakt u één subnet met de naam *openbare*, met het adresvoorvoegsel *10.0.0.0/24*.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --address-prefixes 10.0.0.0/16 \
  --subnet-name Public \
  --subnet-prefix 10.0.0.0/24
```

Omdat een locatie niet in de voorgaande opdracht opgegeven is, Azure maakt het virtuele netwerk op dezelfde locatie die het *myResourceGroup* resourcegroep bestaat in. De **adresvoorvoegsels** en **subnetvoorvoegsel** zijn opgegeven in CIDR-notatie. Het opgegeven adresvoorvoegsel bevat het IP-adressen 10.0.0.0-10.0.255.254. Het voorvoegsel dat is opgegeven voor het subnet moet binnen het adresvoorvoegsel gedefinieerd voor het virtuele netwerk. Azure DHCP IP-adressen worden toegewezen vanuit een adresvoorvoegsel subnet naar bronnen die zijn geïmplementeerd in een subnet. Azure alleen de 10.0.0.4-10.0.0.254 adressen worden toegewezen aan resources die zijn geïmplementeerd in de **openbare** subnet, omdat de eerste vier adressen zijn gereserveerd Azure (10.0.0.0-10.0.0.3 voor het subnet van de in dit voorbeeld) en de laatste adres () 10.0.0.255 voor het subnet van de in dit voorbeeld) in elk subnet.

## <a name="create-a-subnet"></a>Een subnet maken

Maak een subnet met [az network vnet subnet maken](/cli/azure/network/vnet/subnet#az_network_vnet_subnet_create). Het volgende voorbeeld wordt een subnet met de naam *persoonlijke* binnen de *myVirtualNetwork* virtueel netwerk met het adresvoorvoegsel *10.0.1.0/24*. Het adresvoorvoegsel moet binnen het adresvoorvoegsel gedefinieerd voor het virtuele netwerk en het adresvoorvoegsel van andere subnetten in het virtuele netwerk niet overlappen.

```azurecli-interactive 
az network vnet subnet create \
  --vnet-name myVirtualNetwork \
  --resource-group myResourceGroup \
  --name Private \
  --address-prefix 10.0.1.0/24
```

Voordat u deze implementeert in Azure virtuele netwerken en subnetten voor gebruik in productieomgevingen, het is raadzaam dat u zorgvuldig vertrouwd raken met de adresruimte [overwegingen](virtual-network-manage-network.md#create-a-virtual-network) en [virtueel netwerk limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Zodra resources zijn geïmplementeerd in subnetten, kunnen sommige virtueel netwerk en subnet wijzigingen, zoals het wijzigen van-adresbereiken opnieuw distribueren van bestaande Azure-resources geïmplementeerd in subnetten vereisen.

## <a name="test-network-communication"></a>Test de netwerkcommunicatie

Een virtueel netwerk kunt verschillende soorten Azure-resources met het Internet en privé met elkaar communiceren. Een type resource dat u in een virtueel netwerk implementeren kunt is een virtuele machine. Twee virtuele machines maken in het virtuele netwerk, zodat u de netwerkcommunicatie tussen deze en het Internet in een later stadium testen kunt.

### <a name="create-virtual-machines"></a>Virtuele machines maken

Maak een virtuele machine met [az vm maken](/cli/azure/vm#az_vm_create). Het volgende voorbeeld wordt een virtuele machine met de naam *myVmWeb* in de *openbare* subnet. De `--no-wait` parameter zorgt ervoor dat Azure voor het uitvoeren van de opdracht op de achtergrond, zodat u kunt doorgaan naar de volgende opdracht. Deze zelfstudie stroomlijnen, wordt een wachtwoord gebruikt. Sleutels worden doorgaans gebruikt in productie-implementaties. Als u sleutels gebruikt, moet u ook doorsturen voor het voltooien van de resterende stappen van SSH-agent configureren. Zie voor meer informatie over het doorsturen van de SSH-agent, de documentatie voor uw SSH-client. Vervang `<replace-with-your-password>` in de volgende opdracht met een wachtwoord van uw keuze.

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

10.0.0.4 Azure automatisch toegewezen als de persoonlijke IP-adres van de virtuele machine omdat 10.0.0.4 is het eerste beschikbare IP-adres in de *openbare* subnet. 

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
De virtuele machine duurt een paar minuten maken. Nadat de virtuele machine is gemaakt, retourneert de Azure CLI uitvoer lijkt op het volgende voorbeeld: 

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

In het voorbeeld van uitvoer ziet u dat de **privateIpAddress** is *10.0.1.4*. Azure gemaakt een [netwerkinterface](virtual-network-network-interface.md), deze gekoppeld aan de virtuele machine, een particulier IP-adres van de netwerkinterface toegewezen en een **macAddress**. 10.0.1.4 Azure DHCP automatisch toegewezen aan de netwerkinterface omdat dit de eerste beschikbare IP-adres in de *persoonlijke* subnet. De particuliere IP- en MAC-adressen blijft toegewezen aan de netwerkinterface totdat de netwerkinterface wordt verwijderd. 

Noteer de **publicIpAddress**. Dit adres wordt gebruikt voor toegang tot de virtuele machine vanaf het Internet in een later stadium. Hoewel een virtuele machine is niet vereist voor het openbare IP-adres toegewezen, wijst Azure een openbaar IP-adres toe aan elke virtuele machine die u, standaard maakt. Om te communiceren via Internet met een virtuele machine, moet een openbaar IP-adres worden toegewezen aan de virtuele machine. Alle virtuele machines kunnen communiceren met het Internet, uitgaande of er een openbaar IP-adres is toegewezen aan de virtuele machine. Zie voor meer informatie over uitgaande Internet-verbindingen in Azure, [uitgaande verbindingen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

De virtuele machines die in dit artikel hebt [netwerkinterface](virtual-network-network-interface.md) met één IP-adres dat dynamisch wordt toegewezen aan de netwerkinterface. Nadat u de virtuele machine hebt geïmplementeerd, kunt u [meerdere openbare en particuliere IP-adressen toevoegen of wijzigen van de toewijzingsmethode van IP-adres naar statisch](virtual-network-network-interface-addresses.md#add-ip-addresses). U kunt [netwerkinterfaces toevoegen](virtual-network-network-interface-vm.md#vm-add-nic), tot de limiet wordt ondersteund door de [VM-grootte](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) dat u selecteert wanneer u een virtuele machine maken. U kunt ook [één hoofdelement i/o-virtualisatie (SR-IOV) inschakelen](create-vm-accelerated-networking-cli.md) voor een VM, maar alleen wanneer u een virtuele machine maken met een VM-grootte die ondersteuning de mogelijkheid biedt.

### <a name="communicate-between-virtual-machines-and-with-the-internet"></a>Communiceren tussen virtuele machines en met het internet

Gebruik de volgende opdracht voor het maken van een SSH-sessie met de *myVmMgmt* virtuele machine. Vervang `<publicIpAddress>` met het openbare IP-adres van uw virtuele machine. In het vorige voorbeeld het openbare IP-adres is *13.90.242.231*. Wanneer u wordt gevraagd om een wachtwoord, typt u het wachtwoord die u hebt ingevoerd in [maken van virtuele machines](#create-virtual-machines).

```bash 
ssh azureuser@<publicIpAddress>
```

Uit veiligheidsoverwegingen is het gebruikelijk om het aantal virtuele machines die worden op afstand verbinding met een virtueel netwerk gemaakt kunnen te beperken. In deze zelfstudie de *myVmMgmt* virtuele machine wordt gebruikt voor het beheren van de *myVmWeb* virtuele machine in het virtuele netwerk. Gebruik de volgende opdracht om SSH kunt uitvoeren naar de *myVmWeb* virtuele machine van de *myVmMgmt* virtuele machine:

```bash 
ssh azureuser@myVmWeb
```

Om te communiceren met de *myVmMgmt* virtuele machine van de *myVmWeb* virtuele machine, voer de volgende opdracht uit vanaf de opdrachtprompt:

```
ping -c 4 myvmmgmt
```

De uitvoer is vergelijkbaar met de volgende voorbeelduitvoer wordt:
    
```
PING myvmmgmt.hxehizax3z1udjnrx1r4gr30pg.bx.internal.cloudapp.net (10.0.1.4) 56(84) bytes of data.
64 bytes from 10.0.1.4: icmp_seq=1 ttl=64 time=1.45 ms
64 bytes from 10.0.1.4: icmp_seq=2 ttl=64 time=0.628 ms
64 bytes from 10.0.1.4: icmp_seq=3 ttl=64 time=0.529 ms
64 bytes from 10.0.1.4: icmp_seq=4 ttl=64 time=0.674 ms

--- myvmmgmt.hxehizax3z1udjnrx1r4gr30pg.bx.internal.cloudapp.net ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3029ms
rtt min/avg/max/mdev = 0.529/0.821/1.453/0.368 ms
```
      
Kunt u zien dat het adres van de *myVmMgmt* virtuele machine is 10.0.1.4. 10.0.1.4 is het eerste beschikbare IP-adres in het adresbereik van de *persoonlijke* subnet dat u hebt geïmplementeerd de *myVmMgmt* virtuele machine in de vorige stap.  U ziet dat de volledig gekwalificeerde domeinnaam van de virtuele machine *myvmmgmt.hxehizax3z1udjnrx1r4gr30pg.bx.internal.cloudapp.net*. Hoewel de *hxehizax3z1udjnrx1r4gr30pg* gedeelte van de domeinnaam is verschillend voor de virtuele machine, de resterende gedeelten van de domeinnaam zijn hetzelfde. Alle virtuele machines in Azure gebruiken de standaard Azure DNS-service. Alle virtuele machines binnen een virtueel netwerk, kunnen de namen van alle andere virtuele machines in hetzelfde virtuele netwerk met behulp van Azure standaard DNS-service omzetten. In plaats van Azure standaard DNS-service, kunt u uw eigen DNS-server of de mogelijkheid persoonlijke domein van de Azure DNS-service. Zie voor meer informatie [naamomzetting met uw eigen DNS-server](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) of [Azure DNS gebruiken voor persoonlijke domeinen](../dns/private-dns-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Gebruik de volgende opdrachten de nginx-webserver installeren op de *myVmWeb* virtuele machine:

```bash 
# Update package source
sudo apt-get -y update

# Install NGINX
sudo apt-get -y install nginx
```

Nadat de nginx is geïnstalleerd, sluit u de *myVmWeb* SSH-sessie, waarbij u bij de prompt voor de *myVmMgmt* virtuele machine. Voer de volgende opdracht voor het ophalen van het nginx-welkomstscherm van de *myVmWeb* virtuele machine.

```bash
curl myVmWeb
```

Het nginx-welkomstscherm wordt geretourneerd.

Sluit de SSH-sessie met de *myVmMgmt* virtuele machine.

Wanneer Azure gemaakt de *myVmWeb* virtuele machine, een openbare IP-adres met de naam *myVmWebPublicIP* ook is gemaakt en toegewezen aan de virtuele machine. Ophalen van het openbare adres Azure met toegewezen [az netwerk openbare ip-weergeven](/cli/azure/network/public-ip#az_network_public_ip_show).

```azurecli-interactive
az network public-ip show \
  --resource-group myResourceGroup \
  --name myVmWebPublicIP \
  --query ipAddress
```

Vanaf uw eigen computer, typt u de volgende opdracht, vervangen `<publicIpAddress>` met het adres dat wordt geretourneerd door de vorige opdracht:

```bash
curl <publicIpAddress>
```

De poging tot het nginx-welkomstscherm van uw eigen computer curl is mislukt. De poging is mislukt omdat wanneer de virtuele machines zijn geïmplementeerd, Azure standaard een netwerkbeveiligingsgroep voor elke virtuele machine gemaakt. 

Een netwerkbeveiligingsgroep bevat beveiligingsregels voor verbindingen toestaan of weigeren van binnenkomende en uitgaande netwerkverkeer op poort en IP-adres. De standaard netwerkbeveiligingsgroep die Azure gemaakt kan communicatie via alle poorten tussen resources in hetzelfde virtuele netwerk. Voor virtuele Linux-machines, de standaard netwerkbeveiligingsgroep weigert alle binnenkomend verkeer van Internet via alle poorten, TCP-poort 22 (SSH) te accepteren. Als gevolg hiervan standaard kunt u ook maken een SSH-sessie rechtstreeks naar de *myVmWeb* virtuele machine van het Internet, zelfs als u wilt niet poort 22 openen met een webserver. Aangezien de `curl` opdracht communiceert via poort 80, communicatie mislukt, vanaf het Internet omdat er is geen regel in de standaard netwerkbeveiligingsgroep verkeer toestaat via poort 80.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, gebruik [az groep verwijderen](/cli/azure/group#az_group_delete) verwijderen van de resourcegroep en alle resources bevat.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u een virtueel netwerk met meerdere subnetten te implementeren. U hebt ook geleerd wanneer u een virtuele Linux-machine maakt, Azure een netwerkinterface maakt dat wordt gekoppeld aan de virtuele machine en wordt gemaakt van een netwerkbeveiligingsgroep waarmee alleen verkeer via poort 22, vanaf het Internet. Ga naar de volgende zelfstudie voor informatie over het filteren van netwerkverkeer naar subnetten in plaats van afzonderlijke virtuele machines.

> [!div class="nextstepaction"]
> [Filteren van netwerkverkeer naar subnetten](./virtual-networks-create-nsg-arm-cli.md)
