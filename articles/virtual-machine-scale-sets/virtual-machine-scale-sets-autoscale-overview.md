---
title: Overzicht van automatisch schalen met Azure virtual machine scale sets | Microsoft Docs
description: Meer informatie over de verschillende manieren waarop u kunt de schaal van een virtuele Azure-machine ingesteld op basis van prestaties of op een vast schema automatisch schalen
services: virtual-machine-scale-sets
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: d29a3385-179e-4331-a315-daa7ea5701df
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2018
ms.author: cynthn
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4c33466af6e57264e02a7d5c298f7f1f9fbf4ed8
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44717298"
---
# <a name="overview-of-autoscale-with-azure-virtual-machine-scale-sets"></a>Overzicht van automatisch schalen met Azure VM-schaalset wordt ingesteld
Een schaalset voor virtuele machine van Azure kan automatisch vergroten of verkleinen van het aantal VM-exemplaren die uw toepassing wordt uitgevoerd. Dit gedrag van automatische en flexibele vermindert de overhead voor het bewaken en optimaliseren van de prestaties van uw toepassing. U maakt regels die de aanvaardbare prestaties voor een positieve gebruikerservaring bepalen. Wanneer de gedefinieerde drempelwaarden wordt voldaan, maatregelen regels voor automatisch schalen om aan te passen van de capaciteit van uw schaalset. U kunt ook gebeurtenissen automatisch te vergroten of verkleinen, de capaciteit van uw schaalset op tijden vaste plannen. Dit artikel bevat een overzicht van welke prestaties metrische gegevens beschikbaar zijn en welke acties voor automatisch schalen kan uitvoeren.


## <a name="benefits-of-autoscale"></a>Voordelen van automatisch schalen
Als de vraag van uw toepassing toeneemt, neemt de belasting van de VM-exemplaren in de schaalset ook toe. Als deze toegenomen belasting consistent is, en geen piekbelasting is, kunt u regels voor automatisch schalen configureren om het aantal VM-exemplaren in de schaalset te verhogen.

Wanneer deze VM-exemplaren worden gemaakt en uw toepassingen worden geïmplementeerd, zorgt de schaalset ervoor dat er via de load balancer verkeer wordt gedistribueerd naar de exemplaren. U bepaalt welke meetwaarden gegevens te bewaken, zoals CPU of geheugen, hoe lang de belasting van de toepassing moet voldoen aan een bepaalde drempelwaarde en hoeveel VM-exemplaren toegevoegd aan de schaalset ingesteld.

In het weekend of 's avonds kan de vraag voor uw toepassing afnemen. Als deze afgenomen belasting consistent is gedurende een bepaalde periode, kunt u regels voor automatisch schalen configureren om het aantal VM-exemplaren in de schaalset te verlagen. Deze inschaalactie reduceert de kosten voor het uitvoeren van uw schaalset, aangezien u alleen het aantal exemplaren uitvoert dat vereist is om te voldoen aan de actuele vraag.


## <a name="use-host-based-metrics"></a>Hostgebaseerde metrische gegevens gebruiken
U kunt regels voor automatisch schalen die ingebouwde metrische gegevens voor hosts beschikbaar maken van uw VM-exemplaren. Metrische gegevens voor hosts, krijgt u inzicht in de prestaties van de VM-exemplaren in een schaalset zonder de noodzaak om te installeren of configureren van extra agents en verzamelen van gegevens. Regels voor automatisch schalen die gebruikmaken van deze metrische gegevens kunnen schaal uit of in het aantal VM-exemplaren in reactie op CPU-gebruik, geheugen vraag of toegang tot de schijf op.

Regels voor automatisch schalen die gebruikmaken van hostgebaseerde metrische gegevens, kunnen worden gemaakt met een van de volgende hulpprogramma's:

- [Azure Portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Azure PowerShell](tutorial-autoscale-powershell.md)
- [Azure CLI 2.0](tutorial-autoscale-cli.md)
- [Azure-sjabloon](tutorial-autoscale-template.md)

Voor het maken van regels voor automatisch schalen die gebruikmaken van meer gedetailleerde metrische gegevens voor prestaties, kunt u [installeren en configureren van de Azure diagnostics-extensie](#in-guest-vm-metrics-with-the-azure-diagnostics-extension) op VM-instanties, of [configureren van uw toepassing gebruik App Insights](#application-level-metrics-with-app-insights).

Regels voor automatisch schalen die gebruikmaken van hostgebaseerde metrische gegevens, metrische gegevens in de Gast-VM met de diagnostische Azure-extensie en de App Insights kunt gebruiken aan de volgende configuratie-instellingen.

### <a name="metric-sources"></a>Metrische bronnen
Regels voor automatisch schalen kunnen u metrische gegevens gebruiken uit een van de volgende bronnen:

| Bron van metrische gegevens        | Use-case                                                                                                                     |
|----------------------|------------------------------------------------------------------------------------------------------------------------------|
| Huidige schaalset    | Voor hostgebaseerde metrische gegevens waarvoor geen extra agents worden geïnstalleerd of geconfigureerd.                                  |
| Storage-account      | Diagnostische Azure-extensie schrijft prestatiegegevens naar Azure storage, die vervolgens wordt gebruikt voor het activeren van regels voor automatisch schalen. |
| Service Bus-wachtrij    | Berichten op een Azure Service Bus-wachtrij op Triggerregels kunnen worden verzonden door uw toepassing of andere onderdelen.                   |
| Application Insights | Een instrumentatiepakket in uw toepassing die gegevensstromen van metrische gegevens rechtstreeks vanuit de app is geïnstalleerd.                         |


### <a name="autoscale-rule-criteria"></a>De criteria voor automatisch schalen
De volgende hostgebaseerde metrische gegevens zijn beschikbaar voor gebruik bij het maken van regels voor automatisch schalen. Als u de diagnostische Azure-extensie of App Insights gebruikt, kunt u definiëren welke metrische gegevens om te controleren en gebruiken met regels voor automatisch schalen.

| Naam van de meetwaarde               |
|---------------------------|
| Percentage CPU            |
| Netwerk in                |
| Netwerk uit               |
| Gelezen bytes op de schijf           |
| Geschreven bytes op de schijf          |
| Leesbewerkingen op de schijf/seconde  |
| Schrijfbewerkingen op de schijf/seconde |
| Resterend CPU-tegoed     |
| Verbruikt CPU-tegoed      |

Wanneer u regels voor automatisch schalen voor het bewaken van een metriek maakt, zoek de regels op een van de volgende metrische gegevens aggregatie acties:

| Aggregatietype |
|------------------|
| Gemiddeld          |
| Minimum          |
| Maximum          |
| Totaal            |
| Laatste             |
| Count            |

De regels voor automatisch schalen worden vervolgens geactiveerd wanneer de metrische gegevens zijn vergeleken met de ingestelde drempel is gekomen met een van de volgende operators:

| Operator                 |
|--------------------------|
| Groter dan             |
| Groter dan of gelijk aan |
| Kleiner dan                |
| Kleiner dan of gelijk aan    |
| Gelijk aan                 |
| Niet gelijk aan             |


### <a name="actions-when-rules-trigger"></a>Acties bij het activeren van regels
Wanneer een regel voor automatisch schalen triggers wordt uw schaalset kunt automatisch schalen in een van de volgende manieren:

| Schaalbewerking     | Use-case                                                                                                                               |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Aantal verhogen met   | Een vast aantal VM-exemplaren te maken. Dit is handig in schaalsets met een kleiner aantal virtuele machines.                                           |
| Percentage verhogen met | Een toename op basis van een percentage van de VM-exemplaren. Goed voor grotere schaal wordt ingesteld wanneer een toename van de vaste niet merkbaar om prestaties te optimaliseren. |
| Aantal verhogen tot   | Maken omdat het aantal VM-exemplaren moeten een gewenste bedrag voor het maximum bereikt.                                                            |
| Aantal verlagen met   | Een vast aantal VM-exemplaren te verwijderen. Dit is handig in schaalsets met een kleiner aantal virtuele machines.                                           |
| Percentage verlagen met | Een verkleinen op basis van een percentage van de VM-exemplaren. Goed voor grotere schaal wordt ingesteld op waar een toename van de vaste mogelijk niet merkbaar resourceverbruik en kosten verminderen. |
| Aantal verlagen tot   | Worden verwijderd omdat het aantal VM-exemplaren een gewenste minimum aan nodige bereiken zijn vereist.                                                            |


## <a name="in-guest-vm-metrics-with-the-azure-diagnostics-extension"></a>Metrische gegevens in de Gast-VM met de Azure diagnostics-extensie
De Azure diagnostics-extensie is een agent die wordt uitgevoerd binnen een VM-exemplaar. De agent bewaakt en slaat metrische gegevens voor prestaties in Azure storage. Deze metrische gegevens voor prestaties bevat meer gedetailleerde informatie over de status van de virtuele machine, zoals *AverageReadTime* voor schijven of *PercentIdleTime* voor CPU. U kunt regels voor automatisch schalen op basis van een meer gedetailleerde kennis van de prestaties van de virtuele machine, niet alleen het percentage CPU-gebruik of geheugen verbruik maken.

Voor het gebruik van de Azure diagnostics-extensie, moet u Azure storage-accounts maken voor uw VM-exemplaren, de Azure diagnostics-agent installeren en configureren van de VM's naar stream specifieke prestatiemeteritems met de storage-account.

Zie voor meer informatie de artikelen voor het inschakelen van de diagnostische Azure-extensie op een [Linux-VM](../virtual-machines/extensions/diagnostics-linux.md) of [Windows-VM](../virtual-machines/extensions/diagnostics-windows.md).


## <a name="application-level-metrics-with-app-insights"></a>Op toepassingsniveau metrische gegevens met App Insights
Als u wilt meer zichtbaarheid in om de prestaties van uw toepassingen te krijgen, kunt u Application Insights. U installeert een klein instrumentatiepakket in uw toepassing waarmee de app worden gecontroleerd en telemetrie verzendt naar Azure. U kunt metrische gegevens zoals de reactietijden van uw toepassing, de laadprestaties van de pagina, controleren en de sessie wordt geteld. Deze toepassing metrische gegevens kan worden gebruikt om te maken van regels voor automatisch schalen op het niveau van een nauwkeurige en ingesloten als u regels op basis van inzichten die mogelijk van invloed op de klantervaring activeren.

Zie [Wat is Application Insights?](../application-insights/app-insights-overview.md) voor meer informatie over App Insights.


## <a name="scheduled-autoscale"></a>Geplande automatische schaalaanpassing
U kunt ook regels voor automatisch schalen op basis van schema's maken. Deze regels op basis van een planning kunnen u automatisch schalen het aantal VM-exemplaren op tijden vaste. Aan de regels op basis van prestaties mogelijk zijn er een prestatie-impact op de toepassing voordat u de trigger van de regels voor automatisch schalen en de nieuwe VM-exemplaren zijn ingericht. Als u deze aanvraag voorspellen kan, wordt de extra VM-exemplaren zijn ingericht en gereed voor het extra gebruik en de toepassing vraag van klanten.

De volgende voorbeelden zijn scenario's die van het gebruik van regels voor automatisch schalen op basis van een schema profiteren kunnen:

- Automatisch schalen van het aantal VM-exemplaren aan het begin van de werkdag wanneer de vraag van klanten wordt verhoogd. Aan het einde van de werkdag, automatisch schalen in het aantal VM-exemplaren resourcekosten 's nachts minimaliseren wanneer het gebruik laag is.
- Als een afdeling een toepassing sterk op bepaalde onderdelen van de maand of fiscale cyclus gebruikt, automatisch het aantal VM-exemplaren voor de extra vraag te schalen.
- Wanneer er een marketing gebeurtenis, een promotie of een feestdag verkoop, kunt u het aantal VM-exemplaren voor de verwachte vraag automatisch schalen. 


## <a name="next-steps"></a>Volgende stappen
U kunt regels voor automatisch schalen die gebruikmaken van hostgebaseerde metrische gegevens met een van de volgende hulpprogramma's maken:

- [Azure PowerShell](tutorial-autoscale-powershell.md)
- [Azure CLI 2.0](tutorial-autoscale-cli.md)
- [Azure-sjabloon](tutorial-autoscale-template.md)

In dit overzicht beschreven hoe u regels voor automatisch schalen voor horizontaal schalen en vergroten of verkleinen van de *getal* van VM-exemplaren in uw schaal ingesteld. U kunt ook verticaal schalen om te vergroten of verkleinen van het VM-exemplaar *grootte*. Zie voor meer informatie, [verticaal automatisch schalen met Virtual Machine Scale sets](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Zie voor meer informatie over het beheren van uw VM-exemplaren [beheren virtuele-machineschaalsets met Azure PowerShell](virtual-machine-scale-sets-windows-manage.md).

Zie voor informatie over waarschuwingen worden gegenereerd wanneer de functie voor automatisch schalen regels trigger gaat [acties voor automatisch schalen gebruiken voor het verzenden van e-mail en webhook-meldingen van waarschuwingen in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md). U kunt ook [auditlogboeken gebruiken voor het verzenden van e-mail en webhook-meldingen van waarschuwingen in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).
