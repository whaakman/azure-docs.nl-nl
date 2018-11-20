---
title: Prestaties van de database met Intelligent Insights - Azure SQL Database controleren | Microsoft Docs
description: Azure SQL Database Intelligent Insights maakt gebruik van ingebouwde intelligentie om continu te controleren van het Databasegebruik via kunstmatige intelligentie en detecteert verstorende gebeurtenissen die leiden slechte prestaties tot.
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
ms.date: 10/05/2018
ms.openlocfilehash: e8608782c71f8afc2ae32b16077cb26b5dcc6849
ms.sourcegitcommit: 8314421d78cd83b2e7d86f128bde94857134d8e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2018
ms.locfileid: "51976182"
---
# <a name="intelligent-insights-using-ai-to-monitor-and-troubleshoot-database-performance"></a>Intelligent Insights met behulp van AI voor bewaking en probleemoplossing van databaseprestaties

Azure SQL Database Intelligent Insights laat u weten wat er gebeurt met de prestaties van uw SQL-Database en de Managed Instance-database.

Intelligent Insights maakt gebruik van ingebouwde intelligentie om continu te controleren van het Databasegebruik via kunstmatige intelligentie en detecteert verstorende gebeurtenissen die leiden slechte prestaties tot. Wanneer wordt gedetecteerd, wordt een gedetailleerde analyse uitgevoerd waarmee een logboekbestanden met diagnostische gegevens met een intelligente evaluatie van het probleem worden gegenereerd. Deze evaluatie bestaat uit een analyses van hoofdoorzaken van het prestatieprobleem database en, indien mogelijk, aanbevelingen voor verbeterde prestaties.

## <a name="what-can-intelligent-insights-do-for-you"></a>Wat kan Intelligent Insights voor u doen

Intelligent Insights is een unieke mogelijkheid van ingebouwde intelligentie van Azure waarmee u de volgende waarde:

- Proactieve controle
- Op maat gemaakte prestatie-inzichten
- Vroegtijdig van prestatievermindering database
- Hoofdoorzaak analyse van de gedetecteerde problemen
- Aanbevelingen voor verbetering van prestaties
- De schaal vergroten mogelijkheid op honderden of duizenden databases
- Positief effect op de DevOps-resources en de totale eigendomskosten

## <a name="how-does-intelligent-insights-work"></a>Hoe werkt Intelligent Insights

Intelligent Insights analyseert de prestaties van de database door het vergelijken van de werkbelasting van het afgelopen uur van de database met de werkbelasting van de afgelopen zeven dagen basislijn. Database-workload van query's blijkt dat de belangrijkste op de databaseprestaties, zoals de grootste en meest herhaalde query's bestaat. Omdat elke database uniek op basis van de structuur, gegevens, gebruik en toepassingen is, is de basislijn van elke werkbelasting die wordt gegenereerd specifieke en uniek is voor een afzonderlijk exemplaar. Intelligent Insights, onafhankelijk van de basislijn werkbelasting ook absolute operationele drempelwaarden bewaakt en detecteert problemen met overmatige wachttijden, kritieke uitzonderingen en problemen met query-parameteriseringen invloed kunnen hebben op prestaties.

Nadat een prestatieprobleem degradatie is gedetecteerd door meerdere waargenomen metrische gegevens met behulp van kunstmatige intelligentie, wordt analyse uitgevoerd. Een logboek met diagnostische gegevens wordt gegenereerd met een intelligente inzicht in wat met uw database gebeurt er. Intelligent Insights kunt u eenvoudig is het prestatieprobleem database bijhouden van de eerste weergave totdat het probleem zou moeten. Elke gedetecteerde probleem wordt bijgehouden gedurende de levensduur van de eerste probleem detectie en verificatie van de prestaties verbeteren voor de voltooiing.

![Database prestaties analysis-werkstroom](./media/sql-database-intelligent-insights/intelligent-insights-concept.png)

De metrische gegevens die worden gebruikt om te meten en detecteren van prestatieproblemen met de database zijn gebaseerd op de queryduur van de, time-out voor aanvragen, overmatige wachttijden en aanvragen met fouten. Zie voor meer informatie over metrische gegevens over de [detectie van metrische gegevens](sql-database-intelligent-insights.md#detection-metrics) sectie van dit document.

Geïdentificeerd SQL Database performance degradations worden vastgelegd in de logboekbestanden met diagnostische gegevens met intelligente vermeldingen die bestaan uit de volgende eigenschappen:

| Eigenschap             | Details              |
| :------------------- | ------------------- |
| database-informatie | De metagegevens van een database waarvoor een inzicht is gedetecteerd, zoals een resource-URI. |
| Waargenomen tijdsbereik | Begin- en -tijd voor de periode van de gedetecteerde inzicht. |
| Beïnvloede metrische gegevens | Metrische gegevens een inzicht moet worden gegenereerd, waardoor: <ul><li>Query duur verhogen [seconden].</li><li>Overmatige wachten [seconden].</li><li>Time-out aanvragen [percentage].</li><li>Met fouten-out aanvragen [percentage].</li></ul>|
| Waarde van de impact | Waarde van een metrische waarde gemeten. |
| Betrokken query's en foutcodes | Query uitvoeren op hash of foutcode. Deze kunnen worden gebruikt om eenvoudig correleert met betrokken query's. Metrische gegevens die bestaan uit de toename van de duur van de query, wachttijd, time-out tellingen of foutcodes worden geleverd. |
| Detecties | De detectie is geïdentificeerd in de database tijdens het ophalen van een gebeurtenis. Er zijn 15 detectie patronen. Zie voor meer informatie, [oplossen van prestatieproblemen met de database met Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md). |
| Hoofdoorzaakanalyses | Hoofdoorzaak analyse van het probleem dat is geïdentificeerd in een leesbare indeling. Sommige insights bevat mogelijk een verbetering van prestaties aanbeveling waar mogelijk. |
|||

Zie de ingesloten video voor een praktische overzicht over het gebruik van Intelligent Insights met Azure SQL Analytics en voor typische gebruiksscenario's:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Get-Intelligent-Insights-for-Improving-Azure-SQL-Database-Performance/player]
>

Intelligent Insights schijnt in detecteren en oplossen van prestatieproblemen met de SQL-Database. Als u wilt gebruiken Intelligent Insights voor het oplossen van prestatieproblemen met de database SQL-Database en de Managed Instance, Zie [prestatieproblemen oplossen Azure SQL Database met Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md).

## <a name="configure-intelligent-insights"></a>Intelligent Insights configureren

Uitvoer van het Intelligent Insights is een slimme prestatielogboek met diagnostische gegevens. Dit logboek kan worden gebruikt op verschillende manieren - via deze gaat streamen naar Azure SQL Analytics, Azure Event Hubs en Azure-opslag, of een product van derden.

- Gebruik het product met [Azure SQL Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql) om inzicht te krijgen via de gebruikersinterface van de Azure-portal weer te geven. Dit is de geïntegreerde Azure-oplossing en de meest voorkomende manier om inzichten weer te geven.
- Gebruik het product met Azure Event Hubs voor het ontwikkelen van aangepaste bewaking en waarschuwingen van scenario 's
- Gebruik het product met Azure storage voor aangepaste ontwikkeling van toepassingen, zoals zijn bijvoorbeeld aangepaste rapportagegegevens, langdurige archivering enzovoort.

Integratie van Intelligent Insights met andere producten van Azure SQL Analytics, Azure Event Hub, Azure storage of producten van derden voor verbruik wordt uitgevoerd via de eerste inschakelen Intelligent Insights logboekregistratie (de "SQLInsights"-logboek) in de diagnose de instellingenblade van een database en vervolgens configureren Intelligent Insights Registreer om te worden gestreamd naar een van deze producten.

Zie voor meer informatie over hoe u intelligente inzichten logboekregistratie inschakelen en configureren van logboekgegevens kunnen worden gestreamd naar een verbruikende product [Azure SQL Database metrische gegevens en logboekregistratie van diagnostische gegevens](sql-database-metrics-diag-logging.md).

### <a name="set-up-with-azure-sql-analytics"></a>Met Azure SQL Analytics instellen

Azure SQL Analytics-oplossing biedt een grafische gebruikersinterface, rapportage- en waarschuwingsfuncties op de prestaties van de database, samen met het Intelligent Insights-diagnoselogboek gegevens.

> [!TIP]
> Snel aan de slag: de eenvoudigste manier om op te halen uit het begin te beginnen met het gebruik van Intelligent Insights is om deze te gebruiken, samen met Azure SQL Analytics een grafische gebruikersinterface voor prestatieproblemen met de database biedt. Azure SQL Analytics-oplossing toevoegen vanuit de marketplace, een werkruimte in deze oplossing maken en vervolgens voor elke database die u wilt inschakelen, Intelligent Insights configureren streaming van logboek 'SQLInsights' in de blade van de instellingen voor diagnostische gegevens over van een database naar de werkruimte van Azure SQL Analytics.
>

Vooraf vereiste is Azure SQL Analytics toegevoegd aan uw Azure portal-dashboard van de marketplace en bekijken om te maken van een werkruimte [Azure SQL Analytics configureren](../log-analytics/log-analytics-azure-sql.md#configuration)

Configureren voor het gebruik van Intelligent Insights met Azure SQL Analytics, Intelligent Insights-logboekgegevens kunnen worden gestreamd naar Azure SQL Analytics-werkruimte die u hebt gemaakt in de vorige stap, Zie [Azure SQL Database-metrische en diagnostische gegevens logboekregistratie](sql-database-metrics-diag-logging.md).

Het volgende voorbeeld ziet u dat een Intelligent Insights weergegeven via de Azure SQL Analytics:

![Intelligent Insights-rapport](./media/sql-database-intelligent-insights/intelligent-insights-azure-sql-analytics.png)

### <a name="set-up-with-event-hubs"></a>Met Event Hubs instellen

Configureren voor het gebruik van Intelligent Insights met Event Hubs, Intelligent Insights-logboekgegevens te worden gestreamd naar Event Hubs, raadpleegt u [Stream Azure diagnostics-logboeken naar Event Hubs](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md).

Zie voor het gebruik van Event Hubs voor het instellen van aangepaste bewaking en waarschuwingen, [wat te doen met metrische gegevens en diagnostische logboeken in Event Hubs](sql-database-metrics-diag-logging.md#what-to-do-with-metrics-and-diagnostics-logs-in-event-hubs).

### <a name="set-up-with-azure-storage"></a>Met Azure Storage instellen

Configureren voor het gebruik van Intelligent Insights met opslag, Intelligent Insights-logboekgegevens te worden gestreamd naar opslag, raadpleegt u [Stream naar Azure Storage](sql-database-metrics-diag-logging.md#stream-into-storage).

### <a name="custom-integrations-of-intelligent-insights-log"></a>Aangepaste integraties van Intelligent Insights-logboek

Zie voor het gebruik van Intelligent Insights met hulpprogramma's van derden, of voor aangepaste waarschuwingen en bewaking van ontwikkeling, [gebruik het diagnoselogboek voor Intelligent Insights database prestaties](sql-database-intelligent-insights-use-diagnostics-log.md).

## <a name="detection-metrics"></a>Detectie van metrische gegevens

Metrische gegevens die worden gebruikt voor detectie van modellen die intelligente inzichten genereren, zijn afhankelijk van de bewaking:

- Queryduur
- Time-out voor aanvragen
- Overmatige wachttijd
- Met fouten aanvragen

Duur en time-out queryaanvragen worden gebruikt als primaire modellen bij het detecteren van problemen met prestaties van de database-workload. Ze worden gebruikt omdat ze rechtstreeks meten wat er gebeurt met de werkbelasting. Voor het detecteren van alle mogelijke gevallen van de prestaties van de werkbelasting van de degradatie, overmatige wachttijd en aanvragen met fouten-out als aanvullende modellen worden gebruikt om aan te geven van de problemen die invloed hebben op de prestaties van de werkbelastingen.

Het systeem automatisch rekening gehouden met wijzigingen in de werkbelasting en wijzigingen in het aantal queryaanvragen dynamisch gemaakt met de database om te bepalen prestatiedrempels normaal en out gewone database.

Alle van de metrische gegevens in verschillende relaties via een wetenschappelijk afgeleide gegevensmodel dat elk prestatieprobleem gedetecteerd ingedeeld als één geheel worden beschouwd. Informatie die u via een slimme inzichten bevat:

- De details van het prestatieprobleem gedetecteerd.
- Een hoofdoorzaakanalyses van het probleem gedetecteerd.
- Aanbevelingen voor het verbeteren van de prestaties van de gecontroleerde SQL-database, waar mogelijk.

## <a name="query-duration"></a>Queryduur

Het querymodel voor duur verslechtering van de afzonderlijke query's analyseert en detecteert de toename in de tijd die nodig zijn om te compileren en uitvoeren van een query in vergelijking met de basislijn van prestaties.

Als SQL Database ingebouwde intelligentie een aanzienlijke toename in query compileren of query-uitvoering die van invloed op prestaties van de werkbelastingen detecteert, deze query's worden gemarkeerd als queryduur prestaties afnemen.

Het Intelligent Insights-diagnoselogboek voert de queryhash van de query in de prestaties worden verminderd. De queryhash geeft aan of de prestaties achteruitgaan is gerelateerd is aan de query te compileren of uitvoeringstijd voeren, wat verhoogd query duur.

## <a name="timeout-requests"></a>Time-out voor aanvragen

Het model van time-out voor aanvragen verslechtering van de afzonderlijke query's analyseert en detecteert dat er een toename in time-outs op het niveau van de query kan worden uitgevoerd en de totale time-outs aanvraag op het databaseniveau van de ten opzichte van de prestaties van basislijn-periode.

Sommige van de query's mogelijk time-out voordat ze zich bij de fase worden uitgevoerd. Via de middelen van afgebroken werknemers versus aanvragen, SQL Database ingebouwde intelligentie meet en analyseren van alle query's die de database is bereikt of ze naar de uitvoeringsfase of niet is.

Nadat het aantal time-outs voor uitgevoerde query's of het aantal werknemers afgebroken aanvraag de drempelwaarde voor het systeem beheerd overschrijdt, wordt een logboek met diagnostische gegevens wordt gevuld met intelligente inzichten.

De inzichten die zijn gegenereerd, bevat het aantal aanvragen is een time-out en het aantal query's is een time-out. Indicatie van de prestaties achteruitgaan is gerelateerd aan de toename van de time-out in de fase worden uitgevoerd of het databaseniveau van de algemene wordt aangeboden. Wanneer de toename in-time-outs wordt geacht aanzienlijke op de databaseprestaties van de, worden deze query's worden gemarkeerd als time-out prestaties afnemen.

## <a name="excessive-wait-times"></a>Overmatige wachttijden

Het model van de tijd overmatige wacht bewaakt afzonderlijke databasequery's. Wachtstatistieken voor ongewoon hoge query die het systeem beheerd absolute drempelwaarden gepasseerd wordt gedetecteerd. De volgende query overmatige wachttijd metrische gegevens zijn waargenomen met behulp van de nieuwe SQL Server-functie, wacht u statistieken voor Query Store (sys.query_store_wait_stats):

- Bronlimieten bereikt
- Bereikt limieten voor elastische Pools-resource
- Uitzonderlijk groot aantal worker- of -sessie threads
- Overmatige database vergrendelen
- Geheugendruk
- Andere Wachtstatistieken

Bereikt resourcebeperkingen of limieten voor elastische Pools resource geven aan dat verbruik van de beschikbare bronnen op een abonnement of in de elastische pool absolute drempelwaarden overschreden. Deze statistieken geven prestatievermindering werkbelasting. Een uitzonderlijk groot aantal worker- of -sessie threads geeft aan dat een voorwaarde waarin het aantal werkthreads of sessies die zijn gestart absolute drempelwaarden overschreden. Deze statistieken geven prestatievermindering werkbelasting.

Vergrendelen van overmatige database geeft aan dat een voorwaarde waarin het aantal vergrendelingen op een database absolute drempelwaarden heeft overschreden. Deze stat geeft aan dat een verslechtering van de werkbelasting van prestaties. Geheugen druk is een voorwaarde waarin het aantal threads die aanvragen van geheugen verleent overschreden een absolute drempelwaarde. Deze stat geeft aan dat een verslechtering van de werkbelasting van prestaties.

Andere statistieken wacht detectie geeft aan een voorwaarde waarin diverse metrische gegevens die zijn gemeten via de Query Store wacht statistieken een absolute drempelwaarde overschreden. Deze statistieken geven prestatievermindering werkbelasting.

Nadat overmatige wachttijden worden gedetecteerd, afhankelijk van de gegevens die beschikbaar is, de intelligente Insights-diagnoselogboek hashes uitvoer van de query's die gevolgen hebben voor en betrokken verminderde prestaties, details van de metrische gegevens die ervoor zorgen query's dat moet worden gewacht in uitvoering, en gemeten wachttijd.

## <a name="errored-requests"></a>Aanvragen met fouten

Het model met de aanvragen met fouten-verslechtering van de monitors afzonderlijke query's en een toename in het aantal query's die met fouten af in vergelijking met de basislijnperiode detecteert. Dit model controleert ook kritieke uitzonderingen die gepasseerd absolute drempelwaarden die worden beheerd door de ingebouwde intelligentie SQL-Database. Het systeem automatisch rekening gehouden met het aantal queryaanvragen aangebracht in de database en de accounts voor elke workload wordt gewijzigd in de gecontroleerde periode.

Wanneer de gemeten toename van aanvragen met fouten ten opzichte van het totale aantal aanvragen wordt geacht aanzienlijke werkbelasting prestaties zijn, worden betrokken query's worden gemarkeerd als aanvragen met fouten prestaties afnemen.

Het logboek Intelligent Insights levert het aantal aanvragen met fouten. Hiermee wordt aangegeven of de prestaties achteruitgaan is gerelateerd aan een toename van aanvragen met fouten of overschrijden van een bewaakte kritieke uitzondering drempelwaarde en gemeten tijd van de prestatievermindering.

Als een van de bewaakte kritieke uitzonderingen cross-de absolute drempelwaarden die worden beheerd door het systeem, kan een slimme inzichten met details van kritieke uitzondering wordt gegenereerd.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [oplossen van prestatieproblemen met de SQL-Database met Intelligent Insights](sql-database-intelligent-insights-troubleshoot-performance.md).
- Gebruik de [diagnoselogboek voor Intelligent Insights SQL Database performance](sql-database-intelligent-insights-use-diagnostics-log.md).
- Meer informatie over het [SQL-Database bewaken met behulp van SQL-analyse](../log-analytics/log-analytics-azure-sql.md).
- Meer informatie over het [verzamelen en gebruiken van logboekgegevens van uw Azure-resources](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md).
