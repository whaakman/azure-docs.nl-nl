---
title: Azure Monitor-gegevensplatform | Microsoft Docs
description: Bewaking van de gegevens die zijn verzameld door Azure Monitor is verdeeld in de logboeken die worden gebruikt voor geavanceerde analyse en metrische gegevens die zijn lichtgewicht en kan in de buurt van realtime scenario's ondersteunen.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/26/2019
ms.author: bwren
ms.openlocfilehash: 8883c1e7f2874e1e2e61b8eca122f2ec294c7849
ms.sourcegitcommit: d83fa82d6fec451c0cb957a76cfba8d072b72f4f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58862060"
---
# <a name="azure-monitor-data-platform"></a>Azure Monitor-gegevensplatform

Verzamelen van operationele gegevens uit elke laag en elk onderdeel van het gedistribueerde systeem observability inschakelen via de hedendaagse complexe computeromgevingen uitvoeren van gedistribueerde toepassingen die afhankelijk van zowel de cloud als on-premises services zijn, worden vereist. U moet kunt diepe inzichten uitvoeren op deze gegevens en deze te consolideren in een enkel glazen met verschillende perspectieven ter ondersteuning van het grote aantal belanghebbenden in uw organisatie.

[Azure Monitor](../overview.md) verzamelt en verzamelt gegevens uit een groot aantal bronnen in een algemeen gegevensplatform waar deze kan worden gebruikt voor analyse, visualisatie en waarschuwingen. Het biedt een consistente ervaring op gegevens uit meerdere bronnen, waardoor u veel inzicht in uw bewaakte resources en zelfs met gegevens van andere services die hun gegevens opslaan in Azure Monitor.


![Overzicht van Azure Monitor](media/data-platform/overview.png)

## <a name="observability-data-in-azure-monitor"></a>Observability gegevens in Azure Monitor
Metrische gegevens, logboeken en gedistribueerde traceringen worden vaak aangeduid als de drie pijlers van observability. Dit zijn de verschillende soorten gegevens die een hulpprogramma voor bewaking moet verzamelen en analyseren voor voldoende observability van een bewaakt systeem. Observability kan worden bereikt door het correleren van gegevens uit meerdere onderdelen en aggregeren van gegevens voor de volledige set van resources die worden bewaakt. Omdat gegevens uit meerdere bronnen samen worden opgeslagen in Azure Monitor, worden de gegevens kunnen worden gecorreleerd en geanalyseerd op basis van een gemeenschappelijke set hulpprogramma's. Deze ook een koppeling tussen van gegevens op meerdere Azure-abonnementen en tenants, naast het hosten van gegevens voor andere services.

Azure-resources genereren een aanzienlijke hoeveelheid gegevens te controleren. Azure Monitor consolideert deze gegevens, samen met de bewaking van gegevens uit andere bronnen bij een platform voor metrische gegevens of Logboeken. Elk is geoptimaliseerd voor bepaalde bewakingsscenario's en elk biedt ondersteuning voor verschillende functies in Azure Monitor. Functies zoals data-analyse, visualisaties of waarschuwingen moeten u weten wat de verschillen, zodat u uw vereiste scenario op de meest efficiënte en voordelige manier implementeren kunt. Inzichten in Azure Monitor, zoals [Application Insights](../app/app-insights-overview.md) of [Azure Monitor voor virtuele machines](../insights/vminsights-overview.md) hebt analyseprogramma's waarmee u zich kunt richten op het opgegeven controle scenario zonder dat u begrijpt de verschillen tussen de twee typen gegevens. 


### <a name="metrics"></a>Metrische gegevens
[Metrische gegevens](data-platform-metrics.md) zijn numerieke waarden die een bepaald aspect van een systeem op een bepaald punt in tijd beschrijven. Ze worden verzameld met regelmatige tussenpozen en worden aangeduid met een tijdstempel, een naam, een waarde en een of meer definiëren labels. Metrische gegevens kunnen worden samengevoegd met behulp van verschillende algoritmen, in vergelijking met andere metrische gegevens en geanalyseerd voor trends na verloop van tijd. 

Metrische gegevens in Azure Monitor worden opgeslagen in een time series-database die is geoptimaliseerd voor het analyseren van gegevens met tijdstempel. Dit worden metrische gegevens, met name geschikt voor waarschuwingen en snelle detectie van problemen. Ze kunnen laat u weten hoe uw systeem wordt uitgevoerd, maar meestal moeten worden gecombineerd met logboeken naar de hoofdoorzaak van problemen identificeren.

Metrische gegevens zijn beschikbaar voor interactieve analyses in Azure portal met [Metrics Explorer](../app/metrics-explorer.md). Ze kunnen worden toegevoegd aan een [Azure-dashboard](../learn/tutorial-app-dashboards.md) voor visualisatie in combinatie met andere gegevens en gebruikt voor bijna realtime [waarschuwingen](alerts-metric.md).

Meer informatie over Azure Monitor Metrics, met inbegrip van de bronnen van de gegevens in [metrische gegevens in Azure Monitor](data-platform-metrics.md).

### <a name="logs"></a>Logboeken
[Logboeken](data-platform-logs.md) zijn gebeurtenissen die hebben plaatsgevonden binnen het systeem. Ze kunnen verschillende soorten gegevens bevatten en mag worden gestructureerd of vrije tekst met een tijdstempel. Ze kunnen sporadisch worden gemaakt als gebeurtenissen in de omgeving logboekvermeldingen genereren en een systeem zwaar belast wordt doorgaans meer logboekvolume gegenereerd.

Logboeken in Azure Monitor worden opgeslagen in een Log Analytics-werkruimte die gebaseerd op [Azure Data Explorer](/azure/data-explorer/) waarmee u een krachtige analyse-engine en [uitgebreide querytaal](/azure/kusto/query/). Logboeken meestal voldoende informatie gegeven om bieden volledige context van het probleem worden geïdentificeerd en zijn nuttig voor het identificeren van hoofdmap geval van problemen.

> [!NOTE]
> Het is belangrijk onderscheid maken tussen Azure Monitor-logboeken en bronnen van logboekgegevens in Azure. Bijvoorbeeld, abonnement op gebeurtenissen in Azure worden geschreven naar een [activiteitenlogboek](activity-logs-overview.md) die u kunt bekijken in het menu Azure Monitor. De meeste resources schrijft operationele gegevens naar een [diagnostisch logboek](diagnostic-logs-overview.md) die u kunt doorsturen naar verschillende locaties. Logboeken in Azure Monitor is een platform voor logboeken die worden verzameld van activiteitenlogboeken en diagnostische logboeken samen met andere bewakingsgegevens voor geavanceerde analyse voor de volledige set van resources.


 U kunt werken met [query's bijgehouden](../log-query/log-query-overview.md) interactief met [Log Analytics](../log-query/portals.md) in Azure portal of toevoegen van de resultaten naar een [Azure-dashboard](../learn/tutorial-app-dashboards.md) voor visualisatie in combinatie met andere gegevens. U kunt ook maken [waarschuwingen voor activiteitenlogboeken](alerts-log.md) die een waarschuwing op basis van de resultaten van een schema-query wordt geactiveerd.

Meer informatie over Azure Monitor-logboeken met inbegrip van gegevens in hun [registreert in Azure Monitor](data-platform-logs.md).

### <a name="distributed-traces"></a>Gedistribueerde traceringen
Traceringen worden reeks gerelateerde gebeurtenissen die de aanvraag van een gebruiker via een gedistribueerd systeem volgen. Ze kunnen worden gebruikt om te bepalen het gedrag van toepassingscode en de prestaties van andere transacties. Terwijl Logboeken vaak door de afzonderlijke onderdelen van een gedistribueerd systeem gemaakt wordt, meet een tracering de werking en prestaties van uw toepassing voor de volledige set van onderdelen.

Gedistribueerde tracering in Azure Monitor is ingeschakeld met de [Application Insights-SDK](../app/distributed-tracing.md), en traceringsgegevens worden opgeslagen met een andere toepassing logboekgegevens die zijn verzameld door Application Insights. Dit maakt ze beschikbaar voor de dezelfde analyseprogramma's als andere logboekgegevens met inbegrip van Logboeken-query's, -dashboards en -waarschuwingen.

Meer informatie over gedistribueerde tracering op [wat is er gedistribueerde tracering?](../app/distributed-tracing.md).


## <a name="compare-azure-monitor-metrics-and-logs"></a>Vergelijk logboeken en metrische gegevens met Azure Monitor

De volgende tabel vergelijkt de metrische gegevens en Logboeken in Azure Monitor.

| Kenmerk  | Metrische gegevens | Logboeken |
|:---|:---|:---|
| Voordelen | Lichtgewicht en geschikt voor bijna realtime scenario's zoals waarschuwingen. Ideaal voor de snelle detectie van problemen. | Geanalyseerd met krachtige querytaal. Ideaal voor grondige analyse en het identificeren van de hoofdoorzaak te achterhalen. |
| Gegevens | Alleen numerieke waarden | Tekst- of numerieke gegevens |
| structuur | De standaardset van eigenschappen zoals tijd van de steekproef, resource, die worden bewaakt, een numerieke waarde. Sommige metrische gegevens over bevatten meerdere dimensies voor verdere definitie. | Unieke set eigenschappen, afhankelijk van het logboektype. |
| Verzameling | Verzameld met regelmatige intervallen. | Kan worden verzameld sporadisch als gebeurtenissen een record die activeren moet worden gemaakt. |
| Weergeven in Azure portal | Metrics Explorer | Log Analytics |
| Gegevensbronnen opnemen | Platform metrische gegevens verzameld van Azure-resources.<br>Toepassingen bewaakt door Application Insights.<br>Aangepaste gedefinieerd door de toepassing of API. | Toepassings- en diagnostische logboeken.<br>Bewakingsoplossingen.<br>Agents en VM-extensies.<br>Toepassing aanvragen en uitzonderingen.<br>Azure Security Center.<br>Gegevensverzamelaar-API. |

## <a name="collect-monitoring-data"></a>Verzamelen van gegevens
Verschillende [gegevensbronnen voor Azure Monitor](data-sources.md) wordt geschreven naar een Log Analytics-werkruimte (Logboeken) of de database in Azure Monitor metrics (metrische gegevens) of beide. Sommige bronnen wordt rechtstreeks naar deze gegevensarchieven schrijven, terwijl anderen kunnen naar een andere locatie, zoals Azure storage schrijven en sommige configuratie voor het vullen van Logboeken of metrische gegevens vereisen. 

Zie [metrische gegevens in Azure Monitor](data-platform-metrics.md) en [registreert in Azure Monitor](data-platform-logs.md) voor een overzicht van verschillende gegevensbronnen die elk type.


## <a name="stream-data-to-external-systems"></a>Stream-gegevens met externe systemen
Naast het gebruik van de hulpprogramma's in Azure voor het analyseren van gegevens, mogelijk hebt u een vereiste dit doorsturen naar een extern hulpprogramma, zoals een security information en event management (SIEM) product. Doorsturen van dit gebeurt meestal rechtstreeks vanuit de bewaakte resources via [Azure Event Hubs](/azure/event-hubs/). Sommige gegevensbronnen kunnen worden geconfigureerd voor het verzenden van gegevens rechtstreeks naar een event hub, terwijl u een ander proces, zoals een logische App gebruiken kunt om de vereiste gegevens te halen. Zie [Stream Azure-bewakingsgegevens naar een event hub voor gebruik door een extern hulpprogramma](stream-monitoring-data-event-hubs.md) voor meer informatie.



## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [metrische gegevens in Azure Monitor](data-platform-metrics.md).
- Meer informatie over [registreert in Azure Monitor](data-platform-logs.md).
- Meer informatie over de [bewakingsgegevens beschikbaar](data-sources.md) voor verschillende resources in Azure.
