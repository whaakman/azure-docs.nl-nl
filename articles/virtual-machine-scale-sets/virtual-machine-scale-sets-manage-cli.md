---
title: Beheren van virtuele-Machineschaalsets met de Azure CLI 2.0 | Microsoft Docs
description: Algemene Azure CLI 2.0-opdrachten voor het beheren van virtuele-Machineschaalsets, zoals het starten en stoppen van een exemplaar of de schaal wijzigen instellen capaciteit.
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: iainfou
ms.openlocfilehash: ca447f3ca0ed6656912a0d3e5082ebd2dd308a14
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652481"
---
# <a name="manage-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Een virtuele-machineschaalset ingesteld met de Azure CLI 2.0 beheren
Gedurende de levenscyclus van een schaalset voor virtuele machines moet u mogelijk een of meer beheertaken uitvoeren. Bovendien wilt u misschien scripts maken die verschillende levenscyclustaken automatiseren. In dit artikel vindt u details van de algemene 2.0 voor Azure CLI-opdrachten die u kunnen deze taken uitvoeren.

Als u wilt deze beheertaken hebt voltooid, moet u de nieuwste Azure CLI 2.0. Zie voor informatie [2.0 Azure CLI installeren](/cli/azure/install-azure-cli). Als u maken van een virtuele-machineschaalset wilt, kunt u [maken van een schaal ingesteld met de Azure CLI 2.0](quick-create-cli.md).


## <a name="view-information-about-a-scale-set"></a>Informatie over een schaalset weergeven
Gebruiken om de algemene informatie weergeven over een schaalset, [az vmss weergeven](/cli/azure/vmss#az_vmss_show). Het volgende voorbeeld wordt informatie over de set met de naam scale *myScaleSet* in de *myResourceGroup* resourcegroep. Voer uw eigen namen:

```azurecli
az vmss show --resource-group myResourceGroup --name myScaleSet
```


## <a name="view-vms-in-a-scale-set"></a>Virtuele machines weergeven in een schaalset
Een overzicht van VM-instantie in een schaalset gebruiken [az vmss lijstexemplaren](/cli/azure/vmss#list-instances). Het volgende voorbeeld worden alle VM-exemplaren in de set met de naam scale *myScaleSet* in de *myResourceGroup* resourcegroep. Uw eigen waarden opgeven voor deze namen:

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
Verbinding maken met de virtuele machines in een schaalset, u SSH of RDP naar een toegewezen openbare IP-adres en poort nummer. Standaard worden network address translation (NAT) regels toegevoegd aan de Azure load balancer die verbinding met extern verkeer naar elke virtuele machine doorstuurt. U kunt het adres en de poorten verbinding maken met de VM-exemplaren in een schaalset gebruiken [az vmss lijst--verbinding-Instantiegegevens](/cli/azure/vmss#list-instance-connection-info). Het volgende voorbeeld worden gegevens van de verbinding voor VM-exemplaren in de set met de naam scale *myScaleSet* en in de *myResourceGroup* resourcegroep. Uw eigen waarden opgeven voor deze namen:

```azurecli
az vmss list-instance-connection-info \
    --resource-group myResourceGroup \
    --name myScaleSet
```


## <a name="change-the-capacity-of-a-scale-set"></a>De capaciteit van een schaalset wijzigen
De voorgaande opdrachten hebt u geleerd informatie over uw schaalset en de VM-exemplaren. Als u wilt vergroten of verkleinen het aantal exemplaren in de schaalset, kunt u de capaciteit. De schaalaanpassingsset maakt of verwijdert u het vereiste aantal virtuele machines en configureert u de virtuele machines voor het ontvangen verkeer van de toepassing.

Als u het aantal instanties wilt weergeven dat zich momenteel in een schaalset bevindt, gebruikt u [az vmss show](/cli/azure/vmss#az_vmss_show) en voert u een query uit op *sku.capacity*:

```azurecli
az vmss show \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

U kunt vervolgens het aantal virtuele machines in de schaalset handmatig vergroten of verkleinen met [az vmss scale](/cli/azure/vmss#az_vmss_scale). Het volgende voorbeeld wordt het aantal VM's in uw ingesteld op schaal *5*:

```azurecli
az vmss scale \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --new-capacity 5
```

Het duurt een paar minuten om de capaciteit van de schaalset bij te werken. Als u verlaagt de capaciteit van een schaal ingesteld, wordt de virtuele machines met de hoogste exemplaar id's eerst worden verwijderd.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Stoppen en starten van virtuele machines in een schaalset
Voor het beëindigen van een of meer virtuele machines in een schaalset gebruikt [az vmss stop](/cli/azure/vmss/stop). Met de parameter `--instance-ids` kunt u een of meer VM's opgeven om te stoppen. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset gestopt. Als u wilt meerdere virtuele machines stoppen, scheidt u elke instantie-ID met een spatie.

Het volgende voorbeeld gestopt exemplaar *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```azurecli
az vmss stop --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```

Gestopte virtuele machines blijven toegewezen en nog steeds compute-kosten. Als u wilt in plaats daarvan de VM's ongedaan en opslag alleen kosten, gebruik [az vmss ongedaan](/cli/azure/vmss#az_vmss_deallocate). Als u wilt meerdere VM's ongedaan, scheidt u elke instantie-ID met een spatie. Het volgende voorbeeld wordt gestopt en exemplaar deallocates *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```azurecli
az vmss deallocate --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


### <a name="start-vms-in-a-scale-set"></a>Starten van virtuele machines in een schaalset
Gebruik voor het starten van een of meer virtuele machines in een schaalset, [az vmss start](/cli/azure/vmss#az_vmss_start). Met de parameter `--instance-ids` kunt u een of meer VM's opgeven om te starten. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset gestart. Als u wilt meerdere virtuele machines, scheidt u elke instantie-ID met een spatie.

Het volgende voorbeeld wordt een exemplaar gestart *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```azurecli
az vmss start --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="restart-vms-in-a-scale-set"></a>Opnieuw opstarten van virtuele machines in een schaalset
Als een of meer virtuele machines in een schaalset opnieuw, wilt u [az vmss opnieuw](/cli/azure/vmss#az_vmss_restart). Met de parameter `--instance-ids` kunt u een of meer VM's opgeven om opnieuw op te starten. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset opnieuw opgestart. Om meerdere virtuele machines starten, scheidt u elke instantie-ID met een spatie.

Het volgende voorbeeld wordt opnieuw opgestart exemplaar *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```azurecli
az vmss restart --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="remove-vms-from-a-scale-set"></a>Virtuele machines uit een set scale verwijderen
Gebruik te verwijderen van een of meer virtuele machines in een set scale [az vmss delete-exemplaren](/cli/azure/vmss#delete-instances). De `--instance-ids` parameter kunt u opgeven van een of meer VM's te verwijderen. Als u opgeeft * voor het exemplaar-ID, alle virtuele machines in de schaalset worden verwijderd. Als u wilt verwijderen van meerdere virtuele machines, scheidt u elke instantie-ID met een spatie.

Het volgende voorbeeld verwijdert exemplaar *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```azurecli
az vmss delete-instances --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="next-steps"></a>Volgende stappen
Andere algemene taken voor schaalsets omvatten het [implementeren van een toepassing](virtual-machine-scale-sets-deploy-app.md), en [upgrade VM-instanties](virtual-machine-scale-sets-upgrade-scale-set.md). U kunt ook Azure CLI voor [regels voor automatisch schalen configureren](virtual-machine-scale-sets-autoscale-overview.md).
