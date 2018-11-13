---
title: Voorbeelden van Azure CLI voor Azure Cosmos DB | Microsoft Docs
description: Voorbeelden van Azure CLI - Azure DB Cosmos-accounts, -databases, -containers, -regio's en -firewalls maken en beheren.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/26/2018
ms.author: mjbrown
ms.openlocfilehash: 461207d0c9d27ed645dcac98e6256431bb23f8ad
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51005985"
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Voorbeelden van Azure CLI voor Azure Cosmos DB

De volgende tabel bevat koppelingen naar Azure CLI-voorbeeldscripts voor Azure Cosmos DB. Referentiepagina's voor alle Azure Cosmos DB CLI-opdrachten zijn beschikbaar in de [naslaginformatie voor Azure CLI](/cli/azure/cosmosdb).

| |  |
|---|---|
|**Azure Cosmos DB-account, -database en -containers maken**||
| [SQL API-account maken](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee maakt u één Azure Cosmos DB SQL-API-account, -database en -container. |
| [MongoDB-API-account maken](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u één Azure Cosmos DB MongoDB-API-account, -database en -verzameling. |
| [Een Gremlin-API-account maken](scripts/create-gremlin-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u één Azure Cosmos DB Gremlin-API-account, -database en -graaf. |
| [Een Cassandra-API-account maken](scripts/create-cassandra-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u één Azure Cosmos DB Cassandra API-account en -database. |
| [Een tabel-API-account maken](scripts/create-table-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u één Azure Cosmos DB Table-API-account, -database en -tabel. |
|**Azure Cosmos DB schalen**||
| [Doorvoer van container schalen](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee wordt de ingerichte doorvoer voor een container gewijzigd.|
| [Azure Cosmos DB-databaseaccount in meerdere regio's repliceren en failover-prioriteiten configureren](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hiermee worden accountgegevens in meerdere regio's met een bepaalde failover-prioriteit globaal gerepliceerd.|
|**Azure Cosmos DB beveiligen**||
| [Accountsleutels ophalen](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee worden de primaire en secundaire hoofdsleutels voor schrijven en de primaire en secundaire sleutels voor alleen-lezen voor het account opgehaald.|
| [MongoDB-verbindingsreeks ophalen](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee wordt de verbindingsreeks opgehaald om uw MongoDB-app te verbinden met uw Azure Cosmos DB-account.|
| [Accountsleutels opnieuw genereren](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Genereer de sleutels voor het account opnieuw.|
| [Firewall maken](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee wordt toegangsbeheerbeleid voor inkomende IP's gemaakt om de toegang tot het account vanaf een goedgekeurde set computers en/of cloudservices te beperken.|
|**Hoge beschikbaarheid, herstel na noodgevallen, back-up en herstel**||
| [Failoverbeleid configureren](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hiermee wordt de prioriteit van failover ingesteld voor elke regio waarin het account wordt gerepliceerd.|
|**Azure Cosmos DB verbinden met resources**||
| [Een web-app verbinden met Azure Cosmos DB](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Hiermee maakt u een Azure Cosmos DB-database en een Azure-web-app en verbindt u deze met elkaar.|
|||
