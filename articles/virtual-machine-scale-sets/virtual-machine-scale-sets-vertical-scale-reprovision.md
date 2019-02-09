---
title: Verticaal schalen Azure virtuele-machineschaalsets | Microsoft Docs
description: Verticaal schalen van een virtuele Machine in reactie op waarschuwingen met Azure Automation
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 16b17421-6b8f-483e-8a84-26327c44e9d3
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: manayar
ms.openlocfilehash: 1a8bfbe12156156944d4527ebb11fa6f1a1de544
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55977232"
---
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Hiermee stelt u verticaal automatisch schalen met virtuele-machineschaalset

Dit artikel wordt beschreven hoe u verticaal schaalt Azure [Virtual Machine Scale Sets](https://azure.microsoft.com/services/virtual-machine-scale-sets/) met of zonder beëindiging. Raadpleeg voor verticaal schalen van virtuele machines die zich niet in schaalsets, [verticaal schalen met Azure Automation virtuele Azure-machine](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Verticaal schalen, ook wel bekend als *omhoog schalen* en *omlaag schalen*houdt in oplopende of aflopende volgorde van de grootte van de virtuele machines (VM) in reactie op een workload. Dit gedrag met vergelijken [horizontaal schalen](virtual-machine-scale-sets-autoscale-overview.md), ook wel *uitschalen* en *inschalen*, waarbij het aantal virtuele machines wordt gewijzigd, afhankelijk van de werkbelasting.

Beëindiging betekent dat een bestaande virtuele machine verwijderen en deze te vervangen door een nieuw exemplaar. Wanneer u vergroten of verkleinen van de grootte van virtuele machines in een virtuele-machineschaalset ingesteld, in sommige gevallen die u wilt vergroten of verkleinen van bestaande VM's en uw gegevens behouden, terwijl in andere gevallen u moet nieuwe virtuele machines van de nieuwe grootte te implementeren. In dit document bevat informatie over beide gevallen.

Verticaal schalen kan handig zijn wanneer:

* Een service die is gebouwd op virtuele machines is benut (bijvoorbeeld in het weekend). De VM-grootte te beperken, kan de maandelijkse kosten verminderen.
* Steeds meer VM-grootte moet omgaan met grotere aanvraag zonder dat er extra virtuele machines gemaakt.

U kunt instellen verticaal schalen om te worden geactiveerd op basis van metrische waarschuwingen op basis van uw virtuele-machineschaalset. Als de waarschuwing is geactiveerd, deze gebeurtenis wordt gestart een webhook die een runbook met een opschaalmogelijkheid uw schaal ingesteld omhoog of omlaag triggers. Verticaal schalen kan worden geconfigureerd door de volgende stappen:

1. Maak een Azure Automation-account met run as-functionaliteit.
2. Verticaal schalen van Azure Automation-runbooks voor virtuele-machineschaalsets importeren in uw abonnement.
3. Een webhook toevoegen aan uw runbook.
4. Een waarschuwing toevoegen aan uw VM-schaalset met behulp van een webhook-melding.

> [!NOTE]
> Verticaal automatisch schalen kan alleen plaatsvinden binnen bepaalde bereiken van VM-grootten. De specificaties van elke grootte vergelijken voordat u besluit te schalen van één naar een andere (hoe hoger de waarde geeft niet altijd aan grotere VM-grootte). U kunt kiezen om te schalen tussen de volgende sets grootten:
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
Het eerste wat dat u moet doen is een Azure Automation-account dat als host fungeert voor de runbooks gebruikt voor het schalen van de exemplaren in de schaalset virtuele machine maken. Onlangs [Azure Automation](https://azure.microsoft.com/services/automation/) de 'Uitvoeren als-account'-functie waarmee de instelling van de Service-Principal voor het automatisch uitvoeren van de runbooks namens een gebruiker die is geïntroduceerd. Zie voor meer informatie:

* [Runbooks verifiëren met een Azure Uitvoeren als-account](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Verticaal schalen van Azure Automation-runbooks in uw abonnement importeren
De runbooks die nodig zijn voor uw schaalsets voor virtuele machines verticaal schalen zijn al gepubliceerd in de Azure Automation Runbook Gallery. Als u wilt importeren Volg in uw abonnement de stappen in dit artikel:

* [Runbook- en galerieën voor Azure Automation](../automation/automation-runbook-gallery.md)

Kies de optie Bladeren in galerie in het menu Runbooks:

![Runbooks moeten worden geïmporteerd][runbooks]

De runbooks die moeten worden geïmporteerd, worden weergegeven. Selecteer het runbook op basis van of u wilt dat verticaal schalen met of zonder beëindiging:

![Runbookgalerie][gallery]

## <a name="add-a-webhook-to-your-runbook"></a>Een webhook toevoegen aan uw runbook
Nadat u de runbooks hebt geïmporteerd, moet u een webhook toevoegen aan het runbook, zodat deze kan worden geactiveerd door een waarschuwing van een virtuele-machineschaalset. De details van het maken van een webhook voor het Runbook worden beschreven in dit artikel:

* [Azure Automation-webhooks](../automation/automation-webhooks.md)

> [!NOTE]
> Zorg ervoor dat u de URI van de webhook kopiëren voordat de webhook-dialoogvenster te sluiten als u dit adres in de volgende sectie.
> 
> 

## <a name="add-an-alert-to-your-virtual-machine-scale-set"></a>Een waarschuwing toevoegen aan uw virtuele-machineschaalset
Hieronder wordt een PowerShell-script dat laat zien hoe u een waarschuwing toevoegen aan een virtuele-machineschaalset ingesteld. Raadpleeg het volgende artikel om op te halen van de naam van de metrische waarde voor het starten van de waarschuwing in: [Azure Monitor autoscaling common metrics](../azure-monitor/platform/autoscale-common-metrics.md).

```
$actionEmail = New-AzAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzMetricAlertRule  -Name  $alertName `
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
> Het verdient aanbeveling een redelijke tijdvenster voor de waarschuwing configureren om te voorkomen dat verticaal schalen en eventuele bijbehorende onderbrekingen, te vaak wordt geactiveerd. Houd rekening met een venster van minimaal 20-30 minuten of langer. Houd rekening met een horizontaal schalen als u nodig hebt om te voorkomen dat een onderbreking.
> 
> 

Zie de volgende artikelen voor meer informatie over het maken van waarschuwingen:

* [Voorbeelden van Azure Monitor PowerShell-snelstartgids](../azure-monitor/platform/powershell-quickstart-samples.md)
* [Azure Monitor platformoverschrijdende CLI Quick Start-voorbeelden](../azure-monitor/platform/cli-samples.md)

## <a name="summary"></a>Samenvatting
In dit artikel hebt u geleerd eenvoudige voorbeelden van verticaal schalen. Met deze bouwstenen - Automation-account, runbooks, webhooks, waarschuwingen - kunt u verbinding maken met een uitgebreid scala aan gebeurtenissen met een aangepaste set acties.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
