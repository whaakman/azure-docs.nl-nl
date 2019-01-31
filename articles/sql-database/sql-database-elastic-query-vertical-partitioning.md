---
title: Query voor clouddatabases met verschillende schema's | Microsoft Docs
description: over het instellen van query's die databaseoverschrijdend via verticale partities
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: sstein
manager: craigg
ms.date: 01/25/2019
ms.openlocfilehash: e7ba8057cd22c5cc1080b4a6d95f17bf76d4acb2
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55459431"
---
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Query's uitvoeren voor clouddatabases met verschillende schema's (preview)

![Query's uitvoeren voor tabellen in verschillende databases][1]

Verticaal gepartitioneerd databases gebruiken verschillende sets van tabellen op verschillende databases. Dit betekent dat het schema verschilt voor verschillende databases. Alle tabellen voor de voorraad zijn bijvoorbeeld op de ene database terwijl alle accounting-gerelateerde tabellen op een tweede database zijn. 

## <a name="prerequisites"></a>Vereisten

* De gebruiker moet beschikken over machtigingen voor een externe gegevensbron niet wijzigen. Deze machtiging is opgenomen in de machtiging ALTER DATABASE.
* EEN externe gegevensbron ALTER machtigingen nodig om te verwijzen naar de onderliggende gegevensbron.

## <a name="overview"></a>Overzicht

> [!NOTE]
> In tegenstelling tot bij horizontale partitionering, deze DDL-componenten niet afhankelijk zijn over het definiëren van een gegevenslaag met een shard-toewijzing via de clientbibliotheek van elastische database.
>

1. [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx)
2. [MAKEN VAN DATABASE-SCOPED REFERENTIE](https://msdn.microsoft.com/library/mt270260.aspx)
3. [EXTERNE GEGEVENSBRON MAKEN](https://msdn.microsoft.com/library/dn935022.aspx)
4. [CREATE EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="create-database-scoped-master-key-and-credentials"></a>De hoofdsleutel van de databaseconfiguratie en referenties maken

De referentie op die wordt gebruikt door de elastische query's verbinding maken met uw externe databases.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'master_key_password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Zorg ervoor dat de `<username>` omvat geen **"\@servername"** achtervoegsel. 
>

## <a name="create-external-data-sources"></a>Externe gegevensbronnen maken

Syntaxis:

    <External_Data_Source> ::=
    CREATE EXTERNAL DATA SOURCE <data_source_name> WITH 
               (TYPE = RDBMS,
                LOCATION = ’<fully_qualified_server_name>’,
                DATABASE_NAME = ‘<remote_database_name>’,  
                CREDENTIAL = <credential_name> 
                ) [;] 

> [!IMPORTANT]
> Het typeparameter moet worden ingesteld op **RDBMS**. 
>

### <a name="example"></a>Voorbeeld

Het volgende voorbeeld wordt het gebruik van de instructie CREATE voor externe gegevensbronnen. 

    CREATE EXTERNAL DATA SOURCE RemoteReferenceData 
    WITH 
    ( 
        TYPE=RDBMS, 
        LOCATION='myserver.database.windows.net', 
        DATABASE_NAME='ReferenceData', 
        CREDENTIAL= SqlUser 
    ); 

De lijst met huidige externe gegevensbronnen ophalen: 

    select * from sys.external_data_sources; 

### <a name="external-tables"></a>Externe tabellen

Syntaxis:

    CREATE EXTERNAL TABLE [ database_name . [ schema_name ] . | schema_name . ] table_name  
    ( { <column_definition> } [ ,...n ])     
    { WITH ( <rdbms_external_table_options> ) } 
    )[;] 

    <rdbms_external_table_options> ::= 
      DATA_SOURCE = <External_Data_Source>, 
      [ SCHEMA_NAME = N'nonescaped_schema_name',] 
      [ OBJECT_NAME = N'nonescaped_object_name',] 

### <a name="example"></a>Voorbeeld

```sql
    CREATE EXTERNAL TABLE [dbo].[customer]( 
        [c_id] int NOT NULL, 
        [c_firstname] nvarchar(256) NULL, 
        [c_lastname] nvarchar(256) NOT NULL, 
        [street] nvarchar(256) NOT NULL, 
        [city] nvarchar(256) NOT NULL, 
        [state] nvarchar(20) NULL, 
        [country] nvarchar(50) NOT NULL, 
    ) 
    WITH 
    ( 
           DATA_SOURCE = RemoteReferenceData 
    ); 
```

Het volgende voorbeeld laat zien hoe de lijst met externe tabellen ophalen uit de huidige database: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Opmerkingen

Elastische query een uitbreiding voor de syntaxis van de bestaande externe tabel voor het definiëren van externe tabellen die gebruikmaken van externe gegevensbronnen van het type RDBMS. De definitie van een externe tabel voor verticale partities bevat informatie over de volgende aspecten: 

* **Schema**: De externe DDL-tabel definieert een schema dat uw query's kunnen gebruiken. Het opgegeven in de definitie van de externe tabel schema moet overeenkomen met het schema van de tabellen in de externe database waar de werkelijke gegevens worden opgeslagen. 
* **Verwijzing naar externe database**: De externe tabel DDL verwijst naar een externe gegevensbron. De externe gegevensbron Hiermee geeft u de SQL-Database-servernaam en databasenaam op van de externe database waar de werkelijke tabelgegevens is opgeslagen. 

Met behulp van een externe gegevensbron zoals wordt beschreven in de vorige sectie, is de syntaxis voor het maken van externe tabellen als volgt: 

De component DATA_SOURCE definieert de externe gegevensbron (dat wil zeggen de externe database in het geval van verticale partitionering) die wordt gebruikt voor de externe tabel.  

De componenten SCHEMA_NAME en OBJECT_NAME bieden de mogelijkheid de definitie van de externe tabel naar een tabel in een ander schema op de externe database of naar een tabel met een andere naam, respectievelijk toewijzen. Dit is handig als u wilt definiëren van een externe tabel naar een catalogusweergave of DMV op de externe database- of een andere situatie waarin de naam van de externe tabel is al die lokaal worden uitgevoerd.  

De volgende DDL-instructie komt een bestaande definitie van de externe tabel uit de lokale catalogus. Dit heeft geen invloed op de externe database. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Machtigingen voor externe tabel maken/DROP**: EEN externe gegevensbron ALTER machtigingen nodig zijn voor de externe tabel-DDL die ook nodig is om te verwijzen naar de onderliggende gegevensbron.  

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Gebruikers met toegang tot de externe tabel krijgen automatisch toegang tot de onderliggende externe tabellen onder de referentie gegeven in de definitie van de externe gegevensbron. U moet toegang tot de externe tabel zorgvuldig beheren om te voorkomen dat ongewenste verhoging van bevoegdheden via de referentie van de externe gegevensbron. Machtigingen voor reguliere SQL kunnen worden gebruikt voor het verlenen of INTREKKEN van toegang tot een externe tabel net alsof het een gewone tabellen.  

## <a name="example-querying-vertically-partitioned-databases"></a>Voorbeeld: query's verticaal gepartitioneerd databases

De volgende query voert een koppeling drie richtingen tussen de twee lokale tabellen voor orders en regels en de externe tabel voor klanten. Dit is een voorbeeld van de referentie-use-case voor gegevens voor een elastische query: 

```sql
    SELECT      
     c_id as customer,
     c_lastname as customer_name,
     count(*) as cnt_orderline, 
     max(ol_quantity) as max_quantity,
     avg(ol_amount) as avg_amount,
     min(ol_delivery_d) as min_deliv_date
    FROM customer 
    JOIN orders 
    ON c_id = o_c_id
    JOIN  order_line 
    ON o_id = ol_o_id and o_c_id = ol_c_id
    WHERE c_id = 100
```

## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Opgeslagen procedure voor uitvoering op afstand T-SQL: sp\_execute_remote

Elastische query introduceert ook een opgeslagen procedure die rechtstreekse toegang tot de externe database biedt. De opgeslagen procedure is aangeroepen [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) en kan worden gebruikt voor het uitvoeren van extern opgeslagen procedures of T-SQL-code op de externe database. Het bevat de volgende parameters: 

* Naam van gegevensbron (nvarchar): De naam van de externe gegevensbron van het type RDBMS. 
* Query (nvarchar): De T-SQL-query moet worden uitgevoerd op de externe database. 
* Parameterdeclaratie (nvarchar) - optionele: De tekenreeks met gegevens typedefinities voor de parameters die in de queryparameter (zoals sp_executesql) gebruikt. 
* Waarde parameterlijst - optionele: Door komma's gescheiden lijst met parameterwaarden (zoals sp_executesql).

De sp\_uitvoeren\_extern maakt gebruik van de externe gegevensbron die is opgegeven in de Aanroepparameters voor het uitvoeren van de gegeven T-SQL-instructie op de externe database. De referentie van de externe gegevensbron maakt verbinding met de externe database.  

Voorbeeld: 

```sql
    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 
```

## <a name="connectivity-for-tools"></a>Connectiviteit voor hulpprogramma 's

Verbindingsreeksen voor normale SQL Server kunt u uw gegevens en BI-integratie-hulpprogramma's verbinden met databases op de SQL-database-server met elastische query's die zijn ingeschakeld en externe tabellen die zijn gedefinieerd. Zorg ervoor dat SQL Server wordt ondersteund als een gegevensbron voor het hulpprogramma. Raadpleeg vervolgens de elastische query uitvoeren op database en de externe tabellen net als elke andere SQL Server-database waarmee u verbinding met uw hulpprogramma maken wilt. 

## <a name="best-practices"></a>Aanbevolen procedures

* Zorg ervoor dat de elastische query uitvoeren op database-eindpunt toegang tot de externe database gekregen heeft door het inschakelen van toegang voor Azure-Services in de SQL-database-firewall-configuratie. Ook voor zorgen dat de referentie die is opgegeven in de definitie van de externe gegevensbron bij de externe database aanmelden kunt en de machtigingen voor toegang tot de externe tabel.  
* Elastische query's het beste werkt voor query's waar de meeste van de berekening kan worden uitgevoerd op de externe databases. Doorgaans downloadt u de beste prestaties van query's met selectieve filter predicaten die kan worden geëvalueerd op de externe databases of joins die volledig kunnen worden uitgevoerd op de externe database. Andere querypatronen moet mogelijk grote hoeveelheden gegevens van de externe database laden en slecht mag uitvoeren. 

## <a name="next-steps"></a>Volgende stappen

* Zie voor een overzicht van elastische query [elastische query overzicht](sql-database-elastic-query-overview.md).
* Zie voor een verticale partitionering zelfstudie [aan de slag met query van meerdere databases (verticale partitionering)](sql-database-elastic-query-getting-started-vertical.md).
* Zie voor een zelfstudie horizontale partitionering (sharding) [aan de slag met elastische query's voor horizontale partitionering (sharding)](sql-database-elastic-query-getting-started.md).
* Zie voor een voorbeeld en de syntaxis van query's voor horizontaal gepartitioneerde gegevens [gegevens opvragen horizontaal gepartitioneerd)](sql-database-elastic-query-horizontal-partitioning.md)
* Zie [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) voor een opgeslagen procedure die een Transact-SQL-instructie op een één externe SQL-Database van Azure of een set van databases die fungeren als shards in een horizontale partitionering wordt uitgevoerd.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
