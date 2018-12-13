---
title: Query uitvoeren op shard Azure SQL-databases | Microsoft Docs
description: Query's uitvoeren voor shards met behulp van de clientbibliotheek van elastische database.
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
ms.date: 10/05/2018
ms.openlocfilehash: bc0ca62699c21848e4d5fdc18bef292dce4634bf
ms.sourcegitcommit: b0f39746412c93a48317f985a8365743e5fe1596
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52863462"
---
# <a name="multi-shard-querying-using-elastic-database-tools"></a>Multi-shard query's uitvoeren met behulp van hulpprogramma's voor elastische databases

## <a name="overview"></a>Overzicht

Met de [hulpmiddelen voor Elastic Database](sql-database-elastic-scale-introduction.md), kunt u oplossingen voor shard-database maken. **Meerdere shards uitvoeren van query's** wordt gebruikt voor taken zoals verzameling/rapportage gegevens waarvoor een query uit te voeren die zich uitstrekt over meerdere shards. (Deze contrast [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md), die al het werk wordt uitgevoerd op een enkele shard.) 

1. Krijgen een **RangeShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._range_shard_map), [.NET](https://msdn.microsoft.com/library/azure/dn807318.aspx)) of **ListShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.map._list_shard_map), [.NET ](https://msdn.microsoft.com/library/azure/dn807370.aspx)) met behulp van de **TryGetRangeShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetrangeshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx)), wordt de **TryGetListShardMap** ([ Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.trygetlistshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx)), of de **GetShardMap** ([Java](/java/api/com.microsoft.azure.elasticdb.shard.mapmanager._shard_map_manager.getshardmap), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx)) methode. Zie **[maken van een ShardMapManager](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager)** en  **[een RangeShardMap of ListShardMap](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap)**.
2. Maak een **MultiShardConnection** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_connection), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)) object.
3. Maak een **MultiShardStatement of MultiShardCommand** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_statement), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)). 
4. Stel de **eigenschap CommandText** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_statement), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)) aan een T-SQL-opdracht.
5. Hiermee geeft u de opdracht door het aanroepen van de **ExecuteQueryAsync of ExecuteReader** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_statement.executeQueryAsync), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)) methode.
6. Bekijk de resultaten met behulp van de **MultiShardResultSet of MultiShardDataReader** ([Java](/java/api/com.microsoft.azure.elasticdb.query.multishard._multi_shard_result_set), [.NET](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)) klasse. 

## <a name="example"></a>Voorbeeld

De volgende code illustreert het gebruik van meerdere shards uitvoeren van query's met behulp van een bepaalde **ShardMap** met de naam *myShardMap*. 

```csharp
using (MultiShardConnection conn = new MultiShardConnection(myShardMap.GetShards(), myShardConnectionString)) 
{ 
    using (MultiShardCommand cmd = conn.CreateCommand())
    { 
        cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
        cmd.CommandType = CommandType.Text; 
        cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
        cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

        using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
        { 
            while (sdr.Read())
            { 
                var c1Field = sdr.GetString(0); 
                var c2Field = sdr.GetFieldValue<int>(1); 
                var c3Field = sdr.GetFieldValue<Int64>(2);
            } 
        } 
    } 
} 
```

Een belangrijk verschil is het maken van meerdere shard-verbindingen. Waar **SqlConnection** is van invloed op een individuele database, de **MultiShardConnection** duurt een ***verzameling van shards*** als invoer. Vul de verzameling van shards van een shard-toewijzing. De query wordt vervolgens uitgevoerd op de verzameling van shards met behulp van **UNION ALL** semantiek voor het samenstellen van een enkel het uiteindelijke resultaat. (Optioneel) de naam van de shard waar de rij is afkomstig uit kan worden toegevoegd aan de uitvoer met behulp van de **ExecutionOptions** eigenschap van de opdracht. 

Houd er rekening mee de aanroep van **myShardMap.GetShards()**. Deze methode haalt alle shards van de shard-toewijzing en biedt een eenvoudige manier om een query uitvoeren voor alle relevante databases. De verzameling van shards voor een query meerdere shards verfijnd worden kan verder door het uitvoeren van een LINQ-query op de verzameling geretourneerd van de aanroep aan **myShardMap.GetShards()**. In combinatie met het beleid voor gedeeltelijke resultaten, is de huidige capaciteit in meerdere shards uitvoeren van query's zijn ontworpen om goed voor tientallen tot honderden shards.

Een beperking met meerdere shards uitvoeren van query's is momenteel het ontbreken van validatie voor shards en shardlets die zijn opgevraagd. Terwijl gegevensafhankelijke routering heeft geverifieerd dat een bepaalde shard deel van de shard-toewijzing op het moment van het uitvoeren van query's uitmaakt, voer multi-shard query's niet deze controle uit. Dit kan leiden tot meerdere shard-query's die worden uitgevoerd op databases die zijn verwijderd uit de shard-toewijzing.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Multi-shard query's en bewerkingen van splitsen en samenvoegen

Multi-shard query's controleren niet of shardlets in de opgevraagde database lopende bewerkingen voor splitsen en samenvoegen deelneemt. (Zie [schalen met het Elastic Database-hulpprogramma voor splitsen en samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md).) Dit kan leiden tot inconsistenties wanneer rijen uit hetzelfde shardlet weergeven voor meerdere databases in dezelfde query meerdere shards. Houd rekening met deze beperkingen en rekening houden met verwerkingsstop van verbindingen lopende bewerkingen voor splitsen en samenvoegen en wijzigingen in de shard-toewijzing tijdens het uitvoeren van meerdere shard-query's.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]