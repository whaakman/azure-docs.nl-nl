---
title: Beschikbaarheidssets zelfstudie voor virtuele Linux-machines in Azure | Microsoft Docs
description: Meer informatie over de Beschikbaarheidssets voor virtuele Linux-machines in Azure.
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
ms.openlocfilehash: e7780a29f6633b444608d96012fabe67b9b6d924
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-availability-sets"></a>Het gebruik van beschikbaarheidssets


In deze zelfstudie leert u hoe verhoogt de beschikbaarheid en betrouwbaarheid van uw virtuele Machine-oplossingen in Azure met een mogelijkheid Beschikbaarheidssets aangeroepen. Beschikbaarheidssets Zorg ervoor dat de virtuele machines die u implementeert in Azure worden gedistribueerd over meerdere geïsoleerde hardware-clusters. Dit zorgt ervoor dat slechts een subset van uw virtuele machines worden beïnvloed als er een storing hardware of software in Azure gebeurt, doen en dat de algehele oplossing beschikbaar is en operationele blijft.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een beschikbaarheidsset maken
> * Een virtuele machine in een beschikbaarheidsset maken
> * Controleer de beschikbare grootten voor virtuele machine


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="availability-set-overview"></a>Overzicht van de beschikbaarheidsset

Een Beschikbaarheidsset is een logische groepering-functie die u in Azure gebruiken kunt om ervoor te zorgen dat de VM-resources die u in het plaatsen van elkaar geïsoleerd zijn wanneer ze zijn geïmplementeerd in een Azure-datacenter. Azure zorgt ervoor dat de virtuele machines die u binnen een Beschikbaarheidsset uitvoeren op meerdere fysieke servers plaatst, compute rekken eenheden voor opslag en netwerkswitches. Als er een hardware- of Azure software-fout optreedt, alleen een subset van uw virtuele machines worden beïnvloed en uw algehele toepassing blijft en blijft beschikbaar voor uw klanten. Beschikbaarheidssets zijn een essentieel mogelijkheid bij om betrouwbare cloudoplossingen te bouwen.

Laten we eens een typische VM-oplossing op basis van waar u mogelijk 4 front-end-webservers en 2 back-end virtuele machines die een database te hosten. Met Azure twee beschikbaarheidssets definiëren voordat u uw virtuele machines implementeert u zou willen: één beschikbaarheidsset voor de laag 'web' en een beschikbaarheidsset voor de laag 'database'. Bij het maken van een nieuwe virtuele machine vervolgens u kunt de beschikbaarheidsset als een parameter voor de vm az opdracht maken en Azure automatisch zorgt ervoor dat de virtuele machines die u maakt in zijn over meerdere fysieke hardwareresources de beschikbare set geïsoleerd. Als de fysieke hardware die een van uw webserver of VM's Database-Server wordt uitgevoerd op een probleem is, weet u dat de andere exemplaren van uw webserver en de Database virtuele machines actief blijven, omdat ze op andere hardware.

Gebruik Beschikbaarheidssets wanneer u wilt implementeren, betrouwbare oplossingen op basis van virtuele machine in Azure.


## <a name="create-an-availability-set"></a>Een beschikbaarheidsset maken

Kunt u een beschikbaarheidsset met [az vm beschikbaarheidsset maken](/cli/azure/vm/availability-set#create). In dit voorbeeld wordt zowel het aantal update en fouttolerantie domeinen op ingesteld *2* voor de beschikbaarheid van de set met de naam *myAvailabilitySet* in de *myResourceGroupAvailability* resourcegroep.

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

Beschikbaarheidssets kunnen u voor het isoleren van bronnen tussen domeinen met fouten en domeinen bijwerken. Een **foutdomein** vertegenwoordigt een geïsoleerd verzameling server + netwerk en opslag resources. In het voorgaande voorbeeld geven we willen we onze beschikbaarheidsset verdeeld over ten minste twee domeinen met fouten als onze virtuele machines worden geïmplementeerd. We ook aangeven dat we onze beschikbaarheidsset gedistribueerde in twee **domeinen bijwerken**.  Twee update domeinen Zorg ervoor dat wanneer Azure voert de software-updates onze VM-netwerkbronnen geïsoleerd, waardoor alle software die worden uitgevoerd onder de virtuele machine tegelijk worden bijgewerkt.


## <a name="create-vms-inside-an-availability-set"></a>Virtuele machines in een beschikbaarheidsset maken

Virtuele machines moeten worden gemaakt binnen de beschikbaarheidsset om ervoor te zorgen dat ze correct zijn verdeeld over de hardware. U kunt een bestaande virtuele machine toevoegen aan een beschikbaarheidsset nadat deze is gemaakt. 

Wanneer u een virtuele machine met maakt [az vm maken](/cli/azure/vm#create) opgeven van de beschikbaarheidsset met behulp van de `--availability-set` parameter om de naam van de beschikbaarheidsset.

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

Nu hebben we twee virtuele machines in onze nieuwe beschikbaarheidsset. Omdat ze zich in dezelfde beschikbaarheidsset, Azure zorgt ervoor dat de virtuele machines en alle bijbehorende resources (inclusief gegevensschijven) worden gedistribueerd over geïsoleerde fysieke hardware. Deze verdeling kan ervoor zorgen veel hogere beschikbaarheid van de algehele oplossing voor de VM.

Als u de beschikbaarheidsset in de portal door te gaan aan resourcegroepen bekijkt > myResourceGroupAvailability > myAvailabilitySet, moet u zien hoe de virtuele machines zijn verdeeld over de fout met de 2 en domeinen bijwerken.

![Beschikbaarheidsset voor de portal](./media/tutorial-availability-sets/fd-ud.png)

## <a name="check-for-available-vm-sizes"></a>Controleren op beschikbare VM-grootten 

U kunt meer virtuele machines toevoegen aan de beschikbaarheid later instellen, maar u moet weten welke VM-grootten zijn beschikbaar op de hardware.  Gebruik [az vm beschikbaarheidsset lijst met grootten](/cli/azure/availability-set#list-sizes) voor een lijst met alle van de beschikbare grootten van de hardware-cluster gebruikt voor de beschikbaarheidsset.

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
> * Een virtuele machine in een beschikbaarheidsset maken
> * Controleer de beschikbare grootten voor virtuele machine

Ga naar de volgende zelfstudie voor meer informatie over virtuele-machineschaalsets.

> [!div class="nextstepaction"]
> [Een VM-schaalset maken](tutorial-create-vmss.md)

