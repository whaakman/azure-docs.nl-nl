---
title: Migreren van bestaande databases voor scale-out | Microsoft Docs
description: Shard databases voor het gebruik van hulpprogramma's voor elastische database door het maken van een shard-toewijzing manager converteren
services: sql-database
documentationcenter: 
author: ddove
manager: jhubbard
editor: 
ms.assetid: 8c851d8e-8fd5-4327-89c1-9178b20ddd69
ms.service: sql-database
ms.custom: scale out apps
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 099f40d00753b7c86ba726a818f17d440a125221
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-existing-databases-to-scale-out"></a>Bestaande databases migreren voor uitschaling
Uw bestaande uitgebreid shard databases met hulpprogramma's van Azure SQL Database database eenvoudig te beheren (zoals de [clientbibliotheek voor elastische Database](sql-database-elastic-database-client-library.md)). U moet eerst een bestaande set van databases moeten worden gebruikt converteren de [shard kaart manager](sql-database-elastic-scale-shard-map-management.md). 

## <a name="overview"></a>Overzicht
Voor het migreren van een bestaande database in de shard: 

1. Bereid de [shard-toewijzing manager-database](sql-database-elastic-scale-shard-map-management.md).
2. Maak de shard-toewijzing.
3. Bereid de afzonderlijke shards.  
4. Toewijzingen toevoegen aan de shard-toewijzing.

Deze technieken kunnen worden geïmplementeerd met behulp van de [clientbibliotheek voor .NET Framework](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/), of de PowerShell-scripts gevonden op [Azure SQL DB - elastische Database extra scripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db). De voorbeelden die hier worden de PowerShell-scripts gebruiken.

Zie voor meer informatie over de ShardMapManager [Shard kaart management](sql-database-elastic-scale-shard-map-management.md). Zie voor een overzicht van de hulpprogramma's van elastische database [elastische Database functies overzicht](sql-database-elastic-scale-introduction.md).

## <a name="prepare-the-shard-map-manager-database"></a>Voorbereiden van de manager-database van de shard-kaart
De shard-toewijzing manager is een speciale database die de gegevens voor het beheren van uitgebreide databases bevat. U kunt een bestaande database gebruiken of een nieuwe database maken. Houd er rekening mee dat een database die fungeert als de manager van shard-toewijzing niet dezelfde database als een shard moet worden. Houd er ook rekening mee dat het PowerShell-script niet de database voor u maken. 

## <a name="step-1-create-a-shard-map-manager"></a>Stap 1: Maak een shard kaart manager
    # Create a shard map manager. 
    New-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 
    #<server_name> and <smm_db_name> are the server name and database name 
    # for the new or existing database that should be used for storing 
    # tenant-database mapping information.

### <a name="to-retrieve-the-shard-map-manager"></a>Voor het ophalen van de manager shard-kaart
Na het maken, kunt u de shard-toewijzing manager met deze cmdlet ophalen. Deze stap is nodig voor elke keer dat u wilt gebruiken, het ShardMapManager-object.

    # Try to get a reference to the Shard Map Manager  
    $ShardMapManager = Get-ShardMapManager -UserName '<user_name>' 
    -Password '<password>' 
    -SqlServerName '<server_name>' 
    -SqlDatabaseName '<smm_db_name>' 


## <a name="step-2-create-the-shard-map"></a>Stap 2: Maak de shard-kaart
U moet het type van shard-toewijzing te maken. De keuze is afhankelijk van de database-architectuur: 

1. Één tenant per database (voor termen, Zie de [verklarende woordenlijst](sql-database-elastic-scale-glossary.md).) 
2. Meerdere tenants per database (twee typen):
   1. Toewijzing van de lijst
   2. Bereik toewijzing

Voor een model voor één tenant, maakt u een **lijst toewijzing** shard-toewijzing. Het model voor één tenant wijst één database per tenant. Dit is een doeltreffend model voor SaaS-ontwikkelaars aangezien vereenvoudigt het beheer.

![Toewijzing van de lijst][1]

Het model multitenant verschillende tenants toegewezen aan een individuele database (en u kunt groepen van tenants verdelen over meerdere databases). Dit model gebruiken wanneer u verwacht dat elke tenant kleine hoeveelheden gegevens nodig hebben. In dit model we een scala aan tenants toewijzen aan een database met **bereik toewijzing**. 

![Bereik toewijzing][2]

Of u kunt implementeren met een multitenant database model met een *lijst toewijzing* meerdere tenants toewijzen aan een individuele database. Bijvoorbeeld, DB1 wordt gebruikt voor het opslaan van informatie over de tenant-id 1 en 5 en DB2 slaat gegevens voor de tenant 7- en 10. 

![Muliple tenants in één DB][3] 

**Op basis van uw keuze, kies een van de volgende opties:**

### <a name="option-1-create-a-shard-map-for-a-list-mapping"></a>Optie 1: een shard-toewijzing voor de toewijzing van een lijst maken
Maakt een shard-toewijzing met behulp van het object ShardMapManager. 

    # $ShardMapManager is the shard map manager object. 
    $ShardMap = New-ListShardMap -KeyType $([int]) 
    -ListShardMapName 'ListShardMap' 
    -ShardMapManager $ShardMapManager 


### <a name="option-2-create-a-shard-map-for-a-range-mapping"></a>Optie 2: Maak een shard-toewijzing voor de toewijzing van een bereik
Houd er rekening mee dat voor het gebruik van deze toewijzing patroon, tenant-id waarden moet worden continu bereiken en het is acceptabel om onderbreking in de bereiken door gewoon het bereik wordt overgeslagen bij het maken van de databases.

    # $ShardMapManager is the shard map manager object 
    # 'RangeShardMap' is the unique identifier for the range shard map.  
    $ShardMap = New-RangeShardMap 
    -KeyType $([int]) 
    -RangeShardMapName 'RangeShardMap' 
    -ShardMapManager $ShardMapManager 

### <a name="option-3-list-mappings-on-a-single-database"></a>Optie 3: De toewijzingen van de lijst op een individuele database
Instellen van dit patroon vereist ook het maken van een kaart lijst zoals u in stap 2, optie 1.

## <a name="step-3-prepare-individual-shards"></a>Stap 3: Afzonderlijke shards voorbereiden
Elke shard (database) toevoegen aan de shard-toewijzing manager. Hiermee wordt de afzonderlijke databases voorbereid voor het opslaan van informatie over de Identiteitstoewijzing. Deze methode niet uitvoeren op elke shard.

    Add-Shard 
    -ShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>'
    # The $ShardMap is the shard map created in step 2.


## <a name="step-4-add-mappings"></a>Stap 4: Toewijzingen toevoegen
Toewijzingen toe te voegen, is afhankelijk van het soort shard-toewijzing die u hebt gemaakt. Als u een lijst kaart hebt gemaakt, kunt u de toewijzingen van de lijst toevoegen. Als u een bereik kaart hebt gemaakt, voegt u bereik toewijzingen.

### <a name="option-1-map-the-data-for-a-list-mapping"></a>Optie 1: de gegevens voor een lijst toewijzing toewijzen
De gegevens moeten worden toegewezen door de toewijzing van een lijst toe te voegen voor elke tenant.  

    # Create the mappings and associate it with the new shards 
    Add-ListMapping 
    -KeyType $([int]) 
    -ListPoint '<tenant_id>' 
    -ListShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 

### <a name="option-2-map-the-data-for-a-range-mapping"></a>Optie 2: de gegevens voor de toewijzing van een bereik toewijzen
Voeg de bereik-toewijzingen voor de tenant-id bereik met alle - database koppelingen:

    # Create the mappings and associate it with the new shards 
    Add-RangeMapping 
    -KeyType $([int]) 
    -RangeHigh '5' 
    -RangeLow '1' 
    -RangeShardMap $ShardMap 
    -SqlServerName '<shard_server_name>' 
    -SqlDatabaseName '<shard_database_name>' 


### <a name="step-4-option-3-map-the-data-for-multiple-tenants-on-a-single-database"></a>Stap 4-optie 3: de gegevens voor meerdere tenants op een individuele database toewijzen
Voer voor elke tenant de Add-ListMapping (optie 1, hierboven). 

## <a name="checking-the-mappings"></a>De toewijzingen controleren
Informatie over de bestaande shards en de bijbehorende toewijzingen kan worden opgevraagd met volgende opdrachten:  

    # List the shards and mappings 
    Get-Shards -ShardMap $ShardMap 
    Get-Mappings -ShardMap $ShardMap 

## <a name="summary"></a>Samenvatting
Nadat u de installatie hebt voltooid, kunt u beginnen met het gebruik van de clientbibliotheek voor elastische Database. U kunt ook [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md) en [query meerdere shard](sql-database-elastic-scale-multishard-querying.md).

## <a name="next-steps"></a>Volgende stappen
Ophalen van de PowerShell-scripts uit [Azure SQL DB elastische Database tools sripts](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

De hulpprogramma's zijn ook op GitHub: [/elastische-db-hulpprogramma's van Azure](https://github.com/Azure/elastic-db-tools).

Het samenvoegen van gesplitste-hulpprogramma gebruiken om gegevens te verplaatsen naar of van een multi-tenant model naar een model voor één tenant. Zie [gesplitste merge tool](sql-database-elastic-scale-get-started.md).

## <a name="additional-resources"></a>Aanvullende bronnen
Zie voor informatie over algemene gegevensarchitectuurpatronen van multitenant software as a service (SaaS)-databasetoepassingen, [Ontwerppatronen voor multitenant SaaS-toepassingen met Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md).

## <a name="questions-and-feature-requests"></a>Vragen en Functieaanvragen
Voor vragen kunt contact met ons op de [SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) en voor functieaanvragen, deze toe te voegen aan de [forum met feedback van SQL-Database](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-convert-to-use-elastic-tools/listmapping.png
[2]: ./media/sql-database-elastic-convert-to-use-elastic-tools/rangemapping.png
[3]: ./media/sql-database-elastic-convert-to-use-elastic-tools/multipleonsingledb.png

