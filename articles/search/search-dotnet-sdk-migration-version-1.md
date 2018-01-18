---
title: Een upgrade naar de Azure Search .NET SDK versie 1.1 | Microsoft Docs
description: Een upgrade naar de Azure Search .NET SDK versie 1.1
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/15/2018
ms.author: brjohnst
ms.openlocfilehash: 387a052a116388cc9ad816ec8b339347d5c28322
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-11"></a>Een upgrade naar de Azure Search .NET SDK versie 1.1

Als u versie 1.0.2-preview of ouder van de [Azure Search .NET SDK](https://aka.ms/search-sdk), dit artikel helpt u bij het bijwerken van uw toepassing versie 1.1 te gebruiken.

Zie voor een meer algemene overzicht van de SDK, inclusief voorbeelden [het gebruik van Azure Search vanuit een .NET-toepassing](search-howto-dotnet-sdk.md).

> [!NOTE]
> Na de upgrade naar versie 1.1 of als u al een versie tussen 1.1 en 2.0-preview liggen, moet u upgraden naar versie 3. Zie [upgraden naar Azure Search .NET SDK versie 3](search-dotnet-sdk-migration.md) voor instructies.
>

Eerst uw NuGet-verwijzing voor bijwerken `Microsoft.Azure.Search` via de NuGet Package Manager-Console of door met de rechtermuisknop op uw projectverwijzingen en 'Beheren NuGet-pakketten...' selecteren in Visual Studio.

Zodra de NuGet heeft de nieuwe pakketten en afhankelijkheden zijn gedownload, opnieuw worden opgebouwd uw project.

Als u eerder versie 1.0.0-preview, 1.0.1-preview of 1.0.2-preview, de build moet slagen en u bent nu klaar voor!

Als u eerder versie 0.13.0-preview of ouder, ziet u bouwen fouten als volgt:

    Program.cs(137,56,137,62): error CS0117: 'Microsoft.Azure.Search.Models.IndexBatch' does not contain a definition for 'Create'
    Program.cs(137,99,137,105): error CS0117: 'Microsoft.Azure.Search.Models.IndexAction' does not contain a definition for 'Create'
    Program.cs(146,41,146,54): error CS1061: 'Microsoft.Azure.Search.IndexBatchException' does not contain a definition for 'IndexResponse' and no extension method 'IndexResponse' accepting a first argument of type 'Microsoft.Azure.Search.IndexBatchException' could be found (are you missing a using directive or an assembly reference?)
    Program.cs(163,13,163,42): error CS0246: The type or namespace name 'DocumentSearchResponse' could not be found (are you missing a using directive or an assembly reference?)

De volgende stap is de build-fouten één voor één te herstellen. De meeste is vereist voor het wijzigen van sommige klasse en methode namen die zijn gewijzigd in de SDK. [Lijst met wijzigingen op te splitsen in versie 1.1](#ListOfChangesV1) bevat een overzicht van deze wijzigingen in de naam.

Als u aangepaste klassen om uw documenten te modelleren en deze klassen eigenschappen met niet-nullbare primitieve typen hebben (bijvoorbeeld `int` of `bool` in C#), er is een fix bug in de versie 1.1 van de SDK van waarmee u rekening moet houden. Zie [oplossingen voor problemen in versie 1.1](#BugFixesV1) voor meer informatie.

Als u eventuele fouten in de build hebt opgelost, kunt u tot slot wijzigingen aanbrengen aan uw toepassing om te profiteren van nieuwe functionaliteit als u wenst.

<a name="ListOfChangesV1"></a>

### <a name="list-of-breaking-changes-in-version-11"></a>Lijst met wijzigingen op te splitsen in versie 1.1
De volgende lijst is gesorteerd door de kans dat de wijziging invloed is op uw toepassingscode.

#### <a name="indexbatch-and-indexaction-changes"></a>Wijzigingen in IndexBatch en IndexAction
`IndexBatch.Create`is gewijzigd in `IndexBatch.New` en niet langer heeft een `params` argument. U kunt `IndexBatch.New` voor batches die een mix van verschillende soorten acties (samenvoegingen, verwijderingen, enzovoort). Bovendien, er zijn nieuwe statische methoden voor het maken van batches waar alle acties hetzelfde zijn: `Delete`, `Merge`, `MergeOrUpload`, en `Upload`.

`IndexAction`Openbare constructors beschikt niet langer over en de eigenschappen zijn nu niet-wijzigbaar. U moet de nieuwe statische methoden voor het maken van acties voor verschillende doeleinden gebruiken: `Delete`, `Merge`, `MergeOrUpload`, en `Upload`. `IndexAction.Create`is verwijderd. Als u de overbelasting waarvoor alleen een document gebruikt, moet u `Upload` in plaats daarvan.

##### <a name="example"></a>Voorbeeld
Als uw code ziet er als volgt:

    var batch = IndexBatch.Create(documents.Select(doc => IndexAction.Create(doc)));
    indexClient.Documents.Index(batch);

U kunt deze wijzigen in dit herstel de fouten build:

    var batch = IndexBatch.New(documents.Select(doc => IndexAction.Upload(doc)));
    indexClient.Documents.Index(batch);

Als u wilt, kunt u het verdere vereenvoudigen hierop:

    var batch = IndexBatch.Upload(documents);
    indexClient.Documents.Index(batch);

#### <a name="indexbatchexception-changes"></a>IndexBatchException wijzigingen
De `IndexBatchException.IndexResponse` eigenschap is gewijzigd in `IndexingResults`, en het type is nu `IList<IndexingResult>`.

##### <a name="example"></a>Voorbeeld
Als uw code ziet er als volgt:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexResponse.Results.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

U kunt deze wijzigen in dit herstel de fouten build:

    catch (IndexBatchException e)
    {
        Console.WriteLine(
            "Failed to index some of the documents: {0}",
            String.Join(", ", e.IndexingResults.Where(r => !r.Succeeded).Select(r => r.Key)));
    }

<a name="OperationMethodChanges"></a>

#### <a name="operation-method-changes"></a>Bewerking methode wijzigingen
Elke bewerking in de Azure Search .NET SDK is beschikbaar als een set van methode overloads voor synchrone en asynchrone aanroepfuncties. De handtekeningen en waarbij van deze methode overloads is in versie 1.1 gewijzigd.

De bewerking 'indexstatistieken ophalen' in oudere versies van de SDK blootgesteld bijvoorbeeld deze handtekeningen:

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

De methode handtekeningen voor dezelfde bewerking in versie 1.1 moeten uitzien:

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

Vanaf versie 1.1, ordent de Azure Search .NET SDK bewerking methoden anders:

* Optionele parameters zijn nu gemodelleerd als parameters in plaats daarvan standaard dan aanvullende methode overloads. Hierdoor het aantal methode overloads, soms aanzienlijk wordt gereduceerd.
* De uitbreidingsmethoden verbergen nu veel overbodige gegevens van de aanroeper van HTTP. Bijvoorbeeld: oudere versies van de SDK een antwoordobject met een HTTP-statuscode u vaak niet controleren moet omdat de bewerking methoden genereert een geretourneerd `CloudException` voor elke statuscode die een fout aangeeft. De nieuwe uitbreidingsmethoden retourneren alleen modelobjecten, zodat u het niet meer hoeft uit te pakken ze in uw code.
* Als u daarentegen interfaces de kern nu zichtbaar methoden waarmee u meer controle op het niveau van de HTTP-als u deze nodig hebt. U kunt nu doorgeven in aangepaste HTTP-headers moeten worden opgenomen in aanvragen en de nieuwe `AzureOperationResponse<T>` retourtype biedt u directe toegang tot de `HttpRequestMessage` en `HttpResponseMessage` voor de bewerking. `AzureOperationResponse`is gedefinieerd in de `Microsoft.Rest.Azure` naamruimte en vervangt `Hyak.Common.OperationResponse`.

#### <a name="scoringparameters-changes"></a>ScoringParameters wijzigingen
Een nieuwe klasse met de naam `ScoringParameter` is toegevoegd aan de nieuwste SDK om eenvoudiger Geef parameters op voor score berekenen voor profielen in een zoekopdracht. Eerder de `ScoringProfiles` eigenschap van de `SearchParameters` klasse was getypeerd als `IList<string>`; Nu het is getypeerd als `IList<ScoringParameter>`.

##### <a name="example"></a>Voorbeeld
Als uw code ziet er als volgt:

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters = new[] { "featuredParam-featured", "mapCenterParam-" + lon + "," + lat };

U kunt deze wijzigen in dit herstel de fouten build: 

    var sp = new SearchParameters();
    sp.ScoringProfile = "jobsScoringFeatured";      // Use a scoring profile
    sp.ScoringParameters =
        new[]
        {
            new ScoringParameter("featuredParam", new[] { "featured" }),
            new ScoringParameter("mapCenterParam", GeographyPoint.Create(lat, lon))
        };

#### <a name="model-class-changes"></a>Wijzigingen in het gegevensmodel klasse
Als gevolg van de handtekening wijzigingen beschreven in [bewerking methode wijzigingen](#OperationMethodChanges), veel klassen in de `Microsoft.Azure.Search.Models` naamruimte hebt gewijzigd of verwijderd. Bijvoorbeeld:

* `IndexDefinitionResponse`is vervangen door`AzureOperationResponse<Index>`
* De naam van `DocumentSearchResponse` is gewijzigd in `DocumentSearchResult`
* De naam van `IndexResult` is gewijzigd in `IndexingResult`
* `Documents.Count()`nu retourneert een `long` met het aantal documenten in plaats van een`DocumentCountResponse`
* De naam van `IndexGetStatisticsResponse` is gewijzigd in `IndexGetStatisticsResult`
* De naam van `IndexListResponse` is gewijzigd in `IndexListResult`

Samengevat: `OperationResponse`-afgeleide klassen die bestonden alleen inpakken van een modelobject zijn verwijderd. De resterende klassen hebben gehad hun achtervoegsel gewijzigd van `Response` naar `Result`.

##### <a name="example"></a>Voorbeeld
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

U kunt deze wijzigen in dit herstel de fouten build:

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

##### <a name="response-classes-and-ienumerable"></a>Antwoord klassen en IEnumerable
Een aanvullende wijzigen die invloed kan zijn op uw code is dat antwoord-klassen die verzamelingen houdt niet langer implementeren `IEnumerable<T>`. In plaats daarvan kunt u de eigenschap collection rechtstreeks openen. Als bijvoorbeeld uw code ziet er als volgt:

    DocumentSearchResponse<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response)
    {
        Console.WriteLine(result.Document);
    }

U kunt deze wijzigen in dit herstel de fouten build:

    DocumentSearchResult<Hotel> response = indexClient.Documents.Search<Hotel>(searchText, sp);
    foreach (SearchResult<Hotel> result in response.Results)
    {
        Console.WriteLine(result.Document);
    }

##### <a name="special-case-for-web-applications"></a>Speciaal geval voor webtoepassingen
Als u een webtoepassing die serialiseert `DocumentSearchResponse` direct wilt zoekresultaten verzenden naar de browser, moet u uw code te wijzigen of de resultaten niet correct worden geconverteerd. Als bijvoorbeeld uw code ziet er als volgt:

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

U moet letten dergelijke gevallen in uw code zelf; **De compiler wordt geen waarschuwing** omdat `JsonResult.Data` is van het type `object`.

#### <a name="cloudexception-changes"></a>CloudException wijzigingen
De `CloudException` klasse is verplaatst van de `Hyak.Common` -naamruimte om aan de `Microsoft.Rest.Azure` naamruimte. Ook de `Error` eigenschap is gewijzigd in `Body`.

#### <a name="searchserviceclient-and-searchindexclient-changes"></a>Wijzigingen in SearchServiceClient en SearchIndexClient
Het type van de `Credentials` eigenschap is gewijzigd van `SearchCredentials` naar de basisklasse `ServiceClientCredentials`. Als u nodig hebt voor toegang tot de `SearchCredentials` van een `SearchIndexClient` of `SearchServiceClient`, gebruik de nieuwe `SearchCredentials` eigenschap.

In oudere versies van de SDK `SearchServiceClient` en `SearchIndexClient` had constructors die een `HttpClient` parameter. Deze zijn vervangen door een constructors die een `HttpClientHandler` en een matrix van `DelegatingHandler` objecten. Dit vergemakkelijkt het installeren van aangepaste handlers voor het vooraf verwerken HTTP-aanvragen, indien nodig.

Ten slotte de constructors die een `Uri` en `SearchCredentials` zijn gewijzigd. Bijvoorbeeld, als er een code die er als volgt uit:

    var client =
        new SearchServiceClient(
            new SearchCredentials("abc123"),
            new Uri("http://myservice.search.windows.net"));

U kunt deze wijzigen in dit herstel de fouten build:

    var client =
        new SearchServiceClient(
            new Uri("http://myservice.search.windows.net"),
            new SearchCredentials("abc123"));

Ook Let op het type van de parameter referenties is gewijzigd in `ServiceClientCredentials`. Dit is waarschijnlijk niet van invloed zijn op uw code sinds `SearchCredentials` is afgeleid van `ServiceClientCredentials`.

#### <a name="passing-a-request-id"></a>Een aanvraag-ID doorgeven
In oudere versies van de SDK kan stelt u een aanvraag-ID op de `SearchServiceClient` of `SearchIndexClient` en zou worden opgenomen in elke aanvraag naar de REST-API. Dit is handig voor het oplossen van problemen met uw search-service als u contact op met ondersteuning. Het is echter nuttiger om in te stellen van een unieke aanvraag-ID voor elke bewerking in plaats van met dezelfde ID voor alle bewerkingen. Daarom moet de `SetClientRequestId` methoden van `SearchServiceClient` en `SearchIndexClient` zijn verwijderd. U kunt een aanvraag-ID in plaats daarvan doorgeven aan de bewerkingsmethode van elke via de optionele `SearchRequestOptions` parameter.

> [!NOTE]
> In een toekomstige versie van de SDK wordt er een nieuw mechanisme voor het instellen van een aanvraag-ID globaal op de client objecten die consistent zijn met de methode die wordt gebruikt door andere Azure-SDK toevoegen.
> 
> 

#### <a name="example"></a>Voorbeeld
Als u hebt de code die er als volgt uit:

    client.SetClientRequestId(Guid.NewGuid());
    ...
    long count = client.Documents.Count();

U kunt deze wijzigen in dit herstel de fouten build:

    long count = client.Documents.Count(new SearchRequestOptions(requestId: Guid.NewGuid()));

#### <a name="interface-name-changes"></a>Wijzigingen in de naam van de gebruikersinterface
Interfacenamen voor de bewerking hebt gewijzigd zodat deze consistent zijn met de bijbehorende Eigenschapsnamen:

* Het type `ISearchServiceClient.Indexes` gewijzigd van `IIndexOperations` naar `IIndexesOperations`.
* Het type `ISearchServiceClient.Indexers` gewijzigd van `IIndexerOperations` naar `IIndexersOperations`.
* Het type `ISearchServiceClient.DataSources` gewijzigd van `IDataSourceOperations` naar `IDataSourcesOperations`.
* Het type `ISearchIndexClient.Documents` gewijzigd van `IDocumentOperations` naar `IDocumentsOperations`.

Deze wijziging is waarschijnlijk niet van invloed zijn op uw code tenzij u mocks van deze interfaces voor testdoeleinden gemaakt.

<a name="BugFixesV1"></a>

### <a name="bug-fixes-in-version-11"></a>Oplossingen voor problemen in versie 1.1
Er is een fout in oudere versies van de Azure Search .NET SDK met betrekking tot serialisatie van aangepaste modelklassen. Deze fout kan optreden als u een aangepaste modelklasse met een eigenschap van een niet-nullbare waardetype gemaakt.

#### <a name="steps-to-reproduce"></a>Stappen om te reproduceren
Maak een aangepaste modelklasse met een eigenschap van niet-nullbare waardetype. Bijvoorbeeld, Voeg een openbare `UnitCount` eigenschap van het type `int` in plaats van `int?`.

Als u een document met de standaardwaarde van dat type index (bijvoorbeeld 0 voor `int`), het veld wordt niet null zijn in Azure Search. Als u later naar dat document zoeken de `Search` aanroep genereert `JsonSerializationException` klagen dat kan niet worden geconverteerd `null` naar `int`.

Ook filters werkt mogelijk niet zoals verwacht omdat null is geschreven naar de index in plaats van de beoogde waarde.

#### <a name="fix-details"></a>Gegevens herstellen
We hebben dit probleem opgelost in versie 1.1 van de SDK. Op dit moment hebt u een modelklasse als volgt:

    public class Model
    {
        public string Key { get; set; }

        public int IntValue { get; set; }
    }

en u `IntValue` op 0, wordt die waarde wordt nu correct geserialiseerd als 0 op de kabel en opgeslagen als 0 in de index. Ophalen ook werkt zoals verwacht.

Er is een mogelijk probleem moet houden met deze methode: als u een modeltype met een niet-nullbare eigenschap gebruikt, hebt u **garanderen** dat de documenten in uw index geen null-waarde voor het betreffende veld bevatten. De SDK noch de Azure Search REST-API kunt u dit afdwingen.

Dit is niet alleen een hypothetische probleem: Stel een scenario voor waarin u een nieuw veld toevoegt aan een bestaande index van het type `Edm.Int32`. Na het bijwerken van de indexdefinitie hebben alle documenten een null-waarde voor het nieuwe veld (omdat alle typen null in Azure Search zijn). Als u vervolgens een modelklasse met een niet-nullbare `int`-eigenschap voor dat veld gebruikt, ontvangt u een `JsonSerializationException` zoals deze bij het ophalen van documenten:

    Error converting value {null} to type 'System.Int32'. Path 'IntValue'.

Daarom nog steeds wordt aangeraden nullbare typen te gebruiken in uw modelklassen als een best practice.

Zie voor meer informatie over deze fout en de oplossing [dit probleem op GitHub](https://github.com/Azure/azure-sdk-for-net/issues/1063).

