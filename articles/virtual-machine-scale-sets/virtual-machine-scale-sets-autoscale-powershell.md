---
title: Automatisch schalen virtuele-machineschaalsets met Azure PowerShell | Microsoft Docs
description: Het maken van regels voor automatisch schalen voor virtuele-machineschaalset ingesteld met Azure PowerShell
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 88886cad-a2f0-46bc-8b58-32ac2189fc93
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: iainfou
ms.openlocfilehash: 8928e56f353858234db314714d411a9c2990eb4e
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="automatically-scale-a-virtual-machine-scale-set-with-azure-powershell"></a>Automatisch schalen van een virtuele-machineschaalset ingesteld met Azure PowerShell
Wanneer u een schaalset maakt, definieert u het aantal VM-exemplaren die u wilt uitvoeren. Als uw toepassing vraag verandert, kunt u automatisch vergroten of verkleinen van het aantal VM-exemplaren. De mogelijkheid om te schalen kunt u met de vraag van klanten houden of reageren op wijzigingen in de toepassingsprestaties gedurende de levenscyclus van uw app.

In dit artikel leest u hoe automatisch schalen om regels te maken met Azure PowerShell die de prestaties van de VM-exemplaren in de schaalset controleren. Deze regels voor automatisch schalen vergroten of verkleinen het aantal VM-exemplaren in reactie op deze maatstaven voor prestaties. U kunt ook deze stappen voltooien met de [Azure CLI 2.0](virtual-machine-scale-sets-autoscale-cli.md) of in de [Azure-portal](virtual-machine-scale-sets-autoscale-portal.md).


## <a name="prerequisites"></a>Vereisten
Voor het maken van regels voor automatisch schalen, moet u een bestaande virtuele machine schaalset. U kunt een instellen met schaal maken de [Azure-portal](virtual-machine-scale-sets-create-portal.md), [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md), of [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md).

Zodat het eenvoudiger te maken van de regels voor automatisch schalen, het definiëren van sommige variabelen voor uw scale set. Het volgende voorbeeld definieert de variabelen voor de set met de naam scale *myScaleSet* in de resourcegroep met de naam *myResourceGroup* en in de *VS-Oost* regio. Uw abonnement ID wordt verkregen met [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription). Als u meerdere abonnementen die zijn gekoppeld aan uw account hebt, wordt alleen het eerste abonnement geretourneerd. De namen en de abonnements-ID als volgt aanpassen:

```powershell
$mySubscriptionId = (Get-AzureRmSubscription).Id
$myResourceGroup = "myResourceGroupScaleSet"
$myScaleSet = "myScaleSet"
$myLocation = "East US"
```


## <a name="create-a-rule-to-automatically-scale-out"></a>Maak een regel automatisch uit te schalen
Als uw toepassing vraag toeneemt stelt de belasting van de VM-exemplaren in uw scale toeneemt. Als deze toegenomen belasting consistent is, in plaats van slechts een korte vraag, kunt u regels voor automatisch schalen voor een verhoging van het aantal VM-exemplaren in de schaalset configureren. Wanneer deze VM-exemplaren zijn gemaakt en uw toepassingen worden geïmplementeerd, wordt de schaalaanpassingsset begint met het distribueren van verkeer via de load balancer. U bepaalt welke metrische gegevens moeten worden bewaakt, zoals CPU of schijf, hoe lang het laden van toepassingen moet voldoen aan een bepaalde drempelwaarde en instellen hoeveel exemplaren van virtuele machine toevoegen aan de schaal.

We gaan maken van een regel met [nieuw AzureRmAutoscaleRule](/powershell/module/AzureRM.Insights/New-AzureRmAutoscaleRule) die verhoogt het aantal VM-exemplaren in een schaal ingesteld wanneer de gemiddelde CPU-belasting groter dan 70% gedurende een periode van 5 minuten is. Wanneer de regel wordt geactiveerd, wordt het aantal VM-instanties verhoogd met 20%. In met een klein aantal exemplaren van de VM-schaalsets, u kan weglaten `-ScaleActionScaleType` en geef alleen `-ScaleActionValue` verbeteren door *1* of *2* exemplaren. In-schaalsets met een groot aantal VM-instanties, een toename van 10% of % 20 VM-instanties mogelijk meer geschikt is.

De volgende parameters worden gebruikt voor deze regel:

| Parameter               | Uitleg                                                                                                         | Waarde          |
|-------------------------|---------------------------------------------------------------------------------------------------------------------|----------------|
| *-MetricName*           | De metriek prestaties bewaken en schalen van toepassing instellen acties op.                                                   | Percentage CPU |
| *-TimeGrain*            | Hoe vaak de metrische gegevens worden verzameld voor analyse.                                                                   | 1 minuut       |
| *-MetricStatistic*      | Hiermee definieert u hoe de verzamelde metrische gegevens moeten worden samengevoegd voor analyse.                                                | Gemiddeld        |
| *-Waarde voor TimeWindow*           | De hoeveelheid tijd gecontroleerd voordat de metrische gegevens en drempelwaarde waarden worden vergeleken.                                   | 10 minuten      |
| *-Operator*             | De operator is gebruikt voor het vergelijken van de metrische gegevens tegen de drempelwaarde.                                                     | Groter dan   |
| *-Drempelwaarde*            | De waarde zorgt ervoor dat de regel voor automatisch schalen voor het activeren van een actie.                                                      | 70%            |
| *-ScaleActionDirection* | Als de schaalaanpassingsset omhoog of omlaag wanneer de regel van toepassing moet schalen definieert.                                             | Verhogen       |
| *– ScaleActionScaleType* | Hiermee wordt aangegeven dat het aantal exemplaren van de virtuele machine moet worden gewijzigd door een percentage.                                 | Percentage wijzigen |
| *-ScaleActionValue*     | Het percentage van de VM-exemplaren moet worden gewijzigd wanneer de regel wordt geactiveerd.                                            | 20             |
| *-ScaleActionCooldown*  | De hoeveelheid tijd moet worden gewacht voordat de regel wordt opnieuw toegepast zodat de acties voor automatisch schalen die tijd hebt om te laten treden. | 5 minuten      |

Het volgende voorbeeld wordt een object met de naam *myRuleScaleOut* die deze opschaling van de regel bevat. De *- MetricResourceId* gebruikt de variabelen die eerder zijn gedefinieerd voor de abonnements-ID, de naam van een resourcegroep en de schaal naam instellen:

```powershell
$myRuleScaleOut = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -TimeGrain 00:01:00 `
  -MetricStatistic Average `
  -TimeWindow 00:10:00 `
  -Operator GreaterThan `
  -Threshold 70 `
  -ScaleActionDirection Increase `
  –ScaleActionScaleType PercentChangeCount `
  -ScaleActionValue 20 `
  -ScaleActionCooldown 00:05:00
```


## <a name="create-a-rule-to-automatically-scale-in"></a>Maak een regel automatisch schalen in
Op een 's avonds of het weekend en kan uw aanvraag toepassing afnemen. Als deze minder belasting gedurende een periode consistent is, kunt u regels voor automatisch schalen Verminder het aantal VM-exemplaren in de schaalset configureren. Deze actie schaal in reduceert de kosten voor het uitvoeren van uw scale instellen als u alleen het aantal exemplaren is vereist om te voldoen aan de huidige aanvraag uitvoeren.

Maak een regel met [nieuw AzureRmAutoscaleRule](/powershell/module/AzureRM.Insights/New-AzureRmAutoscaleRule) die verkleinen het aantal VM-exemplaren in een schaal ingesteld wanneer de gemiddelde CPU-belasting vervolgens onder 30% gedurende een periode 10 minuten zakt. Als de regel wordt gegenereerd, wordt het aantal VM-exemplaren van 20% verminderd. Het volgende voorbeeld wordt een object met de naam *myRuleScaleDown* die deze opschaling van de regel bevat. De *- MetricResourceId* gebruikt de variabelen die eerder zijn gedefinieerd voor de abonnements-ID, de naam van een resourcegroep en de schaal naam instellen:

```powershell
$myRuleScaleIn = New-AzureRmAutoscaleRule `
  -MetricName "Percentage CPU" `
  -MetricResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -Operator LessThan `
  -MetricStatistic Average `
  -Threshold 30 `
  -TimeGrain 00:01:00 `
  -TimeWindow 00:10:00 `
  -ScaleActionCooldown 00:05:00 `
  -ScaleActionDirection Decrease `
  –ScaleActionScaleType PercentChangeCount `
  -ScaleActionValue 20
```


## <a name="define-an-autoscale-profile"></a>Definieer een profiel voor automatisch schalen
Een schaalset koppelt u uw regels voor automatisch schalen, moet u een profiel maken. Het profiel voor automatisch schalen definieert de standaard, minimale en maximale scale set capaciteit en koppelt u uw regels voor automatisch schalen. Maken van een profiel voor automatisch schalen met [nieuw AzureRmAutoscaleProfile](/powershell/module/AzureRM.Insights/New-AzureRmAutoscaleProfile). Het volgende voorbeeld wordt de standaard- en ten minste capaciteit van *2* VM-exemplaren en maximaal *10*. De scale-out en schaal in de regels die zijn gemaakt in de voorgaande stappen vervolgens zijn gekoppeld:

```powershell
$myScaleProfile = New-AzureRmAutoscaleProfile `
  -DefaultCapacity 2  `
  -MaximumCapacity 10 `
  -MinimumCapacity 2 `
  -Rules $myRuleScaleOut,$myRuleScaleIn `
  -Name "autoprofile"
```


## <a name="apply-autoscale-rules-to-a-scale-set"></a>Automatisch schalen regels toepassen op een schaalset
De laatste stap is het profiel voor automatisch schalen toepassen op uw schaalset. Schaal kan vervolgens automatisch schalen in- of uitzoomen op basis van de vraag van de toepassing. Toepassen van het profiel voor automatisch schalen met [toevoegen AzureRmAutoscaleSetting](/powershell/module/AzureRM.Insights/Add-AzureRmAutoscaleSetting) als volgt:

```powershell
Add-AzureRmAutoscaleSetting `
  -Location $myLocation `
  -Name "autosetting" `
  -ResourceGroup $myResourceGroup `
  -TargetResourceId /subscriptions/$mySubscriptionId/resourceGroups/$myResourceGroup/providers/Microsoft.Compute/virtualMachineScaleSets/$myScaleSet `
  -AutoscaleProfiles $myScaleProfile
```


## <a name="monitor-number-of-instances-in-a-scale-set"></a>Aantal exemplaren in een set scale bewaken
Het aantal en de status van de VM-exemplaren Geef weer voor een lijst met instanties in uw instellen met schaal [Get-AzureRmVmssVM](/powershell/module/AzureRM.Compute/Get-AzureRmVmssVM). De status geeft aan of de VM-instantie is inrichting als de schaal automatisch uitgeschaald of is opheffen van inrichting tijdens de schaal automatisch schalen. De status van het VM-exemplaar voor de schaal set met de naam van het volgende voorbeeld-weergaven *myScaleSet* in de resourcegroep met de naam *myResourceGroup*:

```powershell
Get-AzureRmVmssVM -ResourceGroupName "myResourceGroup" -VMScaleSetName "myScaleSet"
```


## <a name="autoscale-based-on-a-schedule"></a>Op basis van een schema voor automatisch schalen
De eerdere voorbeelden automatisch geschaald op een schaal in- of instellen met basic host metrische gegevens zoals CPU-gebruik. U kunt ook regels voor automatisch schalen op basis van schema's maken. Deze regels op basis van een planning kunnen u automatisch schalen het aantal VM-instanties van tevoren een verwachte toename van de vraag van de toepassing, zoals core werkuren, en vervolgens automatisch schalen in het aantal exemplaren op een tijdstip dat u minder verwacht vraag, zoals het weekend.

Gebruik de Azure-portal voor het maken van regels voor automatisch schalen op basis van een schema in plaats van host metrische gegevens. Regels op basis van een planning kunnen niet op dit moment worden gemaakt met Azure PowerShell.


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u regels voor automatisch schalen gebruiken om horizontaal schalen en vergroten of verkleinen de *getal* van VM-exemplaren in uw scale ingesteld. Kunt u ook de schaal verticaal vergroten of verkleinen van de VM-instantie *grootte*. Zie voor meer informatie [verticale automatisch geschaald met de virtuele Machine-schaalsets](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Zie voor meer informatie over het beheren van uw VM-exemplaren [beheren virtuele-machineschaalsets met Azure PowerShell](virtual-machine-scale-sets-windows-manage.md).

Zie voor meer informatie over hoe waarschuwingen worden gegenereerd wanneer uw automatisch schalen regels trigger, [acties automatisch schalen gebruiken voor het verzenden van e-mail en -webhook-waarschuwingsmeldingen in de Azure-Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md). U kunt ook [controlelogboeken voor gebruik met het verzenden van e-mail en webhook-waarschuwingsmeldingen in de Azure-Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).
