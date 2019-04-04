---
title: Logboeken in Azure Monitor | Microsoft Docs
description: Hierin wordt beschreven in Logboeken in Azure Monitor die worden gebruikt voor geavanceerde analyse van gegevens te controleren.
documentationcenter: ''
author: bwren
manager: carmonm
editor: tysonn
ms.service: monitoring
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 0203/26/2019
ms.author: bwren
ms.openlocfilehash: a7271aa3faf438b42319f8c2c297c6e39baab92e
ms.sourcegitcommit: 9f4eb5a3758f8a1a6a58c33c2806fa2986f702cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58904148"
---
# <a name="logs-in-azure-monitor"></a>Logboeken in Azure Monitor

> [!NOTE]
> Alle gegevens die zijn verzameld door Azure Monitor in een van twee fundamentele typen, logboeken en metrische gegevens past. Dit artikel beschrijft de logboeken. Raadpleeg [metrische gegevens in Azure Monitor](data-platform-metrics.md) voor een gedetailleerde beschrijving van metrische gegevens en zo [door gegevens te controleren die worden verzameld door Azure Monitor](data-platform.md) voor een vergelijking van de twee.

Logboeken in Azure Monitor zijn met name nuttig voor het uitvoeren van complexe analyse van gegevens uit een groot aantal bronnen. Dit artikel wordt beschreven hoe de logboeken in Azure Monitor, kunt u doen met de gegevens, gestructureerd en identificeert van verschillende gegevensbronnen die gegevens opslaan in Logboeken.

> [!NOTE]
> Het is belangrijk onderscheid maken tussen Azure Monitor-logboeken en bronnen van logboekgegevens in Azure. Bijvoorbeeld, abonnement op gebeurtenissen in Azure worden geschreven naar een [activiteitenlogboek](activity-logs-overview.md) die u kunt bekijken in het menu Azure Monitor. De meeste resources schrijft operationele gegevens naar een [diagnostisch logboek](diagnostic-logs-overview.md) die u kunt doorsturen naar verschillende locaties. Logboeken in Azure Monitor is een platform voor logboeken die worden verzameld van activiteitenlogboeken en diagnostische logboeken samen met andere bewakingsgegevens voor geavanceerde analyse voor de volledige set van resources.

## <a name="what-are-azure-monitor-logs"></a>Wat zijn de logboeken van Azure Monitor?

Logboeken in Azure Monitor bevatten verschillende soorten gegevens die zijn ingedeeld in records met verschillende sets van eigenschappen voor elk type. Logboeken kunnen numerieke waarden, zoals Azure Monitor Metrics bevatten maar bevatten doorgaans tekstgegevens met gedetailleerde beschrijvingen. Ze verder verschillen van metrische gegevens in dat ze in de structuur ervan variëren en vaak niet verzameld met regelmatige intervallen worden. Telemetrie, zoals gebeurtenissen en traceringen worden opgeslagen logboeken van Azure Monitor naast prestatiegegevens zodat alle dat kan worden gecombineerd voor analyse.

Een gemeenschappelijk type logboekvermelding is een gebeurtenis, sporadisch worden verzameld. Gebeurtenissen worden gemaakt door een toepassing of service en zijn meestal voldoende informatie gegeven om een volledige context te bieden op hun eigen. Een gebeurtenis kan bijvoorbeeld duiden dat een bepaalde resource is gemaakt of gewijzigd, een nieuwe host gestart als reactie op toegenomen verkeer, of er is een fout gedetecteerd in een toepassing.

 Omdat de indeling van de gegevens variëren kan, kunnen toepassingen aangepaste logboeken kunnen maken met behulp van de structuur die ze nodig hebben. Metrische gegevens kunnen ook worden opgeslagen in Logboeken om ze te combineren met andere bewakingsgegevens voor trending en andere data-analyse.


## <a name="what-can-you-do-with-azure-monitor-logs"></a>Wat kunt u doen met Azure Monitor-Logboeken
De volgende tabel bevat de verschillende manieren dat u Logboeken in Azure Monitor gebruiken kunt.


|  |  |
|:---|:---|
| Analyseren | Gebruik [Log Analytics](../log-query/get-started-portal.md) in Azure portal om te schrijven [query's bijgehouden](../log-query/log-query-overview.md) en interactief analyseren logboekgegevens met behulp van de krachtige analyse-engine van Data Explorer.<br>Gebruik de [Application Insights analytics-console](../app/analytics.md) in Azure portal om te schrijven logboeken-query's en analyseren van logboekgegevens van Application Insights. |
| Visualiseren | Queryresultaten weergegeven als tabellen of grafieken vastmaken een [Azure-dashboard](../../azure-portal/azure-portal-dashboards.md).<br>Maak een [werkmap](../app/usage-workbooks.md) te combineren met meerdere sets met gegevens in een interactieve rapport. <br>De resultaten van een query voor het exporteren [Power BI](powerbi.md) verschillende visualisaties gebruiken en delen met gebruikers buiten Azure.<br>De resultaten van een query voor het exporteren [Grafana](grafana-plugin.md) gebruikmaken van de dashboarding en combineren met andere gegevensbronnen.|
| Waarschuwing | Configureren van een [waarschuwingsregel](alerts-log.md) die duurt of verzendt een melding [automatische actie](action-groups.md) wanneer de resultaten van de query overeenkomt met een bepaalde resultaat.<br>Configureer een [waarschuwingsregel voor metrische gegevens](alerts-metric-logs.md) op bepaalde log gegevenslogboeken geëxtraheerd als metrische gegevens. |
| Ophalen | Toegang tot log queryresultaten vanuit een opdrachtregel met [Azure CLI](/azure/ext/log-analytics/monitor/log-analytics).<br>Toegang tot log queryresultaten vanuit een opdrachtregel met [PowerShell-cmdlets](https://docs.microsoft.com/powershell/module/az.operationalinsights).<br>Toegang tot log queryresultaten vanuit een aangepaste toepassing met [REST-API](https://dev.loganalytics.io/). |
| Exporteren | Een werkstroom voor het ophalen van logboekgegevens en kopieer deze naar een externe locatie met bouwen [Logic Apps](~/articles/logic-apps/index.yml). |


## <a name="how-is-data-in-azure-monitor-logs-structured"></a>Hoe zijn de gegevens in Azure controleren logboeken gestructureerd?
Gegevens die worden verzameld door Azure Monitor-logboeken worden opgeslagen in een [Log Analytics-werkruimte](../platform/manage-access.md). U kunt [meerdere werkruimten maken](manage-access.md#determine-the-number-of-workspaces-you-need) in uw abonnement voor het beheren van verschillende sets van logboekgegevens. Elke werkruimte bevat meerdere tabellen dat elk opslaan van gegevens van een bepaalde bron. Hoewel alle tabellen delen [enkele algemene eigenschappen](log-standard-properties.md), allemaal een unieke set eigenschappen, afhankelijk van het soort gegevens dat wordt opgeslagen. Een nieuwe werkruimte wordt standaard set met tabellen, en meer tabellen worden toegevoegd door verschillende bewakingsoplossingen en andere services die naar de werkruimte schrijven.

Logboekgegevens van Application Insights gebruikt dezelfde Log Analytics-engine als werkruimten, maar deze afzonderlijk voor elke bewaakte toepassing opgeslagen. Elke toepassing heeft een standaardset aan tabellen voor het opslaan van gegevens, zoals aanvragen, uitzonderingen en paginaweergaven.

Logboeken-query's worden gegevens uit een Log Analytics-werkruimte gebruiken ofwel een Application Insights-toepassing. U kunt een [meerdere bronnen query](../log-query/cross-workspace-query.md) voor het analyseren van gegevens, samen met andere gegevens van een toepassing of query's, met inbegrip van meerdere werkruimten of toepassingen maken.

![Workspaces](media/data-platform-logs/workspaces.png)

## <a name="log-queries"></a>Logboekquery's
Gegevens in Logboeken van Azure Monitor worden opgehaald met behulp van een [logboekquery](../log-query/log-query-overview.md) die zijn geschreven met de [Kusto-querytaal](../log-query/get-started-queries.md), waarmee u snel ophalen, samenvoegen en analyseren van verzamelde gegevens. Gebruik [Log Analytics](../log-query/portals.md) schrijven en testen van Logboeken-query's in Azure portal. Hiermee kunt u interactief werken met resultaten of vastmaken aan een dashboard om deze met andere visualisaties weer te geven.

![Log Analytics](media/data-platform-logs/log-analytics.png)

Open [Log Analytics van Application Insights](../app/analytics.md) om Application Insights-gegevens te analyseren.

![Application Insights Analytics](media/data-platform-logs/app-insights-analytics.png)

U kunt ook logboekgegevens ophalen met behulp van de [Log Analytics API](https://dev.loganalytics.io/documentation/overview) en de [Application Insights REST-API](https://dev.applicationinsights.io/documentation/overview).


## <a name="sources-of-azure-monitor-logs"></a>Bronnen van Logboeken van Azure Monitor
Azure Monitor kunt logboekgegevens verzamelen uit een verscheidenheid aan bronnen binnen Azure en on-premises bronnen. De volgende tabellen worden de verschillende gegevensbronnen beschikbaar vanuit verschillende bronnen die gegevens naar de logboeken van Azure Monitor schrijven. Elk heeft een koppeling naar meer informatie over de vereiste configuratie.

### <a name="azure-tenant-and-subscription"></a>Azure-tenant en -abonnement

| Gegevens | Description |
|:---|:---|
| Auditlogboeken van Azure Active Directory | Geconfigureerd via diagnostische instellingen voor elke map. Zie [logboeken van Azure AD integreren met Azure Monitor logboeken](../../active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md).  |
| Activiteitenlogboeken | Standaard afzonderlijk opgeslagen en kan worden gebruikt voor bijna realtime waarschuwingen. Installeer de oplossing Activity Log Analytics om te schrijven naar Log Analytics-werkruimte. Zie [verzamelen en analyseren van Azure-activiteitenlogboeken in Log Analytics](collect-activity-logs.md). |

### <a name="azure-resources"></a>Azure-resources

| Gegevens | Description |
|:---|:---|
| Diagnostische gegevens van bron | Diagnostische instellingen schrijven om diagnostische gegevens, met inbegrip van metrische gegevens naar Log Analytics-werkruimte te configureren. Zie [diagnostische logboeken naar Log Analytics in Azure Stream](diagnostic-logs-stream-log-store.md). |
| Bewakingsoplossingen | Bewakingsoplossingen schrijven gegevens verzamelen ze voor hun Log Analytics-werkruimte. Zie [details van de verzameling gegevens voor de beheeroplossingen in Azure](../insights/solutions-inventory.md) voor een overzicht van oplossingen. Zie [bewakingsoplossingen in Azure Monitor](../insights/solutions.md) voor meer informatie over het installeren en gebruiken van oplossingen. |
| Metrische gegevens | Platform metrische gegevens voor resources van Azure Monitor verzenden naar Log Analytics-werkruimte aan het logboekgegevens bewaren gedurende langere perioden en complexe analyses uitvoeren met andere gegevenstypen met behulp van de [Kusto-querytaal](/azure/kusto/query/). Zie [diagnostische logboeken naar Log Analytics in Azure Stream](diagnostic-logs-stream-log-store.md). |
| Azure-tabelopslag | Verzamelen van gegevens uit Azure storage, waarbij sommige Azure-resources schrijven door gegevens te controleren. Zie [gebruikt Azure blob-opslag voor IIS en Azure table storage voor gebeurtenissen met Log Analytics](azure-storage-iis-table.md). |

### <a name="virtual-machines"></a>Virtuele machines

| Gegevens | Description |
|:---|:---|
|  Agent-gegevensbronnen | Gegevensbronnen die worden verzameld van [Windows](agent-windows.md) en [Linux](../learn/quick-collect-linux-computer.md) agents zijn gebeurtenissen, prestatiegegevens en aangepaste logboeken. Zie [Agent gegevensbronnen in Azure Monitor](data-sources.md) voor een lijst van gegevensbronnen en gegevens over de configuratie. |
| Bewakingsoplossingen | Bewakingsoplossingen schrijven gegevens dat ze verzamelen van agents aan de Log Analytics-werkruimte. Zie [details van de verzameling gegevens voor de beheeroplossingen in Azure](../insights/solutions-inventory.md) voor een overzicht van oplossingen. Zie [bewakingsoplossingen in Azure Monitor](../insights/solutions.md) voor meer informatie over het installeren en gebruiken van oplossingen. |
| System Center Operations Manager | Operations Manager-beheergroep verbinden met Azure Monitor voor het verzamelen van gebeurtenissen en prestatiegegevens van on-premises agents in Logboeken. Zie [Operations Manager verbinden met Log Analytics](om-agents.md) voor meer informatie over deze configuratie. |


### <a name="applications"></a>Applicaties

| Gegevens | Description |
|:---|:---|
| Aanvragen en uitzonderingen | Gedetailleerde gegevens over aanvragen en uitzonderingen zijn de _aanvragen_, _pageViews_, en _uitzonderingen_ tabellen. Aanroepen naar [externe onderdelen](../app/asp-net-dependencies.md) zijn de _afhankelijkheden_ tabel. |
| Gebruik en prestaties | Prestaties voor de toepassing is beschikbaar in de _aanvragen_, _browserTimings_ en _performanceCounters_ tabellen. Gegevens voor [aangepaste metrische gegevens](../app/api-custom-events-metrics.md#trackevent) is in de _customMetrics_ tabel.|
| Gegevens traceren | Resultaat is van een [gedistribueerde tracering](/app/distributed-tracing) worden opgeslagen in de _traceringen_ tabel. |
| Beschikbaarheidstests | Gegevens van de samenvatting van [beschikbaarheidstests](/app/monitor-web-app-availability) wordt opgeslagen in de _availabilityResults_ tabel. Gedetailleerde gegevens van deze tests worden in afzonderlijke opslag en Application Insights in Azure portal gebruikt. |

### <a name="insights"></a>Inzichten

| Gegevens | Description |
|:---|:---|
| Azure Monitor voor Containers | Inventaris-en prestatiegegevens verzameld door [Azure Monitor voor containers](../insights/container-insights-overview.md). Zie [Container gegevensverzameling details](../insights/container-insights-analyze.md#container-data-collection-details) voor een lijst van de tabellen. |
| Azure Monitor voor virtuele machines | Map-en prestatiegegevens verzameld door [Azure Monitor voor virtuele machines](../insights/vminsights-overview.md). Zie [hoe u logboeken van Azure Monitor opvragen voor virtuele machines](../insights/vminsights-log-search.md) voor meer informatie over het opvragen van deze gegevens. |

### <a name="custom"></a>Aangepast telefoonnummer 

| Gegevens | Beschrijving |
|:---|:---|
| REST-API | Gegevens schrijven naar Log Analytics-werkruimte van een REST-client. Zie [logboekgegevens verzenden naar Azure Monitor met de API HTTP Data Collector](data-collector-api.md) voor meer informatie.
| Logische apps | Geen gegevens schrijven naar Log Analytics-werkruimte van een werkstroom voor logische App met de **Azure Log Analytics-gegevensverzamelaar** actie. |

### <a name="security"></a>Beveiliging

| Gegevens | Description |
|:---|:---|
| Azure Security Center | [Azure Security Center](/azure/security-center/) worden gegevens opgeslagen die worden verzameld in een Log Analytics-werkruimte waar deze kan worden geanalyseerd met andere logboekgegevens. Zie [verzamelen van gegevens in Azure Security Center](../../security-center/security-center-enable-data-collection.md) voor meer informatie over de configuratie van de standaardwerkruimte. |
| Azure Sentinel | [Azure Sentinel](/azure/sentinel/) gegevens uit gegevensbronnen worden opgeslagen in een Log Analytics-werkruimte. Zie [](/sentinel/connect-data-sources.md)  |


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [gegevensplatform in de Azure Monitor](data-platform.md).
- Meer informatie over [metrische gegevens in Azure Monitor](data-platform-metrics.md).
- Meer informatie over de [bewakingsgegevens beschikbaar](data-sources.md) voor verschillende resources in Azure.
