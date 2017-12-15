---
title: De query via cloud-databases met een ander schema | Microsoft Docs
description: het instellen van meerdere databases query's via verticale partities
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: 84c261f2-9edc-42f4-988c-cf2f251f5eff
ms.service: sql-database
ms.custom: scale out apps
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/12/2017
ms.author: mlandzic
ms.openlocfilehash: f3bf919aa4aab8d37a5a97b90138b1f5434eb6ea
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="query-across-cloud-databases-with-different-schemas-preview"></a>Query voor cloud-databases met verschillende schema's (preview)
![Query tussen tabellen in verschillende databases][1]

Databases verticaal gepartitioneerd Gebruik verschillende sets van tabellen op verschillende databases. Dat betekent dat het schema van andere databases verschilt. Alle tabellen voor inventarisatie zijn bijvoorbeeld op de ene database terwijl alle accounting-gerelateerde tabellen op een tweede database zijn. 

## <a name="prerequisites"></a>Vereisten
* De gebruiker moet beschikken over een externe gegevensbron ALTER machtiging. Deze machtiging is opgenomen in de machtiging ALTER DATABASE.
* EEN externe gegevensbron ALTER machtigingen nodig om te verwijzen naar de onderliggende gegevensbron.

## <a name="overview"></a>Overzicht

> [!NOTE]
> In tegenstelling tot met horizontale partitioneren, afhankelijk deze DDL-componenten niet van het definiëren van een gegevenslaag met een shard-toewijzing via de clientbibliotheek voor elastische database.
>

1. [CREATE MASTER KEY](https://msdn.microsoft.com/library/ms174382.aspx)
2. [MAKEN VAN DATABASE-SCOPED REFERENTIE](https://msdn.microsoft.com/library/mt270260.aspx)
3. [EXTERNE GEGEVENSBRON MAKEN](https://msdn.microsoft.com/library/dn935022.aspx)
4. [CREATE EXTERNAL TABLE](https://msdn.microsoft.com/library/dn935021.aspx) 

## <a name="create-database-scoped-master-key-and-credentials"></a>Bereik databasehoofdsleutel en referenties maken
De referentie wordt gebruikt door de elastische query verbinding maken met uw externe databases.  

    CREATE MASTER KEY ENCRYPTION BY PASSWORD = 'master_key_password';
    CREATE DATABASE SCOPED CREDENTIAL <credential_name>  WITH IDENTITY = '<username>',  
    SECRET = '<password>'
    [;]

> [!NOTE]
> Zorg ervoor dat de `<username>` omvat geen **'@servername'** achtervoegsel. 
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
Het volgende voorbeeld ziet u het gebruik van de instructie CREATE voor externe gegevensbronnen. 

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

Het volgende voorbeeld ziet u hoe de lijst met externe tabellen ophalen uit de huidige database: 

    select * from sys.external_tables; 

### <a name="remarks"></a>Opmerkingen
Elastische query breidt de syntaxis van de bestaande externe tabel om te definiëren van de externe tabellen die gebruikmaken van externe gegevensbronnen van het type RDBMS. De definitie van een externe tabel voor de verticale partities worden de volgende aspecten: 

* **Schema**: de externe DDL-tabel definieert een schema dat uw query's kunnen gebruiken. Het opgegeven in de definitie van de externe tabel schema moet overeenkomen met het schema van de tabellen in de externe database waarin de werkelijke gegevens worden opgeslagen. 
* **Verwijzing naar de externe database**: de externe tabel DDL naar een externe gegevensbron verwijst. De externe gegevensbron geeft de naam van de logische server en de databasenaam op van de externe database waarin de werkelijke tabelgegevens is opgeslagen. 

Met een externe gegevensbron, zoals wordt beschreven in de vorige sectie, is de syntaxis voor het maken van externe tabellen als volgt: 

De component DATA_SOURCE definieert de externe gegevensbron (dat wil zeggen de externe database in geval van een verticale partities) die wordt gebruikt voor de externe tabel.  

De componenten SCHEMA_NAME en OBJECT_NAME bieden de mogelijkheid de definitie van de externe tabel naar een tabel in een ander schema op de externe database of aan een tabel met een andere naam respectievelijk toewijzen. Dit is handig als u wilt definiëren van een externe tabel een catalogusweergave of DMV op de externe database- of een andere situatie waarin de naam van de externe tabel is al in gebruik lokaal.  

De definitie van een bestaande externe tabel verwijderd de volgende DDL-instructie van de lokale catalogus. Dit heeft geen gevolgen voor de externe database. 

    DROP EXTERNAL TABLE [ [ schema_name ] . | schema_name. ] table_name[;]  

**Machtigingen voor de externe tabel CREATE/DROP**: een externe gegevensbron ALTER machtigingen nodig voor de externe tabel DDL die ook nodig is om te verwijzen naar de onderliggende gegevensbron.  

## <a name="security-considerations"></a>Beveiligingsoverwegingen
Gebruikers met toegang tot de externe tabel krijgen automatisch toegang tot de onderliggende externe tabellen onder de referentie die is opgegeven in de definitie van de externe gegevensbron. U moet zorgvuldig toegang beheren tot de externe tabel om te voorkomen dat onbevoegde verhoging van bevoegdheden via de referenties van de externe gegevensbron. Reguliere SQL machtigingen kunnen worden gebruikt om toegang te verlenen of INTREKKEN van toegang tot een externe tabel net alsof het een gewone tabellen.  

## <a name="example-querying-vertically-partitioned-databases"></a>Voorbeeld: verticaal opvragen gepartitioneerd databases
De volgende query voert een koppeling drie richtingen tussen de twee lokale tabellen voor orders en regels en de externe tabel voor klanten. Dit is een voorbeeld van de verwijzing naar gegevens gebruiksvoorbeeld voor elastische query: 

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


## <a name="stored-procedure-for-remote-t-sql-execution-spexecuteremote"></a>Opgeslagen procedure voor uitvoering op afstand T-SQL: sp\_execute_remote
Elastische query bevat ook een opgeslagen procedure die rechtstreekse toegang tot de externe database biedt. De opgeslagen procedure is aangeroepen [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) en kan worden gebruikt voor het uitvoeren van extern opgeslagen procedures of T-SQL-code op de externe database. Het bevat de volgende parameters: 

* Naam van de gegevensbron (nvarchar): de naam van de externe gegevensbron van het type RDBMS. 
* Query (nvarchar): de T-SQL-query moet worden uitgevoerd op de externe database. 
* Parameterdeclaratie (nvarchar) - optioneel: de tekenreeks met typedefinities gegevens voor de parameters in de Query-parameter (zoals sp_executesql) gebruikt. 
* Waarde parameterlijst - optioneel: door komma's gescheiden lijst met parameterwaarden (zoals sp_executesql).

De sp\_uitvoeren\_afstand maakt gebruik van de externe gegevensbron opgegeven in de Aanroepparameters voor het uitvoeren van de gegeven T-SQL-instructie op de externe database. Gebruikt de referenties van de externe gegevensbron verbinding maken met de externe database.  

Voorbeeld: 

    EXEC sp_execute_remote
        N'MyExtSrc',
        N'select count(w_id) as foo from warehouse' 



## <a name="connectivity-for-tools"></a>Connectiviteit voor hulpprogramma 's
Reguliere verbindingsreeksen voor SQL Server kunt u uw integratie BI en gegevens extra verbinding te maken met databases op de SQL-database-server met elastische query ingeschakeld en externe tabellen die zijn gedefinieerd. Zorg ervoor dat SQL Server wordt ondersteund als een gegevensbron voor het hulpprogramma. Raadpleeg de elastische database en de externe tabellen net als elke andere SQL Server-database die u verbinding met het hulpprogramma maken wilt. 

## <a name="best-practices"></a>Aanbevolen procedures
* Zorg ervoor dat de elastische database eindpunt toegang tot de externe database gekregen heeft door het inschakelen van toegang voor Azure-Services in de firewall-configuratie van SQL-database. Controleer ook of de referentie die is opgegeven in de definitie van de externe gegevensbron bij de externe database aanmelden kunt en de machtigingen heeft voor toegang tot de externe tabel.  
* Elastische query werkt het beste voor query's waar de meeste van de berekening kan worden uitgevoerd op de externe databases. U doorgaans ophalen de beste queryprestaties met selectief filter predicaten die kan worden geëvalueerd op de externe databases of joins die volledig kunnen worden uitgevoerd op de externe database. Andere querypatronen moet mogelijk grote hoeveelheden gegevens uit de externe database laden en slecht mag uitvoeren. 

## <a name="next-steps"></a>Volgende stappen

* Zie voor een overzicht van elastische query [elastische query overzicht](sql-database-elastic-query-overview.md).
* Zie voor een verticale partitie zelfstudie [aan de slag met cross-databasequery (verticale partities)](sql-database-elastic-query-getting-started-vertical.md).
* Zie voor een zelfstudie horizontaal partitioneren (sharding) [aan de slag met elastische query voor horizontale partitioneren (sharding)](sql-database-elastic-query-getting-started.md).
* Zie voor de syntaxis en voorbeelden query's voor horizontaal gepartitioneerde gegevens [gegevens opvragen horizontaal gepartitioneerd)](sql-database-elastic-query-horizontal-partitioning.md)
* Zie [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) voor een opgeslagen procedure die een Transact-SQL-instructie op een enkele externe Azure SQL Database of een set van databases die fungeren als shards in een horizontale partitieschema wordt uitgevoerd.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-vertical-partitioning/verticalpartitioning.png


<!--anchors-->
