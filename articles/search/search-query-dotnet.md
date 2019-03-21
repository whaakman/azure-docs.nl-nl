---
title: Opvragen van gegevens naar een Azure Search-index in C# (.NET SDK) - Azure Search
description: Voorbeeld van C#-code voor het maken van een zoekquery in Azure Search. Voeg zoekparameters toe om de zoekresultaten te filteren en te sorteren.
author: heidisteen
manager: cgronlun
ms.author: heidist
services: search
ms.service: search
ms.devlang: dotnet
ms.topic: quickstart
ms.date: 03/20/2019
ms.openlocfilehash: 6bb170a5f3353288ab9c393e01b7a0902361913b
ms.sourcegitcommit: 8a59b051b283a72765e7d9ac9dd0586f37018d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58287006"
---
# <a name="quickstart-3---query-an-azure-search-index-in-c"></a>Quickstart: 3 - query uitvoeren op een Azure Search-index inC#

In dit artikel leest u hoe u query [een Azure Search-index](search-what-is-an-index.md) met behulp van C# en de [.NET SDK](https://aka.ms/search-sdk). Zoeken naar documenten in uw index wordt bereikt door het uitvoeren van deze taken:

> [!div class="checklist"]
> * Maak een [ `SearchIndexClient` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient?view=azure-dotnet) object verbinding maken met een search-index met alleen-lezen rechten.
> * Maak een [ `SearchParameters` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.models.searchparameters?view=azure-dotnet) -object met de definitie van de zoeken of filteren.
> * Roep de `Documents.Search` methode voor `SearchIndexClient` query's verzenden naar een index.

## <a name="prerequisites"></a>Vereisten

[Laden van een Azure Search-index](search-import-data-dotnet.md) met de voorbeeldgegevens hotels.

De querysleutel van een die wordt gebruikt voor alleen-lezen toegang tot documenten ophalen. Tot nu toe hebt u een beheer-API-sleutel gebruikt, zodat u objecten en inhoud op een search-service kunt maken. Maar voor de Queryondersteuning van de in apps, wordt aangeraden met behulp van een querysleutel. Zie voor instructies [maken van een querysleutel](search-security-api-keys.md#create-query-keys).

## <a name="create-a-client"></a>Een client maken
Maak een instantie van de `SearchIndexClient` klasse zodat u eraan, kunt u een querysleutel voor alleen-lezentoegang toewijzen kunt (in plaats van de toegang voor schrijven rechten verleend de `SearchServiceClient` gebruikt in de vorige les).

Deze klasse heeft verschillende constructors. De gewenste constructor krijgt uw search-servicenaam, naam van de index en een [ `SearchCredentials` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchcredentials?view=azure-dotnet) object als parameters. `SearchCredentials` verpakt uw api-sleutel.

De code hieronder maakt u een nieuwe `SearchIndexClient` voor de index "hotels" met waarden voor de search-servicenaam en api-sleutel die zijn opgeslagen in het configuratiebestand van de toepassing (`appsettings.json` in het geval van de [voorbeeldtoepassing](https://aka.ms/search-dotnet-howto)):

```csharp
private static SearchIndexClient CreateSearchIndexClient(IConfigurationRoot configuration)
{
    string searchServiceName = configuration["SearchServiceName"];
    string queryApiKey = configuration["SearchServiceQueryApiKey"];

    SearchIndexClient indexClient = new SearchIndexClient(searchServiceName, "hotels", new SearchCredentials(queryApiKey));
    return indexClient;
}
```

`SearchIndexClient` heeft een [ `Documents` ](https://docs.microsoft.com/dotnet/api/microsoft.azure.search.searchindexclient.documents?view=azure-dotnet) eigenschap. Deze eigenschap bevat alle methoden die u nodig hebt om een query uit te voeren in een Azure Search-index.

## <a name="construct-searchparameters"></a>Om voor te bereiden SearchParameters
Zoeken met de .NET SDK is net zo eenvoudig als het aanroepen van de `Documents.Search`-methode op uw `SearchIndexClient`. Deze methode heeft een aantal parameters, waaronder de zoektekst, en een `SearchParameters`-object dat kan worden gebruikt om de query te verfijnen.

### <a name="types-of-queries"></a>Typen query 's
De twee belangrijkste [querytypen](search-query-overview.md#types-of-queries) die u zult gebruiken zijn `search` en `filter`. Een `search`-query zoekt naar een of meer voorwaarden in alle *doorzoekbare* velden in de index. Een `filter`-query evalueert een Booleaanse expressie op alle *filterbare* velden in een index. U kunt zoekopdrachten en filters samen of afzonderlijk gebruiken.

Zoekopdrachten en filters worden allebei uitgevoerd met behulp van de `Documents.Search`-methode. Een zoekopdracht kan worden doorgegeven aan de `searchText`-parameter, terwijl een filterexpressie kan worden doorgegeven in de eigenschap `Filter` van de `SearchParameters`-klasse. Als u wilt filteren zonder te zoeken, geeft u `"*"` door voor de `searchText`-parameter. Om te zoeken zonder te filteren, stelt u de eigenschap `Filter` niet in of geeft u niet door aan een `SearchParameters`-instantie.

### <a name="example-queries"></a>Voorbeelden van query 's
De volgende voorbeeldcode bevat een aantal verschillende manieren om op te vragen van de index "hotels" is gedefinieerd in [maken van een Azure Search-index in C# ](search-create-index-dotnet.md#DefineIndex). Houd er rekening mee dat de documenten die zijn geretourneerd met de lijst met zoekresultaten instanties van zijn de `Hotel` klasse, die is gedefinieerd in [gegevens importeren in een Azure Search-index in C# ](search-import-data-dotnet.md#construct-indexbatch). De voorbeeldcode maakt gebruik van een `WriteDocuments`-methode om de zoekresultaten in de console uit te voeren. Deze methode wordt in de volgende sectie beschreven.

```csharp
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
```

## <a name="handle-search-results"></a>Zoekresultaten verwerken
De methode `Documents.Search` retourneert een `DocumentSearchResult`-object dat de resultaten van de query bevat. In het voorbeeld in de vorige sectie is gebruikgemaakt van een methode met de naam `WriteDocuments` om de zoekresultaten in de console uit te voeren:

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

Hier ziet u hoe de resultaten voor de query's in de vorige sectie, ervan uitgaande dat de index "hotels" is gevuld met de voorbeeldgegevens eruit zien:

```
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
```

De bovenstaande voorbeeldcode maakt gebruik van de console om de zoekresultaten uit te voeren. U moet ook zoekresultaten in uw eigen toepassing weergeven. Zie voor een voorbeeld van hoe zoekresultaten in een ASP.NET MVC-gebaseerde webtoepassing, de [DotNetSample project](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetSample) op GitHub.

## <a name="next-steps"></a>Volgende stappen

Als u dit nog niet hebt gedaan, raadpleegt u de voorbeeldcode in [DotNetHowTo](https://github.com/Azure-Samples/search-dotnet-getting-started/tree/master/DotNetHowTo) op GitHub, samen met [over het gebruik van Azure Search via een .NET-toepassing ](search-howto-dotnet-sdk.md) voor meer beschrijvingen van de voorbeeldcode gedetailleerde. 