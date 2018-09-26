---
title: Beheren van historische gegevens analyseren in tijdelijke tabellen met beleid voor Gegevensretentie | Microsoft Docs
description: Informatie over het gebruik van tijdelijke bewaarbeleid om historische gegevens onder uw beheer te houden.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: f339cadc63d5e5cd934d07e7b0fffc6342ca04c7
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47159092"
---
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Beheren van historische gegevens analyseren in tijdelijke tabellen met beleid voor Gegevensretentie
Tijdelijke tabellen verhogen grootte van de database meer dan gewone tabellen, met name als u historische gegevens voor een langere periode bewaren. Bewaarbeleid voor historische gegevens is daarom een belangrijk aspect van plannen en beheren van de levenscyclus van elke tijdelijke tabel. Tijdelijke tabellen in Azure SQL Database worden geleverd met eenvoudig te gebruiken retentie-mechanisme waarmee u deze taak wordt uitgevoerd.

Geschiedenis van tijdelijke inhouding kan worden geconfigureerd op het niveau van de afzonderlijke tabel, kunnen de gebruikers te maken van flexibele veroudering beleidsregels. Het toepassen van tijdelijke inhouding is eenvoudig: deze moet slechts één parameter worden ingesteld tijdens het maken of een schema wijzigen van de tabel.

Na het definiëren van beleid voor Gegevensretentie, start Azure SQL Database regelmatig te controleren of er zijn historische rijen die in aanmerking komen voor automatische opruimen. Identificatie van de overeenkomende rijen en hun verwijderen uit de geschiedenistabel optreden transparant, in de achtergrondtaak die is gepland en uitgevoerd door het systeem. Leeftijdsvoorwaarde voor de geschiedenis tabelrijen wordt gecontroleerd op basis van de kolom die het einde van SYSTEM_TIME-periode. Als de bewaarperiode, bijvoorbeeld is ingesteld op zes maanden, voldoen om op te schonen in aanmerking komende rijen aan de volgende voorwaarde:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

In het voorgaande voorbeeld wordt ervan uitgegaan dat die **ValidTo** kolom komt overeen met het einde van SYSTEM_TIME-periode.

## <a name="how-to-configure-retention-policy"></a>Het configureren van beleid voor het bewaren?
Voordat u beleid voor het bewaren voor een tijdelijke tabel configureert, Controleer eerst of tijdelijke historische bewaarperiode is ingeschakeld *op databaseniveau*.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

De vlag database **is_temporal_history_retention_enabled** is standaard ingesteld op aan, maar gebruikers kunnen deze met de instructie ALTER DATABASE wijzigen. Het is ook automatisch ingesteld op OFF na [tijdstip](sql-database-recovery-using-backups.md) bewerking. Om in te schakelen tijdelijke geschiedenistabel retentieopschoning voor uw database, voert u de volgende instructie uit:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [!IMPORTANT]
> U kunt bewaren voor tijdelijke tabellen, zelfs als **is_temporal_history_retention_enabled** is uitgeschakeld, maar automatisch op te schonen voor verouderde rijen niet in dat geval wordt geactiveerd.
> 
> 

Het bewaarbeleid is geconfigureerd tijdens het maken van de tabel door de waarde voor de parameter infinite op te geven:

````
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
````

Azure SQL Database kunt u bewaarperiode opgeven met behulp van verschillende tijdseenheden: dagen, weken, maanden en jaren. Als infinite wordt weggelaten, wordt uitgegaan van oneindige bewaarperiode. U kunt ook expliciet oneindige sleutelwoord gebruiken.

In sommige scenario's kunt u bewaren configureren nadat de tabel is gemaakt of als u wilt wijzigen, eerder geconfigureerde waarde. In dat geval gebruikt u ALTER TABLE-instructie:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [!IMPORTANT]
> System_versioning instellen op OFF *blijven niet behouden* waarde voor de bewaarperiode. System_versioning instellen op ON zonder infinite expliciet resultaten in de bewaarperiode geen EINDDATUM opgegeven.
> 
> 

Als u wilt controleren op huidige status van het bewaarbeleid, gebruik de volgende query die lid wordt van de tijdelijke inhouding activering vlag op het databaseniveau van de met bewaarperioden voor afzonderlijke tabellen:

````
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
````


## <a name="how-sql-database-deletes-aged-rows"></a>Hoe SQL-Database verwijdert verouderde rijen?
Het opruimproces, is afhankelijk van de indexindeling van de geschiedenistabel. Het is belangrijk dat u ziet dat *alleen geschiedenis tabellen met een geclusterde index (B-tree- of columnstore) kunnen geen eindige bewaarbeleid geconfigureerd hebben*. Een achtergrondtaak wordt gemaakt voor het uitvoeren van verouderde gegevens opschonen voor alle tijdelijke tabellen met een eindige bewaarperiode.
Schijfopruiming logica voor de geclusterde index van de rowstore (B-tree) Hiermee verwijdert u verouderde rij in kleinere chunks (maximaal 10 K) druk op de databaselogboek- en i/o-subsysteem worden geminimaliseerd. Hoewel opschonen logische maakt gebruik van vereiste B-tree-index, ordenen van verwijderingen voor de rijen die ouder zijn dan de bewaarperiode kan niet goed kan worden gegarandeerd. Daarom kan *eventuele afhankelijkheden worden pas van kracht op de volgorde van de opschoning in uw toepassingen*.

De taak opschonen voor de geclusterde columnstore verwijdert gehele [rij groepen](https://msdn.microsoft.com/library/gg492088.aspx) in één keer (meestal bevatten 1 miljoen rijen elke), die is zeer efficiënt, met name wanneer de historische gegevens wordt gegenereerd in een hoog tempo.

![Geclusterde columnstore-retentie](./media/sql-database-temporal-tables-retention-policy/cciretention.png)

Uitstekende gegevenscompressie en efficiënte retentie opschonen wordt geclusterde columnstore-index een ideale keuze voor scenario's wanneer uw workload snel grote hoeveelheid historische gegevens wordt gegenereerd. Dit patroon is normaal voor intensieve [transactionele verwerking van workloads die gebruikmaken van tijdelijke tabellen](https://msdn.microsoft.com/library/mt631669.aspx) voor wijzigingen bijhouden en controle, trendanalyse of IoT-gegevensopname.

## <a name="index-considerations"></a>Overwegingen voor index
De taak opschonen voor tabellen met geclusterde rowstore-index moet index te beginnen met de kolom die het einde van SYSTEM_TIME-periode overeenkomt. Als de index bestaat, kunt u niet een eindige retentieperiode configureren:

*Msg 13765, 16 niveau 1 staat <br> </br> geen eindige retentieperiode is mislukt voor de tijdelijke systeemversietabel 'temporalstagetestdb.dbo.WebsiteUserInfo', omdat de geschiedenistabel ' temporalstagetestdb.dbo.WebsiteUserInfoHistory' bevat geen vereiste geclusterde index. Houd rekening met het maken van een geclusterde columnstore- of B-tree-index die begint met de kolom die overeenkomt met het einde van SYSTEM_TIME period voor de geschiedenistabel.*

Het is belangrijk dat u ziet dat de standaard-geschiedenistabel die al zijn gemaakt door Azure SQL Database index, die voldoen aan het beleid voor het bewaarbeleid is geclusterd. Als u probeert te verwijderen die index op een tabel met een eindige retentieperiode, mislukt de bewerking met de volgende fout:

*Msg 13766, 16 niveau 1 staat <br> </br> kan de geclusterde index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' niet verwijderen omdat deze wordt gebruikt voor het automatisch opschonen van verouderde gegevens. U kunt instellen op infinite history_retention_period voor de overeenkomende tijdelijke systeemversietabel als u wilt deze index verwijderen.*

Het opruimen van de geclusterde columnstore-index werkt optimaal als historische rijen zijn ingevoegd in de oplopende volgorde (besteld op het einde van de kolom period), die altijd het geval is wanneer de geschiedenistabel uitsluitend door het mechanisme voor SYSTEM_VERSIONIOING is gevuld. Als rijen in de geschiedenistabel zijn niet geordend op het einde van de kolom period (die mogelijk het geval als u bestaande historische gegevens hebt gemigreerd), moet u opnieuw boven op de B-tree rowstore-index die goed zijn besteld, voor een optimale geclusterde columnstore-index maken prestaties.

Vermijd opnieuw opbouwen van geclusterde columnstore-index voor de geschiedenistabel met de geen eindige retentieperiode omdat deze kan worden gewijzigd in de Rijgroepen op natuurlijke wijze die zijn opgelegd door de bewerking versiebeheer door het systeem te bestellen. Als u opnieuw opbouwen van geclusterde columnstore-index voor de geschiedenistabel wilt, dit doen door opnieuw te maken op compatibele B-tree-index, ordenen in de Rijgroepen nodig om op te schonen regelmatig gegevens te behouden. Diezelfde aanpak moet worden uitgevoerd als u de tijdelijke tabel maken met bestaande geschiedenistabel met de geclusterde kolomindex zonder order gegarandeerde gegevens:

````
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Als geen eindige retentieperiode voor de geschiedenistabel met de geclusterde columnstore-index is geconfigureerd, kunt u geen aanvullende niet-geclusterde B-tree indexen maken voor deze tabel:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Een poging om uit te voeren boven de instructie is mislukt met de volgende fout:

*Msg 13772, 16 niveau 1 staat <br> </br> kan niet-geclusterde index maken op een tijdelijke geschiedenistabel 'WebsiteUserInfoHistory' omdat er geen eindige retentieperiode en geclusterde columnstore-index die zijn gedefinieerd.*

## <a name="querying-tables-with-retention-policy"></a>Een query uitvoeren op tabellen met beleid voor Gegevensretentie
Alle query's op de tijdelijke tabel automatisch filteren historische rijen die overeenkomen met beperkte bewaarbeleid, om te voorkomen dat onvoorspelbaar en inconsistente resultaten, omdat de verouderde rijen kunnen worden verwijderd door de Schijfopruiming *op elk gewenst moment in de tijd en willekeurige volgorde*.

De volgende afbeelding ziet u het queryplan voor een eenvoudige query:

````
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
````

Het queryplan bevat een extra filter toegepast op het einde van de kolom period (ValidTo) in de operator geclusterde Index scannen voor de geschiedenistabel (gemarkeerd). In dit voorbeeld wordt ervan uitgegaan dat één maand bewaarperiode is ingesteld op WebsiteUserInfo tabel.

![Retentie-queryfilter](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Echter, als u een geschiedenistabel rechtstreeks query, ziet u mogelijk rijen die ouder dan de opgegeven bewaartermijn periode, maar zonder dat een garantie voor herhaalbare queryresultaten zijn. De volgende afbeelding ziet queryplan kan worden uitgevoerd voor de query voor de geschiedenistabel zonder extra filters toegepast:

![Een query uitvoeren op geschiedenis zonder retentie-filter](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Vertrouw niet uw zakelijke logica voor het lezen van geschiedenistabel buiten de bewaarperiode zoals u inconsistente of onverwachte resultaten krijgt mogelijk. U wordt aangeraden dat u tijdelijke query's met FOR SYSTEM_TIME-component gebruiken voor het analyseren van gegevens in de tijdelijke tabellen.

## <a name="point-in-time-restore-considerations"></a>Punt in tijd herstel overwegingen
Wanneer u een nieuwe database door maakt [bestaande database herstellen naar een bepaald punt in tijd](sql-database-recovery-using-backups.md), heeft tijdelijke inhouding uitgeschakeld op het databaseniveau van de. (**is_temporal_history_retention_enabled** vlag ingesteld op OFF). Deze functie kunt u het onderzoeken van alle historische rijen bij het herstellen, zonder u zorgen te maken dat verouderde rijen zijn verwijderd voordat u query's voor uitvoert. U kunt deze *inspecteren van historische gegevens buiten de geconfigureerde bewaarperiode*.

Stel dat een tijdelijke tabel één maand bewaarperiode opgegeven tijdsperiode heeft. Als uw database is gemaakt in Premium-servicelaag, zou u kunnen maken van database-exemplaar met de status van de database omhoog tot 35 dagen terug in het verleden zijn. Die effectief kunt u voor het analyseren van historische rijen die door het opvragen van de geschiedenistabel rechtstreeks maximaal 65 dagen oud.

Als u activeren tijdelijke retentieopschoning wilt, voert u de volgende Transact-SQL-instructie na punt bepaald tijstip:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

## <a name="next-steps"></a>Volgende stappen
Bekijk voor meer informatie over het gebruik van tijdelijke tabellen in uw toepassingen, [aan de slag met tijdelijke tabellen in Azure SQL Database](sql-database-temporal-tables.md).

Ga naar Channel 9 graag een [Succesverhaal van een echte klant tijdelijke implementatie](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) en bekijk een [tijdelijke demonstratie live](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

Raadpleeg voor gedetailleerde informatie over tijdelijke tabellen [MSDN-documentatie](https://msdn.microsoft.com/library/dn935015.aspx).

