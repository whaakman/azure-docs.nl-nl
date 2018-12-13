---
title: Intelligent Insights prestaties diagnoselogboek - Azure SQL Database | Microsoft Docs
description: Intelligent Insights biedt een logboekbestanden met diagnostische gegevens van prestatieproblemen met de Azure SQL Database
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: v-daljep
ms.reviewer: carlrab
manager: craigg
ms.date: 04/04/2018
ms.openlocfilehash: 2809dd45042e41c8337ecddccc76ec4e16d7cb8b
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2018
ms.locfileid: "52887692"
---
# <a name="use-the-intelligent-insights-azure-sql-database-performance-diagnostics-log"></a>Gebruik het diagnoselogboek voor Intelligent Insights Azure SQL Database-prestaties

Deze pagina vindt u informatie over het gebruik van de Azure SQL Database performance logboekbestanden met diagnostische gegevens die worden gegenereerd door [Intelligent Insights](sql-database-intelligent-insights.md), de indeling en de daarin opgenomen voor de ontwikkeling van uw aangepaste gegevens. U kunt deze logboekbestanden met diagnostische gegevens te verzenden [Azure Log Analytics](../azure-monitor/insights/azure-sql.md), [Azure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md), [Azure Storage](sql-database-metrics-diag-logging.md#stream-into-storage), of een oplossing van derden voor aangepaste DevOps waarschuwingen en rapporten mogelijkheden.

## <a name="log-header"></a>Logboekheader

De logboekbestanden met diagnostische gegevens wordt standaard JSON-indeling gebruikt om uit te voeren van Intelligent Insights bevindingen. De exacte categorie-eigenschap voor toegang tot een Intelligent Insights-logboek is het vaste waarde 'SQLInsights'.

De koptekst van het logboek is normaal en bestaat uit het tijdstempel (TimeGenerated) waarin wordt weergegeven wanneer een item is gemaakt. Dit omvat ook een resource-ID (ResourceId) die naar de specifieke SQL-Database het item is gekoppeld verwijst aan. De categorie (categorie), het niveau (niveau) en de naam van de bewerking (OperationName) zijn opgelost voor eigenschappen waarvan de waarden niet wijzigen. Ze geven aan dat de logboekvermelding informatief is en dat het afkomstig van Intelligent Insights (SQLInsights is).

```json
"TimeGenerated" : "2017-9-25 11:00:00", // time stamp of the log entry
"ResourceId" : "database identifier", // value points to a database resource
"Category": "SQLInsights", // fixed property
"Level" : "Informational", // fixed property
"OperationName" : "Insight", // fixed property
```

## <a name="issue-id-and-database-affected"></a>Probleem-ID en -database worden beïnvloed

De probleem-id-eigenschap (issueId_d) biedt een manier om een unieke bijhouden van problemen met prestaties tot opgelost. Meerdere gebeurtenis legt vast in het logboek voor rapportage over de status van hetzelfde probleem hebben dezelfde probleem-ID.

Samen met de probleem-ID rapporteert de logboekbestanden met diagnostische gegevens het begin (intervalStartTime_t) en de tijdstempels einde (intervalEndTme_t) van de gebeurtenis die betrekking hebben op een probleem dat in de logboekbestanden met diagnostische gegevens wordt gerapporteerd.

De eigenschap elastische pool (elasticPoolName_s) geeft aan welke elastische pool de database met een probleem behoort. Als de database geen deel uitmaken van een elastische pool, is deze eigenschap heeft geen waarde. De database waarin een probleem is gedetecteerd, wordt vermeld in de eigenschap van de database-naam (databaseName_s).

```json
"intervalStartTime_t": "2017-9-25 11:00", // start of the issue reported time stamp
"intervalEndTme_t":"2017-9-25 12:00", // end of the issue reported time stamp
"elasticPoolName_s" : "", // resource elastic pool (if applicable) 
"databaseName_s" : "db_name",  // database name
"issueId_d" : 1525, // unique ID of the issue detected
"status_s" : "Active" // status of the issue – possible values: "Active", "Verifying", and "Complete"
```

## <a name="detected-issues"></a>Gedetecteerde problemen

De volgende sectie van het logboekbestand voor Intelligent Insights-prestaties bevat prestatieproblemen die zijn gedetecteerd via ingebouwde kunstmatige intelligentie. Detecties worden vermeld in de eigenschappen in de JSON-diagnoselogboek. Deze detecties bestaan uit de categorie van een probleem, de impact van het probleem, de query's die worden beïnvloed en de metrische gegevens. De detectie-eigenschappen kunnen bevatten meerdere prestatieproblemen die zijn gedetecteerd.

De van de gedetecteerde prestatieproblemen worden gemeld met de volgende structuur van de detectie-eigenschap:

```json
"detections_s" : [{
"impact" : 1 to 3, // impact of the issue detected, possible values 1-3 (1 low, 2 moderate, 3 high impact)
"category" : "Detectable performance pattern", // performance issue detected, see the table
"details": <Details outputted> // details of an issue (see the table)
}] 
```

Detecteerbare prestatiepatronen en de details die zijn gegenereerd aan de logboekbestanden met diagnostische gegevens zijn opgegeven in de volgende tabel.

### <a name="detection-category"></a>Detectie van categorie

De eigenschap category (categorie) beschrijft de categorie van detecteerbare prestatiepatronen. Zie de volgende tabel voor alle mogelijke categorieën van detecteerbare prestatiepatronen. Zie voor meer informatie, [oplossen van prestatieproblemen met de database met Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md).

Afhankelijk van het prestatieprobleem gedetecteerd, de details in de diagnostische gegevens over output logboekbestand dienovereenkomstig verschillen.

| Detecteerbare prestatiepatronen | Details van output |
| :------------------- | ------------------- |
| Bronlimieten bereikt | <li>Betrokken resources</li><li>Query-hashes</li><li>Percentage van resource-verbruik</li> |
| Toename van de werkbelasting | <li>Aantal query's waarvan de uitvoering is verhoogd</li><li>Query-hashes van query's met de grootste bijdrage aan de toename van de werkbelasting</li> |
| Geheugendruk | <li>Geheugen-clerk</li> |
| Vergrendelen | <li>Query-hashes worden beïnvloed</li><li>Query-hashes blokkeren</li> |
| Verbeterde MAXDOP | <li>Query-hashes</li><li>CXP wachttijden</li><li>Wachttijden</li> |
| Pagelatch conflicten | <li>Query-hashes van query's veroorzaken conflicten</li> |
| Ontbrekende Index | <li>Query-hashes</li> |
| Nieuwe query | <li>Queryhash van de nieuwe query 's</li> |
| Ongebruikelijke wacht statistiek | <li>Ongebruikelijke wait-typen</li><li>Query-hashes</li><li>Query-wachttijden</li> |
| TempDB conflicten | <li>Query-hashes van query's veroorzaken conflicten</li><li>Query attribution aan de algehele database pagelatch conflicten wachttijd [%]</li> |
| Elastische pool-DTU tekort | <li>Elastische pool</li><li>Bovenste DTU verbruikt database</li><li>Procent van de groep van toepassingen die worden gebruikt door de bovenste consument DTU</li> |
| Regressie plannen | <li>Query-hashes</li><li>Goede plan-id 's</li><li>Ongeldige abonnement-id 's</li> |
| Het wijzigen van de database Scoped Configuration | <li>Database-scoped configuratiewijzigingen in vergelijking met de standaardwaarden</li> |
| Trage Client | <li>Query-hashes</li><li>Wachttijden</li> |
| Downgrade van laag-prijzen | <li>Tekstmelding</li> |

### <a name="impact"></a>Impact

De impact (invloed) eigenschap beschrijft hoeveel een gedetecteerd probleem hebben bijgedragen aan het probleem dat een database heeft. Heeft gevolgen voor het bereik van 1 tot 3, met 3 als de hoogste bijdrage, Gemiddeld, 2 en 1 als de bijdrage van de laagste. De waarde van de impact kan worden gebruikt als invoer voor aangepaste waarschuwingen automatisering, afhankelijk van uw specifieke behoeften. De eigenschap query's betrokken (QueryHashes) bieden een overzicht van de query-hashes die door een bepaalde detectie.

### <a name="impacted-queries"></a>Betrokken query's

De volgende sectie van het logboek Intelligent Insights bevat informatie over bepaalde query's die worden beïnvloed door de gedetecteerde prestatieproblemen. Deze informatie wordt vermeld als een matrix met objecten die zijn ingesloten in de eigenschap impact_s. De eigenschap gevolgen bestaat uit entiteiten en metrische gegevens. Entiteiten verwijst naar een bepaalde query (Type: Query). De unieke queryhash wordt vermeld onder de eigenschap value (waarde). Bovendien wordt elk van de query's vermeld gevolgd door een metrische waarde en een waarde die wijzen op een prestatieprobleem aangetroffen.

In het volgende voorbeeld voor het logboek worden de query's uitvoeren met de hash-0x9102EXZ4 is aangetroffen op een verbeterde duur van de uitvoering van (metrische gegevens: DurationIncreaseSeconds). De waarde van 110 seconden geeft aan dat deze bepaalde query 110 seconden langer duurde om uit te voeren. Omdat meerdere query's kunnen worden gedetecteerd, kan deze bepaalde uplogboekgedeelte meerdere query vermeldingen bevatten.

```json
"impact" : [{
"entity" : { 
"Type" : "Query", // type of entity - query
"Value" : "query hash value", // for example "0x9102EXZ4" query hash value },
"Metric" : "DurationIncreaseSeconds", // measured metric and the measurement unit (in this case seconds)
"Value" : 110 // value of the measured metric (in this case seconds)
}]
```

### <a name="metrics"></a>Metrische gegevens

De maateenheid voor alle gegevens die zijn gerapporteerd is opgegeven bij de eigenschap metrische gegevens (meeteenheid) met de mogelijke waarden van seconden, aantal en percentage. De waarde van een gemeten metrische waarde wordt vermeld in de eigenschap value (waarde).

De eigenschap DurationIncreaseSeconds bevat de maateenheid in seconden. De CriticalErrorCount maateenheid is een getal dat staat voor een aantal fouten.

```json
"metric" : "DurationIncreaseSeconds", // issue metric type – possible values: DurationIncreaseSeconds, CriticalErrorCount, WaitingSeconds
"value" : 102 // value of the measured metric (in this case seconds)
```

## <a name="root-cause-analysis-and-improvement-recommendations"></a>Hoofdmap oorzaak aanbevelingen voor analyse en verbetering van de gebruikerservaring

Het laatste deel van het logboek van de prestaties van Intelligent Insights heeft betrekking op de geautomatiseerde hoofdoorzaakanalyses van het geïdentificeerde degradatie prestatieprobleem. De informatie wordt weergegeven in human-vriendelijk bewoordingen in de eigenschap root oorzaak analysis (rootCauseAnalysis_s). Aanbevelingen voor prestatieverbetering zijn opgenomen in het logboek waar mogelijk.

```json
// example of reported root cause analysis of the detected performance issue, in a human-readable format

"rootCauseAnalysis_s" : "High data IO caused performance to degrade. It seems that this database is missing some indexes that could help."
```

U kunt het logboekbestand voor de prestaties van Intelligent Insights met [Azure Log Analytics]( https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql) of een oplossing van derden voor aangepaste DevOps waarschuwingen en rapportagemogelijkheden.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Intelligent Insights](sql-database-intelligent-insights.md) concepten.
- Meer informatie over het [oplossen van prestatieproblemen met de Azure SQL Database met intelligente inzichten](sql-database-intelligent-insights-troubleshoot-performance.md).
- Meer informatie over het [Azure SQL Database controleren met behulp van Azure SQL Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql).
- Meer informatie over het [verzamelen en gebruiken van logboekgegevens van uw Azure-resources](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).



