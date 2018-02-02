---
title: Maak een virtueel netwerk - Azure CLI | Microsoft Docs
description: "Snel informatie over het maken van een virtueel netwerk met de Azure CLI. Een virtueel netwerk kan veel soorten Azure-bronnen te privé met elkaar communiceren."
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
ms.date: 01/25/2018
ms.author: jdial
ms.custom: 
ms.openlocfilehash: b2fc8a622549a9858c6c769a7e648fe07a3d01c1
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Een virtueel netwerk maken met de Azure CLI

In dit artikel leert u hoe u een virtueel netwerk maken. Na het maken van een virtueel netwerk, moet u twee virtuele machines implementeren in het virtuele netwerk en privé communiceren tussen deze twee.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstartgids de versie Azure CLI 2.0.4 of hoger uitvoeren. Ga voor de geïnstalleerde versie uitvoeren `az --version`. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az_group_create). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*. Alle Azure-resources worden in een Azure-locatie (of regio) gemaakt.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Maak een virtueel netwerk met de [az network vnet maken](/cli/azure/network/vnet#az_network_vnet_create) opdracht. Het volgende voorbeeld wordt een virtueel netwerk met de naam van standaard *myVirtualNetwork* met één subnet met de naam *standaard*. Omdat een locatie is niet opgegeven, wordt het virtuele netwerk in Azure gemaakt in dezelfde locatie als de resourcegroep.

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

Nadat het virtuele netwerk is gemaakt, is als volgt een deel van de geretourneerde informatie.

```azurecli
"newVNet": {
    "addressSpace": {
      "addressPrefixes": [
        "10.0.0.0/16"
```

Alle virtuele netwerken zijn een of meer adresvoorvoegsels aan hen toegewezen. Omdat een adresvoorvoegsel niet is opgegeven bij het maken van het virtuele netwerk, wordt in Azure de adresruimte 10.0.0.0/16 standaard gedefinieerd. De adresruimte is opgegeven in CIDR-notatie. Het adresruimte 10.0.0.0/16 omvat 10.0.0.0-10.0.255.254.

Een ander deel van de informatie wordt gegeven is de **addressPrefix** van *10.0.0.0/24* voor de *standaard* subnet is opgegeven in de opdracht. Een virtueel netwerk bevat nul of meer subnetten. De opdracht gemaakt één subnet met de naam *standaard*, maar er is geen adresvoorvoegsel opgegeven voor het subnet. Wanneer een adresvoorvoegsel is niet voor een virtueel netwerk of een subnet opgegeven, definieert Azure 10.0.0.0/24 standaard als het adresvoorvoegsel voor het eerste subnet. Als gevolg hiervan wordt het subnet 10.0.0.0-10.0.0.254 omvat, maar alleen 10.0.0.4-10.0.0.254 beschikbaar zijn, omdat Azure reserveert de eerste vier adressen (0-3) en het laatste adres in elk subnet.

## <a name="create-virtual-machines"></a>Virtuele machines maken

Een virtueel netwerk kunt verschillende soorten Azure-bronnen te privé met elkaar communiceren. Een type resource dat u in een virtueel netwerk implementeren kunt is een virtuele machine. Twee virtuele machines maken in het virtuele netwerk, zodat u kunt valideren en inzicht in de werking van communicatie tussen virtuele machines in een virtueel netwerk in een latere stap.

Maak een virtuele machine met de [az vm maken](/cli/azure/vm#az_vm_create) opdracht. Het volgende voorbeeld wordt een virtuele machine met de naam *myVm1*. Als SSH-sleutels niet al bestaan op de standaardlocatie van de sleutel, wordt deze de opdracht maken. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`. De `--no-wait` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

Azure maakt automatisch de virtuele machine in de *standaard* subnet van de *myVirtualNetwork* virtuele netwerk, omdat het virtuele netwerk in de resourcegroep en geen virtuele netwerken bestaat of subnet is opgegeven in de opdracht. Azure DHCP automatisch toegewezen 10.0.0.4 aan de virtuele machine tijdens het maken, omdat dit het eerste beschikbare adres in de *standaard* subnet. De locatie die in een virtuele machine wordt gemaakt, moet het virtuele netwerk bestaat in dezelfde locatie. De virtuele machine is niet in dezelfde resourcegroep bevinden als de virtuele machine is vereist als het in dit artikel.

Maak een tweede virtuele machine. Standaard maakt Azure ook deze virtuele machine in de *standaard* subnet.

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

De virtuele machine duurt een paar minuten maken. Nadat de virtuele machine is gemaakt, retourneert de Azure CLI uitvoer lijkt op het volgende voorbeeld: 

```azurecli 
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVm1",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.5",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

In het voorbeeld ziet u dat de **privateIpAddress** is *10.0.0.5*. Azure DHCP automatisch toegewezen *10.0.0.5* aan de virtuele machine omdat het adres van de volgende beschikbaar in de *standaard* subnet. Noteer de **publicIpAddress**. Dit adres wordt gebruikt voor toegang tot de virtuele machine vanaf het Internet in een later stadium. Het openbare IP-adres is niet toegewezen binnen het virtuele netwerk of subnet adresvoorvoegsels. Openbare IP-adressen worden toegewezen vanuit een [groep adressen toegewezen aan elke Azure-regio](https://www.microsoft.com/download/details.aspx?id=41653). Terwijl Azure welk openbare IP-adres is toegewezen aan een virtuele machine weet, is het besturingssysteem in een virtuele machine geen bewust te maken van een openbaar IP-adres toegewezen.

## <a name="connect-to-a-virtual-machine"></a>Verbinding maken met een virtuele machine

Gebruik de volgende opdracht voor het maken van een SSH-sessie met de *myVm2* virtuele machine. Vervang `<publicIpAddress>` met het openbare IP-adres van uw virtuele machine. In het bovenstaande voorbeeld is het IP-adres is *40.68.254.142*.

```bash 
ssh <publicIpAddress>
```

## <a name="validate-communication"></a>Communicatie valideren

Gebruik de volgende opdracht om te bevestigen dat communicatie met *myVm1* van *myVm2*:

```bash
ping myVm1 -c 4
```

U ontvangt vier reacties van *10.0.0.4*. U kunt contact opnemen met *myVm1* van *myVm2*omdat beide virtuele machines hebben privé IP-adressen toegewezen vanuit de *standaard* subnet. U zijn kunt pingen op hostnaam, omdat de DNS-naamomzetting in Azure automatisch worden verstrekt voor alle hosts binnen een virtueel netwerk.

Gebruik de volgende opdracht om te bevestigen uitgaande communicatie met Internet:

```bash
ping bing.com -c 4
```

U ontvangt vier reacties van bing.com. Standaard kan een virtuele machine in een virtueel netwerk uitgaand naar het Internet communiceren.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u de [az groep verwijderen](/cli/azure/group#az_group_delete) opdracht om te verwijderen van de resourcegroep en alle resources bevat. De SSH-sessie met uw virtuele machine afsluiten en verwijder vervolgens de bronnen.

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel als u een standaard virtuele netwerk met één subnet en twee virtuele machines geïmplementeerd. Als u wilt weten hoe u een aangepaste virtueel netwerk maken met meerdere subnetten en algemene beheertaken uitvoeren, blijven de zelfstudie voor het maken van een aangepaste virtueel netwerk en van het beheer.


> [!div class="nextstepaction"]
> [Een aangepaste virtueel netwerk maken en deze te beheren](virtual-networks-create-vnet-arm-pportal.md#azure-cli)
