---
title: Voorbeelden van Azure CLI voor Azure Cosmos DB | Microsoft Docs
description: Voorbeelden van Azure CLI - maken en beheren van Azure DB die Cosmos-accounts, databases, containers, regio's en firewalls.
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: database
ms.date: 11/02/2017
ms.author: mimig
ms.openlocfilehash: 989ef9915028c42a4da817bf2dd3aa5ad2beb2ef
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="azure-cli-samples-for-azure-cosmos-db"></a>Azure CLI-voorbeelden voor Azure Cosmos-DB

De volgende tabel bevat koppelingen naar de Azure CLI-voorbeeldscripts voor Azure Cosmos DB. Pagina's met naslaginformatie voor alle Azure Cosmos DB CLI-opdrachten zijn beschikbaar in de [naslaginformatie over Azure CLI 2.0](https://docs.microsoft.com/cli/azure/cosmosdb).

| |  |
|---|---|
|**Azure DB die Cosmos-account, database en containers maken**||
|[Een DocumentDB-API-account maken](scripts/create-database-account-collections-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Maakt een enkele Cosmos DB-API van Azure-account, database en container voor gebruik met de DocumentDB-API. |
| [Een API MongoDB-account maken](scripts/create-mongodb-database-account-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Maakt een enkel account, database en verzameling in Azure Cosmos DB MongoDB-API. |
|**Azure Cosmos DB schalen**||
| [Schaal container doorvoer](scripts/scale-collection-throughput-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee wijzigt u de ingerichte througput in een container.|
|[Azure DB die Cosmos-databaseaccount in meerdere regio's worden gerepliceerd en het configureren van failover prioriteiten](scripts/scale-multiregion-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Globaal repliceert accountgegevens naar meerdere regio's met een opgegeven failover-prioriteit.|
|**Azure Cosmos DB beveiligen**||
| [Sleutels van account ophalen](scripts/secure-get-account-key-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee haalt u de primaire en secundaire master schrijven en primaire en secundaire alleen-lezen sleutels voor het account.|
| [MongoDB-verbindingsreeks ophalen](scripts/secure-mongo-connection-string-cli.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee haalt u de verbindingsreeks naar uw MongoDB-app verbinden met uw Azure DB die Cosmos-account.|
|[Opnieuw genereren van sleutels](scripts/secure-regenerate-key-cli.md?toc=%2fcli%2fazure%2ftoc.json)|De sleutel van het model of de alleen-lezen voor het account genereert.|
|[Maken van een firewall](scripts/create-firewall-cli.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee maakt u een binnenkomende IP-beleid voor toegangsbeheer om aan het account de toegang beperken tot een goedgekeurde aantal machines en/of cloudservices.|
|**Hoge beschikbaarheid, herstel na noodgevallen, back-up en herstel**||
|[Failover-beleid configureren](scripts/ha-failover-policy-cli.md?toc=%2fcli%2fazure%2ftoc.json)|Hiermee stelt u de prioriteit van de failover van elke regio waarin het account wordt gerepliceerd.|
|**Azure Cosmos DB verbinding te maken met bronnen**||
|[Een WebApp verbinden met Azure Cosmos-DB](../app-service/scripts/app-service-cli-app-service-documentdb.md?toc=%2fcli%2fazure%2ftoc.json)|Maken en koppelen van een Azure DB die Cosmos-database en een Azure-web-app.|
|||
