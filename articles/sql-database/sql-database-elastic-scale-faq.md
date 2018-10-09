---
title: Azure SQL Elastic Scale Veelgestelde vragen over | Microsoft Docs
description: Veelgestelde vragen over Azure SQL Database Elastic Scale.
services: sql-database
ms.service: sql-database
subservice: elastic-scale
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 10/05/2018
ms.openlocfilehash: d4840c155439efb22cc5f068afb0635e72b97e91
ms.sourcegitcommit: 0bb8db9fe3369ee90f4a5973a69c26bff43eae00
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48868374"
---
# <a name="elastic-database-tools-frequently-asked-questions-faq"></a>Hulpmiddelen voor elastic database Veelgestelde vragen (FAQ)

#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-the-sharding-key-for-the-schema-info"></a>Als ik één tenant per shard en geen sharding-sleutel, hoe ik vullen de sharding-sleutel voor de schema-informatie?

Het schemaobject-informatie wordt alleen gebruikt om op te splitsen samenvoegen scenario's. Als een toepassing inherent één tenant is, hoeven er geen het hulpprogramma voor splitsen samenvoegen en dus is het niet nodig voor het vullen van het schema info-object.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Kan ik een database hebt ingericht en ik heb al een Shard-Toewijzingsbeheer, hoe kan ik deze nieuwe database registreren als een shard?

Raadpleeg  **[een shard toevoegen aan een toepassing met behulp van de clientbibliotheek van elastische database](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Hoeveel kost het hulpprogramma's voor elastische databases?

Alle kosten leidt niet tot de met behulp van de clientbibliotheek van elastische database. Kosten toenemen alleen voor de Azure SQL-databases die u voor shards en de Shard-Toewijzingsbeheer gebruiken, evenals de web/worker-rollen die u inricht voor het hulpprogramma voor splitsen samenvoegen.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Waarom wordt mijn referenties niet werken wanneer ik een shard vanuit een andere server toevoegen?

Gebruik geen referenties in de vorm van ' gebruikers-ID =username@servername", gewoon gebruiken in plaats daarvan ' gebruikers-ID = gebruikersnaam '.  Daarnaast moet u dat de aanmelding 'gebruikersnaam' machtigingen op de shard heeft.

#### <a name="do-i-need-to-create-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Moet ik een Shard-Toewijzingsbeheer maken en vullen van shards telkens wanneer ik mijn toepassingen?

Nee, het maken van de Shard-Toewijzingsbeheer (bijvoorbeeld  **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) is een eenmalige bewerking.  De aanroep moet in uw toepassing gebruiken **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)** tijdens het opstarten van een toepassing.  Er moet slechts één aanroep per toepassingsdomein.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Ik heb vragen over het gebruik van hulpmiddelen voor elastic database, hoe krijg ik deze laten beantwoorden?

Neem contact met ons opnemen op de [-forum Azure SQL Database](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-the-same-shard--is-this-by-design"></a>Wanneer ik een databaseverbinding met behulp van een sharding-sleutel, kan ik nog steeds gegevens opvragen voor andere sleutels sharding in dezelfde shard.  Is dit standaard?

De Elastic Scale API's bieden u een verbinding met de juiste database voor uw sharding-sleutel, maar bieden geen sharding-sleutel filteren.  Voeg **waar** componenten aan uw query aan het bereik beperken tot de opgegeven sharding-sleutel, indien nodig.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Kan ik een andere editie van Azure Database voor elke shard in mijn shard-verzameling gebruiken?

Ja, een shard is een afzonderlijke database, en één shard kan dus een Premium-editie worden, terwijl een andere een Standard-Editie worden. Bovendien kan de editie van een shard omhoog of omlaag schalen meerdere keren tijdens de levensduur van de shard.

#### <a name="does-the-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Een database tijdens een bewerking splitsen of samenvoegen wordt het hulpprogramma voor splitsen en samenvoegen inrichten (of verwijderen)?

Nee. Voor **splitsen** bewerkingen, de doeldatabase moet bestaan met het bijbehorende schema en worden geregistreerd bij de Shard-toewijzing.  Voor **samenvoegen** bewerkingen, moet u de shard verwijderen uit de shard-Toewijzingsbeheer en verwijder vervolgens de database.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]