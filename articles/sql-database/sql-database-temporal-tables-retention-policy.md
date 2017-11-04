---
title: Historische gegevens beheren in tijdelijke tabellen met bewaarbeleid | Microsoft Docs
description: Informatie over het gebruik van tijdelijke bewaarbeleid om historische gegevens onder uw beheer te houden.
services: sql-database
documentationcenter: 
author: bonova
manager: drasumic
editor: 
ms.assetid: 76cfa06a-e758-453e-942c-9f1ed6a38c2a
ms.service: sql-database
ms.custom: develop databases
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: Inactive
ms.date: 10/12/2016
ms.author: bonova
ms.openlocfilehash: b4e1524008837094b57a3df469439ceaebf9c166
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Historische gegevens in tijdelijke tabellen met bewaarbeleid beheren
Tijdelijke tabellen wordt mogelijk uitgebreid grootte van de database meer dan gewone tabellen, met name als u historische gegevens voor een langere periode behouden. Bewaarbeleid voor historische gegevens is daarom een belangrijk aspect van het plannen en beheren van de levenscyclus van elke tijdelijke tabel. Tijdelijke tabellen in Azure SQL Database worden geleverd met eenvoudig te gebruiken bewaren mechanisme waarmee u deze taak.

Bewaren van de tijdelijke geschiedenistabel kan worden geconfigureerd op het niveau van de afzonderlijke tabel, kunnen de gebruikers voor het maken van flexibele veroudering beleidsregels. Toepassen van de tijdelijke bewaarperiode is eenvoudig: slechts één parameter worden ingesteld tijdens het maken of schema wijzigen van de tabel vereist.

Nadat u een bewaarbeleid gedefinieerd, start Azure SQL Database regelmatig te controleren of er zijn historische rijen die in aanmerking voor automatische opruimen komen. Overeenkomende rijen te identificeren en hun verwijderen uit de geschiedenistabel optreden transparant, in de achtergrondtaak die is gepland en uitgevoerd door het systeem. Leeftijdsvoorwaarde voor de tabelrijen geschiedenis wordt gecontroleerd op basis van de kolom die het einde van de SYSTEM_TIME-periode. Als de bewaarperiode, bijvoorbeeld is ingesteld op zes maanden, voldoen tabelrijen in aanmerking komen voor het opruimen van aan de volgende voorwaarde:

````
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
````

In het voorgaande voorbeeld wordt vanuit gegaan dat **ValidTo** kolom komt overeen met het einde van de SYSTEM_TIME-periode.

## <a name="how-to-configure-retention-policy"></a>Het bewaarbeleid configureren?
Voordat u een bewaarbeleid voor een tijdelijke tabel configureert, Controleer eerst of tijdelijke historische bewaarperiode is ingeschakeld *op databaseniveau*.

````
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
````

Vlag database **is_temporal_history_retention_enabled** is standaard ingesteld op ON, maar gebruikers kunnen worden wijzigen met de instructie ALTER DATABASE. Het is ook automatisch ingesteld op OFF na [wijst naar een bepaald tijstip](sql-database-recovery-using-backups.md) bewerking. Voer de volgende instructie zodat het opruimen van de tijdelijke geschiedenistabel bewaren voor uw database:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

> [!IMPORTANT]
> U kunt de bewaarperiode voor tijdelijke tabellen, zelfs als configureren **is_temporal_history_retention_enabled** is uitgeschakeld, maar automatisch opschonen voor verouderde rijen niet in dat geval wordt geactiveerd.
> 
> 

Bewaarbeleid is tijdens maken van de tabel geconfigureerd door de waarde voor de parameter HISTORY_RETENTION_PERIOD te geven:

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

Azure SQL Database kunt u de bewaarperiode opgeven met behulp van verschillende tijdseenheden: dagen, weken, maanden en jaren. Als HISTORY_RETENTION_PERIOD wordt weggelaten, wordt oneindige bewaren verondersteld. U kunt ook expliciet oneindige sleutelwoord gebruiken.

In sommige gevallen kunt u configureren retentie na het maken van de tabel of als u wilt wijzigen, eerder geconfigureerde waarde. In dat geval gebruikt u ALTER TABLE-instructie:

````
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
````

> [!IMPORTANT]
> System_versioning instellen op OFF *blijven niet behouden* periodewaarde bewaren. System_versioning instellen op ON zonder HISTORY_RETENTION_PERIOD expliciet resultaten in de oneindige bewaarperiode opgegeven.
> 
> 

Gebruik de volgende query die lid wordt van de tijdelijke bewaren stuk vlag op het databaseniveau van de met de bewaarperiode voor afzonderlijke tabellen u controleert de huidige status van het bewaarbeleid door:

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


## <a name="how-sql-database-deletes-aged-rows"></a>Hoe SQL-Database worden verwijderd, verouderde rijen?
Het opruimproces is afhankelijk van de indexindeling van de tabel met de geschiedenis. Het is belangrijk dat u ziet dat *slechts geschiedenis tabellen met een geclusterde index (B-tree of columnstore) kunnen niet-oneindig bewaarbeleid geconfigureerd*. Een achtergrondtaak is gemaakt voor het uitvoeren van verouderde gegevens wissen voor alle tijdelijke tabellen met beperkte bewaarperiode.
Opruimen logica voor de geclusterde index rowstore (B-tree) Hiermee verwijdert u verouderde rij in kleinere reeksen (maximaal 10 kB) voor het minimaliseren van zware belasting op het databaselogboek en i/o-subsysteem. Hoewel opschonen logica maakt gebruik van vereiste B-tree-index, ordenen van verwijderingen voor de rijen die ouder zijn dan de bewaarperiode kan niet goed kan worden gegarandeerd. Daarom *eventuele afhankelijkheden worden pas van kracht op de volgorde van de opschoning in uw toepassingen*.

De opschoontaak voor de geclusterde columnstore Hiermee verwijdert u het volledige [rij groepen](https://msdn.microsoft.com/library/gg492088.aspx) tegelijk (doorgaans bevatten 1 miljoen rijen elke), die zeer efficiënte, vooral wanneer u historische gegevens in een hoog tempo wordt gegenereerd.

![De geclusterde columnstore-bewaren](./media/sql-database-temporal-tables-retention-policy/cciretention.png)

Uitstekend gegevenscompressie en efficiënte bewaren opschonen wordt geclusterde columnstore-index een ideale keuze voor scenario's wanneer uw werkbelasting snel hoge mate van historische gegevens genereert. Dit patroon is Typerend voor intensieve [transactionele verwerking werkbelastingen die gebruikmaken van tijdelijke tabellen](https://msdn.microsoft.com/library/mt631669.aspx) voor wijzigingen bijhouden en controle, trendanalyse of IoT gegevensopname.

## <a name="index-considerations"></a>Overwegingen voor index
De opschoontaak voor tabellen met de geclusterde index rowstore vereist index worden gestart met de kolom die het einde van de SYSTEM_TIME-periode overeenkomt. Als de index niet bestaat, kunt u een eindige bewaarperiode niet configureren:

*Bericht 13765, 16 niveau 1 staat <br> </br> op de tijdelijke systeemversietabel 'temporalstagetestdb.dbo.WebsiteUserInfo' eindig bewaarperiode instellen is mislukt omdat de geschiedenistabel ' temporalstagetestdb.dbo.WebsiteUserInfoHistory' bevat geen vereiste geclusterde index. Houd rekening met het maken van een geclusterde columnstore of B-tree-index die begint met de kolom die overeenkomt met het einde van de SYSTEM_TIME period voor de geschiedenistabel.*

Het is belangrijk dat u ziet dat de standaard geschiedenistabel gemaakt met Azure SQL Database al index, die voldoen aan de voorwaarden bewaarbeleid is geclusterd. Als u probeert te verwijderen die index in een tabel met beperkte bewaarperiode, mislukt de bewerking met de volgende fout:

*Bericht 13766, 16 niveau 1 staat <br> </br> kan de geclusterde index 'WebsiteUserInfoHistory.IX_WebsiteUserInfoHistory' niet verwijderen omdat deze wordt gebruikt voor het automatisch opschonen van verouderde gegevens. U kunt HISTORY_RETENTION_PERIOD infinite op de bijbehorende tijdelijke systeemversietabel instellen als u moet deze index verwijderen.*

Het opruimen van de geclusterde columnstore-index werkt optimaal als historische rijen worden ingevoegd in de oplopende volgorde (besteld op het einde van de periodekolom), die altijd het geval is wanneer de geschiedenistabel exclusief gebruik door het mechanisme voor SYSTEM_VERSIONIOING wordt gevuld. Als de rijen in de geschiedenistabel niet besteld door het einde van de periodekolom (dit is mogelijk het geval als u bestaande historische gegevens hebt gemigreerd), moet u opnieuw boven op B-tree rowstore-index die goed zijn besteld, als u de optimale geclusterde columnstore-index maken de prestaties.

Vermijd opnieuw opbouwen van de geclusterde columnstore-index in de geschiedenistabel met de beperkte bewaarperiode omdat kunnen worden gewijzigd in de Rijgroepen toe die zijn opgelegd door de bewerking voor de systeemversiebeheergeschiedenis bestellen. Als u geclusterde columnstore-index in de geschiedenistabel bouwen wilt, kunt u dat door opnieuw te maken op compatibele B-tree-index, behouden van volgorde in de rowgroups die nodig zijn voor reguliere opruimen doen. Dezelfde manier moet worden uitgevoerd als u de tijdelijke tabel maken met bestaande geschiedenistabel die kolomindex zonder gegarandeerde gegevensvolgorde geclusterde bevat:

````
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
````

Wanneer beperkte bewaarperiode is geconfigureerd voor de geschiedenistabel met de geclusterde columnstore-index, kunt u geen extra niet-geclusterde B-tree indexen op tabel maken:

````
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
````

Een poging tot het hierboven instructie execute is mislukt met de volgende fout:

*Bericht 13772, 16 niveau 1 staat <br> </br> kan niet-geclusterde index maken op een tijdelijke geschiedenistabel 'WebsiteUserInfoHistory' omdat er beperkte bewaarperiode en de geclusterde columnstore-index die zijn gedefinieerd.*

## <a name="querying-tables-with-retention-policy"></a>Opvragen van tabellen met bewaarbeleid
Historische rijen die overeenkomen met beperkte bewaarbeleid om te voorkomen dat resultaten onvoorspelbare en inconsistent, omdat verouderde rijen kunnen worden verwijderd door de opschoontaak automatisch alle query's op de tijdelijke tabel filteren *op elk punt in tijd en willekeurige volgorde*.

De volgende afbeelding ziet het queryplan voor een eenvoudige query:

````
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
````

Het queryplan bevat extra filter toegepast op het einde van de periodekolom (ValidTo) in de operator geclusterde Index scannen voor de geschiedenistabel (gemarkeerd). In dit voorbeeld wordt ervan uitgegaan dat één maand bewaarperiode is ingesteld op WebsiteUserInfo tabel.

![Queryfilter bewaren](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Echter, als u rechtstreeks query geschiedenistabel uitvoert, ziet u mogelijk rijen die ouder dan de opgegeven bewaartermijn tijdsperiode, maar zonder een garantie voor herhaalbare queryresultaten zijn. De volgende afbeelding ziet de uitvoeringsplan query voor de query voor de geschiedenistabel zonder aanvullende filters zijn toegepast:

![Geschiedenis zonder bewaren filter opvragen](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Vertrouw niet uw bedrijfslogica op de geschiedenistabel buiten de bewaarperiode lezen als u krijgt u mogelijk inconsistente of onverwachte resultaten. U wordt aangeraden tijdelijke query's te gebruiken met FOR SYSTEM_TIME-component voor het analyseren van gegevens in tijdelijke tabellen.

## <a name="point-in-time-restore-considerations"></a>Punt in tijd terugzetten overwegingen
Wanneer u een nieuwe database door maakt [bestaande database herstellen naar een bepaald punt in tijd](sql-database-recovery-using-backups.md), heeft tijdelijke bewaren uitgeschakeld op het databaseniveau. (**is_temporal_history_retention_enabled** vlag ingesteld op OFF). Deze functie kunt u onderzoeken alle historische rijen bij het herstellen, zonder dat u dat de verouderde rijen worden verwijderd voordat u deze query worden opgehaald. U kunt deze *inspecteren van historische gegevens buiten de geconfigureerde bewaarperiode*.

Stel dat een tijdelijke tabel die een maand bewaren opgegeven tijdsperiode heeft. Als uw database is gemaakt in de laag Premium-Service, kunt u zou kunnen maken van database-exemplaar met de status van de database van tot 35 dagen terug in het verleden. Die effectief kunt u historische rijen die maximaal 65 dagen rechtstreeks een query uitgevoerd op de geschiedenistabel analyseren.

Als u activeren tijdelijke bewaren opruimen wilt, voert u de volgende Transact-SQL-instructie na punt naar een bepaald tijstip:

````
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
````

## <a name="next-steps"></a>Volgende stappen
Bekijk voor meer informatie over het gebruik van tijdelijke tabellen in uw toepassingen, [aan de slag met tijdelijke tabellen in Azure SQL Database](sql-database-temporal-tables.md).

Ga naar Channel 9 horen een [echte klant tijdelijke implementatie Succesverhaal](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) en bekijk een [tijdelijke demonstratie live](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016).

Raadpleeg voor gedetailleerde informatie over tijdelijke tabellen [MSDN-documentatie](https://msdn.microsoft.com/library/dn935015.aspx).

