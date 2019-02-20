---
title: Aangepaste synchronisatie implementeren voor het optimaliseren voor hogere beschikbaarheid en prestaties in Azure Cosmos DB
description: Informatie over het implementeren van aangepaste synchronisatie voor het optimaliseren voor hogere beschikbaarheid en prestaties in Azure Cosmos DB
author: markjbrown
ms.service: cosmos-db
ms.topic: sample
ms.date: 2/12/2019
ms.author: mjbrown
ms.openlocfilehash: 9033a7502919c8dc05053048272f3fa62f81b31d
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118835"
---
# <a name="how-to-implement-custom-synchronization-to-optimize-for-higher-availability-and-performance"></a>Aangepaste synchronisatie implementeren voor het optimaliseren voor hogere beschikbaarheid en prestaties

Azure Cosmos DB biedt vijf duidelijk gedefinieerde consistentieniveaus waaruit u kunt kiezen om het verschil tussen de consistentie, prestaties en beschikbaarheid in balans te houden. Sterke consistentie zorgt ervoor dat gegevens asynchroon worden gerepliceerd en duurzaam worden opgeslagen in elke regio waarmee het Cosmos-account is geconfigureerd. Deze configuratie biedt het hoogste duurzaamheidsniveau, maar dit gaat ten koste van de prestaties en beschikbaarheid. Als een toepassing de duurzaamheid van gegevens wil besturen/versoepelen ten gunste van de behoeften van de toepassing, zonder verlies van beschikbaarheid, kan deze gebruik maken van aangepaste synchronisatie op het niveau van de toepassing om het gewenste niveau van duurzaamheid te bereiken.

Het onderstaande diagram geeft het aangepaste synchronisatiemodel op een visuele manier weer.

![Aangepaste synchronisatie](./media/how-to-custom-synchronization/custom-synchronization.png)

In dit scenario wordt een Cosmos-container wereldwijd gerepliceerd in verschillende regio's op meerdere continenten. Als u in sterke consistentie zou gebruiken voor alle regio's in dit scenario, dan zou dit invloed op de prestaties hebben. Om een hogere duurzaamheid van gegevens te verzekeren zonder verlies van de schrijflatentie, kan de toepassing twee clients gebruiken die hetzelfde sessietoken delen.

De eerste client kan gegevens schrijven naar de lokale regio (bijvoorbeeld US - west). De tweede client (bijvoorbeeld in US - oost) is een leesclient die wordt gebruikt om voor de synchronisatie te zorgen. Door het sessietoken uit het schrijfantwoord naar de volgende leesbewerking te sturen, zorgt de leesbewerking voor de synchronisatie van schrijfbewerkingen naar US - oost. Azure Cosmos DB zorgt ervoor dat schrijfbewerkingen zichtbaar zijn voor ten minste één regio en dat ze worden gegarandeerd een regionale storing te overleven als de oorspronkelijke schrijfregio uitvalt. In dit scenario wordt elke schrijfbewerking gesynchroniseerd met US - oost, waardoor de latentie wordt verminderd wanneer van sterke consistentie in alle regio's gebruik wordt gemaakt. In een scenario voor meerdere masters, waarbij schrijfbewerkingen in elke regio plaatsvinden, kan dit model worden uitgebreid om in meerdere regio's tegelijkertijd te synchroniseren.

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
