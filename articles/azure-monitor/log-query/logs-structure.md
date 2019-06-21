---
title: Structuur van de logboeken van Azure Monitor | Microsoft Docs
description: U moet een logboekquery voor het ophalen van logboekgegevens van Azure Monitor.  In dit artikel wordt beschreven hoe u met nieuwe logboekbestanden query's worden gebruikt in Azure Monitor en concepten die u nodig hebt om te begrijpen voordat deze is gemaakt.
services: log-analytics
author: bwren
ms.service: log-analytics
ms.topic: conceptual
ms.date: 06/16/2019
ms.author: bwren
ms.openlocfilehash: e243ebbc31f9e941678ac76a83738276995b5ba1
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67297189"
---
# <a name="structure-of-azure-monitor-logs"></a>Structuur van de logboeken van Azure Monitor
De mogelijkheid om snel inzicht verkrijgen in uw gegevens met een [logboekquery](log-query-overview.md) is een krachtige functie van Azure Monitor. U moet enkele fundamentele concepten, zoals waar de gegevens die u wilt zich bevindt en hoe deze wordt opgebouwd begrijpen voor het maken van efficiënte en handige query's. Dit artikel bevat de basisconcepten, moet u aan de slag.

## <a name="overview"></a>Overzicht
Gegevens in Azure controleren logboeken worden opgeslagen in een Log Analytics-werkruimte of in een Application Insights-toepassing. Beide worden aangestuurd door [Azure Data Explorer](/azure/data-explorer/) wat betekent dat ze gebruikmaken van de krachtige en query-engine-taal.

Gegevens in beide werkruimten en toepassingen zijn onderverdeeld in tabellen, die elk verschillende soorten gegevens worden opgeslagen en heeft een eigen unieke set eigenschappen. De meeste [gegevensbronnen](../platform/data-sources.md) wordt geschreven naar hun eigen tabellen in een Log Analytics-werkruimte, terwijl Application Insights wordt geschreven naar een vooraf gedefinieerde set met tabellen in een Application Insights-toepassing. Logboeken-query's zijn zeer flexibel zodat u kunt eenvoudig gegevens uit meerdere tabellen combineren en zelfs een query meerdere bronnen gebruiken om gegevens uit de tabellen in meerdere werkruimten te combineren, of het schrijven van query's die werkruimte en de toepassing gegevens combineren.

De volgende afbeelding ziet u voorbeelden van gegevensbronnen die naar verschillende tabellen die worden gebruikt in de voorbeeldquery's schrijven.

![Tabellen](media/logs-structure/queries-tables.png)

## <a name="log-analytics-workspace"></a>Log Analytics-werkruimte
Alle gegevens die zijn verzameld door Azure Monitor-Logboeken, met uitzondering van Application Insights wordt opgeslagen in een [Log Analytics-werkruimte](../platform/manage-access.md). U kunt een of meer werkruimten maken, afhankelijk van uw specifieke vereisten. [Gegevensbronnen](../platform/data-sources.md) zoals activiteitenlogboeken en diagnostische logboeken van Azure-resources, agents op virtuele machines en gegevens van inzichten en bewakingsoplossingen gegevens worden geschreven naar een of meer werkruimten die u als onderdeel van hun onboarding configureert. Andere services zoals [Azure Security Center](/azure/security-center/) en [Azure Sentinel](/azure/sentinel/) ook een Log Analytics-werkruimte gebruiken om op te slaan hun gegevens, zodat ze kan worden geanalyseerd met behulp van Logboeken-query's samen met gegevens van andere te controleren bronnen.

Verschillende soorten gegevens worden opgeslagen in verschillende tabellen in de werkruimte en elke tabel heeft een unieke set eigenschappen. Een standaardset aan tabellen aan een werkruimte worden toegevoegd wanneer deze gemaakt, en nieuwe tabellen worden toegevoegd voor verschillende gegevensbronnen, oplossingen en services zoals ze zijn toegevoegd. U kunt ook maken met aangepaste tabellen met behulp van de [Data Collector API](../platform/data-collector-api.md).

U kunt bladeren in de tabellen in een werkruimte en hun schema in de **Schema** tabblad in Log Analytics voor de werkruimte.

![Schema van de werkruimte](media/scope/workspace-schema.png)

Gebruik de volgende query uit om de lijst met de tabellen in de werkruimte en het aantal records verzameld in elk in de vorige dag. 

```Kusto
union withsource = table * 
| where TimeGenerated > ago(1d)
| summarize count() by table
| sort by table asc
```
Zie de documentatie voor elke gegevensbron voor meer informatie over het maken van tabellen. Voorbeelden zijn onder meer artikelen voor [agent gegevensbronnen](../platform/agent-data-sources.md), [diagnostische logboeken](../platform/diagnostic-logs-schema.md), en [bewakingsoplossingen](../insights/solutions-inventory.md).

### <a name="workspace-permissions"></a>Machtigingen voor de werkruimte
Zie [machtigingen voor de werkruimte en het bereik](../platform/manage-access.md#workspace-permissions-and-scope) voor meer informatie over toegang tot de gegevens in een werkruimte. Naast het verlenen van toegang tot de werkruimte zelf, kunt u beperken van toegang tot afzonderlijke tabellen met behulp van [tabel niveau RBAC](../platform/manage-access.md#table-level-rbac).

## <a name="application-insights-application"></a>Application Insights-toepassing
Wanneer u een toepassing in Application Insights maakt, wordt een bijbehorende toepassing wordt automatisch gemaakt in Azure Monitor-Logboeken. Er is geen configuratie vereist om gegevens te verzamelen en de toepassing automatisch wordt geschreven gegevens, zoals paginaweergaven, aanvragen en uitzonderingen te controleren.

In tegenstelling tot een Log Analytics-werkruimte heeft een Application Insights-toepassing een vaste set met tabellen. U kunt gegevens van andere bronnen om te schrijven naar de toepassing, zodat er geen aanvullende tabellen kunnen worden gemaakt niet configureren. 

| Tabel | Description | 
|:---|:---|
| availabilityResults | Gegevens van de samenvatting van de beschikbaarheidstests. |
| browserTimings      | Gegevens over de prestaties van de client, zoals de tijd die nodig om de binnenkomende gegevens te verwerken. |
| customEvents        | Aangepaste gebeurtenissen die zijn gemaakt door uw toepassing. |
| customMetrics       | Aangepaste metrische gegevens die zijn gemaakt door uw toepassing. |
| Afhankelijkheden        | Aanroepen van de toepassing naar externe onderdelen. |
| Uitzonderingen          | De uitzonderingen die door de runtime voor de toepassing. |
| pageViews           | Gegevens over elke website met de browser informatie weergeven. |
| PerformanceCounters | De metingen van de prestaties van de compute-resources voor het ondersteunen van de toepassing. |
| aanvragen            | De details van elke toepassingsaanvraag.  |
| traceringen              | Resultaten van gedistribueerde tracering. |

Vindt u het schema voor elke tabel in de **Schema** tabblad in Log Analytics voor de toepassing.

![Toepassingsschema](media/scope/application-schema.png)

## <a name="standard-properties"></a>Standaardeigenschappen
Elke tabel in de logboeken van Azure Monitor heeft een eigen schema, maar er zijn standaard eigenschappen die worden gedeeld door alle tabellen. Zie [standaardeigenschappen in Azure controleren logboeken](../platform/log-standard-properties.md) voor details van elk.

| Log Analytics-werkruimte | Application Insights-toepassing | Description |
|:---|:---|:---|
| TimeGenerated | timestamp  | De datum en tijd waarop die de record is gemaakt. |
| Type          | itemType   | Naam van de tabel die de record is opgehaald uit. |
| _ResourceId   |            | De unieke id voor de resource van de record is gekoppeld. |
| _IsBillable   |            | Hiermee geeft u op of opgenomen gegevens betaald zijn. |
| _BilledSize   |            | Hiermee geeft u de grootte in bytes aan gegevens dat wordt in rekening gebracht. |

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over het gebruik van [Log Analytics maken en bewerken van zoekopdrachten](../log-query/portals.md).
- Bekijk een [zelfstudie over het schrijven van query's](../log-query/get-started-queries.md) met behulp van de nieuwe querytaal.
