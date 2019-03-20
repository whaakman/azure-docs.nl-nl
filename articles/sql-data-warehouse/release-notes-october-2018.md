---
title: Azure SQL Data Warehouse opmerkingen bij de Release oktober 2018 | Microsoft Docs
description: Opmerkingen bij de release voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: twounder
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 12/04/2018
ms.author: mausher
ms.reviewer: twounder
ms.openlocfilehash: 6a27f23b5a0eba04ea3fa73a2f59364f9101c069
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57852163"
---
# <a name="whats-new-in-azure-sql-data-warehouse-october-2018"></a>Wat is er nieuw in Azure SQL Data Warehouse? Oktober 2018
Azure SQL Data Warehouse ontvangt voortdurend verbeteringen. Dit artikel beschrijft de nieuwe functies en wijzigingen die zijn geïntroduceerd in oktober 2018.

## <a name="devops-for-data-warehousing"></a>DevOps voor gegevensopslag
De maximaal aangevraagde functie voor SQL Data Warehouse (SQL DW) is nu in Preview-versie met de ondersteuning voor SQL Server Data hulpprogramma (SSDT) in Visual Studio. Teams van ontwikkelaars kunnen nu via een enkele, versiebeheer codebase samenwerken en wijzigingen snel implementeren op een instantie in de hele wereld. Wilt u toevoegen? Deze functie is beschikbaar voor Preview-versie vandaag nog! U kunt registreren door naar de pagina de [SQL Data Warehouse Visual Studio SQL Server Data Tools (SSDT) - Preview-inschrijving formulier](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR4-brmKy3TZOjoktwuHd7S1UODkwQ1lVMEw1NDBGRjNLRDNWOFlQRUpIRi4u). Gezien de grote vraag, zijn we opnamen in de Preview-versie om te controleren of de beste ervaring voor onze klanten beheren. Zodra u zich hebt aangemeld, wordt ons doel is om te bevestigen van de status van uw binnen zeven dagen.

## <a name="row-level-security-generally-available"></a>Beveiliging op rijniveau algemeen beschikbaar
Azure SQL Data Warehouse (SQL DW) biedt nu ondersteuning voor beveiliging op rijniveau (RLS) toe te voegen een krachtige mogelijkheid om uw gevoelige gegevens te beveiligen. Met de introductie van beveiliging op Rijniveau, kunt u beveiligingsbeleid voor het beheren van toegang tot rijen in de tabellen, zoals in wie toegang heeft tot welke rijen implementeren. Beveiliging op Rijniveau kan deze uiterst gedetailleerd toegangsbeheer zonder te hoeven ontwerpen van uw datawarehouse. Beveiliging op Rijniveau vereenvoudigt het algehele beveiligingsmodel als de logica van de beperking van toegang zich bevindt in de databaselaag zelf in plaats van weg van de gegevens in een andere toepassing. Beveiliging op Rijniveau wordt ook elimineert de noodzaak om te introduceren weergaven uit rijen voor toegangsbeheer te filteren. Er is geen extra kosten voor deze beveiligingsfunctie van ondernemingsniveau voor al onze klanten.

## <a name="advanced-advisors"></a>Geavanceerde adviseurs
Geavanceerde afstemmen voor Azure SQL Data Warehouse (SQL DW) net is eenvoudiger met aanbevelingen voor het datawarehouse van aanvullende gegevens en metrische gegevens. Er zijn meer geavanceerde prestatieaanbevelingen via Azure Advisor beschikken, met inbegrip van:
1.  Adaptieve cache – wanneer worden aangeraden om te schalen naar het Optimaliseer het gebruik van de cache.
2.  Tabeldistributie – bepalen wanneer u voor het repliceren van tabellen waarmee verplaatsing van gegevens en de prestaties van de werkbelastingen verhoogd. 
3.  TempDB-begrijpen wanneer schalen en configureren van de resource-klassen te tempdb-conflicten te verminderen.

Er is een diepere integratie van datawarehouse-metrieken met [Azure Monitor](https://azure.microsoft.com/blog/enhanced-capabilities-to-monitor-manage-and-integrate-sql-data-warehouse-in-the-azure-portal/) met inbegrip van een verbeterde aanpasbare controle grafiek voor bijna realtime metrische gegevens in de overzichtsblade. Wanneer u het verbruik wilt controleren of aanbevelingen voor het datawarehouse wilt valideren en toepassen, hoeft u de overzichtsblade van het datawarehouse niet langer te verlaten voor toegang tot metrische gegevens van Azure Monitor. Er zijn bovendien nieuwe metrische gegevens beschikbaar zijn, zoals tempdb en adaptieve cache gebruik als aanvulling op uw aanbevelingen voor prestaties.

## <a name="advanced-tuning-with-integrated-advisors"></a>Geavanceerde afstemmen met geïntegreerde adviseurs
Geavanceerde afstemmen voor Azure SQL Data Warehouse (SQL DW) net is eenvoudiger met aanbevelingen voor het datawarehouse van aanvullende gegevens en metrische gegevens en een nieuw ontwerp van de blade overzicht van portal een geïntegreerde ervaring met Azure Advisor en Azure Monitor biedt.

## <a name="accelerated-database-recovery-adr"></a>Versneld databaseherstel (ADR)
Azure SQL Data Warehouse versnelde Database Recovery (ADR) is nu in openbare Preview. ADR is een nieuwe versie van SQL Server Engine waarmee de beschikbaarheid van de database, met name in de aanwezigheid van langlopende transacties, aanzienlijk verbeterd door het huidige herstelproces vanaf het begin volledig opnieuw te ontwerpen van. De belangrijkste voordelen van ADR zijn snel en consistent databaseherstel en onmiddellijk transactie wordt teruggedraaid.

## <a name="azure-monitor-diagnostics-logs"></a>Diagnostische logboeken van Azure Monitor
SQL Data Warehouse (SQL DW) kunnen nu uitgebreid inzicht in analytische werkbelastingen door te integreren met Azure Monitor diagnostische logboeken rechtstreeks. Deze nieuwe functionaliteit kan ontwikkelaars werkbelasting gedrag gedurende een langere periode te analyseren en geïnformeerde beslissingen op query-optimalisatie of capaciteit management. We hebben nu een proces externe logboekregistratie via geïntroduceerd [diagnostische logboeken van Azure Monitor](https://docs.microsoft.com/azure/monitoring/monitoring-data-collection?toc=/azure/azure-monitor/toc.json#logs) waarmee u meer inzicht in uw datawarehouse-workload. Met één klik een knop te klikken, kunt u zich nu configureren diagnostische logboeken voor historische queryprestaties met behulp van mogelijkheden voor probleemoplossing [Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-queries). Diagnostische logboeken in Azure Monitor ondersteuning van aanpasbare bewaarperioden door op te slaan van de logboeken naar een opslagaccount voor controledoeleinden, de mogelijkheid voor het Logboeken streamen naar eventhubs in de buurt van real-time telemetrie, inzichten, en de mogelijkheid om met behulp van Log Analytics-logboekbestanden analyseren met Logboeken-query's. Een diagnostisch logboek bestaat uit telemetrische weergaven van uw datawarehouse, vergelijkbaar met de meestgebruikte DMV’s voor het oplossen van prestatieproblemen voor SQL Data Warehouse. Voor deze eerste release is voorzien van weergaven voor de volgende dynamische beheerweergaven van systeem:

- [sys.dm_pdw_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-requests-transact-sql)
- [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql)
- [sys.dm_pdw_dms_workers](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-dms-workers-transact-sql)
- [sys.dm_pdw_waits](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-waits-transact-sql)
- [sys.dm_pdw_sql_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-sql-requests-transact-sql)

## <a name="columnstore-memory-management"></a>Beheer van de Columnstore-geheugen
Als het aantal gecomprimeerde kolom store Rijgroepen toeneemt, neemt het geheugen die nodig is voor het beheren van de metagegevens van het segment interne kolom voor deze Rijgroepen.  Als gevolg hiervan, de prestaties van query's en query's uitgevoerd op een aantal Columnstore dynamische beheerweergaven (DMV's) kunnen verslechteren.  Verbeteringen aangebracht in deze release de grootte van de interne metagegevens voor deze gevallen leidt tot verbeterde ervaring en prestaties voor dergelijke query's optimaliseren. 

## <a name="azure-data-lake-storage-gen2-integration-ga"></a>Integratie van Azure Data Lake Storage Gen2 (GA)
Azure SQL Data Warehouse (SQL DW) heeft nu de systeemeigen integratie met Azure Data Lake Storage Gen2. Klanten kunnen nu laden van gegevens met behulp van externe tabellen uit ABFS in SQL DW. Deze functionaliteit kan klanten om te integreren in hun datalakes in Data Lake Storage Gen2. 

## <a name="bug-fixes"></a>Opgeloste fouten

| Titel | Description |
|:---|:---|
| **CETAS Parquet storingen op kleine resourceklassen op datawarehouses DW2000 en meer** | Deze oplossing een null-verwijzing in het maken van externe tabel als Parquet code pad de juiste manier wordt geïdentificeerd. |
|**De waarde van identiteitskolom kan verloren gaan in een CTAS-bewerking** | De waarde van een kolom vaststellen niet kan worden behouden als CTASed naar een andere tabel. Gemeld in een blog: [ https://blog.westmonroepartners.com/azure-sql-dw-identity-column-bugs/ ](https://blog.westmonroepartners.com/azure-sql-dw-identity-column-bugs/). |
| **Interne fout in sommige gevallen wanneer een sessie is beëindigd terwijl een query wordt nog steeds uitgevoerd.** | Deze oplossing wordt geactiveerd een InvalidOperationException als een sessie wordt beëindigd wanneer de query wordt nog steeds uitgevoerd. |
| **(Geïmplementeerd in November 2018) Klanten zijn de prestaties van een suboptimale ondervindt tijdens het laden van meerdere kleine bestanden van ADLS (Gen1) met behulp van Polybase.** | De systeemprestaties is tijdens beveiligingsvalidatie voor AAD-token knelpunt. Problemen met de prestaties zijn verholpen door het inschakelen van de cache van beveiligingstokens. |


## <a name="next-steps"></a>Volgende stappen
Nu u een en ander weet over SQL Data Warehouse, kunt u leren hoe u snel [maken van een SQL Data Warehouse][create a SQL Data Warehouse]. Als u niet bekend bent met Azure, kan de [Azure-woordenlijst][Azure glossary] handig zijn bij het opzoeken van nieuwe terminologie. U kunt ook enkele andere SQL Data Warehouse-resources bekijken.  

* [Succesverhalen van klanten]
* [Blogs]
* [Functieverzoeken]
* [Video's]
* [Teamblogs met adviezen voor klanten]
* [Stack Overflow-forum]
* [Twitter]


[Blogs]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Teamblogs met adviezen voor klanten]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Succesverhalen van klanten]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Functieverzoeken]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Stack Overflow-forum]: https://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Video's]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[create a SQL Data Warehouse]: ./create-data-warehouse-portal.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md
