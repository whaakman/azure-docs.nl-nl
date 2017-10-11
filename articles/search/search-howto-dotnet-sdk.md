---
title: Het gebruik van Azure Search vanuit een .NET-toepassing | Microsoft Docs
description: Het gebruik van Azure Search vanuit een .NET-toepassing
services: search
documentationcenter: 
author: brjohnstmsft
manager: jlembicz
editor: 
ms.assetid: 93653341-c05f-4cfd-be45-bb877f964fcb
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 05/22/2017
ms.author: brjohnst
ms.openlocfilehash: 552a7ab193e12d2e72da494166d774e974c85d47
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-search-from-a-net-application"></a>Het gebruik van Azure Search vanuit een .NET-toepassing
In dit artikel is een overzicht krijgen van u actief en werkend met de [Azure Search .NET SDK](https://aka.ms/search-sdk). U kunt de .NET SDK gebruiken voor het implementeren van een uitgebreide zoekervaring in uw toepassing met behulp van Azure Search.

## <a name="whats-in-the-azure-search-sdk"></a>Wat is er in de Azure SDK zoeken
De SDK bestaat uit een clientbibliotheek `Microsoft.Azure.Search`. Hiermee kunt u voor het beheren van uw indexen, gegevensbronnen en indexeerfuncties, evenals uploaden en documenten beheren en uitvoeren van query's zonder te hoeven te bekommeren om de details van HTTP- en JSON.

De clientbibliotheek definieert klassen als `Index`, `Field`, en `Document`, evenals bewerkingen, zoals `Indexes.Create` en `Documents.Search` op de `SearchServiceClient` en `SearchIndexClient` klassen. Deze klassen zijn ingedeeld in de volgende naamruimten:

* [Microsoft.Azure.Search](https://docs.microsoft.com/dotnet/api/microsoft.azure.search)
* [Microsoft.Azure.Search.Models](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models)

De huidige versie van de Azure Search .NET SDK is nu algemeen beschikbaar. Als u uw feedback voor ons wilt opnemen in de volgende versie, een bezoek onze [feedbackpagina](https://feedback.azure.com/forums/263029-azure-search/).

De .NET SDK versie ondersteunt `2016-09-01` van de [Azure Search REST API](https://docs.microsoft.com/rest/api/searchservice/). Deze versie biedt nu ondersteuning voor aangepaste analyzers en ondersteuning van Azure-Blob en Azure Table-indexeerfunctie. Preview-functies die zijn *niet* deel uitmaken van deze versie, zoals ondersteuning voor het indexeren van JSON en CSV-bestanden bevinden zich in [preview](search-api-2015-02-28-preview.md) en beschikbaar via de oudere [2.0-preview-versie van de .NET SDK](https://aka.ms/search-sdk-preview).

Deze SDK biedt geen ondersteuning voor [beheerbewerkingen](https://docs.microsoft.com/rest/api/searchmanagement/) zoals maken en schalen van de Search-services en API-sleutels beheren. Als u zoeken in resources beheren vanuit een .NET-toepassing wilt, kunt u de [Azure Search .NET Management SDK](https://aka.ms/search-mgmt-sdk).

## <a name="upgrading-to-the-latest-version-of-the-sdk"></a>Upgraden naar de meest recente versie van de SDK
Als u al een oudere versie van de Azure Search .NET SDK en u wilt bijwerken naar de nieuwe versie van de algemeen beschikbaar [in dit artikel](search-dotnet-sdk-migration.md) wordt uitgelegd hoe.

## <a name="requirements-for-the-sdk"></a>Vereisten voor de SDK
1. Visual Studio 2017.
2. Uw eigen Azure Search-service. Als u wilt gebruiken in de SDK, moet u de naam van uw service en een of meer API-sleutels. [Maken van een service in de portal](search-create-service-portal.md) helpt u bij deze stappen.
3. Download de Azure Search .NET SDK [NuGet-pakket](http://www.nuget.org/packages/Microsoft.Azure.Search) met behulp van 'NuGet-pakketten beheren' in Visual Studio. Naam van het pakket zoeken `Microsoft.Azure.Search` op NuGet.org.

De Azure Search .NET SDK biedt ondersteuning voor toepassingen die gericht is op de .NET Framework 4.6 en .NET Core.

## <a name="core-scenarios"></a>Belangrijkste scenario 's
Er zijn verschillende dingen die u moet in uw zoektoepassing. In deze zelfstudie aan bod deze belangrijkste scenario's:

* Een index te maken
* De index met documenten in te vullen
* Zoeken naar documenten met behulp van de zoekopdracht in volledige tekst en filters

De volgende voorbeeldcode ziet u elk van deze. U kunt de codefragmenten gebruiken in uw eigen toepassing.

### <a name="overview"></a>Overzicht
De voorbeeldtoepassing we je worden verkennen maakt u een nieuwe index met de naam "hotels" gevuld met een paar documenten en vervolgens bepaalde zoekquery's worden uitgevoerd. Dit is het belangrijkste programma, de algemene stroom wordt weergegeven:

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

We doorlopen deze stap voor stap. Eerst moet u een nieuwe `SearchServiceClient`. Dit object kunt u voor het beheren van indexen. U moet een samenstellen, geef de naam van uw Azure Search-service, evenals een beheer-API-sleutel. U kunt deze informatie invoeren in de `appsettings.json` -bestand van de [voorbeeldtoepassing](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

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
> Als u een onjuist sleutel (bijvoorbeeld een query waarin een beheersleutel vereist is), bieden de `SearchServiceClient` genereert een `CloudException` bericht met de fout 'Verboden' de eerste keer dat u de bewerkingsmethode van een, zoals aanroepen `Indexes.Create`. Als dit gebeurt aan u is, Controleer onze API-sleutel.
> 
> 

De volgende regels aanroepen methoden voor het maken van een index met de naam "hotels", eerst verwijderen als deze al bestaat. We doorlopen die deze methoden enigszins hoger.

```csharp
Console.WriteLine("{0}", "Deleting index...\n");
DeleteHotelsIndexIfExists(serviceClient);

Console.WriteLine("{0}", "Creating index...\n");
CreateHotelsIndex(serviceClient);
```

De index moet vervolgens worden ingevuld. Om dit te doen, moeten we een `SearchIndexClient`. Er zijn twee manieren een ophalen: door deze samen te stellen of door het aanroepen van `Indexes.GetClient` op de `SearchServiceClient`. We gebruiken deze laatste voor het gemak.

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

> [!NOTE]
> In een standaardzoektoepassing wordt het beheer van de index en de invulling daarvan afgehandeld door een afzonderlijk onderdeel van de zoekquery’s. `Indexes.GetClient` is handig voor het vullen van een index, omdat u geen andere `SearchCredentials` hoeft op te geven. Dit wordt uitgevoerd door de administratorsleutel die u hebt gebruikt om de `SearchServiceClient` te maken die u wilt doorgeven aan de nieuwe `SearchIndexClient`. In het gedeelte van uw toepassing waarmee u query's uitvoert, is het beter om de `SearchIndexClient` direct te maken, zodat u een querysleutel kunt toepassen in plaats van een administratorsleutel. Dit is consistent zijn met het principe van minimale bevoegdheden en zorgt ervoor dat uw toepassing beter te beveiligen. U vindt meer informatie over administratorsleutels en querysleutels [hier](https://docs.microsoft.com/rest/api/searchservice/#authentication-and-authorization).
> 
> 

Nu dat we hebben een `SearchIndexClient`, kan de index vult. Dit wordt gedaan door een andere methode die u kunt later zien.

```csharp
Console.WriteLine("{0}", "Uploading documents...\n");
UploadDocuments(indexClient);
```

Ten slotte we enkele zoekopdrachten uitvoeren en de resultaten weer te geven. Nu we gebruiken een ander `SearchIndexClient`:

```csharp
ISearchIndexClient indexClientForQueries = CreateSearchIndexClient(configuration);

RunQueries(indexClientForQueries);
```

We nader bekeken duurt de `RunQueries` methode later. Dit is de code voor het maken van de nieuwe `SearchIndexClient`:

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

Nu we een querysleutel gebruiken aangezien er geen schrijftoegang tot de index hoeft. U kunt deze informatie invoeren in de `appsettings.json` -bestand van de [voorbeeldtoepassing](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo).

Als u deze toepassing met een geldige servicenaam en API-sleutels uitvoert, wordt de uitvoer ziet als volgt:

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

De volledige broncode van de toepassing wordt verstrekt aan het einde van dit artikel.

Vervolgens gaat we nader bekeken elk van de methoden aangeroepen door `Main`.

### <a name="creating-an-index"></a>Een index te maken
Na het maken van een `SearchServiceClient`, het volgende dat `Main` biedt is verwijderd als deze al bestaat de index "hotels". Die wordt uitgevoerd door de volgende methode:

```csharp
private static void DeleteHotelsIndexIfExists(SearchServiceClient serviceClient)
{
    if (serviceClient.Indexes.Exists("hotels"))
    {
        serviceClient.Indexes.Delete("hotels");
    }
}
```

Deze methode maakt gebruik van de opgegeven `SearchServiceClient` om te controleren of de index bestaat, en zo ja, verwijderen.

> [!NOTE]
> In de voorbeeldcode in dit artikel is voor het gemak gebruikgemaakt van de synchrone methoden van de Azure Search .NET SDK. Het wordt aanbevolen in uw eigen toepassingen asynchrone methoden te gebruiken, zodat de toepassingen schaalbaar zijn en goed reageren. Bijvoorbeeld, in de bovenstaande methode kunt u `ExistsAsync` en `DeleteAsync` in plaats van `Exists` en `Delete`.
> 
> 

Vervolgens `Main` maakt een nieuwe index "hotels" door deze methode aanroept:

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

Deze methode maakt u een nieuwe `Index` object met een lijst met `Field` objecten die het schema van de nieuwe index definieert. Elk veld heeft een naam, gegevenstype en verschillende kenmerken die het zoekgedrag definiëren. De `FieldBuilder` klasse reflectie gebruikt voor het maken van een lijst met `Field` objecten voor de index door in de openbare eigenschappen en kenmerken van de opgegeven `Hotel` modelklasse. We gaan die nader bekeken de `Hotel` klasse later op.

> [!NOTE]
> U kunt de lijst met altijd maken `Field` objecten rechtstreeks in plaats van `FieldBuilder` indien nodig. Bijvoorbeeld, u niet wilt gebruiken een modelklasse of mogelijk moet u een bestaande modelklasse die u niet wijzigen wilt door de kenmerken toe te voegen.
>
> 

Naast velden, kunt u scoreprofiel profielen, suggestiefunctie of opties voor CORS ook toevoegen aan de Index (deze worden weggelaten uit het voorbeeld als beknopt alternatief bevat). U vindt meer informatie over het object Index en de samenstellende delen in de [SDK-naslaginformatie](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.index#microsoft_azure_search_models_index), alsmede in de [Azure Search REST API-verwijzing](https://docs.microsoft.com/rest/api/searchservice/).

### <a name="populating-the-index"></a>De index te vullen
De volgende stap in `Main` voor het vullen van de gemaakte index is. Dit doet u in de volgende methode:

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

Deze methode heeft vier onderdelen. De eerste maakt een matrix van `Hotel` objecten die als onze invoergegevens fungeert uploaden naar de index. Deze gegevens zijn vastgelegd voor eenvoud. In uw eigen toepassing, wordt uw gegevens waarschijnlijk afkomstig zijn van een externe gegevensbron, zoals een SQL-database.

Het tweede gedeelte maakt een `IndexBatch` met de documenten. Geef van de bewerking die u toepassen op de batch op het moment dat u dit hebt gemaakt, in dit geval wilt door het aanroepen van `IndexBatch.Upload`. De batch wordt vervolgens geüpload naar de Azure Search-index met de `Documents.Index` methode.

> [!NOTE]
> In dit voorbeeld zijn er alleen documenten te uploaden. Als u wijzigingen in bestaande documenten samenvoegen of verwijderen van documenten wilt, kunt u batches maken door het aanroepen van `IndexBatch.Merge`, `IndexBatch.MergeOrUpload`, of `IndexBatch.Delete` in plaats daarvan. Ook kunt u verschillende bewerkingen in één batch elkaar door aan te roepen `IndexBatch.New`, wat een verzameling van duurt `IndexAction` objecten, die elk Azure Search, een bepaalde bewerking uitvoeren op een document wordt uitgelegd. U kunt elk maken `IndexAction` met een eigen bewerking door het aanroepen van de bijbehorende methode, zoals `IndexAction.Merge`, `IndexAction.Upload`, enzovoort.
> 
> 

Het derde deel van deze methode is een blok catch die verantwoordelijk is voor een belangrijke foutaanvraag voor indexering. Als uw Azure Search-service geen index van een aantal documenten in de batch kan maken, wordt een `IndexBatchException` verstuurd door `Documents.Index`. Dit kan gebeuren als u documenten indexeert terwijl uw service zwaar wordt belast. **Wij raden u aan deze aanvraag expliciet in uw code te behandelen.** U kunt de indexering van documenten die niet zijn geïndexeerd vertragen en vervolgens opnieuw uitvoeren, maar u kunt ook een logboek maken en doorgaan zoals in het voorbeeld. U kunt ook een andere bewerking uitvoeren, afhankelijk van de vereisten omtrent de gegevensconsistentie van de toepassing.

> [!NOTE]
> U kunt de `FindFailedActionsToRetry` methode voor het maken van een nieuwe batch met alleen de acties die niet in een eerdere aanroep voor `Index`. De methode wordt beschreven [hier](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.indexbatchexception#Microsoft_Azure_Search_IndexBatchException_FindFailedActionsToRetry_Microsoft_Azure_Search_Models_IndexBatch_System_String_) en er is een beschrijving van hoe goed het [op StackOverflow](http://stackoverflow.com/questions/40012885/azure-search-net-sdk-how-to-use-findfailedactionstoretry).
>
>

Ten slotte de `UploadDocuments` methode twee seconden vertraagd. Het indexeren verloopt asynchroon in uw Azure Search-service. De voorbeeldtoepassing moet even wachten totdat de documenten beschikbaar zijn voor een zoekbewerking. Dergelijke vertragingen zijn doorgaans alleen nodig is demo’s, testen en voorbeeldtoepassingen.

#### <a name="how-the-net-sdk-handles-documents"></a>De verwerking van documenten door .NET SDK
U vraagt zich misschien af hoe de Azure Search .NET SDK instanties van een door een gebruiker gedefinieerde klasse zoals `Hotel` naar de index kan uploaden. Als u wilt deze vraag te beantwoorden, bekijken we de `Hotel` klasse:

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

Het eerste dat opvalt, is dat elke openbare eigenschap van `Hotel` overeenkomt met een veld in de definitie van de index. Er is echter een cruciaal verschil: de naam van elk veld begint met een kleine letter ("kamelen"), terwijl de naam van elke openbare eigenschap van `Hotel` met een hoofdletter ("Pascal") begint. Dit is een algemeen scenario in .NET-toepassingen die gegevens koppelen waarbij het doelschema buiten de controle van de ontwikkelaar van de toepassing valt. In plaats van het schenden van de .NET-naamgevingsregels door een eigenschap met bijvoorbeeld de naam "kamelen" te maken, kunt u instellen dat de SDK de eigenschapsnamen automatisch moet toewijzen aan het `[SerializePropertyNamesAsCamelCase]`-kenmerk.

> [!NOTE]
> De Azure Search .NET SDK maakt gebruik van de [NewtonSoft JSON.NET](http://www.newtonsoft.com/json/help/html/Introduction.htm)-bibliotheek voor het serialiseren en deserialiseren van uw aangepaste modelobjecten naar en van JSON. U kunt deze serialisatie indien nodig aanpassen. Zie voor meer informatie [aangepaste serialisatie met JSON.NET](#JsonDotNet).
> 
> 

Het tweede dat opvalt zijn de kenmerken, zoals `IsFilterable`, `IsSearchable`, `Key`, en `Analyzer` dat elke openbare eigenschap opmaken. Deze kenmerken toewijzen rechtstreeks naar de [overeenkomende kenmerken van de Azure Search-index](https://docs.microsoft.com/rest/api/searchservice/create-index#request). De `FieldBuilder` klasse gebruikt deze velddefinities voor de index te maken.

Het derde belangrijkste over de `Hotel` klasse, zijn de gegevenstypen van de openbare eigenschappen. De .NET-typen van deze eigenschappen worden toegewezen aan de gelijkwaardige veldtypen in de definitie van de index. De tekenreekseigenschap `Category` is bijvoorbeeld toegewezen aan het veld `category` van type `Edm.String`. Er zijn vergelijkbare type toewijzingen tussen `bool?` en `Edm.Boolean`, `DateTimeOffset?` en `Edm.DateTimeOffset`, enzovoort. De specifieke regels voor de toewijzing van het type worden gedocumenteerd met de methode `Documents.Get` in de [Azure Search .NET SDK-verwijzing](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.idocumentsoperations#Microsoft_Azure_Search_IDocumentsOperations_GetWithHttpMessagesAsync__1_System_String_System_Collections_Generic_IEnumerable_System_String__Microsoft_Azure_Search_Models_SearchRequestOptions_System_Collections_Generic_Dictionary_System_String_System_Collections_Generic_List_System_String___System_Threading_CancellationToken_). De `FieldBuilder` klasse zorgt voor deze toewijzing is voor u, maar kan nog steeds handig om te begrijpen als u problemen ondervindt serialisatie moet zijn.

Deze mogelijkheid aan uw eigen klassen te gebruiken als documenten werkt beide kanten; U kunt ook zoekresultaten ophalen en de SDK automatisch deserialiseert naar een type van uw keuze, zoals we in de volgende sectie ziet.

> [!NOTE]
> De Azure Search .NET SDK biedt ook ondersteuning voor dynamisch getypeerde documenten met behulp van de `Document`-klasse, die een sleutel/waarde-toewijst aan veldnamen naar waarden. Dit is handig in situaties waar u het schema van de index op het moment van ontwerp nog niet weet of wanneer het niet handig zou zijn om verbinding te maken met specifieke modelklassen Alle methoden in de SDK die werken met documenten hebben overloads die met werken de `Document`-klasse, evenals sterk getypeerde overloads die een generiek typeparameter moeten uitvoeren. Alleen de laatste worden gebruikt in de voorbeeldcode in deze zelfstudie. De `Document` klasse neemt over van `Dictionary<string, object>`. U vindt andere details [hier](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.document#microsoft_azure_search_models_document).
> 
> 

**Waarom u nullable-gegevenstypen moet gebruiken**

Bij het ontwerpen van uw eigen modelklassen die u wilt toewijzen aan een Azure Search-index raden we u aan om eigenschappen van waardentypen `bool` en `int` in te stellen op null-waarden (bijvoorbeeld `bool?` in plaats van `bool`). Als u een niet-nullbare eigenschap gebruikt, moet u **garanderen** dat de documenten in de index geen null-waarde voor het betreffende veld bevatten. Noch de SDK noch de Azure Search-service helpt u om dit af te dwingen.

Dit is niet alleen een hypothetische probleem: Stel een scenario voor waarin u een nieuw veld toevoegt aan een bestaande index van het type `Edm.Int32`. Na het bijwerken van de indexdefinitie hebben alle documenten een null-waarde voor het nieuwe veld (omdat alle typen null in Azure Search zijn). Als u vervolgens een modelklasse met een niet-nullbare `int`-eigenschap voor dat veld gebruikt, ontvangt u een `JsonSerializationException` zoals deze bij het ophalen van documenten:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Daarom wordt u aangeraden nullbare typen in uw modelklassen te gebruiken.

<a name="JsonDotNet"></a>

#### <a name="custom-serialization-with-jsonnet"></a>Aangepaste met JSON.NET-serialisatie
De SDK gebruikt JSON.NET voor het serialiseren en deserialiseren van documenten. U kunt aanpassen serialisatie en deserialisatie indien nodig door het definiëren van uw eigen `JsonConverter` of `IContractResolver` (Zie de [JSON.NET documentatie](http://www.newtonsoft.com/json/help/html/Introduction.htm) voor meer informatie). Dit kan nuttig zijn wanneer u wilt aanpassen van een bestaande modelklasse van uw toepassing voor gebruik met Azure Search en andere meer geavanceerde scenario's. Met aangepaste serialisatie kunt u bijvoorbeeld:

* Opnemen of uitsluiten van bepaalde eigenschappen van uw modelklasse als documentvelden worden opgeslagen.
* Koppeling tussen de namen van eigenschappen in uw code en veldnamen in uw index.
* Aangepaste kenmerken die kunnen worden gebruikt voor het toewijzen van eigenschappen aan documentvelden maken.

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

Telkens wanneer een query wordt uitgevoerd, wordt deze methode maakt eerst een nieuwe `SearchParameters` object. Dit wordt gebruikt voor extra opties opgeven voor de query zoals sorteren en filteren van, paginering facetten. Bij deze methode instelt we de `Filter`, `Select`, `OrderBy`, en `Top` eigenschap voor verschillende query's. Alle de `SearchParameters` eigenschappen zijn gedocumenteerd [hier](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters).

De volgende stap is het daadwerkelijk uitvoeren van de zoekopdracht. Dit wordt gedaan met behulp van de `Documents.Search` methode. Voor elke query, geven we de zoektekst te gebruiken als een tekenreeks (of `"*"` als er geen zoektekst), plus de zoekparameters eerder hebt gemaakt. We ook opgeven `Hotel` als het typeparameter voor `Documents.Search`, die de SDK voor het deserialiseren van documenten in de zoekresultaten in objecten van het type vertelt `Hotel`.

> [!NOTE]
> U vindt meer informatie over de syntaxis van de zoekquery expressie [hier](https://docs.microsoft.com/rest/api/searchservice/Simple-query-syntax-in-Azure-Search).
> 
> 

Ten slotte na elke query doorlopen met deze methode alle overeenkomsten in de zoekresultaten elk document naar de console afdrukken:

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

U gaat nu een nader bekeken elk van de query's op zijn beurt. Dit is de code de eerste query uit te voeren:

```csharp
parameters =
    new SearchParameters()
    {
        Select = new[] { "hotelName" }
    };

results = indexClient.Documents.Search<Hotel>("budget", parameters);

WriteDocuments(results);
```

In dit geval we zoeken hotels die overeenkomen met het woord 'budget' en willen we weer toegang te krijgen alleen de namen van het hotel, zoals opgegeven door de `Select` parameter. Hier volgen de resultaten:

    Name: Roach Motel

We willen vervolgens zoekt de hotels met een elke nacht tarief van minder dan €150 en retourneert alleen de hotel-ID en de beschrijving:

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

Deze query gebruikt een OData `$filter` expressie, `baseRate lt 150`, voor het filteren van de documenten in de index. U vindt meer informatie over de syntaxis van de OData-die ondersteuning biedt voor Azure Search [hier](https://docs.microsoft.com/rest/api/searchservice/OData-Expression-Syntax-for-Azure-Search).

Hier volgen de resultaten van de query:

    ID: 2   Description: Cheapest hotel in town
    ID: 3   Description: Close to town hall and the river

We willen vervolgens om de eerste twee hotels die onlangs zijn renovated en ziet u de naam van hotel en datum van laatste renovatie. Dit is de code: 

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

In dit geval we opnieuw OData-syntaxis gebruiken om op te geven de `OrderBy` parameter als `lastRenovationDate desc`. We ook ingesteld `Top` op 2 om ervoor te zorgen we alleen de twee bovenste documenten ophalen. Net als voorheen kunt we ingesteld `Select` om op te geven welke velden moeten worden geretourneerd.

Hier volgen de resultaten:

    Name: Fancy Stay        Last renovated on: 6/27/2010 12:00:00 AM +00:00
    Name: Roach Motel       Last renovated on: 4/28/1982 12:00:00 AM +00:00

Ten slotte wordt er gezocht naar alle hotels die overeenkomen met het woord 'motel':

```csharp
parameters = new SearchParameters();
results = indexClient.Documents.Search<Hotel>("motel", parameters);

WriteDocuments(results);
```

Hier zijn de resultaten die alle velden bevatten omdat er is geen opgegeven de `Select` eigenschap:

    ID: 2   Base rate: 79.99        Description: Cheapest hotel in town     Description (French): Hôtel le moins cher en ville      Name: Roach Motel       Category: Budget        Tags: [motel, budget]   Parking included: yes   Smoking allowed: yes    Last renovated on: 4/28/1982 12:00:00 AM +00:00 Rating: 1/5     Location: Latitude 49.678581, longitude -122.131577

Deze stap is voltooid voor de zelfstudie, maar hier niet stoppen. **Volgende stappen** bevat aanvullende bronnen voor meer informatie over Azure Search.

## <a name="next-steps"></a>Volgende stappen
* Neem de referenties voor de [.NET SDK](https://docs.microsoft.com/dotnet/api/microsoft.azure.search) en [REST API](https://docs.microsoft.com/rest/api/searchservice/) door.
* Uw medeweten via verdiepen [video's en andere voorbeelden en zelfstudies](search-video-demo-tutorial-list.md).
* Bekijk [naamconventies](https://docs.microsoft.com/rest/api/searchservice/Naming-rules) voor meer informatie over de regels voor het benoemen van verschillende objecten.
* Bekijk [ondersteunde gegevenstypen](https://docs.microsoft.com/rest/api/searchservice/Supported-data-types) in Azure Search.
