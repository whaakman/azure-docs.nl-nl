---
title: Rapport over cloud uitgebreide databases (horizontale partitionering) | Microsoft Docs
description: Gebruik cross database databasequery's om te rapporteren over meerdere databases.
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: c81ef5e3-41e9-4fd2-8631-868f2e168147
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/23/2016
ms.author: mlandzic
ms.openlocfilehash: 996ad1d47ece592dcf03a6eb8ed1c1916ceba374
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="report-across-scaled-out-cloud-databases-preview"></a>Rapport over cloud uitgebreide databases (preview)
U kunt rapporten maken uit meerdere Azure SQL-databases vanuit een enkele verbinding punt met een [elastische query](sql-database-elastic-query-overview.md). De databases worden horizontaal gepartitioneerd (ook wel bekend als 'shard').

Als u een bestaande database hebt, raadpleegt u [migreren van bestaande databases met uitgebreide databases](sql-database-elastic-convert-to-use-elastic-tools.md).

Zie voor informatie over de SQL-objecten die nodig zijn om op te vragen, [Query over horizontaal gepartitioneerde databases](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="prerequisites"></a>Vereisten
Downloaden en uitvoeren van de [aan de slag met elastische Database extra voorbeeld](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Een shard-toewijzing manager met behulp van de voorbeeld-app maken
Hier maakt u een shard-toewijzing manager samen met enkele shards, gevolgd door het invoegen van gegevens in de shards. Als u al hebben shards setup met gedeelde gegevens bevat, kunt u de volgende stappen overslaan en naar de volgende sectie gaan.

1. Bouwen en uitvoeren van de **aan de slag met hulpprogramma's voor elastische Database** voorbeeldtoepassing. Volg de stappen totdat stap 7 in de sectie [downloaden en uitvoeren van de voorbeeld-app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). Aan het einde van stap 7 ziet u de volgende opdrachtprompt:

    ![opdrachtprompt][1]
2. Typ in het opdrachtvenster '1' en druk op **Enter**. Dit maakt de shard kaart manager en voegt twee shards aan de server. Vervolgens typt u "3" en druk op **Enter**; de actie vier keer herhalen. Dit invoegen voorbeeld gegevensrijen in uw shards.
3. De [Azure-portal](https://portal.azure.com) moet drie nieuwe databases worden weergegeven in de server:

   ![Visual Studio-bevestiging][2]

   Cross-databasequery's worden op dit moment ondersteund via de clientbibliotheken elastische Database. Gebruik bijvoorbeeld de optie 4 in het opdrachtvenster. De resultaten van een query meerdere shard zijn altijd een **UNION ALL** van de resultaten van alle shards.

   In de volgende sectie maken we een voorbeeld database eindpunt die ondersteuning biedt voor uitgebreide query van de gegevens over shards.

## <a name="create-an-elastic-query-database"></a>Een voor elastische querydatabase maken
1. Open de [Azure-portal](https://portal.azure.com) en zich aanmelden.
2. Maak een nieuwe Azure SQL-database op dezelfde server als de shard-instellingen. Naam van de database 'ElasticDBQuery'.

    ![Azure-portal en de prijscategorie][3]

    > [!NOTE]
    > u kunt een bestaande database gebruiken. Als u dit doen kunt, moet dit niet een van de shards die u wilt uw query's uitvoeren. Deze database wordt gebruikt voor het maken van de van metagegevensobjecten voor een elastische database-query.
    >

## <a name="create-database-objects"></a>Database-objecten maken
### <a name="database-scoped-master-key-and-credentials"></a>Database-scoped hoofdsleutel en referenties
Deze worden gebruikt voor het verbinding maken met de shard-toewijzing manager en de shards:

1. Open SQL Server Management Studio of SQL Server Data Tools in Visual Studio.
2. Verbinding maken met ElasticDBQuery database en voer de volgende T-SQL-opdrachten:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    'gebruikersnaam' en 'password' moeten hetzelfde zijn als de aanmeldingsgegevens die zijn gebruikt in stap 6 van [downloaden en uitvoeren van de voorbeeld-app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) in [aan de slag met hulpprogramma's voor elastische database](sql-database-elastic-scale-get-started.md).

### <a name="external-data-sources"></a>Externe gegevensbronnen
Voer de volgende opdracht op de database ElasticDBQuery voor het maken van een externe gegevensbron:

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
       SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 'CustomerIDShardMap' is de naam van de shard-kaart, als u de shard-toewijzing en de shard-toewijzing manager met behulp van het voorbeeld van de hulpprogramma's voor elastische database gemaakt. Als u uw aangepaste instellingen voor dit voorbeeld gebruikt, klikt u vervolgens moet het echter de shard-toewijzingsnaam die u hebt gekozen in uw toepassing.

### <a name="external-tables"></a>Externe tabellen
Een externe tabel die overeenkomt met de tabel Klanten op de shards door het uitvoeren van de volgende opdracht op ElasticDBQuery database maken:

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Een voorbeeld elastische database T-SQL-query uitvoeren
U kunt nu volledige T-SQL gebruiken via uw externe tabellen, nadat u uw externe gegevensbron en uw externe tabellen hebt gedefinieerd.

Voer deze query uit op de database ElasticDBQuery:

    select count(CustomerId) from [dbo].[Customers]

U ziet dat de query cumuleert de resultaten van de shards en de volgende uitvoer biedt:

![Uitvoerdetails][4]

## <a name="import-elastic-database-query-results-to-excel"></a>De queryresultaten elastische database importeren in Excel
 U kunt de resultaten van van een query met een Excel-bestand importeren.

1. Start Excel 2013.
2. Navigeer naar de **gegevens** lint.
3. Klik op **van andere bronnen** en klik op **van SQL Server**.

   ![Excel importeren uit andere bronnen][5]
4. In de **Wizard Gegevensverbinding** Typ de servernaam en aanmeldingsreferenties van de server. Klik op **Volgende**.
5. In het dialoogvenster **selecteert u de database met de gegevens die u wilt dat**, selecteer de **ElasticDBQuery** database.
6. Selecteer de **klanten** tabel in de lijstweergave en klikt u op **volgende**. Klik vervolgens op **voltooien**.
7. In de **importgegevens** formulier onder **selecteren hoe u wilt weergeven van deze gegevens in uw werkmap**, selecteer **tabel** en klik op **OK**.

Alle rijen uit **klanten** tabel, opgeslagen in verschillende shards vullen de Excel-werkblad.

Nu kunt u krachtige gegevensfuncties visualisatie van Excel. U kunt de verbindingsreeks gebruiken met de servernaam, databasenaam en referenties verbinding maken met uw integratiefuncties BI en gegevens van de elastische database. Zorg ervoor dat SQL Server wordt ondersteund als een gegevensbron voor het hulpprogramma. U kunt verwijzen naar de elastische query uitvoeren op database en de externe tabellen net als elke andere SQL Server-database en SQL Server-tabellen die u met het hulpprogramma verbinding wilt maken.

### <a name="cost"></a>Kosten
Er zijn geen extra kosten voor het gebruik van de functie voor elastische Database-Query.

Zie voor informatie over prijzen [SQL Database Pricing Details](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Volgende stappen

* Zie voor een overzicht van elastische query [elastische query overzicht](sql-database-elastic-query-overview.md).
* Zie voor een verticale partitie zelfstudie [aan de slag met cross-databasequery (verticale partities)](sql-database-elastic-query-getting-started-vertical.md).
* Zie voor de syntaxis en voorbeelden query's voor verticaal gepartitioneerde gegevens [gegevens opvragen verticaal gepartitioneerd)](sql-database-elastic-query-vertical-partitioning.md)
* Zie voor de syntaxis en voorbeelden query's voor horizontaal gepartitioneerde gegevens [gegevens opvragen horizontaal gepartitioneerd)](sql-database-elastic-query-horizontal-partitioning.md)
* Zie [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) voor een opgeslagen procedure die een Transact-SQL-instructie op een enkele externe Azure SQL Database of een set van databases die fungeren als shards in een horizontale partitieschema wordt uitgevoerd.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
