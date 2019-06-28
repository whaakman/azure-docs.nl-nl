---
title: 'Zelfstudie: Meerdere gegevensbronnen - Azure Search-index'
description: Leer hoe u gegevens uit meerdere gegevensbronnen importeren in een enkele Azure Search-index.
author: RobDixon22
manager: HeidiSteen
services: search
ms.service: search
ms.topic: tutorial
ms.date: 06/21/2019
ms.author: v-rodixo
ms.custom: seodec2018
ms.openlocfilehash: 4186c422836771de4f8a283616d77214b91bfc02
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67462700"
---
# <a name="c-tutorial-combine-data-from-multiple-data-sources-in-one-azure-search-index"></a>C#Zelfstudie: Combineer gegevens uit meerdere gegevensbronnen in een Azure Search-index

Azure Search kunt importeren, analyseren en indexeren van gegevens uit meerdere bronnen in één gecombineerde zoekindex. Dit biedt ondersteuning voor situaties waarin gestructureerde gegevens is samengevoegd met de tekst minder gestructureerd of zelfs gegevens uit andere bronnen, zoals tekst, HTML, of JSON-documenten.

In deze zelfstudie wordt beschreven hoe u Hotelgegevens uit een Azure Cosmos DB-gegevensbron indexeren en die samen te voegen met hotel room informatie opgehaald uit Azure Blob Storage-documenten. Het resultaat is een gecombineerde hotel search-index met complexe gegevenstypen.

Deze zelfstudie wordt gebruikgemaakt van C#, de .NET-SDK voor Azure Search en Azure portal naar de volgende taken uitvoeren:

> [!div class="checklist"]
> * Voorbeeldgegevens uploaden en gegevensbronnen maken
> * De documentsleutel identificeren
> * Definiëren en de index maken
> * Gegevens van de index hotels van cosmos DB
> * Samenvoegen hotel room gegevens uit blob storage

## <a name="prerequisites"></a>Vereisten

De volgende services, hulpprogramma's en gegevens worden gebruikt in deze Quick Start. 

- [Maak een Azure Search-service](search-create-service-portal.md) of [vinden van een bestaande service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in uw huidige abonnement. U kunt een gratis service voor deze zelfstudie gebruiken.

- [Maak een Azure Cosmos DB-account](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) voor het opslaan van het hotel voorbeeldgegevens.

- [Een Azure storage-account maken](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account) blob voor het opslaan van de voorbeeld-JSON-gegevens.

- [Visual Studio installeren](https://visualstudio.microsoft.com/) om te gebruiken als de IDE.

### <a name="install-the-project-from-github"></a>Installeren van het project via GitHub

1. Ga naar de opslagplaats met voorbeelden op GitHub: [azure-search-dotnet-samples](https://github.com/Azure-Samples/azure-search-dotnet-samples).
1. Selecteer **klonen of downloaden** en maakt u uw persoonlijke lokale kopie van de opslagplaats.
1. Open Visual Studio en installeer de Microsoft Azure Search NuGet-pakket, indien niet reeds geïnstalleerd. In de **extra** in het menu **NuGet Package Manager** en vervolgens **NuGet-pakketten beheren voor oplossing...** . Selecteer de **Bladeren** tab en typt u 'Azure Search' in het zoekvak. Installeer **Microsoft.Azure.Search** wanneer deze wordt weergegeven in de lijst (versie 9.0.1, of hoger). U moet doorklikken extra dialoogvensters om de installatie te voltooien.

    ![Azure-bibliotheken toevoegen met behulp van NuGet](./media/tutorial-csharp-create-first-app/azure-search-nuget-azure.png)

1. Met behulp van Visual Studio, navigeer naar uw lokale opslagplaats en open het oplossingsbestand **AzureSearchMultipleDataSources.sln**.

## <a name="get-a-key-and-url"></a>Een sleutel en -URL ophalen

Om te communiceren met uw Azure Search-service, moet u de service-URL en een toegangssleutel. Een zoekservice wordt gemaakt met beide, dus als u Azure Search hebt toegevoegd aan uw abonnement, volgt u deze stappen om de benodigde gegevens op te halen:

1. [Meld u aan bij Azure portal](https://portal.azure.com/), en in uw zoekservice **overzicht** pagina, de URL ophalen. Een eindpunt ziet er bijvoorbeeld uit als `https://mydemo.search.windows.net`.

1. In **instellingen** > **sleutels**, een beheersleutel voor volledige rechten voor de service ophalen. Er zijn twee uitwisselbaar beheersleutels, verstrekt voor bedrijfscontinuïteit voor het geval u moet een meegenomen. U kunt de primaire of secundaire sleutel gebruiken voor verzoeken voor toevoegen, wijzigen en verwijderen van objecten.

![Een HTTP-eindpunt en -sleutel ophalen](media/search-fiddler/get-url-key.png "een HTTP-eindpunt en -sleutel ophalen")

Alle aanvragen vereisen een api-sleutel bij elke aanvraag verzonden naar uw service. Een geldige sleutel stelt trust, op basis van per aanvraag, tussen het verzenden van de aanvraag en de service die verantwoordelijk is voor deze toepassing.

## <a name="prepare-sample-azure-cosmos-db-data"></a>Voorbeeld van Azure Cosmos DB-gegevens voorbereiden

In dit voorbeeld maakt gebruik van twee kleine sets met gegevens die zeven fictieve hotels beschrijven. Één set beschrijft de hotels zelf, en worden geladen in een Azure Cosmos DB-database. De andere set hotel room details bevat, en wordt geleverd als zeven afzonderlijke JSON-bestanden worden geüpload naar Azure Blob Storage.

1. [Meld u aan bij Azure portal](https://portal.azure.com), en navigeert u vervolgens de overzichtspagina van uw Azure Cosmos DB-account.

1. Klik in de menubalk op Container toevoegen. Geef 'Nieuwe database maken' en gebruikt u de naam van de **hotel-ruimten-db**. Voer **hotel-ruimten** voor de naam van de verzameling en **/HotelId** voor de partitiesleutel. Klik op **OK** om de database en de container te maken.

   ![Azure Cosmos DB-container toevoegen](media/tutorial-multiple-data-sources/cosmos-add-container.png "een Azure Cosmos DB-container toevoegen")

1. Ga naar de Cosmos DB Data Explorer en selecteer de **items** element onder de **hotels** container binnen de **hotel-ruimten-db** database. Klik vervolgens op **-Item uploaden** op de opdrachtbalk.

   ![Uploaden naar Azure Cosmos DB-verzameling](media/tutorial-multiple-data-sources/cosmos-upload.png "uploaden naar Cosmos DB-verzameling")

1. In het deelvenster uploaden, klikt u op de mapknop en navigeer vervolgens naar het bestand **cosmosdb/HotelsDataSubset_CosmosDb.json** in de projectmap. Klik op **OK** om te beginnen met uploaden.

   ![Bestand selecteren om te uploaden](media/tutorial-multiple-data-sources/cosmos-upload2.png "selecteren dat moet worden geüpload")

1. Gebruik de knop Vernieuwen om uw weergave van de items in de verzameling hotels te vernieuwen. Hier ziet u zeven nieuwe databasedocumenten die worden weergegeven.

## <a name="prepare-sample-blob-data"></a>Voorbeeld van blob-gegevens voorbereiden

1. [Meld u aan bij Azure portal](https://portal.azure.com), gaat u naar uw Azure storage-account, klikt u op **Blobs**, en klik vervolgens op **+ Container**.

1. [Maak een blobcontainer](https://docs.microsoft.com/azure/storage/blobs/storage-quickstart-blobs-portal) met de naam **hotel-ruimten** voor het opslaan van de steekproef hotel room JSON-bestanden. U kunt het niveau van de openbare toegang instellen op een van de geldige waarden.

   ![Maak een blobcontainer](media/tutorial-multiple-data-sources/blob-add-container.png "een blob-container maken")

1. Nadat de container is gemaakt, opent u het en selecteer **uploaden** op de opdrachtbalk.

   ![Uploaden op de opdrachtbalk](media/search-semi-structured-data/upload-command-bar.png "uploaden op de opdrachtbalk")

1. Navigeer naar de map met de voorbeeldbestanden. Selecteer alle mappen en klik vervolgens op **uploaden**.

   ![Bestanden uploaden](media/tutorial-multiple-data-sources/blob-upload.png "bestanden uploaden")

Nadat het uploaden is voltooid, moeten de bestanden worden weergegeven in de lijst voor de gegevenscontainer.

## <a name="set-up-connections"></a>Verbindingen instellen

Verbindingsgegevens voor de search-service en de gegevensbronnen die is opgegeven in de **appsettings.json** bestand in de oplossing. 

1. Open in Visual Studio, de **AzureSearchMultipleDataSources.sln** bestand.

1. Bewerken in Solution Explorer de **appsettings.json** bestand.  

```json
{
  "SearchServiceName": "Put your search service name here",
  "SearchServiceAdminApiKey": "Put your primary or secondary API key here",
  "BlobStorageAccountName": "Put your Azure Storage account name here",
  "BlobStorageConnectionString": "Put your Azure Blob Storage connection string here",
  "CosmosDBConnectionString": "Put your Cosmos DB connection string here",
  "CosmosDBDatabaseName": "hotel-rooms-db"
}
```

De eerste twee items gebruiken de URL en beheer sleutels voor uw Azure Search-service. Een eindpunt van de opgegeven `https://mydemo.search.windows.net`, bijvoorbeeld de naam van de service te geven is `mydemo`.

De volgende vermeldingen opgeven accountnamen en verbindingsinformatie voor de Azure Blob Storage en Azure Cosmos DB-gegevensbronnen.

### <a name="identify-the-document-key"></a>De documentsleutel identificeren

In Azure Search identificatie het sleutelveld unieke van elk document in de index. Elke search-index moet exact één sleutelveld van het type `Edm.String`. Veld met deze sleutel moet aanwezig zijn voor elk document in een gegevensbron die wordt toegevoegd aan de index. (In feite, dit is de enige vereiste veld.)

Bij het indexeren van gegevens uit meerdere gegevensbronnen, moet elke data source-sleutelwaarde worden toegewezen aan de dezelfde sleutelveld in de gecombineerde index. Hiervoor wordt vaak een vooraf planning om te bepalen van een zinvolle documentsleutel voor de index en zorg ervoor dat het zich bevindt in elke gegevensbron.

Azure Search-Indexeerfuncties kunnen veldtoewijzingen gebruiken om te wijzigen en zelfs opnieuw opmaken van gegevens tijdens het indexeringsproces worden geautomatiseerd, zodat gegevens kunnen worden omgeleid naar de juiste indexveld.

Bijvoorbeeld, de id van het hotel wordt genoemd in onze CosmosDB voorbeeldgegevens **HotelId**. Maar in de JSON-blob-bestanden voor de ruimten hotel, de id van het hotel heet **Id**. Het programma doet dit door het toewijzen van de **Id** veld van de blobs de **HotelId** sleutelveld in de index.

> [!NOTE]
> Document automatisch gegenereerde sleutels, zoals systemen die standaard worden gemaakt door sommige indexeerfuncties Maak in de meeste gevallen niet goed document sleutels voor gecombineerde indexen. In het algemeen u zinvolle, unieke waarde van een sleutel die al in gebruik of eenvoudig kan worden toegevoegd aan uw gegevensbronnen.

## <a name="understand-the-code"></a>De code begrijpen

Zodra de gegevens en configuratie-instellingen zijn gemaakt, wordt het voorbeeld programma **AzureSearchMultipleDataSources.sln** klaar om in te bouwen en uitvoeren.

Dit eenvoudige C#/.NET-consoletoepassing worden de volgende taken uitgevoerd:
* Hiermee maakt u een nieuwe Azure Search-index op basis van de gegevensstructuur van de C# Hotel klasse (die ook verwijst naar het adres en de ruimte klassen).
* Hiermee maakt u een Azure Cosmos DB-gegevensbron en een indexeerfunctie die Azure Cosmos DB-gegevens wordt toegewezen aan indexvelden.
* Voert de CosmosDB-indexeerfunctie om Hotelgegevens te laden.
* Hiermee maakt u een Azure Blob Storage-gegevensbron en een indexeerfunctie die JSOn-Blob-gegevens wordt toegewezen aan indexvelden.
* Voert de indexeerfunctie Azure blob storage om ruimten gegevens te laden.

 Voordat u het programma uitvoert, een ogenblik om te kijken naar de code en de index en indexeerfunctie definities voor dit voorbeeld uit te voeren. De relevante code staat in twee bestanden:

  + **Hotel.cs** bevat het schema dat de index definieert
  + **Program.cs** -functies die de Azure Search-index, gegevensbronnen en indexeerfuncties maken en laad de gecombineerde resultaten in de index bevat.

### <a name="define-the-index"></a>De index definiëren

Deze voorbeeldcode maakt gebruik van de .NET SDK om te definiëren en een Azure Search-index maken. Maakt gebruik van de [FieldBuilder](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.fieldbuilder) klasse voor het genereren van de indexstructuur van een van een C# modelklasse van de gegevens.

Het gegevensmodel is gedefinieerd door de klasse Hotel, vindt u ook verwijzingen naar het adres en de ruimte-klassen. De FieldBuilder zoomt in op via meerdere klassedefinities voor het genereren van een complexe gegevensstructuur voor de index. METADATA-codes worden gebruikt voor het definiëren van de kenmerken van elk veld, zoals doorzoekbare of sorteerbaar.

De volgende codefragmenten uit de **Hotel.cs** bestand laten zien hoe een veld en een verwijzing naar een andere klasse voor model, kunnen worden opgegeven.

```csharp
. . . 
[IsSearchable, IsFilterable, IsSortable]
public string HotelName { get; set; }
. . .
public Room[] Rooms { get; set; }
. . .
```

In de **Program.cs** -bestand, de index wordt gedefinieerd met een naam en een veldverzameling die zijn gegenereerd door de `FieldBuilder.BuildForType<Hotel>()` methode, en vervolgens als volgt gemaakt:

```csharp
private static async Task CreateIndex(string indexName, SearchServiceClient searchService)
{
    // Create a new search index structure that matches the properties of the Hotel class.
    // The Address and Room classes are referenced from the Hotel class. The FieldBuilder
    // will enumerate these to create a complex data structure for the index.
    var definition = new Index()
    {
        Name = indexName,
        Fields = FieldBuilder.BuildForType<Hotel>()
    };
    await searchService.Indexes.CreateAsync(definition);
}
```

### <a name="create-azure-cosmos-db-data-source-and-indexer"></a>Azure Cosmos DB-gegevensbron en indexeerfunctie maken

Naast bevat het hoofdprogramma de logica voor het maken van de Azure Cosmos DB-gegevensbron voor de gegevens hotels.

Eerst voegt de naam van de Azure Cosmos DB-database op de verbindingstekenreeks samen. Vervolgens wordt het gegevensbronobject, met inbegrip van instellingen die specifiek zijn voor Azure Cosmos DB-bronnen, zoals de eigenschap [useChangeDetection] gedefinieerd.

  ```csharp
private static async Task CreateAndRunCosmosDbIndexer(string indexName, SearchServiceClient searchService)
{
    // Append the database name to the connection string
    string cosmosConnectString = 
        configuration["CosmosDBConnectionString"]
        + ";Database=" 
        + configuration["CosmosDBDatabaseName"];

    DataSource cosmosDbDataSource = DataSource.CosmosDb(
        name: configuration["CosmosDBDatabaseName"], 
        cosmosDbConnectionString: cosmosConnectString,
        collectionName: "hotels",
        useChangeDetection: true);

    // The Azure Cosmos DB data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(cosmosDbDataSource);
  ```

Nadat de gegevensbron is gemaakt, wordt het programma stelt u een Azure Cosmos DB-indexeerfunctie met de naam **hotel-ruimten-cosmos-indexeerfunctie**.

```csharp
    Indexer cosmosDbIndexer = new Indexer(
        name: "hotel-rooms-cosmos-indexer",
        dataSourceName: cosmosDbDataSource.Name,
        targetIndexName: indexName,
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));
    
    // Indexers keep metadata about how much they have already indexed.
    // If we already ran this sample, the indexer will remember that it already
    // indexed the sample data and not run again.
    // To avoid this, reset the indexer if it exists.
    bool exists = await searchService.Indexers.ExistsAsync(cosmosDbIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(cosmosDbIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(cosmosDbIndexer);
```
Het programma worden alle bestaande indexeerfuncties met dezelfde naam verwijderd voordat u de nieuwe maakt als u wilt meer dan eenmaal in het volgende voorbeeld wordt uitgevoerd.

In het volgende voorbeeld wordt een schema voor de indexeerfunctie gedefinieerd zodat deze één keer per dag wordt uitgevoerd. Als u niet wilt dat de indexeerfunctie in de toekomst automatisch opnieuw uitvoeren, kunt u de eigenschap schema van deze aanroep.

### <a name="index-azure-cosmos-db-data"></a>Index Azure Cosmos DB-gegevens

Zodra de gegevensbron en de indexeerfunctie is gemaakt, wordt de code die wordt uitgevoerd van de indexeerfunctie korte:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

In dit voorbeeld bevat een eenvoudige probeer-catch-blok voor het rapporteren van eventuele fouten die tijdens de uitvoering optreden.

Nadat de Azure Cosmos DB-indexeerfunctie is uitgevoerd, wordt de search-index een volledige set met voorbeeldgegevens hotel documenten bevatten. Het veld ruimten voor elke hotel is echter een lege matrix, omdat de Azure Cosmos DB-gegevensbron geen ruimte meer informatie bevat. Het programma wordt vervolgens opgehaald uit Blob storage te laden en samenvoegen van de ruimtegegevens.

### <a name="create-blob-storage-data-source-and-indexer"></a>Blob storage-gegevensbron en indexeerfunctie maken

Als u de ruimte meer informatie stelt het programma eerst u de gegevensbron van een Blob-opslag om te verwijzen naar een set afzonderlijke JSON-blob-bestanden.

```csharp
private static async Task CreateAndRunBlobIndexer(string indexName, SearchServiceClient searchService)
{
    DataSource blobDataSource = DataSource.AzureBlobStorage(
        name: configuration["BlobStorageAccountName"],
        storageConnectionString: configuration["BlobStorageConnectionString"],
        containerName: "hotel-rooms");

    // The blob data source does not need to be deleted if it already exists,
    // but the connection string might need to be updated if it has changed.
    await searchService.DataSources.CreateOrUpdateAsync(blobDataSource);
```

Nadat de gegevensbron is gemaakt, wordt het programma stelt u een indexeerfunctie blob met de naam **hotel-ruimten-blob-indexeerfunctie**.

```csharp
    // Add a field mapping to match the Id field in the documents to 
    // the HotelId key field in the index
    List<FieldMapping> map = new List<FieldMapping> {
        new FieldMapping("Id", "HotelId")
    };

    Indexer blobIndexer = new Indexer(
        name: "hotel-rooms-blob-indexer",
        dataSourceName: blobDataSource.Name,
        targetIndexName: indexName,
        fieldMappings: map,
        parameters: new IndexingParameters().ParseJson(),
        schedule: new IndexingSchedule(TimeSpan.FromDays(1)));

    // Reset the indexer if it already exists
    bool exists = await searchService.Indexers.ExistsAsync(blobIndexer.Name);
    if (exists)
    {
        await searchService.Indexers.ResetAsync(blobIndexer.Name);
    }
    await searchService.Indexers.CreateOrUpdateAsync(blobIndexer);
```

De JSON-blobs bevatten een sleutelveld met de naam **Id** in plaats van **HotelId**. De code wordt gebruikgemaakt van de `FieldMapping` klasse die u wilt zien van de indexeerfunctie om te leiden de **Id** veld waarde aan de **HotelId** documentsleutel in de index.

BLOB storage-indexeerfuncties kunt parameters die de parseren modus identificeren moet worden gebruikt. De modus voor het parseren verschilt voor blobs die staan voor één document of meerdere documenten binnen dezelfde blob. In dit voorbeeld wordt elke blob vertegenwoordigt een document dat één index, zodat de code wordt gebruikgemaakt van de `IndexingParameters.ParseJson()` parameter.

Zie voor meer informatie over het parseren van de parameters voor JSON-blobs voor indexering [Index JSON-blobs](search-howto-index-json-blobs.md). Zie voor meer informatie over het opgeven van deze parameters met de .NET SDK, de [IndexerParametersExtension](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.indexingparametersextensions) klasse.

Het programma worden alle bestaande indexeerfuncties met dezelfde naam verwijderd voordat u de nieuwe maakt als u wilt meer dan eenmaal in het volgende voorbeeld wordt uitgevoerd.

In het volgende voorbeeld wordt een schema voor de indexeerfunctie gedefinieerd zodat deze één keer per dag wordt uitgevoerd. Als u niet wilt dat de indexeerfunctie in de toekomst automatisch opnieuw uitvoeren, kunt u de eigenschap schema van deze aanroep.

### <a name="index-blob-data"></a>Index blob-gegevens

Zodra de Blob storage-gegevensbron en indexeerfunctie zijn gemaakt, wordt de code die wordt uitgevoerd van de indexeerfunctie is eenvoudig:

```csharp
    try
    {
        await searchService.Indexers.RunAsync(cosmosDbIndexer.Name);
    }
    catch (CloudException e) when (e.Response.StatusCode == (HttpStatusCode)429)
    {
        Console.WriteLine("Failed to run indexer: {0}", e.Response.Content);
    }
```

Omdat de index is al ingevuld met Hotelgegevens uit de Azure Cosmos DB-database, wordt de blob-indexeerfunctie updates van de bestaande documenten in de index en voegt u de details van de ruimte toe.

> [!NOTE]
> Als u dezelfde zonder sleutel velden in zowel van uw gegevensbronnen en de gegevens in deze velden komt niet overeen, en vervolgens de index de waarden bevat van de indexeerfunctie laatst is uitgevoerd. In ons voorbeeld beide gegevensbronnen bevatten een **HotelName** veld. Als voor een of andere reden de gegevens in dit veld verschillend zijn, voor documenten met dezelfde sleutelwaarde is, dan zal de **HotelName** gegevens uit de gegevensbron die is geïndexeerd meest recent is de waarde die is opgeslagen in de index.

## <a name="search-your-json-files"></a>Uw JSON-bestanden doorzoeken

U kunt verkennen de ingevuld zoekindex nadat het programma is uitgevoerd, met behulp van de [ **Search explorer** ](search-explorer.md) in de portal.

Open de search-service in Azure portal, **overzicht** pagina en zoek de **hotel-ruimten-sample** index in de **indexen** lijst.

  ![Lijst met Azure Search-index](media/tutorial-multiple-data-sources/index-list.png "lijst van Azure Search-index")

Klik op de index hotels-ruimten-voorbeeld in de lijst. Hier ziet u een Search Explorer-interface voor de index. Voer een query voor een termijn, zoals 'Luxe'. U moet ten minste één document in de resultaten zien en dit document moet een lijst met ruimten objecten worden weergegeven in de matrix ruimten.

## <a name="clean-up-resources"></a>Resources opschonen

De snelste manier om op te schonen na een zelfstudie is de resourcegroep met de Azure Search-service te verwijderen. U kunt de resourcegroep nu verwijderen om alles daarin permanent te verwijderen. In de portal is de naam van de resourcegroep op de pagina overzicht van de Azure Search-service.

## <a name="next-steps"></a>Volgende stappen

Er zijn verschillende benaderingen en meerdere opties voor indexeren van JSON-blobs. Als de brongegevens JSON-inhoud bevat, kunt u deze opties om te zien wat het beste werkt voor uw scenario kunt bekijken.

> [!div class="nextstepaction"]
> [Indexeren van JSON-blobs met behulp van de indexeerfunctie Azure Search Blob](search-howto-index-json-blobs.md)

Het is raadzaam om te verbeteren, gestructureerde indexering van gegevens uit een gegevensbron met cognitively verrijkt gegevens van niet-gestructureerde blobs of inhoud van de volledige tekst. De volgende zelfstudie laat zien hoe het gebruik van Cognitive Services samen met Azure Search met behulp van de .NET SDK.

> [!div class="nextstepaction"]
> [Cognitive Services API's aanroepen in een Azure Search indexeren van pijplijn](cognitive-search-tutorial-blob-dotnet.md)
