---
title: Rapport over uitgeschaalde clouddatabases (horizontale partitionering) | Microsoft Docs
description: Databaseoverschrijdende database query's naar rapport gebruiken voor meerdere databases.
services: sql-database
ms.service: sql-database
subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: MladjoA
ms.author: mlandzic
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: f6e9c36634435c759004e6102148a072bbdba94a
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47166687"
---
# <a name="report-across-scaled-out-cloud-databases-preview"></a>Rapportage voor meerdere uitgeschaalde clouddatabases (preview)
U kunt rapporten maken van meerdere Azure SQL-databases vanuit een enkele verbinding punt met een [elastische query](sql-database-elastic-query-overview.md). De databases worden horizontaal gepartitioneerd (ook bekend als 'shard').

Als u een bestaande database hebt, raadpleegt u [bestaande databases migreren naar uitgeschaalde databases](sql-database-elastic-convert-to-use-elastic-tools.md).

Zie voor meer informatie over de SQL-objecten die nodig zijn om op te vragen, [Query met een horizontaal gepartitioneerde databases](sql-database-elastic-query-horizontal-partitioning.md).

## <a name="prerequisites"></a>Vereisten
Downloaden en uitvoeren van de [aan de slag met hulpprogramma's elastische Database voor klantorders](sql-database-elastic-scale-get-started.md).

## <a name="create-a-shard-map-manager-using-the-sample-app"></a>Een shard-Toewijzingsbeheer met behulp van de voorbeeld-app maken
Hier maakt u een shard-toewijzing manager samen met verschillende shards, gevolgd door het invoegen van gegevens in de shards. Als u al shards installatie met behulp van gedeelde gegevens erin hebt, kunt u de volgende stappen overslaan en naar de volgende sectie gaan.

1. Bouwen en uit de **aan de slag met elastische Databasehulpprogramma's** voorbeeldtoepassing. Volg de stappen tot stap 7 in de sectie [downloaden en uitvoeren van de voorbeeld-app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app). Aan het einde van stap 7 ziet u de volgende opdrachtprompt:

    ![opdrachtprompt][1]
2. Typ in het opdrachtvenster "1" en druk op **Enter**. Dit maakt de shard-Toewijzingsbeheer en twee shards toevoegen aan de server. Vervolgens typt u '3' en druk op **Enter**; de actie vier keer herhalen. Dit voorbeeldrijen met gegevens in uw shards ingevoegd.
3. De [Azure-portal](https://portal.azure.com) drie nieuwe databases in de server moet worden weergegeven:

   ![Visual Studio-bevestiging][2]

   Query's die databaseoverschrijdend zijn op dit moment ondersteund via de Elastic Database-clientbibliotheken. Gebruik bijvoorbeeld optie 4 in het opdrachtvenster. De resultaten van een query meerdere shards zijn altijd een **UNION ALL** van de resultaten van alle shards.

   In de volgende sectie maken we een voorbeeld van database-eindpunt die ondersteuning biedt voor uitgebreidere opvragen van de gegevens van meerdere shards.

## <a name="create-an-elastic-query-database"></a>Een query voor elastic database maken
1. Open de [Azure-portal](https://portal.azure.com) en zich aanmeldt.
2. Maak een nieuwe Azure SQL-database op dezelfde server als de shard-configuratie. Naam van de database "ElasticDBQuery."

    ![Azure portal en de prijscategorie][3]

    > [!NOTE]
    > u kunt een bestaande database gebruiken. Als u dit doen kunt, moet het niet een van de shards die u uw query's uitvoeren wilt op zijn. Deze database wordt gebruikt voor het maken van de van metagegevensobjecten voor een elastische database-query.
    >

## <a name="create-database-objects"></a>Database-objecten maken
### <a name="database-scoped-master-key-and-credentials"></a>Hoofdsleutel databasebereik en referenties
Deze worden gebruikt verbinding maken met de shard-Toewijzingsbeheer en de shards:

1. Open SQL Server Management Studio of SQL Server Data Tools in Visual Studio.
2. Verbinding maken met database ElasticDBQuery en voer de volgende T-SQL-opdrachten uit:

        CREATE MASTER KEY ENCRYPTION BY PASSWORD = '<password>';

        CREATE DATABASE SCOPED CREDENTIAL ElasticDBQueryCred
        WITH IDENTITY = '<username>',
        SECRET = '<password>';

    notatie 'gebruikersnaam' en 'wachtwoord' moeten gelijk zijn aan de aanmeldingsgegevens die zijn gebruikt in stap 6 van [downloaden en uitvoeren van de voorbeeld-app](sql-database-elastic-scale-get-started.md#download-and-run-the-sample-app) in [aan de slag met elastische Databasehulpprogramma's](sql-database-elastic-scale-get-started.md).

### <a name="external-data-sources"></a>Externe gegevensbronnen
Voor het maken van een externe gegevensbron dan de volgende opdracht op de database ElasticDBQuery:

    CREATE EXTERNAL DATA SOURCE MyElasticDBQueryDataSrc WITH
      (TYPE = SHARD_MAP_MANAGER,
      LOCATION = '<server_name>.database.windows.net',
      DATABASE_NAME = 'ElasticScaleStarterKit_ShardMapManagerDb',
      CREDENTIAL = ElasticDBQueryCred,
       SHARD_MAP_NAME = 'CustomerIDShardMap'
    ) ;

 "CustomerIDShardMap" is de naam van de shard-toewijzing, als u de shard-toewijzing en de shard-Toewijzingsbeheer met behulp van het voorbeeld van de hulpprogramma's elastische database hebt gemaakt. Echter, als u uw aangepaste instellingen voor dit voorbeeld gebruikt, klikt u vervolgens het moet de naam van de shard-toewijzing die u hebt gekozen in uw toepassing.

### <a name="external-tables"></a>Externe tabellen
Maak een externe tabel die overeenkomt met de tabel klanten over de shards door het uitvoeren van de volgende opdracht op ElasticDBQuery database:

    CREATE EXTERNAL TABLE [dbo].[Customers]
    ( [CustomerId] [int] NOT NULL,
      [Name] [nvarchar](256) NOT NULL,
      [RegionId] [int] NOT NULL)
    WITH
    ( DATA_SOURCE = MyElasticDBQueryDataSrc,
      DISTRIBUTION = SHARDED([CustomerId])
    ) ;

## <a name="execute-a-sample-elastic-database-t-sql-query"></a>Een voorbeeld elastische database T-SQL-query uitvoeren
Nadat u uw externe gegevensbron en uw externe tabellen die u kunt nu volledige T-SQL gebruiken via uw externe tabellen hebt gedefinieerd.

Voer deze query uit op de database ElasticDBQuery:

    select count(CustomerId) from [dbo].[Customers]

U ziet dat de query cumuleert de resultaten van alle shards en de volgende uitvoer biedt:

![Uitvoergegevens][4]

## <a name="import-elastic-database-query-results-to-excel"></a>Queryresultaten voor elastische database importeren in Excel
 U kunt de resultaten van een query naar een Excel-bestand importeren.

1. Start Excel 2013.
2. Navigeer naar de **gegevens** lint.
3. Klik op **van andere bronnen** en klikt u op **van SQL Server**.

   ![Excel importeren uit andere bronnen][5]
4. In de **Wizard Gegevensverbinding** typt u de referenties voor de servernaam en aanmeldingsgegevens. Klik op **Volgende**.
5. In het dialoogvenster **selecteert u de database met de gegevens die u wilt dat**, selecteer de **ElasticDBQuery** database.
6. Selecteer de **klanten** tabel in de lijstweergave en klikt u op **volgende**. Klik vervolgens op **voltooien**.
7. In de **importgegevens** formulier, onder **selecteert u hoe u wilt weergeven van deze gegevens in uw werkmap**, selecteer **tabel** en klikt u op **OK**.

Alle rijen uit **klanten** tabel, die zijn opgeslagen in verschillende shards de Excel-werkblad vullen.

U kunt nu Excel van krachtige visualisatie-functies gebruiken. U kunt de verbindingsreeks met de servernaam, databasenaam en referenties verbinding maken met uw gegevens en BI-integratie-hulpprogramma's van de query's in elastic database. Zorg ervoor dat SQL Server wordt ondersteund als een gegevensbron voor het hulpprogramma. U kunt verwijzen naar de query's in elastic database en de externe tabellen net als elke andere SQL Server-database en SQL Server-tabellen waarmee u verbinding met uw hulpprogramma maken wilt.

### <a name="cost"></a>Kosten
Er is geen extra kosten in rekening gebracht voor het gebruik van de Query voor Elastic Database-functie.

Zie voor informatie over de prijzen [prijsinformatie voor SQL Database](https://azure.microsoft.com/pricing/details/sql-database/).

## <a name="next-steps"></a>Volgende stappen

* Zie voor een overzicht van elastische query [elastische query overzicht](sql-database-elastic-query-overview.md).
* Zie voor een verticale partitionering zelfstudie [aan de slag met query van meerdere databases (verticale partitionering)](sql-database-elastic-query-getting-started-vertical.md).
* Zie voor een voorbeeld en de syntaxis van query's voor verticaal gepartitioneerde gegevens [opvragen van verticaal gepartitioneerde gegevens)](sql-database-elastic-query-vertical-partitioning.md)
* Zie voor een voorbeeld en de syntaxis van query's voor horizontaal gepartitioneerde gegevens [gegevens opvragen horizontaal gepartitioneerd)](sql-database-elastic-query-horizontal-partitioning.md)
* Zie [sp\_uitvoeren \_externe](https://msdn.microsoft.com/library/mt703714) voor een opgeslagen procedure die een Transact-SQL-instructie op een één externe SQL-Database van Azure of een set van databases die fungeren als shards in een horizontale partitionering wordt uitgevoerd.


<!--Image references-->
[1]: ./media/sql-database-elastic-query-getting-started/cmd-prompt.png
[2]: ./media/sql-database-elastic-query-getting-started/portal.png
[3]: ./media/sql-database-elastic-query-getting-started/tiers.png
[4]: ./media/sql-database-elastic-query-getting-started/details.png
[5]: ./media/sql-database-elastic-query-getting-started/exel-sources.png
<!--anchors-->
