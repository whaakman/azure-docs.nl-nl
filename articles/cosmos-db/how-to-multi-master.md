---
title: Configureren voor meerdere masters in Azure Cosmos DB
description: Leer hoe u meerdere masters kunt configureren in uw toepassingen in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: rimman
ms.openlocfilehash: b862c59002369662d37b6d6a9de28370b0000497
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "61054589"
---
# <a name="how-to-configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Meerdere masters configureren in uw toepassingen die Azure Cosmos DB gebruiken

Meerdere masters als functie wilt gebruiken in uw toepassing, moet u schrijfbewerkingen in meerdere regio's inschakelen en configureren van de multihoming-functie in Azure Cosmos DB. Multihoming is geconfigureerd door in te stellen van de regio waar de toepassing is geïmplementeerd.

## <a id="netv2"></a>.NET SDK v2

Om in te schakelen van meerdere masters in uw toepassingen set `UseMultipleWriteLocations` true en configureren van `SetCurrentLocation` naar de regio waarin de toepassing wordt geïmplementeerd en Azure Cosmos DB is gerepliceerd.

```csharp
ConnectionPolicy policy = new ConnectionPolicy
    {
        ConnectionMode = ConnectionMode.Direct,
        ConnectionProtocol = Protocol.Tcp,
        UseMultipleWriteLocations = true
    };
policy.SetCurrentLocation("West US 2");
```

## <a id="netv3"></a>.NET SDK v3 (preview)

Om meerdere masters in uw toepassingen in te schakelen, moet u `UseCurrentRegion` configureren naar de regio waarin de toepassing wordt geïmplementeerd en Cosmos DB wordt gerepliceerd.

```csharp
CosmosConfiguration config = new CosmosConfiguration("endpoint", "key");
config.UseCurrentRegion("West US");
CosmosClient client = new CosmosClient(config);
```

## <a id="java"></a>Java Async SDK

Om in te schakelen van meerdere masters in uw toepassingen set `policy.setUsingMultipleWriteLocations(true)` en configureer `policy.setPreferredLocations` naar de regio waarin de toepassing wordt geïmplementeerd en Cosmos DB is gerepliceerd.

```java
ConnectionPolicy policy = new ConnectionPolicy();
policy.setUsingMultipleWriteLocations(true);
policy.setPreferredLocations(Collections.singletonList(region));

AsyncDocumentClient client =
    new AsyncDocumentClient.Builder()
        .withMasterKeyOrResourceToken(this.accountKey)
        .withServiceEndpoint(this.accountEndpoint)
        .withConsistencyLevel(ConsistencyLevel.Eventual)
        .withConnectionPolicy(policy).build();
```

## <a id="javascript"></a>Node.js, JavaScript, TypeScript SDK

Om meerdere masters in uw toepassingen in te schakelen, moet u `connectionPolicy.UseMultipleWriteLocations` op Waar instellen en `connectionPolicy.PreferredLocations` configureren naar de regio waarin de toepassing wordt geïmplementeerd en Cosmos DB wordt gerepliceerd.

```javascript
const connectionPolicy: ConnectionPolicy = new ConnectionPolicy();
connectionPolicy.UseMultipleWriteLocations = true;
connectionPolicy.PreferredLocations = [region];

const client = new CosmosClient({
  endpoint: config.endpoint,
  auth: { masterKey: config.key },
  connectionPolicy,
  consistencyLevel: ConsistencyLevel.Eventual
});
```

## <a id="python"></a>Python SDK

Om meerdere masters in uw toepassingen in te schakelen, moet u `connection_policy.UseMultipleWriteLocations` op Waar instellen en `connection_policy.PreferredLocations` configureren naar de regio waarin de toepassing wordt geïmplementeerd en Cosmos DB wordt gerepliceerd.

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>Volgende stappen

U kunt vervolgens de volgende artikelen lezen:

* [Sessietokens gebruiken om de consistentie te beheren in Azure Cosmos DB](how-to-manage-consistency.md#utilize-session-tokens)
* [Conflictsoorten en oplossingsbeleid in Azure Cosmos DB](conflict-resolution-policies.md)
* [Hoge beschikbaarheid in Azure Cosmos DB](high-availability.md)
* [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md)
* [Het juiste consistentieniveau kiezen in Azure Cosmos DB](consistency-levels-choosing.md)
* [Compromissen tussen consistentie, beschikbaarheid en prestaties in Azure Cosmos DB](consistency-levels-tradeoffs.md)
* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [Wereldwijd schalen ingerichte doorvoer](scaling-throughput.md)
* [Wereldwijde distributie - achter de schermen](global-dist-under-the-hood.md)
