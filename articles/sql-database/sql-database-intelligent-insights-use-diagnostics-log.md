---
title: Intelligent Insights diagnostics prestatielogboek - Azure SQL Database | Microsoft Docs
description: Intelligent Insights voorziet in een logboek diagnostische gegevens van prestatieproblemen met de Azure SQL Database
services: sql-database
documentationcenter: 
author: danimir
manager: drasumic
editor: carlrab
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Inactive
ms.date: 09/25/2017
ms.author: v-daljep
ms.openlocfilehash: e147ca57658c232580fd9867790e55c3454e7e05
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="use-the-intelligent-insights-azure-sql-database-performance-diagnostics-log"></a>De Intelligent Insights Azure SQL Database prestaties diagnostische logboeken gebruikt

Deze pagina bevat informatie over het gebruik van de Azure SQL Database prestaties diagnostics logboek gegenereerd door [Intelligent Insights](sql-database-intelligent-insights.md), de indeling en de gegevens voor uw aangepaste ontwikkeling. U kunt dit logboek diagnostische gegevens naar verzenden [Azure Log Analytics](../log-analytics/log-analytics-azure-sql.md), [Azure Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md), [Azure Storage](sql-database-metrics-diag-logging.md#stream-into-storage), of een oplossing van derden voor aangepaste DevOps waarschuwingen en rapportages mogelijkheden.

## <a name="log-header"></a>Logboekheader

Het logboek diagnostics gebruikt standaard JSON-indeling naar uitvoer Intelligent Insights bevindingen. De eigenschap exacte categorie voor toegang tot een logboek Intelligent Insights is de vaste waarde 'SQLInsights'.

De koptekst van het logboek is gebruikelijk en bestaat uit de tijdstempel (TimeGenerated) die wordt aangegeven wanneer er een vermelding is gemaakt. Dit omvat ook een resource-ID (ResourceId) die naar de vermelding is gekoppeld verwijst aan de specifieke SQL-Database. De categorie (categorie), het niveau (niveau) en de naam van de bewerking (OperationName) vast eigenschappen waarvan de waarden niet wijzigen. Ze geven aan dat de logboekvermelding informatief is en dat ze afkomstig zijn van Intelligent Insights (SQLInsights).

```json
"TimeGenerated" : "2017-9-25 11:00:00", // time stamp of the log entry
"ResourceId" : "database identifier", // value points to a database resource
"Category": "SQLInsights", // fixed property
"Level" : "Informational", // fixed property
"OperationName" : "Insight", // fixed property
```

## <a name="issue-id-and-database-affected"></a>Probleem-ID en van invloed op een database

De eigenschap in probleem-id (issueId_d) biedt een manier voor het unieke prestatieproblemen bijhouden totdat ze zijn opgelost. Intelligent Insights toetsenbordinvoer elk probleem lifecycle als 'Active', 'Controleren' of 'Voltooid'. Door elk van deze fasen status vastleggen Intelligent Insights meerdere gebeurtenisrecords in het logboek. Voor elk van deze vermeldingen blijft het probleem id-nummer uniek zijn. Intelligent Insights houdt het probleem gedurende de levensduur en genereert een beter inzicht in het logboek voor diagnostische gegevens om de 15 minuten.

Nadat een prestatieprobleem wordt gedetecteerd en voor zo lang duurt, het probleem wordt gerapporteerd als 'Actief' onder de statuseigenschap (status_s). Nadat een gedetecteerde probleem is verholpen, heeft gecontroleerd en gerapporteerd als 'Controleren' onder de statuseigenschap (status_s). Als het probleem zich niet langer aanwezig, rapporteert de statuseigenschap (status_s) dit probleem als 'Voltooid'.

Het logboek diagnostics rapporten samen met de ID van het probleem, de begintijd (intervalStartTime_t) en de tijdstempels einde (intervalEndTme_t) van de specifieke gebeurtenis met betrekking tot een probleem dat wordt gerapporteerd in het logboek voor diagnostische gegevens.

De eigenschap elastische pool (elasticPoolName_s) geeft aan welke elastische groep die de database van een probleem behoort. Als de database geen deel uitmaakt van een elastische pool, is deze eigenschap heeft geen waarde. De database waarin een probleem is gedetecteerd wordt in de database-naameigenschap (databaseName_s) vermeld.

```json
"intervalStartTime_t": "2017-9-25 11:00", // start of the issue reported time stamp
"intervalEndTme_t":"2017-9-25 12:00", // end of the issue reported time stamp
"elasticPoolName_s" : "", // resource elastic pool (if applicable) 
"databaseName_s" : "db_name",  // database name
"issueId_d" : 1525, // unique ID of the issue detected
"status_s" : "Active" // status of the issue – possible values: "Active", "Verifying", and "Complete"
```

## <a name="detected-issues"></a>Gedetecteerde problemen

De volgende sectie van het logboek van de prestaties Intelligent Insights bevat prestatieproblemen die zijn gedetecteerd via ingebouwde kunstmatige intelligence. Detecties worden vermeld in de eigenschappen in het logboek van JSON diagnostische gegevens. Deze detecties bestaan uit de categorie van een probleem, de impact van het probleem en de query's die van invloed op de metrische gegevens. De eigenschappen detecties bevat mogelijk meerdere prestatieproblemen die zijn gedetecteerd.

Gedetecteerde prestatieproblemen worden gerapporteerd met de volgende detecties eigenschap structuur:

```json
"detections_s" : [{
"impact" : 1 to 3, // impact of the issue detected, possible values 1-3 (1 low, 2 moderate, 3 high impact)
"category" : "Detectable performance pattern", // performance issue detected, see the table
"details": <Details outputted> // details of an issue (see the table)
}] 
```

Prestatiepatronen waarneembaar en de details die zijn gegenereerd in het logboek van diagnostische gegevens vindt u in de volgende tabel.

### <a name="detection-category"></a>Detectie-categorie

De eigenschap category (categorie) beschrijving van de categorie van waarneembaar prestatiepatronen. Zie de volgende tabel voor alle mogelijke categorieën van waarneembaar prestatiepatronen. Zie voor meer informatie [oplossen van prestatieproblemen met de database met Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md).

Afhankelijk van het prestatieprobleem gedetecteerd, de details in de diagnostische gegevens output logboekbestand dienovereenkomstig verschillen.

| Prestatiepatronen waarneembaar | Details output |
| :------------------- | ------------------- |
| Limieten bereiken | <li>Resources die van invloed op een</li><li>Query-hashes</li><li>Percentage van resource-verbruik</li> |
| Toename van de werkbelasting | <li>Aantal query's waarvan de uitvoering verhoogd</li><li>Query-hashes van query's met de grootste bijdrage aan de toename van de werkbelasting</li> |
| Geheugendruk | <li>Geheugen-clerk</li> |
| Vergrendelen | <li>Query-hashes van invloed op</li><li>Query-hashes worden geblokkeerd</li> |
| Verbeterde MAXDOP | <li>Query-hashes</li><li>Wachttijden CXP</li><li>Wachttijden</li> |
| Pagelatch conflicten | <li>Query-hashes van query's veroorzaken conflicten</li> |
| Ontbrekende Index | <li>Query-hashes</li> |
| Nieuwe query | <li>Query-hash van de nieuwe query 's</li> |
| Ongebruikelijke wacht statistiek | <li>Ongebruikelijke wacht typen</li><li>Query-hashes</li><li>Wachttijden query</li> |
| TempDB conflicten | <li>Query-hashes van query's veroorzaken conflicten</li><li>Toekenning van de query voor de algehele database pagelatch conflicten wachttijd [%]</li> |
| DTU tekort aan resources van elastische groep | <li>Elastische pool</li><li>Bovenste DTU verbruikende database</li><li>Percentage van de groep van toepassingen die worden gebruikt door de bovenste consument DTU</li> |
| Regressie plannen | <li>Query-hashes</li><li>Goed plan-id 's</li><li>Ongeldige abonnement-id 's</li> |
| Database-Scoped waarde configuratiewijziging | <li>Database-scoped configuratiewijzigingen vergeleken met de standaardwaarden</li> |
| Trage Client | <li>Query-hashes</li><li>Wachttijden</li> |
| Laag Downgrade prijzen | <li>Tekstmelding</li> |

### <a name="impact"></a>Impact

De impact (impact) eigenschap beschrijft hoeveel gedetecteerde gedrag hebben bijgedragen aan het probleem dat een database heeft. Effecten bereik van 1 tot 3, 3 al de hoogste bijdrage, 2 als matig en 1 als de laagste bijdrage. De waarde van invloed kan worden gebruikt als invoer voor aangepaste waarschuwingen automation, afhankelijk van uw specifieke behoeften. De eigenschap query's betrokken (QueryHashes) bieden een lijst van de query hashes die is van invloed op een bepaalde detectie.

### <a name="impacted-queries"></a>Betrokken query 's

De volgende sectie van het logboek Intelligent Insights bevat informatie over bepaalde query's die is van invloed op de gedetecteerde prestatieproblemen. Deze informatie wordt vermeld als een matrix met objecten die zijn ingesloten in de eigenschap impact_s. De eigenschap invloed bestaat uit entiteiten en metrische gegevens. Entiteiten verwijst naar een bepaalde query (Type: Query). De unieke query-hash wordt vermeld onder de eigenschap value (waarde). De query's vermeld wordt bovendien gevolgd door een waarde en een waarde die wijzen op een gedetecteerde prestatieprobleem.

De query met de hash-0x9102EXZ4 aangetroffen in het volgende voorbeeld van de logboekbestanden hebben een hogere duur van de uitvoering van (metriek: DurationIncreaseSeconds). De waarde van 110 seconden geeft aan dat deze bepaalde query heeft meer 110 seconden geduurd om uit te voeren. Omdat er meerdere query's kunnen worden gedetecteerd, kan in deze sectie logboek meerdere query vermeldingen bevatten.

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

De maateenheid voor elke metriek gerapporteerd wordt verstrekt onder de eigenschap metriek (metrisch) met de mogelijke waarden van seconden, het aantal en percentage. De waarde van een gemeten metriek wordt vermeld in de eigenschap value (waarde).

De eigenschap DurationIncreaseSeconds bevat de maateenheid in seconden. De maateenheid CriticalErrorCount is een getal dat een aantal fouten.

```json
"metric" : "DurationIncreaseSeconds", // issue metric type – possible values: DurationIncreaseSeconds, CriticalErrorCount, WaitingSeconds
"value" : 102 // value of the measured metric (in this case seconds)
```

## <a name="root-cause-analysis-and-improvement-recommendations"></a>Hoofdmap oorzaak analyse en verbetering aanbevelingen

Het laatste deel van het logboek van de prestaties Intelligent Insights is voor de analyse van de geautomatiseerde hoofdmap oorzaak van het geïdentificeerde prestatieprobleem vermindering van toepassing. De informatie wordt weergegeven in human-vriendelijk bewoordingen in de eigenschap root oorzaak analysis (rootCauseAnalysis_s). Verbetering aanbevelingen zijn opgenomen in het logboek, waar mogelijk.

```json
// example of reported root cause analysis of the detected performance issue, in a human-readable format

"rootCauseAnalysis_s" : "High data IO caused performance to degrade. It seems that this database is missing some indexes that could help."
```

U kunt het logboek van de prestaties Intelligent Insights met [Azure Log Analytics]( https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql) of een oplossing van derden voor aangepaste DevOps waarschuwingen en rapportagemogelijkheden.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Intelligent Insights](sql-database-intelligent-insights.md) concepten.
- Meer informatie over hoe [oplossen van prestatieproblemen Azure SQL Database met Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md).
- Meer informatie over hoe [Azure SQL Database bewaken met behulp van Azure SQL Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql).
- Meer informatie over hoe [verzamelen en gebruiken van de logboekgegevens van uw Azure-resources](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs).



