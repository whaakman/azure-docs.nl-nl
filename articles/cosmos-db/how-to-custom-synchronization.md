---
title: Aangepaste synchronisatie implementeren voor het optimaliseren voor hogere beschikbaarheid en prestaties in Azure Cosmos DB
description: Informatie over het implementeren van aangepaste synchronisatie te optimaliseren voor hogere beschikbaarheid en prestaties in Azure Cosmos DB.
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: rimman
ms.openlocfilehash: f10e260432a93a0413d65d6f5814d00a50e9465a
ms.sourcegitcommit: 179918af242d52664d3274370c6fdaec6c783eb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/13/2019
ms.locfileid: "65560272"
---
# <a name="implement-custom-synchronization-to-optimize-for-higher-availability-and-performance"></a>Implementeren van aangepaste synchronisatie te optimaliseren voor hogere beschikbaarheid en prestaties

Azure Cosmos DB biedt [vijf goed gedefinieerde consistentieniveaus](consistency-levels.md) voor u om de verkeersbelasting te verdelen van de balans tussen consistentie, prestaties en beschikbaarheid. Sterke consistentie zorgt ervoor dat gegevens asynchroon worden gerepliceerd en blijvend zijn doorgevoerd in elke regio waar het Azure Cosmos-account beschikbaar is. Deze configuratie biedt het hoogste niveau van duurzaamheid, maar wordt geleverd ten koste van prestaties en beschikbaarheid. Als u wilt dat uw toepassing om te bepalen of versoepelen duurzaamheid van gegevens op basis van de toepassing moeten zonder verlies van beschikbaarheid, kunt u *aangepaste synchronisatie* op het niveau van de toepassing voor het duurzaamheidsniveau u wilt.

De volgende afbeelding ziet u visueel het model aangepaste synchronisatie:

![Aangepaste synchronisatie](./media/how-to-custom-synchronization/custom-synchronization.png)

In dit scenario wordt een Azure Cosmos-container wereldwijd gerepliceerd in meerdere regio's op meerdere continenten. Met behulp van sterke consistentie voor alle regio's in dit scenario is van invloed op prestaties. De toepassing kunt om te controleren of een hogere duurzaamheid van gegevens zonder concessies schrijflatentie, twee clients die dezelfde [sessietoken](how-to-manage-consistency.md#utilize-session-tokens).

De eerste client kan gegevens schrijven naar de lokale regio (bijvoorbeeld US - west). De tweede client (bijvoorbeeld in VS Oost) is een lezen-client die wordt gebruikt om te controleren of de synchronisatie. Door de sessietoken van het antwoord schrijven naar het volgende lezen stromen, de lezen zorgt ervoor dat de synchronisatie van schrijfbewerkingen naar VS Oost. Azure Cosmos DB zorgt ervoor dat ten minste één regio zijn zichtbaar voor schrijfbewerkingen. Ze worden gegarandeerd een regionale storing worden bewaard als de oorspronkelijke schrijfregio uitvalt. In dit scenario wordt is elke schrijfbewerking gesynchroniseerd met VS-Oost, waardoor de latentie van die gebruikmaakt van sterke consistentie in alle regio's. U kunt dit model om te synchroniseren in meerdere regio's parallel kunt uitbreiden in een scenario waarbij schrijfbewerkingen in elke regio plaatsvinden, meerdere masters.

## <a name="configure-the-clients"></a>De clients configureren

Het volgende voorbeeld ziet u een laag voor gegevenstoegang waarmee een instantie van twee clients voor aangepaste synchronisatie:

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

## <a name="implement-custom-synchronization"></a>Aangepaste synchronisatie implementeren

Nadat de clients zijn geïnitialiseerd, kunt de toepassing uitvoeren schrijfbewerkingen naar de lokale regio (VS West) en de force-synchronisatie de schrijfbewerkingen naar VS Oost als volgt.

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

U kunt het model om te synchroniseren in meerdere regio's tegelijk kunt uitbreiden.

## <a name="next-steps"></a>Volgende stappen

Lees deze artikelen voor meer informatie over globale distributie en consistentie in Azure Cosmos DB:

* [Kies de juiste consistentieniveau in Azure Cosmos DB](consistency-levels-choosing.md)
* [Compromissen tussen consistentie, beschikbaarheid en prestaties in Azure Cosmos DB](consistency-levels-tradeoffs.md)
* [Consistentie in Azure Cosmos DB beheren](how-to-manage-consistency.md)
* [Partitioneren en gegevensdistributie in Azure Cosmos DB](partition-data.md)
