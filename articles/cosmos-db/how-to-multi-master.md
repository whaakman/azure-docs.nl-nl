---
title: Configureren voor meerdere masters in Azure Cosmos DB
description: Leer hoe u meerdere masters kunt configureren in uw toepassingen in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 2/12/2019
ms.author: mjbrown
ms.openlocfilehash: 84c8e2921602bb653c0b1ef0adffd3d89e91bd78
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56312137"
---
# <a name="how-to-configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Meerdere masters configureren in uw toepassingen die Azure Cosmos DB gebruiken

Als u functies voor meerdere masters in uw toepassingen wilt gebruiken, moet u schrijfbewerkingen in meerdere regio's inschakelen en de multihoming-mogelijkheid configureren. Multihoming wordt geconfigureerd door de regio in te stellen waarin de toepassing op dat moment is geïmplementeerd.

## <a id="netv2"></a>.NET SDK v2

Om meerdere masters in uw toepassingen in te schakelen, moet u `UseMultipleWriteLocations` op Waar instellen en `SetCurrentLocation` configureren naar de regio waarin de toepassing wordt geïmplementeerd en Azure Cosmos DB wordt gerepliceerd.

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

Om meerdere masters in uw toepassingen in te schakelen, moet u `policy.setUsingMultipleWriteLocations(true)` op Waar instellen en `policy.setPreferredLocations` configureren naar de regio waarin de toepassing wordt geïmplementeerd en Cosmos DB wordt gerepliceerd.

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

Meer informatie over meerdere masters, wereldwijde distributie en consistentie in Azure Cosmos DB. Zie de volgende artikelen:

* [Sessietokens gebruiken om de consistentie te beheren in Azure Cosmos DB](how-to-manage-consistency.md#utilize-session-tokens)

* [Conflictsoorten en oplossingsbeleid in Azure Cosmos DB](conflict-resolution-policies.md)

* [Hoge beschikbaarheid in Azure Cosmos DB](high-availability.md)

* [Het juiste consistentieniveau kiezen in Azure Cosmos DB](consistency-levels-choosing.md)

* [Compromissen tussen consistentie, beschikbaarheid en prestaties in Azure Cosmos DB](consistency-levels-tradeoffs.md)
