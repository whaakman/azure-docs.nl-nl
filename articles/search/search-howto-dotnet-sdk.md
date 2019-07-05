---
title: Het gebruik van Azure Search via een .NET-toepassing - Azure Search
description: Meer informatie over het gebruik van Azure Search in een .NET-toepassing met C# en de .NET SDK. Code gebaseerde taken omvatten het verbinding maken met de service, inhoud indexeren, en een index op te vragen.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: brjohnst
ms.openlocfilehash: 9f0af40d442747181636b50612f7d2162ead6a86
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450017"
---
# <a name="how-to-use-azure-search-from-a-net-application"></a>Het gebruik van Azure Search via een .NET-toepassing

In dit artikel is een overzicht te krijgen u actief en werkend de [Azure Search .NET SDK](https://aka.ms/search-sdk). U kunt de .NET SDK gebruiken voor het implementeren van een uitgebreide zoekervaring in uw toepassing met behulp van Azure Search.

## <a name="whats-in-the-azure-search-sdk"></a>Wat is er in de Azure SDK zoeken
De SDK bestaat uit een aantal clientbibliotheken waarmee u uw indexen beheren, gegevensbronnen en indexeerfuncties synoniem kaarten, evenals uploaden en beheren van documenten, en uitvoeren van query's, zonder te hoeven te bekommeren om de details van HTTP en JSON. Deze clientbibliotheken worden gedistribueerd als een NuGet-pakketten.

Het belangrijkste NuGet-pakket is `Microsoft.Azure.Search`, dit is een meta-pakket met de andere pakketten als afhankelijkheden. Dit pakket gebruiken als u net begint of als u weet dat uw toepassing moet alle functies van Azure Search.

De andere NuGet-pakketten in de SDK zijn:
 
  - `Microsoft.Azure.Search.Data`: Dit pakket gebruiken als u een .NET-toepassing met behulp van Azure Search ontwikkelt en u alleen hoeft opvragen of bijwerken van de documenten in uw indexen. Als u ook wilt maken of bijwerken van indexen, synoniementoewijzingen of andere service level-resources, gebruikt de `Microsoft.Azure.Search` in plaats daarvan het pakket.
  - `Microsoft.Azure.Search.Service`: Dit pakket gebruiken als u automation in .NET voor het beheren van Azure Search-index, synoniementoewijzingen, indexeerfuncties, gegevensbronnen of andere bronnen serviceniveau ontwikkelt. Als u alleen query- of update-documenten in uw indexen moet, gebruikt u de `Microsoft.Azure.Search.Data` in plaats daarvan het pakket. Als u de functionaliteit van Azure Search nodig hebt, gebruikt u de `Microsoft.Azure.Search` in plaats daarvan het pakket.
  - `Microsoft.Azure.Search.Common`: Algemene typen die nodig zijn voor de Azure Search .NET-bibliotheken. U hoeft niet te gebruiken van dit pakket rechtstreeks in uw toepassing. Het is alleen bedoeld als een afhankelijkheid moet worden gebruikt.

De clientbibliotheken van verschillende klassen, zoals definiëren `Index`, `Field`, en `Document`, evenals bewerkingen, zoals `Indexes.Create` en `Documents.Search` op de `SearchServiceClient` en `SearchIndexClient` klassen. Deze klassen zijn ingedeeld in de volgende naamruimten:

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

Als u wilt om feedback te geven voor een toekomstige update van de SDK, raadpleegt u onze [feedbackpagina](https://feedback.azure.com/forums/263029-azure-search/) of maken van een probleem op [GitHub](https://github.com/azure/azure-sdk-for-net/issues) en vermeld de 'Azure Search' in de titel van het probleem.

De .NET-SDK biedt ondersteuning voor versie `2019-05-06` van de [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/). Deze versie bevat ondersteuning voor [complexe typen](search-howto-complex-data-types.md), [cognitief zoeken](cognitive-search-concept-intro.md), [automatisch aanvullen](https://docs.microsoft.com/rest/api/searchservice/autocomplete), en [JsonLines parseermodus](search-howto-index-json-blobs.md) wanneer Azure-Blobs indexeren. 

Deze SDK biedt geen ondersteuning voor [beheerbewerkingen](https://docs.microsoft.com/rest/api/searchmanagement/) , zoals het maken en schalen van de Search-services en beheren van API-sleutels. Als u nodig hebt voor het beheren van uw resources zoeken via een .NET-toepassing, kunt u de [Azure Search .NET Management SDK](https://aka.ms/search-mgmt-sdk).

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>Een upgrade naar de nieuwste versie van de SDK
Als u al een oudere versie van de Azure Search .NET SDK gebruikt en u wilt een upgrade uitvoeren naar de meest recente algemeen beschikbare versie [in dit artikel](search-dotnet-sdk-migration-version-9.md) wordt uitgelegd hoe.

## <a name="requirements-for-the-sdk"></a>Vereisten voor de SDK
1. Visual Studio 2017 of later.
2. Uw eigen Azure Search-service. Als u wilt gebruiken in de SDK, moet u de naam van uw service en een of meer API-sleutels. [Maken van een service in de portal](search-create-service-portal.md) helpt u tijdens de volgende stappen.
3. De Azure Search .NET SDK downloaden [NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Search) met behulp van 'NuGet-pakketten beheren' in Visual Studio. Naam van het pakket zoeken `Microsoft.Azure.Search` op NuGet.org (of een van de andere bovenstaande namen verpakken als u alleen een subset van de functionaliteit hoeft).

De Azure Search .NET SDK biedt ondersteuning voor toepassingen die zijn gericht op het .NET Framework 4.5.2 en hoger, ook als .NET Core 2.0 en hoger.

## <a name="core-scenarios"></a>Belangrijkste scenario 's
Er zijn verschillende dingen die u in uw zoektoepassing moet doen. In deze zelfstudie aan de orde deze belangrijkste scenario's:

* Een index te maken
* Invullen van de index met behulp van documenten
* Zoeken naar documenten met behulp van zoeken in volledige tekst en filters

De volgende voorbeeldcode illustreert elk van deze scenario's. U kunt de codefragmenten in uw eigen toepassing gebruiken.

### <a name="overview"></a>Overzicht
De voorbeeldtoepassing die we u worden verkennen maakt u een nieuwe index met de naam "hotels", gevuld met een aantal documenten en bepaalde zoekquery's wordt uitgevoerd. Hier is het belangrijkste programma, met de algehele stroom:

```csharp
// This sample shows how to delete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

    string indexName = configuration["SearchIndexName"];

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteIndexIfExists(indexName, serviceClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateIndex(indexName, serviceClient);

    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);

    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(indexClient);

    ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

    RunQueries(indexClientForQueries);

    Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
    Console.ReadKey();
}
```

> [!NOTE]
> U vindt de volledige broncode van de voorbeeldtoepassing die in deze walkthrough wordt gebruikt, op [GitHub](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).
> 
>

We behandelen deze stapsgewijze instructies. Eerst moeten we maken een nieuwe `SearchServiceClient`. Dit object kunt u voor het beheren van indexen. Het bouwen van een, moet u de naam van uw Azure Search-service, evenals een beheer-API-sleutel opgeven. U kunt deze informatie in de `appsettings.json` -bestand van de [voorbeeldtoepassing](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

```csharp
private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string adminApiKey = configuration["SearchServiceAdminApiKey"];

    SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
    return serviceClient;
}
```

> [!NOTE]
> Als u een onjuiste sleutel (bijvoorbeeld een query waarin geen Administrator-code vereist is), de `SearchServiceClient` genereert een `CloudException` bericht met de fout 'Verboden' de eerste keer dat u de bewerkingsmethode van een, zoals aanroepen `Indexes.Create`. Als dit het geval u, controleer dan onze API-sleutel.
> 
> 

De volgende paar regels aanroepen methoden voor het maken van een index met de naam "hotels", eerst verwijderen als deze al bestaat. We helpen bij deze methoden even later opnieuw.

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteIndexIfExists(indexName, serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateIndex(indexName, serviceClient);
```

Vervolgens moet de index wordt gevuld. Als u wilt de index te vullen, moeten we een `SearchIndexClient`. Er zijn twee manieren een ophalen: door deze of door het aanroepen van `Indexes.GetClient` op de `SearchServiceClient`. We gebruiken deze om u te helpen.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);
```

> [!NOTE]
> In een toepassing standaardzoekopdrachten kunnen beheer van de index en de bevolking worden afgehandeld door een afzonderlijk onderdeel van de zoekquery's. `Indexes.GetClient` is handig voor het invullen van een index, omdat u de moeite van het leveren van aanvullende `SearchCredentials`. Dit wordt uitgevoerd door de administratorsleutel die u hebt gebruikt om de `SearchServiceClient` te maken die u wilt doorgeven aan de nieuwe `SearchIndexClient`. Echter, in het gedeelte van uw toepassing waarmee query's uitvoert, is het beter om te maken de `SearchIndexClient` rechtstreeks zodat u kunt doorgeven in een querysleutel, die alleen kunt u lezen van gegevens, in plaats van een administratorsleutel. Dit is consistent zijn met het principe van minimale bevoegdheden en zorgt ervoor dat uw toepassingen beter te beveiligen. U vindt meer informatie over administratorsleutels en querysleutels [hier](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).
> 
> 

Nu dat we hebben een `SearchIndexClient`, kunnen we de index te vullen. Populatie van de index wordt uitgevoerd door een andere methode die we later bekijken.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Ten slotte we enkele zoekopdrachten uitvoeren en de resultaten weer te geven. Nu we gebruiken een andere `SearchIndexClient`:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(indexName, configuration);

RunQueries(indexClientForQueries);
```

We zullen de eens nader bekijken de `RunQueries` methode later opnieuw. Hier volgt de code voor het maken van de nieuwe `SearchIndexClient`:

```csharp
private static SearchIndexClient CreateSearchIndexClient(string indexName, IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, indexName, new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Nu we een querysleutel gebruiken omdat we niet de toegang voor schrijven naar de index hoeft. U kunt deze informatie in de `appsettings.json` -bestand van de [voorbeeldtoepassing](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

Als u deze toepassing met een geldige servicenaam en API-sleutels uitvoert, wordt de uitvoer moet eruitzien zoals in dit voorbeeld: (Sommige console-uitvoer is vervangen door '...' ter illustratie wordt.)

    Deleting index...

    Creating index...

    Uploading documents...

    Waiting for documents to be indexed...

    Search the entire index for the term 'motel' and return only the HotelName field:

    Name: Secret Point Motel

    Name: Twin Dome Motel


    Apply a filter to the index to find hotels with a room cheaper than $100 per night, and return the hotelId and description:

    HotelId: 1
    Description: The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Times Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.

    HotelId: 2
    Description: The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.


    Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:

    Name: Triple Landscape Hotel
    Last renovated on: 9/20/2015 12:00:00 AM +00:00

    Name: Twin Dome Motel
    Last renovated on: 2/18/1979 12:00:00 AM +00:00


    Search the hotel names for the term 'hotel':

    HotelId: 3
    Name: Triple Landscape Hotel
    ...

    Complete.  Press any key to end application... 

De volledige broncode van de toepassing wordt geleverd aan het einde van dit artikel.

Vervolgens, we zullen eens nader bekijken voor elk van de methoden aangeroepen door `Main`.

### <a name="creating-an-index"></a>Een index te maken
Na het maken van een `SearchServiceClient`, `Main` Hiermee verwijdert u de index "hotels" Als deze al bestaat. Deze verwijdering wordt uitgevoerd door de volgende methode:

```csharp
private static void DeleteIndexIfExists(string indexName, SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists(indexName))
    {
        serviceClient.Indexes.Delete(indexName);
    }
}
```

Deze methode maakt gebruik van de opgegeven `SearchServiceClient` om te controleren of de index bestaat, en zo ja, deze verwijderen.

> [!NOTE]
> In de voorbeeldcode in dit artikel is voor het gemak gebruikgemaakt van de synchrone methoden van de Azure Search .NET SDK. Het wordt aanbevolen in uw eigen toepassingen asynchrone methoden te gebruiken, zodat de toepassingen schaalbaar zijn en goed reageren. Bijvoorbeeld, in de bovenstaande methode kan u `ExistsAsync` en `DeleteAsync` in plaats van `Exists` en `Delete`.
> 
> 

Volgende `Main` maakt u een nieuwe index "hotels" met deze methode aanroept:

```csharp
private static void CreateIndex(string indexName, SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = indexName,
        Fields = FieldBuilder.BuildForType<Hotel>()
    };
    
    serviceClient.Indexes.Create(definition);
}
```

Deze methode maakt u een nieuwe `Index` object met een lijst met `Field` objecten die het schema van de nieuwe index definieert. Elk veld heeft een naam, gegevenstype en verschillende kenmerken die het zoekgedrag te definiëren. De `FieldBuilder` klasse reflectie gebruikt voor het maken van een lijst met `Field` objecten voor de index door een onderzoek van de openbare eigenschappen en kenmerken van de opgegeven `Hotel` modelklasse. We werpen eens nader bekijken de `Hotel` klasse later op.

> [!NOTE]
> U kunt altijd maken met de lijst met `Field` objecten rechtstreeks in plaats van `FieldBuilder` indien nodig. Bijvoorbeeld, u niet wilt gebruiken een modelklasse of moet u mogelijk gebruik van een bestaande modelklasse die u niet wijzigen wilt door de kenmerken toe te voegen.
>
> 

Naast de velden, kunt u ook scoreprofielen, suggesties en CORS-opties toevoegen aan de Index (deze parameters worden weggelaten uit het voorbeeld kort te houden). U vindt meer informatie over de Index-object en de bijbehorende onderdelen in de [SDK-referentie](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index), alsmede in de [Azure Search REST API-verwijzingen](https://docs.microsoft.com/rest/api/searchservice/).

### <a name="populating-the-index"></a>De index vullen
De volgende stap in de `Main` vult de zojuist gemaakte index. De populatie van deze index wordt uitgevoerd in de volgende methode: (Code wordt vervangen door '...' ter illustratie wordt.  Zie de volledige oplossing voor de volledige gegevens populatie-code.)

```csharp
private static void UploadDocuments(ISearchIndexClient indexClient)
{
    var hotels = new Hotel[]
    {
        new Hotel()
        {
            HotelId = "1",
            HotelName = "Secret Point Motel",
            ...
            Address = new Address()
            {
                StreetAddress = "677 5th Ave",
                ...
            },
            Rooms = new Room[]
            {
                new Room()
                {
                    Description = "Budget Room, 1 Queen Bed (Cityside)",
                    ...
                },
                new Room()
                {
                    Description = "Budget Room, 1 King Bed (Mountain View)",
                    ...
                },
                new Room()
                {
                    Description = "Deluxe Room, 2 Double Beds (City View)",
                    ...
                }
            }
        },
        new Hotel()
        {
            HotelId = "2",
            HotelName = "Twin Dome Motel",
            ...
            {
                StreetAddress = "140 University Town Center Dr",
                ...
            },
            Rooms = new Room[]
            {
                new Room()
                {
                    Description = "Suite, 2 Double Beds (Mountain View)",
                    ...
                },
                new Room()
                {
                    Description = "Standard Room, 1 Queen Bed (City View)",
                    ...
                },
                new Room()
                {
                    Description = "Budget Room, 1 King Bed (Waterfront View)",
                    ...
                }
            }
        },
        new Hotel()
        {
            HotelId = "3",
            HotelName = "Triple Landscape Hotel",
            ...
            Address = new Address()
            {
                StreetAddress = "3393 Peachtree Rd",
                ...
            },
            Rooms = new Room[]
            {
                new Room()
                {
                    Description = "Standard Room, 2 Queen Beds (Amenities)",
                    ...
                },
                new Room ()
                {
                    Description = "Standard Room, 2 Double Beds (Waterfront View)",
                    ...
                },
                new Room()
                {
                    Description = "Deluxe Room, 2 Double Beds (Cityside)",
                    ...
                }
            }
        }
    };

    var batch = IndexBatch.Upload(hotels);

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
}
```

Deze methode heeft vier onderdelen. De eerste maakt u een matrix van 3 `Hotel` allemaal zijn voorzien van 3-objecten `Room` objecten die als onze invoergegevens om te uploaden naar de index moeten fungeren. Deze gegevens zijn vastgelegd voor het gemak. In uw eigen toepassing, worden uw gegevens waarschijnlijk afkomstig zijn van een externe gegevensbron, zoals een SQL-database.

Het tweede gedeelte maakt u een `IndexBatch` met de documenten. Geeft u de bewerking die u toepassen op de batch op het moment dat u, in dit geval maakt wilt door het aanroepen van `IndexBatch.Upload`. De batch wordt vervolgens geüpload naar de Azure Search-index met de `Documents.Index` methode.

> [!NOTE]
> In dit voorbeeld zijn we alleen documenten te uploaden. Als u wijzigingen in bestaande documenten samen te voegen of te verwijderen van documenten wilt, kunt u batches maken door het aanroepen van `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, of `IndexBatch.Delete` in plaats daarvan. U kunt ook verschillende bewerkingen in een enkel batchproces combineren door het aanroepen van `IndexBatch.New`, die gebruikt een verzameling `IndexAction` objecten, die elk Azure Search om uit te voeren van een bepaalde bewerking op een document wordt uitgelegd. U kunt elk maken `IndexAction` met een eigen bewerking door het aanroepen van de bijbehorende methode zoals `IndexAction.Merge`, `IndexAction.Upload`, enzovoort.
> 
> 

Het derde deel van deze methode is een catch-blok die verantwoordelijk is voor een belangrijke foutaanvraag voor indexering. Als uw Azure Search-service geen index van een aantal documenten in de batch kan maken, wordt een `IndexBatchException` verstuurd door `Documents.Index`. Deze uitzondering kan gebeuren als u documenten indexeert terwijl uw service zwaar belast wordt. **Wij raden u aan deze aanvraag expliciet in uw code te behandelen.** U kunt de indexering van documenten die niet zijn geïndexeerd vertragen en vervolgens opnieuw uitvoeren, maar u kunt ook een logboek maken en doorgaan zoals in het voorbeeld. U kunt ook een andere bewerking uitvoeren, afhankelijk van de vereisten omtrent de gegevensconsistentie van de toepassing.

> [!NOTE]
> U kunt de [ `FindFailedActionsToRetry` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception.findfailedactionstoretry) methode voor het maken van een nieuwe batch met alleen de acties die niet in een vorige aanroep aan `Index`. Er is een discussie over het gebruik ervan goed [op StackOverflow](https://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).
>
>

Ten slotte de `UploadDocuments` methode twee seconden vertraagd. Het indexeren verloopt asynchroon in uw Azure Search-service. De voorbeeldtoepassing moet even wachten totdat de documenten beschikbaar zijn voor een zoekbewerking. Dergelijke vertragingen zijn doorgaans alleen nodig is demo’s, testen en voorbeeldtoepassingen.

<a name="how-dotnet-handles-documents"></a>

#### <a name="how-the-net-sdk-handles-documents"></a>De verwerking van documenten door .NET SDK
U vraagt zich misschien af hoe de Azure Search .NET SDK instanties van een door een gebruiker gedefinieerde klasse zoals `Hotel` naar de index kan uploaden. Als u wilt deze vraag te beantwoorden, we kijken naar de `Hotel` klasse:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsSearchable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.EnLucene)]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("Description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    // SmokingAllowed reflects whether any room in the hotel allows smoking.
    // The JsonIgnore attribute indicates that a field should not be created 
    // in the index for this property and it will only be used by code in the client.
    [JsonIgnore]
    public bool? SmokingAllowed => (Rooms != null) ? Array.Exists(Rooms, element => element.SmokingAllowed == true) : (bool?)null;

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? Rating { get; set; }

    public Address Address { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }

    public Room[] Rooms { get; set; }
}
```

Het eerste dat opvalt is dat de naam van elke openbare eigenschap in de `Hotel` klasse wordt toegewezen aan een veld met dezelfde naam in het definitie van de index. Als u elk veld om te beginnen met een kleine letter ("kamelen"), kunt u instellen dat de SDK de namen van eigenschappen toewijzen aan camelcase-automatisch met de `[SerializePropertyNamesAsCamelCase]` kenmerk voor de klasse. In dit scenario is gebruikelijk in .NET-toepassingen die gegevens koppelen waarbij het doelschema is buiten het besturingselement van de ontwikkelaar van de toepassing zonder dat het schenden van de "Pascal aanvraag' naamgevingsregels in .NET.

> [!NOTE]
> De Azure Search .NET SDK maakt gebruik van de [NewtonSoft JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm)-bibliotheek voor het serialiseren en deserialiseren van uw aangepaste modelobjecten naar en van JSON. U kunt deze serialisatie indien nodig aanpassen. Zie voor meer informatie, [aangepaste serialisatie met JSON.NET](#JsonDotNet).
> 
> 

De tweede dat opvalt, is elke eigenschap is voorzien van kenmerken zoals `IsFilterable`, `IsSearchable`, `Key`, en `Analyzer`. Deze kenmerken wijzen rechtstreeks naar de [overeenkomende veld kenmerken in een Azure Search-index](https://docs.microsoft.com/rest/api/searchservice/create-index#request). De `FieldBuilder` klasse maakt gebruik van deze eigenschappen velddefinities voor de index te maken.

Het derde belangrijkste over de `Hotel` klasse is de gegevenstypen van de openbare eigenschappen. De .NET-typen van deze eigenschappen worden toegewezen aan de gelijkwaardige veldtypen in de definitie van de index. De tekenreekseigenschap `Category` is bijvoorbeeld toegewezen aan het veld `category` van type `Edm.String`. Er zijn vergelijkbare type toewijzingen tussen `bool?`, `Edm.Boolean`, `DateTimeOffset?`, en `Edm.DateTimeOffset` enzovoort. De specifieke regels voor de toewijzing van het type worden gedocumenteerd met de methode `Documents.Get` in de [Azure Search .NET SDK-verwijzing](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.get). De `FieldBuilder` klasse zorgt dat deze toewijzing voor u, maar kan nog steeds nuttig om te begrijpen als u het oplossen van serialisatieproblemen zijn.

Hebt u per ongeluk ziet u de `SmokingAllowed` eigenschap?

```csharp
[JsonIgnore]
public bool? SmokingAllowed => (Rooms != null) ? Array.Exists(Rooms, element => element.SmokingAllowed == true) : (bool?)null;
```

De `JsonIgnore` kenmerk voor deze eigenschap vertelt de `FieldBuilder` aan niet serialiseren naar de index als een veld.  Dit is een uitstekende manier om client-side berekende eigenschappen die u als hulpprogramma's in uw toepassing gebruiken kunt te maken.  In dit geval de `SmokingAllowed` eigenschap weerspiegelt of willekeurig welke `Room` in de `Rooms` verzameling kunt soorten.  Als alle ingesteld op false zijn, betekent dit dat het hele hotel soorten niet toelaat.

Sommige eigenschappen zoals `Address` en `Rooms` zijn exemplaren van .NET-klassen.  Deze eigenschappen vertegenwoordigen meer complexe gegevensstructuren en, als gevolg hiervan moet velden met een [complex gegevenstype](https://docs.microsoft.com/azure/search/search-howto-complex-data-types) in de index.

De `Address` eigenschap vertegenwoordigt een set met meerdere waarden in de `Address` klasse, hieronder gedefinieerd:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Newtonsoft.Json;

namespace AzureSearch.SDKHowTo
{
    public partial class Address
    {
        [IsSearchable]
        public string StreetAddress { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string City { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string StateProvince { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string PostalCode { get; set; }

        [IsSearchable, IsFilterable, IsSortable, IsFacetable]
        public string Country { get; set; }
    }
}
```

Deze klasse bevat de standaard waarden gebruikt voor het beschrijven van adressen in de Verenigde Staten of Canada. Typen zoals dit kunt u logische velden groeperen in de index.

De `Rooms` eigenschap vertegenwoordigt een reeks `Room` objecten:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Newtonsoft.Json;

namespace AzureSearch.SDKHowTo
{
    public partial class Room
    {
        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.EnMicrosoft)]
        public string Description { get; set; }

        [IsSearchable]
        [Analyzer(AnalyzerName.AsString.FrMicrosoft)]
        [JsonProperty("Description_fr")]
        public string DescriptionFr { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string Type { get; set; }

        [IsFilterable, IsFacetable]
        public double? BaseRate { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string BedOptions { get; set; }

        [IsFilterable, IsFacetable]
        public int SleepsCount { get; set; }

        [IsFilterable, IsFacetable]
        public bool? SmokingAllowed { get; set; }

        [IsSearchable, IsFilterable, IsFacetable]
        public string[] Tags { get; set; }
    }
}
```

Uw gegevensmodel in .NET en de bijbehorende indexschema moet worden ontworpen ter ondersteuning van de zoekervaring die u wilt toewijzen aan de eindgebruiker. Elk object op het hoogste niveau in .NET, bijvoorbeeld een document in de index, komt overeen met een zoekresultaat u die aanwezig in de gebruikersinterface zijn zou. Bijvoorbeeld in een hotel-zoektoepassing uw eindgebruikers mogelijk wilt zoeken op naam van het hotel kenmerken van het hotel, of de kenmerken van een specifieke ruimte. Aan de orde queryvoorbeelden iets later opnieuw.

Deze mogelijkheid om te gebruiken van uw eigen klassen om te communiceren met documenten in de index werkt in beide richtingen; U kunt ook zoekresultaten ophalen en hebben de SDK automatisch deserialiseert naar een type van uw keuze, zoals we in de volgende sectie zien.

> [!NOTE]
> De Azure Search .NET SDK biedt ook ondersteuning voor dynamisch getypeerde documenten met behulp van de `Document`-klasse, die een sleutel/waarde-toewijst aan veldnamen naar waarden. Dit is handig in situaties waar u het schema van de index op het moment van ontwerp nog niet weet of wanneer het niet handig zou zijn om verbinding te maken met specifieke modelklassen Alle methoden in de SDK die werken met documenten hebben overloads die met werken de `Document`-klasse, evenals sterk getypeerde overloads die een generiek typeparameter moeten uitvoeren. Alleen de laatste worden gebruikt in de voorbeeldcode in deze zelfstudie. De [ `Document` klasse](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document) neemt over van `Dictionary<string, object>`.
> 
>

**Waarom u nullable-gegevenstypen moet gebruiken**

Bij het ontwerpen van uw eigen modelklassen die u wilt toewijzen aan een Azure Search-index raden we u aan om eigenschappen van waardentypen `bool` en `int` in te stellen op null-waarden (bijvoorbeeld `bool?` in plaats van `bool`). Als u een niet-nullbare eigenschap gebruikt, moet u **garanderen** dat de documenten in de index geen null-waarde voor het betreffende veld bevatten. Noch de SDK noch de Azure Search-service helpt u om dit af te dwingen.

Dit is niet alleen een hypothetisch probleem: Stelt u zich een scenario voor waarin u een nieuw veld toevoegt aan een bestaande index van het type `Edm.Int32`. Na het bijwerken van de indexdefinitie hebben alle documenten een null-waarde voor het nieuwe veld (omdat alle typen null in Azure Search zijn). Als u vervolgens een modelklasse met een niet-nullbare `int`-eigenschap voor dat veld gebruikt, ontvangt u een `JsonSerializationException` zoals deze bij het ophalen van documenten:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Daarom wordt u aangeraden nullbare typen in uw modelklassen te gebruiken.

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a>Aangepaste serialisatie met JSON.NET
De SDK gebruikt JSON.NET voor het serialiseren en deserialiseren van documenten. U kunt aanpassen serialisatie en deserialisatie indien nodig door het definiëren van uw eigen `JsonConverter` of `IContractResolver`. Zie voor meer informatie de [JSON.NET documentatie](https://www.newtonsoft.com/json/help/html/Introduction.htm). Dit kan nuttig zijn wanneer u een bestaande modelklasse van uw toepassing voor gebruik met Azure Search en andere meer geavanceerde scenario's aan te passen. Met aangepaste serialisatie kunt u bijvoorbeeld:

* Opnemen of uitsluiten van bepaalde eigenschappen van uw modelklasse als documentvelden worden opgeslagen.
* Koppeling tussen de namen van eigenschappen in uw code en veldnamen in uw index.
* Aangepaste kenmerken die kunnen worden gebruikt voor de toewijzing van eigenschappen voor documentvelden maken.

Hier vindt u voorbeelden van de implementatie van aangepaste serialisatie in de eenheidstests voor de Azure Search .NET SDK op GitHub. Is een goed uitgangspunt [deze map](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). Deze bevat klassen die worden gebruikt door de aangepaste serialisatie-tests.

### <a name="searching-for-documents-in-the-index"></a>Zoeken naar documenten in de index
De laatste stap in de voorbeeldtoepassing is om te zoeken naar een aantal documenten in de index:

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
    SearchParameters parameters;
    DocumentSearchResult<Hotel> results;

    Console.WriteLine("Search the entire index for the term 'motel' and return only the HotelName field:\n");

    parameters =
        new SearchParameters()
        {
            Select = new[] { "HotelName" }
        };

    results = indexClient.Documents.Search<Hotel>("motel", parameters);

    WriteDocuments(results);

    Console.Write("Apply a filter to the index to find hotels with a room cheaper than $100 per night, ");
    Console.WriteLine("and return the hotelId and description:\n");

    parameters =
        new SearchParameters()
        {
            Filter = "Rooms/any(r: r/BaseRate lt 100)",
            Select = new[] { "HotelId", "Description" }
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
    Console.Write("in descending order, take the top two results, and show only hotelName and ");
    Console.WriteLine("lastRenovationDate:\n");

    parameters =
        new SearchParameters()
        {
            OrderBy = new[] { "LastRenovationDate desc" },
            Select = new[] { "HotelName", "LastRenovationDate" },
            Top = 2
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.WriteLine("Search the entire index for the term 'hotel':\n");

    parameters = new SearchParameters();
    results = indexClient.Documents.Search<Hotel>("hotel", parameters);

    WriteDocuments(results);
}
```

Elke keer dat deze een query wordt uitgevoerd, met deze methode maakt eerst een nieuwe `SearchParameters` object. Dit object wordt gebruikt om op te geven aanvullende opties voor de query, zoals sorteren, filteren, wisselbestand en op meerdere niveaus. Bij deze methode instellen we de `Filter`, `Select`, `OrderBy`, en `Top` eigenschap voor verschillende query's. Alle de `SearchParameters` eigenschappen zijn gedocumenteerd [hier](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).

De volgende stap is het daadwerkelijk uitvoeren van de zoekquery. Uitvoeren van de zoekopdracht wordt uitgevoerd met behulp van de `Documents.Search` methode. Voor elke query, geven we de zoektekst te gebruiken als een tekenreeks (of `"*"` als er geen zoektekst), plus de zoekparameters eerder hebt gemaakt. We ook opgeven `Hotel` als het typeparameter voor `Documents.Search`, waarin staat dat de SDK voor het deserialiseren van documenten in de lijst met zoekresultaten in objecten van het type `Hotel`.

> [!NOTE]
> U vindt meer informatie over de zoeksyntaxis van de query-expressie [hier](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).
> 
> 

Ten slotte na elke query doorloopt met deze methode alle overeenkomsten in de lijst met zoekresultaten, elk document naar de console afdrukken:

```csharp
private static void WriteDocuments(DocumentSearchResult<Hotel> searchResults)
{
    foreach (SearchResult<Hotel> result in searchResults.Results)
    {
        Console.WriteLine(result.Document);
    }

    Console.WriteLine();
}
```

We gaan bespreken eens nader bekijken elk van de query's. Dit is de code de eerste query uit te voeren:

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "HotelName" }
    };

results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

In dit geval we de gehele index voor het woord "motel" zoeken in elk doorzoekbaar veld en we alleen wilt ophalen van het hotel namen, zoals opgegeven door de `Select` parameter. Hier volgen de resultaten:

    Name: Secret Point Motel

    Name: Twin Dome Motel

De volgende query is iets interessanter.  We willen vinden alle hotels die beschikken over een lokaal een 's nachts snelheid van minder dan $100 en terug te keren alleen de hotel-ID en -beschrijving:

```csharp
parameters =
    new SearchParameters()
    {
        Filter = "Rooms/any(r: r/BaseRate lt 100)",
        Select = new[] { "HotelId", "Description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

Deze query maakt gebruik van een OData `$filter` expressie `Rooms/any(r: r/BaseRate lt 100)`, voor het filteren van de documenten in de index. Dit maakt gebruik van de [een operator](https://docs.microsoft.com/azure/search/search-query-odata-collection-operators) om toe te passen de ' BaseRate lt 100' op elk item in de verzameling ruimten. U vindt meer informatie over de OData-syntaxis die ondersteuning biedt voor Azure Search [hier](https://docs.microsoft.com/azure/search/query-odata-filter-orderby-syntax).

Hier volgen de resultaten van de query:

    HotelId: 1
    Description: The hotel is ideally located on the main commercial artery of the city in the heart of New York...

    HotelId: 2
    Description: The hotel is situated in a nineteenth century plaza, which has been expanded and renovated to...

We willen vervolgens de bovenste twee hotels die onlangs zijn renovated zoeken en weergeven van het hotelnaam en datum van laatste renovatie. Dit is de code: 

```csharp
parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "LastRenovationDate desc" },
        Select = new[] { "HotelName", "LastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

In dit geval we opnieuw OData-syntaxis gebruiken om op te geven de `OrderBy` parameter als `lastRenovationDate desc`. We ook ingesteld `Top` op 2 om ervoor te zorgen we alleen de twee bovenste documenten ophalen. Als voorheen, we stellen `Select` om op te geven welke velden moeten worden geretourneerd.

Hier volgen de resultaten:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Tot slot gaan we alle hotels namen die overeenkomen met het woord "Hotels" zoeken:

```csharp
parameters = new SearchParameters()
{
    SearchFields = new[] { "HotelName" }
};
results = indexClient.Documents.Search<Hotel>("hotel", parameters);

WriteDocuments(results);
```

En hier vindt u de resultaten van alle velden bevatten omdat er is geen opgegeven voor de `Select` eigenschap:

    HotelId: 3
    Name: Triple Landscape Hotel
    ...

Deze stap wordt de zelfstudie voltooid, maar hier niet stoppen. ** De volgende stappen bevatten aanvullende bronnen voor meer informatie over Azure Search.

## <a name="next-steps"></a>Volgende stappen
* Neem de referenties voor de [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) en [REST API](https://docs.microsoft.com/rest/api/searchservice/) door.
* Beoordeling [naamconventies](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) voor meer informatie over de regels voor het benoemen van verschillende objecten.
* Beoordeling [ondersteunde gegevenstypen](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) in Azure Search.
