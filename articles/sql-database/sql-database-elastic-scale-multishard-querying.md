---
title: Query uitvoeren op Azure SQL-databases shard | Microsoft Docs
description: Query's uitvoeren via shards met behulp van de clientbibliotheek voor elastische database.
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: a4379c15-f213-4026-ab6f-a450ee9d5758
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2016
ms.author: torsteng
ms.openlocfilehash: 67bcb3c7fe33341103f28bc70e8cc2acbb924cae
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="multi-shard-querying"></a>Meerdere shard opvragen
## <a name="overview"></a>Overzicht
Met de [hulpmiddelen voor elastische databases](sql-database-elastic-scale-introduction.md), u kunt shard databaseoplossingen maken. **Uitvoeren van query's met meerdere shard** wordt gebruikt voor taken zoals verzameling/rapportage gegevens waarvoor het uitvoeren van een query die zich uitstrekt over meerdere shards. (Deze optie om te vergelijken [gegevensafhankelijke routering](sql-database-elastic-scale-data-dependent-routing.md), die al het werk wordt uitgevoerd op een enkele shard.) 

1. Ophalen van een [ **RangeShardMap** ](https://msdn.microsoft.com/library/azure/dn807318.aspx) of [ **ListShardMap** ](https://msdn.microsoft.com/library/azure/dn807370.aspx) met behulp van de [ **TryGetRangeShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), wordt de [ **TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), of de [ **GetShardMap** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) methode. Zie [ **samenstellen van een ShardMapManager** ](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) en [ **ophalen van een RangeShardMap of ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Maak een  **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)**  object.
3. Maak een  **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
4. Stel de  **[eigenschap CommandText](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)**  aan een T-SQL-opdracht.
5. Voer de opdracht door het aanroepen van de  **[ExecuteReader methode](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
6. Bekijk de resultaten met behulp van de  **[MultiShardDataReader klasse](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Voorbeeld
De volgende code ziet u het gebruik van meerdere shard uitvoeren van query's met behulp van een bepaalde **ShardMap** met de naam *myShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
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


Een belangrijk verschil is het maken van meerdere shard-verbindingen. Waar **SqlConnection** is van invloed op een individuele database de **MultiShardConnection** duurt een ***verzameling shards*** als invoer. Vul de verzameling van shards uit een shard-toewijzing. De query wordt vervolgens uitgevoerd op de verzameling van shards met **UNION ALL** semantiek voor het samenstellen van een enkele algemene resultaat. U kunt desgewenst de naam van de shard waar de rij is afkomstig uit kan worden toegevoegd aan de uitvoer met behulp de **ExecutionOptions** eigenschap op de opdracht. 

Let op de aanroep van **myShardMap.GetShards()**. Deze methode haalt alle shards uit de shard-toewijzing en een eenvoudige manier om een query uitvoeren voor alle relevante databases. De verzameling van shards voor een query meerdere shard kan verfijnd verder door het uitvoeren van een LINQ-query via de verzameling geretourneerd van de aanroep aan **myShardMap.GetShards()**. In combinatie met het beleid gedeeltelijke resultaten, is de huidige mogelijkheden in meerdere shard-query ontworpen om te werken goed bij tientallen tot honderden shards.

Een beperking voor het uitvoeren van query's met meerdere shard is momenteel het gebrek aan validatie voor shards en shardlets die worden opgevraagd. Terwijl gegevensafhankelijke routering, bevestigt dat een bepaalde shard deel uitmaakt van de shard-toewijzing op het moment van query's op, Voer meerdere shard-query's niet deze controle uit. Dit kan leiden tot meerdere shard-query uitvoeren op databases die zijn verwijderd uit de shard-toewijzing.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Meerdere shard-query's en gesplitste samenvoegbewerkingen
Meerdere shard-query's controleren niet of shardlets op de aangevraagde database lopende gesplitste merge-bewerkingen deelnemen. (Zie [schalen met het hulpprogramma voor elastische Database gesplitste samenvoegen](sql-database-elastic-scale-overview-split-and-merge.md).) Dit kan leiden tot inconsistenties waar rijen uit de dezelfde shardlet voor meerdere databases in dezelfde query meerdere shard weergeven. Hoogte zijn van deze beperkingen en overweeg verwerkingsstop lopende gesplitste merge-bewerkingen en wijzigingen in de shard-toewijzing tijdens het uitvoeren van query's met meerdere shard.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Zie ook
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)**  klassen en methoden.

Beheren van shards met behulp van de [clientbibliotheek voor elastische Database](sql-database-elastic-database-client-library.md). Bevat een naamruimte aangeroepen [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) die biedt de mogelijkheid om op te vragen van meerdere shards met een enkele query en het resultaat. Biedt een query uitvoert abstractie via een verzameling met shards. Het bevat ook alternatieve uitvoeringsbeleidsregels, met name gedeeltelijke resultaten om te gaan met fouten tijdens het opvragen via veel shards.  

