---
title: Verzamelen bewakingsgegevens in Azure | Microsoft Docs
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
ms.openlocfilehash: efbf0907f3ed75957159c38a536bd31e88a0dbb3
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213296"
---
# <a name="collecting-monitoring-data-in-azure"></a>Verzamelen van bewakingsgegevens in Azure
Dit artikel bevat een overzicht van de gegevens die worden verzameld van toepassingen en services in Azure en de hulpprogramma's gebruikt om te analyseren. 

## <a name="types-of-monitoring-data"></a>Typen gegevens te controleren
Alle controlegegevens in een van twee fundamentele typen, metrische gegevens of Logboeken past. Elk type heeft verschillende kenmerken en is het meest geschikt voor bepaalde scenario's zoals hieronder wordt beschreven.

### <a name="metrics"></a>Metrische gegevens
Metrische gegevens zijn numerieke waarden die een bepaald aspect van een systeem op een bepaald punt in tijd beschrijven. Ze bevatten verschillende gegevens, inclusief de waarde zelf, de tijd die de waarde die is verzameld, het type meting vertegenwoordigt de waarde en de specifieke bron die de waarde die is gekoppeld aan. Metrische gegevens worden verzameld met regelmatige tussenpozen al dan niet de waarde wordt gewijzigd. U kunt bijvoorbeeld CPU-gebruik verzamelen van een virtuele machine die elke minuut of het aantal gebruikers die zijn aangemeld bij uw toepassing om de 10 minuten.

Metrische gegevens zijn lichtgewicht en kan in de buurt van realtime scenario's ondersteunen. Ze zijn bijzonder nuttig voor waarschuwingen over metrische gegevens vaak steekproef kan worden genomen en een waarschuwing kan snel worden geactiveerd met relatief eenvoudige logica. U kunt bijvoorbeeld een waarschuwing geactiveerd wanneer een metriek een drempelwaarde overschrijdt of een waarschuwing geactiveerd wanneer een bepaalde waarde bereikt die is het verschil tussen de waarde van de twee metrische gegevens.

Afzonderlijke metrische gegevens bieden doorgaans weinig inzicht in hun eigen. Ze bieden een enkele waarde zonder context dan vergelijking met een enkelvoudige drempelwaarde. Ze zijn echter waardevolle in combinatie met andere metrische gegevens om patronen en trends te identificeren of in combinatie met Logboeken die een context om bepaalde waarden bieden. Bijvoorbeeld, kan een bepaald aantal gebruikers in uw toepassing op een bepaald moment u vertellen weinig over de status van de toepassing. Een plotselinge afname in gebruikers echter aangegeven door meerdere waarden van de dezelfde metrische gegevens kan duiden op een probleem. Overmatige uitzonderingen die door de toepassing veroorzaakt en aangegeven door een afzonderlijke metrische waarde kunnen een toepassingsprobleem veroorzaakt door de vervolgkeuzelijst identificeren. Gebeurtenissen die zijn gemaakt door de toepassing in het bijzonder fout identificeren onderdelen van de toepassing kunnen u helpen bij het identificeren van de hoofdoorzaak te achterhalen.

Waarschuwingen op basis van de logboeken zijn niet optimaal reageert als waarschuwingen op basis van metrische gegevens, maar ze kunnen ook complexere logica bevatten. U kunt een waarschuwing op basis van de resultaten van een query waarmee complexe analyse van de gegevens uit meerdere bronnen maken.

### <a name="logs"></a>Logboeken
Logboeken bevatten verschillende soorten gegevens die zijn ingedeeld in records met verschillende sets van eigenschappen voor elk type. Logboeken kunnen numerieke waarden, zoals metrische gegevens bevatten, maar bevatten doorgaans tekstgegevens met gedetailleerde beschrijvingen. Ze verder verschillen van metrische gegevens in dat ze in de structuur ervan variëren en worden vaak niet verzameld met regelmatige intervallen.

Een gemeenschappelijk type logboekvermelding is een gebeurtenis. Gebeurtenissen sporadisch worden verzameld als ze zijn gemaakt door een toepassing of service en zijn meestal voldoende informatie gegeven om een volledige context te bieden op hun eigen.  Een gebeurtenis kan bijvoorbeeld duiden dat een bepaalde resource is gemaakt of gewijzigd, een nieuwe host gestart als reactie op het toegenomen verkeer of er is een fout gedetecteerd in een toepassing.

Logboeken zijn bijzonder nuttig voor het combineren van gegevens uit een groot aantal bronnen voor complexe analyse en trends na verloop van tijd. Omdat de indeling van de gegevens variëren kan, kunnen toepassingen aangepaste logboeken met behulp van de structuur die ze nodig hebben te maken. Metrische gegevens kunnen zelfs worden gerepliceerd in Logboeken te combineren met andere bewakingsgegevens voor trending en andere data-analyse.


## <a name="monitoring-tools-in-azure"></a>Controleprogramma's in Azure
Bewaking van gegevens in Azure worden verzameld en geanalyseerd op basis van meerdere bronnen die worden beschreven in de volgende secties.

### <a name="azure-metrics"></a>Metrische gegevens van Azure
Metrische gegevens van Azure-resources en toepassingen worden in Azure metrische gegevens verzameld. Metrische gegevens is geïntegreerd in de pagina's in de Azure-portal voor bepaalde Azure-resources zoals virtuele machines, waaronder grafieken van deze metrische gegevens als het CPU- en netwerkgebruik voor de geselecteerde computer. Ze kan ook worden geanalyseerd met de [Metrics Explorer](../monitoring-and-diagnostics/monitoring-metric-charts.md) die de waarden van meerdere metrische gegevens kunt grafiek na verloop van tijd.  U kunt de grafieken interactief weergeven of vastmaken aan een dashboard om deze met andere visualisaties weer te geven. U kunt ook ophalen met metrische gegevens met de [Azure REST-API bewaken](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

U kunt informatie krijgen over de metrische gegevens die worden verzameld door verschillende soorten Azure-resources op [bronnen van bewakingsgegevens in Azure](monitoring-data-sources.md). 

![Metrics Explorer](media/monitoring-data-collection/metrics-explorer.png)


### <a name="azure-activity-log"></a>Azure-activiteitenlogboek 
De [Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) slaat logboeken over de configuratie en status van Azure-services. U kunt de activiteit Log-Explorer gebruiken om deze logboeken in Azure portal weer te geven, maar ze vaak [gekopieerd naar Log Analytics](../log-analytics/log-analytics-activity.md) kunnen worden geanalyseerd met andere logboekgegevens.

U kunt de activiteit Log Explorer gebruiken om weer te geven van het activiteitenlogboek filteren om te voldoen aan bepaalde criteria.  De meeste resources heeft ook een optie in hun menu in de Azure-portal waarop de activiteit Log Explorer gefilterd voor die resource Activity-Log. U kunt ook ophalen activiteitenlogboeken met de [Azure REST-API bewaken](../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Activiteit Log Explorer](media/monitoring-data-collection/activity-log-explorer.png)


### <a name="log-analytics"></a>Log Analytics
Log Analytics biedt een algemeen gegevensplatform voor beheer in Azure. Het is de primaire service die wordt gebruikt voor opslag en analyse van Logboeken in Azure, verzamelen van gegevens uit diverse bronnen, met inbegrip van agents op virtuele machines, oplossingen voor het beheer en andere Azure-resources. Gegevens uit andere bronnen, waaronder metrische gegevens en het activiteitenlogboek kunnen worden gekopieerd naar Log Analytics om te kunnen maken van een volledige centrale opslagplaats van de gegevens te controleren.

Log Analytics is een uitgebreide querytaal voor het analyseren van de gegevens die worden verzameld.  U kunt [portals voor zoeken in logboeken](../log-analytics/log-analytics-log-search-portals.md) voor het interactief schrijven en testen van query's en analyseren van de resultaten ervan. U kunt ook [weergaven maken](../log-analytics/log-analytics-view-designer.md) te visualiseren de resultaten van zoekopdrachten in Logboeken of plakt u de resultaten van een query rechtstreeks op een Azure-dashboard.  Beheeroplossingen bevatten weergaven en zoekopdrachten in Logboeken in Log Analytics voor het analyseren van de gegevens die zij verzamelen. Andere services, zoals Application Insights gegevens opslaan in Log Analytics en bieden extra hulpprogramma's voor analyse.  

![Logboeken](media/monitoring-data-collection/logs.png)

### <a name="application-insights"></a>Application Insights
Application Insights verzamelt telemetrie voor webtoepassingen die zijn geïnstalleerd op een aantal verschillende platformen. Het de gegevens worden opgeslagen in Azure metrische gegevens en Log Analytics en biedt een uitgebreide set met uitgebreide hulpprogramma's, boven op het bestaande hulpprogramma's voor het analyseren van deze gegevens voor het analyseren en visualiseren van de gegevens. Hiermee kunt u gebruikmaken van een gemeenschappelijke set services zoals waarschuwingen, zoekopdrachten in Logboeken en dashboards die u voor andere bewaking gebruiken.


![App Insights](media/monitoring-data-collection/app-insights.png)

### <a name="service-map"></a>Serviceoverzicht
Serviceoverzicht biedt een visuele representatie van virtuele machines met hun processen en afhankelijkheden. Wordt de meeste van deze gegevens opgeslagen in Log Analytics, zodat u ze met andere gegevens analyseren kunt. De Serviceoverzicht-console worden ook gegevens opgehaald van Log Analytics om deze in de context van de virtuele machine die wordt geanalyseerd.

![Serviceoverzicht](media/monitoring-data-collection/service-map.png)


## <a name="transferring-monitoring-data"></a>Gegevens overbrengen

### <a name="metrics-to-logs"></a>Metrische gegevens in Logboeken
Metrische gegevens kunnen ook worden gerepliceerd in Log Analytics om uit te voeren complexe analyse met andere gegevenstypen met behulp van de uitgebreide querytaal. U kunt ook logboekgegevens bewaren voor langere tijd dan metrische gegevens, zodat u kunt het uitvoeren van de trends na verloop van tijd. Wanneer metrische gegevens of geen prestatiegegevens worden opgeslagen in Log Analytics, die gegevens als een logboekbestand fungeert. Metrische gegevens voor de ondersteuning van bijna realtime analyse en waarschuwingsdoeleinden tijdens het gebruik van Logboeken voor trending en -analyse met andere gegevens gebruiken.

Informatie voor het verzamelen van metrische gegevens van Azure-resources op [Azure verzamelen van Logboeken en metrische gegevens voor gebruik in Log Analytics](../log-analytics/log-analytics-azure-storage.md). Begeleiding voor het verzamelen van metrische gegevens voor resources van Azure PaaS-resources op [verzamelen van metrische gegevens voor Azure PaaS-resources configureren met Log Analytics](../log-analytics/log-analytics-collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Logboeken naar metrische gegevens
Zoals hierboven beschreven, zijn metrische gegevens sneller reageren dan Logboeken zodat u kunt waarschuwingen maken met een lagere latentie en tegen lagere kosten. Log Analytics verzamelt een aanzienlijke hoeveelheid numerieke gegevens die zijn geschikt voor metrische gegevens, maar wordt niet opgeslagen in Azure metrische gegevens. Een veelvoorkomend voorbeeld is de prestatiegegevens die worden verzameld van agents en oplossingen voor beheer. Sommige van deze waarden kunnen worden gekopieerd naar Azure metrische gegevens waar ze beschikbaar voor waarschuwingen en voor analyse met Metrics Explorer zijn.

De uitleg van deze functie is beschikbaar op [sneller metrische waarschuwingen voor logboeken nu beschikbaar in de beperkte openbare preview](https://azure.microsoft.com/blog/faster-metric-alerts-for-logs-now-in-limited-public-preview/). De lijst met waarden ondersteuning is beschikbaar op [ondersteunde metrische gegevens en het maken van methoden voor nieuwe metrische waarschuwingen](../monitoring-and-diagnostics/monitoring-near-real-time-metric-alerts.md).

### <a name="event-hub"></a>Event Hub
Naast het gebruik van de hulpprogramma's in Azure voor het analyseren van gegevens, is het raadzaam om door te sturen naar een extern hulpprogramma een beveiligingsgegevens en event management (SIEM) product. Dit gebeurt meestal met [Azure Event Hub](https://docs.microsoft.com/azure/event-hubs/). 

U kunt hulp krijgen voor de verschillende soorten bewakingsgegevens op [Stream Azure-bewakingsgegevens naar een event hub voor gebruik door een extern hulpprogramma](../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [bewakingsgegevens beschikbaar](monitoring-data-sources.md) voor verschillende resources in Azure. 