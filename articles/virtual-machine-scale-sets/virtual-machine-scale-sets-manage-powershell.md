---
title: Beheren van Virtual Machine Scale Sets met Azure PowerShell | Microsoft Docs
description: Algemene Azure PowerShell-cmdlets voor het beheren van Virtual Machine Scale Sets, zoals het starten en stoppen van een exemplaar of de schaal wijzigen instellen capaciteit.
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
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
ms.author: cynthn
ms.openlocfilehash: c4684dd27aeed4fab4335a93ea5a458b4a9f5d80
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56821180"
---
# <a name="manage-a-virtual-machine-scale-set-with-azure-powershell"></a>Een virtuele-machineschaalset met Azure PowerShell beheren

Gedurende de levenscyclus van een schaalset voor virtuele machines moet u mogelijk een of meer beheertaken uitvoeren. Bovendien wilt u misschien scripts maken die verschillende levenscyclustaken automatiseren. Dit artikel worden enkele van de algemene Azure PowerShell-cmdlets waarmee u kunt deze taken uitvoeren.

Als u maken van een virtuele-machineschaalset wilt, kunt u [maken van een schaalset met Azure PowerShell](quick-create-powershell.md).

[!INCLUDE [updated-for-az-vm.md](../../includes/updated-for-az-vm.md)]

## <a name="view-information-about-a-scale-set"></a>Informatie weergeven over een schaalset
Als u wilt weergeven van de algemene informatie over een schaalset, gebruikt u [Get-AzVmss](/powershell/module/az.compute/get-azvmss). Het volgende voorbeeld wordt informatie over de schaalset met de naam *myScaleSet* in de *myResourceGroup* resourcegroep. Voer uw eigen namen als volgt in:

```powershell
Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## <a name="view-vms-in-a-scale-set"></a>Virtuele machines weergeven in een schaalset
Een overzicht van VM-exemplaar in een schaalset, gebruikt u [Get-AzVmssVM](/powershell/module/az.compute/get-azvmssvm). Het volgende voorbeeld worden alle VM-exemplaren in de schaalset met de naam *myScaleSet* en in de *myResourceGroup* resourcegroep. Geef uw eigen waarden op voor deze namen:

```powershell
Get-AzVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```

Als wilt weergeven als u meer informatie over een specifieke VM-exemplaar, voegt de `-InstanceId` parameter [Get-AzVmssVM](/powershell/module/az.compute/get-azvmssvm) en geeft u een exemplaar om weer te geven. Het volgende voorbeeld wordt informatie opgevraagd over VM-exemplaar *0* in de schaalset met de naam *myScaleSet* en de *myResourceGroup* resourcegroep. Voer uw eigen namen als volgt in:

```powershell
Get-AzVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="change-the-capacity-of-a-scale-set"></a>De capaciteit van een schaalset wijzigen
De voorgaande opdrachten hebt u geleerd informatie over uw schaalset en de VM-exemplaren. Als u wilt vergroten of verkleinen van het aantal exemplaren in de schaalset, kunt u de capaciteit te wijzigen. De schaalset automatisch wordt gemaakt of verwijdert u het vereiste aantal virtuele machines en configureert u de virtuele machines voor het ontvangen van toepassingsverkeer.

Maak eerst een schaalsetobject met [Get-AzVmss](/powershell/module/az.compute/get-azvmss) en geef vervolgens een nieuwe waarde op voor `sku.capacity`. Gebruik [Update-AzVmss](/powershell/module/az.compute/update-azvmss) om de capaciteitswijziging toe te passen. Het volgende voorbeeld updates *myScaleSet* in de *myResourceGroup* resourcegroep op een capaciteit van *5* exemplaren. Geef uw eigen waarden als volgt:

```powershell
# Get current scale set
$vmss = Get-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"

# Set and update the capacity of your scale set
$vmss.sku.capacity = 5
Update-AzVmss -ResourceGroupName "myResourceGroup" -Name "myScaleSet" -VirtualMachineScaleSet $vmss
```

Het duurt een paar minuten om de capaciteit van de schaalset bij te werken. Als u de capaciteit van een schaalset instellen, de virtuele machines met het hoogste exemplaar id's worden het eerst verwijderd.


## <a name="stop-and-start-vms-in-a-scale-set"></a>Stoppen en starten van virtuele machines in een schaalset
Als u een of meer VM's in een schaalset wilt stoppen, gebruikt u [Stop-AzVmss](/powershell/module/az.compute/stop-azvmss). Met de parameter `-InstanceId` kunt u een of meer VM's opgeven om te stoppen. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset gestopt. Als u wilt stoppen met meerdere virtuele machines, elk exemplaar-ID met een komma te scheiden.

Het volgende voorbeeld stopt exemplaar *0* in de schaalset met de naam *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```powershell
Stop-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```

De standaardinstelling is dat de toewijzing van gestopte VM's ongedaan wordt gemaakt en er dus geen compute-kosten meer in rekening worden gebracht. Als u wilt dat de VM de ingerichte status blijft houden nadat deze is gestopt, voeg u de parameter `-StayProvisioned` toe aan de voorgaande opdracht. Voor gestopte VM's die ingericht blijven, worden periodieke compute-kosten in rekening gebracht.


### <a name="start-vms-in-a-scale-set"></a>Virtuele machines in een schaalset starten
Als u een of meer VM's in een schaalset wilt starten, gebruikt u [Start-AzVmss](/powershell/module/az.compute/start-azvmss). Met de parameter `-InstanceId` kunt u een of meer VM's opgeven om te starten. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset gestart. Voor het starten van meerdere virtuele machines, elk exemplaar-ID met een komma te scheiden.

Het volgende voorbeeld start exemplaar *0* in de schaalset met de naam *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```powershell
Start-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="restart-vms-in-a-scale-set"></a>Virtuele machines in een schaalset opnieuw opstarten
Als u wilt een of meer virtuele machines opnieuw opstarten in een schaalset, gebruikt u [opnieuw opstarten-AzVmss](/powershell/module/az.compute/restart-azvmss). Met de parameter `-InstanceId` kunt u een of meer VM's opgeven om opnieuw op te starten. Als u geen exemplaar-id opgeeft, worden alle VM's in de schaalset opnieuw opgestart. Als u wilt meerdere virtuele machines opnieuw opstarten, elk exemplaar-ID met een komma te scheiden.

Het volgende voorbeeld wordt opnieuw opgestart exemplaar *0* in de schaalset met de naam *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```powershell
Restart-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="remove-vms-from-a-scale-set"></a>Virtuele machines verwijderen uit een schaalset
Als u wilt verwijderen van een of meer virtuele machines in een schaalset, gebruikt u [Remove-AzVmss](/powershell/module/az.compute/remove-azvmss). De `-InstanceId` parameter kunt u opgeven van een of meer virtuele machines te verwijderen. Als u geen exemplaar-ID opgeeft, worden alle virtuele machines in de schaalset verwijderd. Als u wilt verwijderen van meerdere virtuele machines, elk exemplaar-ID met een komma te scheiden.

Het volgende voorbeeld verwijdert u exemplaar *0* in de schaalset met de naam *myScaleSet* en de *myResourceGroup* resourcegroep. Geef uw eigen waarden als volgt:

```powershell
Remove-AzVmss -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet" -InstanceId "0"
```


## <a name="next-steps"></a>Volgende stappen
Andere algemene taken voor schaalsets zijn onder andere hoe u [implementeren van een toepassing](virtual-machine-scale-sets-deploy-app.md), en [VM-exemplaren een upgrade uitvoert](virtual-machine-scale-sets-upgrade-scale-set.md). U kunt ook Azure PowerShell te gebruiken [regels voor automatisch schalen configureren](virtual-machine-scale-sets-autoscale-overview.md).
