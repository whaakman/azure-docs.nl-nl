---
title: Bronnen van de gegevens in Azure Monitor | Microsoft Docs
description: Beschrijving van de gegevens beschikbaar voor het bewaken van de status en prestaties van uw Azure-resources en de toepassingen die daarop worden uitgevoerd.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/13/2018
ms.author: bwren
ms.openlocfilehash: 450e9c90fff7f210fe48c930e3bbb1d50851232d
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713228"
---
# <a name="sources-of-data-in-azure-monitor"></a>Bronnen van de gegevens in Azure Monitor
Dit artikel beschrijft de bronnen van de gegevens die zijn verzameld door Azure Monitor voor het bewaken van de status en prestaties van uw resources en de toepassingen die daarop worden uitgevoerd. Deze resources mogelijk in Azure, in een andere cloud of on-premises.  Zie [gegevens verzameld door Azure Monitor](monitoring-data-collection.md) voor meer informatie over hoe deze gegevens worden opgeslagen en hoe u deze kunt bekijken.

Gegevens in Azure te controleren is afkomstig van een groot aantal bronnen die kunnen worden ingedeeld in lagen, de hoogste niveaus wordt uw toepassing en alle besturingssystemen en de lagere lagen worden onderdelen van Azure-platform. Dit wordt geïllustreerd in het volgende diagram met elke laag in de volgende secties gedetailleerd beschreven.

![Lagen van bewakingsgegevens](media/monitoring-data-sources/monitoring-tiers.png)

## <a name="azure-tenant"></a>Azure-Tenant
Telemetrie met betrekking tot uw Azure-tenant worden verzameld van de tenant-brede services zoals Azure Active Directory.

![Verzameling van de Azure-tenant](media/monitoring-data-sources/tenant-collection.png)

### <a name="azure-active-directory-audit-logs"></a>Auditlogboeken van Azure Active Directory
[Azure Active Directory-rapportage](../active-directory/reports-monitoring/overview-reports.md) bevat de geschiedenis van aanmelding activiteit en audit audittrail van wijzigingen in een bepaalde tenant. Deze auditlogboeken kunnen worden geschreven naar Log Analytics voor het analyseren van deze met andere logboekgegevens.


## <a name="azure-platform"></a>Azure-platform
Telemetrie met betrekking tot de status en het gebruik van Azure die zelf bevat gegevens over de werking en het beheer van uw Azure-abonnement. Service health-gegevens die zijn opgeslagen in de Azure-activiteitenlogboek en de auditlogboeken van Azure Active Directory bevat.

![Verzameling van de Azure-abonnement](media/monitoring-data-sources/azure-collection.png)

### <a name="azure-service-health"></a>Azure Service Health
[Azure Service Health](../monitoring-and-diagnostics/monitoring-service-notifications.md) bevat informatie over de status van de Azure-services in uw abonnement die uw toepassing en de resources afhankelijk van zijn. U kunt waarschuwingen om te worden geïnformeerd over de huidige en verwachte kritieke problemen die invloed kunnen zijn op uw toepassing kunt maken. Servicestatus records worden opgeslagen in de [Azure-activiteitenlogboek](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md), zodat u kunt ze in de activiteit Log Explorer weergeven en kopieer ze naar Log Analytics.

### <a name="azure-activity-log"></a>Azure-activiteitenlogboek
De [Azure Activity Log](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md) service medische dossiers, samen met records in de van configuratiewijzigingen aan uw Azure-resources bevat. Het activiteitenlogboek is beschikbaar voor alle Azure-resources en vertegenwoordigt de _externe_ weergeven. De specifieke typen records in het activiteitenlogboek worden beschreven in [gebeurtenisschema in het Azure-activiteitenlogboek](../monitoring-and-diagnostics/monitoring-activity-log-schema.md).

U kunt het activiteitenlogboek voor een bepaalde resource weergeven op de pagina in de Azure portal of de weergave van de logboeken van meerdere resources in de [activiteit Log Explorer](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md). Het is met name handig voor het kopiëren van de logboekvermeldingen naar Log Analytics te combineren met andere gegevens. U kunt ze ook verzenden naar andere locaties met behulp van [Event Hubs](../monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md).



## <a name="azure-services"></a>Azure-services
Metrische gegevens en resources niveau diagnostische logboeken bieden informatie over de _interne_ werking van Azure-resources. Dit zijn beschikbaar voor de meeste Azure-services en oplossingen bieden extra inzichten in bepaalde services.

![Azure-resource-verzameling](media/monitoring-data-sources/azure-resource-collection.png)


### <a name="metrics"></a>Metrische gegevens
De meeste Azure-services wordt gegenereerd [platform metrische gegevens](monitoring-data-collection.md#metrics) die overeenkomen de prestaties en de bewerking. De specifieke [metrische gegevens varieert voor elk type resource](../monitoring-and-diagnostics/monitoring-supported-metrics.md).  Ze zijn toegankelijk is vanaf de Metrics explorer en kunnen worden gekopieerd naar Log Analytics voor trending en andere analyse.


### <a name="resource-diagnostic-logs"></a>Diagnostische logboeken van resource
Terwijl het activiteitenlogboek informatie over de bewerkingen die worden uitgevoerd op een Azure-resources, resourceniveau bevat [diagnostische logboeken](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) inzicht geven in de werking van de resource zelf.   De configuratievereisten en de inhoud van deze logboeken [varieert per resourcetype](../monitoring-and-diagnostics/monitoring-diagnostic-logs-schema.md).

U kunt rechtstreeks logboeken met diagnostische gegevens niet weergeven in Azure portal, maar u kunt [ze verzenden naar Azure storage voor het archiveren van](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) en ze te exporteren [Event Hub](../event-hubs/event-hubs-about.md) voor omleiding naar andere services, of [in logboek Analytics](../monitoring-and-diagnostics/monitor-stream-diagnostic-logs-log-analytics.md) voor analyse. Sommige resources rechtstreeks naar Log Analytics kunnen schrijven terwijl anderen naar een opslagaccount voordat u schrijven [geïmporteerd in Log Analytics](../log-analytics/log-analytics-azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).

### <a name="monitoring-solutions"></a>Bewakingsoplossingen
 [Bewakingsoplossingen](../azure-monitor/insights/solutions.md) verzamelen van gegevens voor meer inzicht in de werking van een bepaalde service of toepassing. Verzamelen van gegevens in Log Analytics waar deze kan worden geanalyseerd op basis van de [querytaal](../log-analytics/log-analytics-queries.md) of [weergaven](../log-analytics/log-analytics-view-designer.md) die gewoonlijk zijn opgenomen in de oplossing.

## <a name="guest-operating-system"></a>Gast-besturingssysteem
COMPUTE-resources in Azure, in andere clouds en on-premises hebben een gast-besturingssysteem om te controleren. U kunt met de installatie van een of meer agents, telemetrie verzamelen van de Gast in de dezelfde controleprogramma's als de Azure-services zelf.

![Verzameling van Azure compute-resource](media/monitoring-data-sources/compute-resource-collection.png)

### <a name="azure-diagnostic-extension"></a>Diagnostische Azure-extensie
Met de Azure Diagnostics-extensie, biedt een basisniveau van de bewaking met het verzamelen van Logboeken en prestatiegegevens van het besturingssysteem van de client van Azure compute-resources.   

### <a name="log-analytics-agent"></a>Log Analytics-agent
Uitgebreide bewaking en beheer van uw Windows- of Linux-machines of fysieke computer wordt geleverd met de Log Analytics-agent. De virtuele machine kan worden uitgevoerd in Azure, een andere cloud of on-premises en de agent een verbinding met Log Analytics maakt rechtstreeks of via System Center Operations Manager en kunt u het verzamelen van gegevens uit [gegevensbronnen](../log-analytics/log-analytics-data-sources.md) die u hebt configureren of [bewakingsoplossingen](../azure-monitor/insights/solutions.md) die u meer inzicht geven in toepassingen die worden uitgevoerd op de virtuele machine.

### <a name="dependency-agent"></a>Agent voor afhankelijkheden
[Serviceoverzicht](../monitoring/monitoring-service-map.md) en [Azure Monitor voor virtuele machines](../azure-monitor/insights/vminsights-overview.md) vereist een Agent voor afhankelijkheden op Windows en Linux-machines. Dit kan worden geïntegreerd met de Log Analytics-agent verzamelt gedetecteerde gegevens over de processen die worden uitgevoerd op de virtuele machine en een extern Procesafhankelijkheden. Het deze gegevens worden opgeslagen in Log Analytics en visualiseert de gedetecteerde onderling verbonden onderdelen.  

Zie voor meer inzicht in de verschillen tussen de agents en te gebruiken, afhankelijk van uw bewakingsvereisten, [agents bewakingsoverzicht](monitoring-overview-azure-agents.md).

## <a name="applications"></a>Toepassingen
Naast de telemetrie die uw toepassing naar het gastbesturingssysteem schrijven kan, gedetailleerde bewaking wordt gedaan met [Application Insights](https://docs.microsoft.com/azure/application-insights/). Application Insights kunnen gegevens verzamelen vanuit toepassingen die worden uitgevoerd op een aantal verschillende platformen. De toepassing kan worden uitgevoerd in Azure, een andere cloud of on-premises.

![Verzamelen van gegevens van toepassing](media/monitoring-data-sources/application-collection.png)


### <a name="application-data"></a>Toepassingsgegevens
Wanneer u Application Insights voor een toepassing inschakelt door een instrumentatiepakket installeren, verzamelt deze metrische gegevens en logboeken met betrekking tot de prestaties en het gebruik van de toepassing. Dit omvat gedetailleerde informatie over paginaweergaven, aanvragen en uitzonderingen. Application Insights slaat de gegevens worden verzameld in Azure metrische gegevens en Log Analytics. Het uitgebreide hulpmiddelen voor het analyseren van deze gegevens bevat, maar u kunt deze ook analyseren met gegevens uit andere bronnen met behulp van hulpprogramma's zoals Metrics Explorer en zoekopdrachten in Logboeken.

Ook kunt u Application Insights aan [maken van een aangepaste meetwaarde](../application-insights/app-insights-api-custom-events-metrics.md).  Hiermee kunt u uw eigen logica voor het berekenen van een numerieke waarde en vervolgens opslaan die waarde met andere metrische gegevens die kunnen worden geopend via de Metric Explorer en kan worden gebruikt voor het definiëren [voor automatisch schalen](../monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md) en metrische waarschuwingen.

### <a name="dependencies"></a>Afhankelijkheden
Als u wilt bewaken verschillende logische bewerkingen van een toepassing, moet u [verzamelen van telemetrie over meerdere onderdelen](../application-insights/app-insights-transaction-diagnostics.md). Application Insights biedt ondersteuning [telemetriecorrelatie gedistribueerd](../application-insights/application-insights-correlation.md) waarin de afhankelijkheden tussen onderdelen, zodat u ze samen te analyseren.

### <a name="availability-tests"></a>Beschikbaarheidstests
[Beschikbaarheidstests](../application-insights/app-insights-monitor-web-app-availability.md) in Application Insights kunt u voor het testen van de beschikbaarheid en reactiesnelheid van uw toepassing vanaf verschillende locaties op het openbare Internet. U kunt een eenvoudige ping-test om te bevestigen dat de toepassing actief is of Visual Studio gebruiken voor het maken van een WebTest die een gebruikersscenario simuleert.  Beschikbaarheidstests vereisen instrumentatie in de toepassing.

## <a name="custom-sources"></a>Aangepaste bronnen
Naast de standaard-laag van een toepassing moet u mogelijk andere bronnen die telemetrie die niet kan worden verzameld met de andere gegevensbronnen hebben controleren. Voor deze resources moet u deze gegevens met behulp van een API van Azure Monitor te schrijven.

![Aangepaste gegevens verzamelen](media/monitoring-data-sources/custom-collection.png)

### <a name="data-collector-api"></a>Gegevensverzamelaar-API
Azure Monitor kunt logboekgegevens verzamelen vanuit elke REST-client met behulp van de [Data Collector API](../log-analytics/log-analytics-data-collector-api.md). Hiermee kunt u aangepaste bewakingsscenario's maken en uitbreiden van de bewaking aan resources die niet beschikbaar voor telemetrie via andere bronnen maken.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [typen gegevens te controleren die worden verzameld door Azure Monitor](monitoring-data-collection.md) en hoe u kunt weergeven en analyseren van deze gegevens.
