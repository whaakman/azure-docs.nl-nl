---
title: Een upgrade uitvoert naar de Azure Search .NET SDK versie 1.1 - Azure Search
description: Code migreren naar Azure Search .NET SDK versie 1.1 van oudere versies van de API. Ontdek wat er nieuw is en welke wijzigingen in de code zijn vereist.
author: brjohnstmsft
manager: jlembicz
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: conceptual
ms.date: 01/15/2018
ms.author: brjohnst
ms.custom: seodec2018
ms.openlocfilehash: 82823bae76521080634d4f7ff285d94ce8495fbf
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53317284"
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-11"></a>Een upgrade naar de Azure Search .NET SDK versie 1.1

Als u versie 1.0.2-preview of ouder bent van de [Azure Search .NET SDK](https://aka.ms/search-sdk), in dit artikel ziet u een upgrade van uw toepassing versie 1.1 te gebruiken.

Zie voor een meer algemeen overzicht van de SDK, inclusief voorbeelden [over het gebruik van Azure Search via een .NET-toepassing](search-howto-dotnet-sdk.md).

> [!NOTE]
> Nadat u een upgrade naar versie 1.1, of als u al een versie tussen 1.1 en 2.0-preview vallen, moet u bijwerken naar versie 3. Zie [upgraden naar de Azure Search .NET SDK versie 3](search-dotnet-sdk-migration.md) voor instructies.
>

Update eerst uw NuGet-verwijzing voor `Microsoft.Azure.Search` met behulp van de NuGet Package Manager-Console of door met de rechtermuisknop op uw projectverwijzingen en 'Beheren NuGet-pakketten...' selecteren in Visual Studio.

Nadat NuGet heeft de nieuwe pakketten en de bijbehorende afhankelijkheden gedownload, bouwt u uw project opnieuw.

Als u eerder versie 1.0.0-preview, 1.0.1-preview of 1.0.2-preview, de build moet slagen en u bent klaar om te beginnen.

Als u eerder versie 0.13.0-preview of ouder bent, ziet u buildfouten als volgt:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

De volgende stap is om op te lossen van de build-fouten één voor één. De meeste moet wijzigen, enkele klasse en methode namen die zijn gewijzigd in de SDK. [Lijst met belangrijke wijzigingen in versie 1.1](#ListOfChangesV1) bevat een overzicht van deze wijzigingen in de naam.

Als u aangepaste klassen om uw documenten te modelleren en deze klassen eigenschappen van niet-nullbare primitieve typen hebben (bijvoorbeeld `int` of `bool` in C#), wordt er een opgelost probleem in de versie 1.1 van de SDK van waarmee u rekening moet houden. Zie [oplossingen voor problemen in versie 1.1](#BugFixesV1) voor meer informatie.

Ten slotte, als u eventuele fouten in de build hebt opgelost, kunt u wijzigingen aanbrengen aan uw toepassing om te profiteren van nieuwe functionaliteit als u wenst.

<a name="ListOfChangesV1"></a>

## <a name="list-of-breaking-changes-in-version-11"></a>Lijst met belangrijke wijzigingen in versie 1.1
De volgende lijst wordt gesorteerd op de kans dat de wijziging invloed hebben op uw toepassingscode.

### <a name="indexbatch-and-indexaction-changes"></a>Wijzigingen in IndexBatch en IndexAction
`IndexBatch.Create` is gewijzigd in `IndexBatch.New` en heeft niet langer een `params` argument. U kunt `IndexBatch.New` voor batches die verschillende soorten acties (samenvoegingen, verwijderingen, enz.) combineren. Bovendien er zijn nieuwe statische methoden voor het maken van batches waarvoor alle acties, hetzelfde: `Delete`, `Merge`, `MergeOrUpload`, en `Upload`.

`IndexAction` heeft niet langer openbare constructors en de eigenschappen ervan zijn nu onveranderbaar. U moet de nieuwe statische methoden gebruiken voor het maken van acties voor verschillende doeleinden: `Delete`, `Merge`, `MergeOrUpload`, en `Upload`. `IndexAction.Create` is verwijderd. Als u de overbelasting waarvoor alleen een document gebruikt, zorg ervoor dat u `Upload` in plaats daarvan.

#### <a name="example"></a>Voorbeeld
Als uw code ziet er als volgt:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

U kunt deze wijzigen in dit een build-fouten te herstellen:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Als u wilt, kunt u deze verder vereenvoudigen dit:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

### <a name="indexbatchexception-changes"></a>IndexBatchException wijzigingen
De `IndexBatchException.IndexResponse` eigenschap is gewijzigd in `IndexingResults`, en het type is nu `IList<IndexingResult>`.

#### <a name="example"></a>Voorbeeld
Als uw code ziet er als volgt:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

U kunt deze wijzigen in dit een build-fouten te herstellen:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

### <a name="operation-method-changes"></a>Bewerking methode wijzigingen
Elke bewerking in de Azure Search .NET SDK is beschikbaar als een set van methode overloads voor synchrone en asynchrone aanroepers. De handtekeningen en het kiezen van de overloads van deze methode is gewijzigd in versie 1.1.

De bewerking 'Indexstatistieken ophalen' in oudere versies van de SDK blootgesteld bijvoorbeeld deze handtekeningen:

In `IIndexOperations`:

    // Asynchronous operation with all parameters
    Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        string indexName,
        CancellationToken cancellationToken);

In `IndexOperationsExtensions`:

    // Asynchronous operation with only required parameters
    public static Task<IndexGetStatisticsResponse> GetStatisticsAsync(
        this IIndexOperations operations,
        string indexName);

    // Synchronous operation with only required parameters
    public static IndexGetStatisticsResponse GetStatistics(
        this IIndexOperations operations,
        string indexName);

De handtekeningen voor methodes voor dezelfde bewerking in versie 1.1 er als volgt:

In `IIndexesOperations`:

    // Asynchronous operation with lower-level HTTP features exposed
    Task<AzureOperationResponse<IndexGetStatisticsResult>> GetStatisticsWithHttpMessagesAsync(
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        Dictionary<string, List<string>> customHeaders = null,
        CancellationToken cancellationToken = default(CancellationToken));

In `IndexesOperationsExtensions`:

    // Simplified asynchronous operation
    public static Task<IndexGetStatisticsResult> GetStatisticsAsync(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions),
        CancellationToken cancellationToken = default(CancellationToken));

    // Simplified synchronous operation
    public static IndexGetStatisticsResult GetStatistics(
        this IIndexesOperations operations,
        string indexName,
        SearchRequestOptions searchRequestOptions = default(SearchRequestOptions));

Vanaf versie 1.1 is de Azure Search .NET SDK bewerking methoden anders gerangschikt:

* Optionele parameters zijn nu gemodelleerd als parameters in plaats van standaard dan aanvullende methode overloads. Dit vermindert het aantal methode overloads, soms aanzienlijk.
* De uitbreidingsmethoden verbergen nu veel van de details van de overbodige van HTTP van de oproepende functie. Bijvoorbeeld een antwoordobject voor met een HTTP-statuscode u vaak niet controleren moet omdat de bewerking methoden throw in oudere versies van de SDK worden geretourneerd `CloudException` voor een statuscode die een fout aangeeft. De nieuwe uitbreidingsmethoden retourneert alleen modelobjecten, zodat u de moeite van het hoeven ze uitpakken in uw code.
* De belangrijkste interfaces daarentegen nu expose methoden die u meer controle op het niveau van de HTTP-hebt als u deze nodig hebt. U kunt nu doorgeven in aangepaste HTTP-headers moeten worden opgenomen in aanvragen en de nieuwe `AzureOperationResponse<T>` retourtype biedt u directe toegang tot de `HttpRequestMessage` en `HttpResponseMessage` voor de bewerking. `AzureOperationResponse` is gedefinieerd in de `Microsoft.Rest.Azure` naamruimte en vervangt `Hyak.Common.OperationResponse`.

### <a name="scoringparameters-changes"></a>ScoringParameters wijzigingen
Een nieuwe klasse met de naam `ScoringParameter` is toegevoegd aan de nieuwste SDK om eenvoudiger Geef parameters op voor scoreprofielen in een zoekquery. Eerder de `ScoringProfiles` eigenschap van de `SearchParameters` klasse is getypt als `IList<string>`; Nu deze wordt getypt als `IList<ScoringParameter>`.

#### <a name="example"></a>Voorbeeld
Als uw code ziet er als volgt:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

U kunt deze wijzigen in dit een build-fouten te herstellen: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

### <a name="model-class-changes"></a>Wijzigingen in het gegevensmodel klasse
Vanwege de handtekening wijzigingen beschreven in [bewerking methode wijzigingen](#OperationMethodChanges), veel klassen in de `Microsoft.Azure.Search.Models` naamruimte hebt gewijzigd of verwijderd. Bijvoorbeeld:

* `IndexDefinitionResponse` is vervangen door `AzureOperationResponse<Index>`
* De naam van `DocumentSearchResponse` is gewijzigd in `DocumentSearchResult`
* De naam van `IndexResult` is gewijzigd in `IndexingResult`
* `Documents.Count()` retourneert nu een `long` met het documentaantal in plaats van een `DocumentCountResponse`
* De naam van `IndexGetStatisticsResponse` is gewijzigd in `IndexGetStatisticsResult`
* De naam van `IndexListResponse` is gewijzigd in `IndexListResult`

Om samen te vatten, `OperationResponse`-afgeleide klassen die bestonden alleen het inpakken van een modelobject zijn verwijderd. De resterende klassen hebben gehad tot hun achtervoegsel gewijzigd van `Response` naar `Result`.

#### <a name="example"></a>Voorbeeld
Als uw code ziet er als volgt:

    IndexerGetStatusResponse statusResponse = null;

    try
    {
        statusResponse = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = statusResponse.ExecutionInfo.LastResult;

U kunt deze wijzigen in dit een build-fouten te herstellen:

    IndexerExecutionInfo status = null;

    try
    {
        status = _searchClient.Indexers.GetStatus(indexer.Name);
    }
    catch (Exception ex)
    {
        Console.WriteLine("Error polling for indexer status: {0}", ex.Message);
        return;
    }

    IndexerExecutionResult lastResult = status.LastResult;

#### <a name="response-classes-and-ienumerable"></a>Antwoord-klassen en IEnumerable
Een aanvullende wijzigingen die invloed kan zijn op uw code is dat antwoord-klassen die verzamelingen bevatten niet meer implementeren `IEnumerable<T>`. In plaats daarvan kunt u de verzamelingseigenschap rechtstreeks openen. Bijvoorbeeld, als uw code er als volgt uitzien:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

U kunt deze wijzigen in dit een build-fouten te herstellen:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

#### <a name="special-case-for-web-applications"></a>Speciaal geval voor webtoepassingen
Hebt u een webtoepassing die serialiseert `DocumentSearchResponse` direct voor het verzenden van zoekresultaten in de browser, moet u uw code te wijzigen of de resultaten worden niet correct worden geserialiseerd. Bijvoorbeeld, als uw code er als volgt uitzien:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q)
        };
    }

U kunt dit wijzigen door de `.Results` eigenschap van het antwoord van de zoekactie om op te lossen zoeken resultaat rendering:

    public ActionResult Search(string q = "")
    {
        // If blank search, assume they want to search everything
        if (string.IsNullOrWhiteSpace(q))
            q = "*";

        return new JsonResult
        {
            JsonRequestBehavior = JsonRequestBehavior.AllowGet,
            Data = _featuresSearch.Search(q).Results
        };
    }

U moet zoeken naar dergelijke gevallen in uw code zelf; **De compiler waarschuwt u niet** omdat `JsonResult.Data` is van het type `object`.

### <a name="cloudexception-changes"></a>CloudException wijzigingen
De `CloudException` klasse is verplaatst uit de `Hyak.Common` naamruimte voor de `Microsoft.Rest.Azure` naamruimte. Ook de `Error` eigenschap is gewijzigd in `Body`.

### <a name="searchserviceclient-and-searchindexclient-changes"></a>Wijzigingen in SearchServiceClient en SearchIndexClient
Het type van de `Credentials` eigenschap is gewijzigd van `SearchCredentials` naar de basisklasse `ServiceClientCredentials`. Als u nodig hebt voor toegang tot de `SearchCredentials` van een `SearchIndexClient` of `SearchServiceClient`, gebruik de nieuwe `SearchCredentials` eigenschap.

In oudere versies van de SDK, `SearchServiceClient` en `SearchIndexClient` constructors dat heeft geduurd had een `HttpClient` parameter. Deze zijn vervangen door constructors die een `HttpClientHandler` en een matrix van `DelegatingHandler` objecten. Dit maakt het gemakkelijker voor het installeren van aangepaste handlers voor het vooraf verwerken van HTTP-aanvragen indien nodig.

Ten slotte de constructors die nodig was een `Uri` en `SearchCredentials` zijn gewijzigd. Bijvoorbeeld, hebt u code die er als uitzien volgt:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

U kunt deze wijzigen in dit een build-fouten te herstellen:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Ook merk op dat het type van de parameter referenties is gewijzigd in `ServiceClientCredentials`. Dit is het onwaarschijnlijk dat van invloed zijn op uw code sinds `SearchCredentials` is afgeleid van `ServiceClientCredentials`.

### <a name="passing-a-request-id"></a>Doorgeven van een aanvraag-ID
In oudere versies van de SDK, u een aanvraag-ID instellen op de `SearchServiceClient` of `SearchIndexClient` en deze zou worden opgenomen in elke aanvraag voor de REST-API. Dit is handig voor het oplossen van problemen met uw search-service als u contact opnemen met ondersteuning nodig hebt. Het is echter nuttiger zijn om in te stellen voor elke bewerking een unieke aanvraag-ID in plaats van met dezelfde ID voor alle bewerkingen. Om deze reden de `SetClientRequestId` methoden van `SearchServiceClient` en `SearchIndexClient` zijn verwijderd. U kunt een aanvraag-ID in plaats daarvan doorgeven aan de bewerkingsmethode van elke via de optionele `SearchRequestOptions` parameter.

> [!NOTE]
> We gaan een nieuwe mechanisme voor het instellen van een aanvraag-ID wereldwijd op de client objecten die overeenkomt met de benadering die wordt gebruikt door andere Azure-SDK's toevoegen in een toekomstige versie van de SDK.
> 
> 

### <a name="example"></a>Voorbeeld
Als u code die er als uitzien hebt volgt:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

U kunt deze wijzigen in dit een build-fouten te herstellen:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

### <a name="interface-name-changes"></a>Wijzigingen in de naam van de gebruikersinterface
De namen van de gebruikersinterface van de bewerking groep hebt gewijzigd zodat deze consistent zijn met de bijbehorende Eigenschapsnamen:

* Het type `ISearchServiceClient.Indexes` gewijzigd van `IIndexOperations` naar `IIndexesOperations`.
* Het type `ISearchServiceClient.Indexers` gewijzigd van `IIndexerOperations` naar `IIndexersOperations`.
* Het type `ISearchServiceClient.DataSources` gewijzigd van `IDataSourceOperations` naar `IDataSourcesOperations`.
* Het type `ISearchIndexClient.Documents` gewijzigd van `IDocumentOperations` naar `IDocumentsOperations`.

Deze wijziging is klein dat van invloed zijn op uw code, tenzij u mocks van deze interfaces voor test-doeleinden gemaakt.

<a name="BugFixesV1"></a>

## <a name="bug-fixes-in-version-11"></a>Oplossingen voor problemen in versie 1.1
Er is een fout in oudere versies van de Azure Search .NET SDK met betrekking tot serialisatie van aangepast modelklassen. De fout kan zich voordoen als u een aangepaste modelklasse hebt gemaakt met een eigenschap van een waardetype null.

### <a name="steps-to-reproduce"></a>Stappen om te reproduceren
Een aangepast model-klasse maken met een eigenschap van het waardetype null. Voeg bijvoorbeeld een openbare `UnitCount` eigenschap van het type `int` in plaats van `int?`.

Als u een document met de standaardwaarde van dat type index (bijvoorbeeld 0 voor `int`), het veld wordt niet null zijn in Azure Search. Als u vervolgens het document, zoekt de `Search` aanroep genereert `JsonSerializationException` klagen dat kan niet worden geconverteerd `null` naar `int`.

Filters kunnen ook niet werkt zoals verwacht omdat null is geschreven naar de index in plaats van de beoogde waarde.

### <a name="fix-details"></a>Gegevens herstellen
We hebben dit probleem is opgelost in versie 1.1 van de SDK. Nu hebt u een modelklasse als volgt:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

en u stelt `IntValue` op 0, wordt deze waarde is nu correct geserialiseerd als 0 op de kabel en opgeslagen als 0 in de index. Ophalen ook werkt zoals verwacht.

Er is een mogelijk probleem rekening mee moet houden met deze methode: Als u een modeltype met een niet-nullbare eigenschap gebruikt, hebt u **garanderen** dat geen documenten in uw index een null-waarde voor het betreffende veld bevatten. De SDK noch de Azure Search REST API helpt u om af te dwingen dit.

Dit is niet alleen een hypothetische probleem: Stel een scenario waarin u een nieuw veld toevoegen aan een bestaande index die is van het type `Edm.Int32`. Na het bijwerken van de indexdefinitie hebben alle documenten een null-waarde voor het nieuwe veld (omdat alle typen null in Azure Search zijn). Als u vervolgens een modelklasse met een niet-nullbare `int`-eigenschap voor dat veld gebruikt, ontvangt u een `JsonSerializationException` zoals deze bij het ophalen van documenten:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Om deze reden nog steeds wordt aangeraden nullbare typen te gebruiken in uw modelklassen als een best practice.

Zie voor meer informatie over deze fout en de oplossing [dit probleem op GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).

