---
title: Azure SQL elastisch schalen Veelgestelde vragen | Microsoft Docs
description: Veelgestelde vragen over Azure SQL Database elastisch schalen.
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: e60dde9c-bb7b-4f2f-b52c-bdb506d49fcb
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: f0a7b5ce61feaead608d457465f64813737fa112
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="elastic-database-tools-faq"></a>Hulpprogramma's voor elastische database Veelgestelde vragen
#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Als ik een één-tenant per shard en geen sharding-sleutel hebt, hoe ik vullen de sharding-sleutel voor de schema-informatie?
Het gegevensobject schema wordt alleen gebruikt om op te splitsen samenvoegen scenario's. Als een toepassing inherent single-tenant is, hoeft het hulpprogramma gesplitste samenvoegen niet en is dus niet nodig voor het vullen van het gegevensobject schema.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Een database hebt ingericht en ik heb al een Manager Shard-kaart, hoe kan ik deze nieuwe database registreren als een shard?
Zie  **[een shard toe te voegen aan een toepassing met de clientbibliotheek voor elastische database](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Wat elastische database extra kosten?
Met behulp van de clientbibliotheek voor elastische database worden eventuele kosten. Kosten doorlopen alleen voor de Azure SQL-databases die u gebruikt voor shards en de Shard-toewijzing Manager, evenals de web/worker rollen die u voor het hulpprogramma gesplitste samenvoegen inrichten.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Waarom wordt mijn referenties niet werkt wanneer ik een shard uit een andere server toevoegen?
Gebruik geen referenties in de vorm van ' gebruikers-ID =username@servername', in plaats daarvan gewoon gebruiken ' gebruikers-ID = gebruikersnaam '.  Zorg ook dat de aanmelding 'gebruikersnaam' machtigingen op de shard heeft.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Heb ik nodig voor het maken van een Manager Shard-toewijzing en shards vullen telkens wanneer ik mijn toepassingen starten?
Nee, het maken van de Manager Shard-toewijzing (bijvoorbeeld  **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) is een eenmalige bewerking.  Uw toepassing moet gebruiken de aanroep  **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)**  tijdens het opstarten van een toepassing.  Er mag slechts één aanroep per toepassingsdomein.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Ik heb vragen over het gebruik van hulpprogramma's voor elastische database, hoe krijg ik ze beantwoord?
Kunt contact met ons op de [-forum Azure SQL Database](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Bij het ophalen van een databaseverbinding met een sharding-sleutel, maar ik kan nog steeds gegevens opvragen voor andere sharding-sleutels op de dezelfde shard.  Dit is standaard?
De Elastic Scale API's bieden u een verbinding met de juiste database voor uw sharding-sleutel, maar bieden geen sharding sleutel filteren.  Voeg **waar** componenten toe aan de query van het bereik beperken tot de opgegeven sharding-sleutel, indien nodig.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Kan ik een andere editie van Azure-Database gebruiken voor elke shard in mijn shard-set?
Ja, een shard is een individuele database en één shard kan dus een Premium-editie terwijl een andere een Standard-editie. Bovendien kan de editie van een shard omhoog of omlaag schalen meerdere keren gedurende de levensduur van de shard.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Een database tijdens een gesplitste of merge-bewerking heeft de splitsing Merge tool inrichten (of verwijderen)?
Nee. Voor **splitsen** bewerkingen, de doeldatabase zijn met het bijbehorende schema en deze moet worden geregistreerd bij de Shard-toewijzing Manager.  Voor **samenvoegen** bewerkingen, moet u de shard verwijderen uit de shard-toewijzing manager en verwijder vervolgens de database.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

