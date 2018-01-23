---
title: Databaseprestaties bewaken in Azure SQL Database | Microsoft Docs
description: Lees meer over de opties voor het bewaken van uw database met Azure-hulpprogramma's en dynamische beheerweergave.
keywords: database bewaken, prestaties van clouddatabase
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: a2e47475-c955-4a8d-a65c-cbef9a6d9b9f
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 09/20/2017
ms.author: carlrab
ms.openlocfilehash: 2286843317230b8167b315b1e8e413e7571da4fe
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2018
---
# <a name="monitoring-database-performance-in-azure-sql-database"></a>Databaseprestaties bewaken in Azure SQL Database
Het bewaken van de prestaties van een SQL-database in Azure begint met het bewaken van het resourcegebruik ten opzichte van het gekozen niveau van databaseprestaties. Bewaking helpt u om te bepalen of de database te veel capaciteit heeft of juist problemen heeft omdat resources volledig worden benut. Vervolgens kunt u beslissen of u het prestatieniveau en de [servicelaag](sql-database-service-tiers.md) van uw database moet aanpassen. U kunt een database bewaken met grafische hulpprogramma's in de [Azure-portal](https://portal.azure.com) of met [dynamische beheerweergaven](https://msdn.microsoft.com/library/ms188754.aspx) van SQL.

> [!TIP]
> Gebruik [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md) voor de automatische bewaking van de databaseprestaties van uw. Zodra een prestatieprobleem wordt gedetecteerd, wordt een diagnostische logboek is gegenereerd met de details en hoofdmap oorzaak Analysis (RCA) van het probleem. Prestaties verbetering aanbeveling wordt verstrekt wanneer mogelijk.
>

## <a name="monitor-databases-using-the-azure-portal"></a>Databases bewaken via de Azure-portal
In de [Azure-portal](https://portal.azure.com/) kunt u het verbruik van een individuele database bewaken door de database te selecteren en op de grafiek **Bewaking** te klikken. Nu wordt een venster **Metrische gegevens** geopend, dat u kunt wijzigen door op de knop **Grafiek bewerken** te klikken. Voeg de volgende metrische gegevens toe:

* CPU-percentage
* DTU-percentage
* Gegevens-I/O-percentage
* Databaseomvangpercentage

Wanneer u deze metrische gegevens hebt toegevoegd, kunt u ze in de grafiek **Bewaking** gedetailleerder bekijken in het venster **Metrische gegevens**. De vier metrische gegevens tonen het gemiddelde gebruikspercentage ten opzichte van de **DTU** van uw database. Raadpleeg het artikel over [servicelagen](sql-database-service-tiers.md) voor meer informatie over DTU's.

![Servicelaagbewaking van databaseprestaties.](./media/sql-database-single-database-monitoring/sqldb_service_tier_monitoring.png)

U kunt ook meldingen configureren voor prestatiewaarden. Klik op de knop **Melding toevoegen** in het venster **Metrische gegevens**. Volg de wizard om de melding te configureren. U hebt de keuze om een melding weer te geven als de metrische gegevens een bepaalde drempelwaarde overschrijden of als het meetpunt onder een bepaalde drempelwaarde komt.

Als u bijvoorbeeld verwacht dat de workload van de database zal toenemen, kunt u configureren dat er een e-mailmelding wordt verstuurd wanneer de database 80% van een van de prestatiewaarden heeft bereikt. U kunt dit gebruiken als een aankondiging dat u misschien moet overschakelen naar een hoger prestatieniveau.

Aan de hand van de prestatiewaarden kunt u ook bepalen of u misschien moet downgraden naar een lager prestatieniveau. Stel dat u een Standard S2-database gebruikt en alle prestatiewaarden aangeven dat de database gemiddeld nooit meer dan 10% gebruikt. Het is dan waarschijnlijk dat de database in Standard S1 goed werkt. Houd echter wel rekening met workloads die pieken of fluctueren, voordat u besluit om over te stappen op een lager prestatieniveau.

## <a name="monitor-databases-using-dmvs"></a>Databases bewaken via DMV's
Dezelfde metrische gegevens die in de portal worden weergegeven, zijn ook beschikbaar via systeemweergaven: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) in de logische **hoofd**database van de server en [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) in de gebruikersdatabase. Gebruik **sys.resource_stats** als u voor een langere periode minder gedetailleerde gegevens wilt bewaken. Gebruik **sys.dm_db_resource_stats** als u gedurende kortere tijd meer gedetailleerde gegevens wilt bewaken. Zie [Richtlijnen voor Azure SQL Database-prestaties](sql-database-single-database-monitor.md#monitor-resource-use) voor meer informatie.

> [!NOTE]
> **sys.dm_db_resource_stats** retourneert een lege resultatenset als deze wordt gebruikt voor de buiten gebruik gestelde Web en Business Edition-databases.
>
>

### <a name="monitor-resource-use"></a>Brongebruik controleren

U kunt controleren met resource-gebruik [SQL Database Query Performance Insight](sql-database-query-performance.md) en [Query Store](https://msdn.microsoft.com/library/dn817826.aspx).

U kunt ook gebruik van deze twee weergaven met bewaken:

* [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
* [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

#### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
U kunt de [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) weergave in elke SQL-database. De **sys.dm_db_resource_stats** weergave toont recente gegevens ten opzichte van de servicetier resource gebruiken. Gemiddelde percentages voor CPU, i/o-gegevens, logboekschrijfbewerkingen en geheugen elke 15 seconden worden vastgelegd en worden bewaard 1 uur.

Omdat deze weergave een meer gedetailleerd blik op gebruik van bronnen biedt, gebruikt u **sys.dm_db_resource_stats** eerste voor een analyse van de huidige status of het oplossen van problemen. Deze query geeft bijvoorbeeld het gebruik van gemiddelde en maximale resource voor de huidige database via het afgelopen uur:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data I/O in percent',
        MAX(avg_data_io_percent) AS 'Maximum data I/O in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Zie voor andere query's in de voorbeelden in [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

#### <a name="sysresourcestats"></a>sys.resource_stats
De [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) weergeven in de **master** database bevat extra informatie kunt u de prestaties van uw SQL-database op de specifieke prijscategorie en prestatieniveau serviceniveau controleren. De gegevens worden verzameld om de 5 minuten en ongeveer 14 dagen behouden. Deze weergave is handig voor een langere historische analyse van hoe uw SQL-database maakt gebruik van bronnen.

De volgende grafiek ziet de CPU gebruik van bronnen voor een Premium-database met het prestatieniveau P2 voor elk uur in een week. Deze grafiek begint op een maandag, ziet u 5 werkdagen en geeft vervolgens een weekend, als er veel minder wordt uitgevoerd op de toepassing.

![Gebruik van SQL database-bronnen](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

Van de gegevens, heeft deze database momenteel een piek CPU-belasting van iets meer dan 50 procent CPU-gebruik ten opzichte van het prestatieniveau P2 (middaguur op dinsdag). Als CPU de verwerkingsflexibiliteit factor in resource-profiel van de toepassing is, kunt u vervolgens besluiten dat P2 het juiste prestatieniveau is te garanderen dat de werkbelasting altijd past. Als u verwacht een toepassing dat op den duur uitgebreid, is het een goed idee om een extra bron buffer hebben, zodat de toepassing niet ooit de prestatieniveau limiet bereikt. Als u het prestatieniveau verhoogt, kunt u helpen te voorkomen dat zichtbaar is voor een klant fouten die optreden mogelijk wanneer een database beschikt niet over voldoende stroom voor het verwerken van aanvragen effectief, met name in omgevingen latentie gevoelig. Een voorbeeld is een database die een toepassing die worden getekend door webpagina's op basis van de resultaten van de database-aanroepen ondersteunt.

Andere soorten toepassingen mogelijk dezelfde grafiek anders interpreteren. Bijvoorbeeld, als een toepassing probeert te verwerken van gegevens van elke dag en dezelfde grafiek heeft, kunt dit soort 'batchverwerking' model doen goed op prestatieniveau P1. Het prestatieniveau P1 heeft 100 dtu's ten opzichte van 200 dtu's op het niveau van de prestaties P2. Het prestatieniveau P1 biedt de helft van de prestaties van het prestatieniveau P2. Dus 50 procent van de CPU-gebruik in P2 is gelijk aan 100 procent CPU-gebruik in P1. Als de toepassing geen time-outs, maakt het niet uit als een taak duurt 2 uur of 2,5 uren in beslag, als het vandaag wordt voltooid. Een toepassing in deze categorie kunt waarschijnlijk prestatieniveau P1 gebruiken. U kunt profiteren van het feit dat er perioden gedurende de dag wanneer bronnengebruik ligt, zodat eventuele 'big piek' kan worden gelekt via in een van de holten later op de dag. Het prestatieniveau P1 mogelijk geschikt voor dit soort van toepassing (en geld besparen), zolang de taken op tijd per dag kunnen worden voltooid.

Azure SQL Database worden verbruikt resourcegegevens voor elke actieve database in de **sys.resource_stats** weergeven van de **master** database op elke server. De gegevens in de tabel worden samengevoegd voor 5 minuten durende intervallen. De gegevens kan worden weergegeven in de tabel betekent dat deze gegevens nuttiger voor historische analyse in plaats van in de buurt van de realtime-analyses meer dan 5 minuten duren voordat met de Servicelagen Basic, Standard en Premium. Query de **sys.resource_stats** de recente geschiedenis van een database weergeven en valideren of de reservering u hebt ervoor gekozen de prestaties die u wilt dat wanneer deze nodig is geleverd.

> [!NOTE]
> U moet verbonden zijn met de **master** database van de logische SQL database-server aan query **sys.resource_stats** in de volgende voorbeelden.
> 
> 

Dit voorbeeld ziet u hoe de gegevens in deze weergave wordt weergegeven:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![De catalogusweergave sys.resource_stats](./media/sql-database-performance-guidance/sys_resource_stats.png)

Het volgende voorbeeld ziet u de verschillende manieren waarop u kunt de **sys.resource_stats** catalogusweergave voor informatie over hoe resources in uw SQL-database worden gebruikt:

1. Om te kijken naar de afgelopen week resource voor de database userdb1 gebruiken, kunt u deze query uitvoeren:
   
        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;
2. Als u wilt evalueren hoe goed uw werkbelasting geschikt is voor het prestatieniveau, die u wilt inzoomen op elk aspect van de resource metrische gegevens: CPU, lezen, schrijven, aantal werknemers en aantal sessies. Hier volgt een herziene query met **sys.resource_stats** voor het rapporteren van de gemiddelde en maximale waarden van deze resource metrische gegevens:
   
        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data I/O use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data I/O use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
3. U kunt met deze informatie over de gemiddelde en maximale waarden van elke resource metrische gegevens kunt beoordelen hoe goed uw werkbelasting in het prestatieniveau dat u hebt gekozen past. Normaal gesproken gemiddelde waarden van **sys.resource_stats** bieden u een goede basis te gebruiken op basis van de doelgrootte. Moet uw primaire meting stick. Voor een voorbeeld kan de laag Standard-service worden gebruikt met S2 prestatieniveau. Het gemiddelde gebruik percentages voor CPU- en i/o-leesbewerkingen en schrijfbewerkingen zijn dan 40 procent, het gemiddelde aantal werknemers lager is dan 50 en het gemiddelde aantal sessies lager is dan 200. Uw werkbelasting mogelijk in het prestatieniveau S1 past. Het is gemakkelijk om te zien of de database in de limieten van de werknemer en sessie past. Om te zien of een database in een lager prestatieniveau met betrekking tot de CPU past, leest en schrijft, verdeelt het DTU-nummer van de lager prestatieniveau door het DTU-nummer van uw huidige prestatieniveau en vervolgens het resultaat vermenigvuldigd met 100:
   
    **S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40**
   
    Het resultaat is het relatieve prestatieverschil tussen de twee prestatieniveaus in een percentage. Als uw gebruik van de resource niet groter zijn dan deze hoeveelheid, kan uw werkbelasting past in lager prestatieniveau. Echter, moet u alle bereiken van waarden voor het gebruik van resources, en bepalen, op percentage, hoe vaak de werkbelasting van uw database valt in de lager prestatieniveau. De volgende query levert het percentage aanpassen per resourcedimensie, op basis van de drempelwaarde van 40 procent die in dit voorbeeld is berekend:
   
        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Op basis van uw database serviceniveaudoelstelling (SLO), kunt u beslissen of uw werkbelasting in lager prestatieniveau past. Als de werkbelasting van uw database SLO 99,9 procent is en de voorgaande query waarden groter dan 99,9% voor alle drie resourcedimensies retourneert, wordt uw workload waarschijnlijk in de lager prestatieniveau past.
   
    Kijken naar de geschikte percentage biedt ook inzicht in of u naar de hoger prestatieniveau overstappen moet om te voldoen aan uw SLO. Userdb1 ziet u bijvoorbeeld het volgende CPU-gebruik voor de afgelopen week:
   
   | Gemiddelde CPU-percentage | Maximale CPU-percentage |
   | --- | --- |
   | 24.5 |100.00 |
   
    De gemiddelde CPU is over een kwartaal van de limiet van het prestatieniveau dat zijn voor het prestatieniveau van de database geschikt zou. Maar de maximale waarde ziet u dat de database heeft de limiet van het prestatieniveau bereikt. Moet u naar de hoger prestatieniveau verplaatsen? Kijken hoe vaak uw werkbelasting bereikt 100 procent en vergelijk deze met de werkbelasting van uw database SLO.
   
        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data I/O fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Als deze query retourneert een waarde minder dan 99,9 procent voor een van de resourcedimensies drie Overweeg beide verplaatsen naar de hoger prestatieniveau of technieken afstemming van toepassingen om de belasting van de SQL-database te verminderen.
4. In deze oefening tevens rekening gehouden met de verwachte werkbelasting toename in de toekomst.

Voor elastische pools kunt u afzonderlijke databases in de pool bewaken met de technieken die in deze sectie zijn beschreven. U kunt echter ook de groep als geheel bewaken. Zie [Een elastische pool bewaken en beheren](sql-database-elastic-pool-manage-portal.md) voor meer informatie.


### <a name="maximum-concurrent-requests"></a>Maximum aantal gelijktijdige aanvragen
Overzicht van het aantal gelijktijdige aanvragen uitvoeren deze Transact-SQL-query op de SQL database:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Wijzig deze query om te filteren op specifieke database die u wilt analyseren voor het analyseren van de werkbelasting van een on-premises SQL Server database. Bijvoorbeeld, als er een lokale database met de naam MijnDatabase, retourneert deze Transact-SQL-query het aantal gelijktijdige aanvragen in de database:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Dit is slechts een momentopname op één punt in tijd. Als u een beter begrip van uw werkbelasting en de vereisten voor gelijktijdige aanvragen, moet u veel voorbeelden verzamelen gedurende een bepaalde periode.

### <a name="maximum-concurrent-logins"></a>Maximum aantal gelijktijdige aanmeldingen
U kunt uw gebruikers- en patronen voor een beter beeld van de frequentie van aanmeldingen analyseren. U kunt ook de werkelijke hoeveelheden in een testomgeving om ervoor te zorgen dat u bent niet kunt u door deze of andere limieten in dit artikel besproken uitvoeren. Er is een enkele query of de dynamische beheerweergave (DMV) die u kunt u zien gelijktijdige aanmelding telt of de geschiedenis niet.

Als meerdere clients de verbindingsreeks gebruiken, verifieert de service voor elke aanmelding. Als er verbinding tegelijkertijd 10 gebruikers met een database met behulp van dezelfde gebruikersnaam en wachtwoord, zou er 10 gelijktijdige aanmeldingen. Deze beperking geldt alleen voor de duur van de aanmelding en verificatie. Als dezelfde 10 gebruikers verbinding sequentieel worden verwerkt met de database, zou het aantal gelijktijdige aanmeldingen niet groter dan 1.

> [!NOTE]
> Op dit moment is deze limiet niet van toepassing op databases in een elastische pools.
> 
> 

### <a name="maximum-sessions"></a>Maximum aantal sessies
Het aantal huidige actieve sessies vindt deze Transact-SQL-query op de SQL-database worden uitgevoerd:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Als u van een lokale SQL Server-werkbelasting analyseren bent, wijzigt u de query om zich te richten op een specifieke database. Deze query kunt u bepalen mogelijke sessie voor de database als u overweegt naar Azure SQL Database te verplaatsen.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Deze query's retourneren opnieuw een punt in tijd telling. Als u meerdere steekproeven gedurende een bepaalde periode verzamelt, hebt u het beste inzicht in uw sessie gebruiken.

Voor analyse van de SQL-Database, kunt u historische statistieken opvragen op sessies door het uitvoeren van query's de [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) weergeven en beoordelen van de **active_session_count** kolom. 

## <a name="next-steps"></a>Volgende stappen

- Automatisch afstemmen van de database-indexen en uitvoering plannen met behulp van een query [automatische afstemming van Azure SQL Database](sql-database-automatic-tuning.md).
- Controleren van de prestaties van de database automatisch met [Azure SQL Intelligent Insights](sql-database-intelligent-insights.md). Deze functie biedt diagnostische gegevens en analyse van prestatieproblemen hoofdoorzaak.
