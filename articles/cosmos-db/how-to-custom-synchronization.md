---
title: Aangepaste synchronisatie implementeren voor het optimaliseren voor hogere beschikbaarheid en prestaties in Azure Cosmos DB
description: Informatie over het implementeren van aangepaste synchronisatie te optimaliseren voor hogere beschikbaarheid en prestaties in Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 05/23/2019
ms.author: rimman
ms.openlocfilehash: de66149a2ea3e01e62aa8e33ea5a99121a21524f
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67986085"
---
# <a name="implement-custom-synchronization-to-optimize-for-higher-availability-and-performance"></a>Implementeren van aangepaste synchronisatie te optimaliseren voor hogere beschikbaarheid en prestaties

Azure Cosmos DB biedt [vijf goed gedefinieerde consistentieniveaus](consistency-levels.md) voor u om de verkeersbelasting te verdelen van de balans tussen consistentie, prestaties en beschikbaarheid. Sterke consistentie zorgt ervoor dat gegevens asynchroon worden gerepliceerd en blijvend zijn doorgevoerd in elke regio waar het Azure Cosmos-account beschikbaar is. Deze configuratie biedt het hoogste niveau van duurzaamheid, maar wordt geleverd ten koste van prestaties en beschikbaarheid. Als u wilt dat uw toepassing om te bepalen of versoepelen duurzaamheid van gegevens op basis van de toepassing moeten zonder verlies van beschikbaarheid, kunt u *aangepaste synchronisatie* op het niveau van de toepassing voor het duurzaamheidsniveau u wilt.

De volgende afbeelding ziet u visueel het model aangepaste synchronisatie:

![Aangepaste synchronisatie](./media/how-to-custom-synchronization/custom-synchronization.png)

In dit scenario wordt een Azure Cosmos-container wereldwijd gerepliceerd in meerdere regio's op meerdere continenten. Met behulp van sterke consistentie voor alle regio's in dit scenario is van invloed op prestaties. De toepassing kunt om te controleren of een hogere duurzaamheid van gegevens zonder concessies schrijflatentie, twee clients die dezelfde [sessietoken](how-to-manage-consistency.md#utilize-session-tokens).

De eerste client kan gegevens schrijven naar de lokale regio (bijvoorbeeld US - west). De tweede client (bijvoorbeeld in VS Oost) is een lezen-client die wordt gebruikt om te controleren of de synchronisatie. Door de sessietoken van het antwoord schrijven naar het volgende lezen stromen, de lezen zorgt ervoor dat de synchronisatie van schrijfbewerkingen naar VS Oost. Azure Cosmos DB zorgt ervoor dat ten minste één regio zijn zichtbaar voor schrijfbewerkingen. Ze worden gegarandeerd een regionale storing worden bewaard als de oorspronkelijke schrijfregio uitvalt. In dit scenario wordt is elke schrijfbewerking gesynchroniseerd met VS-Oost, waardoor de latentie van die gebruikmaakt van sterke consistentie in alle regio's. U kunt dit model om te synchroniseren in meerdere regio's parallel kunt uitbreiden in een scenario waarbij schrijfbewerkingen in elke regio plaatsvinden, meerdere masters.

## <a name="configure-the-clients"></a>De clients configureren

Het volgende voorbeeld ziet u een laag voor gegevenstoegang waarmee een instantie van twee clients voor aangepaste synchronisatie:

### <a name="net-v2-sdk"></a>.Net V2 SDK
```csharp
class MyDataAccessLayer
{
    private DocumentClient writeClient;
    private DocumentClient readClient;

    public async Task InitializeAsync(Uri accountEndpoint, string key)
    {
        ConnectionPolicy writeConnectionPolicy = new ConnectionPolicy
        {
            ConnectionMode = ConnectionMode.Direct,
            ConnectionProtocol = Protocol.Tcp,
            UseMultipleWriteLocations = true
        };
        writeConnectionPolicy.SetCurrentLocation(LocationNames.WestUS);

        ConnectionPolicy readConnectionPolicy = new ConnectionPolicy
        {
            ConnectionMode = ConnectionMode.Direct,
            ConnectionProtocol = Protocol.Tcp
        };
        readConnectionPolicy.SetCurrentLocation(LocationNames.EastUS);

        writeClient = new DocumentClient(accountEndpoint, key, writeConnectionPolicy);
        readClient = new DocumentClient(accountEndpoint, key, readConnectionPolicy, ConsistencyLevel.Session);

        await Task.WhenAll(new Task[]
        {
            writeClient.OpenAsync(),
            readClient.OpenAsync()
        });
    }
}
```

### <a name="net-v3-sdk"></a>.Net V3 SDK
```csharp
class MyDataAccessLayer
{
    private CosmosClient writeClient;
    private CosmosClient readClient;

    public void InitializeAsync(string accountEndpoint, string key)
    {
        CosmosClientOptions writeConnectionOptions = new CosmosClientOptions();
        writeConnectionOptions.ConnectionMode = ConnectionMode.Direct;
        writeConnectionOptions.ApplicationRegion = "West US";

        CosmosClientOptions readConnectionOptions = new CosmosClientOptions();
        readConnectionOptions.ConnectionMode = ConnectionMode.Direct;
        readConnectionOptions.ApplicationRegion = "East US";


        writeClient = new CosmosClient(accountEndpoint, key, writeConnectionOptions);
        writeClient = new CosmosClient(accountEndpoint, key, writeConnectionOptions);
    }
}
```

## <a name="implement-custom-synchronization"></a>Aangepaste synchronisatie implementeren

Nadat de clients zijn geïnitialiseerd, kunt de toepassing uitvoeren schrijfbewerkingen naar de lokale regio (VS West) en de force-synchronisatie de schrijfbewerkingen naar VS Oost als volgt.

### <a name="net-v2-sdk"></a>.Net V2 SDK
```csharp
class MyDataAccessLayer
{
    public async Task CreateItem(string containerLink, Document document)
    {
        ResourceResponse<Document> response = await writeClient.CreateDocumentAsync(
            containerLink, document);

        await readClient.ReadDocumentAsync(response.Resource.SelfLink,
            new RequestOptions { SessionToken = response.SessionToken });
    }
}
```

### <a name="net-v3-sdk"></a>.Net V3 SDK
```csharp
class MyDataAccessLayer
{
     public async Task CreateItem(string databaseId, string containerId, dynamic item)
     {
        ItemResponse<dynamic> response = await writeClient.GetContainer("containerId", "databaseId")
            .CreateItemAsync<dynamic>(
                item,
                new PartitionKey("test"));

        await readClient.GetContainer("containerId", "databaseId").ReadItemAsync<dynamic>(
            response.Resource.id,
            new PartitionKey("test"),
            new ItemRequestOptions { SessionToken = response.Headers.Session, ConsistencyLevel = ConsistencyLevel.Session });
    }
}
```

U kunt het model om te synchroniseren in meerdere regio's tegelijk kunt uitbreiden.

## <a name="next-steps"></a>Volgende stappen

Lees deze artikelen voor meer informatie over globale distributie en consistentie in Azure Cosmos DB:

* [Kies de juiste consistentieniveau in Azure Cosmos DB](consistency-levels-choosing.md)
* [Compromissen tussen consistentie, beschikbaarheid en prestaties in Azure Cosmos DB](consistency-levels-tradeoffs.md)
* [Consistentie in Azure Cosmos DB beheren](how-to-manage-consistency.md)
* [Partitioneren en gegevensdistributie in Azure Cosmos DB](partition-data.md)
