---
title: 'Een virtueel netwerk maken: snelstart - Azure CLI | Microsoft Docs'
description: In deze snelstart leert u hoe u een virtueel netwerk maakt met het Azure Portal. Met een virtueel netwerk kunnen Azure-resources, zoals virtuele machines, privé met elkaar en met internet communiceren.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
Customer intent: I want to create a virtual network so that virtual machines can communicate with privately with each other and with the internet.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: quickstart
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/09/2018
ms.author: jdial
ms.custom: mvc
ms.openlocfilehash: 2290d2975326c35ec4ec18a77a181a4ad0a205b2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46962790"
---
# <a name="quickstart-create-a-virtual-network-using-the-azure-cli"></a>Snelstart: een virtueel netwerk maken met Azure Portal

Met een virtueel netwerk kunnen Azure-resources, zoals virtuele machines (VM), privé met elkaar en met internet communiceren. In deze snelstart leert u hoe u een virtueel netwerk maakt. Nadat u een virtueel netwerk hebt gemaakt, implementeert u twee virtuele machines in het virtuele netwerk. Vervolgens maakt u verbinding met één virtuele machine via internet en is er privécommunicatie tussen de twee virtuele machines mogelijk.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze snelstart de Azure CLI versie 2.0.28 of later uitvoeren. Voer `az --version` uit om na te gaan welke versie er is geïnstalleerd. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 


## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Voordat u een virtueel netwerk kunt maken, moet u een resourcegroep maken die het virtuele netwerk bevat. Maak een resourcegroep maken met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak een virtueel netwerk met [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). In het volgende voorbeeld wordt een standaard virtueel netwerk gemaakt met de naam *myVirtualNetwork* op de locatie *default*:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

## <a name="create-virtual-machines"></a>Virtuele machines maken

Twee virtuele machines in het virtuele netwerk maken:

### <a name="create-the-first-vm"></a>De eerste VM maken

Maak een VM met [az vm create](/cli/azure/vm#az_vm_create). Als SSH-sleutels niet al bestaan op de standaardlocatie van de sleutel, worden ze met deze opdracht gemaakt. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`. Met de optie `--no-wait` wordt de virtuele machine op de achtergrond gemaakt, zodat u kunt doorgaan met de volgende stap. In het volgende voorbeeld wordt een VM met de naam *myVM1* gemaakt:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>De tweede VM maken

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm2 \
  --image UbuntuLTS \
  --generate-ssh-keys
```

Het maken van de virtuele machine duurt een paar minuten. Nadat de virtuele machine is gemaakt, geeft de Azure CLI uitvoer die lijkt op het volgende voorbeeld: 

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

Let op het **openbare IP-adres**. Dit adres wordt in de volgende stap gebruikt om verbinding te maken met de virtuele machine via internet.

## <a name="connect-to-a-vm-from-the-internet"></a>Verbinding maken met een virtuele machine via internet

Vervang `<publicIpAddress>` met het openbare IP-adres van uw virtuele machine *myVm2* en voer de volgende opdracht in:

```bash 
ssh <publicIpAddress>
```

## <a name="communicate-between-vms"></a>Communiceren tussen VM's

Voer de volgende opdracht in om privécommunicatie te bevestigen tussen de virtuele machines *myVm2* en *myVm1*:

```bash
ping myVm1 -c 4
```

U ontvangt vier reacties van *10.0.0.4*.

Sluit de SSH-sessie met de virtuele machine *myVm2* af.

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete](/cli/azure/group#az_group_delete) gebruiken om de resourcegroep en alle resources die deze bevat te verwijderen, als deze niet meer nodig zijn:

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een standaard virtueel netwerk en twee virtuele machines gemaakt. U hebt met één virtuele machine verbinding gemaakt via internet en er is er privécommunicatie tussen de twee virtuele machines geweest. Zie [Een virtueel netwerk beheren](manage-virtual-network.md) voor meer informatie over instellingen voor virtuele netwerken. 

Standaard staat Azure onbeperkte privécommunicatie tussen virtuele machines toe, maar staat alleen binnenkomende externe bureaubladverbindingen met Windows VM's via internet toe. Voor informatie over het toestaan of beperken van verschillende soorten netwerkcommunicatie naar en van virtuele machines gaat u verder met de zelfstudie [Netwerkverkeer filteren](tutorial-filter-network-traffic.md).
