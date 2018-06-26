---
title: Bing Visual zoeken SDK ImageInsightsToken-zelfstudie | Microsoft Docs
description: Klik hier voor meer informatie over het gebruik van de Bing-SDK voor Visual zoeken naar URL's van installatiekopieën die zijn opgegeven door ImageInsightsToken ophalen.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/21/2018
ms.author: rosh
ms.openlocfilehash: 578fa90f504920030b488d2b8fa3a2d0232cccce
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36753653"
---
# <a name="tutorial-bing-visual-search-sdk-imageinsightstoken-and-results"></a>Zelfstudie: Bing Visual zoeken SDK ImageInsightsToken en resultaten
De visuele zoeken SDK bevat een optie voor het vinden van online installatiekopieën van een vorige zoekquery waarmee een `ImageInsightsToken`.  In dit voorbeeld wordt een `ImageInsightsToken` en gebruikt het token in een volgende zoekopdracht.  De code verzendt de `ImageInsightsToken` naar Bing en retourneert omvatten de resultaten Bing zoeken-URL's en van soortgelijke afbeeldingen online gevonden.

## <a name="prerequisites"></a>Vereisten
Visual Studio 2017. Als nodig is, u gratis community-versie van hieruit downloaden kunt: https://www.visualstudio.com/vs/community/.
Een cognitieve services API-sleutel is vereist voor het verifiëren van de SDK-aanroepen. Aanmelden voor een gratis proefversie sleutel. De evaluatieversie sleutel is geschikt is voor de zeven dagen met één aanroep per seconde. Koop een toegangssleutel voor productiescenario's. Zie ook prijsinformatie.
De mogelijkheid om uit te voeren .NET core SDK, .net core 1.1-apps. U krijgt CORE en Framework Runtime vanaf hier: https://www.microsoft.com/net/download/.

##<a name="application-dependencies"></a>Afhankelijkheden voor toepassingen
Als u een consoletoepassing met de Bing Web Search SDK instelt, blader naar de optie NuGet-pakketten beheren vanuit de Solution Explorer in Visual Studio. Toevoegen:
* Microsoft.Azure.CognitiveServices.Search.VisualSearch
* Microsoft.Azure.CognitiveServices.Search.ImageSearchpackage pakketten.

Het pakket NuGet Web Search SDK installeert, installeert ook afhankelijkheden, met inbegrip van:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="get-the-imageinsightstoken-from-image-search"></a>De ImageInsightsToken ophalen uit de afbeelding zoeken
In dit voorbeeld wordt een `ImageInsightsToken` verkregen door de volgende methode.  Zie voor meer informatie over deze aanroep [installatiekopie zoeken SDK C# Quick Start](https://docs.microsoft.com/en-us/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart).

De code voor de resultaten van een query zoekt 'Canadees Rockies' en een ImageInsightsToken opgehaald. Worden de eerste afbeelding insights token, miniatuur-url en inhoud afbeeldings-url.  De methode retourneert de `ImageInsightsToken`voor gebruik in een volgende Visual Search-aanvraag.

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

## <a name="specify-the-imageinsightstoken-for-visual-search-request"></a>Geef de ImageInsightsToken voor Visual zoekaanvraag
In dit voorbeeld gebruikt de insights-token dat is geretourneerd door de vorige methode. De volgende code maakt een `ImageInfo` object uit de `ImageInsightsToken` en laadt het object ImageInfo in een `VisualSearchRequest`. Geef `ImageInsightsToken` in een `ImageInfo` voor de `VisualSearchRequest`

```
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```
## <a name="use-visual-search-to-find-images-from-an-imageinsightstoken"></a>Visual zoekfunctie waarmee u installatiekopieën van een ImageInsightsToken zoeken
De `VisualSearchRequest` bevat informatie over de installatiekopie te zoeken in de `ImageInfo` object.  De `VisualSearchMethodAsync` methode haalt de resultaten.
```
// An image binary is not necessary here, as the image is specified by insights token.
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;
Console.WriteLine("\r\nVisual search request with knowledgeRequest");

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>De URL-gegevens ophalen uit ImageModuleAction
Visual zoekresultaten zijn `ImageTag` objecten.  Elke tag bevat een lijst met `ImageAction` objecten.  Elke `ImageAction` bevat een `Data` veld met een lijst met waarden die afhankelijk van het type actie zijn:

U kunt de verschillende typen krijgen met de volgende code:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
De volledige toepassing geretourneerd:

* ActionType: MoreSizes -> WebSearchUrl:
* ActionType: In het VisualSearch-WebSearchUrl >:
* ActionType: In het ImageById-WebSearchUrl >:
* ActionType: In het RelatedSearches-WebSearchUrl >:
* ActionType: In het DocumentLevelSuggestions-WebSearchUrl >:
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&p=DevEx, 5823.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx, 5831.1

Zoals wordt weergegeven in de voorgaande lijst de `TopicResults` en `ImageResults` typen query's voor verwante installatiekopieën bevatten. De URL's in de lijst koppelen aan Bing zoekresultaten.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>PagesIncluding ActionType URL's van installatiekopieën vinden door Visual zoeken

De werkelijke afbeelding-URL's ophalen cast die leest vereist een `ActionType` als `ImageModuleAction`, waarin een `Data` element met een lijst met waarden.  Elke waarde is de URL van een afbeelding.  De volgende webcasts de `PagesIncluding` actietype wilt `ImageModuleAction` en de waarden worden gelezen.
```
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```
Zie voor meer informatie over deze gegevenstypen [installatiekopieën - Visual zoeken](https://docs.microsoft.com/en-us/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch).
## <a name="complete-code"></a>Volledige code

De volgende code wordt de eerdere voorbeelden uitgevoerd. Het verzendt de `ImageInsightsToken` in een post-aanvraag. Vervolgens wordt de Bing afgedrukt URL's voor elke ActionType zoeken. Als de ActionType `PagesIncluding`, haalt de code de `ImageObject` -items in `Data`.  De `Data` bevat een lijst met waarden die de URL's van afbeeldingen op webpagina's.  Kopieer en plak de resulterende Visual URL's zoeken naar de browser om resultaten weer te geven. Kopieer en plak ContentUrl items naar de browser om afbeeldingen weer te geven.

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
[Visual zoeken antwoord](https://review.docs.microsoft.com/en-us/azure/cognitive-services/bing-visual-search/overview?branch=pr-en-us-44614#the-response)