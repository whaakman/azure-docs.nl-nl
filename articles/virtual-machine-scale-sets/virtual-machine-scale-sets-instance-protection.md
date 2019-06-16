---
title: Beveiliging-exemplaar voor Azure VM-schaalset instanties instellen | Microsoft Docs
description: Informatie over het beveiligen van virtuele Azure-machine schaalsetinstanties uit bewerkingen voor in- en -schaalset.
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: drewm
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2019
ms.author: manayar
ms.openlocfilehash: 61430f5a43a04fa0e5b2f0c79ff03419c73aaf28
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66416546"
---
# <a name="instance-protection-for-azure-virtual-machine-scale-set-instances-preview"></a>Beveiliging-exemplaar voor Azure VM-schaalset instanties (Preview) instellen
Schaalsets voor virtuele Azure-machine inschakelen betere flexibiliteit voor uw workloads via [voor automatisch schalen](virtual-machine-scale-sets-autoscale-overview.md), zodat u configureren kunt wanneer de schaal van uw infrastructuur en wanneer deze worden geschaald in. Schaalsets ook kunnen u centraal beheren, configureren en bijwerken van een groot aantal virtuele machines via verschillende [Upgradebeleid](virtual-machine-scale-sets-upgrade-scale-set.md#how-to-bring-vms-up-to-date-with-the-latest-scale-set-model) instellingen. U kunt een update voor het model met een schaalset configureren en de nieuwe configuratie wordt automatisch toegepast op elke schaal set-exemplaar als u het Upgradebeleid hebt ingesteld op automatische of Rolling.

Als uw toepassing verwerkt verkeer, kunnen er situaties waar u specifieke exemplaren van de rest van de schaal anders worden behandeld exemplaar instellen. Bijvoorbeeld bepaalde exemplaren in de schaalset langlopende bewerkingen kunnen uitvoeren, en u niet wilt dat deze instanties worden geschaald bij tot de bewerkingen worden voltooid. U hebt mogelijk een paar exemplaren in de schaalset om uit te voeren extra of andere taken dan de andere leden van de schaalset ook speciale. U moet deze 'speciale' virtuele machines niet te worden gewijzigd met de andere exemplaren in de schaalset. Beveiliging-exemplaar biedt de extra besturingselementen voor het inschakelen van deze en andere scenario's voor uw toepassing.

Dit artikel wordt beschreven hoe u kunt toepassen en de mogelijkheden voor preventie van ander exemplaar met schaalsetinstanties gebruiken.

> [!NOTE]
>Beveiliging-exemplaar is momenteel in openbare Preview. Geen procedure aanmelden is nodig voor het gebruik van de openbare preview-functionaliteit die hieronder worden beschreven. Preview-versie van exemplaar protection wordt alleen ondersteund met API-versie 2019-03-01 en klik op schaalsets met beheerde schijven.

## <a name="types-of-instance-protection"></a>Typen van de beveiliging van exemplaar
Schaalsets bieden twee typen mogelijkheden voor preventie van exemplaar:

-   **Beveiligen van schaal**
    - Ingeschakeld via de **protectFromScaleIn** eigenschap op schaal exemplaar instellen
    - Exemplaar beschermt tegen schaal in voor automatisch schalen is gestart
    - Bewerkingen voor de gebruiker geïnitieerde exemplaar (met inbegrip van exemplaar verwijderen) zijn **niet geblokkeerd**
    - Bewerkingen, gestart op de schaalset (een upgrade uitvoert, installatiekopie terugzetten, toewijzing ongedaan maken, enz.) zijn **niet geblokkeerd**

-   **Beschermen tegen scale set acties**
    - Ingeschakeld via de **protectFromScaleSetActions** eigenschap op schaal exemplaar instellen
    - Exemplaar beschermt tegen schaal in voor automatisch schalen is gestart
    - Beveiligd exemplaar van bewerkingen, gestart op de schaalset (zoals een upgrade kan de installatiekopie terugzetten, toewijzing ongedaan maken, enz.)
    - Bewerkingen voor de gebruiker geïnitieerde exemplaar (met inbegrip van exemplaar verwijderen) zijn **niet geblokkeerd**
    - Het verwijderen van de volledige schaalset is **niet geblokkeerd**

## <a name="protect-from-scale-in"></a>Beveiligen van schaal
Beveiliging-exemplaar kan worden toegepast instanties van schaalset nadat de exemplaren worden gemaakt. Beveiliging is toegepast en alleen gewijzigd de [model met instanties](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-vm-model-view) en niet op de [Virtual Machine scale sets model](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-model).

Er zijn meerdere manieren om schaal beveiliging toepast op exemplaren in uw schaalset zoals beschreven in de onderstaande voorbeelden.

### <a name="rest-api"></a>REST-API

Het volgende voorbeeld wordt de schaal beveiliging met een exemplaar in de schaalset.

```
PUT on `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vmScaleSetName}/virtualMachines/{instance-id}?api-version=2019-03-01`
```

```json
{
  "properties": {
    "protectionPolicy": {
      "protectFromScaleIn": true
    }
  }        
}

```

> [!NOTE]
>Beveiliging-exemplaar wordt alleen ondersteund met API-versie 2019-03-01 en hoger

### <a name="azure-powershell"></a>Azure PowerShell

Gebruik de [Update AzVmssVM](/powershell/module/az.compute/update-azvmssvm) cmdlet schaal beveiliging toepassen op uw schaal ingesteld exemplaar.

Het volgende voorbeeld wordt de schaal beveiliging met een exemplaar in de schaalset met de exemplaar-ID 0.

```azurepowershell-interactive
Update-AzVmssVM `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myVMScaleSet" `
  -InstanceId 0 `
  -ProtectFromScaleIn $true
```

### <a name="azure-cli-20"></a>Azure CLI 2.0

Gebruik [az vmss update](/cli/azure/vmss#az-vmss-update) schaal beveiliging toepassen op uw exemplaar van de schaalset.

Het volgende voorbeeld wordt de schaal beveiliging met een exemplaar in de schaalset met de exemplaar-ID 0.

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --instance-id 0 \
  --protect-from-scale-in true
```

## <a name="protect-from-scale-set-actions"></a>Beschermen tegen scale set acties
Beveiliging-exemplaar kan worden toegepast instanties van schaalset nadat de exemplaren worden gemaakt. Beveiliging is toegepast en alleen gewijzigd de [model met instanties](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-vm-model-view) en niet op de [Virtual Machine scale sets model](virtual-machine-scale-sets-upgrade-scale-set.md#the-scale-set-model).

Beveiligen van een exemplaar van de scale set acties ook voorkomt dat het exemplaar schalen in voor automatisch schalen is gestart.

Er zijn meerdere manieren van het toepassen van schaal acties de beveiliging instelt voor uw schaalset instanties zoals beschreven in de onderstaande voorbeelden.

### <a name="rest-api"></a>REST-API

Het volgende voorbeeld wordt de beveiliging van de scale set acties met een exemplaar in de schaalset.

```
PUT on `/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Compute/virtualMachineScaleSets/{vMScaleSetName}/virtualMachines/{instance-id}?api-version=2019-03-01`
```

```json
{
  "properties": {
    "protectionPolicy": {
      "protectFromScaleIn": true,
      "protectFromScaleSetActions": true
    }
  }        
}

```

> [!NOTE]
>Beveiliging-exemplaar wordt alleen ondersteund met API-versie 2019-03-01 en hoger.</br>
Beveiligen van een exemplaar van de scale set acties ook voorkomt dat het exemplaar schalen in voor automatisch schalen is gestart. U kunt geen 'protectFromScaleIn' opgeven: false bij het instellen van "protectFromScaleSetActions": true

### <a name="azure-powershell"></a>Azure PowerShell

Gebruik de [Update AzVmssVM](/powershell/module/az.compute/update-azvmssvm) cmdlet voor het toepassen van beveiliging tot schaal acties ingesteld op-exemplaar in uw schaalset.

Het volgende voorbeeld wordt de beveiliging van de scale set acties met een exemplaar in de schaalset met de exemplaar-ID 0.

```azurepowershell-interactive
Update-AzVmssVM `
  -ResourceGroupName "myResourceGroup" `
  -VMScaleSetName "myVMScaleSet" `
  -InstanceId 0 `
  -ProtectFromScaleIn $true `
  -ProtectFromScaleSetAction $true
```

### <a name="azure-cli-20"></a>Azure CLI 2.0

Gebruik [az vmss update](/cli/azure/vmss#az-vmss-update) beveiliging van de scale set acties aan-exemplaar in uw schaalset toepassen.

Het volgende voorbeeld wordt de beveiliging van de scale set acties met een exemplaar in de schaalset met de exemplaar-ID 0.

```azurecli-interactive
az vmss update \  
  --resource-group <myResourceGroup> \
  --name <myVMScaleSet> \
  --instance-id 0 \
  --protect-from-scale-in true \
  --protect-from-scale-set-actions true
```

## <a name="troubleshoot"></a>Problemen oplossen
### <a name="no-protectionpolicy-on-scale-set-model"></a>Er is geen protectionPolicy op model met een schaalset
Beveiliging-exemplaar is alleen van toepassing op schaalsetinstanties en niet op het model met een schaalset.

### <a name="no-protectionpolicy-on-scale-set-instance-model"></a>Er is geen protectionPolicy op schaal model met instanties instellen
Standaard protection-beleid niet toegepast op een exemplaar wanneer deze wordt gemaakt.

U kunt de beveiliging van het exemplaar instanties van schaalset nadat de exemplaren die zijn gemaakt toepassen.

### <a name="not-able-to-apply-instance-protection"></a>Kan geen exemplaar-beveiliging toepassen
Beveiliging-exemplaar wordt alleen ondersteund met API-versie 2019-03-01 en hoger. Controleer de API-versie die wordt gebruikt en behoefte bijwerken. Mogelijk moet u ook uw PowerShell of CLI bijwerken naar de nieuwste versie.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het [Implementeer uw toepassing](virtual-machine-scale-sets-deploy-app.md) ingesteld op de virtuele-machineschaalset.
