---
title: Virtuele machine van Azure-schaalsets verticaal te schalen | Microsoft Docs
description: Een virtuele Machine verticaal te schalen in reactie op waarschuwingen met Azure Automation bewaken
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 16b17421-6b8f-483e-8a84-26327c44e9d3
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: negat
ms.openlocfilehash: 6e4733e023d1dc27fb099216f9afea07fe07446c
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Hiermee stelt u verticale automatisch geschaald met virtuele-machineschaalset
Dit artikel wordt beschreven hoe u Azure verticaal te schalen [virtuele-Machineschaalsets](https://azure.microsoft.com/services/virtual-machine-scale-sets/) met of zonder reprovisioning. Raadpleeg voor verticale schaling van virtuele machines die zich niet in-schaalsets [verticaal schalen Azure virtuele machine met Azure Automation](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Verticale schaling, ook wel bekend als *opschalen* en *omlaag schalen*houdt in oplopende of aflopende volgorde van de grootte van de virtuele machines (VM) in reactie op een werkbelasting. Dit gedrag met vergelijken [horizontaal schalen](virtual-machine-scale-sets-autoscale-overview.md), ook wel *uitschalen* en *schalen*, waarbij het aantal VM's afhankelijk van de werkbelasting wordt gewijzigd.

Reprovisioning betekent het verwijderen van een bestaande VM te vervangen door een nieuwe. Wanneer u vergroten of verkleinen van de grootte van virtuele machines in een virtuele-machineschaalset ingesteld, in sommige gevallen die u wilt vergroten of verkleinen bestaande virtuele machines en uw gegevens behouden, terwijl in andere gevallen moet u nieuwe virtuele machines van de nieuwe grootte implementeren. Dit document bevat informatie over beide gevallen.

Verticale schaling kan handig zijn wanneer:

* Een service die is gebaseerd op virtuele machines is onder gebruikt (bijvoorbeeld in het weekend). De VM-grootte verminderen kunt maandelijkse kosten te verlagen.
* VM-grootte toenemend omgaan met grotere verzoek zonder extra virtuele machines maken.

U kunt instellen verticaal schalen om te worden geactiveerd op basis van metrische waarschuwingen op basis van uw virtuele-machineschaalset. Wanneer de waarschuwing is geactiveerd, deze gebeurtenis wordt gestart een webhook die een runbook die kan worden geschaald schaal omhoog of omlaag ingesteld triggers. Verticale schaling kan worden geconfigureerd met de volgende stappen:

1. Een Azure Automation-account maken met run as-functionaliteit.
2. Verticale schaal van Azure Automation-runbooks voor virtuele-machineschaalsets in uw abonnement worden geïmporteerd.
3. Een webhook toevoegen aan uw runbook.
4. Een waarschuwing toevoegen aan uw virtuele-machineschaalset instellen met behulp van een webhook-melding.

> [!NOTE]
> Verticale automatisch schalen kan alleen plaatsvinden binnen een bepaalde adresbereiken van VM-formaten. Vergelijk de specificaties van elke grootte voordat u besluit om te schalen van een aan een andere (hoger de waarde niet altijd wordt aangegeven in de VM-grootte groter). U kunt kiezen tussen de volgende paren van grootte schalen:
> 
> | VM-grootten paar schalen |  |
> | --- | --- |
> | Standard_A0 |Standard_A11 |
> | Standard_D1 |Standard_D14 |
> | Standard_DS1 |Standard_DS14 |
> | Standard_D1v2 |Standard_D15v2 |
> | Standard_G1 |Standard_G5 |
> | Standard_GS1 |Standard_GS5 |
> 
> 

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Een Azure Automation-Account maken met run as-functionaliteit
Het eerste wat dat u moet doen is een Azure Automation-account dat als host fungeert voor de runbooks gebruikt voor het schalen van de virtuele machine scale set exemplaren maken. Recent [Azure Automation](https://azure.microsoft.com/services/automation/) geïntroduceerd van de functie 'Run As-account' is het instellen van de Service-Principal voor het automatisch uitvoeren van de runbooks namens een gebruiker. Zie voor meer informatie:

* [Runbooks verifiëren met een Azure Uitvoeren als-account](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Verticale schaal van Azure Automation-runbooks importeert in uw abonnement
De runbooks die nodig zijn voor uw virtuele-machineschaalsets verticaal te schalen zijn al gepubliceerd in de galerie van Azure Automation Runbook. Als u wilt importeren stappen in uw abonnement de in dit artikel:

* [Runbook- en galerieën voor Azure Automation](../automation/automation-runbook-gallery.md)

Kies de optie Galerij bladeren in het menu Runbooks:

![Runbooks moeten worden geïmporteerd][runbooks]

De runbooks die moeten worden geïmporteerd, worden weergegeven. Selecteer het runbook op basis van of u wilt dat verticale met of zonder reprovisioning schalen:

![Galerie met Runbooks][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Een webhook toevoegen aan uw runbook
Nadat u de runbooks hebt geïmporteerd, moet u een webhook toevoegen aan het runbook zodat deze kan worden geactiveerd door een waarschuwing van een virtuele-machineschaalset. De details van het maken van een webhook voor uw Runbook worden beschreven in dit artikel:

* [Azure Automation-webhooks.](../automation/automation-webhooks.md)

> [!NOTE]
> Zorg ervoor dat u de webhook URI kopiëren voordat u sluit het dialoogvenster webhook want u dit adres in de volgende sectie moet.
> 
> 

## <a name="add-an-alert-to-your-virtual-machine-scale-set"></a>Een waarschuwing toevoegen aan uw virtuele-machineschaalset
Hieronder wordt een PowerShell-script dat toont hoe u een waarschuwing toevoegt aan een virtuele-machineschaalset ingesteld. Raadpleeg het volgende artikel om op te halen van de naam van de metrische gegevens voor het starten van de waarschuwing in: [Azure Monitor automatisch schalen algemene metrische gegevens](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [!NOTE]
> Het verdient aanbeveling voor het configureren van een redelijke tijdvenster voor de waarschuwing om te voorkomen dat verticaal schalen en de bijbehorende service-onderbreking, te vaak activering. U kunt een venster van minimaal 20-30 minuten of langer. U kunt een horizontale schaal als u wilt vermijden.
> 
> 

Zie de volgende artikelen voor meer informatie over het maken van waarschuwingen:

* [Azure PowerShell Monitor Quick Start-voorbeelden](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Azure Monitor platformoverschrijdende CLI Quick Start-voorbeelden](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Samenvatting
In dit artikel hebt u geleerd eenvoudige verticaal vergroten/verkleinen voorbeelden. Met deze bouwstenen - Automation-account, runbooks, webhooks, waarschuwingen - kunt u een uitgebreide scala aan gebeurtenissen met een aangepaste set acties.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
