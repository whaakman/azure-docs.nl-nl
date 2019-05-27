---
title: Informatie over het beheren van consistentie in Azure Cosmos DB
description: Informatie over het beheren van consistentie in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: rimman
ms.openlocfilehash: 68a1a757b5c5e4ce63d7f12a8502d57942d4ec42
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240934"
---
# <a name="manage-consistency-levels-in-azure-cosmos-db"></a>Consistentieniveaus in Azure Cosmos DB beheren

In dit artikel wordt uitgelegd hoe u consistentieniveaus beheert in Azure Cosmos DB. U leert hoe u het standaardconsistentieniveau configureert, de standaardconsistentie overschrijft, handmatig sessietokens beheert en wat metrische PBS-gegevens (Probabilistically Bounded Staleness) zijn.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="configure-the-default-consistency-level"></a>Het standaardconsistentieniveau configureren

De [consistentieniveau standaard](consistency-levels.md) is het consistentieniveau die clients standaard gebruiken. Clients kunnen deze altijd overschrijven.

### <a name="cli"></a>CLI

```bash
# create with a default consistency
az cosmosdb create --name <name of Cosmos DB Account> --resource-group <resource group name> --default-consistency-level Strong

# update an existing account's default consistency
az cosmosdb update --name <name of Cosmos DB Account> --resource-group <resource group name> --default-consistency-level BoundedStaleness
```

### <a name="powershell"></a>PowerShell

In dit voorbeeld maakt een nieuwe Azure Cosmos-account met meerdere regio's voor schrijven ingeschakeld, in regio's VS-Oost en VS-West. Het standaardconsistentieniveau is ingesteld op *sessie* consistentie.

```azurepowershell-interactive
$locations = @(@{"locationName"="East US"; "failoverPriority"=0},
             @{"locationName"="West US"; "failoverPriority"=1})

$iprangefilter = ""

$consistencyPolicy = @{"defaultConsistencyLevel"="Session"}

$CosmosDBProperties = @{"databaseAccountOfferType"="Standard";
                        "locations"=$locations;
                        "consistencyPolicy"=$consistencyPolicy;
                        "ipRangeFilter"=$iprangefilter;
                        "enableMultipleWriteLocations"="true"}

New-AzResource -ResourceType "Microsoft.DocumentDb/databaseAccounts" `
  -ApiVersion "2015-04-08" `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -Name "myCosmosDbAccount" `
  -Properties $CosmosDBProperties
```

### <a name="azure-portal"></a>Azure Portal

Als u het standaardconsistentieniveau wilt weergeven of wijzigen, moet u zich aanmelden bij de Azure-portal. Zoek uw Azure Cosmos-account en open de **Standaardconsistentie** deelvenster. Selecteer het consistentieniveau dat u als nieuwe standaard wilt gebruiken en selecteer **Opslaan**. Azure portal biedt ook een visualisatie van verschillende consistentieniveaus met opmerkingen bij de muziek. 

![Menu Consistentie in de Azure-portal](./media/how-to-manage-consistency/consistency-settings.png)

## <a name="override-the-default-consistency-level"></a>Het standaardconsistentieniveau overschrijven

Clients kunnen het standaardconsistentieniveau dat is ingesteld door de service overschrijven. Consistentieniveau kan worden ingesteld op een per aanvraag, waarmee het standaardconsistentieniveau ingesteld op het accountniveau onderdrukt.

### <a id="override-default-consistency-dotnet"></a>.NET SDK

```csharp
// Override consistency at the client level
ConsistencyPolicy consistencyPolicy = new ConsistencyPolicy
    {
        DefaultConsistencyLevel = ConsistencyLevel.BoundedStaleness,
        MaxStalenessIntervalInSeconds = 5,
        MaxStalenessPrefix = 100
    };
documentClient = new DocumentClient(new Uri(endpoint), authKey, connectionPolicy, consistencyPolicy);

// Override consistency at the request level via request options
RequestOptions requestOptions = new RequestOptions { ConsistencyLevel = ConsistencyLevel.Strong };

var response = await client.CreateDocumentAsync(collectionUri, document, requestOptions);
```

### <a id="override-default-consistency-java-async"></a>Java Async SDK

```java
// Override consistency at the client level
ConnectionPolicy policy = new ConnectionPolicy();

AsyncDocumentClient client =
        new AsyncDocumentClient.Builder()
                .withMasterKey(this.accountKey)
                .withServiceEndpoint(this.accountEndpoint)
                .withConsistencyLevel(ConsistencyLevel.Eventual)
                .withConnectionPolicy(policy).build();
```

### <a id="override-default-consistency-java-sync"></a>Java Sync SDK

```java
// Override consistency at the client level
ConnectionPolicy connectionPolicy = new ConnectionPolicy();
DocumentClient client = new DocumentClient(accountEndpoint, accountKey, connectionPolicy, ConsistencyLevel.Strong);
```

### <a id="override-default-consistency-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
// Override consistency at the client level
const client = new CosmosClient({
  /* other config... */
  consistencyLevel: ConsistencyLevel.Strong
});

// Override consistency at the request level via request options
const { body } = await item.read({ consistencyLevel: ConsistencyLevel.Eventual });
```

### <a id="override-default-consistency-python"></a>Python SDK

```python
# Override consistency at the client level
connection_policy = documents.ConnectionPolicy()
client = cosmos_client.CosmosClient(self.account_endpoint, {'masterKey': self.account_key}, connection_policy, documents.ConsistencyLevel.Strong)
```

## <a name="utilize-session-tokens"></a>Sessietokens gebruiken

Een van de consistentieniveaus in Azure Cosmos DB is *sessie* consistentie. Dit is het standaardniveau voor Cosmos-accounts wordt standaard toegepast. Als u werkt met *sessie* consistentie, de client wordt intern gebruikmaken van de sessietoken van een met elke aanvraag lezen/query om ervoor te zorgen dat het consistentieniveau set wordt gehandhaafd.

Als u sessietokens handmatig wilt beheren, haalt u het sessietoken op uit het antwoord en stelt u het in per aanvraag. Als u sessietokens niet handmatig hoeft te beheren, hoeft u deze voorbeelden niet te gebruiken. De SDK houdt sessietokens automatisch bij. Als u het sessietoken niet handmatig instelt, gebruikt de SDK standaard het meest recente sessietoken.

### <a id="utilize-session-tokens-dotnet"></a>.NET SDK

```csharp
var response = await client.ReadDocumentAsync(
                UriFactory.CreateDocumentUri(databaseName, collectionName, "SalesOrder1"));
string sessionToken = response.SessionToken;

RequestOptions options = new RequestOptions();
options.SessionToken = sessionToken;
var response = await client.ReadDocumentAsync(
                UriFactory.CreateDocumentUri(databaseName, collectionName, "SalesOrder1"), options);
```

### <a id="utilize-session-tokens-java-async"></a>Java Async SDK

```java
// Get session token from response
RequestOptions options = new RequestOptions();
options.setPartitionKey(new PartitionKey(document.get("mypk")));
Observable<ResourceResponse<Document>> readObservable = client.readDocument(document.getSelfLink(), options);
readObservable.single()           // we know there will be one response
  .subscribe(
      documentResourceResponse -> {
          System.out.println(documentResourceResponse.getSessionToken());
      },
      error -> {
          System.err.println("an error happened: " + error.getMessage());
      });

// Resume the session by setting the session token on RequestOptions
RequestOptions options = new RequestOptions();
requestOptions.setSessionToken(sessionToken);
Observable<ResourceResponse<Document>> readObservable = client.readDocument(document.getSelfLink(), options);
```

### <a id="utilize-session-tokens-java-sync"></a>Java Sync SDK

```java
// Get session token from response
ResourceResponse<Document> response = client.readDocument(documentLink, null);
String sessionToken = response.getSessionToken();

// Resume the session by setting the session token on the RequestOptions
RequestOptions options = new RequestOptions();
options.setSessionToken(sessionToken);
ResourceResponse<Document> response = client.readDocument(documentLink, options);
```

### <a id="utilize-session-tokens-javascript"></a>Node.js/JavaScript/TypeScript SDK

```javascript
// Get session token from response
const { headers, item } = await container.items.create({ id: "meaningful-id" });
const sessionToken = headers["x-ms-session-token"];

// Immediately or later, you can use that sessionToken from the header to resume that session.
const { body } = await item.read({ sessionToken });
```

### <a id="utilize-session-tokens-python"></a>Python SDK

```python
// Get the session token from the last response headers
item = client.ReadItem(item_link)
session_token = client.last_response_headers["x-ms-session-token"]

// Resume the session by setting the session token on the options for the request
options = {
    "sessionToken": session_token
}
item = client.ReadItem(doc_link, options)
```

## <a name="monitor-probabilistically-bounded-staleness-pbs-metric"></a>Metrische gegevens van aan waarschijnlijkheid gebonden veroudering (PBS) controleren

Hoe uiteindelijke is uiteindelijke consistentie? Voor de gemiddelde aanvraag kunt bieden wij veroudering grenzen met betrekking tot versiegeschiedenis en tijd. De [ **Probabilistically gebonden veroudering (PBS)** ](https://pbs.cs.berkeley.edu/) metrische gegevens probeert te kwantificeren de kans op veroudering en wordt deze weergegeven als een metrische waarde. Als u wilt de PBS metrische gegevens weergeven, gaat u naar uw Azure Cosmos-account in Azure portal. Open het deelvenster **Metrische gegevens** en selecteer het tabblad **Consistentie**. Bekijk de grafiek met de naam '**Kans op sterk consistente leesbewerkingen op basis van uw werkbelasting (zie PBS)** '.

![PBS-grafiek in de Azure-portal](./media/how-to-manage-consistency/pbs-metric.png)


## <a name="next-steps"></a>Volgende stappen

Lees meer informatie over het beheer van gegevensconflicten of ga verder met het volgende sleutelbegrip in Azure Cosmos DB. Zie de volgende artikelen:

* [Consistentieniveaus in Azure Cosmos DB](consistency-levels.md)
* [Conflicten tussen regio 's beheren](how-to-manage-conflicts.md)
* [Partitionering en gegevensdistributie](partition-data.md)
* [Optimalisatie van de consistentie in het ontwerp van moderne, gedistribueerde database-systemen](https://www.computer.org/csdl/magazine/co/2012/02/mco2012020037/13rRUxjyX7k)
* [Hoge beschikbaarheid](high-availability.md)
* [Azure Cosmos DB SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
