---
title: Databasegebruik controleren met Intelligent Insights - Azure SQL Database | Microsoft Docs
description: Azure SQL Database Intelligent inzicht maakt gebruik van ingebouwde intelligentie continu Databasegebruik via kunstmatige intelligence bewaken en verstoren gebeurtenissen die leiden slechte prestaties tot detecteren.
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
ms.openlocfilehash: 86011610885ff913bfd70aa46389e4e39989d0a3
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="intelligent-insights"></a>Intelligent Insights

Azure SQL Database Intelligent inzicht laat u weten wat er gebeurt met de databaseprestaties van uw.

Ingebouwde intelligentie Intelligent Insights gebruikt voor het voortdurend controleren van het Databasegebruik via kunstmatige intelligence en verstoren gebeurtenissen die leiden slechte prestaties tot detecteren. Zodra er gedetecteerd, wordt een gedetailleerde analyse uitgevoerd die een logboek diagnostische gegevens met een intelligente beoordeling van het probleem genereert. Deze evaluatie bestaat uit een analyse van de hoofdmap oorzaak van het prestatieprobleem database en, indien mogelijk, aanbevelingen voor verbeterde prestaties. 

## <a name="what-can-intelligent-insights-do-for-you"></a>Wat kan Intelligent Insights voor u doen?

Intelligent Insights is een unieke mogelijkheid van Azure ingebouwde intelligentie die bepaalde de volgende waarde biedt:

- Proactieve controle
- Op maat gemaakte inzichten
- Vroegtijdig van verslechtering van de database-prestaties
- Hoofdoorzaak analyse van problemen gedetecteerd
- Aanbevelingen voor verbetering van prestaties
- Mogelijkheid op honderden of duizenden databases uitbreiden
- Positieve invloed op de DevOps-resources en de totale eigendomskosten

## <a name="how-does-intelligent-insights-work"></a>Hoe werkt Intelligent Insights?

Intelligent Insights analyseert de prestaties van de SQL-Database door te vergelijken met de werkbelasting van het afgelopen uur van de database met de afgelopen zeven dagen basislijn werkbelasting. Database-werkbelasting bestaat van query's bepaald dat de belangrijkste voor de databaseprestaties van de, zoals de meest herhaalde en de grootste query's. Omdat elke database uniek op basis van de structuur, gegevens, gebruik en toepassing, is elke basislijn werkbelasting die wordt gegenereerd specifieke en uniek is voor een afzonderlijke instantie. Intelligent Insights, onafhankelijk van de basislijn werkbelasting ook absolute operationele drempelwaarden controleert en problemen met buitensporige wachttijden, kritieke uitzonderingen en problemen met query parameteriseringen invloed kunnen hebben op prestaties detecteert.

Nadat een verslechtering van prestatieprobleem is gedetecteerd door meerdere waargenomen metrische gegevens met behulp van de kunstmatige intelligence, wordt analyse uitgevoerd. Een logboek met diagnostische gegevens wordt gegenereerd met een intelligente inzicht in wat er met uw database gebeurt. Intelligent Insights kunt gemakkelijk het prestatieprobleem database van de eerste weergave tot resolutie bijhouden. Elk probleem wordt bijgehouden gedurende de levensduur van het oorspronkelijke probleem detectie en verificatie van de prestaties verbeteren met de voltooiing ervan gedetecteerd. Updates worden uitgegeven in het logboek voor diagnostische gegevens om de 15 minuten. 

![Database prestaties analysis werkstroom](./media/sql-database-intelligent-insights/intelligent-insights-concept.png)

De metrische gegevens gebruikt om te meten en Detecteer prestatieproblemen met de database zijn gebaseerd op de duur van de query, time-out voor aanvragen, overmatige wachttijden en foutieve aanvragen. Zie voor meer informatie over metrische gegevens de [detectie metrische gegevens](sql-database-intelligent-insights.md#detection-metrics) gedeelte van dit document.

## <a name="degradations-detected"></a>Degradations gedetecteerd

Geïdentificeerd SQL-Database prestaties degradations worden vastgelegd in het logboek voor diagnostische gegevens met intelligent vermeldingen die bestaan uit de volgende eigenschappen:

| Eigenschap             | Details              |
| :------------------- | ------------------- |
| Database-informatie | De metagegevens over een database op waarop een beter inzicht is gedetecteerd, zoals een resource-URI. |
| Waargenomen tijdsbereik | Begin- en tijd voor de periode van de gedetecteerde inzicht. |
| Betrokken metrische gegevens | Metrische gegevens, waardoor een beter inzicht moeten worden gegenereerd: <ul><li>Query duur verhogen [seconden].</li><li>Overmatige wachten [seconden].</li><li>Time-out voor aanvragen [percentage].</li><li>Fout bij out aanvragen [percentage].</li></ul>|
| Waarde van de impact | Waarde van een metriek gemeten. |
| Betrokken query's en foutcodes | Query-hash of foutcode. Deze kunnen worden gebruikt voor het eenvoudig correleert met de betreffende query's. Metrische gegevens die bestaan uit verhoging van de duur van de query, wachttijd, time-out aantallen of foutcodes zijn beschikbaar. |
| Detecties | De detectie is tijdens de tijd van een gebeurtenis op de database wordt geïdentificeerd. Er zijn 15 detectie patronen. Zie voor meer informatie [oplossen van prestatieproblemen met de database met Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md). |
| Hoofdmap hoofdoorzaken analyseren | Hoofdoorzaak analyse van het probleem dat is aangetroffen in een leesbare indeling. Sommige insights bevatten mogelijk een prestaties verbetering aanbeveling waar mogelijk. |
|||

## <a name="issues-state-lifecycle-active-verifying-and-complete"></a>Problemen met de levenscyclus van status: 'Active', 'Controleren' en 'Voltooi'

Prestatieproblemen die zijn vastgelegd in het logboek voor diagnostische gegevens zijn gemarkeerd met een van de drie statussen van de levenscyclus van een probleem: 'Active', 'Controleren' en 'Voltooid'. Na een prestaties probleem wordt gedetecteerd en als lang deze heeft vastgesteld dat als aanwezig ingebouwde intelligentie SQL-Database, het probleem is gemarkeerd als 'Actief'. Als het probleem wordt beschouwd als verholpen, wordt gecontroleerd en wordt de status van het probleem wordt gewijzigd in 'Controleren'. Nadat de SQL-Database ingebouwde intelligentie beschouwt het probleem is opgelost, wordt de status van probleem gemarkeerd als 'Voltooid'.

## <a name="use-intelligent-insights"></a>Intelligent Insights gebruiken

U kunt het logboek van Intelligent Insights diagnostische gegevens verzenden Azure Log Analytics, Azure Event Hubs en Azure Storage. Zie voor meer informatie [Azure SQL Database metrische gegevens en logboekregistratie van diagnostische gegevens](sql-database-metrics-diag-logging.md). Nadat u het logboek op een van deze doelen verzonden, kan het logboek worden gebruikt voor aangepaste waarschuwingen en de controle van ontwikkeling met behulp van Microsoft of hulpprogramma's van derden. 

Zie voor meer informatie op de prestaties van SQL-Database problemen oplossen met behulp van Intelligent Insights [Azure SQL Database oplossen van prestatieproblemen met Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md).

## <a name="built-in-intelligent-insights-analytics-with-log-analytics"></a>Ingebouwde Intelligent Insights analytics met Log Analytics 

Een Log Analytics-oplossing biedt reporting en waarschuwen mogelijkheden boven op de diagnostics Intelligent Insights gegevens vastleggen. Het volgende voorbeeld ziet een rapport Intelligent inzichten in Azure SQL Analytics:

![Intelligent Insights-rapport](./media/sql-database-intelligent-insights/intelligent-insights-azure-sql-analytics.png)

Nadat het logboek van de diagnostische gegevens Intelligent Insights is geconfigureerd voor het SQL-Analytics van stroomgegevens, kunt u [bewaken van de SQL-database met behulp van SQL Analytics](../log-analytics/log-analytics-azure-sql.md).

## <a name="custom-integrations-of-intelligent-insights-log"></a>Aangepaste integraties van Intelligent Insights-logboek

Zie voor meer informatie over aangepaste waarschuwingen en de controle van ontwikkeling met behulp van Microsoft of hulpprogramma's van derden [gebruik de prestatiecontrole Intelligent Insights database Meld](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="set-up-intelligent-insights-with-event-hubs"></a>Intelligent Insights met Event Hubs instellen

- Zie voor het configureren van Intelligent Insights om te streamen van gebeurtenissen naar Event Hubs [stroom Azure diagnostics logboeken naar Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md).
- Zie voor het gebruik van Event Hubs voor aangepaste bewaking en waarschuwingen, [wat te doen met metrische gegevens en diagnostische gegevens geregistreerd in Event Hubs](sql-database-metrics-diag-logging.md#what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs). 

## <a name="set-up-intelligent-insights-with-storage"></a>Intelligent Insights met opslag instellen

- Zie voor het configureren van Intelligent inzicht moeten worden opgeslagen met Storage [Stream in Azure Storage](sql-database-metrics-diag-logging.md#stream-into-storage).

## <a name="detection-metrics"></a>Detectie van metrische gegevens

Metrische gegevens die worden gebruikt voor detectie van modellen die Intelligent Insights genereren zijn gebaseerd op de bewaking:

- duur van de query
- Time-out voor aanvragen
- Overmatige wachttijd
- Fout bij aanvragen

Query duur en time-aanvragen worden gebruikt als primaire modellen in het opsporen van problemen met databaseprestaties werkbelasting. Deze worden gebruikt omdat ze rechtstreeks meten wat er gebeurt met de werkbelasting. Om te detecteren van alle mogelijke gevallen van de prestaties van de werkbelasting van de degradatie, overmatige wachttijd en fouten aanvragen worden gebruikt als extra modellen voor wijzen op problemen die invloed hebben op de prestaties van de workload.

Het systeem wordt automatisch wijzigingen in de werkbelasting en wijzigingen in het aantal queryaanvragen voor de database naar dynamisch bepalen drempelwaarden voor prestaties normaal en out gewone database.

Alle van de metrische gegevens in verschillende relaties via een wetenschappelijke afgeleide gegevensmodel dat elke prestatieprobleem gedetecteerd ingedeeld als samen worden beschouwd. Informatie geboden via een intelligente inzicht omvat:

* Details van het prestatieprobleem gedetecteerd. 
* Een hoofdoorzaak analyse van het gedetecteerde probleem. 
* Aanbevelingen voor het verbeteren van de prestaties van de bewaakte SQL-database, waar mogelijk.

## <a name="query-duration"></a>duur van de query

De query duur verslechtering van model analyseert afzonderlijke query's en detecteert dat de toename in de tijd die nodig zijn om te compileren en uitvoeren van een query vergeleken met de basislijn van prestaties.

Als de ingebouwde intelligentie SQL-Database een aanzienlijke toename in query compileren of query-uitvoering die van invloed op prestaties van de werkbelasting detecteert, deze query's worden gemarkeerd als duur van de query vermindering van prestatieproblemen. 

Het logboek van de diagnostische gegevens Intelligent Insights levert de query-hash van de query slechtere prestaties. De hash van de query geeft aan of de prestaties achteruitgaan is gerelateerd aan query compileren of uitvoeringstijd toename, die de opgegeven tijdsduur voor query verhoogd.

## <a name="timeout-requests"></a>Time-out voor aanvragen

De time-out voor aanvragen verslechtering van model analyseert afzonderlijke query's en een toename in time-outs op het niveau van de query kan worden uitgevoerd en de algehele aanvraag time-outs op databaseniveau vergeleken met de basislijnperiode prestaties detecteert.

Sommige van de query's mogelijk time-out zelfs voordat ze de uitvoeringsfase bereiken. De wijze van afgebroken werknemers versus aanvragen SQL-Database ingebouwde intelligentie meet en analyseert alle query's die de database of ze naar de uitvoeringsfase hebben gekregen of niet is bereikt. 

Nadat u het aantal time-outs voor uitgevoerde query's of het aantal afgebroken aanvraag werknemers de drempelwaarde voor het systeem beheerd overschrijdt, wordt een logboek met diagnostische gegevens ingevuld met intelligent insights.

De inzichten die zijn gegenereerd bevatten het aantal aanvragen van de time-out en het aantal query's is een time-out. Indicatie van de prestaties achteruitgaan is gerelateerd aan time-out toename in het stadium uitvoering of het algemene databaseniveau is opgegeven. Wanneer de toename van time-outs dient te worden beschouwd voor de databaseprestaties van de, worden deze query's gemarkeerd als time-out verslechtering van prestatieproblemen. 

## <a name="excessive-wait-times"></a>Overmatige wachttijden

Het model van de tijd overmatige wacht bewaakt afzonderlijke databasequery's. Deze detecteert query ongewoon hoge wachttijd statistieken die het systeem beheerde absoluut drempels gepasseerd. De volgende query overmatige wachttijd metrische gegevens die worden waargenomen door met de nieuwe SQL Server-functie, wacht u statistieken voor Query Store (sys.query_store_wait_stats):

- Limieten voor bereikt
- Bereikt limieten elastische groep
- Uitzonderlijk groot aantal worker- of -sessie threads
- Overmatige database vergrendelen
- Geheugendruk
- Andere wait-statistieken

Limieten voor bereikt of limieten elastische pool geven aan dat het verbruik van de beschikbare bronnen op een abonnement of in de elastische groep absoluut drempels gepasseerd. Deze statistieken geven prestatievermindering werkbelasting. Een uitzonderlijk groot aantal worker- of -sessie threads geeft aan een voorwaarde waarin het aantal werkthreads of sessies die zijn gestart absoluut drempels overschreden. Deze statistieken geven prestatievermindering werkbelasting.

Vergrendelen van overmatige database geeft aan een voorwaarde waarin het aantal vergrendelingen voor een database absolute drempelwaarden heeft overschreden. Deze stat geeft aan dat een verslechtering van de prestaties werkbelasting. Geheugen druk is een voorwaarde waarin het aantal threads aanvragen geheugen verleent overschreden een absolute drempelwaarde. Deze stat geeft aan dat een verslechtering van de prestaties werkbelasting.

De detectie van andere wacht statistieken geeft aan een voorwaarde waarin diverse metrische gegevens die zijn gemeten via de Query Store wacht statistieken een absolute drempelwaarde overschreden. Deze statistieken geven prestatievermindering werkbelasting.

Nadat overmatige wachttijden worden gedetecteerd, afhankelijk van de gegevens beschikbaar zijn, de Intelligent Insights diagnostische gegevens meldt u zich de hashes uitvoer van de query's die gevolgen hebben voor en betrokken slechtere prestaties, details van de metrische gegevens die ertoe leiden dat query's moet worden gewacht in uitvoering, en gemeten wachttijd.

## <a name="errored-requests"></a>Fout bij aanvragen

Het foutieve aanvragen verslechtering van model monitors afzonderlijke query's en een toename in het aantal query's dat foutieve uit met de basislijnperiode vergeleken detecteert. Dit model bewaakt ook kritieke uitzonderingen die gepasseerd absolute drempelwaarden die worden beheerd door de ingebouwde intelligentie SQL-Database. Het systeem beschouwt automatisch het aantal queryaanvragen voor de database en de accounts voor werkbelasting wijzigingen in de gecontroleerde periode.

Wanneer de gemeten toename in foutieve aanvragen ten opzichte van het totale aantal aanvragen dient te worden beschouwd voor de prestaties van de werkbelasting, worden de betreffende query's gemarkeerd als foutieve aanvragen verslechtering van prestatieproblemen.

Het logboek Intelligent Insights levert het aantal foutieve aanvragen. Hiermee wordt aangegeven of de prestaties achteruitgaan is gerelateerd aan een toename van foutieve aanvragen of een bewaakte kritieke uitzondering drempelwaarde en gemeten tijd van de prestaties achteruitgaan overschrijden. 

Als een van de bewaakte kritieke uitzonderingen cross de absolute drempelwaarden beheerd door het systeem, wordt een intelligente inzicht met details voor kritieke uitzondering gegenereerd.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [oplossen van prestatieproblemen SQL-Database met Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md).
* Gebruik de [Intelligent Insights SQL-Database diagnostics prestatielogboek](sql-database-intelligent-insights-use-diagnostics-log.md).
* Meer informatie over hoe [SQL-Database controleren met behulp van SQL Analytics](../log-analytics/log-analytics-azure-sql.md).
* Meer informatie over hoe [verzamelen en gebruiken van de logboekgegevens van uw Azure-resources](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).


