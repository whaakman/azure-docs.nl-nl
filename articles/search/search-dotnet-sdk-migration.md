---
title: Een upgrade naar de Azure Search .NET SDK versie 1.1 | Microsoft Docs
description: Een upgrade naar de Azure Search .NET SDK versie 1.1
services: search
documentationcenter: 
author: brjohnstmsft
manager: pablocas
editor: 
ms.assetid: 66f89958-a320-4a24-87f9-69315848909f
ms.service: search
ms.devlang: dotnet
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/11/2017
ms.author: brjohnst
ms.openlocfilehash: 9782454e3bfc697b63cde8aa28a14be0c393c36b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="upgrading-to-the-azure-search-net-sdk-version-3"></a>Een upgrade naar de Azure Search .NET SDK versie 3
Als u versie 2.0 preview of oudere van de [Azure Search .NET SDK](https://aka.ms/search-sdk), dit artikel helpt u bij het bijwerken van uw toepassing versie 3 gebruiken.

Zie voor een meer algemene overzicht van de SDK, inclusief voorbeelden [het gebruik van Azure Search vanuit een .NET-toepassing](search-howto-dotnet-sdk.md).

Versie 3 van de Azure Search .NET SDK bevat een aantal wijzigingen uit eerdere versies. Dit zijn vooral kleine, zodat u uw code wijzigt, moet alleen minimale inspanning nodig. Zie [stappen voor het upgraden](#UpgradeSteps) voor instructies over het wijzigen van uw code voor het gebruik van de nieuwe versie van de SDK.

> [!NOTE]
> Als u versie 1.0.2-preview of ouder, moet u een upgrade uitvoeren naar versie 1.1 eerste en vervolgens een upgrade uitvoeren naar versie 3. Zie [bijlage: stappen voor het upgraden naar versie 1.1](#UpgradeStepsV1) voor instructies.
>
> Uw Azure Search-service-exemplaar ondersteunt verschillende versies van de REST-API, met inbegrip van de meest recente versie. U kunt blijven gebruiken van een versie wanneer het is niet meer de meest recente versie, maar het is raadzaam dat u uw code voor het gebruik van de nieuwste versie migreren. Wanneer u de REST-API gebruikt, moet u de API-versie opgeven in elke aanvraag via de api-versie-parameter. Wanneer u de .NET SDK, bepaalt de versie van de SDK die u in de desbetreffende versie van de REST-API. Als u een oudere SDK gebruikt, kunt u blijven code uit te voeren die zonder wijzigingen zelfs als de service is bijgewerkt zodat een nieuwere versie van de API wordt ondersteund.

<a name="WhatsNew"></a>

## <a name="whats-new-in-version-3"></a>Wat is er nieuw in versie 3
Versie 3 van de Azure Search .NET SDK gericht is op de meest recente versie van de Azure Search REST API, speciaal 2016-09-01 van algemeen beschikbaar. Dit maakt het mogelijk te veel nieuwe functies van Azure Search vanuit een .NET-toepassing, waaronder de volgende gebruiken:

* [Analysevoorzieningen aanpassen](https://aka.ms/customanalyzers)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md) en [Azure Table Storage](search-howto-indexing-azure-tables.md) indexeerfunctie-ondersteuning
* Aanpassing van de indexeerfunctie via [veld toewijzingen](search-indexer-field-mappings.md)
* ETags ondersteunen om in te schakelen veilige gelijktijdige bijwerken van definities van de index, Indexeerfuncties en gegevensbronnen
* Ondersteuning voor het bouwen van de index velddefinities declaratief met versieren van uw modelklasse en het gebruik van de nieuwe `FieldBuilder` klasse.
* Ondersteuning voor .NET Core en draagbare .NET-profiel 111

<a name="UpgradeSteps"></a>

## <a name="steps-to-upgrade"></a>Stappen voor het upgraden
Eerst uw NuGet-verwijzing voor bijwerken `Microsoft.Azure.Search` via de NuGet Package Manager-Console of door met de rechtermuisknop op uw projectverwijzingen en 'Beheren NuGet-pakketten...' selecteren in Visual Studio.

Zodra de NuGet heeft de nieuwe pakketten en afhankelijkheden zijn gedownload, opnieuw worden opgebouwd uw project. Afhankelijk van hoe uw code is gestructureerd, deze mogelijk opnieuw worden opgebouwd is. Zo ja, bent u nu klaar voor!

Als uw build is mislukt, ziet u een build-fout als volgt:

    Program.cs(31,45,31,86): error CS0266: Cannot implicitly convert type 'Microsoft.Azure.Search.ISearchIndexClient' to 'Microsoft.Azure.Search.SearchIndexClient'. An explicit conversion exists (are you missing a cast?)

De volgende stap is om deze build-fout te corrigeren. Zie [wijzigingen op te splitsen in versie 3](#ListOfChanges) voor meer informatie over wat de fout veroorzaakt en hoe u dit herstelt.

Mogelijk ziet u extra build-waarschuwingen verouderd methoden of eigenschappen. De waarschuwingen bevat instructies over wat u moet gebruiken in plaats van de afgeschafte functies. Als uw toepassing gebruikt bijvoorbeeld de `IndexingParameters.Base64EncodeKeys` eigenschap, krijgt u een waarschuwing dat`"This property is obsolete. Please create a field mapping using 'FieldMapping.Base64Encode' instead."`

Als u eventuele fouten in de build hebt opgelost, kunt u wijzigingen aanbrengen aan uw toepassing om te profiteren van nieuwe functionaliteit als u wenst. Nieuwe functies in de SDK worden beschreven in [wat is er nieuw in versie 3](#WhatsNew).

<a name="ListOfChanges"></a>

## <a name="breaking-changes-in-version-3"></a>Wijzigingen op te splitsen in versie 3
Er is een klein aantal wijzigingen op te splitsen in versie 3 waarvoor code wijzigingen naast opnieuw opbouwen van uw toepassing.

### <a name="indexesgetclient-return-type"></a>Het retourtype Indexes.GetClient
De `Indexes.GetClient` methode heeft een nieuwe retourtype. Voorheen geretourneerd `SearchIndexClient`, maar dit is gewijzigd in `ISearchIndexClient` in preview-versie 2.0 en die wijziging geldt ook voor versie 3. Dit is de ondersteuning van klanten die willen model de `GetClient` methode voor de eenheidstests door te retourneren van een mock-implementatie van `ISearchIndexClient`.

#### <a name="example"></a>Voorbeeld
Als uw code ziet er als volgt:

```csharp
SearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

U kunt deze wijzigen in dit herstel de fouten build:

```csharp
ISearchIndexClient indexClient = serviceClient.Indexes.GetClient("hotels");
```

### <a name="analyzername-datatype-and-others-are-no-longer-implicitly-convertible-to-strings"></a>AnalyzerName, gegevenstype en andere zijn niet langer impliciet te converteren naar tekenreeksen
Er zijn vele typen in de Azure Search .NET SDK, die zijn afgeleid van `ExtensibleEnum`. Eerder deze typen zijn alle impliciet omgezet naar type `string`. Echter een probleem is gedetecteerd op de `Object.Equals` implementatie voor deze klassen en het verhelpen van de fout vereist deze impliciete conversie uit te schakelen. Expliciete conversie naar `string` is wel toegestaan.

#### <a name="example"></a>Voorbeeld
Als uw code ziet er als volgt:

```csharp
var customTokenizerName = TokenizerName.Create("my_tokenizer"); 
var customTokenFilterName = TokenFilterName.Create("my_tokenfilter"); 
var customCharFilterName = CharFilterName.Create("my_charfilter"); 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        customTokenizerName,  
        new[] { customTokenFilterName },  
        new[] { customCharFilterName }), 
}; 
```

U kunt deze wijzigen in dit herstel de fouten build:

```csharp
const string CustomTokenizerName = "my_tokenizer"; 
const string CustomTokenFilterName = "my_tokenfilter"; 
const string CustomCharFilterName = "my_charfilter"; 
 
var index = new Index();
index.Analyzers = new Analyzer[] 
{ 
    new CustomAnalyzer( 
        "my_analyzer",  
        CustomTokenizerName,  
        new TokenFilterName[] { CustomTokenFilterName },  
        new CharFilterName[] { CustomCharFilterName })
}; 
```

### <a name="removed-obsolete-members"></a>Verouderd leden verwijderd

Mogelijk ziet u build fouten met betrekking tot methoden of eigenschappen die zijn gemarkeerd als verouderd in versie 2.0-preview en vervolgens verwijderd in versie 3. Als u dergelijke fouten optreden, wordt hier hoe u deze op te lossen:

- Als u deze constructor: `ScoringParameter(string name, string value)`, gebruik in plaats daarvan deze:`ScoringParameter(string name, IEnumerable<string> values)`
- Als u de `ScoringParameter.Value` eigenschap, gebruikt de `ScoringParameter.Values` eigenschap of de `ToString` methode in plaats daarvan.
- Als u de `SearchRequestOptions.RequestId` eigenschap, gebruikt de `ClientRequestId` eigenschap in plaats daarvan.

### <a name="removed-preview-features"></a>Verwijderde preview-functies

Let als bij een van versie 2.0 preview-versie 3 upgrade of JSON- en CSV-ondersteuning voor Blob indexeerfuncties parseren is verwijderd omdat deze functies zijn nog steeds in preview. In het bijzonder de volgende methoden van de `IndexingParametersExtensions` klasse zijn verwijderd:

- `ParseJson`
- `ParseJsonArrays`
- `ParseDelimitedTextFiles`

Als uw toepassing een vaste afhankelijkheid van deze functies heeft, kunt u zich niet bijwerken naar versie 3 van de Azure Search .NET SDK. U kunt blijven gebruiken preview-versie 2.0. Echter, neem Houd rekening met het **we raden niet met behulp van voorbeeld SDK's in productietoepassingen**. Preview-functies voor evaluatie alleen zijn en kunnen worden gewijzigd.

## <a name="conclusion"></a>Conclusie
Als u meer informatie over het gebruik van Azure Search .NET SDK nodig hebt, raadpleegt u onze onlangs bijgewerkt [How-to](search-howto-dotnet-sdk.md).

Uw feedback is Welkom op de SDK. Als u problemen ondervindt, gerust ons om hulp te vragen op de [Azure Search MSDN-forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=azuresearch). Als u een bug vinden, kunt u het bestand een probleem in de [Azure .NET SDK GitHub-opslagplaats](https://github.com/Azure/azure-sdk-for-net/issues). Zorg ervoor dat de titel van uw probleem met het voorvoegsel ' Search SDK: '.

Hartelijk dank voor het gebruik van Azure Search.

<a name="UpgradeStepsV1"></a>

## <a name="appendix-steps-to-upgrade-to-version-11"></a>Bijlage: Stappen voor het upgraden naar versie 1.1
> [!NOTE]
> Deze sectie geldt alleen voor gebruikers van de Azure Search .NET SDK versie 1.0.2-preview en ouder.
> 
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

