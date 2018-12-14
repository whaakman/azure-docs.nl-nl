---
title: Door gegevens te controleren die worden verzameld door Azure Monitor | Microsoft Docs
description: Bewaking van de gegevens die zijn verzameld door Azure Monitor is onderverdeeld in metrische gegevens die zijn lichtgewicht en kan ondersteunen bijna realtime scenario's en de logboeken die zijn opgeslagen in Log Analytics voor geavanceerde analyse.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/27/2018
ms.author: bwren
ms.openlocfilehash: 3795db2928e23b16d93e544c4f148b049eccfdef
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384833"
---
# <a name="monitoring-data-collected-by-azure-monitor"></a>Door gegevens te controleren die worden verzameld door Azure Monitor
[Azure Monitor](../overview.md) is een service waarmee u uw toepassingen en de resources die ze erop dat vertrouwen bewaken. Centrale op deze functie is opslag van Telemetrie en andere gegevens van bewaakte resources. Dit artikel bevat een volledige beschrijving van hoe deze gegevens worden opgeslagen en gebruikt door Azure Monitor.

Alle gegevens die zijn verzameld door Azure Monitor in een van twee fundamentele typen, past [metrische gegevens](#metrics) en [logboeken](#logs). Metrische gegevens zijn numerieke waarden die een bepaald aspect van een systeem op een bepaald punt in tijd beschrijven. Ze zijn lichtgewicht en kan in de buurt van realtime scenario's ondersteunen. Logboeken bevatten verschillende soorten gegevens die zijn ingedeeld in records met verschillende sets van eigenschappen voor elk type. Telemetrie, zoals gebeurtenissen en traceringen worden opgeslagen als Logboeken ook naar prestatiegegevens zodat deze kan allemaal worden gecombineerd voor analyse.

![Overzicht van Azure Monitor](media/data-collection/overview.png)

## <a name="metrics"></a>Metrische gegevens
Metrische gegevens zijn numerieke waarden die worden beschreven van een bepaald aspect van een systeem op een bepaald tijdstip. Ze zijn lichtgewicht en kan in de buurt van realtime scenario's ondersteunen. Metrische gegevens worden verzameld met regelmatige tussenpozen al dan niet de waarde wordt gewijzigd. Ze zijn nuttig omdat ze vaak kunnen worden verzameld, en een waarschuwing kan snel worden geactiveerd met relatief eenvoudige logica op waarschuwingen. 

Bijvoorbeeld, u mogelijk verzamelen CPU-gebruik van een virtuele machine per minuut, maar het aantal gebruikers dat is aangemeld bij uw toepassing om de 10 minuten. U kunt een waarschuwing kan worden geactiveerd wanneer één van de waarden die worden verzameld of zelfs het verschil tussen twee waarden van een opgegeven drempelwaarde overschrijdt.

Specifieke kenmerken van metrische gegevens in Azure zijn onder andere het volgende:

* Verzameld op één minuut frequentie, tenzij anders aangegeven in de definitie van de metrische gegevens.
* Onderscheiden op basis van een metrische naam en een naamruimte die als een categorie fungeert.
* 93 dagen bewaard. U kunt metrische gegevens kopiëren naar Log Analytics voor de lange termijn trends.

Elke metrische waarde heeft de volgende eigenschappen:
* De tijd die de waarde die is verzameld.
* Het type van de meting van de waarde vertegenwoordigt.
* De waarde van de resource is gekoppeld.
* De waarde zelf.
* Sommige metrische gegevens kan meerdere dimensies hebben, zoals beschreven in de volgende sectie. Aangepaste metrische gegevens kan maximaal 10 dimensies hebben.

### <a name="multi-dimensional-metrics"></a>Multi-dimensionale metrische gegevens
De grootte van een metrische waarde zijn naam / waarde-paren die aanvullende gegevens om te beschrijven van de metrische waarde bevatten. Bijvoorbeeld, een metrische waarde _beschikbare ruimte op schijf_ kan een dimensie met de naam hebben _station_ met waarden _C:_, _D:_, die wordt bevraagd weergeven de beschikbare schijfruimte op alle stations of voor elk station afzonderlijk. 

In het volgende voorbeeld ziet u twee gegevenssets voor een hypothetische metrische gegevens met de naam _netwerkdoorvoer_. De eerste gegevensset heeft geen dimensies. De tweede gegevensset bevat de waarden met twee dimensies, I_P Address_ en _richting_:

### <a name="network-throughput"></a>Netwerkdoorvoer

 |Tijdstempel        | Metrische waarde | 
   | ------------- |:-------------| 
   | 8/9/2017 8:14 | 1,331.8 kbps | 
   | 8/9/2017 8:15 | 1,141.4 kbps |
   | 8/9/2017 8:16 | 1,110.2 kbps |

Deze niet-dimensionale metrische gegevens kan alleen een eenvoudige vraag beantwoorden, zoals "Wat is mijn netwerkdoorvoer op een bepaald moment?"

### <a name="network-throughput--two-dimensions-ip-and-direction"></a>De netwerkdoorvoer + twee dimensies (IP '-' en "Richting")

| Tijdstempel          | Dimensie '-IP | Dimensie "Richting" | Metrische waarde| 
   | ------------- |:-----------------|:------------------- |:-----------|  
   | 8/9/2017 8:14 | IP = "192.168.5.2" | Richting = "Verzenden"    | 646.5 kbps |
   | 8/9/2017 8:14 | IP = "192.168.5.2" | Richting = 'Ontvangen' | 420.1 kbps |
   | 8/9/2017 8:14 | IP = "10.24.2.15"  | Richting = "Verzenden"    | 150,0 kbps | 
   | 8/9/2017 8:14 | IP = "10.24.2.15"  | Richting = 'Ontvangen' | 115,2 kbps |
   | 8/9/2017 8:15 | IP = "192.168.5.2" | Richting = "Verzenden"    | 515.2 kbps |
   | 8/9/2017 8:15 | IP = "192.168.5.2" | Richting = 'Ontvangen' | 371.1 kbps |
   | 8/9/2017 8:15 | IP = "10.24.2.15"  | Richting = "Verzenden"    | 155.0 kbps |
   | 8/9/2017 8:15 | IP = "10.24.2.15"  | Richting = 'Ontvangen' | 100.1 kbps |

Deze metrische gegevens kunt beantwoorden vragen zoals "Wat was de doorvoer van het netwerk voor elk IP-adres?" en "hoeveel gegevens is verzonden en ontvangen?" Multi-dimensionale metrische gegevens over uitvoeren als u meer analytische en diagnostische waarde ten opzichte van niet-dimensionale metrische gegevens.

### <a name="value-of-metrics"></a>Waarde van metrische gegevens
Afzonderlijke metrische gegevens bieden doorgaans weinig inzicht in hun eigen. Ze bieden een enkele waarde zonder context dan vergelijking met een enkelvoudige drempelwaarde. Ze zijn nuttig in combinatie met andere metrische gegevens om patronen en trends te identificeren of in combinatie met Logboeken die een context om bepaalde waarden bieden. 

Bijvoorbeeld, kan een bepaald aantal gebruikers in uw toepassing op een bepaald moment u vertellen weinig over de status van de toepassing. Maar een plotselinge afname in gebruikers, aangegeven met meerdere waarden van de dezelfde metrische gegevens, kan duiden op een probleem. Overmatige uitzonderingen die door de toepassing veroorzaakt en aangegeven door een afzonderlijke metrische gegevens, mogelijk een probleem met toepassing die wordt veroorzaakt door de vervolgkeuzelijst identificeren. Gebeurtenissen die de toepassing maakt voor het identificeren van fouten in de bijbehorende onderdelen kunt u de hoofdoorzaak identificeren.

### <a name="sources-of-metric-data"></a>Bronnen van metrische gegevens
Er zijn drie fundamentele bronnen van metrische gegevens die door Azure Monitor worden verzameld. Al deze metrische gegevens zijn beschikbaar in de metrische store waar ze kunnen worden geëvalueerd samen, ongeacht de bron.

**Metrische gegevens platform** worden gemaakt door Azure-resources en geven u inzicht in hun status en prestaties. Elk type resource maakt een [aparte set metrische gegevens](../../monitoring-and-diagnostics/monitoring-supported-metrics.md) zonder configuratie vereist. 

**Metrische toepassingsgegevens** zijn gemaakt door Application Insights voor uw bewaakte toepassingen en kunt u prestatieproblemen detecteren en bijhouden van trends in hoe uw toepassing wordt gebruikt. Dit omvat ook deze waarden als _serverreactietijd_ en _browseruitzonderingen_.

**Aangepaste metrische gegevens** metrische gegevens die u definieert naast de standaard metrische gegevens die zijn automatisch beschikbaar zijn. Aangepaste metrische gegevens moeten worden gemaakt op basis van één resource in dezelfde regio als die resource. U kunt aangepaste metrische gegevens met behulp van de volgende manieren maken:
    - [Aangepaste metrische gegevens in uw toepassing definiëren](../../application-insights/app-insights-api-custom-events-metrics.md) die wordt bewaakt door Application Insights. Deze worden naast de standaard set metrische toepassingsgegevens.
    - Publiceren van aangepaste metrische gegevens van uw Windows-virtuele machines met behulp van [Windows diagnostische extensie (WAD),](../../azure-monitor/platform/diagnostics-extension-overview.md).
    - Publiceren van aangepaste metrische gegevens van uw Linux-machines met [InfluxData Telegraf Agent](https://www.influxdata.com/time-series-platform/telegraf/).
    - Aangepaste metrische gegevens van een Azure-service met behulp van de aangepaste API voor metrische gegevens schrijven.
    
![Overzicht van metrische gegevens](media/data-collection/metrics-overview.png)

### <a name="what-can-you-do-with-metrics"></a>Wat kunt u doen met metrische gegevens?
Taken die u met metrische gegevens uitvoeren kunt omvatten het volgende:

- Gebruik [Metrics explorer](../../azure-monitor/platform/metrics-charts.md) verzamelde metrische gegevens analyseren en ze op een grafiek te tekenen. De prestaties van een resource (zoals een virtuele machine, de website of de logische app) bijhouden door grafieken om vast te maken een [Azure-dashboard](../../azure-portal/azure-portal-dashboards.md).
- Configureer een [waarschuwingsregel voor metrische gegevens](alerts-metric.md) die duurt of een melding verzendt [automatische actie](action-groups.md) wanneer de metriek een drempelwaarde overschrijdt.
- Gebruik [voor automatisch schalen](../../azure-monitor/platform/autoscale-overview.md) vergroten of verkleinen van resources op basis van een metriek een drempelwaarde overschrijden.
- Metrische gegevens doorsturen naar Log Analytics om metrische gegevens, samen met logboekgegevens te analyseren en voor het opslaan van de metrische waarden voor langer dan 93 dagen. 
- Metrische gegevens naar Stream een [Event Hub](../../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md) te routeren [Azure Stream Analytics](../../stream-analytics/stream-analytics-introduction.md) of met externe systemen.
- [Archief](../../monitoring-and-diagnostics/monitor-tutorial-archive-monitoring-data.md) de geschiedenis van prestaties of de status van uw resource voor naleving, controle- of offline rapportagedoeleinden.
- Toegang tot metrische waarden van een opdrachtregel of met behulp van aangepaste toepassing [PowerShell-cmdlets](https://docs.microsoft.com/powershell/module/azurerm.insights/?view=azurermps-6.7.0) of [REST-API](../../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).



### <a name="viewing-metrics"></a>Metrische gegevens weergeven
Metrische gegevens in Azure worden verzameld in de database van Azure Monitor metrische gegevens. Dit is een tijdreeks database die zijn geoptimaliseerd voor het snel ophalen en winkels metrische waarden voor 93 dagen. Kopieer metrische gegevens naar Log Analytics voor de lange termijn analyse en trends.

Metrische gegevens wordt gebruikt in verschillende manieren, zoals hierboven is beschreven. Gebruik [Metrics explorer](../../azure-monitor/platform/metrics-charts.md) om rechtstreeks analyseren van de gegevens in uw metrische store en de waarden van meerdere metrische gegevens gedurende een periode van grafiek. U kunt de grafieken interactief weergeven of vastmaken aan een dashboard om deze met andere visualisaties weer te geven. U kunt ook metrische gegevens ophalen met behulp van de [Azure REST-API bewaken](../../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Metrics Explorer](media/data-collection/metrics-explorer.png)





## <a name="logs"></a>Logboeken
Logboeken bevatten verschillende soorten gegevens die zijn ingedeeld in records met verschillende sets van eigenschappen voor elk type. Logboeken kunnen numerieke waarden, zoals metrische gegevens bevatten, maar bevatten doorgaans tekstgegevens met gedetailleerde beschrijvingen. Ze verder verschillen van metrische gegevens in dat ze in de structuur ervan variëren en worden vaak niet verzameld met regelmatige intervallen.

Een algemene type logboekvermelding is een gebeurtenis die sporadisch worden verzameld. Ze worden gemaakt door een toepassing of service en zijn meestal voldoende informatie gegeven om een volledige context te bieden op hun eigen. Een gebeurtenis kan bijvoorbeeld duiden dat een bepaalde resource is gemaakt of gewijzigd, een nieuwe host gestart als reactie op toegenomen verkeer, of er is een fout gedetecteerd in een toepassing.

Logboeken zijn met name nuttig voor het combineren van gegevens uit diverse bronnen, complexe analyse, en voor de trends na verloop van tijd. Omdat de indeling van de gegevens variëren kan, kunnen toepassingen aangepaste logboeken kunnen maken met behulp van de structuur die ze nodig hebben. Metrische gegevens worden ook gerepliceerd in Logboeken om ze te combineren met andere bewakingsgegevens voor trending en andere data-analyse.



### <a name="log-analytics"></a>Log Analytics
Logboeken die worden verzameld door Azure Monitor worden opgeslagen in Log Analytics dat Telemetrie en andere gegevens worden verzameld uit een groot aantal bronnen. Het biedt een uitgebreide querytaal en een analyse-engine waarmee u inzicht in de werking van uw toepassingen en resources. Andere Azure-services zoals [Azure Security Center](../../security-center/security-center-intro.md) hun gegevens opslaan in Log Analytics om te voorzien van een algemeen gegevensplatform in de Azure management.

> [!IMPORTANT]
> Gegevens uit Application Insights wordt opgeslagen in Log Analytics, zoals andere logboekgegevens, behalve dat deze opgeslagen in een afzonderlijke partitie. Dit biedt ondersteuning voor dezelfde functionaliteit als andere Log Analytics-gegevens, maar moet u de [Application Insights-console](../../application-insights/app-insights-analytics.md) of de [Application Insights-API](https://dev.applicationinsights.io/) voor toegang tot deze gegevens. U kunt een [meerdere bronnen query](../log-query/cross-workspace-query.md) voor het analyseren van gegevens, samen met andere gegevens van een toepassing.


### <a name="sources-of-log-data"></a>Bronnen van logboekgegevens
Log Analytics kan gegevens verzamelen uit diverse bronnen, zowel binnen Azure en on-premises bronnen. Bronnen van gegevens die worden geschreven naar Log Analytics omvatten het volgende:

- [Activiteitenlogboeken](collect-activity-logs.md) van Azure-resources met informatie over de configuratie en de status en [diagnostische logboeken](../../monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics.md) die inzicht geven in hun werking.
- Agents op [Windows](../../log-analytics/log-analytics-windows-agent.md) en [Linux](../learn/quick-collect-linux-computer.md) virtuele machines die telemetrie vanuit de Gast-besturingssysteem en toepassingen naar Log Analytics volgens verzenden [gegevensbronnen](agent-data-sources.md) die u configureert.
- Toepassingsgegevens verzameld door [Application Insights](https://docs.microsoft.com/azure/application-insights/).
- Gegevens die inzicht bieden in een bepaalde toepassing of service van [bewakingsoplossingen](../insights/solutions.md) of functies zoals Container Insights, inzichten van de virtuele machine of Resource Group inzichten.
- Beveiligingsgegevens die zijn verzameld door [Azure Security Center](https://docs.microsoft.com/azure/security-center/).
- [Metrische gegevens](#metrics) van Azure-resources. Hiermee kunt u metrische gegevens langer duurt dan 93 dagen opslaan en analyseren met andere logboekgegevens.
- Telemetrie naar geschreven [Azure Storage](azure-storage-iis-table.md).
- Aangepaste gegevens vanaf een willekeurige REST-API-client met behulp van de [HTTP Data Collector API](data-collector-api.md) client of vanuit een [Azure Logic App](https://docs.microsoft.com/azure/logic-apps/) werkstroom.

![Onderdelen van Log Analytics](media/data-collection/logs-overview.png)




### <a name="what-can-you-do-with-logs"></a>Wat kunt u doen met Logboeken?
Taken die u met Logboeken uitvoeren kunt omvatten het volgende:

- Gebruik de [Log Analytics-pagina](../log-query/get-started-portal.md) in de Azure portal voor het schrijven van query's voor het analyseren van logboekgegevens.  Resultaten weergegeven als tabellen of grafieken vastmaken een [Azure-dashboard](../../azure-portal/azure-portal-dashboards.md).
- Configureren van een [waarschuwingsregel](alerts-log.md) die duurt of verzendt een melding [automatische actie](action-groups.md) wanneer de resultaten van de query overeenkomt met een bepaalde resultaat.
- Een werkstroom op basis van gegevens in met behulp van Log Analytics bouwen [Logic Apps](~/articles/logic-apps/index.yml).
- De resultaten van een query voor het exporteren [Power BI](powerbi.md) verschillende visualisaties gebruiken en delen met gebruikers buiten Azure.
- Toegang tot metrische waarden van een opdrachtregel of met behulp van aangepaste toepassing [PowerShell-cmdlets](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/?view=azurermps-6.8.1) of [REST-API](https://dev.loganalytics.io/).

### <a name="viewing-log-data"></a>Logboekgegevens weergeven
Alle gegevens van Log Analytics is opgehaald met een [logboekquery](../log-query/log-query-overview.md) die een bepaalde set gegevens bevat. Query's zijn geschreven met behulp van de [querytaal van Log Analytics](../log-query/get-started-queries.md) is een uitgebreide querytaal snel ophalen, samenvoegen en analyseren van verzamelde gegevens. Gebruik de [Log Analytics-pagina](../log-query/portals.md) in Azure portal voor het rechtstreeks analyseren van de gegevens in uw metrische gegevens opslaan en de waarden van meerdere metrische gegevens gedurende een periode van grafiek. U kunt de grafieken interactief weergeven of vastmaken aan een dashboard om deze met andere visualisaties weer te geven. U kunt ook metrische gegevens ophalen met behulp van de [Azure REST-API bewaken](../../monitoring-and-diagnostics/monitoring-rest-api-walkthrough.md).

![Logboeken](media/data-collection/logs.png)

## <a name="convert-monitoring-data"></a>Converteren van gegevens te controleren

### <a name="metrics-to-logs"></a>Metrische gegevens in Logboeken
U kunt metrische gegevens kopiëren naar Log Analytics om uit te voeren complexe analyse met andere gegevenstypen met behulp van de uitgebreide querytaal. U kunt ook logboekgegevens voor langere tijd dan metrische gegevens, waarmee u om uit te voeren van de trends na verloop van tijd kunt bewaren. Wanneer metrische gegevens of geen prestatiegegevens worden opgeslagen in Log Analytics, die gegevens als een logboekbestand fungeert. Metrische gegevens voor de ondersteuning van bijna realtime analyse en waarschuwingsdoeleinden tijdens het gebruik van Logboeken voor trending en -analyse met andere gegevens gebruiken.

Informatie voor het verzamelen van metrische gegevens van Azure-resources op [Azure verzamelen van Logboeken en metrische gegevens voor gebruik in Log Analytics](collect-azure-metrics-logs.md). Begeleiding voor het verzamelen van metrische gegevens voor resources van Azure PaaS-resources op [verzamelen van metrische gegevens voor Azure PaaS-resources configureren met Log Analytics](collect-azurepass-posh.md).

### <a name="logs-to-metrics"></a>Logboeken naar metrische gegevens
Zoals hierboven beschreven, zijn metrische gegevens sneller reageren dan Logboeken, zodat u waarschuwingen met een lagere latentie en tegen lagere kosten maken kunt. Log Analytics verzamelt een aanzienlijke hoeveelheid numerieke gegevens die zijn geschikt voor metrische gegevens, maar wordt niet opgeslagen in de metrische gegevens van Azure-database.  Een veelvoorkomend voorbeeld is de prestatiegegevens die worden verzameld van agents en oplossingen voor beheer. Sommige van deze waarden kunnen worden gekopieerd naar de database van metrische gegevens, waar ze beschikbaar voor waarschuwingen en voor analyse met Metrics explorer zijn.

De uitleg van deze functie is beschikbaar op [metrische waarschuwingen maken voor logboeken in Azure Monitor](../../azure-monitor/platform/alerts-metric-logs.md). De lijst met waarden ondersteuning is beschikbaar op [ondersteunde metrische gegevens met Azure Monitor](../../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftoperationalinsightsworkspaces).

## <a name="stream-data-to-external-systems"></a>Stream-gegevens met externe systemen
Naast het gebruik van de hulpprogramma's in Azure voor het analyseren van gegevens, mogelijk hebt u een vereiste dit doorsturen naar een extern hulpprogramma, zoals een security information en event management (SIEM) product. Doorsturen van dit gebeurt meestal rechtstreeks vanuit de bewaakte resources via [Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/). 

U kunt hulp krijgen voor de verschillende soorten bewakingsgegevens op [Stream Azure-bewakingsgegevens naar een event hub voor gebruik door een extern hulpprogramma](../../monitoring-and-diagnostics/monitor-stream-monitoring-data-event-hubs.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [bewakingsgegevens beschikbaar](data-sources.md) voor verschillende resources in Azure.
