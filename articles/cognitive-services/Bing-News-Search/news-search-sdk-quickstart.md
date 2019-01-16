---
title: 'Snelstartgids: Nieuws zoeken - Bing News Search-SDK voor C#'
titleSuffix: Azure Cognitive Services
description: Gebruik deze snelstartgids om nieuws te zoeken met de Bing News Search-SDK voor Python en om het antwoord te verwerken.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: quickstart
ms.date: 01/30/2018
ms.author: v-gedod
ms.custom: seodec2018
ms.openlocfilehash: 5b3e68765fbcff12dcb5337aec38623b8994882c
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54156796"
---
# <a name="quickstart-perform-a-news-search-with-the-bing-news-search-sdk-for-c"></a>Snelstartgids: Nieuws zoeken met de Bing News Search-SDK voor C#

De Bing Nieuws zoeken-SDK bevat de functionaliteit van de REST API voor nieuwsaanvragen en parseerresultaten. 

De [broncode voor voorbeelden van de Bing Nieuws zoeken-SDK voor C#](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingNewsSearch) is beschikbaar op Git Hub.

## <a name="application-dependencies"></a>Afhankelijkheden van de toepassing
Haal een [Cognitive Services-toegangssleutel](https://azure.microsoft.com/try/cognitive-services/) op onder **Zoeken**.  Zie ook [Prijsinformatie Cognitive Services - Bing Zoeken-API](https://azure.microsoft.com/pricing/details/cognitive-services/search-api/).  

Als u een consoletoepassing wilt instellen met behulp van de Bing Nieuws zoeken-SDK, gaat u naar de optie `Manage NuGet Packages` van Solution Explorer in Visual Studio.  Voeg het pakket `Microsoft.Azure.CognitiveServices.Search.NewsSearch` toe.

Wanneer u het [Nieuws zoeken-SDK-pakket voor NuGet](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.NewsSearch/1.2.0) installeert, worden onder andere ook de volgende afhankelijkheden geïnstalleerd:
* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="news-search-client"></a>Nieuws zoeken-client
Als u een exemplaar van de `NewsSearchClient` wilt maken, voegt u dit toe met behulp van de instructie:
```
using Microsoft.Azure.CognitiveServices.Search.NewsSearch;

```
Instantieer vervolgens de client:
```
var client = new NewsSearchClient(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));


```
Gebruik de client om te zoeken met een querytekst:
```
var newsResults = client.News.SearchAsync(query: "Quantum  Computing", market: "en-us", count: 10).Result;
Console.WriteLine("Search news for query \"Quantum  Computing\" with market and count");

```
Parseer het nieuws dat is geretourneerd in de resultaten van de vorige query:
```
if (newsResults.Value.Count > 0)
{
    var firstNewsResult = newsResults.Value[0];

    Console.WriteLine($"TotalEstimatedMatches value: {newsResults.TotalEstimatedMatches}");
    Console.WriteLine($"News result count: {newsResults.Value.Count}");
    Console.WriteLine($"First news name: {firstNewsResult.Name}");
    Console.WriteLine($"First news url: {firstNewsResult.Url}");
    Console.WriteLine($"First news description: {firstNewsResult.Description}");
    Console.WriteLine($"First news published time: {firstNewsResult.DatePublished}");
    Console.WriteLine($"First news provider: {firstNewsResult.Provider[0].Name}");
}

else
{
    Console.WriteLine("Couldn't find news results!");
}

```
## <a name="complete-console-application"></a>Consoletoepassing voltooien

De volgende consoletoepassing voert de eerder gedefinieerde query uit en zoekt naar nieuws voor 'Quantum Computing'. De aanvraag bevat de parameters `market` en `count`. De code controleert het aantal resultaten en geeft `totalEstimatedMatches`, `name`, `url`, `description`, `published time` en `name` van `provider` weer voor het eerste nieuwsresultaat.

```
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.NewsSearch;

namespace NewsSrchSDK
{
    class Program
    {
        static void Main(string[] args)
        {
            var client = new NewsSearchClient(new ApiKeyServiceClientCredentials("YOUR-ACCESS-KEY"));

            try
            {
                var newsResults = client.News.SearchAsync(query: "Quantum  Computing", market: "en-us", count: 10).Result;
                Console.WriteLine("Search news for query \"Quantum  Computing\" with market and count");

                if (newsResults == null)
                {
                    Console.WriteLine("Didn't see any news result data..");
                }
                else
                {
                    if (newsResults.Value.Count > 0)
                    {
                        var firstNewsResult = newsResults.Value.First();

                        Console.WriteLine($"TotalEstimatedMatches value: {newsResults.TotalEstimatedMatches}");
                        Console.WriteLine($"News result count: {newsResults.Value.Count}");
                        Console.WriteLine($"First news name: {firstNewsResult.Name}");
                        Console.WriteLine($"First news url: {firstNewsResult.Url}");
                        Console.WriteLine($"First news description: {firstNewsResult.Description}");
                        Console.WriteLine($"First news published time: {firstNewsResult.DatePublished}");
                        Console.WriteLine($"First news provider: {firstNewsResult.Provider.First().Name}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find news results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }

            // Include these methods to run queries explained under headings following.
            // NewsSearchWithFilters(client);
            // NewsCategory(client);
            // TrendingTopics(client);

            Console.WriteLine("Any key to exit...");
            Console.ReadKey();
        }

    }
}

```
## <a name="recent-news-freshness-and-sortby-parameters"></a>De parameters voor recent nieuws, nieuwheid en sortBy
De volgende code zoekt naar het meest recente nieuws voor 'Artificial Intelligence' met de parameters `freshness` en `sortBy`. De code controleert het aantal resultaten en geeft `totalEstimatedMatches`, `name`, `url`, `description`, `published time` en `name` weer voor de provider voor het eerste nieuwsresultaat.
```
        public static void NewsSearchWithFilters(NewsSearchClient client)
        {
            try
            {
                var newsResults = client.News.SearchAsync(query: "Artificial Intelligence", market: "en-us", freshness: "Week", sortBy: "Date").Result;
                Console.WriteLine("Search most recent news for query \"Artificial Intelligence\" with freshness and sortBy");

                if (newsResults == null)
                {
                    Console.WriteLine("Didn't see any news result data..");
                }
                else
                {
                    if (newsResults.Value.Count > 0)
                    {
                        var firstNewsResult = newsResults.Value.First();

                        Console.WriteLine($"\r\nTotalEstimatedMatches value: {newsResults.TotalEstimatedMatches}");
                        Console.WriteLine($"News result count: {newsResults.Value.Count}");
                        Console.WriteLine($"First news name: {firstNewsResult.Name}");
                        Console.WriteLine($"First news url: {firstNewsResult.Url}");
                        Console.WriteLine($"First news description: {firstNewsResult.Description}");
                        Console.WriteLine($"First news published time: {firstNewsResult.DatePublished}");
                        Console.WriteLine($"First news provider: {firstNewsResult.Provider.First().Name}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find news results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```

## <a name="category-news-safe-search"></a>Nieuwscategorie, veilig zoeken
De volgende code doorzoekt de nieuwscategorieën voor films en tv-entertainment met veilig zoeken.  De code controleert het aantal resultaten en geeft `category`, `name`, `url`, `description`, `published time` en `name` weer voor de provider voor het eerste nieuwsresultaat.
```
        public static void NewsCategory(NewsSearchClient client)
        {
            try
            {
                var newsResults = client.News.CategoryAsync(category: "Entertainment_MovieAndTV", market: "en-us", safeSearch: "strict").Result;
                Console.WriteLine("Search category news for movie and TV entertainment with safe search");

                if (newsResults == null)
                {
                    Console.WriteLine("Didn't see any news result data..");
                }
                else
                {
                    if (newsResults.Value.Count > 0)
                    {
                        var firstNewsResult = newsResults.Value.First();

                        Console.WriteLine($"\r\nNews result count: {newsResults.Value.Count}");
                        Console.WriteLine($"First news category: {firstNewsResult.Category}");
                        Console.WriteLine($"First news name: {firstNewsResult.Name}");
                        Console.WriteLine($"First news url: {firstNewsResult.Url}");
                        Console.WriteLine($"First news description: {firstNewsResult.Description}");
                        Console.WriteLine($"First news published time: {firstNewsResult.DatePublished}");
                        Console.WriteLine($"First news provider: {firstNewsResult.Provider.First().Name}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find news results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```
## <a name="trending-topics"></a>Trending onderwerpen
De volgende code doorzoekt trending nieuwsonderwerpen in Bing. De code controleert het aantal resultaten en geeft `name`, `text of query`, `webSearchUrl`, `newsSearchUrl` en `image.Url` weer voor het eerste nieuwsresultaat.
```
        public static void TrendingTopics(NewsSearchClient client)
        {
            try
            {
                var trendingTopics = client.News.TrendingAsync(market: "en-us").Result;
                Console.WriteLine("Search news trending topics in Bing");

                if (trendingTopics == null)
                {
                    Console.WriteLine("Didn't see any news trending topics..");
                }
                else
                {
                    if (trendingTopics.Value.Count > 0)
                    {
                        var firstTopic = trendingTopics.Value.First();

                        Console.WriteLine($"\r\nTrending topics count: {trendingTopics.Value.Count}");
                        Console.WriteLine($"First topic name: {firstTopic.Name}");
                        Console.WriteLine($"First topic query: {firstTopic.Query.Text}");
                        Console.WriteLine($"First topic image url: {firstTopic.Image.Url}");
                        Console.WriteLine($"First topic webSearchUrl: {firstTopic.WebSearchUrl}");
                        Console.WriteLine($"First topic newsSearchUrl: {firstTopic.NewsSearchUrl}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find news trending topics!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }

```

## <a name="next-steps"></a>Volgende stappen

[Voorbeelden voor Cognitive Services .NET SDK](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
