---
title: Het gebruik van Azure Search via een .NET-toepassing - Azure Search
description: Meer informatie over het gebruik van Azure Search in een .NET-toepassing met C# en de .NET SDK. Code gebaseerde taken omvatten het verbinding maken met de service, inhoud indexeren, en een index op te vragen.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 04/20/2018
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 6f263511a7d1df4af82a690c1d6b04fecd2a8a91
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53634538"
---
# <a name="how-to-use-azure-search-from-a-net-application"></a>Het gebruik van Azure Search via een .NET-toepassing
In dit artikel is een overzicht te krijgen u actief en werkend de [Azure Search .NET SDK](https://aka.ms/search-sdk). U kunt de .NET SDK gebruiken voor het implementeren van een uitgebreide zoekervaring in uw toepassing met behulp van Azure Search.

## <a name="whats-in-the-azure-search-sdk"></a>Wat is er in de Azure SDK zoeken
De SDK bestaat uit een aantal clientbibliotheken waarmee u uw indexen beheren, gegevensbronnen en indexeerfuncties synoniem kaarten, evenals uploaden en beheren van documenten, en uitvoeren van query's, zonder te hoeven te bekommeren om de details van HTTP en JSON. Deze clientbibliotheken worden gedistribueerd als een NuGet-pakketten.

Het belangrijkste NuGet-pakket is `Microsoft.Azure.Search`, dit is een meta-pakket met de andere pakketten als afhankelijkheden. Dit pakket gebruiken als u net begint of als u weet dat uw toepassing moet alle functies van Azure Search.

De andere NuGet-pakketten in de SDK zijn:
 
  - `Microsoft.Azure.Search.Data`: Dit pakket gebruiken als u een .NET-toepassing met behulp van Azure Search ontwikkelt en u alleen hoeft opvragen of bijwerken van de documenten in uw indexen. Als u ook wilt maken of bijwerken van indexen, synoniementoewijzingen of andere service level-resources, gebruikt de `Microsoft.Azure.Search` in plaats daarvan het pakket.
  - `Microsoft.Azure.Search.Service`: Dit pakket gebruiken als u automation in .NET voor het beheren van Azure Search-index, synoniementoewijzingen, indexeerfuncties, gegevensbronnen of andere bronnen serviceniveau ontwikkelt. Als u alleen query- of update-documenten in uw indexen moet, gebruikt u de `Microsoft.Azure.Search.Data` in plaats daarvan het pakket. Als u de functionaliteit van Azure Search nodig hebt, gebruikt u de `Microsoft.Azure.Search` in plaats daarvan het pakket.
  - `Microsoft.Azure.Search.Common`: Algemene typen die nodig zijn voor de Azure Search .NET-bibliotheken. U hoeft niet op het gebruik van dit pakket rechtstreeks in uw toepassing. Het is alleen bedoeld als een afhankelijkheid moet worden gebruikt.

De clientbibliotheken van verschillende klassen, zoals definiëren `Index`, `Field`, en `Document`, evenals bewerkingen, zoals `Indexes.Create` en `Documents.Search` op de `SearchServiceClient` en `SearchIndexClient` klassen. Deze klassen zijn ingedeeld in de volgende naamruimten:

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

De huidige versie van de Azure Search .NET SDK is nu algemeen beschikbaar. Als u feedback geven voor ons om op te nemen in de volgende versie wilt, gaat u naar onze [feedbackpagina](https://feedback.azure.com/forums/263029-azure-search/).

De .NET-SDK biedt ondersteuning voor versie `2017-11-11` van de [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/). Deze versie biedt nu ondersteuning voor synoniemen, evenals incrementele verbeteringen aan indexeerfuncties. Preview-functies die zijn *niet* deel van deze versie, zoals ondersteuning voor het indexeren van JSON-matrices en CSV-bestanden, worden [preview](search-api-2016-09-01-preview.md) en beschikbaar zijn via [4.0-preview-versie van de .NET SDK](https://aka.ms/search-sdk-preview).

Deze SDK biedt geen ondersteuning voor [beheerbewerkingen](https://docs.microsoft.com/rest/api/searchmanagement/) , zoals het maken en schalen van de Search-services en beheren van API-sleutels. Als u nodig hebt voor het beheren van uw resources zoeken via een .NET-toepassing, kunt u de [Azure Search .NET Management SDK](https://aka.ms/search-mgmt-sdk).

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>Een upgrade naar de nieuwste versie van de SDK
Als u al een oudere versie van de Azure Search .NET SDK gebruikt en u wilt upgraden naar de nieuwe algemeen beschikbare versie [in dit artikel](search-dotnet-sdk-migration-version-5.md) wordt uitgelegd hoe.

## <a name="requirements-for-the-sdk"></a>Vereisten voor de SDK
1. Visual Studio 2017.
2. Uw eigen Azure Search-service. Als u wilt gebruiken in de SDK, moet u de naam van uw service en een of meer API-sleutels. [Maken van een service in de portal](search-create-service-portal.md) helpt u tijdens de volgende stappen.
3. De Azure Search .NET SDK downloaden [NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.Search) met behulp van 'NuGet-pakketten beheren' in Visual Studio. Naam van het pakket zoeken `Microsoft.Azure.Search` op NuGet.org (of een van de andere bovenstaande namen verpakken als u alleen een subset van de functionaliteit hoeft).

De Azure Search .NET SDK biedt ondersteuning voor toepassingen die zijn gericht op het .NET Framework 4.5.2 en hoger, en .NET Core.

## <a name="core-scenarios"></a>Belangrijkste scenario 's
Er zijn verschillende dingen die u in uw zoektoepassing moet doen. In deze zelfstudie aan de orde deze belangrijkste scenario's:

* Een index te maken
* Invullen van de index met behulp van documenten
* Zoeken naar documenten met behulp van zoeken in volledige tekst en filters

De volgende voorbeeldcode ziet u elk van deze. U kunt de codefragmenten in uw eigen toepassing gebruiken.

### <a name="overview"></a>Overzicht
De voorbeeldtoepassing die we u worden verkennen maakt u een nieuwe index met de naam "hotels", gevuld met een aantal documenten en bepaalde zoekquery's wordt uitgevoerd. Hier is het belangrijkste programma, met de algehele stroom:

```csharp
// This sample shows how to delete, create, upload documents and query an index
static void Main(string[] args)
{
    IConfigurationBuilder builder = new ConfigurationBuilder().AddJsonFile("appsettings.json");
    IConfigurationRoot configuration = builder.Build();

    SearchServiceClient serviceClient = CreateSearchServiceClient(configuration);

    Console.WriteLine("{0}", "Deleting index...\n");
    DeleteHotelsIndexIfExists(serviceClient);

    Console.WriteLine("{0}", "Creating index...\n");
    CreateHotelsIndex(serviceClient);

    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");

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
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

Vervolgens moet de index wordt gevuld. Om dit te doen, moeten we een `SearchIndexClient`. Er zijn twee manieren een ophalen: door deze of door het aanroepen van `Indexes.GetClient` op de `SearchServiceClient`. We gebruiken deze om u te helpen.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> In een standaardzoektoepassing wordt het beheer van de index en de invulling daarvan afgehandeld door een afzonderlijk onderdeel van de zoekquery’s. `Indexes.GetClient` is handig voor het vullen van een index, omdat u geen andere `SearchCredentials` hoeft op te geven. Dit wordt uitgevoerd door de administratorsleutel die u hebt gebruikt om de `SearchServiceClient` te maken die u wilt doorgeven aan de nieuwe `SearchIndexClient`. In het gedeelte van uw toepassing waarmee u query's uitvoert, is het beter om de `SearchIndexClient` direct te maken, zodat u een querysleutel kunt toepassen in plaats van een administratorsleutel. Dit is consistent zijn met het principe van minimale bevoegdheden en zorgt ervoor dat uw toepassingen beter te beveiligen. U vindt meer informatie over administratorsleutels en querysleutels [hier](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).
> 
> 

Nu dat we hebben een `SearchIndexClient`, kunnen we de index te vullen. Dit wordt gedaan door een andere methode die we later bekijken.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Ten slotte we enkele zoekopdrachten uitvoeren en de resultaten weer te geven. Nu we gebruiken een andere `SearchIndexClient`:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

We zullen de eens nader bekijken de `RunQueries` methode later opnieuw. Hier volgt de code voor het maken van de nieuwe `SearchIndexClient`:

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Nu we een querysleutel gebruiken omdat we niet de toegang voor schrijven naar de index hoeft. U kunt deze informatie in de `appsettings.json` -bestand van de [voorbeeldtoepassing](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

Als u deze toepassing met een geldige servicenaam en API-sleutels uitvoert, moet de uitvoer er als volgt:

    Deleting index...
    
    Creating index...
    
    Uploading documents...
    
    Waiting for documents to be indexed...
    
    Search the entire index for the term 'budget' and return only the hotelName field:
    
    Name: Roach Motel
    
    Apply a filter to the index to find hotels cheaper than $150 per night, and return the hotelId and description:
    
    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river
    
    Search the entire index, order by a specific field (lastRenovationDate) in descending order, take the top two results, and show only hotelName and lastRenovationDate:
    
    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00
    
    Search the entire index for the term 'motel':
    
    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577
    
    Complete.  Press any key to end application...

De volledige broncode van de toepassing wordt geleverd aan het einde van dit artikel.

Vervolgens, we zullen eens nader bekijken voor elk van de methoden aangeroepen door `Main`.

### <a name="creating-an-index"></a>Een index te maken
Na het maken van een `SearchServiceClient`, het volgende wat `Main` biedt verwijderen is de index "hotels" Als deze al bestaat. Dat is gebeurd door de volgende methode:

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
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
private static void CreateHotelsIndex(SearchServiceClient serviceClient)
{
    var definition = new Index()
    {
        Name = "hotels",
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

Naast de velden, kunt u ook scoreprofielen, suggesties en CORS-opties toevoegen aan de Index (deze worden weggelaten uit het voorbeeld kort te houden). U vindt meer informatie over de Index-object en de bijbehorende onderdelen in de [SDK-referentie](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index), alsmede in de [Azure Search REST API-verwijzingen](https://docs.microsoft.com/rest/api/searchservice/).

### <a name="populating-the-index"></a>De index vullen
De volgende stap in de `Main` is om de zojuist gemaakte index te vullen. Dit doet u in de volgende methode:

```csharp
private static void UploadDocuments(ISearchIndexClient indexClient)
{
    var hotels = new Hotel[]
    {
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
        },
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
        },
        new Hotel() 
        { 
            HotelId = "3", 
            BaseRate = 129.99,
            Description = "Close to town hall and the river"
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

Deze methode heeft vier onderdelen. De eerste maakt een matrix van `Hotel` objecten die als onze invoergegevens om te uploaden naar de index moeten fungeren. Deze gegevens zijn vastgelegd voor het gemak. In uw eigen toepassing, worden uw gegevens waarschijnlijk afkomstig zijn van een externe gegevensbron, zoals een SQL-database.

Het tweede gedeelte maakt u een `IndexBatch` met de documenten. Geeft u de bewerking die u toepassen op de batch op het moment dat u, in dit geval maakt wilt door het aanroepen van `IndexBatch.Upload`. De batch wordt vervolgens geüpload naar de Azure Search-index met de `Documents.Index` methode.

> [!NOTE]
> In dit voorbeeld zijn we alleen documenten te uploaden. Als u wijzigingen in bestaande documenten samen te voegen of te verwijderen van documenten wilt, kunt u batches maken door het aanroepen van `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, of `IndexBatch.Delete` in plaats daarvan. U kunt ook verschillende bewerkingen in een enkel batchproces combineren door het aanroepen van `IndexBatch.New`, die gebruikt een verzameling `IndexAction` objecten, die elk Azure Search om uit te voeren van een bepaalde bewerking op een document wordt uitgelegd. U kunt elk maken `IndexAction` met een eigen bewerking door het aanroepen van de bijbehorende methode zoals `IndexAction.Merge`, `IndexAction.Upload`, enzovoort.
> 
> 

Het derde deel van deze methode is een catch-blok die verantwoordelijk is voor een belangrijke foutaanvraag voor indexering. Als uw Azure Search-service geen index van een aantal documenten in de batch kan maken, wordt een `IndexBatchException` verstuurd door `Documents.Index`. Dit kan gebeuren als u documenten indexeert terwijl uw service zwaar wordt belast. **Wij raden u aan deze aanvraag expliciet in uw code te behandelen.** U kunt de indexering van documenten die niet zijn geïndexeerd vertragen en vervolgens opnieuw uitvoeren, maar u kunt ook een logboek maken en doorgaan zoals in het voorbeeld. U kunt ook een andere bewerking uitvoeren, afhankelijk van de vereisten omtrent de gegevensconsistentie van de toepassing.

> [!NOTE]
> U kunt de [ `FindFailedActionsToRetry` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception.findfailedactionstoretry) methode voor het maken van een nieuwe batch met alleen de acties die niet in een vorige aanroep aan `Index`. Er is een discussie over het gebruik ervan goed [op StackOverflow](https://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).
>
>

Ten slotte de `UploadDocuments` methode twee seconden vertraagd. Het indexeren verloopt asynchroon in uw Azure Search-service. De voorbeeldtoepassing moet even wachten totdat de documenten beschikbaar zijn voor een zoekbewerking. Dergelijke vertragingen zijn doorgaans alleen nodig is demo’s, testen en voorbeeldtoepassingen.

#### <a name="how-the-net-sdk-handles-documents"></a>De verwerking van documenten door .NET SDK
U vraagt zich misschien af hoe de Azure Search .NET SDK instanties van een door een gebruiker gedefinieerde klasse zoals `Hotel` naar de index kan uploaden. Als u wilt deze vraag te beantwoorden, we kijken naar de `Hotel` klasse:

```csharp
using System;
using Microsoft.Azure.Search;
using Microsoft.Azure.Search.Models;
using Microsoft.Spatial;
using Newtonsoft.Json;

// The SerializePropertyNamesAsCamelCase attribute is defined in the Azure Search .NET SDK.
// It ensures that Pascal-case property names in the model class are mapped to camel-case
// field names in the index.
[SerializePropertyNamesAsCamelCase]
public partial class Hotel
{
    [System.ComponentModel.DataAnnotations.Key]
    [IsFilterable]
    public string HotelId { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public double? BaseRate { get; set; }

    [IsSearchable]
    public string Description { get; set; }

    [IsSearchable]
    [Analyzer(AnalyzerName.AsString.FrLucene)]
    [JsonProperty("description_fr")]
    public string DescriptionFr { get; set; }

    [IsSearchable, IsFilterable, IsSortable]
    public string HotelName { get; set; }

    [IsSearchable, IsFilterable, IsSortable, IsFacetable]
    public string Category { get; set; }

    [IsSearchable, IsFilterable, IsFacetable]
    public string[] Tags { get; set; }

    [IsFilterable, IsFacetable]
    public bool? ParkingIncluded { get; set; }

    [IsFilterable, IsFacetable]
    public bool? SmokingAllowed { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public DateTimeOffset? LastRenovationDate { get; set; }

    [IsFilterable, IsSortable, IsFacetable]
    public int? Rating { get; set; }

    [IsFilterable, IsSortable]
    public GeographyPoint Location { get; set; }
}
```

Het eerste dat opvalt is dat elke openbare eigenschap van `Hotel` overeenkomt met een veld in de indexdefinitie, maar met één cruciaal verschil: De naam van elk veld begint met een kleine letter ('camelCase'), terwijl de naam van elke openbare eigenschap van `Hotel` begint met een hoofdletter ('Pascal case'). Dit is een algemeen scenario in .NET-toepassingen die gegevens koppelen waarbij het doelschema buiten de controle van de ontwikkelaar van de toepassing valt. In plaats van het schenden van de .NET-naamgevingsregels door een eigenschap met bijvoorbeeld de naam "kamelen" te maken, kunt u instellen dat de SDK de eigenschapsnamen automatisch moet toewijzen aan het `[SerializePropertyNamesAsCamelCase]`-kenmerk.

> [!NOTE]
> De Azure Search .NET SDK maakt gebruik van de [NewtonSoft JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm)-bibliotheek voor het serialiseren en deserialiseren van uw aangepaste modelobjecten naar en van JSON. U kunt deze serialisatie indien nodig aanpassen. Zie voor meer informatie, [aangepaste serialisatie met JSON.NET](#JsonDotNet).
> 
> 

De tweede dat zijn de kenmerken zoals `IsFilterable`, `IsSearchable`, `Key`, en `Analyzer` dat elke openbare eigenschap opmaken. Deze kenmerken wijzen rechtstreeks naar de [bijbehorende kenmerken van de Azure Search-index](https://docs.microsoft.com/rest/api/searchservice/create-index#request). De `FieldBuilder` klasse gebruikt deze velddefinities voor de index te maken.

Het derde belangrijkste over de `Hotel` klasse, zijn de gegevenstypen van de openbare eigenschappen. De .NET-typen van deze eigenschappen worden toegewezen aan de gelijkwaardige veldtypen in de indexdefinitie. De tekenreekseigenschap `Category` is bijvoorbeeld toegewezen aan het veld `category` van type `Edm.String`. Er zijn vergelijkbare type toewijzingen tussen `bool?` en `Edm.Boolean`, `DateTimeOffset?` en `Edm.DateTimeOffset`, enzovoort. De specifieke regels voor de toewijzing van het type worden gedocumenteerd met de methode `Documents.Get` in de [Azure Search .NET SDK-verwijzing](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.get). De `FieldBuilder` klasse zorgt dat deze toewijzing voor u, maar kan nog steeds nuttig om te begrijpen als u het oplossen van serialisatieproblemen zijn.

Deze mogelijkheid naar uw eigen klassen gebruiken als documenten werkt in beide richtingen; U kunt ook zoekresultaten ophalen en hebben de SDK automatisch deserialiseert naar een type van uw keuze, zoals we in de volgende sectie zien.

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
De SDK gebruikt JSON.NET voor het serialiseren en deserialiseren van documenten. U kunt aanpassen serialisatie en deserialisatie indien nodig door het definiëren van uw eigen `JsonConverter` of `IContractResolver` (Zie de [JSON.NET documentatie](https://www.newtonsoft.com/json/help/html/Introduction.htm) voor meer informatie). Dit kan nuttig zijn wanneer u een bestaande modelklasse van uw toepassing voor gebruik met Azure Search en andere meer geavanceerde scenario's aan te passen. Met aangepaste serialisatie kunt u bijvoorbeeld:

* Opnemen of uitsluiten van bepaalde eigenschappen van uw modelklasse als documentvelden worden opgeslagen.
* Koppeling tussen de namen van eigenschappen in uw code en veldnamen in uw index.
* Aangepaste kenmerken die kunnen worden gebruikt voor de toewijzing van eigenschappen voor documentvelden maken.

Hier vindt u voorbeelden van de implementatie van aangepaste serialisatie in de eenheidstests voor de Azure Search .NET SDK op GitHub. Is een goed uitgangspunt [deze map](https://github.com/Azure/azure-sdk-for-net/tree/AutoRest/src/Search/Search.Tests/Tests/Models). Deze bevat klassen die worden gebruikt door de aangepaste serialisatie-tests.

### <a name="searching-for-documents-in-the-index"></a>Zoeken naar documenten in de index
De laatste stap in de voorbeeldtoepassing is om te zoeken naar een aantal documenten in de index. De volgende methode doet dit:

```csharp
private static void RunQueries(ISearchIndexClient indexClient)
{
    SearchParameters parameters;
    DocumentSearchResult<Hotel> results;

    Console.WriteLine("Search the entire index for the term 'budget' and return only the hotelName field:\n");

    parameters =
        new SearchParameters()
        {
            Select = new[] { "hotelName" }
        };

    results = indexClient.Documents.Search<Hotel>("budget", parameters);

    WriteDocuments(results);

    Console.Write("Apply a filter to the index to find hotels cheaper than $150 per night, ");
    Console.WriteLine("and return the hotelId and description:\n");

    parameters =
        new SearchParameters()
        {
            Filter = "baseRate lt 150",
            Select = new[] { "hotelId", "description" }
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.Write("Search the entire index, order by a specific field (lastRenovationDate) ");
    Console.Write("in descending order, take the top two results, and show only hotelName and ");
    Console.WriteLine("lastRenovationDate:\n");

    parameters =
        new SearchParameters()
        {
            OrderBy = new[] { "lastRenovationDate desc" },
            Select = new[] { "hotelName", "lastRenovationDate" },
            Top = 2
        };

    results = indexClient.Documents.Search<Hotel>("*", parameters);

    WriteDocuments(results);

    Console.WriteLine("Search the entire index for the term 'motel':\n");

    parameters = new SearchParameters();
    results = indexClient.Documents.Search<Hotel>("motel", parameters);

    WriteDocuments(results);
}
```

Elke keer dat deze een query wordt uitgevoerd, met deze methode maakt eerst een nieuwe `SearchParameters` object. Dit wordt gebruikt om op te geven aanvullende opties voor de query, zoals sorteren, filteren, wisselbestand en op meerdere niveaus. Bij deze methode instellen we de `Filter`, `Select`, `OrderBy`, en `Top` eigenschap voor verschillende query's. Alle de `SearchParameters` eigenschappen zijn gedocumenteerd [hier](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).

De volgende stap is het daadwerkelijk uitvoeren van de zoekquery. Dit wordt gedaan met behulp van de `Documents.Search` methode. Voor elke query, geven we de zoektekst te gebruiken als een tekenreeks (of `"*"` als er geen zoektekst), plus de zoekparameters eerder hebt gemaakt. We ook opgeven `Hotel` als het typeparameter voor `Documents.Search`, waarin staat dat de SDK voor het deserialiseren van documenten in de lijst met zoekresultaten in objecten van het type `Hotel`.

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
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

In dit geval we zoeken hotels die overeenkomen met het woord 'budget' en we willen weer toegang te krijgen alleen de namen van het hotel, zoals opgegeven door de `Select` parameter. Hier volgen de resultaten:

    Name: Roach Motel

Vervolgens gaan we de hotels met een 's nachts tarief van minder dan €150 zoeken, en retourneert alleen de hotel-ID en de beschrijving:

```csharp
parameters =
    new SearchParameters()
    {
        Filter = "baseRate lt 150",
        Select = new[] { "hotelId", "description" }
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

Deze query maakt gebruik van een OData `$filter` expressie `baseRate lt 150`, voor het filteren van de documenten in de index. U vindt meer informatie over de OData-syntaxis die ondersteuning biedt voor Azure Search [hier](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).

Hier volgen de resultaten van de query:

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river

We willen vervolgens de bovenste twee hotels die onlangs zijn renovated zoeken en weergeven van het hotelnaam en datum van laatste renovatie. Dit is de code: 

```csharp
parameters =
    new SearchParameters()
    {
        OrderBy = new[] { "lastRenovationDate desc" },
        Select = new[] { "hotelName", "lastRenovationDate" },
        Top = 2
    };

results = indexClient.Documents.Search<Hotel>("*", parameters);

WriteDocuments(results);
```

In dit geval we opnieuw OData-syntaxis gebruiken om op te geven de `OrderBy` parameter als `lastRenovationDate desc`. We ook ingesteld `Top` op 2 om ervoor te zorgen we alleen de twee bovenste documenten ophalen. Als voorheen, we stellen `Select` om op te geven welke velden moeten worden geretourneerd.

Hier volgen de resultaten:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Tot slot gaan we vinden alle hotels die overeenkomen met het woord "motel":

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

En hier vindt u de resultaten van alle velden bevatten omdat er is geen opgegeven voor de `Select` eigenschap:

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

Deze stap wordt de zelfstudie voltooid, maar hier niet stoppen. **Volgende stappen** vindt u aanvullende bronnen voor meer informatie over Azure Search.

## <a name="next-steps"></a>Volgende stappen
* Neem de referenties voor de [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) en [REST API](https://docs.microsoft.com/rest/api/searchservice/) door.
* Beoordeling [naamconventies](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) voor meer informatie over de regels voor het benoemen van verschillende objecten.
* Beoordeling [ondersteunde gegevenstypen](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) in Azure Search.
