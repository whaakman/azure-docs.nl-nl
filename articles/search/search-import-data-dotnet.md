---
title: Gegevens laden in een Azure Search-index in C# (.NET SDK) - Azure Search
description: Lees hoe u gegevens uploadt naar een doorzoekbare volledige-tekstindex in Azure Search met behulp van C#-voorbeeldcode en de .NET SDK.
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/20/2019
ms.openlocfilehash: d2d54d1425bbb67a3f5ba1b6081a9f74ff87f4d6
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58286904"
---
# <a name="quickstart-2---load-data-to-an-azure-search-index-using-c"></a>Quickstart: 2 - gegevens laden in een Azure Search-index metC#

Dit artikel leest u hoe u gegevens importeert in [een Azure Search-index](search-what-is-an-index.md) met behulp van C# en de [.NET SDK](https://aka.ms/search-sdk). Documenten in uw index pushen wordt bereikt door het uitvoeren van deze taken:

> [!div class="checklist"]
> * Maak een [ `SearchIndexClient` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) object verbinding maken met een search-index.
> * Maak een [ `IndexBatch` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) object met de documenten die moeten worden toegevoegd, gewijzigd of verwijderd.
> * Roep de `Documents.Index` methode voor `SearchIndexClient` om documenten te uploaden naar een index.

## <a name="prerequisites"></a>Vereisten

[Een Azure Search-index maken](search-create-index-dotnet.md) en een `SearchServiceClient` -object, zoals wordt weergegeven in ['Maken een client'](search-create-index-dotnet.md#CreateSearchServiceClient).


## <a name="create-a-client"></a>Een client maken
Om gegevens te importeren, moet u een exemplaar van de `SearchIndexClient` klasse. Er zijn verschillende manieren voor het maken van deze klasse, inclusief het gebruik van de `SearchServiceClient` -exemplaar dat al is gemaakt. 

Zoals in het volgende voorbeeld te zien, kunt u de `SearchServiceClient` exemplaar en de aanroep van de `Indexes.GetClient` methode. Dit fragment verkrijgt een `SearchIndexClient` voor de index "hotels" met de naam van een `SearchServiceClient` met de naam `serviceClient`.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

`SearchIndexClient` heeft een `Documents`-eigenschap. Deze eigenschap bevat de methoden die u wilt toevoegen, wijzigen, verwijderen, of om documenten in uw index op te vragen.

> [!NOTE]
> In een toepassing standaardzoekopdrachten worden query's en indexeren afzonderlijk verwerkt. Terwijl `Indexes.GetClient` is handig omdat u kunt objecten zoals hergebruiken `SearchCredentials`, een meer robuuste benadering omvat het maken van de `SearchIndexClient` rechtstreeks zodat u in een querysleutel in plaats van een administratorsleutel doorgeven kunt. Met deze procedure is consistent met de [principe van minimale bevoegdheden](https://en.wikipedia.org/wiki/Principle_of_least_privilege) en helpt bij het beter beveiligen van uw toepassing. U moet opgeven, een `SearchIndexClient` in de volgende oefening. Zie voor meer informatie over sleutels, [maken en beheren van api-sleutels voor Azure Search-service](search-security-api-keys.md).
> 
> 

<a name="construct-indexbatch"></a>

## <a name="construct-indexbatch"></a>Construct IndexBatch

Voor het importeren van gegevens met behulp van de .NET SDK-pakket van uw gegevens in een `IndexBatch` object. Een `IndexBatch` bevat een verzameling van `IndexAction` objecten, die elk een document en een eigenschap die Azure Search welke actie om uit te voeren voor het desbetreffende document (uploaden, samenvoegen, verwijderen en mergeOrUpload) bevat. Zie voor meer informatie over de indexing acties [indexeren acties: uploaden, samenvoegen, mergeOrUpload, verwijder](search-what-is-data-import.md#indexing-actions).

Ervan uitgaande dat u weet welke bewerkingen om uit te voeren op uw documenten, bent u klaar om samen te stellen de `IndexBatch`. Het volgende voorbeeld laat zien hoe u een batch met verschillende bewerkingen kunt maken. Het voorbeeld wordt een aangepaste klasse genaamd `Hotel` die wordt toegewezen aan een document in de index "hotels".

```csharp
var actions =
    new IndexAction<Hotel>[]
    {
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "1",
                BaseRate = 199.0,
                Description = "Best hotel in town",
                DescriptionFr = "Meilleur hôtel en ville",
                HotelName = "Fancy Stay",
                Category = "Luxury",
                Tags = new[] { "pool", "view", "wifi", "concierge" },
                ParkingIncluded = false,
                SmokingAllowed = false,
                LastRenovationDate = new DateTimeOffset(2010, 6, 27, 0, 0, 0, TimeSpan.Zero),
                Rating = 5,
                Location = GeographyPoint.Create(47.678581, -122.131577)
            }),
        IndexAction.Upload(
            new Hotel()
            {
                HotelId = "2",
                BaseRate = 79.99,
                Description = "Cheapest hotel in town",
                DescriptionFr = "Hôtel le moins cher en ville",
                HotelName = "Roach Motel",
                Category = "Budget",
                Tags = new[] { "motel", "budget" },
                ParkingIncluded = true,
                SmokingAllowed = true,
                LastRenovationDate = new DateTimeOffset(1982, 4, 28, 0, 0, 0, TimeSpan.Zero),
                Rating = 1,
                Location = GeographyPoint.Create(49.678581, -122.131577)
            }),
        IndexAction.MergeOrUpload(
            new Hotel()
            {
                HotelId = "3",
                BaseRate = 129.99,
                Description = "Close to town hall and the river"
            }),
        IndexAction.Delete(new Hotel() { HotelId = "6" })
    };

var batch = IndexBatch.New(actions);
```

In dit geval gebruiken we `Upload`, `MergeOrUpload`, en `Delete` als de zoekacties, zoals opgegeven door de methoden die worden aangeroepen voor de `IndexAction`-klasse.

We gaan ervan uit dat deze voorbeeldindex "hotels" al is gevuld met een aantal documenten. We hoefden niet alle mogelijke documentvelden op te geven voor het gebruik van `MergeOrUpload`. We hebben alleen de documentsleutel (`HotelId`) opgegeven voor het gebruik van `Delete`.

U kunt tot 1000 documenten in een enkele indexeringsaanvraag opnemen.

> [!NOTE]
> In dit voorbeeld passen we verschillende bewerkingen op verschillende documenten toe. Als u dezelfde bewerkingen op alle documenten in de batch wilt uitvoeren, kunt u in plaats van `IndexBatch.New` aan te roepen, een andere statische methode van `IndexBatch` gebruiken. U kunt bijvoorbeeld batches maken door `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, of `IndexBatch.Delete` aan te roepen. Deze methoden maken gebruik van een verzameling van documenten (objecten van het type `Hotel` in dit voorbeeld) in plaats van `IndexAction`-objecten.
> 
> 

## <a name="call-documentsindex"></a>Documents.Index aanroepen
Nu u een geïnitialiseerd `IndexBatch`-object hebt, kunt u het object naar de index sturen door `Documents.Index` aan te roepen op uw `SearchIndexClient`-object. Het volgende voorbeeld laat zien hoe u `Index` kunt aanroepen, plus een aantal extra stappen die u moet uitvoeren:

```csharp
try
{
    indexClient.Documents.Index(batch);
}
catch (IndexBatchException e)
{
    // Sometimes when your Search service is under load, indexing will fail for some of the documents in
    // the batch. Depending on your application, you can take compensating actions like delaying and
    // retrying. For this simple demo, we just log the failed document keys and continue.
    Console.WriteLine(
        "Failed to index some of the documents: {0}",
        String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
}

Console.WriteLine("Waiting for documents to be indexed...\n");
Thread.Sleep(2000);
```

Let op de `try`/`catch` rond de aanroep van de `Index`-methode. Het catch-blok verwerkt een belangrijke foutaanvraag voor indexering. Als uw Azure Search-service geen index van een aantal documenten in de batch kan maken, wordt een `IndexBatchException` verstuurd door `Documents.Index`. Dit kan gebeuren als u documenten indexeert terwijl uw service zwaar wordt belast. **Wij raden u aan deze aanvraag expliciet in uw code te behandelen.** U kunt de indexering van documenten die niet zijn geïndexeerd vertragen en vervolgens opnieuw uitvoeren, maar u kunt ook een logboek maken en doorgaan zoals in het voorbeeld. U kunt ook een andere bewerking uitvoeren, afhankelijk van de vereisten omtrent de gegevensconsistentie van de toepassing.

De code in bovenstaand voorbeeld wordt dan twee seconden vertraagd. Het indexeren verloopt asynchroon in uw Azure Search-service. De voorbeeldtoepassing moet even wachten totdat de documenten beschikbaar zijn voor een zoekbewerking. Dergelijke vertragingen zijn doorgaans alleen nodig is demo’s, testen en voorbeeldtoepassingen.

Zie voor meer informatie over de verwerking van documenten, ["Hoe de .NET SDK omgaat met documenten"](search-howto-dotnet-sdk.md#how-dotnet-handles-documents).


## <a name="next-steps"></a>Volgende stappen
Na het vullen van uw Azure Search-index, is de volgende stap uitgeven van query's om te zoeken naar documenten. 

> [!div class="nextstepaction"]
> [Query uitvoeren op een Azure Search-index inC#](search-query-dotnet.md)
