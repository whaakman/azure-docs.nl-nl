---
title: Verzamelen de bewakingsgegevens in Azure | Microsoft Docs
description: Overzicht van de gegevens die worden verzameld van toepassingen en services in Azure en de hulpprogramma's gebruikt om te analyseren.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/06/2018
ms.author: bwren
ms.openlocfilehash: 35580d71aa2592fa94f42cfdbad3c192acc303c5
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39363982"
---
# <a name="collect-monitoring-data-in-azure"></a>Verzamelen van gegevens in Azure
Dit artikel bevat een overzicht van de gegevens die worden verzameld van toepassingen en services in Azure. Hierin wordt ook de hulpprogramma's die u gebruiken kunt om de gegevens te analyseren. 

## <a name="types-of-monitoring-data"></a>Typen gegevens te controleren
Alle controlegegevens in een van twee fundamentele typen, metrische gegevens of Logboeken past. Elk type heeft verschillende kenmerken en is het meest geschikt voor bepaalde scenario's.

### <a name="metrics"></a>Metrische gegevens
Metrische gegevens zijn numerieke waarden die worden beschreven van een bepaald aspect van een systeem op een bepaald tijdstip. Ze omvatten:

* Verschillende gegevens, inclusief de waarde zelf.
* De tijd die de waarde is verzameld.
* Het type van de meting die de waarde vertegenwoordigt.
* De resource die de waarde die is gekoppeld aan. 

Metrische gegevens worden verzameld met regelmatige tussenpozen al dan niet de waarde wordt gewijzigd. Bijvoorbeeld, u mogelijk verzamelen CPU-gebruik van een virtuele machine per minuut, maar het aantal gebruikers dat is aangemeld bij uw toepassing om de 10 minuten.

Metrische gegevens zijn lichtgewicht en kan in de buurt van realtime scenario's ondersteunen. Ze zijn handig omdat metrische gegevens vaak steekproef kan worden genomen en een waarschuwing kan snel worden geactiveerd met relatief eenvoudige logica op waarschuwingen. U kunt bijvoorbeeld een waarschuwing geactiveerd wanneer een metriek een drempelwaarde overschrijdt. Of u mogelijk een waarschuwing geactiveerd wanneer een bepaalde waarde bereikt die is het verschil tussen twee metrische gegevens.

Afzonderlijke metrische gegevens bieden doorgaans weinig inzicht in hun eigen. Ze bieden een enkele waarde zonder context dan vergelijking met een enkelvoudige drempelwaarde. Ze zijn nuttig in combinatie met andere metrische gegevens om patronen en trends te identificeren of in combinatie met Logboeken die een context om bepaalde waarden bieden. 

Bijvoorbeeld, kan een bepaald aantal gebruikers in uw toepassing op een bepaald moment u vertellen weinig over de status van de toepassing. Maar een plotselinge afname in gebruikers, aangegeven met meerdere waarden van de dezelfde metrische gegevens, kan duiden op een probleem. Overmatige uitzonderingen die door de toepassing veroorzaakt en aangegeven door een afzonderlijke metrische gegevens, mogelijk een probleem met toepassing die wordt veroorzaakt door de vervolgkeuzelijst identificeren. Gebeurtenissen die de toepassing maakt voor het identificeren van fouten in de bijbehorende onderdelen kunt u de hoofdoorzaak identificeren.

Waarschuwingen op basis van de logboeken zijn niet optimaal reageert als waarschuwingen op basis van metrische gegevens, maar ze kunnen ook complexere logica bevatten. U kunt een waarschuwing op basis van de resultaten van een query waarmee complexe analyse van de gegevens uit meerdere bronnen maken.

### <a name="logs"></a>Logboeken
Logboeken bevatten verschillende soorten gegevens die zijn ingedeeld in records met verschillende sets van eigenschappen voor elk type. Logboeken kunnen numerieke waarden, zoals metrische gegevens bevatten, maar bevatten doorgaans tekstgegevens met gedetailleerde beschrijvingen. Ze verder verschillen van metrische gegevens in dat ze in de structuur ervan variëren en worden vaak niet verzameld met regelmatige intervallen.

Een gemeenschappelijk type logboekvermelding is een gebeurtenis. Gebeurtenissen worden sporadisch verzameld. Ze worden gemaakt door een toepassing of service en zijn meestal voldoende informatie gegeven om een volledige context te bieden op hun eigen. Een gebeurtenis kan bijvoorbeeld duiden dat een bepaalde resource is gemaakt of gewijzigd, een nieuwe host gestart als reactie op toegenomen verkeer, of er is een fout gedetecteerd in een toepassing.

Logboeken zijn vooral handig voor het combineren van gegevens uit diverse bronnen, voor complexe analyse en trends na verloop van tijd. Omdat de indeling van de gegevens variëren kan, kunnen toepassingen aangepaste logboeken kunnen maken met behulp van de structuur die ze nodig hebben. Metrische gegevens kunnen zelfs worden gerepliceerd in Logboeken om ze te combineren met andere bewakingsgegevens voor trending en andere data-analyse.


## <a name="monitoring-tools-in-azure"></a>Controleprogramma's in Azure
Bewaking van gegevens in Azure worden verzameld en geanalyseerd door de volgende bronnen.

### <a name="azure-monitor"></a>Azure Monitor
Metrische gegevens van Azure-resources en toepassingen worden verzameld in Azure Monitor. Metrische gegevens is geïntegreerd in de pagina's in de Azure-portal voor Azure-resources. Voor virtuele machines weergegeven grafieken van deze metrische gegevens als het CPU- en netwerkgebruik voor de geselecteerde machine. 

U kunt ook gegevens analyseren met behulp van [Metrics Explorer](../monitoring-and-diagnostics/monitoring-metric-charts.md), die de waarden van meerdere metrische gegevens gedurende een periode grafieken. U kunt de grafieken interactief weergeven of vastmaken aan een dashboard om deze met andere visualisaties weer te geven. U kunt ook metrische gegevens ophalen met behulp van de [Azure REST-API bewaken](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

Zie voor meer informatie over de metrische gegevens die verschillende soorten Azure-resources verzamelen [bronnen van bewakingsgegevens in Azure](monitoring-data-sources.md). 

![Metrics Explorer](media/monitoring-data-collection/metrics-explorer.png)


### <a name="activity-log"></a>Activiteitenlogboek 
De [Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) slaat logboeken over de configuratie en status van Azure-services. U kunt activiteit Log Explorer gebruiken om deze logboeken in Azure portal weer te geven, maar ze vaak [gekopieerd naar Azure Log Analytics](../log-analytics/log-analytics-activity.md) kunnen worden geanalyseerd met andere logboekgegevens.

U kunt activiteit Log Explorer gebruiken om het activiteitenlogboek filteren om te voldoen aan bepaalde criteria weer te geven. De meeste resources hebben ook een **activiteitenlogboek** optie in hun menu in de Azure-portal. Activiteit Log Explorer gefilterd voor de resource wordt weergegeven. U kunt ook activiteitenlogboeken ophalen met behulp van de [REST-API voor Azure-bewaking](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Activiteit Log Explorer](media/monitoring-data-collection/activity-log-explorer.png)


### <a name="log-analytics"></a>Log Analytics
Log Analytics biedt een algemeen gegevensplatform voor beheer in Azure. Het is de primaire service die wordt gebruikt voor opslag en analyse van Logboeken in Azure. Het verzamelt gegevens uit diverse bronnen, met inbegrip van agents op virtuele machines, oplossingen voor het beheer en Azure-resources. U kunt gegevens kopiëren van andere bronnen, met inbegrip van metrische gegevens en het activiteitenlogboek, om te maken van een volledige centrale opslagplaats van de gegevens te controleren.

Log Analytics is een uitgebreide querytaal voor het analyseren van de gegevens die worden verzameld. U kunt [portals voor zoeken in logboeken](../log-analytics/log-analytics-log-search-portals.md) voor het interactief schrijven en testen van query's en analyseren van de resultaten ervan. U kunt ook [weergaven maken](../log-analytics/log-analytics-view-designer.md) te visualiseren de resultaten van zoekopdrachten in Logboeken of plakt u de resultaten van een query rechtstreeks op een Azure-dashboard.  

Beheeroplossingen bevatten weergaven en zoekopdrachten in Logboeken in Log Analytics voor het analyseren van de gegevens die zij verzamelen. Andere services, zoals Azure Application Insights, gegevens opslaan in Log Analytics en bieden extra hulpprogramma's voor analyse.  

![Logboeken](media/monitoring-data-collection/logs.png)

### <a name="application-insights"></a>Application Insights
Application Insights verzamelt telemetrie voor webtoepassingen die zijn geïnstalleerd op een aantal verschillende platformen. De gegevens worden opgeslagen in Azure Monitor en Log Analytics. En biedt een uitgebreide set hulpprogramma's voor het analyseren en visualiseren van de gegevens. Deze mogelijkheden kunnen u gebruikmaken van een gemeenschappelijke set services zoals waarschuwingen, zoekopdrachten in Logboeken en dashboards die u voor andere bewaking gebruiken.


![Application Insights](media/monitoring-data-collection/app-insights.png)

### <a name="service-map"></a>Serviceoverzicht
Serviceoverzicht biedt een visuele representatie van virtuele machines met hun processen en afhankelijkheden. Wordt de meeste van deze gegevens opgeslagen in Log Analytics, zodat u ze met andere gegevens analyseren kunt. De Serviceoverzicht-console worden ook gegevens opgehaald van Log Analytics om deze in de context van de virtuele machine die wordt geanalyseerd.

![Serviceoverzicht](media/monitoring-data-collection/service-map.png)


## <a name="transferring-monitoring-data"></a>Gegevens overbrengen

### <a name="metrics-to-logs"></a>Metrische gegevens in Logboeken
U kunt metrische gegevens in Log Analytics om uit te voeren complexe analyse met andere gegevenstypen met behulp van de uitgebreide querytaal repliceren. U kunt ook logboekgegevens voor langere tijd dan metrische gegevens, waarmee u om uit te voeren van de trends na verloop van tijd kunt bewaren. Wanneer metrische gegevens of geen prestatiegegevens worden opgeslagen in Log Analytics, die gegevens als een logboekbestand fungeert. Metrische gegevens voor de ondersteuning van bijna realtime analyse en waarschuwingsdoeleinden tijdens het gebruik van Logboeken voor trending en -analyse met andere gegevens gebruiken.

Informatie voor het verzamelen van metrische gegevens van Azure-resources op [Azure verzamelen van Logboeken en metrische gegevens voor gebruik in Log Analytics](../log-analytics/log-analytics-azure-storage.md). Begeleiding voor het verzamelen van metrische gegevens voor resources van Azure PaaS-resources op [verzamelen van metrische gegevens voor Azure PaaS-resources configureren met Log Analytics](../log-analytics/log-analytics-collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Logboeken naar metrische gegevens
Zoals eerder beschreven, zijn metrische gegevens sneller reageren dan Logboeken, zodat u waarschuwingen met een lagere latentie en tegen lagere kosten maken kunt. Log Analytics verzamelt een aanzienlijke hoeveelheid numerieke gegevens die zijn geschikt voor metrische gegevens, maar niet zijn opgeslagen in Azure Monitor. 

Een veelvoorkomend voorbeeld is de prestatiegegevens die worden verzameld van agents en oplossingen voor beheer. Sommige van deze waarden kunnen worden gekopieerd naar Azure Monitor, waar ze beschikbaar voor waarschuwingen en voor analyse met Metrics Explorer zijn.

De uitleg van deze functie is beschikbaar op [sneller metrische waarschuwingen voor logboeken nu beschikbaar in de beperkte openbare preview](https://azure.microsoft.com/blog/faster-metric-alerts-for-logs-now-in-limited-public-preview/). De lijst met waarden ondersteuning is beschikbaar op [ondersteunde metrische gegevens en het maken van methoden voor nieuwe metrische waarschuwingen](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md).

### <a name="event-hubs"></a>Event Hubs
Naast het gebruik van de hulpprogramma's in Azure voor het analyseren van gegevens, is het raadzaam dit doorsturen naar een extern hulpprogramma, zoals een security information en event management (SIEM) product. Deze doorsturen wordt gewoonlijk uitgevoerd via [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/). 

U kunt hulp krijgen voor de verschillende soorten bewakingsgegevens op [Stream Azure-bewakingsgegevens naar een event hub voor gebruik door een extern hulpprogramma](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [bewakingsgegevens beschikbaar](monitoring-data-sources.md) voor verschillende resources in Azure. 