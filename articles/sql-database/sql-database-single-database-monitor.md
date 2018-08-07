---
title: Databaseprestaties bewaken in Azure SQL Database | Microsoft Docs
description: Lees meer over de opties voor het bewaken van uw database met Azure-hulpprogramma's en dynamische beheerweergave.
keywords: database bewaken, prestaties van clouddatabase
services: sql-database
author: danimir
manager: craigg
ms.service: sql-database
ms.custom: monitor & tune
ms.topic: conceptual
ms.date: 07/16/2018
ms.author: v-daljep
ms.reviewer: carlrab
ms.openlocfilehash: 081c6210181815be079404b10e7aa4b0df8327ec
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39520976"
---
# <a name="monitoring-database-performance-in-azure-sql-database"></a>Databaseprestaties bewaken in Azure SQL Database
Het bewaken van de prestaties van een SQL-database in Azure begint met het bewaken van het resourcegebruik ten opzichte van het gekozen niveau van databaseprestaties. Bewaking helpt u te bepalen of uw database overtollige capaciteit heeft of problemen heeft juist omdat resources volledig worden benut uit en klikt u vervolgens beslissen of is het tijd om het prestatieniveau aanpassen en service-lagen van de database in de [op basis van DTU aankoopmodel](sql-database-service-tiers-dtu.md) of [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md). U kunt een database bewaken met grafische hulpprogramma's in de [Azure-portal](https://portal.azure.com) of met [dynamische beheerweergaven](https://msdn.microsoft.com/library/ms188754.aspx) van SQL.

> [!TIP]
> Gebruik [Intelligent Insights van Azure SQL](sql-database-intelligent-insights.md) voor de automatische bewaking van de databaseprestaties van uw. Zodra een prestatieprobleem wordt gedetecteerd, wordt een diagnostisch logboek gegenereerd met de details en Root oorzaak Analysis (RCA) van het probleem. Prestaties verbetering aanbeveling wordt geleverd, indien mogelijk.
>

## <a name="monitor-databases-using-the-azure-portal"></a>Databases bewaken via de Azure-portal
In de [Azure-portal](https://portal.azure.com/) kunt u het verbruik van een individuele database bewaken door de database te selecteren en op de grafiek **Bewaking** te klikken. Nu wordt een venster **Metrische gegevens** geopend, dat u kunt wijzigen door op de knop **Grafiek bewerken** te klikken. Voeg de volgende metrische gegevens toe:

* CPU-percentage
* DTU-percentage
* Gegevens-I/O-percentage
* Databaseomvangpercentage

Als u deze metrische gegevens hebt toegevoegd, kunt u doorgaan met deze bekijken in de **bewaking** grafiek met meer informatie over de **Metric** venster. De vier metrische gegevens tonen het gemiddelde gebruikspercentage ten opzichte van de **DTU** van uw database. Zie de [DTU gebaseerde aankoopmodel](sql-database-service-tiers-dtu.md) en [vCore gebaseerde aankoopmodel](sql-database-service-tiers-vcore.md) artikelen voor meer informatie over Servicelagen.  

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

### <a name="monitor-resource-use"></a>Resource-gebruik bewaken

U kunt controleren resource gebruik [SQL Database Query Performance Insight](sql-database-query-performance.md) en [Query Store](https://msdn.microsoft.com/library/dn817826.aspx).

U kunt ook gebruik van deze twee weergaven met bewaken:

* [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx)
* [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx)

#### <a name="sysdmdbresourcestats"></a>sys.dm_db_resource_stats
U kunt de [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) weergeven in elke SQL-database. De **sys.dm_db_resource_stats** weergave toont recente gegevens in de resource gebruiken ten opzichte van de service tier. Gemiddelde percentages voor CPU, gegevens-i/o-logboek geschreven en geheugen elke 15 seconden worden vastgelegd en 1 uur worden bewaard.

Omdat deze weergave een meer gedetailleerd blik op gebruik van bronnen biedt, gebruikt u **sys.dm_db_resource_stats** eerste voor de analyse van de huidige status of het oplossen van problemen. Deze query geeft bijvoorbeeld het gebruik van gemiddelde en maximale resource voor de huidige database in het afgelopen uur:

    SELECT  
        AVG(avg_cpu_percent) AS 'Average CPU use in percent',
        MAX(avg_cpu_percent) AS 'Maximum CPU use in percent',
        AVG(avg_data_io_percent) AS 'Average data IO in percent',
        MAX(avg_data_io_percent) AS 'Maximum data IO in percent',
        AVG(avg_log_write_percent) AS 'Average log write use in percent',
        MAX(avg_log_write_percent) AS 'Maximum log write use in percent',
        AVG(avg_memory_usage_percent) AS 'Average memory use in percent',
        MAX(avg_memory_usage_percent) AS 'Maximum memory use in percent'
    FROM sys.dm_db_resource_stats;  

Voor andere query's, Zie de voorbeelden in [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx).

#### <a name="sysresourcestats"></a>sys.resource_stats
De [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) weergeven in de **master** database bevat extra informatie waarmee u de prestaties van uw SQL-database op het niveau van de specifieke service-laag en de prestaties controleren. De gegevens worden verzameld om de 5 minuten en is ongeveer 14 dagen bewaard. In deze weergave is handig voor een langere historische analyse van hoe uw SQL-database maakt gebruik van resources.

In het volgende diagram ziet u het CPU-resource gebruik voor een Premium-database met het prestatieniveau P2 voor elk uur in een week. Deze grafiek begint op een maandag, ziet u 5 werkdagen en geeft vervolgens een weekend, als er veel minder wordt uitgevoerd op de toepassing.

![SQL database resource gebruiken](./media/sql-database-performance-guidance/sql_db_resource_utilization.png)

De gegevens, heeft deze database momenteel een piek-CPU-belasting van iets meer dan 50 procent CPU-gebruik ten opzichte van het prestatieniveau P2 (twaalf uur 's middags op dinsdag). Als de CPU is de dominante factor bij het profiel van de resource van de toepassing, dan kunt u besluiten dat P2 het juiste prestatieniveau is te garanderen dat de werkbelasting altijd past. Als u verwacht een toepassing na verloop van tijd toeneemt dat, is het een goed idee om een extra bron buffer zodat de toepassing niet steeds de prestatieniveau-limiet is bereikt. Als u het prestatieniveau verhoogt, kunt u helpen te voorkomen dat zichtbaar is voor een klant fouten die optreden wanneer een database beschikt niet over voldoende kracht voor het verwerken van aanvragen effectief, met name in omgevingen met latentiegevoelige. Een voorbeeld is een database die ondersteuning biedt voor een toepassing die u webpagina's op basis van de resultaten van de database-aanroepen tekent.

Andere soorten toepassingen mogelijk dezelfde grafiek anders worden geïnterpreteerd. Bijvoorbeeld, als een toepassing probeert te verwerken gegevens van elke dag en dezelfde grafiek, dit soort 'batch-taak' model kan van pas komen goed op een prestatieniveau P1. Het prestatieniveau P1 heeft 100 dtu's in vergelijking tot 200 dtu's op het prestatieniveau P2. Het prestatieniveau P1 biedt de helft van de prestaties van het prestatieniveau P2. Dus, 50 procent van de CPU-gebruik in P2 is gelijk aan 100 procent CPU-gebruik van P1. Als de toepassing heeft geen time-outs, maakt het niet uit als een taak duurt 2 uur of 2,5 uren in beslag, als het vandaag nog wordt uitgevoerd. Een toepassing in deze categorie kunt waarschijnlijk een prestatieniveau P1 gebruiken. U kunt profiteren van het feit dat er perioden gedurende de dag wanneer Resourcegebruik lager, is zodat eventuele 'big piek' kan worden gelekt via in een van de holten later op de dag. Het prestatieniveau P1 kan zijn goed voor die van toepassing (en geld besparen) van het type, zolang de taken op elke dag tijd voltooien kunnen.

Azure SQL Database beschikbaar stelt verbruikt resource-informatie voor elke actieve database in de **sys.resource_stats** weergave van de **master** database op elke server. De gegevens in de tabel wordt samengevoegd voor 5 minuten durende intervallen. De Servicelagen Basic, Standard en Premium, kan de gegevens worden weergegeven in de tabel, zodat deze gegevens nog meer nuttige voor historische analyse in plaats van in de buurt van de realtime-analyse is meer dan vijf minuten maken. Query de **sys.resource_stats** weergeven om te zien van de recente geschiedenis van een database en om te valideren of de reservering die u kiest de prestaties die u wilt dat wanneer dat nodig is geleverd.

> [!NOTE]
> U moet zijn verbonden met de **master** database van de logische SQL database-server op query **sys.resource_stats** in de volgende voorbeelden.
> 
> 

Dit voorbeeld ziet u hoe de gegevens in deze weergave wordt weergegeven:

    SELECT TOP 10 *
    FROM sys.resource_stats
    WHERE database_name = 'resource1'
    ORDER BY start_time DESC

![De catalogusweergave sys.resource_stats](./media/sql-database-performance-guidance/sys_resource_stats.png)

Het volgende voorbeeld ziet u verschillende manieren waarop u kunt de **sys.resource_stats** catalogusweergave voor informatie over hoe uw SQL-database maakt gebruik van resources:

1. Om te kijken naar de afgelopen week resource voor de database-userdb1 gebruiken, kunt u deze query uitvoeren:
   
        SELECT *
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND
              start_time > DATEADD(day, -7, GETDATE())
        ORDER BY start_time DESC;
2. Om te evalueren hoe goed uw werkbelasting geschikt is voor het prestatieniveau dat is, moet u inzoomen op elk aspect van de metrische gegevens voor resources: CPU, leesbewerkingen, schrijfbewerkingen, aantal werkrollen en sessies. Hier volgt een gewijzigde query's uitvoeren met behulp van **sys.resource_stats** voor het rapporteren van de gemiddelde en maximale waarden van deze metrische gegevens voor resources:
   
        SELECT
            avg(avg_cpu_percent) AS 'Average CPU use in percent',
            max(avg_cpu_percent) AS 'Maximum CPU use in percent',
            avg(avg_data_io_percent) AS 'Average physical data IO use in percent',
            max(avg_data_io_percent) AS 'Maximum physical data IO use in percent',
            avg(avg_log_write_percent) AS 'Average log write use in percent',
            max(avg_log_write_percent) AS 'Maximum log write use in percent',
            avg(max_session_percent) AS 'Average % of sessions',
            max(max_session_percent) AS 'Maximum % of sessions',
            avg(max_worker_percent) AS 'Average % of workers',
            max(max_worker_percent) AS 'Maximum % of workers'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
3. Met deze informatie over de gemiddelde en maximale waarden van elke metrisch resourcegegeven, kunt u beoordelen hoe goed uw workload past het prestatieniveau dat u hebt gekozen. Normaal gesproken gemiddelde waarden van **sys.resource_stats** bieden u een goede basis te gebruiken op basis van de doelgrootte. Uw primaire meting stick dient te zijn. Voor een voorbeeld mogelijk gebruikt u de servicelaag Standard met S2-prestatieniveau. Het gemiddelde gebruik percentages voor CPU- en i/o-leesbewerkingen, schrijfbewerkingen zijn dan 40 procent, het gemiddelde aantal werknemers lager is dan 50 en het gemiddelde aantal sessies lager is dan 200. Uw workload passen mogelijk in het prestatieniveau S1. Het is gemakkelijk om te zien of uw database in de sessie en werknemer limieten past. Als u wilt zien of een database een lager prestatieniveau met betrekking tot CPU past, leest en schrijft, verdeelt het DTU-nummer van het lagere prestatieniveau door het DTU-nummer van het huidige prestatieniveau en vervolgens het resultaat vermenigvuldigd met 100:
   
    **S1 DTU / S2 DTU * 100 = 20 / 50 * 100 = 40**
   
    Het resultaat is de relatieve prestatieverschil tussen de twee prestatieniveaus in een percentage. Als uw gebruik van de resource niet deze hoeveelheid overschrijdt, kan uw workload passen in de lager prestatieniveau. U moet echter te kijken naar alle bereikwaarden van waarden voor het gebruik van resources, en te bepalen, op percentage, hoe vaak de databaseworkload zou past binnen de lager prestatieniveau. De volgende query levert het percentage passend per resourcedimensie, op basis van de drempelwaarde van 40 procent die in dit voorbeeld is berekend:
   
        SELECT
            (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log Write Fit Percent'
            ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 40 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical Data IO Fit Percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Op basis van uw database serviceniveaudoelstelling (SLO), kunt u bepalen of uw workload de lager prestatieniveau past. Als de databaseworkload SLO 99,9 procent is en de voorgaande query de waarden groter is dan 99,9% voor alle drie resourcedimensies retourneert, wordt uw workload waarschijnlijk in de lager prestatieniveau past.
   
    Kijken naar het passend percentage geeft ook inzicht in of u naar de volgende hoger prestatieniveau overstappen moet om te voldoen aan de SLO. Userdb1 ziet u bijvoorbeeld het volgende CPU-gebruik voor de afgelopen week:
   
   | Gemiddelde CPU-percentage | Maximale CPU-percentage |
   | --- | --- |
   | 24.5 |100.00 |
   
    De gemiddelde CPU is over een kwart van de limiet van het prestatieniveau dat zijn voor het prestatieniveau van de database geschikt zou. Maar de maximale waarde te zien dat de database heeft de limiet van het prestatieniveau dat is bereikt. Wilt u verplaatsen naar de volgende hoger prestatieniveau? Bekijken hoe vaak uw werkbelasting bereikt 100 procent en dit vervolgens vergelijken met de databaseworkload SLO.
   
        SELECT
        (COUNT(database_name) - SUM(CASE WHEN avg_cpu_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'CPU fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_log_write_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Log write fit percent'
        ,(COUNT(database_name) - SUM(CASE WHEN avg_data_io_percent >= 100 THEN 1 ELSE 0 END) * 1.0) / COUNT(database_name) AS 'Physical data IO fit percent'
        FROM sys.resource_stats
        WHERE database_name = 'userdb1' AND start_time > DATEADD(day, -7, GETDATE());
   
    Deze query retourneert een waarde minder dan 99,9 procent voor een van de drie resourcedimensies, kunt u overwegen om een van beide naar de volgende hoger prestatieniveau of toepassing afstemmen technieken gebruiken om de belasting op de SQL-database te verminderen.
4. In deze oefening wordt ook rekening gehouden uw toename van de verwachte werkbelasting in de toekomst.

Voor elastische pools kunt u afzonderlijke databases in de pool bewaken met de technieken die in deze sectie zijn beschreven. U kunt echter ook de groep als geheel bewaken. Zie [Een elastische pool bewaken en beheren](sql-database-elastic-pool-manage-portal.md) voor meer informatie.


### <a name="maximum-concurrent-requests"></a>Maximum aantal gelijktijdige aanvragen
Als u wilt zien van het aantal gelijktijdige aanvragen, deze Transact-SQL-query op uw SQL-database worden uitgevoerd:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R

Voor het analyseren van de werkbelasting van een on-premises SQL Server-database, wijzigt u deze query kunt u filteren op de specifieke database die u wilt analyseren. Als u een on-premises database met de naam MijnDatabase hebt, retourneert deze Transact-SQL-query bijvoorbeeld het aantal gelijktijdige aanvragen in de database:

    SELECT COUNT(*) AS [Concurrent_Requests]
    FROM sys.dm_exec_requests R
    INNER JOIN sys.databases D ON D.database_id = R.database_id
    AND D.name = 'MyDatabase'

Dit is slechts een momentopname op een bepaald tijdstip. Als u een beter inzicht in uw werkbelasting en de vereisten voor gelijktijdige aanvraag, moet u voor het verzamelen van veel voorbeelden na verloop van tijd.

### <a name="maximum-concurrent-logins"></a>Maximum aantal gelijktijdige aanmeldingen
U kunt uw gebruikers en toepassingen patronen voor een beter beeld van de frequentie van aanmeldingen analyseren. U kunt ook echte belastingen uitvoeren in een testomgeving om ervoor te zorgen dat u niet te maken met bent deze of andere limieten we in dit artikel bespreken. Er is een eenvoudige query uitvoeren of dynamische beheerweergave (DMV) waarin kunt u gelijktijdige aanmelding wordt geteld of geschiedenis niet.

Als meerdere clients de verbindingsreeks gebruiken, verifieert de service voor elke aanmelding. Als er 10 gebruikers tegelijk verbinding met een database met behulp van dezelfde gebruikersnaam en wachtwoord, wordt 10 gelijktijdige aanmeldingen. Deze limiet geldt alleen voor de duur van de aanmelding en verificatie. Als dezelfde 10 gebruikers verbinding met de database sequentieel worden verwerkt, wordt het aantal gelijktijdige aanmeldingen nooit groter zijn dan 1.

> [!NOTE]
> Op dit moment is deze limiet niet van toepassing op databases in elastische pools.
> 
> 

### <a name="maximum-sessions"></a>Maximum aantal sessies
Als u wilt zien van het aantal huidige actieve sessies, deze Transact-SQL-query op uw SQL-database worden uitgevoerd:

    SELECT COUNT(*) AS [Sessions]
    FROM sys.dm_exec_connections

Als u een on-premises SQL Server-werkbelasting analyseert, wijzigt u de query om zich te richten op een specifieke database. Deze query kunt u mogelijke sessie behoeften voor de database te bepalen of u overweegt u deze verplaatst naar Azure SQL Database.

    SELECT COUNT(*)  AS [Sessions]
    FROM sys.dm_exec_connections C
    INNER JOIN sys.dm_exec_sessions S ON (S.session_id = C.session_id)
    INNER JOIN sys.databases D ON (D.database_id = S.database_id)
    WHERE D.name = 'MyDatabase'

Deze query's retourneren, een aantal point-in-time. Als u meerdere voorbeelden na verloop van tijd verzamelt, hebt u het beste begrip van de sessie gebruikt.

Voor de analyse van de SQL-Database, kunt u historische statistieken opvragen op sessies door het opvragen van de [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) weergeven en controleren van de **active_session_count** kolom. 

## <a name="next-steps"></a>Volgende stappen

- Automatisch afstemmen van de indexen van de database en query-uitvoering plannen met behulp van [Azure SQL Database automatisch afstemmen](sql-database-automatic-tuning.md).
- Bewaken van prestaties van de database automatisch met [Intelligent Insights van Azure SQL](sql-database-intelligent-insights.md). Deze functie biedt diagnostische gegevens en analyse van problemen met prestaties van de hoofdoorzaak.
