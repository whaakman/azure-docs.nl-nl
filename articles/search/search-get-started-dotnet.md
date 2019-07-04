---
title: "C#Snelstartgids: Maken en query's uitvoeren met behulp van .NET SDK - Azure Search indexen laden"
description: Wordt uitgelegd hoe u een index maken, gegevens laden en query's uitvoeren met C# en de Azure Search .NET SDK.
author: heidisteen
manager: cgronlun
ms.author: heidist
tags: azure-portal
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 06/20/2019
ms.openlocfilehash: a5cbd2036f92c27709d92d0cf415cc9837645fb8
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485607"
---
# <a name="quickstart-create-an-azure-search-index-in-c-using-the-net-sdk"></a>Quickstart: Maken van een Azure Search-index in C# met de .NET SDK
> [!div class="op_single_selector"]
> * [C#](search-get-started-dotnet.md)
> * [Portal](search-get-started-portal.md)
> * [PowerShell](search-create-index-rest-api.md)
> * [Python](search-get-started-python.md)
> * [Postman](search-get-started-postman.md)
>*

Maken van een .NET Core C# consoletoepassing die wordt gemaakt, wordt geladen en query's van een Azure Search-index met behulp van Visual Studio en de [Azure Search .NET SDK](https://aka.ms/search-sdk). In dit artikel wordt uitgelegd hoe u de toepassing stap voor stap maakt. U kunt ook [downloaden en uitvoeren van de volledige toepassing](https://github.com/Azure-Samples/azure-search-dotnet-samples/tree/master/Quickstart).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

> [!NOTE]
> De demo-code in dit artikel wordt de synchrone methoden van de Azure Search .NET SDK gebruikt voor het gemak. Echter voor productiescenario's, wordt u aangeraden asynchrone methoden in uw eigen toepassingen dat ze voor schaalbaarheid en reactiesnelheid. U kunt bijvoorbeeld `CreateAsync` en `DeleteAsync` in plaats van `Create` en `Delete`.

## <a name="prerequisites"></a>Vereisten

De volgende services, hulpprogramma's en gegevens worden gebruikt in deze Quick Start. 

+ [Visual Studio](https://visualstudio.microsoft.com/downloads/), alle edities. Voorbeeldcode en instructies zijn getest op de gratis Community-versie.

+ Een voorbeeldindex en documenten vindt u in dit artikel in de [Visual Studio-oplossing](https://github.com/Azure-Samples/azure-search-dotnet-samples/quickstart) voor deze Quick Start.

+ [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze Quick Start.

<a name="get-service-info"></a>

## <a name="get-a-key-and-url"></a>Een sleutel en -URL ophalen

Aanroepen naar de service vereist een URL-eindpunt en een toegangssleutel bij elke aanvraag. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij Azure portal](https://portal.azure.com/), en in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

2. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

   De querysleutel ook ophalen. Het is een aanbevolen procedure om uit te geven van queryaanvragen met alleen-lezen toegang.

![Een HTTP-eindpunt en -sleutel ophalen](media/search-get-started-postman/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service. Met een geldige sleutel stelt u per aanvraag een vertrouwensrelatie in tussen de toepassing die de aanvraag verzendt en de service die de aanvraag afhandelt.

## <a name="set-up-your-environment"></a>Uw omgeving instellen

Beginnen met Visual Studio te openen en een nieuwe Console-App-project maken dat kan worden uitgevoerd op .NET Core.

### <a name="install-nuget-packages"></a>NuGet-pakketten installeren

De [Azure Search .NET SDK](https://aka.ms/search-sdk) bestaat uit een aantal clientbibliotheken die worden gedistribueerd als een NuGet-pakketten.

Gebruik voor dit project, versie 9 van de `Microsoft.Azure.Search` NuGet-pakket en de meest recente `Microsoft.Extensions.Configuration.Json` NuGet-pakket.

1. In **extra** > **NuGet Package Manager**, selecteer **NuGet-pakketten beheren voor oplossing...** . 

1. Klik op **Bladeren**.

1. Zoeken naar `Microsoft.Azure.Search` en selecteer versie 9.0.1 of hoger.

1. Klik op **installeren** aan de rechterkant de assembly toevoegen aan uw project en de oplossing.

1. Herhaal dit voor `Microsoft.Extensions.Configuration.Json`, versie 2.2.0 selecteren of hoger.


### <a name="add-azure-search-service-information"></a>Informatie over Azure Search-service toevoegen

1. Klik in Solution Explorer, klik met de rechtermuisknop op het project en selecteer **toevoegen** > **Nieuw Item...**  . 

1. Zoek in Add New Item 'JSON' om een JSON-gerelateerde lijst itemtypen te retourneren.

1. Kies **JSON-bestand**, Noem het bestand 'appsettings.json' en klikt u op **toevoegen**. 

1. Het bestand toevoegen aan uw map met de uitvoer. Met de rechtermuisknop op appsettings.json en selecteer **eigenschappen**. In **naar uitvoermap kopiëren**, selecteer **kopiëren indien nieuwer**.

1. Kopieer de volgende JSON naar de nieuwe JSON-bestand. Vervang de naam van de zoekopdracht-service (uw-SEARCH-SERVICE-naam) en beheer-API-sleutel (uw-ADMIN-API-sleutel) met geldige waarden. Als uw service-eindpunt is `https://mydemo.search.windows.net`, naam van de service 'mydemo' zou zijn.

```json
{
  "SearchServiceName": "<YOUR-SEARCH-SERVICE-NAME>",
  "SearchServiceAdminApiKey": "<YOUR-ADMIN-API-KEY>",
  "SearchIndexName": "hotels-quickstart"
}
```

### <a name="add-class-method-files-to-your-project"></a>Klasse toevoegen '. Methode' bestanden aan uw project

Wanneer het afdrukken van resultaten in het consolevenster, moeten u afzonderlijke velden van het Hotel-object geretourneerd als tekenreeksen. U kunt implementeren [ToString()](https://docs.microsoft.com/dotnet/api/system.object.tostring?view=netframework-4.8) om uit te voeren van deze taak, de benodigde code kopiëren naar twee nieuwe bestanden.

1. Twee lege klassedefinities toevoegen aan uw project: Address.Methods.cs, Hotel.Methods.cs

1. In Address.Methods.cs, de Standaardinhoud wordt overschreven door de volgende code [regels 1 en 32](https://github.com/Azure-Samples/azure-search-dotnet-samples/blob/master/Quickstart/AzureSearchQuickstart/Address.Methods.cs/#L1-L32).

1. Kopieer in Hotel.Methods.cs, [regels 1-66](https://github.com/Azure-Samples/azure-search-dotnet-samples/blob/master/Quickstart/AzureSearchQuickstart/Hotel.Methods.cs/#L1-L66).


## <a name="1---create-index"></a>1 - index maken

De index hotels bestaat uit eenvoudige en complexe velden, waarbij een eenvoudige veldverwijzing 'HotelName' of 'Description', en complexe velden zijn een adres met subvelden, of een verzameling van ruimten. Wanneer een index complexe typen bevat, isolatie van de complexe velddefinities in afzonderlijke klassen.

1. Twee lege klassedefinities toevoegen aan uw project: Address.cs, Hotel.cs

1. In Address.cs, de Standaardinhoud wordt overschreven door de volgende code:

    ```csharp
    using System;
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    using Newtonsoft.Json;

    namespace AzureSearchQuickstart
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

1. De klasse definieert in Hotel.cs, de algemene structuur van de index, met inbegrip van verwijzingen naar de adresklasse.

    ```csharp
    namespace AzureSearchQuickstart
    {
        using System;
        using Microsoft.Azure.Search;
        using Microsoft.Azure.Search.Models;
        using Newtonsoft.Json;

        public partial class Hotel
        {
            [System.ComponentModel.DataAnnotations.Key]
            [IsFilterable]
            public string HotelId { get; set; }

            [IsSearchable, IsSortable]
            public string HotelName { get; set; }

            [IsSearchable]
            [Analyzer(AnalyzerName.AsString.EnMicrosoft)]
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

            [IsFilterable, IsSortable, IsFacetable]
            public DateTimeOffset? LastRenovationDate { get; set; }

            [IsFilterable, IsSortable, IsFacetable]
            public double? Rating { get; set; }

            public Address Address { get; set; }
        }
    }
    ```

    Kenmerken in het veld bepalen hoe deze wordt gebruikt in een toepassing. Bijvoorbeeld, de `IsSearchable` kenmerk is toegewezen aan elk veld dat u in een zoekopdracht in volledige tekst opnemen wilt. In de .NET SDK, wordt de standaardwaarde is om uit te schakelen veld gedrag die niet expliciet zijn ingeschakeld.

    Precies één veld in de index van het type `string` moet de *sleutel* veld, elk document een unieke id. In dit schema, de sleutel is `HotelId`.

    In deze index gebruikt de velden beschrijving als u wilt overschrijven van de standaard standard Lucene analyzer opgegeven eigenschap optioneel analyzer. De `description_fr` veld de Frans-Lucene analyzer gebruikt ([FrLucene](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername.frlucene?view=azure-dotnet)) omdat Franse tekst worden opgeslagen. De `description` met behulp van de optionele taalanalyse van Microsoft ([EnMicrosoft](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.analyzername.enmicrosoft?view=azure-dotnet)).

1. In Program.cs, maakt u een exemplaar van de [ `SearchServiceClient` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient?view=azure-dotnet) klasse verbinding maken met de service, met behulp van waarden die zijn opgeslagen in het configuratiebestand van de toepassing (appsettings.json). 

   `SearchServiceClient` heeft een [ `Indexes` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchserviceclient.indexes?view=azure-dotnet) eigenschap, die alle methoden die u maken wilt, weergeven, bijwerken of verwijderen van Azure Search-index. 

    ```csharp
    using System;
    using System.Linq;
    using System.Threading;
    using Microsoft.Azure.Search;
    using Microsoft.Azure.Search.Models;
    using Microsoft.Extensions.Configuration;

    namespace AzureSearchQuickstart
    {
        class Program
            // Demonstrates index delete, create, load, and query
            // Commented-out code is uncommented in later steps
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

                // Uncomment next 3 lines in "2 - Load documents"
                // ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);
                // Console.WriteLine("{0}", "Uploading documents...\n");
                // UploadDocuments(indexClient);

                // Uncomment next 2 lines in "3 - Search an index"
                // Console.WriteLine("{0}", "Searching index...\n");
                // RunQueries(indexClient);

                Console.WriteLine("{0}", "Complete.  Press any key to end application...\n");
                Console.ReadKey();
            }

            // Create the search service client
            private static SearchServiceClient CreateSearchServiceClient(IConfigurationRoot configuration)
            {
                string searchServiceName = configuration["SearchServiceName"];
                string adminApiKey = configuration["SearchServiceAdminApiKey"];

                SearchServiceClient serviceClient = new SearchServiceClient(searchServiceName, new SearchCredentials(adminApiKey));
                return serviceClient;
            }

            // Delete an existing index to reuse its name
            private static void DeleteIndexIfExists(string indexName, SearchServiceClient serviceClient)
            {
                if (serviceClient.Indexes.Exists(indexName))
                {
                    serviceClient.Indexes.Delete(indexName);
                }
            }

            // Create an index whose fields correspond to the properties of the Hotel class.
            // The Address property of Hotel will be modeled as a complex field.
            // The properties of the Address class in turn correspond to sub-fields of the Address complex field.
            // The fields of the index are defined by calling the FieldBuilder.BuildForType() method.
            private static void CreateIndex(string indexName, SearchServiceClient serviceClient)
            {
                var definition = new Index()
                {
                    Name = indexName,
                    Fields = FieldBuilder.BuildForType<Hotel>()
                };

                serviceClient.Indexes.Create(definition);
            }
        }
    }    
    ```

    Indien mogelijk, delen één exemplaar van `SearchServiceClient` in uw toepassing om te voorkomen dat te veel verbindingen te openen. Methoden van de klasse zijn thread-safe om in te schakelen delen.

   De klasse heeft verschillende constructors. De gewenste constructor krijgt u de naam van uw zoekservice en een `SearchCredentials`-object als parameters. `SearchCredentials` verpakt uw api-sleutel.

    In de definitie van de index, de eenvoudigste manier om het maken van de `Field` objecten is door het aanroepen van de `FieldBuilder.BuildForType` methode, waarbij een modelklasse voor het typeparameter doorgegeven. Een modelklasse heeft eigenschappen die worden toegewezen aan de velden van uw index. Deze toewijzing kunt u documenten uit de zoekindex binden aan exemplaren van uw modelklasse.

    > [!NOTE]
    > Als u niet van plan bent om een modelklasse te gebruiken, moet u alsnog de index definiëren door rechtstreeks `Field`-objecten te maken. U kunt de naam van het veld aan de constructor doorgeven, samen met het gegevenstype (of analyse voor tekenreeksvelden). U kunt ook andere eigenschappen, zoals instellen `IsSearchable`, `IsFilterable`, een paar te noemen.
    >

1. Druk op F5 om te maken van de app en de index te maken. 

    Als het project wordt gemaakt is, wordt een consolevenster geopend, statusberichten schrijven naar het scherm voor het verwijderen en de index te maken. 

<a name="load-documents"></a>

## <a name="2---load-documents"></a>2 - documenten laden

Documenten bestaan uit in Azure Search gegevensstructuren die zowel invoer om te indexeren en uitvoer van query's. Die is verkregen met een externe gegevensbron, kunnen het document invoer rijen in een database, blobs in Blob storage- of JSON-documenten op schijf zijn. In dit voorbeeld zijn we nemen een snelkoppeling en JSON-documenten voor vier hotels insluiten in de code zelf. 

Tijdens het uploaden van documenten, moet u een [ `IndexBatch` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexbatch?view=azure-dotnet) object. Een `IndexBatch` bevat een verzameling van [ `IndexAction` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexaction?view=azure-dotnet) objecten, die elk bevat een document en een eigenschap die Azure Search te vertellen welke actie om uit te voeren ([uploaden, samenvoegen, verwijderen en mergeOrUpload](search-what-is-data-import.md#indexing-actions)).

1. Maken van een matrix van documenten en de Indexacties in Program.cs, en geeft u de matrix, zodat `IndexBatch`. De onderstaande documenten voldoen aan de index hotels-quickstart, zoals gedefinieerd door de klassen hotel en het adres.

    ```csharp
    // Upload documents as a batch
    private static void UploadDocuments(ISearchIndexClient indexClient)
    {
        var actions = new IndexAction<Hotel>[]
        {
            IndexAction.Upload(
                new Hotel()
                {
                    HotelId = "1",
                    HotelName = "Secret Point Motel",
                    Description = "The hotel is ideally located on the main commercial artery of the city in the heart of New York. A few minutes away is Time's Square and the historic centre of the city, as well as other places of interest that make New York one of America's most attractive and cosmopolitan cities.",
                    DescriptionFr = "L'hôtel est idéalement situé sur la principale artère commerciale de la ville en plein cœur de New York. A quelques minutes se trouve la place du temps et le centre historique de la ville, ainsi que d'autres lieux d'intérêt qui font de New York l'une des villes les plus attractives et cosmopolites de l'Amérique.",
                    Category = "Boutique",
                    Tags = new[] { "pool", "air conditioning", "concierge" },
                    ParkingIncluded = false,
                    LastRenovationDate = new DateTimeOffset(1970, 1, 18, 0, 0, 0, TimeSpan.Zero),
                    Rating = 3.6,
                    Address = new Address()
                    {
                        StreetAddress = "677 5th Ave",
                        City = "New York",
                        StateProvince = "NY",
                        PostalCode = "10022",
                        Country = "USA"
                    }
                }
            ),
            IndexAction.Upload(
                new Hotel()
                {
                    HotelId = "2",
                    HotelName = "Twin Dome Motel",
                    Description = "The hotel is situated in a  nineteenth century plaza, which has been expanded and renovated to the highest architectural standards to create a modern, functional and first-class hotel in which art and unique historical elements coexist with the most modern comforts.",
                    DescriptionFr = "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
                    Category = "Boutique",
                    Tags = new[] { "pool", "free wifi", "concierge" },
                    ParkingIncluded = false,
                    LastRenovationDate =  new DateTimeOffset(1979, 2, 18, 0, 0, 0, TimeSpan.Zero),
                    Rating = 3.60,
                    Address = new Address()
                    {
                        StreetAddress = "140 University Town Center Dr",
                        City = "Sarasota",
                        StateProvince = "FL",
                        PostalCode = "34243",
                        Country = "USA"
                    }
                }
            ),
            IndexAction.Upload(
                new Hotel()
                {
                    HotelId = "3",
                    HotelName = "Triple Landscape Hotel",
                    Description = "The Hotel stands out for its gastronomic excellence under the management of William Dough, who advises on and oversees all of the Hotel’s restaurant services.",
                    DescriptionFr = "L'hôtel est situé dans une place du XIXe siècle, qui a été agrandie et rénovée aux plus hautes normes architecturales pour créer un hôtel moderne, fonctionnel et de première classe dans lequel l'art et les éléments historiques uniques coexistent avec le confort le plus moderne.",
                    Category = "Resort and Spa",
                    Tags = new[] { "air conditioning", "bar", "continental breakfast" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(2015, 9, 20, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4.80,
                    Address = new Address()
                    {
                        StreetAddress = "3393 Peachtree Rd",
                        City = "Atlanta",
                        StateProvince = "GA",
                        PostalCode = "30326",
                        Country = "USA"
                    }
                }
            ),
            IndexAction.Upload(
                new Hotel()
                {
                    HotelId = "4",
                    HotelName = "Sublime Cliff Hotel",
                    Description = "Sublime Cliff Hotel is located in the heart of the historic center of Sublime in an extremely vibrant and lively area within short walking distance to the sites and landmarks of the city and is surrounded by the extraordinary beauty of churches, buildings, shops and monuments. Sublime Cliff is part of a lovingly restored 1800 palace.",
                    DescriptionFr = "Le sublime Cliff Hotel est situé au coeur du centre historique de sublime dans un quartier extrêmement animé et vivant, à courte distance de marche des sites et monuments de la ville et est entouré par l'extraordinaire beauté des églises, des bâtiments, des commerces et Monuments. Sublime Cliff fait partie d'un Palace 1800 restauré avec amour.",
                    Category = "Boutique",
                    Tags = new[] { "concierge", "view", "24-hour front desk service" },
                    ParkingIncluded = true,
                    LastRenovationDate = new DateTimeOffset(1960, 2, 06, 0, 0, 0, TimeSpan.Zero),
                    Rating = 4.6,
                    Address = new Address()
                    {
                        StreetAddress = "7400 San Pedro Ave",
                        City = "San Antonio",
                        StateProvince = "TX",
                        PostalCode = "78216",
                        Country = "USA"
                    }
                }
            ),
        };

        var batch = IndexBatch.New(actions);

        try
        {
            indexClient.Documents.Index(batch);
        }
        catch (IndexBatchException e)
        {
            // When a service is under load, indexing might fail for some documents in the batch. 
            // Depending on your application, you can compensate by delaying and retrying. 
            // For this simple demo, we just log the failed document keys and continue.
            Console.WriteLine(
                "Failed to index some of the documents: {0}",
                String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
        }

        // Wait 2 seconds before starting queries 
        Console.WriteLine("Waiting for indexing...\n");
        Thread.Sleep(2000);
    }
    ```

    Zodra u initialiseert de`IndexBatch` -object, kunt u deze verzenden naar de index door het aanroepen van [ `Documents.Index` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.index?view=azure-dotnet) op uw [ `SearchIndexClient` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) object. `Documents` is een eigenschap van `SearchIndexClient` die biedt methoden voor het toevoegen, wijzigen, verwijderen of opvragen van documenten in uw index.

    De `try` / `catch` rond de aanroep naar de `Index` methode gevangen indexeren van fouten, wat gebeuren kunnen als uw service zwaar belast wordt. U kunt bij de productiecode vertraging en probeer het vervolgens opnieuw de documenten die is mislukt, aanmelden en doorgaan zoals in het voorbeeld doet of verwerkt in een andere manier die voldoet aan de vereisten van consistentie van gegevens van uw toepassing te indexeren.

    De vertraging 2 seconden gecompenseerd voor indexeren, die is asynchroon, zodat alle documenten kunnen worden geïndexeerd voordat de query's worden uitgevoerd. Coderen in een vertraging is doorgaans alleen nodig in de demo's, testen en voorbeeldtoepassingen.

1. In Program.cs in het hoofdvenster, verwijder opmerkingen bij de regels voor "2 - Load documenten". 

    ```csharp
    // Uncomment next 3 lines in "2 - Load documents"
    ISearchIndexClient indexClient = serviceClient.Indexes.GetClient(indexName);
    Console.WriteLine("{0}", "Uploading documents...\n");
    UploadDocuments(indexClient);
    ```
1. Druk op F5 om de app opnieuw. 

    Als het project is gebaseerd, een consolevenster wordt geopend, statusberichten, ditmaal met een bericht over het uploaden documenten schrijven. In de Azure portal, in de search-service **overzicht** pagina, de index hotels-quickstart moet nu zijn 4 documenten.

Zie voor meer informatie over de verwerking van documenten, ["Hoe de .NET SDK omgaat met documenten"](search-howto-dotnet-sdk.md#how-dotnet-handles-documents).

## <a name="3---search-an-index"></a>3 - Een index doorzoeken

U kunt de resultaten van query krijgen zodra het eerste document wordt geïndexeerd, maar daadwerkelijke tests van de index wachten moet totdat alle documenten worden geïndexeerd. 

In deze sectie voegt u twee soorten functionaliteit: querylogica en resultaten. Voor query's, gebruikt u de [ `Search` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.documentsoperationsextensions.search?view=azure-dotnet
) methode. Deze methode heeft de zoektekst, evenals andere [parameters](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters?view=azure-dotnet). 

De [ `DocumentsSearchResult` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.documentsearchresult-1?view=azure-dotnet) klasse vertegenwoordigt de resultaten.


1. Maak een WriteDocuments methode dat af te drukken in de console zoekresultaten in Program.cs.

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

1. Maak een RunQueries-methode voor het uitvoeren van query's en de resultaten worden geretourneerd. De resultaten zijn Hotel objecten. U kunt de select-parameters voor afzonderlijke velden. Als een veld niet in de select-parameters opgenomen is, wordt de bijbehorende Hotel-eigenschap niet null zijn.

    ```csharp
    private static void RunQueries(ISearchIndexClient indexClient)
    {
        SearchParameters parameters;
        DocumentSearchResult<Hotel> results;

        // Query 1 
        Console.WriteLine("Query 1: Search for term 'Atlanta' with no result trimming");
        parameters = new SearchParameters();
        results = indexClient.Documents.Search<Hotel>("Atlanta", parameters);
        WriteDocuments(results);

        // Query 2
        Console.WriteLine("Query 2: Search on the term 'Atlanta', with trimming");
        Console.WriteLine("Returning only these fields: HotelName, Tags, Address:\n");
        parameters =
            new SearchParameters()
            {
                Select = new[] { "HotelName", "Tags", "Address" },
            };
        results = indexClient.Documents.Search<Hotel>("Atlanta", parameters);
        WriteDocuments(results);

        // Query 3
        Console.WriteLine("Query 3: Search for the terms 'restaurant' and 'wifi'");
        Console.WriteLine("Return only these fields: HotelName, Description, and Tags:\n");
        parameters =
            new SearchParameters()
            {
                Select = new[] { "HotelName", "Description", "Tags" }
            };
        results = indexClient.Documents.Search<Hotel>("restaurant, wifi", parameters);
        WriteDocuments(results);

        // Query 4 -filtered query
        Console.WriteLine("Query 4: Filter on ratings greater than 4");
        Console.WriteLine("Returning only these fields: HotelName, Rating:\n");
        parameters =
            new SearchParameters()
            {
                Filter = "Rating gt 4",
                Select = new[] { "HotelName", "Rating" }
            };
        results = indexClient.Documents.Search<Hotel>("*", parameters);
        WriteDocuments(results);

        // Query 5 - top 2 results
        Console.WriteLine("Query 5: Search on term 'boutique'");
        Console.WriteLine("Sort by rating in descending order, taking the top two results");
        Console.WriteLine("Returning only these fields: HotelId, HotelName, Category, Rating:\n");
        parameters =
            new SearchParameters()
            {
                OrderBy = new[] { "Rating desc" },
                Select = new[] { "HotelId", "HotelName", "Category", "Rating" },
                Top = 2
            };
        results = indexClient.Documents.Search<Hotel>("boutique", parameters);
        WriteDocuments(results);
    }
    ```

    Er zijn twee [manieren van overeenkomende termen in een query](search-query-overview.md#types-of-queries): zoeken in volledige tekst en filters. Een query zoeken in volledige tekst zoekt naar een of meer voorwaarden in `IsSearchable` velden in uw index. Een filter is een Booleaanse expressie die wordt geëvalueerd ten opzichte van `IsFilterable` velden in een index. U kunt zoeken in volledige tekst en filters samen of afzonderlijk gebruiken.

    Zoekopdrachten en filters worden allebei uitgevoerd met behulp van de `Documents.Search`-methode. Een zoekopdracht kan worden doorgegeven aan de `searchText`-parameter, terwijl een filterexpressie kan worden doorgegeven in de eigenschap `Filter` van de `SearchParameters`-klasse. Als u wilt filteren zonder te zoeken, geeft u `"*"` door voor de `searchText`-parameter. Om te zoeken zonder te filteren, stelt u de eigenschap `Filter` niet in of geeft u niet door aan een `SearchParameters`-instantie.

1. In Program.cs in het hoofdvenster, verwijder opmerkingen bij de regels voor '3 - Search'. 

    ```csharp
    // Uncomment next 2 lines in "3 - Search an index"
    Console.WriteLine("{0}", "Searching documents...\n");
    RunQueries(indexClient);
    ```
1. De oplossing is nu voltooid. Druk op F5 voor het opnieuw opbouwen van de app en het programma uitvoeren in zijn geheel toe. 

    De uitvoer bevat de dezelfde berichten als voordat u, met het toevoegen van gegevens opvragen en de resultaten.

## <a name="clean-up"></a>Opruimen

Wanneer u klaar bent met een index en wilt verwijderen, roept de `Indexes.Delete` methode voor uw `SearchServiceClient`.

```csharp
serviceClient.Indexes.Delete("hotels");
```

Als u ook klaar met de search-service bent, kunt u resources verwijderen uit Azure portal.

## <a name="next-steps"></a>Volgende stappen

In deze C# quickstart, u hebt gewerkt door een reeks taken aan een index maken en query's uitvoeren met documenten worden geladen. In verschillende stadia werd snelkoppelingen voor het vereenvoudigen van de code voor de leesbaarheid en begrip. Als u vertrouwd met de basisconcepten bent, raden we het volgende artikel voor een verkenning van alternatieve methoden en concepten die van uw kennis wordt verdiepen. 

De voorbeeldcode en de index zijn uitgevouwen versies van deze gegevensset. Het volgende voorbeeld wordt een verzameling ruimten toegevoegd, maakt gebruik van verschillende klassen en acties en het duurt nog eens kijken hoe verwerking werkt.

> [!div class="nextstepaction"]
> [Over het ontwikkelen in .NET](search-howto-dotnet-sdk.md)