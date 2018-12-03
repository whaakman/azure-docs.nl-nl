---
title: 'Zelfstudie: gebied voor bijsnijden afbeelding en resultaten - Bing Visual Search'
description: De Bing Visual Search-SDK gebruiken voor het ophalen van URL's van afbeeldingen die overeenkomen met het bijsnijdgebied van een geüploade afbeelding.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: tutorial
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: 27141c014c9ccdf9d62c9bde5c96bd31abfc025e
ms.sourcegitcommit: 5aed7f6c948abcce87884d62f3ba098245245196
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52447092"
---
# <a name="tutorial-bing-visual-search-sdk-image-crop-area-and-results"></a>Zelfstudie: gebied voor bijsnijden afbeelding en resultaten van de Bing Visual Search-SDK
De Visual Search-SDK bevat een optie voor het selecteren van een gebied van een afbeelding en het online zoeken van afbeeldingen die lijken op het bijsnijdgebied van de grotere afbeelding.  In dit voorbeeld wordt een bijsnijdgebied getoond met één persoon, afkomstig uit een afbeelding met meerdere personen.  De code verzendt het bijsnijdgebied en de URL van de grotere afbeelding en retourneert resultaten met Bing Search-URL's en URL's van vergelijkbare online gevonden afbeeldingen.

## <a name="prerequisites"></a>Vereisten

U hebt [Visual Studio 2017](https://www.visualstudio.com/downloads/) nodig om de code in Windows te kunnen uitvoeren. (De gratis Community-editie volstaat.)

Voor deze zelfstudie moet u een abonnement in de prijscategorie S9 starten, zoals wordt weergegeven in [Prijsinformatie Cognitive Services Bing Zoeken-API](https://azure.microsoft.com/en-us/pricing/details/cognitive-services/search-api/). 

Zo start u een abonnement in de Azure-portal:
1. Typ 'BingSearchV7' in het tekstvak bovenin de Azure-portal waar `Search resources, services, and docs` wordt weergegeven.  
2. Selecteer `Bing Search v7` in de vervolgkeuzelijst onder Marketplace.
3. Voer `Name` in voor de nieuwe resource.
4. Selecteer `Pay-As-You-Go`-abonnement.
5. Selecteer prijscategorie `S9`.
6. Klik op `Enable` om het abonnement te starten.

## <a name="application-dependencies"></a>Afhankelijkheden van de toepassing
Als u een consoletoepassing wilt instellen met behulp van de Bing Web Search SDK, gaat u naar de optie NuGet-pakketten beheren van Solution Explorer in Visual Studio. Voeg het pakket Microsoft.Azure.CognitiveServices.Search.VisualSearch toe.

Wanneer u het NuGet Web Search SDK-pakket installeert, worden onder andere ook de volgende afhankelijkheden geïnstalleerd:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="image-and-crop-area"></a>Afbeelding en bijsnijdgebied
In de volgende afbeelding ziet u het team senior leidinggevenden van Microsoft.  Met de Visual Search-SDK uploadt u een bijsnijdgebied van de afbeelding en zoekt u andere afbeeldingen en webpagina's met de opgegeven entiteit in het geselecteerde deel van de grotere afbeelding.  In dit geval is de entiteit een persoon.

![Senior leidinggevenden bij Microsoft](./media/MS_SrLeaders.jpg)

## <a name="specify-the-crop-area-as-imageinfo-in-visualsearchrequest"></a>Geef het bijsnijdgebied de naam ImageInfo in VisualSearchRequest
In dit voorbeeld wordt het bijsnijdgebied van de vorige afbeelding gebruikt, met de coördinaten linksboven en rechtsonder weergegeven als percentage van de gehele afbeelding.  De volgende code maakt een `ImageInfo`-object van het bijsnijdgebied en laadt het `ImageInfo`-object in een `VisualSearchRequest`.  Het `ImageInfo`-object bevat ook de URL van de onlineafbeelding.

```
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg;
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```
## <a name="search-for-images-similar-to-crop-area"></a>Zoeken naar afbeeldingen die lijken op het bijsnijdgebied
De `VisualSearchRequest` bevat informatie over het bijsnijdgebied in de afbeelding en de bijbehorende URL.  Met de `VisualSearchMethodAsync`-methode worden de resultaten opgehaald.
```
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>De URL-gegevens ophalen uit ImageModuleAction
Visual Search-resultaten zijn `ImageTag`-objecten.  Elke tag bevat een lijst met `ImageAction`-objecten.  Elke `ImageAction` bevat een `Data`-veld met een lijst met waarden die afhankelijk zijn van het type actie:

U kunt de verschillende typen ophalen met de volgende code:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
De volledige toepassing retourneert:

* ActionType: PagesIncluding WebSearchURL:
* ActionType: MoreSizes WebSearchURL:
* ActionType: VisualSearch WebSearchURL:
* ActionType: ImageById WebSearchURL: 
* ActionType: RelatedSearches  WebSearchURL:
* ActionType: Entity -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5380.1
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx,5382.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5384.1

Zoals wordt weergegeven in de voorgaande lijst, bevat de `Entity` `ActionType` een Bing Search-query waarmee informatie wordt geretourneerd over een herkenbare persoon, een herkenbare plaats of een herkenbaar ding.  De typen `TopicResults` en `ImageResults` bevatten zoekopdrachten naar gerelateerde afbeeldingen. De URL's in de lijst zijn koppelingen naar Bing-zoekresultaten.


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

## <a name="complete-code"></a>Volledige code

Met de volgende code worden de eerdere voorbeelden uitgevoerd. Er wordt binaire code van een afbeelding verzonden in de plaatsingsaanvraag, in combinatie met een cropArea-object. De Bing Search-URL's van elke ActionType worden geretourneerd. Als het ActionType PagesIncluding is, worden met de code ImageObject-items opgehaald in de ImageObject-gegevens.  De gegevens bevatten een lijst met waarden: de URL's van afbeeldingen op webpagina's.  Kopieer en plak de resulterende Visual Search-URL's naar de browser om resultaten weer te geven. Kopieer en plak ContentUrl-items naar de browser om afbeeldingen weer te geven.

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

            VisualSearchImageBinaryWithCropArea(subscriptionKey);

            Console.WriteLine("Any key to quit...");
            Console.ReadKey();

        }

        public static void VisualSearchImageBinaryWithCropArea(string subscriptionKey)
        {
            var client = new VisualSearchAPI(new Microsoft.Azure.CognitiveServices.Search.VisualSearch.ApiKeyServiceClientCredentials(subscriptionKey));

            try
            {
                CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
                
                // The ImageInfo struct specifies the crop area in the image and the URL of the larger image. 
                string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg";
                ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);
                
                VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);

                var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

                Console.WriteLine("\r\nVisual search request with image from URL and crop area combined in knowledgeRequest");

                if (visualSearchResults == null)
                {
                    Console.WriteLine("No visual search result data.");
                }
                else
                {
                    // List of tags
                    if (visualSearchResults.Tags.Count > 0)
                    {

                        foreach(ImageTag t in visualSearchResults.Tags)
                        {
                            foreach (ImageAction i in t.Actions)
                            { 
                                Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

                                if (i.ActionType == "PagesIncluding")
                                {
                                    foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
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