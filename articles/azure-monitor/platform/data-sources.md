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
ms.openlocfilehash: 0349e3f482923a236ea29332abfc3a9206828ba7
ms.sourcegitcommit: 803e66de6de4a094c6ae9cde7b76f5f4b622a7bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2019
ms.locfileid: "53976882"
---
# <a name="sources-of-data-in-azure-monitor"></a>Bronnen van de gegevens in Azure Monitor
Dit artikel beschrijft de bronnen van de gegevens die zijn verzameld door Azure Monitor voor het bewaken van de status en prestaties van uw resources en de toepassingen die daarop worden uitgevoerd. Deze resources mogelijk in Azure, in een andere cloud of on-premises.  Zie [gegevens verzameld door Azure Monitor](data-collection.md) voor meer informatie over hoe deze gegevens worden opgeslagen en hoe u deze kunt bekijken.

Gegevens in Azure te controleren is afkomstig van een groot aantal bronnen die kunnen worden ingedeeld in lagen, de hoogste niveaus wordt uw toepassing en alle besturingssystemen en de lagere lagen worden onderdelen van Azure-platform. Dit wordt geïllustreerd in het volgende diagram met elke laag in de volgende secties gedetailleerd beschreven.

![Lagen van bewakingsgegevens](media/data-sources/monitoring-tiers.png)

## <a name="azure-tenant"></a>Azure-Tenant
Telemetrie met betrekking tot uw Azure-tenant worden verzameld van de tenant-brede services zoals Azure Active Directory.

![Verzameling van de Azure-tenant](media/data-sources/tenant-collection.png)

### <a name="azure-active-directory-audit-logs"></a>Auditlogboeken van Azure Active Directory
[Azure Active Directory-rapportage](../../active-directory/reports-monitoring/overview-reports.md) bevat de geschiedenis van aanmelding activiteit en audit audittrail van wijzigingen in een bepaalde tenant. Deze auditlogboeken kunnen worden geschreven naar Azure Monitor-logboeken voor het analyseren van deze met andere logboekgegevens.


## <a name="azure-platform"></a>Azure-platform
Telemetrie met betrekking tot de status en het gebruik van Azure die zelf bevat gegevens over de werking en het beheer van uw Azure-abonnement. Service health-gegevens die zijn opgeslagen in de Azure-activiteitenlogboek en de auditlogboeken van Azure Active Directory bevat.

![Verzameling van de Azure-abonnement](media/data-sources/azure-collection.png)

### <a name="azure-service-health"></a>Azure Service Health
[Azure Service Health](../../azure-monitor/platform/service-notifications.md) bevat informatie over de status van de Azure-services in uw abonnement die uw toepassing en de resources afhankelijk van zijn. U kunt waarschuwingen om te worden geïnformeerd over de huidige en verwachte kritieke problemen die invloed kunnen zijn op uw toepassing kunt maken. Servicestatus records worden opgeslagen in de [Azure-activiteitenlogboek](../../azure-monitor/platform/activity-logs-overview.md), zodat u kunt ze in de activiteit Log Explorer weergeven en kopieer deze in Logboeken van Azure Monitor.

### <a name="azure-activity-log"></a>Azure-activiteitenlogboek
De [Azure Activity Log](../../azure-monitor/platform/activity-logs-overview.md) service medische dossiers, samen met records in de van configuratiewijzigingen aan uw Azure-resources bevat. Het activiteitenlogboek is beschikbaar voor alle Azure-resources en vertegenwoordigt de _externe_ weergeven. De specifieke typen records in het activiteitenlogboek worden beschreven in [gebeurtenisschema in het Azure-activiteitenlogboek](../../azure-monitor/platform/activity-log-schema.md).

U kunt het activiteitenlogboek voor een bepaalde resource weergeven op de pagina in de Azure portal of de weergave van de logboeken van meerdere resources in de [activiteit Log Explorer](../../azure-monitor/platform/activity-logs-overview.md). Het is met name handig voor het kopiëren van de logboekvermeldingen naar Azure Monitor te combineren met andere gegevens. U kunt ze ook verzenden naar andere locaties met behulp van [Event Hubs](../../azure-monitor/platform/activity-logs-stream-event-hubs.md).



## <a name="azure-services"></a>Azure-services
Metrische gegevens en resources niveau diagnostische logboeken bieden informatie over de _interne_ werking van Azure-resources. Dit zijn beschikbaar voor de meeste Azure-services en oplossingen bieden extra inzichten in bepaalde services.

![Azure-resource-verzameling](media/data-sources/azure-resource-collection.png)


### <a name="metrics"></a>Metrische gegevens
De meeste Azure-services wordt gegenereerd [platform metrische gegevens](data-collection.md#metrics) die overeenkomen de prestaties en de bewerking. De specifieke [metrische gegevens varieert voor elk type resource](../../azure-monitor/platform/metrics-supported.md).  Ze zijn toegankelijk is vanaf de Metrics explorer en kunnen worden gekopieerd naar Log Analytics voor trending en andere analyse.


### <a name="resource-diagnostic-logs"></a>Diagnostische logboeken van resource
Terwijl het activiteitenlogboek informatie over de bewerkingen die worden uitgevoerd op een Azure-resources, resourceniveau bevat [diagnostische logboeken](../../azure-monitor/platform/diagnostic-logs-overview.md) inzicht geven in de werking van de resource zelf.   De configuratievereisten en de inhoud van deze logboeken [varieert per resourcetype](../../azure-monitor/platform/tutorial-dashboards.md).

U kunt rechtstreeks logboeken met diagnostische gegevens niet weergeven in Azure portal, maar u kunt [ze verzenden naar Azure storage voor het archiveren van](../../azure-monitor/platform/archive-diagnostic-logs.md) en ze te exporteren [Event Hub](../../event-hubs/event-hubs-about.md) voor omleiding naar andere services, of [in logboek Analytics](../../azure-monitor/platform/diagnostic-logs-stream-log-store.md) voor analyse. Sommige resources rechtstreeks naar Log Analytics kunnen schrijven terwijl anderen naar een opslagaccount voordat u schrijven [geïmporteerd in Log Analytics](../../azure-monitor/platform/azure-storage-iis-table.md#use-the-azure-portal-to-collect-logs-from-azure-storage).

### <a name="monitoring-solutions"></a>Bewakingsoplossingen
 [Bewakingsoplossingen](../insights/solutions.md) verzamelen van gegevens voor meer inzicht in de werking van een bepaalde service of toepassing. Deze gegevens verzamelen in Logboeken van Azure Monitor waarbij het wellicht niet geanalyseerd op basis van de [querytaal](../log-query/log-query-overview.md) of [weergaven](view-designer.md) die gewoonlijk zijn opgenomen in de oplossing.

## <a name="guest-operating-system"></a>Gast-besturingssysteem
COMPUTE-resources in Azure, in andere clouds en on-premises hebben een gast-besturingssysteem om te controleren. U kunt met de installatie van een of meer agents, telemetrie verzamelen van de Gast in de dezelfde controleprogramma's als de Azure-services zelf.

![Verzameling van Azure compute-resource](media/data-sources/compute-resource-collection.png)

### <a name="azure-diagnostic-extension"></a>Diagnostische Azure-extensie
Met de Azure Diagnostics-extensie, biedt een basisniveau van de bewaking met het verzamelen van Logboeken en prestatiegegevens van het besturingssysteem van de client van Azure compute-resources.   

### <a name="log-analytics-agent"></a>Log Analytics-agent
Uitgebreide bewaking en beheer van uw Windows- of Linux-machines of fysieke computer wordt geleverd met de Log Analytics-agent. De virtuele machine kan worden uitgevoerd in Azure, een andere cloud of on-premises en de agent een verbinding maakt in Azure controleren rechtstreeks of via System Center Operations Manager en kunt u het verzamelen van gegevens uit [gegevensbronnen](agent-data-sources.md) die u hebt configureren of [bewakingsoplossingen](../insights/solutions.md) die u meer inzicht geven in toepassingen die worden uitgevoerd op de virtuele machine.

### <a name="dependency-agent"></a>Agent voor afhankelijkheden
[Serviceoverzicht](../insights/service-map.md) en [Azure Monitor voor virtuele machines](../insights/vminsights-overview.md) vereist een Agent voor afhankelijkheden op Windows en Linux-machines. Dit kan worden geïntegreerd met de Log Analytics-agent voor het verzamelen van gedetecteerde gegevens over de processen die worden uitgevoerd op de virtuele machine en een extern Procesafhankelijkheden. Het deze gegevens worden opgeslagen in Azure Monitor en visualiseert de gedetecteerde onderling verbonden onderdelen.  

Zie voor meer inzicht in de verschillen tussen de agents en te gebruiken, afhankelijk van uw bewakingsvereisten, [agents bewakingsoverzicht](agents-overview.md).

## <a name="applications"></a>Applicaties
Naast de telemetrie die uw toepassing naar het gastbesturingssysteem schrijven kan, gedetailleerde bewaking wordt gedaan met [Application Insights](https://docs.microsoft.com/azure/application-insights/). Application Insights kunnen gegevens verzamelen vanuit toepassingen die worden uitgevoerd op een aantal verschillende platformen. De toepassing kan worden uitgevoerd in Azure, een andere cloud of on-premises.

![Verzamelen van gegevens van toepassing](media/data-sources/application-collection.png)


### <a name="application-data"></a>Toepassingsgegevens
Wanneer u Application Insights voor een toepassing inschakelt door een instrumentatiepakket installeren, verzamelt deze metrische gegevens en logboeken met betrekking tot de prestaties en het gebruik van de toepassing. Dit omvat gedetailleerde informatie over paginaweergaven, aanvragen en uitzonderingen. Application Insights slaat de gegevens worden verzameld in Azure Monitor. Het uitgebreide hulpmiddelen voor het analyseren van deze gegevens bevat, maar u kunt deze ook analyseren met gegevens uit andere bronnen met behulp van hulpprogramma's, zoals metrische analytics en log analytics.

Ook kunt u Application Insights aan [maken van een aangepaste meetwaarde](../../azure-monitor/app/api-custom-events-metrics.md).  Hiermee kunt u uw eigen logica voor het berekenen van een numerieke waarde en vervolgens opslaan die waarde met andere metrische gegevens die kunnen worden geopend via de Metric Explorer en kan worden gebruikt voor het definiëren [voor automatisch schalen](../../azure-monitor/platform/autoscale-custom-metric.md) en metrische waarschuwingen.

### <a name="dependencies"></a>Afhankelijkheden
Als u wilt bewaken verschillende logische bewerkingen van een toepassing, moet u [verzamelen van telemetrie over meerdere onderdelen](../../application-insights/app-insights-transaction-diagnostics.md). Application Insights biedt ondersteuning [telemetriecorrelatie gedistribueerd](../../azure-monitor/app/correlation.md) waarin de afhankelijkheden tussen onderdelen, zodat u ze samen te analyseren.

### <a name="availability-tests"></a>Beschikbaarheidstests
[Beschikbaarheidstests](../../azure-monitor/app/monitor-web-app-availability.md) in Application Insights kunt u voor het testen van de beschikbaarheid en reactiesnelheid van uw toepassing vanaf verschillende locaties op het openbare Internet. U kunt een eenvoudige ping-test om te bevestigen dat de toepassing actief is of Visual Studio gebruiken voor het maken van een WebTest die een gebruikersscenario simuleert.  Beschikbaarheidstests vereisen instrumentatie in de toepassing.

## <a name="custom-sources"></a>Aangepaste bronnen
Naast de standaard-laag van een toepassing moet u mogelijk andere bronnen die telemetrie die niet kan worden verzameld met de andere gegevensbronnen hebben controleren. Voor deze resources moet u deze gegevens met behulp van een API van Azure Monitor te schrijven.

![Aangepaste gegevens verzamelen](media/data-sources/custom-collection.png)

### <a name="data-collector-api"></a>Gegevensverzamelaar-API
Azure Monitor kunt logboekgegevens verzamelen vanuit elke REST-client met behulp van de [Data Collector API](data-collector-api.md). Hiermee kunt u aangepaste bewakingsscenario's maken en uitbreiden van de bewaking aan resources die niet beschikbaar voor telemetrie via andere bronnen maken.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [typen gegevens te controleren die worden verzameld door Azure Monitor](data-collection.md) en hoe u kunt weergeven en analyseren van deze gegevens.
