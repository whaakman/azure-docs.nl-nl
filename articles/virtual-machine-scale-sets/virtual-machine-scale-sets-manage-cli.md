---
title: Beheren van virtuele-Machineschaalsets met de Azure CLI 2.0 | Microsoft Docs
description: Algemene Azure CLI 2.0-opdrachten voor het beheren van virtuele-Machineschaalsets, zoals het starten en stoppen van een exemplaar of de schaal wijzigen instellen capaciteit.
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: iainfou
ms.openlocfilehash: 6ae05dc8faf950f584806d9b4a3e7e1466ded652
ms.sourcegitcommit: 4256ebfe683b08fedd1a63937328931a5d35b157
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/23/2017
---
# <a name="manage-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Een virtuele-machineschaalset ingesteld met de Azure CLI 2.0 beheren
Gedurende de levenscyclus van een virtuele-machineschaalset, moet u wellicht een of meer beheertaken uitvoeren. Bovendien wilt u scripts maken die verschillende lifecycle-taken automatiseren. In dit artikel vindt u details van de algemene 2.0 voor Azure CLI-opdrachten die u kunnen deze taken uitvoeren.

Als u wilt deze beheertaken hebt voltooid, moet u de laatste build van Azure CLI 2.0. Zie voor meer informatie over het installeren en gebruiken van de meest recente versie [2.0 Azure CLI installeren](/cli/azure/install-azure-cli). Als u maken van een virtuele-machineschaalset wilt, kunt u [een schaal ingesteld in de Azure portal maken](virtual-machine-scale-sets-create-portal.md).


## <a name="view-information-about-a-scale-set"></a>Informatie over een schaalset weergeven
Gebruiken om de algemene informatie weergeven over een schaalset, [az vmss weergeven](/cli/azure/vmss#show). Het volgende voorbeeld wordt informatie over de set met de naam scale *myScaleSet* in de *myResourceGroup* resourcegroep. Voer uw eigen namen:

```azurecli
az vmss show --resource-group myResourceGroup --name myScaleSet
```


## <a name="view-vms-in-a-scale-set"></a>Weergave virtuele machines in een schaalset
Een overzicht van VM-instantie in een schaalset gebruiken [az vmss lijstexemplaren](/cli/azure/vmss#list-instances). Het volgende voorbeeld lijst met alle VM-exemplaren in de set met de naam scale *myScaleSet* in de *myResourceGroup* resourcegroep. Uw eigen waarden opgeven voor deze namen:

```azurecli
az vmss list-instances \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --output table
```

Om weer te geven als u meer informatie over een specifiek exemplaar van de VM, voeg de `--instance-id` -parameter voor [az vmss get--instantieweergave](/cli/azure/vmss#get-instance-view) en geeft u een exemplaar om weer te geven. Informatie over de VM-instantie voor het volgende voorbeeld-weergaven *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Voer uw eigen namen:

```azurecli
az vmss get-instance-view \
  --resource-group myResourceGroup \
  --name myScaleSet \
  --instance-id 0
```


## <a name="list-connection-information-for-vms"></a>Lijst van verbindingsgegevens voor virtuele machines
Verbinding maken met de virtuele machines in een schaalset, u SSH of RDP naar een toegewezen openbare IP-adres en poort nummer. Standaard worden regels voor Network Address Translation (NAT) toegevoegd aan de Azure load balancer die verbinding met extern verkeer naar elke virtuele machine doorstuurt. U kunt het adres en de poorten verbinding maken met de VM-exemplaren in een schaalset gebruiken [az vmss lijst--verbinding-Instantiegegevens](/cli/azure/vmss#list-instance-connection-info). De volgende voorbeeld lijst verbindingsgegevens voor de virtuele machine-exemplaren in de set met de naam scale *myScaleSet* en in de *myResourceGroup* resourcegroep. Uw eigen waarden opgeven voor deze namen:

```azurecli
az vmss list-instance-connection-info \
  --resource-group myResourceGroup \
  --name myScaleSet
```


## <a name="change-the-capacity-of-a-scale-set"></a>De capaciteit van een schaalset wijzigen
De voorgaande opdrachten hebt u geleerd informatie over uw schaalset en de VM-exemplaren. Als u wilt vergroten of verkleinen het aantal exemplaren in de schaalset, kunt u de capaciteit. De schaalaanpassingsset maakt of verwijdert u het vereiste aantal virtuele machines en configureert u de virtuele machines voor het ontvangen verkeer van de toepassing.

Als het aantal exemplaren dat u momenteel in een schaalset hebt wilt weergeven, gebruikt [az vmss weergeven](/cli/azure/vmss#show) en query's uitvoeren op *sku.capacity*:

```azurecli
az vmss show \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

U kunt vervolgens handmatig vergroten of verkleinen het aantal virtuele machines in de schaal in te stellen [az vmss scale](/cli/azure/vmss#scale). Het volgende voorbeeld wordt het aantal VM's in uw ingesteld op schaal *5*:

```azurecli
az vmss scale \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --new-capacity 5
```

Als duurt een paar minuten bijwerken van de capaciteit van de schaal is ingesteld. Als u verlaagt de capaciteit van een schaal ingesteld, wordt de virtuele machines met de hoogste exemplaar id's eerst worden verwijderd.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Stoppen en starten van virtuele machines in een schaalset
Voor het beëindigen van een of meer virtuele machines in een schaalset gebruikt [az vmss stop](/cli/azure/vmss/stop). De `--instance-ids` parameter kunt u een of meer virtuele machines stoppen opgeven. Als u een exemplaar-ID niet opgeeft, worden alle virtuele machines in de schaalset gestopt. Als u wilt meerdere virtuele machines stoppen, scheidt u elke instantie-ID met een spatie.

Het volgende voorbeeld gestopt exemplaar *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef de waarden als volgt:

```azurecli
az vmss stop --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```

Gestopte virtuele machines blijven toegewezen en nog steeds compute-kosten. Als u wilt in plaats daarvan de VM's ongedaan en opslag alleen kosten, gebruik [az vmss ongedaan](/cli/azure/vmss#deallocate). Als u wilt meerdere VM's ongedaan, scheidt u elke instantie-ID met een spatie. Het volgende voorbeeld wordt gestopt en exemplaar deallocates *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef de waarden als volgt:

```azurecli
az vmss deallocate --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


### <a name="start-vms-in-a-scale-set"></a>Starten van virtuele machines in een schaalset
Gebruik voor het starten van een of meer virtuele machines in een schaalset, [az vmss start](/cli/azure/vmss#start). De `--instance-ids` parameter kunt u opgeven van een of meer virtuele machines worden gestart. Als u een exemplaar-ID niet opgeeft, worden alle virtuele machines in de schaalset gestart. Als u wilt meerdere virtuele machines, scheidt u elke instantie-ID met een spatie.

Het volgende voorbeeld wordt een exemplaar gestart *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef de waarden als volgt:

```azurecli
az vmss start --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="restart-vms-in-a-scale-set"></a>Opnieuw opstarten van virtuele machines in een schaalset
Als een of meer virtuele machines in een schaalset opnieuw, wilt u [az vmss opnieuw](/cli/azure/vmss#restart). De `--instance-ids` parameter kunt u een of meer virtuele machines opnieuw opstarten opgeven. Als u een exemplaar-ID niet opgeeft, worden alle virtuele machines in de schaalset opnieuw gestart. Om meerdere virtuele machines starten, scheidt u elke instantie-ID met een spatie.

Het volgende voorbeeld wordt opnieuw opgestart exemplaar *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef de waarden als volgt:

```azurecli
az vmss restart --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="remove-vms-from-a-scale-set"></a>Virtuele machines uit een set scale verwijderen
Gebruik te verwijderen van een of meer virtuele machines in een set scale [az vmss delete-exemplaren](/cli/azure/vmss#delete-instances). De `--instance-ids` parameter kunt u opgeven van een of meer VM's te verwijderen. Als u opgeeft * voor het exemplaar-ID, alle virtuele machines in de schaalset worden verwijderd. Als u wilt verwijderen van meerdere virtuele machines, scheidt u elke instantie-ID met een spatie.

Het volgende voorbeeld verwijdert exemplaar *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef de waarden als volgt:

```azurecli
az vmss delete-instances --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="next-steps"></a>Volgende stappen
Andere algemene taken voor schaalsets omvatten het [implementeren van een toepassing](virtual-machine-scale-sets-deploy-app.md), en [upgrade VM-instanties](virtual-machine-scale-sets-upgrade-scale-set.md). U kunt ook Azure CLI voor [regels voor automatisch schalen configureren](virtual-machine-scale-sets-autoscale-overview.md).
