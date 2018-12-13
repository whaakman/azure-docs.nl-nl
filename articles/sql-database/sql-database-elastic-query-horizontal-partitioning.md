---
title: Rapportage over uitgeschaalde clouddatabases | Microsoft Docs
description: over het instellen van elastische query's via horizontale partities
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: de96de96d68164d021f8b823e69bc52322642aa7
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52865400"
---
# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Rapportage over uitgeschaalde clouddatabases (preview)
![Query's uitvoeren voor shards][1]

Shard-databases rijen verdelen over een uitgeschaalde gegevens laag. Het schema is vrijwel identiek voor alle deelnemende databases, ook wel bekend als horizontale partitionering. Met behulp van een elastische query's, kunt u rapporten met betrekking alle databases in een shard-database tot maken.

Zie voor een snel starten, [rapportage voor uitgeschaalde clouddatabases](sql-database-elastic-query-getting-started.md).

Zie voor niet-shard-databases, [Query voor clouddatabases met verschillende schema's](sql-database-elastic-query-vertical-partitioning.md). 

## <a name="prerequisites"></a>Vereisten
* Een shard-toewijzing met behulp van de clientbibliotheek van elastische database maken. Zie [Shard-Toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md). Of gebruik van de voorbeeld-app in [aan de slag met elastische Databasehulpprogramma's](sql-database-elastic-scale-get-started.md).
* U kunt ook [bestaande databases migreren naar uitgeschaalde databases](sql-database-elastic-convert-to-use-elastic-tools.md).
* De gebruiker moet beschikken over machtigingen voor een externe gegevensbron niet wijzigen. Deze machtiging is opgenomen in de machtiging ALTER DATABASE.
* EEN externe gegevensbron ALTER machtigingen nodig om te verwijzen naar de onderliggende gegevensbron.

## <a name="overview"></a>Overzicht
Deze instructies uit maken de weergave van de metagegevens van de shard-gegevens-laag in de query's in elastic database. 

1. [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx)
2. [MAKEN VAN DATABASE-SCOPED REFERENTIE](https://msdn.microsoft.com/library/mt270260.aspx)
3. [EXTERNE GEGEVENSBRON MAKEN](https://msdn.microsoft.com/library/dn935022.aspx)
4. [EXTERNE TABEL MAKEN](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 maken binnen het bereik van databasehoofdsleutel en referenties
De referentie op die wordt gebruikt door de elastische query's verbinding maken met uw externe databases.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Zorg ervoor dat de *"\<gebruikersnaam\>"* omvat geen *"\@servername"* achtervoegsel. 
> 
> 

## <a name="12-create-external-data-sources"></a>1.2 externe gegevensbronnen maken
Syntaxis:

    <External_Data_Source> ::=    
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH                                              
            (TYPE = SHARD_MAP_MANAGER,
                       LOCATION = '<fully_qualified_server_name>',
            DATABASE_NAME = ‘<shardmap_database_name>',
            CREDENTIAL = <credential_name>, 
            SHARD_MAP_NAME = ‘<shardmapname>’ 
                   ) [;] 

### <a name="example"></a>Voorbeeld
    CREATE EXTERNAL DATA SOURCE MyExtSrc 
    WITH 
    ( 
        TYPE=SHARD_MAP_MANAGER,
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ShardMapDatabase', 
        CREDENTIAL= SMMUser, 
        SHARD_MAP_NAME='ShardMap' 
    );

De lijst met huidige externe gegevensbronnen ophalen: 

    select * from sys.external_data_sources; 

De externe gegevensbron verwijst naar de shard-toewijzing. Een elastische query gebruikt vervolgens de externe gegevensbron en de onderliggende shard-toewijzing om inventariseren van de databases die deel uitmaken van de gegevenslaag. Dezelfde referenties worden gebruikt voor het lezen van de shard-toewijzing en toegang tot de gegevens over de shards tijdens de verwerking van een elastische query's. 

## <a name="13-create-external-tables"></a>1.3 externe tabellen maken
Syntaxis:  

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name  
        ( { <column_definition> } [ ,...n ])     
        { WITH ( <sharded_external_table_options> ) }
    ) [;]  

    <sharded_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>,       
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 
      DISTRIBUTION = SHARDED(<sharding_column_name>) | REPLICATED |ROUND_ROBIN

**Voorbeeld**

    CREATE EXTERNAL TABLE [dbo].[order_line]( 
         [ol_o_id] int NOT NULL, 
         [ol_d_id] tinyint NOT NULL,
         [ol_w_id] int NOT NULL, 
         [ol_number] tinyint NOT NULL, 
         [ol_i_id] int NOT NULL, 
         [ol_delivery_d] datetime NOT NULL, 
         [ol_amount] smallmoney NOT NULL, 
         [ol_supply_w_id] int NOT NULL, 
         [ol_quantity] smallint NOT NULL, 
         [ol_dist_info] char(24) NOT NULL 
    ) 

    WITH 
    ( 
        DATA_SOURCE = MyExtSrc, 
         SCHEMA_NAME = 'orders', 
         OBJECT_NAME = 'order_details', 
        DISTRIBUTION=SHARDED(ol_w_id)
    ); 

De lijst met externe tabellen opgehaald uit de huidige database: 

    SELECT * from sys.external_tables; 

Externe tabellen verwijderen:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Opmerkingen
De gegevens\_bron component definieert de externe gegevensbron (een shard-toewijzing) die wordt gebruikt voor de externe tabel.  

Het SCHEMA\_naam en het OBJECT\_naam van de EU definitie van de externe tabel worden toegewezen aan een tabel in een ander schema. Als u dit weglaat, wordt het schema van het externe object wordt ervan uitgegaan dat 'dbo' en de naam ervan wordt uitgegaan moet gelijk zijn aan de naam van de externe tabel worden gedefinieerd. Dit is handig als de naam van uw externe tabel is al in gebruik in de database waar u wilt maken van de externe tabel. Bijvoorbeeld, u wilt definiëren van een externe tabel om een samengevoegde weergave van catalogusweergaven of DMV's op uw geschaalde gegevens laag. Aangezien lokaal catalogusweergaven en DMV's bestaat, kunt u hun namen voor de definitie van de externe tabel niet gebruiken. In plaats daarvan gebruik van een andere naam en gebruik de catalogusweergave of van de DMV naam in het SCHEMA\_naam en/of OBJECT\_naam van de EU. (Zie het voorbeeld hieronder.) 

De component distributie Hiermee geeft u de gegevensdistributie die worden gebruikt voor deze tabel. De queryprocessor maakt gebruik van de informatie in de component distributie naar de meest efficiënte query's bouwen.  

1. **SHARD** betekent dat gegevens is horizontaal gepartitioneerd in de databases. De te nemen partitionerende sleutel voor de Gegevensdistributie is de **< sharding_column_name >** parameter.
2. **GEREPLICEERD** betekent dat identieke kopieën van de tabel aanwezig op elke database zijn. Het is uw verantwoordelijkheid om ervoor te zorgen dat de replica's identiek voor de databases zijn.
3. **RONDE\_ROBIN** betekent dat de tabel horizontaal is gepartitioneerd met behulp van een methode voor het distribueren van afhankelijk zijn van toepassing. 

**Gegevens in lagen verwijzing**: de externe tabel DDL naar een externe gegevensbron verwijst. De externe gegevensbron Hiermee geeft u een shard-toewijzing waarmee u de externe tabel met de informatie die nodig zijn voor alle databases in uw gegevenslaag vinden. 

### <a name="security-considerations"></a>Beveiligingsoverwegingen
Gebruikers met toegang tot de externe tabel krijgen automatisch toegang tot de onderliggende externe tabellen onder de referentie gegeven in de definitie van de externe gegevensbron. Voorkomen dat ongewenste verhoging van bevoegdheden via de referentie van de externe gegevensbron. Gebruik GRANT of REVOKE voor een externe tabel net alsof het een gewone tabellen.  

Nadat u uw externe gegevensbron en uw externe tabellen hebt gedefinieerd, kunt u nu volledige T-SQL gebruiken via uw externe tabellen.

## <a name="example-querying-horizontal-partitioned-databases"></a>Voorbeeld: uitvoeren van query's horizontaal gepartitioneerde databases
De volgende query voert een koppeling drie richtingen tussen warehouses, orders en regels en maakt gebruik van verschillende statistische functies en een selectieve filter. Wordt ervan uitgegaan dat (1) horizontale partitionering (sharding) en (2) dat warehouses, orders en regels shard door de id-kolom van het datawarehouse zijn en dat de elastische query's kunt de wordt op de shards plaatsen en de dure deel van de query op de shards in verwerken parallelle. 

    select  
         w_id as warehouse,
         o_c_id as customer,
         count(*) as cnt_orderline,
         max(ol_quantity) as max_quantity,
         avg(ol_amount) as avg_amount, 
         min(ol_delivery_d) as min_deliv_date
    from warehouse 
    join orders 
    on w_id = o_w_id
    join order_line 
    on o_id = ol_o_id and o_w_id = ol_w_id 
    where w_id > 100 and w_id < 200 
    group by w_id, o_c_id 

## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Opgeslagen procedure voor uitvoering op afstand T-SQL: sp\_execute_remote
Elastische query introduceert ook een opgeslagen procedure die rechtstreekse toegang tot de shards biedt. De opgeslagen procedure is aangeroepen [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) en kan worden gebruikt voor het uitvoeren van extern opgeslagen procedures of T-SQL-code op de externe databases. Het bevat de volgende parameters: 

* Naam van de gegevensbron (nvarchar): de naam van de externe gegevensbron van het type RDBMS. 
* Query (nvarchar): de T-SQL-query moet worden uitgevoerd op elke shard. 
* Parameterdeclaratie (nvarchar) - optioneel: de tekenreeks met typedefinities gegevens voor de parameters die in de queryparameter (zoals sp_executesql) gebruikt. 
* Waarde parameterlijst - optionele: door komma's gescheiden lijst met parameterwaarden (zoals sp_executesql).

De sp\_uitvoeren\_extern maakt gebruik van de externe gegevensbron die is opgegeven in de Aanroepparameters voor het uitvoeren van de gegeven T-SQL-instructie op de externe databases. De referentie van de externe gegevensbron maakt verbinding met de shardmap manager-database en de externe databases.  

Voorbeeld: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Connectiviteit voor hulpprogramma 's
Normale SQL Server-verbindingsreeksen om uw toepassing verbinding te gebruiken de hulpprogramma's uw BI en data-integratie met de database met de definities van uw externe tabel. Zorg ervoor dat SQL Server wordt ondersteund als een gegevensbron voor het hulpprogramma. Vervolgens naslaginformatie over de elastische query uitvoeren op database, zoals een andere SQL Server-database verbonden met het hulpprogramma en gebruik externe tabellen uit uw hulpprogramma of de toepassing alsof deze lokaal tabellen. 

## <a name="best-practices"></a>Aanbevolen procedures
* Zorg ervoor dat de elastische query uitvoeren op database-eindpunt toegang tot de database shardmap en alle shards worden gewijzigd via de firewalls SQL-database gekregen heeft.  
* Valideren of de verdeling van de gegevens die zijn gedefinieerd door de externe tabel af te dwingen. Als uw werkelijke gegevensdistributie van de distributie die is opgegeven in de definitie van uw tabel verschilt, kunnen uw query's, onverwachte resultaten opleveren. 
* Elastische query's werkt op dit moment niet met shard-opheffing wanneer predicaten via de sharding-sleutel wilt toestaan dat het veilig uitsluiten van bepaalde shards van de verwerking.
* Elastische query's het beste werkt voor query's waar de meeste van de berekening kan worden uitgevoerd op de shards. Doorgaans downloadt u de beste prestaties van query's met selectieve filter predicaten die kan worden geëvalueerd op de shards of joins via de partitionering sleutels die kunnen worden uitgevoerd op een manier uitgelijnd op alle shards. Andere querypatronen mogelijk grote hoeveelheden gegevens laden vanuit de shards naar het hoofdknooppunt en slecht mag uitvoeren

## <a name="next-steps"></a>Volgende stappen

* Zie voor een overzicht van elastische query [elastische query overzicht](sql-database-elastic-query-overview.md).
* Zie voor een verticale partitionering zelfstudie [aan de slag met query van meerdere databases (verticale partitionering)](sql-database-elastic-query-getting-started-vertical.md).
* Zie voor een voorbeeld en de syntaxis van query's voor verticaal gepartitioneerde gegevens [opvragen van verticaal gepartitioneerde gegevens)](sql-database-elastic-query-vertical-partitioning.md)
* Zie voor een zelfstudie horizontale partitionering (sharding) [aan de slag met elastische query's voor horizontale partitionering (sharding)](sql-database-elastic-query-getting-started.md).
* Zie [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) voor een opgeslagen procedure die een Transact-SQL-instructie op een één externe SQL-Database van Azure of een set van databases die fungeren als shards in een horizontale partitionering wordt uitgevoerd.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
