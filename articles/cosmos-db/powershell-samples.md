---
title: Voorbeelden van Azure PowerShell voor Azure Cosmos DB
description: 'Azure PowerShell-voorbeelden: scripts voor het maken en beheren van Azure Cosmos DB-accounts.'
author: SnehaGunda
ms.service: cosmos-db
ms.topic: sample
ms.date: 10/16/2017
ms.author: sngun
ms.openlocfilehash: 3498ac6a2a4aaa1682d7b5bc5aae5383866d5bcd
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56873794"
---
# <a name="azure-powershell-samples-for-azure-cosmos-db"></a>Voorbeelden van Azure PowerShell voor Azure Cosmos DB

De volgende tabel bevat links naar Azure PowerShell-voorbeeldscripts voor Azure Cosmos DB. Op dit moment kunt u alleen het Azure Cosmos DB-account beheren via PowerShell. Andere resources, zoals databases en containers, kunnen niet via PowerShell worden beheerd.

| |  |
|---|---|
|**Azure Cosmos DB-account maken**||
|[Cosmos-account maken en configureren met SQL-API](scripts/create-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee wordt één Azure Cosmos DB-account gemaakt voor gebruik met de SQL-API. |
|[Een Cosmos-account maken en configureren met behulp van de Azure Cosmos DB-API voor MongoDB](scripts/create-mongodb-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee wordt één Cosmos-account gemaakt voor gebruik met de API voor MongoDB van Azure Cosmos DB. |
|[Cosmos-account maken en configureren met Gremlin-API](scripts/create-graph-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee wordt één Azure Cosmos DB-account gemaakt voor gebruik met de Gremlin-API. |
|[Cosmos-account maken en configureren met Cassandra-API](scripts/create-and-configure-cassandra-database.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee wordt één Azure Cosmos DB-account gemaakt voor gebruik met de Cassandra-API. |
|[Cosmos-account maken en configureren met Table-API](scripts/create-table-database-account-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee wordt één Azure Cosmos DB-account gemaakt voor gebruik met de tabel-API. |
|**Azure Cosmos DB schalen**||
|[Azure Cosmos DB-account in meerdere regio's repliceren en failover-prioriteiten configureren](scripts/scale-multiregion-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hiermee worden accountgegevens in meerdere regio's met een bepaalde failover-prioriteit globaal gerepliceerd.|
|**Azure Cosmos DB beveiligen**||
| [Accountsleutels ophalen](scripts/secure-get-account-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee worden de primaire en secundaire hoofdsleutels voor schrijven en de primaire en secundaire sleutels voor alleen-lezen voor het account opgehaald.|
| [MongoDB-verbindingsreeks ophalen](scripts/secure-mongo-connection-string-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json) | Hiermee wordt de verbindingsreeks opgehaald om uw MongoDB-app te verbinden met uw Azure Cosmos DB-account.|
|[Accountsleutels opnieuw genereren](scripts/secure-regenerate-key-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hiermee wordt de hoofdsleutel of alleen-lezensleutel voor het account opnieuw gegenereerd.|
|[Firewall maken](scripts/create-firewall-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)| Hiermee wordt toegangsbeheerbeleid voor inkomende IP's gemaakt om de toegang tot het account vanaf een goedgekeurde set computers en/of cloudservices te beperken.|
|**Hoge beschikbaarheid, herstel na noodgevallen, back-up maken en herstellen**||
|[Failoverbeleid configureren](scripts/ha-failover-policy-powershell.md?toc=%2fpowershell%2fmodule%2ftoc.json)|Hiermee wordt de prioriteit van failover ingesteld voor elke regio waarin het account wordt gerepliceerd.|
|||
