---
title: Maken van een virtuele Azure-netwerk - Azure CLI | Microsoft Docs
description: Snel informatie over het maken van een virtueel netwerk met de Azure CLI. Een virtueel netwerk kunt Azure-resources, zoals virtuele machines, privé communiceren met elkaar en met het internet.
services: virtual-network
documentationcenter: virtual-network
author: jimdial
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: azurecli
ms.topic: ''
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/09/2018
ms.author: jdial
ms.custom: ''
ms.openlocfilehash: d07f06a1a70c859544c3b1ceb6146dc11e4d10aa
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="create-a-virtual-network-using-the-azure-cli"></a>Een virtueel netwerk maken met de Azure CLI

Azure-resources, zoals virtuele machines (VM), kunt een virtueel netwerk privé communiceren met elkaar en met het Internet. In dit artikel leert u hoe u een virtueel netwerk maken. Na het maken van een virtueel netwerk, kunt u twee virtuele machines in het virtuele netwerk implementeert. U vervolgens verbinding maken met één virtuele machine vanaf het internet en privé communiceren met de andere virtuele machine.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in dit artikel is vereist dat u de Azure CLI versie 2.0.28 worden uitgevoerd of hoger. Ga voor de geïnstalleerde versie uitvoeren `az --version`. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). 


## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken

Voordat u een virtueel netwerk maken kunt, moet u een resourcegroep om het virtuele netwerk maken. Maak een resourcegroep maken met [az group create](/cli/azure/group#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Maak een virtueel netwerk met [az network vnet create](/cli/azure/network/vnet#az_network_vnet_create). Het volgende voorbeeld wordt een virtueel netwerk met de naam van standaard *myVirtualNetwork* met één subnet met de naam *standaard*:

```azurecli-interactive 
az network vnet create \
  --name myVirtualNetwork \
  --resource-group myResourceGroup \
  --subnet-name default
```

## <a name="create-virtual-machines"></a>Virtuele machines maken

Maak twee virtuele machines in het virtuele netwerk:

### <a name="create-the-first-vm"></a>De eerste virtuele machine maken

Maak een VM met [az vm create](/cli/azure/vm#az_vm_create). Als SSH-sleutels niet al bestaan op de standaardlocatie van de sleutel, wordt deze de opdracht maken. Als u een specifieke set sleutels wilt gebruiken, gebruikt u de optie `--ssh-key-value`. De `--no-wait` optie maakt de virtuele machine op de achtergrond, zodat u kunt doorgaan met de volgende stap. Het volgende voorbeeld wordt een virtuele machine met de naam *myVm1*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name myVm1 \
  --image UbuntuLTS \
  --generate-ssh-keys \
  --no-wait
```

### <a name="create-the-second-vm"></a>De tweede virtuele machine maken

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

Noteer de **publicIpAddress**. Dit adres wordt gebruikt voor het verbinding maken met de virtuele machine vanaf het Internet in de volgende stap.

## <a name="connect-to-a-vm-from-the-internet"></a>Verbinding maken met een virtuele machine vanaf het internet

Vervang `<publicIpAddress>` met het openbare IP-adres van uw *myVm2* VM in de opdracht de gevolgd en voer de volgende opdracht:

```bash 
ssh <publicIpAddress>
```

## <a name="communicate-privately-between-vms"></a>Privé communiceren tussen VM 's

Om te bevestigen dat persoonlijke communicatie tussen de *myVm2* en *myVm1* virtuele machines, voer de volgende opdracht:

```bash
ping myVm1 -c 4
```

U ontvangt vier reacties van *10.0.0.4*.

Afsluiten van de SSH-sessie met de *myVm2* VM.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u [az groep verwijderen](/cli/azure/group#az_group_delete) verwijderen van de resourcegroep en alle resources die deze bevat:

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel leert u gemaakt een standaard virtueel netwerk en twee virtuele machines. U verbonden met één virtuele machine vanaf het Internet en privé uitgewisseld tussen de virtuele machine en een andere virtuele machine. Zie voor meer informatie over virtuele-netwerkinstellingen, [beheren van een virtueel netwerk](manage-virtual-network.md). 

Standaard Azure onbeperkte persoonlijke communicatie tussen virtuele machines, maar kunt alleen binnenkomende SSH-sessies voor Linux VM's van het Internet. Voor informatie over het toestaan of beperken van verschillende soorten netwerkcommunicatie naar en van virtuele machines, Ga naar de volgende zelfstudie.

> [!div class="nextstepaction"]
> [Filteren van netwerkverkeer](tutorial-filter-network-traffic-cli.md)
