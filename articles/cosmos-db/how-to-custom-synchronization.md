---
title: Aangepaste synchronisatie implementeren voor het optimaliseren voor hogere beschikbaarheid en prestaties in Azure Cosmos DB
description: Informatie over het implementeren van aangepaste synchronisatie voor het optimaliseren voor hogere beschikbaarheid en prestaties in Azure Cosmos DB
author: rimman
ms.service: cosmos-db
ms.topic: sample
ms.date: 04/15/2019
ms.author: rimman
ms.openlocfilehash: d948798f161eb36578cb679b6d96409917424fd4
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59678459"
---
# <a name="how-to-implement-custom-synchronization-to-optimize-for-higher-availability-and-performance"></a>Aangepaste synchronisatie implementeren voor het optimaliseren voor hogere beschikbaarheid en prestaties

Azure Cosmos DB biedt [vijf goed gedefinieerde consistentieniveaus](consistency-levels.md) voor u kunt selecteren in het verschil tussen de consistentie, prestaties en beschikbaarheid in balans brengen. Sterke consistentie zorgt ervoor dat gegevens worden asynchroon worden gerepliceerd en blijvend zijn doorgevoerd in elke regio waar het Azure Cosmos-account beschikbaar is. Deze configuratie biedt het hoogste duurzaamheidsniveau, maar dit gaat ten koste van de prestaties en beschikbaarheid. Als een toepassing wil besturingselement/versoepelen de duurzaamheid van gegevens op basis van de toepassing moeten u zonder verlies van beschikbaarheid, deze kunt gebruiken *aangepaste synchronisatie* op het niveau van de toepassing naar het gewenste niveau van de bereiken duurzaamheid.

Het onderstaande diagram geeft het aangepaste synchronisatiemodel op een visuele manier weer.

![Aangepaste synchronisatie](./media/how-to-custom-synchronization/custom-synchronization.png)

In dit scenario wordt een Azure Cosmos-container wereldwijd gerepliceerd in verschillende regio's op meerdere continenten. Sterke consistentie voor alle regio's in dit scenario zou dit invloed op de prestaties. Om te controleren of een hogere duurzaamheid van gegevens zonder verlies van de latentie schrijven, kunt de toepassing twee clients die dezelfde [sessietoken](how-to-manage-consistency.md#utilize-session-tokens).

De eerste client kan gegevens schrijven naar de lokale regio (bijvoorbeeld US - west). De tweede client (bijvoorbeeld in US - oost) is een leesclient die wordt gebruikt om voor de synchronisatie te zorgen. Door de sessietoken van het antwoord schrijven naar het volgende lezen stromen, lezen zorgt ervoor dat de synchronisatie van schrijfbewerkingen naar VS Oost. Azure Cosmos DB zorgt ervoor dat schrijfbewerkingen zichtbaar zijn voor ten minste één regio en dat ze worden gegarandeerd een regionale storing te overleven als de oorspronkelijke schrijfregio uitvalt. In dit scenario wordt elke schrijfbewerking gesynchroniseerd met US - oost, waardoor de latentie wordt verminderd wanneer van sterke consistentie in alle regio's gebruik wordt gemaakt. In een scenario voor meerdere masters, waarbij schrijfbewerkingen in elke regio plaatsvinden, kan dit model worden uitgebreid om in meerdere regio's tegelijkertijd te synchroniseren.

## <a name="configure-the-clients"></a>De clients configureren

In het onderstaande voorbeeld ziet u hoe de laag voor gegevenstoegang twee clients instantieert voor dit doel.

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

Nadat de clients zijn geïnitialiseerd, kan de toepassing schrijfbewerkingen naar de lokale regio (US - west) uitvoeren en de schrijfbewerkingen naar US - oost als volgt gedwongen synchroniseren.

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

Dit model kan worden uitgebreid om in meerdere regio's tegelijkertijd te synchroniseren.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over meerdere masters, wereldwijde distributie en consistentie in Azure Cosmos DB vindt u in de volgende artikelen:

* [Het juiste consistentieniveau kiezen in Azure Cosmos DB](consistency-levels-choosing.md)
* [Compromissen tussen consistentie, beschikbaarheid en prestaties in Azure Cosmos DB](consistency-levels-tradeoffs.md)
* [Informatie over het beheren van consistentie in Azure Cosmos DB](how-to-manage-consistency.md)
* [Partitioneren en gegevensdistributie in Azure Cosmos DB](partition-data.md)
