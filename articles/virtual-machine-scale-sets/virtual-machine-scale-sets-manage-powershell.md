---
title: Virtuele-Machineschaalsets met Azure PowerShell beheren | Microsoft Docs
description: Algemene Azure PowerShell-cmdlets voor het beheren van virtuele-Machineschaalsets, zoals het starten en stoppen van een exemplaar of de schaal wijzigen instellen capaciteit.
services: virtual-machine-scale-sets
documentationcenter: ''
author: iainfoulds
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d35fa77a-de96-4ccd-a332-eb181d1f4273
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: iainfou
ms.openlocfilehash: 39cd7fa2232329716ba16abf92ba4a5f2cc15487
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34652777"
---
# <a name="manage-a-virtual-machine-scale-set-with-azure-powershell"></a>Een virtuele-machineschaalset ingesteld met Azure PowerShell beheren
Gedurende de levenscyclus van een schaalset voor virtuele machines moet u mogelijk een of meer beheertaken uitvoeren. Bovendien wilt u misschien scripts maken die verschillende levenscyclustaken automatiseren. In dit artikel vindt u details van de algemene Azure PowerShell-cmdlets waarmee u kunt deze taken uitvoeren.

U moet de meest recente Azure PowerShell-module voor het voltooien van deze beheertaken. Zie voor informatie [aan de slag met Azure PowerShell](/powershell/azure/get-started-azureps). Als u maken van een virtuele-machineschaalset wilt, kunt u [maken van een schaal ingesteld met Azure PowerShell](quick-create-powershell.md).


## <a name="view-information-about-a-scale-set"></a>Informatie over een schaalset weergeven
Gebruiken om de algemene informatie weergeven over een schaalset, [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss). Het volgende voorbeeld wordt informatie over de set met de naam scale *myScaleSet* in de *myResourceGroup* resourcegroep. Voer uw eigen namen:

```powershell
Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## <a name="view-vms-in-a-scale-set"></a>Virtuele machines weergeven in een schaalset
Een overzicht van VM-instantie in een schaalset gebruiken [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm). Het volgende voorbeeld worden alle VM-exemplaren in de set met de naam scale *myScaleSet* en in de *myResourceGroup* resourcegroep. Uw eigen waarden opgeven voor deze namen:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Om weer te geven als u meer informatie over een specifiek exemplaar van de VM, voeg de `-InstanceId` -parameter voor [Get-AzureRmVmssVM](/powershell/module/azurerm.compute/get-azurermvmssvm) en geeft u een exemplaar om weer te geven. Informatie over de VM-instantie voor het volgende voorbeeld-weergaven *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Voer uw eigen namen:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="change-the-capacity-of-a-scale-set"></a>De capaciteit van een schaalset wijzigen
De voorgaande opdrachten hebt u geleerd informatie over uw schaalset en de VM-exemplaren. Als u wilt vergroten of verkleinen het aantal exemplaren in de schaalset, kunt u de capaciteit. De schaal automatisch ingesteld maakt of verwijdert u het vereiste aantal virtuele machines en configureert u de virtuele machines voor het ontvangen verkeer van de toepassing.

Maak eerst een schaalsetobject met [Get-AzureRmVmss](/powershell/module/azurerm.compute/get-azurermvmss) en geef vervolgens een nieuwe waarde op voor `sku.capacity`. Gebruik [Update-AzureRmVmss](/powershell/module/azurerm.compute/update-azurermvmss) om de capaciteitswijziging toe te passen. Het volgende voorbeeld updates *myScaleSet* in de *myResourceGroup* resourcegroep naar een capaciteit van *5* exemplaren. Geef uw eigen waarden als volgt:

```powershell
# Get current scale set
$vmss = Get-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 5
Update-AzureRmVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -VirtualMachineScaleSet $vmss
```

Het duurt een paar minuten om de capaciteit van de schaalset bij te werken. Als u verlaagt de capaciteit van een schaal ingesteld, wordt de virtuele machines met de hoogste exemplaar id's eerst worden verwijderd.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Stoppen en starten van virtuele machines in een schaalset
Als u een of meer VM's in een schaalset wilt stoppen, gebruikt u [Stop-AzureRmVmss](/powershell/module/azurerm.compute/stop-azurermvmss). Met de parameter `-InstanceId` kunt u een of meer VM's opgeven om te stoppen. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset gestopt. Als u wilt stoppen met meerdere virtuele machines, elk exemplaar-ID met een komma te scheiden.

Het volgende voorbeeld gestopt exemplaar *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```powershell
Stop-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```

De standaardinstelling is dat de toewijzing van gestopte VM's ongedaan wordt gemaakt en er dus geen compute-kosten meer in rekening worden gebracht. Als u wilt dat de VM de ingerichte status blijft houden nadat deze is gestopt, voeg u de parameter `-StayProvisioned` toe aan de voorgaande opdracht. Voor gestopte VM's die ingericht blijven, worden periodieke compute-kosten in rekening gebracht.


### <a name="start-vms-in-a-scale-set"></a>Starten van virtuele machines in een schaalset
Als u een of meer VM's in een schaalset wilt starten, gebruikt u [Start-AzureRmVmss](/powershell/module/azurerm.compute/start-azurermvmss). Met de parameter `-InstanceId` kunt u een of meer VM's opgeven om te starten. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset gestart. U kunt meerdere virtuele machines starten door elk exemplaar-ID met een komma te scheiden.

Het volgende voorbeeld wordt een exemplaar gestart *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```powershell
Start-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="restart-vms-in-a-scale-set"></a>Opnieuw opstarten van virtuele machines in een schaalset
Als u een of meer VM's in een schaalset opnieuw wilt opstarten, gebruikt u [Restart-AzureRmVmss](/powershell/module/azurerm.compute/restart-azurermvmss). Met de parameter `-InstanceId` kunt u een of meer VM's opgeven om opnieuw op te starten. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset opnieuw opgestart. Als u wilt meerdere virtuele machines opnieuw is opgestart, elke instantie-ID met een komma te scheiden.

Het volgende voorbeeld wordt opnieuw opgestart exemplaar *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```powershell
Restart-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="remove-vms-from-a-scale-set"></a>Virtuele machines uit een set scale verwijderen
Gebruik te verwijderen van een of meer virtuele machines in een set scale [verwijderen AzureRmVmss](/powershell/module/azurerm.compute/remove-azurermvmss). De `-InstanceId` parameter kunt u opgeven van een of meer VM's te verwijderen. Als u een exemplaar-ID niet opgeeft, worden alle virtuele machines in de schaalset verwijderd. Als u wilt verwijderen van meerdere virtuele machines, elk exemplaar-ID met een komma te scheiden.

Het volgende voorbeeld verwijdert exemplaar *0* in de set met de naam scale *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```powershell
Remove-AzureRmVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="next-steps"></a>Volgende stappen
Andere algemene taken voor schaalsets omvatten het [implementeren van een toepassing](virtual-machine-scale-sets-deploy-app.md), en [upgrade VM-instanties](virtual-machine-scale-sets-upgrade-scale-set.md). U kunt ook Azure PowerShell te gebruiken [regels voor automatisch schalen configureren](virtual-machine-scale-sets-autoscale-overview.md).
