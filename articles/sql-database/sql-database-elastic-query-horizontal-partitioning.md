---
title: Rapportage over cloud uitgebreide databases | Microsoft Docs
description: het instellen van elastische query's via horizontale partities
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: f86eccb8-6323-4ba7-8559-8a7c039049f3
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: mlandzic
ms.openlocfilehash: 41accea2e94fc763d0dcbba709829ec07453da78
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="reporting-across-scaled-out-cloud-databases-preview"></a>Rapportage over cloud uitgebreide databases (preview)
![Vragen over shards][1]

Shard databases rijen verdelen over een uitgebreid uit gegevens die laag. Het schema is vrijwel identiek voor alle deelnemende databases, ook wel bekend als horizontaal partitioneren. Een elastische query gebruiken, kunt u rapporten die alle databases in een shard-database omvatten maken.

Zie voor een snel starten [rapportage over cloud uitgebreide databases](sql-database-elastic-query-getting-started.md).

Zie voor niet-shard databases [Query via cloud-databases met verschillende schema's](sql-database-elastic-query-vertical-partitioning.md). 

## <a name="prerequisites"></a>Vereisten
* Maakt een shard-toewijzing met behulp van de clientbibliotheek voor elastische database. Zie [Shard kaart management](sql-database-elastic-scale-shard-map-management.md). Of gebruik de voorbeeld-app in [aan de slag met hulpprogramma's voor elastische database](sql-database-elastic-scale-get-started.md).
* U kunt ook [migreren van bestaande databases naar de uitgebreide databases](sql-database-elastic-convert-to-use-elastic-tools.md).
* De gebruiker moet beschikken over een externe gegevensbron ALTER machtiging. Deze machtiging is opgenomen in de machtiging ALTER DATABASE.
* EEN externe gegevensbron ALTER machtigingen nodig om te verwijzen naar de onderliggende gegevensbron.

## <a name="overview"></a>Overzicht
Deze instructies maken de weergave van de metagegevens van de laag van de gedeelde gegevens in de elastische database. 

1. [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx)
2. [MAKEN VAN DATABASE-SCOPED REFERENTIE](https://msdn.microsoft.com/library/mt270260.aspx)
3. [EXTERNE GEGEVENSBRON MAKEN](https://msdn.microsoft.com/library/dn935022.aspx)
4. [CREATE EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="11-create-database-scoped-master-key-and-credentials"></a>1.1 bereik databasehoofdsleutel en referenties maken
De referentie wordt gebruikt door de elastische query verbinding maken met uw externe databases.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Zorg ervoor dat de *'\<gebruikersnaam\>'* omvat geen *'@servername'* achtervoegsel. 
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

De externe gegevensbron verwijst naar de shard-kaart. Een elastische query gebruikt vervolgens de externe gegevensbron en de onderliggende shard-toewijzing opsommen van de databases die deel uitmaken van de gegevenslaag. Dezelfde referenties worden gebruikt om te lezen van de shard-toewijzing en toegang tot de gegevens op de shards tijdens het verwerken van een elastische query. 

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

De lijst met externe tabellen ophalen uit de huidige database: 

    SELECT * from sys.external_tables; 

Externe tabellen verwijderen:

    DROP EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name. ] table_name[;]

### <a name="remarks"></a>Opmerkingen
De gegevens\_bron component definieert de externe gegevensbron (een shard-kaart) die wordt gebruikt voor de externe tabel.  

Het SCHEMA\_naam en het OBJECT\_naam componenten definitie van de externe tabel toewijzen aan een tabel in een ander schema. Als u dit weglaat, het schema van het externe object wordt ervan uitgegaan dat 'dbo' en de naam ervan wordt ervan uitgegaan dat overeenkomt met de naam van de externe tabel wordt gedefinieerd. Dit is handig als de naam van de externe tabel is al in gebruik in de database waar u wilt maken van de externe tabel. Bijvoorbeeld, u wilt definiëren van een externe tabel om een samengevoegde weergave van catalogusweergaven of DMV's voor uw gegevens geschaalde uit laag. Aangezien lokaal catalogusweergaven en DMV's bestaat, kunt u hun namen voor de definitie van de externe tabel niet gebruiken. In plaats daarvan gebruik een andere naam en gebruik de catalogusweergave of de DMV naam in het SCHEMA\_naam en/of OBJECT\_naam componenten. (Zie het voorbeeld hieronder.) 

De component distributie bepaalt hoe de gegevens die wordt gebruikt voor deze tabel. De queryprocessor maakt gebruik van de informatie in de component DISTRIBUTIEPUNT om de meest efficiënte queryplannen samen te stellen.  

1. **SHARD** betekent dat gegevens horizontaal over de databases zijn gepartitioneerd. De te nemen partitionerende sleutel voor de Gegevensdistributie is de **< sharding_column_name >** parameter.
2. **GEREPLICEERD** betekent dat identieke kopieën van de tabel aanwezig op elke database zijn. Het is uw verantwoordelijkheid om ervoor te zorgen dat de replica's identiek voor de databases zijn.
3. **RONDE\_ROBIN** betekent dat de tabel horizontaal is gepartitioneerd met behulp van een distributiemethode van afhankelijk zijn van toepassing. 

**Gegevens servicetier verwijzing**: de externe tabel DDL naar een externe gegevensbron verwijst. De externe gegevensbron bevat geen shard-toewijzing waardoor de externe tabel met de benodigde gegevens voor alle databases vinden in de gegevenslaag. 

### <a name="security-considerations"></a>Beveiligingsoverwegingen
Gebruikers met toegang tot de externe tabel krijgen automatisch toegang tot de onderliggende externe tabellen onder de referentie die is opgegeven in de definitie van de externe gegevensbron. Vermijd ongewenste verhoging van bevoegdheden via de referenties van de externe gegevensbron. Gebruik GRANT of REVOKE voor een externe tabel net alsof het een gewone tabellen.  

Nadat u uw externe gegevensbron en uw externe tabellen hebt gedefinieerd, kunt u nu volledige T-SQL gebruiken via uw externe tabellen.

## <a name="example-querying-horizontal-partitioned-databases"></a>Voorbeeld: uitvoeren van query's horizontale gepartitioneerde databases
De volgende query voert een koppeling drie richtingen tussen datawarehouses, orders en regels en maakt gebruik van verschillende statistische functies en een selectieve filter. Er wordt vanuit gegaan (1) de horizontale partitionering (sharding) en (2) dat in datawarehouses, orders en regels shard door de id-kolom van het datawarehouse zijn en dat de elastische query kunt plaatsen op de verbindingen in de shards en de dure deel van de query op de shards parallel verwerken. 

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
Elastische query bevat ook een opgeslagen procedure die rechtstreekse toegang tot de shards biedt. De opgeslagen procedure is aangeroepen [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) en kan worden gebruikt voor het uitvoeren van extern opgeslagen procedures of T-SQL-code op de externe databases. Het bevat de volgende parameters: 

* Naam van de gegevensbron (nvarchar): de naam van de externe gegevensbron van het type RDBMS. 
* Query (nvarchar): de T-SQL-query uitgevoerd op elke shard. 
* Parameterdeclaratie (nvarchar) - optioneel: de tekenreeks met typedefinities gegevens voor de parameters in de Query-parameter (zoals sp_executesql) gebruikt. 
* Waarde parameterlijst - optioneel: door komma's gescheiden lijst met parameterwaarden (zoals sp_executesql).

De sp\_uitvoeren\_afstand maakt gebruik van de externe gegevensbron opgegeven in de Aanroepparameters voor het uitvoeren van de gegeven T-SQL-instructie op de externe databases. Gebruikt de referenties van de externe gegevensbron verbinding maken met de shardmap manager-database en de externe databases.  

Voorbeeld: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 

## <a name="connectivity-for-tools"></a>Connectiviteit voor hulpprogramma 's
Reguliere SQL Server-verbindingsreeksen om uw toepassing verbinding te gebruiken uw integratiefuncties BI en gegevens naar de database met de definities van de externe tabel. Zorg ervoor dat SQL Server wordt ondersteund als een gegevensbron voor het hulpprogramma. Vervolgens verwijzing naar de elastische database net als andere SQL Server-database verbonden met het hulpprogramma en de externe tabellen voor gebruik van uw hulpprogramma of de toepassing alsof ze lokaal tabellen. 

## <a name="best-practices"></a>Aanbevolen procedures
* Zorg ervoor dat de elastische database eindpunt toegang tot de database shardmap en alle shards via de SQL-database-firewalls gekregen heeft.  
* Valideren of af te dwingen de gegevensdistributie gedefinieerd door de externe tabel. Als uw werkelijke gegevensdistributie van het distributiepunt dat is opgegeven in de tabeldefinitie van de verschilt, kan uw query's kunnen onverwachte resultaten opleveren. 
* Elastische query momenteel geen uit shard elimineren wanneer predicaten via de sleutel sharding zou toe te staan om bepaalde shards veilig uitsluiten van de verwerking.
* Elastische query werkt het beste voor query's waar de meeste van de berekening kan worden uitgevoerd op de shards. U doorgaans ophalen de beste queryprestaties met selectief filter predicaten die kan worden geëvalueerd op de shards of joins via de partitionering sleutels die kunnen worden uitgevoerd op een manier uitgelijnd op alle shards. Andere querypatronen mogelijk moet het laden van grote hoeveelheden gegevens uit de shards met het hoofdknooppunt en goed

## <a name="next-steps"></a>Volgende stappen

* Zie voor een overzicht van elastische query [elastische query overzicht](sql-database-elastic-query-overview.md).
* Zie voor een verticale partitie zelfstudie [aan de slag met cross-databasequery (verticale partities)](sql-database-elastic-query-getting-started-vertical.md).
* Zie voor de syntaxis en voorbeelden query's voor verticaal gepartitioneerde gegevens [gegevens opvragen verticaal gepartitioneerd)](sql-database-elastic-query-vertical-partitioning.md)
* Zie voor een zelfstudie horizontaal partitioneren (sharding) [aan de slag met elastische query voor horizontale partitioneren (sharding)](sql-database-elastic-query-getting-started.md).
* Zie [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) voor een opgeslagen procedure die een Transact-SQL-instructie op een enkele externe Azure SQL Database of een set van databases die fungeren als shards in een horizontale partitieschema wordt uitgevoerd.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-horizontal-partitioning/horizontalpartitioning.png
<!--anchors-->
