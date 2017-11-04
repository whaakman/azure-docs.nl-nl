---
title: Overzicht van automatisch geschaald met virtuele Azure-machine-schaalsets | Microsoft Docs
description: Meer informatie over de verschillende manieren waarop u kunt een virtuele machine van Azure schaal ingesteld op basis van prestaties of op een vaste planning automatisch schalen
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: d29a3385-179e-4331-a315-daa7ea5701df
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/19/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 868523a3aca441a47218297be2ce9f9e46dd84a1
ms.sourcegitcommit: 2d1153d625a7318d7b12a6493f5a2122a16052e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/20/2017
---
# <a name="overview-of-autoscale-with-azure-virtual-machine-scale-sets"></a>Overzicht van automatisch schalen binnen de schaal van de virtuele machine van Azure wordt ingesteld
Een virtuele machine van Azure scale set kan automatisch vergroten of verkleinen van het aantal VM-exemplaren die uw toepassing uitvoeren. Dit gedrag geautomatiseerde en elastische vermindert de overhead voor het controleren en optimaliseren van de prestaties van uw toepassing. U maken regels die de minimaal toegestane prestaties voor een positieve klantervaring definiëren. Wanneer deze gedefinieerde drempels wordt voldaan, maatregelen regels voor automatisch schalen die nemen om aan te passen, de capaciteit van uw scale set. U kunt ook gebeurtenissen automatisch vergroten of verkleinen de capaciteit van uw scale set op tijden vaste plannen. In dit artikel biedt een overzicht van welke prestaties metrische gegevens beschikbaar zijn en welke acties automatisch schalen kunt uitvoeren.


## <a name="benefits-of-autoscale"></a>Voordelen van het automatisch schalen
Als uw toepassing vraag toeneemt stelt de belasting van de VM-exemplaren in uw scale toeneemt. Als deze toegenomen belasting consistent is, in plaats van slechts een korte vraag, kunt u regels voor automatisch schalen voor een verhoging van het aantal VM-exemplaren in de schaalset configureren.

Wanneer deze VM-exemplaren zijn gemaakt en uw toepassingen worden geïmplementeerd, wordt de schaalaanpassingsset begint met het distribueren van verkeer via de load balancer. U bepaalt welke metrische gegevens moeten worden bewaakt, zoals CPU of geheugen, hoe lang het laden van toepassingen moet voldoen aan een bepaalde drempelwaarde en instellen hoeveel exemplaren van virtuele machine toevoegen aan de schaal.

Op een 's avonds of het weekend en kan uw aanvraag toepassing afnemen. Als deze minder belasting gedurende een periode consistent is, kunt u regels voor automatisch schalen Verminder het aantal VM-exemplaren in de schaalset configureren. Deze actie schaal in reduceert de kosten voor het uitvoeren van uw scale instellen als u alleen het aantal exemplaren is vereist om te voldoen aan de huidige aanvraag uitvoeren.


## <a name="use-host-based-metrics"></a>Gebruik van metrische gegevens op de host
U kunt regels voor automatisch schalen die ingebouwde host metrische gegevens beschikbaar maken van uw VM-exemplaren. Host metrische gegevens krijgt u inzicht in de prestaties van de VM-exemplaren in een schaal instellen zonder te hoeven installeren of configureren van extra agenten en verzamelen van gegevens. Regels voor automatisch schalen die gebruikmaken van deze metrische gegevens kunnen schalen uit of in het aantal VM-exemplaren in reactie op CPU-gebruik, geheugen-aanvraag of toegang tot de schijf.

Regels voor automatisch schalen die gebruikmaken van hostgebaseerde metrische gegevens, kunnen worden gemaakt met een van de volgende hulpprogramma's:

- [Azure Portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md)
- [Azure CLI 2.0](virtual-machine-scale-sets-autoscale-cli.md)

Als u wilt maken voor automatisch schalen regels die gebruikmaken van meer gedetailleerde maatstaven voor prestaties, kunt u [installeren en configureren van de extensie Azure diagnostics](#in-guest-vm-metrics-with-the-azure-diagnostics-extension) op VM-exemplaren of [configureren van uw toepassing gebruik App Insights](#application-level-metrics-with-app-insights).

Regels voor automatisch schalen die gebruikmaken van de host gebaseerde metrische gegevens, metrische gegevens in de Gast-VM met de extensie Azure diagnostische en de App Insights kunt de volgende configuratie-instellingen gebruiken.

### <a name="metric-sources"></a>Metrische bronnen
Regels voor automatisch schalen kunnen gebruiken om metrische gegevens van een van de volgende bronnen:

| Metrische bron        | Gebruiksvoorbeeld                                                                                                                     |
|----------------------|------------------------------------------------------------------------------------------------------------------------------|
| Huidige schaalset    | Voor de host gebaseerde metrische gegevens die niet hoeven te worden geïnstalleerd of geconfigureerd als u meer agents.                                  |
| Storage-account      | De diagnostische Azure-extensie schrijft prestatiegegevens naar Azure-opslag wordt vervolgens geconsumeerd voor het activeren van regels voor automatisch schalen. |
| Service Bus-wachtrij    | Uw toepassing of andere onderdelen kunnen verzenden berichten op een Azure Service Bus-wachtrij voor de Triggerregels.                   |
| Application Insights | Een pakket instrumentation is geïnstalleerd in uw toepassing waarmee streams metrische gegevens rechtstreeks vanuit de app.                         |


### <a name="autoscale-rule-criteria"></a>De criteria voor automatisch schalen
De volgende host gebaseerde metrische gegevens zijn beschikbaar voor gebruik bij het maken van regels voor automatisch schalen. Als u de diagnostische Azure-extensie of de App Insights gebruiken, definieert u welke metrische gegevens om te controleren en te gebruiken met regels voor automatisch schalen.

| Metrische naam               |
|---------------------------|
| CPU-percentage            |
| Het netwerk                |
| Uit het netwerk               |
| Schijf lezen Bytes           |
| Schijf schrijven Bytes          |
| Gelezen bewerkingen per seconde  |
| Geschreven bewerkingen per seconde |
| CPU-tegoed resterend     |
| CPU-tegoed verbruikt      |

Wanneer u regels voor automatisch schalen voor het bewaken van een metriek maakt, bekijken de regels een van de volgende metrische gegevens aggregatie acties:

| Samenvoegingstype |
|------------------|
| Gemiddelde          |
| Minimum          |
| Maximum          |
| Totaal            |
| laatste             |
| Aantal            |

De regels voor automatisch schalen die zijn vervolgens geactiveerd wanneer de metrische gegevens worden vergeleken met de opgegeven drempelwaarde met een van de volgende operators:

| Operator                 |
|--------------------------|
| Groter dan             |
| Groter dan of gelijk aan |
| Kleiner dan                |
| Kleiner dan of gelijk aan    |
| Gelijk aan                 |
| Is niet gelijk aan             |


### <a name="actions-when-rules-trigger"></a>Acties als regels activeren
Wanneer een regel voor automatisch schalen triggers wordt uw scale set kan automatisch schalen in een van de volgende manieren:

| Schaalaanpassing     | Gebruiksvoorbeeld                                                                                                                               |
|---------------------|----------------------------------------------------------------------------------------------------------------------------------------|
| Aantallen per verhogen   | Een vast aantal VM-instanties maken. Dit is handig in schaalsets met een kleiner aantal virtuele machines.                                           |
| Percentage door | Een verhoging van percentage op basis van VM-exemplaren. Geschikt voor grootschalige wordt ingesteld wanneer een vaste verhoging niet merkbaar de prestaties mogelijk verbeterd. |
| Aantal te verhogen   | Maak zoals veel exemplaren van de VM nodig zijn voor een gewenste maximumhoeveelheid bereiken.                                                            |
| Verklein het aantal moet worden   | Een vast aantal VM-exemplaren te verwijderen. Dit is handig in schaalsets met een kleiner aantal virtuele machines.                                           |
| Percentage door verkleinen | Een afname op basis van een percentage van de VM-exemplaren. Geschikt voor grootschalige wordt ingesteld wanneer een vaste verhoging mogen niet aanzienlijk verlagen brongebruik en kosten. |
| Verklein het aantal moet worden   | Worden verwijderd omdat er veel exemplaren van de VM nodig zijn voor een gewenste minimumhoeveelheid bereiken.                                                            |


## <a name="in-guest-vm-metrics-with-the-azure-diagnostics-extension"></a>Metrische gegevens in de Gast-VM met de extensie Azure diagnostics
De extensie Azure diagnostics is een agent die wordt uitgevoerd binnen een VM-instantie. De agent wordt bewaakt en slaat maatstaven voor prestaties in Azure storage. Deze maatstaven voor prestaties bevatten meer gedetailleerde informatie over de status van de virtuele machine, zoals *AverageReadTime* voor schijven of *PercentIdleTime* voor CPU. U kunt regels voor automatisch schalen op basis van een meer gedetailleerde kennis van de prestaties van de VM, niet alleen het percentage CPU-verbruik voor informatie over het gebruik of geheugen.

Voor het gebruik van de extensie Azure diagnostics, moet u Azure storage-accounts maken voor uw VM-exemplaren, de Azure diagnostics-agent installeren en configureren van de virtuele machines naar de stroom specifieke prestatiemeteritems naar het opslagaccount.

Zie voor meer informatie de artikelen voor het inschakelen van de diagnostische Azure-extensie op een [Linux-VM](../virtual-machines/linux/diagnostic-extension.md) of [Windows-VM](../virtual-machines/windows/ps-extensions-diagnostics.md).


## <a name="application-level-metrics-with-app-insights"></a>Metrische gegevens op toepassingsniveau met App Insights
U kunt Application Insights gebruiken voor het verkrijgen van meer zichtbaarheid in op de prestaties van uw toepassingen. U installeren een kleine instrumentation pakket in uw toepassing die de app wordt bewaakt en telemetrie verzendt naar Azure. U kunt metrische gegevens zoals de reactietijden van uw toepassing, de prestaties van de load pagina bewaken en de sessie wordt geteld. Deze toepassing metrische gegevens kan worden gebruikt voor het maken van regels voor automatisch schalen op een gedetailleerde en ingesloten niveau omdat u regels op basis van bruikbare inzichten die mogelijk van invloed op de gebruikerservaring activeren.

Zie [Wat is Application Insights?](../application-insights/app-insights-overview.md) voor meer informatie over App Insights.


## <a name="scheduled-autoscale"></a>Geplande automatisch schalen
U kunt ook regels voor automatisch schalen op basis van schema's maken. Deze regels op basis van een planning kunnen u automatisch het aantal VM-exemplaren op tijden vaste schaal. Er is mogelijk invloed op de prestaties van de toepassing voordat u de trigger van de regels voor automatisch schalen met regels op basis van prestaties en nieuwe VM-exemplaren zijn ingericht. Als u deze aanvraag verwacht kunt, wordt de aanvullende VM-exemplaren zijn ingericht en gereed voor de vraag van de aanvullende klanten gebruik en de toepassing.

De volgende voorbeelden zijn scenario's waarvoor het gebruik van regels voor automatisch schalen op basis van een planning:

- Automatisch schalen het aantal VM-exemplaren aan het begin van de dag wanneer verhoogt de vraag van klanten. Aan het einde van de werkdag, automatisch schalen in het aantal exemplaren van de VM's nachts resourcekosten minimaliseren wanneer een toepassing gebruik laag is.
- Als een afdeling in een toepassing sterk op bepaalde delen van de maand of fiscale cyclus, automatisch het aantal VM-instanties van de aanvullende vereisten voor de schaal.
- Wanneer er een gebeurtenis marketing, promotie of vakantiedag verkoop, kunt u het aantal VM-instanties van tevoren verwachte vraag automatisch schalen. 


## <a name="next-steps"></a>Volgende stappen
U kunt regels voor automatisch schalen die gebruikmaken van metrische gegevens op basis van een host met een van de volgende hulpprogramma's kunt maken:

- [Azure Portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md)
- [Azure CLI 2.0](virtual-machine-scale-sets-autoscale-cli.md)

Dit overzicht beschreven hoe u regels voor automatisch schalen horizontaal schalen en vergroten of verkleinen met de *getal* van VM-exemplaren in uw scale ingesteld. Kunt u ook de schaal verticaal vergroten of verkleinen van de VM-instantie *grootte*. Zie voor meer informatie [verticale automatisch geschaald met de virtuele Machine-schaalsets](virtual-machine-scale-sets-vertical-scale-reprovision.md).

Zie voor meer informatie over het beheren van uw VM-exemplaren [beheren virtuele-machineschaalsets met Azure PowerShell](virtual-machine-scale-sets-windows-manage.md).

Zie voor meer informatie over hoe waarschuwingen worden gegenereerd wanneer uw automatisch schalen regels trigger, [acties automatisch schalen gebruiken voor het verzenden van e-mail en -webhook-waarschuwingsmeldingen in de Azure-Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md). U kunt ook [controlelogboeken voor gebruik met het verzenden van e-mail en webhook-waarschuwingsmeldingen in de Azure-Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md).