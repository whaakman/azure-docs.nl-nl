---
title: Bing Visual Search SDK bijsnijden gebied resulteert zelfstudie | Microsoft Docs
description: Het gebruik van de Bing Visual Search SDK-URL's van installatiekopieën die vergelijkbaar is met het gebied van de geüploade afbeelding bijsnijden ophalen.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: dd51ed7c710cc51a9fe0e63e55aa0d2c4ea24bee
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574486"
---
# <a name="tutorial-bing-visual-search-sdk-image-crop-area-and-results"></a>Zelfstudie: De Bing Visual Search SDK afbeelding bijsnijden oppervlakte en de resultaten
De Visual Search-SDK bevat een optie voor het selecteren van een gebied van een afbeelding en online installatiekopieën die op het gebied van de grote afbeelding bijsnijden lijken zoeken.  In dit voorbeeld geeft gebied bijsnijden met één persoon uit een afbeelding met verschillende mensen.  De code verzendt de gebied bijsnijden en de URL van de grote afbeelding en retourneert resultaten die zijn onder andere Bing zoeken-URL's en URL's van vergelijkbare afbeeldingen online gevonden.

## <a name="prerequisites"></a>Vereisten

U moet [Visual Studio 2017](https://www.visualstudio.com/downloads/) om op te halen van deze code die wordt uitgevoerd op Windows. (De gratis Community-editie volstaat.)

Hebt u een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met Bing zoeken-API's. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) is voldoende voor deze Quick Start. Moet u de toegangssleutel die is opgegeven wanneer u uw gratis proefversie activeren, of u de sleutel van een betaald abonnement van uw Azure-dashboard kunt.

## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen
Als u een consoletoepassing met behulp van de Bing Web Search SDK instelt, blader naar de optie NuGet-pakketten beheren vanuit de Solution Explorer in Visual Studio. Het pakket Microsoft.Azure.CognitiveServices.Search.VisualSearch toevoegen.

Het pakket NuGet Web Search SDK ook installeert, installeert afhankelijkheden, met inbegrip van:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="image-and-crop-area"></a>Afbeelding en bijsnijden gebied
De volgende afbeelding toont het senior management-team van Microsoft.  Met behulp van de Visual Search-SDK, we een gebied bijsnijden van de installatiekopie van het uploaden en de andere afbeeldingen en webpagina's die de entiteit is opgenomen in het geselecteerde gebied van de grote afbeelding zoeken.  In dit geval is de entiteit een persoon.

![Senior Management-Team van Microsoft](./media/MS_SrLeaders.jpg)

## <a name="specify-the-crop-area-as-imageinfo-in-visualsearchrequest"></a>Geef op het gebied bijsnijden als ImageInfo in VisualSearchRequest
Dit voorbeeld wordt een gebied bijsnijden van de vorige afbeelding bovenste links en rechts coördinaten met percentage van de installatiekopie van het hele verlagen.  De volgende code maakt een `ImageInfo` -object van het gebied bijsnijden en belastingen de `ImageInfo` -object in een `VisualSearchRequest`.  De `ImageInfo` object bevat ook de URL van de online-installatiekopie.

```
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg;
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```
## <a name="search-for-images-similar-to-crop-area"></a>Zoeken naar afbeeldingen die vergelijkbaar is met het gebied bijsnijden
De `VisualSearchRequest` bijsnijden gebied informatie over de installatiekopie en de URL bevat.  De `VisualSearchMethodAsync` methode haalt de resultaten.
```
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>De URL-gegevens ophalen uit ImageModuleAction
Visuele zoekresultaten zijn `ImageTag` objecten.  Elke tag bevat een lijst met `ImageAction` objecten.  Elke `ImageAction` bevat een `Data` veld met een lijst met waarden die afhankelijk van het type actie zijn:

U kunt de verschillende typen krijgen door de volgende code:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
De volledige toepassing geeft als resultaat:

* ActionType: PagesIncluding WebSearchURL:
* ActionType: MoreSizes WebSearchURL:
* ActionType: VisualSearch WebSearchURL:
* ActionType: ImageById WebSearchURL: 
* ActionType: RelatedSearches WebSearchURL:
* ActionType: Entiteit -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx, 5380.1
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx, 5382.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx, 5384.1

Zoals wordt weergegeven in de voorgaande lijst de `Entity` `ActionType` bevat een Bing zoeken-query die informatie over een herkenbare persoon, een plaats of een ding retourneert.  De `TopicResults` en `ImageResults` typen query's voor gerelateerde afbeeldingen bevatten. De URL's in de lijst met koppeling naar Bing-zoekresultaten.


## <a name="pagesincluding-actiontype-urls-of-images-found-by-visual-search"></a>PagesIncluding ActionType URL's van installatiekopieën vinden door visuele zoekopdrachten

De werkelijke afbeelding-URL's aan een cast nodig die leest vereist een `ActionType` als `ImageModuleAction`, waarin een `Data` element met een lijst met waarden.  Elke waarde is de URL van een afbeelding.  De volgende webcasts de `PagesIncluding` actietype naar `ImageModuleAction` en de waarden worden gelezen.
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

De volgende code wordt de eerdere voorbeelden uitgevoerd. Het verzendt een installatiekopie van een binaire in de hoofdtekst van de post-aanvraag, samen met een object cropArea en af te drukken om de Bing zoeken op URL's voor elke ActionType. Als de ActionType PagesIncluding, haalt de code de items ImageObject in ImageObject gegevens.  De gegevens bevatten een lijst met waarden die de URL's van installatiekopieën op webpagina's.  Kopieer en plak de resulterende Visual Search-URL's naar de browser om resultaten weer te geven. Kopieer en plak ContentUrl items naar de browser om installatiekopieën weer te geven.

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
[Visual Search-antwoord](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview#the-response)