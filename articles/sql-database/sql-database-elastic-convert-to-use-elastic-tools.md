---
title: Bestaande databases migreren voor uitschalen | Microsoft Docs
description: Shard-databases voor het gebruik van hulpmiddelen voor elastic database door het maken van een shard-Toewijzingsbeheer converteren
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 04/01/2018
ms.openlocfilehash: 03e1974a91a8c3cceacab777e28e8e4a01ccb313
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51251590"
---
# <a name="migrate-existing-databases-to-scale-out"></a>Bestaande databases migreren voor uitschalen
Uw bestaande uitgeschaalde shard databases met behulp van Azure SQL Database-hulpprogramma's voor databases eenvoudig te beheren (zoals de [Elastic Database-clientbibliotheek](sql-database-elastic-database-client-library.md)). Converteert u eerst een bestaande set met databases te gebruiken de [shard-Toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md). 

## <a name="overview"></a>Overzicht
Voor het migreren van een bestaande shard-database: 

1. Bereid de [database voor shard-Toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md).
2. Maak de shard-toewijzing.
3. Bereid de afzonderlijke shards.  
4. Toewijzingen toevoegen aan de shard-toewijzing.

Deze technieken kunnen worden geïmplementeerd met behulp van de [.NET Framework-clientbibliotheek](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), of de PowerShell-scripts gevonden op [Azure SQL DB - scripts voor Elastic Database-hulpprogramma's](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db). Deze voorbeelden gebruiken de PowerShell-scripts.

Zie voor meer informatie over de ShardMapManager [Shard-Toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md). Zie voor een overzicht van de hulpprogramma's voor elastische databases, [Functieoverzicht Elastic Database](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>Voorbereiden van de database voor shard-Toewijzingsbeheer
De shard-Toewijzingsbeheer is een speciale database met de gegevens voor het beheren van uitgeschaalde databases. U kunt een bestaande database gebruiken of een nieuwe database maken. Een database die fungeert als de shard-Toewijzingsbeheer mag niet dezelfde database als een shard zijn. Het PowerShell-script heeft de database niet maken voor u. 

## <a name="step-1-create-a-shard-map-manager"></a>Stap 1: Maak een shard-Toewijzingsbeheer
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>Om op te halen van de shard-Toewijzingsbeheer
Na het maken, kunt u de shard-Toewijzingsbeheer met deze cmdlet ophalen. Deze stap is nodig wanneer u moet het object ShardMapManager gebruiken.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-the-shard-map"></a>Stap 2: Maak de shard-toewijzing
Selecteer het type van de shard-toewijzing te maken. De keuze is afhankelijk van de database-architectuur: 

1. Met één tenant per database (voor voorwaarden, Zie de [verklarende woordenlijst](sql-database-elastic-scale-glossary.md).) 
2. Meerdere tenants per database (twee typen):
   1. Toewijzing van de lijst
   2. Toewijzing van bereik

Voor een model met één tenant, maakt u een **lijst toewijzing** shard-toewijzing. Het model met één tenant wordt één database per tenant toegewezen. Dit is een doeltreffend model voor SaaS-ontwikkelaars omdat dit vereenvoudigt het beheer.

![Toewijzing van de lijst][1]

Het model met meerdere tenants verschillende tenants toegewezen aan een individuele database (en u kunt groepen van tenants verdelen over meerdere databases). Dit model gebruiken wanneer u verwacht elke tenant dat hebben kleine Gegevensbehoeften. In dit model kunt u een bereik van tenants toewijzen voor het gebruik van een database **bereik toewijzing**. 

![Toewijzing van bereik][2]

Of u kunt implementeren met een multitenant-database-model met een *lijst toewijzing* meerdere tenants toewijzen aan een individuele database. Bijvoorbeeld, DB1 wordt gebruikt voor het opslaan van informatie over de tenant-ID 1 en 5 en DB2 slaat gegevens voor de tenant 7- en 10. 

![Meerdere tenants in één DB][3] 

**Op basis van uw keuze, kies een van de volgende opties:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Optie 1: een shard-toewijzing voor de toewijzing van een lijst maken
Een shard-toewijzing met behulp van het object ShardMapManager maken. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Optie 2: een shard-toewijzing voor de toewijzing van een bereik maken
Gebruikmaken van dit patroon toewijzing, tenant-id-waarden moet continue bereiken, en het is acceptabel om lege ruimte in de bereiken door het bereik wordt overgeslagen bij het maken van de databases.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Optie 3: Lijst met toewijzingen voor een individuele database
Instellen van dit patroon is het maken van een kaart lijst ook vereist zoals wordt weergegeven in stap 2, optie 1.

## <a name="step-3-prepare-individual-shards"></a>Stap 3: Voorbereiden afzonderlijke shards
Elke shard (database) toevoegen aan de shard-toewijzing. De afzonderlijke databases nu voorbereid voor het opslaan van informatie over de Identiteitstoewijzing. Deze methode niet uitvoeren op elke shard.

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.


## <a name="step-4-add-mappings"></a>Stap 4: Toewijzingen toevoegen
Het toevoegen van toewijzingen, is afhankelijk van het soort shard-toewijzing die u hebt gemaakt. Als u een lijst-toewijzing gemaakt, voegt u de lijst met toewijzingen. Als u een bereik-toewijzing gemaakt, kunt u bereik toewijzingen toevoegen.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Optie 1: de gegevens voor de toewijzing van een lijst met toewijzen
De gegevens worden toegewezen door de toewijzing van een lijst toe te voegen voor elke tenant.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Optie 2: de gegevens voor de toewijzing van een bereik toewijzen
Voeg de bereik-toewijzingen voor de tenant-ID bereik met alle - database-koppelingen:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Stap 4-optie 3: de gegevens voor meerdere tenants in een individuele database toewijzen
Voor elke tenant, uitvoeren van de Add-ListMapping (optie 1). 

## <a name="checking-the-mappings"></a>De toewijzingen controleren
Informatie over de bestaande shards en de bijbehorende toewijzingen kan worden opgevraagd met de volgende opdrachten:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Samenvatting
Nadat u de installatie hebt voltooid, kunt u beginnen met het gebruik van de clientbibliotheek voor Elastic Database. U kunt ook [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) en [multi-shard query](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Volgende stappen
Ophalen van de PowerShell-scripts uit [Azure SQL DB Elastic Database extra scripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

De hulpprogramma's zijn ook op GitHub: [Azure/elastische-db-hulpprogramma's](https://github.com/Azure/elastic-db-tools).

Gebruik het hulpprogramma voor splitsen en samenvoegen om gegevens te verplaatsen naar of van een multi-tenant model naar een model met één tenant. Zie [hulpprogramma voor splitsen en samenvoegen](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Aanvullende resources
Zie voor informatie over algemene gegevensarchitectuurpatronen van multitenant software as a service (SaaS)-databasetoepassingen, [Ontwerppatronen voor multitenant SaaS-toepassingen met Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Vragen en Functieaanvragen
Voor vragen, gebruikt u de [forum van SQL-Database](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) en voor functieaanvragen, toe te voegen aan de [forum met feedback van SQL-Database](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

