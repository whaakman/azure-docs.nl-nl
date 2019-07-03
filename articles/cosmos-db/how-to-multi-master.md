---
title: Configureren voor meerdere masters in Azure Cosmos DB
description: Informatie over het configureren van meerdere masters in uw toepassingen in Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 07/02/2019
ms.author: mjbrown
ms.openlocfilehash: 73b4ada713e264aaa2504fe4d4f504e07ae45181
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538088"
---
# <a name="configure-multi-master-in-your-applications-that-use-azure-cosmos-db"></a>Configureren van meerdere masters in uw toepassingen die gebruikmaken van Azure Cosmos DB

Wanneer een account is gemaakt met meerdere regio's voor schrijven ingeschakeld, moet u twee wijzigingen aanbrengt in uw toepassing aan de ConnectionPolicy voor de DocumentClient om in te schakelen van de mogelijkheden voor meerdere masters en multihoming in Azure Cosmos DB. In de ConnectionPolicy instellen UseMultipleWriteLocations true en de naam van de regio waar de toepassing is geïmplementeerd voor SetCurrentLocation doorgeven. Hiermee wordt de eigenschap PreferredLocations op basis van de geo-nabijheid van de locatie die wordt doorgegeven in gevuld. Als een nieuwe regio wordt later toegevoegd aan het account, de toepassing hoeft niet te worden bijgewerkt of opnieuw geïmplementeerd, detecteert automatisch de dichter bij de regio en wordt automatisch-startpagina bij deze moet een regionale gebeurtenis worden uitgevoerd.

> [!TIP]
> Cosmos-accounts die zijn gemaakt niet in eerste instantie geconfigureerd voor meerdere masters kunnen worden gemigreerd met nul uitvaltijd. Voor meer informatie over meer Zie [meerdere schrijfregio's configureren](how-to-manage-database-account.md#configure-multiple-write-regions)

## <a id="netv2"></a>.NET SDK v2

Instellen om in te schakelen meerdere masters in uw toepassing, `UseMultipleWriteLocations` naar `true`. Stel ook `SetCurrentLocation` naar de regio waar de toepassing wordt geïmplementeerd en waarin de Azure Cosmos DB is gerepliceerd:

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

Instellen om in te schakelen meerdere masters in uw toepassing, `UseCurrentRegion` naar de regio waar de toepassing wordt geïmplementeerd en waar Cosmos DB worden gerepliceerd:

```csharp
CosmosConfiguration config = new CosmosConfiguration("endpoint", "key");
config.UseCurrentRegion("West US");
CosmosClient client = new CosmosClient(config);
```

## <a id="java"></a>Java Async SDK

Instellen om in te schakelen meerdere masters in uw toepassing, `policy.setUsingMultipleWriteLocations(true)` en stel `policy.setPreferredLocations` naar de regio waar de toepassing wordt geïmplementeerd en waar Cosmos DB worden gerepliceerd:

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

## <a id="javascript"></a>Node.js, JavaScript en TypeScript-SDK 's

Instellen om in te schakelen meerdere masters in uw toepassing, `connectionPolicy.UseMultipleWriteLocations` naar `true`. Stel ook `connectionPolicy.PreferredLocations` naar de regio waar de toepassing wordt geïmplementeerd en waar Cosmos DB worden gerepliceerd:

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

Instellen om in te schakelen meerdere masters in uw toepassing, `connection_policy.UseMultipleWriteLocations` naar `true`. Stel ook `connection_policy.PreferredLocations` naar de regio waar de toepassing wordt geïmplementeerd en waarin een Cosmos DB is gerepliceerd.

```python
connection_policy = documents.ConnectionPolicy()
connection_policy.UseMultipleWriteLocations = True
connection_policy.PreferredLocations = [region]

client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Session)
```

## <a name="next-steps"></a>Volgende stappen

Lees de volgende artikelen:

* [Sessie-tokens gebruiken voor het beheren van consistentie in Azure Cosmos DB](how-to-manage-consistency.md#utilize-session-tokens)
* [Conflictsoorten en oplossingsbeleid in Azure Cosmos DB](conflict-resolution-policies.md)
* [Hoge beschikbaarheid in Azure Cosmos DB](high-availability.md)
* [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md)
* [Kies de juiste consistentieniveau in Azure Cosmos DB](consistency-levels-choosing.md)
* [Compromissen tussen consistentie, beschikbaarheid en prestaties in Azure Cosmos DB](consistency-levels-tradeoffs.md)
* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [Wereldwijd schalen ingerichte doorvoer](scaling-throughput.md)
* [Wereldwijde distributie: Kijkje achter de schermen](global-dist-under-the-hood.md)
