---
title: 'Zelfstudie: ImageInsightsToken - Bing Visual Search'
titlesuffix: Azure Cognitive Services
description: De Bing Visual Search SDK gebruiken voor het ophalen van URL's van afbeeldingen die zijn opgegeven met ImageInsightsToken.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: tutorial
ms.date: 06/21/2018
ms.author: rosh
ms.openlocfilehash: 62780500d29c891182d3869bf0ba3ccdc5e2f715
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52441058"
---
# <a name="tutorial-bing-visual-search-sdk-imageinsightstoken-and-results"></a>Zelfstudie: ImageInsightsToken en resultaten van Bing Visual Search SDK
De Visual Search SDK bevat een optie om online afbeeldingen te zoeken van een eerdere zoekopdracht die een `ImageInsightsToken` retourneert.  In dit voorbeeld wordt een `ImageInsightsToken` opgehaald en wordt het token gebruikt in een volgende zoekopdracht.  De code verzendt de `ImageInsightsToken` naar Bing en retourneert resultaten met Bing Search-URL's en URL's van vergelijkbare online gevonden afbeeldingen.

## <a name="prerequisites"></a>Vereisten
Visual Studio 2017. Indien nodig kunt u hier de gratis communityversie downloaden: https://www.visualstudio.com/vs/community/.
Er is een Cognitive Services API-sleutel vereist voor het verifiëren van SDK-aanroepen. Meld u aan voor een gratis proefsleutel. De proefsleutel is goed voor zeven dagen met één aanroep per seconde. Koop een toegangssleutel voor productiescenario's. Bekijk ook de prijsgegevens.
De mogelijkheid om .NET core SDK-, .net core 1.1-apps uit te voeren. U kunt CORE, Framework en Runtime hier downloaden: https://www.microsoft.com/net/download/.

Voor deze zelfstudie moet u een abonnement in de prijscategorie S9 starten, zoals wordt weergegeven in [Prijsinformatie Cognitive Services Bing Zoeken-API](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/). 

Zo start u een abonnement in de Azure-portal:
1. Typ 'BingSearchV7' in het tekstvak bovenin de Azure-portal waar `Search resources, services, and docs` wordt weergegeven.  
2. Selecteer `Bing Search v7` in de vervolgkeuzelijst onder Marketplace.
3. Voer `Name` in voor de nieuwe resource.
4. Selecteer `Pay-As-You-Go`-abonnement.
5. Selecteer prijscategorie `S9`.
6. Klik op `Enable` om het abonnement te starten.

## <a name="application-dependencies"></a>Afhankelijkheden van de toepassing
Als u een consoletoepassing wilt instellen met behulp van de Bing Web Search SDK, gaat u naar de optie NuGet-pakketten beheren van Solution Explorer in Visual Studio. Voeg toe:
* Microsoft.Azure.CognitiveServices.Search.VisualSearch
* Microsoft.Azure.CognitiveServices.Search.ImageSearchpackage-pakketten.

Wanneer u het NuGet Web Search SDK-pakket installeert, worden onder andere ook de volgende afhankelijkheden geïnstalleerd:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="get-the-imageinsightstoken-from-image-search"></a>ImageInsightsToken ophalen uit Image Search

In dit voorbeeld wordt een `ImageInsightsToken` gebruikt dat is verkregen met de volgende methode.  Zie voor meer informatie over deze aanroep [C#-snelstart voor Image Search SDK](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

De code zoekt naar resultaten van een zoekopdracht naar "Canadian Rockies" en krijgt een ImageInsightsToken. Het inzichttoken, de miniatuur-URL en de URL van de afbeeldingsinhoud worden afgedrukt.  De methode retourneert de `ImageInsightsToken` voor gebruik in een volgende Visual Search-aanvraag.

```
        private static String ImageResults(String subKey)
        {
            String insightTok = "None";
            try
            {
                var client = new ImageSearchAPI(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey)); //
                var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
                Console.WriteLine("Search images for query \"canadian rockies\"");

                if (imageResults == null)
                {
                    Console.WriteLine("No image result data.");
                }
                else
                {
                    // Image results
                    if (imageResults.Value.Count > 0)
                    {
                        var firstImageResult = imageResults.Value.First();

                        Console.WriteLine($"\r\nImage result count: {imageResults.Value.Count}");
                        insightTok = firstImageResult.ImageInsightsToken;
                        Console.WriteLine($"First image insights token: {firstImageResult.ImageInsightsToken}");  
                        Console.WriteLine($"First image thumbnail url: {firstImageResult.ThumbnailUrl}");
                        Console.WriteLine($"First image content url: {firstImageResult.ContentUrl}");
                    }
                    else
                    {
                        insightTok = "None found";
                        Console.WriteLine("Couldn't find image results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("\r\nEncountered exception. " + ex.Message);
            }

            return insightTok;
        }
```

## <a name="specify-the-imageinsightstoken-for-visual-search-request"></a>Het ImageInsightsToken voor Visual Search-aanvraag opgeven

In dit voorbeeld wordt het inzichttoken gebruikt dat door de vorige methode is geretourneerd. De volgende code maakt een `ImageInfo`-object van het `ImageInsightsToken` en laadt het ImageInfo-object in een `VisualSearchRequest`. Geef `ImageInsightsToken` op in een `ImageInfo` voor de `VisualSearchRequest`

```
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```

## <a name="use-visual-search-to-find-images-from-an-imageinsightstoken"></a>Visual Search gebruiken om afbeeldingen te zoeken vanuit een ImageInsightsToken

De `VisualSearchRequest` bevat informatie over de afbeelding waarnaar moet worden gezocht in het `ImageInfo`-object.  Met de `VisualSearchMethodAsync`-methode worden de resultaten opgehaald.
```
// An image binary is not necessary here, as the image is specified by insights token.
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
Console.WriteLine("\r\nVisual search request with knowledgeRequest");

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>De URL-gegevens ophalen uit ImageModuleAction
Visual Search-resultaten zijn `ImageTag`-objecten.  Elke tag bevat een lijst met `ImageAction`-objecten.  Elke `ImageAction` bevat een `Data`-veld met een lijst met waarden die afhankelijk zijn van het type actie:

U kunt de verschillende typen ophalen met de volgende code:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
De volledige toepassing retourneert:

* ActionType: MoreSizes -> WebSearchUrl:
* ActionType: VisualSearch -> WebSearchUrl:
* ActionType: ImageById -> WebSearchUrl:
* ActionType: RelatedSearches -> WebSearchUrl:
* ActionType: DocumentLevelSuggestions -> WebSearchUrl:
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&p=DevEx,5823.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx,5831.1

Zoals weergegeven in de voorgaande lijst, bevatten de typen `TopicResults` en `ImageResults` zoekopdrachten naar gerelateerde afbeeldingen. De URL's in de lijst zijn koppelingen naar Bing-zoekresultaten.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>PagesIncluding ActionType-URL's van met Visual Search gevonden afbeeldingen

Voor het ophalen van de werkelijke afbeeldings-URL's is een cast vereist die een `ActionType` leest als `ImageModuleAction`, die een `Data`-element met een waardelijst bevat.  Elke waarde is de URL van een afbeelding.  De volgende code cast het actietype `PagesIncluding` naar `ImageModuleAction` en leest de waarden.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```
Zie [Afbeeldingen - Visual Search](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch) voor meer informatie over deze gegevenstypen.

## <a name="complete-code"></a>Volledige code

Met de volgende code worden de eerdere voorbeelden uitgevoerd. Het `ImageInsightsToken` wordt verstuurd in een post-aanvraag. Vervolgens worden de Bing-zoek-URL's voor elk ActionType afgedrukt. Als het ActionType `PagesIncluding` is, haalt de code de `ImageObject`-items in `Data` op.  De `Data` bevat een lijst met waarden: de URL's van afbeeldingen op webpagina's.  Kopieer en plak de resulterende Visual Search-URL's naar de browser om resultaten weer te geven. Kopieer en plak ContentUrl-items naar de browser om afbeeldingen weer te geven.

```
using System;
using System.IO;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch;
using Microsoft.Azure.CognitiveServices.Search.VisualSearch.Models;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;

namespace VisualSearchFeatures
{
    class Program
    {
        static void Main(string[] args)
        {
            String subscriptionKey = "YOUR-ACCESS-KEY";

            insightsToken = ImageResults(subscriptionKey);

            VisualSearchInsightsToken(subscriptionKey, insightsToken);

            Console.WriteLine("Any key to quit...");
            Console.ReadKey();

        }

        // Searches for results on query (Canadian Rockies) and gets an ImageInsightsToken.
        // Also prints first image insights token, thumbnail url, and image content url.
        private static String ImageResults(String subKey)
        {
            String insightTok = "None";
            try
            {
                var client = new ImageSearchAPI(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey)); //
                var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
                Console.WriteLine("Search images for query \"canadian rockies\"");

                if (imageResults == null)
                {
                    Console.WriteLine("No image result data.");
                }
                else
                {
                    // Image results
                    if (imageResults.Value.Count > 0)
                    {
                        var firstImageResult = imageResults.Value.First();

                        Console.WriteLine($"\r\nImage result count: {imageResults.Value.Count}");
                        insightTok = firstImageResult.ImageInsightsToken;
                        Console.WriteLine($"First image insights token: {firstImageResult.ImageInsightsToken}");  
                        Console.WriteLine($"First image thumbnail url: {firstImageResult.ThumbnailUrl}");
                        Console.WriteLine($"First image content url: {firstImageResult.ContentUrl}");
                    }
                    else
                    {
                        insightTok = "None found";
                        Console.WriteLine("Couldn't find image results!");
                    }
                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("\r\nEncountered exception. " + ex.Message);
            }

            return insightTok;
        }


        // This method will get Visual Search results from an imageInsightsToken obtained from the return value of the ImageResults method.
        // The method includes imageInsightsToken the in a knowledgeRequest parameter, along with a cropArea object. 
        // It prints out the imageInsightsToken uploaded in the request.
        // Finally the example prints URLs of images found using the imageInsightsToken.

        public static void VisualSearchInsightsToken(string subscriptionKey, string insightsTok)
        {
            var client = new VisualSearchAPI(new Microsoft.Azure.CognitiveServices.Search.VisualSearch.ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                // The image can be specified via an insights token, in the ImageInfo object.
                ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);

                // An image binary is not necessary here, as the image is specified by insights token.
                VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
                Console.WriteLine("\r\nVisual search request with imageInsightsToken and knowledgeRequest");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // Visual Search results
                    if (visualSearchResults.Image?.ImageInsightsToken != null)
                    {
                        Console.WriteLine($"Uploaded image insights token: {insightsTok}");
                    }
                    else
                    {
                        Console.WriteLine("Couldn't find image insights token!");
                    }

                    if (visualSearchResults.Tags.Count > 0)
                    {
                        // List of tags
                        foreach (ImageTag t in visualSearchResults.Tags)
                        {
                            foreach (ImageAction i in t.Actions)
                            {
                                Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " WebSearchURL: " + i.WebSearchUrl);

                                if (i.ActionType == "PagesIncluding")
                                {
                                    foreach (ImageObject o in (i as ImageModuleAction).Data.Value)
                                    {
                                        Console.WriteLine("ContentURL: " + o.ContentUrl);
                                    }
                                }
                            }
                        }
                    }

                    else
                    {
                        Console.WriteLine("Couldn't find image tags!");
                    }

                }
            }

            catch (Exception ex)
            {
                Console.WriteLine("Encountered exception. " + ex.Message);
            }
        }
    }
}

```

## <a name="next-steps"></a>Volgende stappen
[Reactie van Visual Search](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview#the-response)
