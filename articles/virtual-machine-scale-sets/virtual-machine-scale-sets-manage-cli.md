---
title: Beheren van virtuele-Machineschaalsets met de Azure CLI 2.0 | Microsoft Docs
description: Algemene Azure CLI 2.0-opdrachten voor het beheren van Virtual Machine Scale Sets, zoals het starten en stoppen van een exemplaar of de schaal wijzigen instellen capaciteit.
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: a9e01039f1fbf46739ff8dbafea411aad2c3f4f2
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38308050"
---
# <a name="manage-a-virtual-machine-scale-set-with-the-azure-cli-20"></a>Beheren van een virtuele-machineschaalset met de Azure CLI 2.0
Gedurende de levenscyclus van een schaalset voor virtuele machines moet u mogelijk een of meer beheertaken uitvoeren. Bovendien wilt u misschien scripts maken die verschillende levenscyclustaken automatiseren. Dit artikel worden enkele van de algemene Azure CLI 2.0-opdrachten waarmee u kunnen deze taken uitvoeren.

Als u wilt deze beheertaken hebt voltooid, moet u de nieuwste Azure CLI 2.0. Zie voor meer informatie, [Azure CLI 2.0 installeren](/cli/azure/install-azure-cli). Als u maken van een virtuele-machineschaalset wilt, kunt u [maken van een schaalset met Azure CLI 2.0](quick-create-cli.md).


## <a name="view-information-about-a-scale-set"></a>Informatie weergeven over een schaalset
Als u wilt weergeven van de algemene informatie over een schaalset, gebruikt u [az vmss show](/cli/azure/vmss#az_vmss_show). Het volgende voorbeeld wordt informatie over de schaalset met de naam *myScaleSet* in de *myResourceGroup* resourcegroep. Voer uw eigen namen als volgt in:

```azurecli
az vmss show --resource-group myResourceGroup --name myScaleSet
```


## <a name="view-vms-in-a-scale-set"></a>Virtuele machines weergeven in een schaalset
Een overzicht van VM-exemplaar in een schaalset, gebruikt u [az vmss list-instances](/cli/azure/vmss#list-instances). Het volgende voorbeeld worden alle VM-exemplaren in de schaalset met de naam *myScaleSet* in de *myResourceGroup* resourcegroep. Geef uw eigen waarden op voor deze namen:

```azurecli
az vmss list-instances \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --output table
```

Als wilt weergeven als u meer informatie over een specifieke VM-exemplaar, voegt de `--instance-id` parameter [az vmss get-instance-view](/cli/azure/vmss#get-instance-view) en geeft u een exemplaar om weer te geven. Het volgende voorbeeld wordt informatie opgevraagd over VM-exemplaar *0* in de schaalset met de naam *myScaleSet* en de *myResourceGroup* resourcegroep. Voer uw eigen namen als volgt in:

```azurecli
az vmss get-instance-view \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --instance-id 0
```


## <a name="list-connection-information-for-vms"></a>De verbindingsgegevens opvragen voor virtuele machines
Verbinding maken met de virtuele machines in een schaalset, u SSH of RDP naar een toegewezen openbare IP-adres en poort nummer. Standaard worden de regels voor network address translation (NAT) toegevoegd aan de Azure load balancer die verbinding met extern verkeer wordt doorgestuurd naar elke virtuele machine. Als u de adressen en poorten verbinding maken met VM-exemplaren in een schaalset, gebruikt u [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info). Het volgende voorbeeld worden de verbindingsgegevens voor VM-exemplaren in de schaalset met de naam *myScaleSet* en in de *myResourceGroup* resourcegroep. Geef uw eigen waarden op voor deze namen:

```azurecli
az vmss list-instance-connection-info \
    --resource-group myResourceGroup \
    --name myScaleSet
```


## <a name="change-the-capacity-of-a-scale-set"></a>De capaciteit van een schaalset wijzigen
De voorgaande opdrachten hebt u geleerd informatie over uw schaalset en de VM-exemplaren. Als u wilt vergroten of verkleinen van het aantal exemplaren in de schaalset, kunt u de capaciteit te wijzigen. De schaalset maakt of verwijdert u het vereiste aantal virtuele machines en configureert u de virtuele machines voor het ontvangen van toepassingsverkeer.

Als u het aantal instanties wilt weergeven dat zich momenteel in een schaalset bevindt, gebruikt u [az vmss show](/cli/azure/vmss#az_vmss_show) en voert u een query uit op *sku.capacity*:

```azurecli
az vmss show \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --query [sku.capacity] \
    --output table
```

U kunt vervolgens het aantal virtuele machines in de schaalset handmatig vergroten of verkleinen met [az vmss scale](/cli/azure/vmss#az_vmss_scale). Het volgende voorbeeld wordt het aantal virtuele machines in uw schaalset aan *5*:

```azurecli
az vmss scale \
    --resource-group myResourceGroup \
    --name myScaleSet \
    --new-capacity 5
```

Het duurt een paar minuten om de capaciteit van de schaalset bij te werken. Als u de capaciteit van een schaalset instellen, de virtuele machines met het hoogste exemplaar id's worden het eerst verwijderd.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Stoppen en starten van virtuele machines in een schaalset
Als u wilt stoppen met een of meer virtuele machines in een schaalset, gebruikt u [az vmss stop](/cli/azure/vmss/stop). Met de parameter `--instance-ids` kunt u een of meer VM's opgeven om te stoppen. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset gestopt. Als u wilt stoppen met meerdere virtuele machines, scheidt u elke instantie-ID met een spatie.

Het volgende voorbeeld stopt exemplaar *0* in de schaalset met de naam *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```azurecli
az vmss stop --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```

Gestopte VM's blijven toegewezen en nog steeds rekenkosten in rekening gebracht. Als u wilt de VM's ongedaan worden gemaakt en alleen opslag kosten in rekening gebracht, gebruikt u [az vmss deallocate](/cli/azure/vmss#az_vmss_deallocate). Voor de toewijzing ongedaan maken meerdere virtuele machines, scheidt u elke instantie-ID met een spatie. Het volgende voorbeeld wordt gestopt en wordt de toewijzing ingetrokken exemplaar *0* in de schaalset met de naam *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```azurecli
az vmss deallocate --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


### <a name="start-vms-in-a-scale-set"></a>Virtuele machines in een schaalset starten
Gebruiken om een of meer virtuele machines starten in een schaalset, [az vmss start](/cli/azure/vmss#az_vmss_start). Met de parameter `--instance-ids` kunt u een of meer VM's opgeven om te starten. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset gestart. Voor het starten van meerdere virtuele machines, scheidt u elke instantie-ID met een spatie.

Het volgende voorbeeld start exemplaar *0* in de schaalset met de naam *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```azurecli
az vmss start --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="restart-vms-in-a-scale-set"></a>Virtuele machines in een schaalset opnieuw opstarten
Als u wilt een of meer virtuele machines opnieuw opstarten in een schaalset, gebruikt u [az vmss start opnieuw op](/cli/azure/vmss#az_vmss_restart). Met de parameter `--instance-ids` kunt u een of meer VM's opgeven om opnieuw op te starten. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset opnieuw opgestart. Als u wilt meerdere virtuele machines opnieuw opstarten, scheidt u elke instantie-ID met een spatie.

Het volgende voorbeeld wordt opnieuw opgestart exemplaar *0* in de schaalset met de naam *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```azurecli
az vmss restart --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="remove-vms-from-a-scale-set"></a>Virtuele machines verwijderen uit een schaalset
Als u wilt verwijderen van een of meer virtuele machines in een schaalset, gebruikt u [az vmss delete-exemplaren](/cli/azure/vmss#delete-instances). De `--instance-ids` parameter kunt u opgeven van een of meer virtuele machines te verwijderen. Als u opgeeft * voor het exemplaar-ID, alle virtuele machines in de schaalset worden verwijderd. Als u wilt verwijderen van meerdere virtuele machines, scheidt u elke instantie-ID met een spatie.

Het volgende voorbeeld verwijdert u exemplaar *0* in de schaalset met de naam *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```azurecli
az vmss delete-instances --resource-group myResourceGroup --name myScaleSet --instance-ids 0
```


## <a name="next-steps"></a>Volgende stappen
Andere algemene taken voor schaalsets zijn onder andere hoe u [implementeren van een toepassing](virtual-machine-scale-sets-deploy-app.md), en [VM-exemplaren een upgrade uitvoert](virtual-machine-scale-sets-upgrade-scale-set.md). U kunt ook Azure CLI voor [regels voor automatisch schalen configureren](virtual-machine-scale-sets-autoscale-overview.md).
