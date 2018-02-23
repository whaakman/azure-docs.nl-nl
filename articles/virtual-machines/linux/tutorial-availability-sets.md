---
title: "Zelfstudie over beschikbaarheidssets voor Linux-VM’s in Azure | Microsoft Docs"
description: Meer informatie over de beschikbaarheidssets voor virtuele Linux-machines in Azure.
documentationcenter: 
services: virtual-machines-linux
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: tutorial
ms.date: 10/05/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 504c4a666d1abd7a495d6759d62815f53f0b54fa
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2018
---
# <a name="how-to-use-availability-sets"></a>Beschikbaarheidssets gebruiken


In deze zelfstudie leert u hoe u de beschikbaarheid en betrouwbaarheid van uw Virtual Machine-oplossingen op Azure kunt verhogen met behulp van beschikbaarheidssets. Beschikbaarheidssets zorgen ervoor dat de VM's die u op Azure implementeert, verdeeld worden over meerdere geïsoleerde hardwareclusters. Dit zorgt ervoor dat als er zich binnen Azure een hardware- of softwarestoring voordoet, er slechts een subset van uw VM's wordt beïnvloed en dat uw totale oplossing beschikbaar en operationeel blijft.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een beschikbaarheidsset maken
> * Een VM maken in een beschikbaarheidsset
> * Beschikbare VM-grootten controleren


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie Azure CLI 2.0.4 of nieuwer uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="availability-set-overview"></a>Overzicht beschikbaarheidsset

Een beschikbaarheidsset is een logische groeperingsmogelijkheid die u in Azure kunt gebruiken om ervoor te zorgen dat de VM-resources die u erin plaatst, van elkaar worden geïsoleerd wanneer ze in een Azure-datacenter worden geïmplementeerd. Azure zorgt ervoor dat de VM's die u in een beschikbaarheidsset plaatst, op meerdere fysieke servers, rekenrekken, opslageenheden en netwerkswitches worden uitgevoerd. Als er zich een hardware- of softwarestoring in Azure voordoet, wordt slechts een subset van uw VM's getroffen en blijft uw totale toepassing actief en beschikbaar voor uw klanten. Beschikbaarheidssets zijn essentieel wanneer u betrouwbare cloudoplossingen wilt bouwen.

Laten we eens kijken naar een typische VM-oplossing met vier front-end webservers en twee back-end VM's die een database hosten. Met Azure wilt u twee beschikbaarheidssets definiëren voordat u uw VM's implementeert: een beschikbaarheidsset voor de ‘web’-laag en een beschikbaarheidsset voor de ‘database’-laag. Bij het maken van een nieuwe VM kunt u vervolgens de beschikbaarheidsset opgeven als parameter voor de opdracht az vm create, en zorgt Azure er automatisch voor dat de VM's die u binnen de beschikbare set maakt, worden geïsoleerd over meerdere fysieke hardwareresources. Als er een probleem is met de fysieke hardware waarop een van uw webserver- of databaseserver-VM's draait, weet u dat de andere instanties van uw webserver en database-VM's actief blijven, omdat ze worden uitgevoerd op andere hardware.

Gebruik beschikbaarheidssets wanneer u betrouwbare VM-oplossingen wilt implementeren in Azure.


## <a name="create-an-availability-set"></a>Een beschikbaarheidsset maken

U kunt een beschikbaarheidsset maken met behulp van [az vm availability-set create](/cli/azure/vm/availability-set#az_vm_availability_set_create). In dit voorbeeld stellen we het aantal update- en foutdomeinen in op *2* voor de beschikbaarheidsset met de naam *myAvailabilitySet* in de resourcegroep *ResourceGroupAvailability*.

Maak een resourcegroep.

```azurecli-interactive 
az group create --name myResourceGroupAvailability --location eastus
```


```azurecli-interactive 
az vm availability-set create \
    --resource-group myResourceGroupAvailability \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

Met beschikbaarheidssets kunt u bronnen isoleren tussen foutdomeinen en domeinen bijwerken. Een **foutdomein** vertegenwoordigt een geïsoleerde verzameling van server- plus netwerk- en opslagresources. In het voorgaande voorbeeld geven we aan dat we willen dat onze beschikbaarheidsset wordt verdeeld over minstens twee foutdomeinen wanneer onze VM's worden geïmplementeerd. We geven ook aan dat we onze beschikbaarheidsset willen verdelen over twee **updatedomeinen**.  Twee update-domeinen zorgen ervoor dat wanneer er Azure software-updates worden uitgevoerd, onze VM-resources geïsoleerd worden, zodat wordt voorkomen dat alle software onder onze VM tegelijkertijd wordt bijgewerkt.


## <a name="create-vms-inside-an-availability-set"></a>VM's maken in een beschikbaarheidsset

VM's moeten worden gemaakt binnen de beschikbaarheidsset om ervoor te zorgen dat ze correct over de hardware worden verdeeld. U kunt geen bestaande VM toevoegen aan een beschikbaarheidsset nadat deze is gemaakt. 

Wanneer u een VM maakt met behulp van [az vm create](/cli/azure/vm#az_vm_create), geeft u de naam van de beschikbaarheidsset op met behulp van de parameter `--availability-set`.

```azurecli-interactive 
for i in `seq 1 2`; do
   az vm create \
     --resource-group myResourceGroupAvailability \
     --name myVM$i \
     --availability-set myAvailabilitySet \
     --size Standard_DS1_v2  \
     --image Canonical:UbuntuServer:14.04.4-LTS:latest \
     --admin-username azureuser \
     --generate-ssh-keys \
     --no-wait
done 
```

Nu hebben we twee virtuele machines in onze nieuwe beschikbaarheidsset. Omdat ze zich in dezelfde beschikbaarheidsset bevinden, zorgt Azure ervoor dat de virtuele machines en alle bijbehorende resources (inclusief gegevensschijven) worden gedistribueerd over geïsoleerde fysieke hardware. Deze verdeling helpt zorgen voor een veel hogere beschikbaarheid van de algehele VM-oplossing.

Als u de beschikbaarheidsset in de portal bekijkt door naar Resource Groups > myResourceGroupAvailability > myAvailabilitySet te gaan, ziet u hoe de VM's zijn verdeeld over de twee fout- en updatedomeinen.

![Beschikbaarheidsset in de portal](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Beschikbare VM-grootten controleren 

U kunt later meer VM's toevoegen aan de beschikbaarheidsset, maar u moet weten welke VM-grootten beschikbaar zijn op de hardware.  Gebruik [az vm availability-set list-sizes](/cli/azure/availability-set#az_availability_set_list_sizes) om een lijst weer te geven met alle beschikbare grootten voor de beschikbaarheidsset op de hardwarecluster.

```azurecli-interactive 
az vm availability-set list-sizes \
     --resource-group myResourceGroupAvailability \
     --name myAvailabilitySet \
     --output table  
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een beschikbaarheidsset maken
> * Een VM maken in een beschikbaarheidsset
> * Beschikbare VM-grootten controleren

Ga naar de volgende zelfstudie voor meer informatie over virtuele-machineschaalsets.

> [!div class="nextstepaction"]
> [Een virtuele-machineschaalset maken](tutorial-create-vmss.md)

