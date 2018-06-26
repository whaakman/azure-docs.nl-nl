---
title: Gebied van Bing Visual Search SDK bijsnijden resulteert zelfstudie | Microsoft Docs
description: Klik hier voor meer informatie over het gebruik van de Bing-SDK voor Visual zoeken naar URL's van installatiekopieën lijkt op het gebied van geüploade afbeelding bijsnijden ophalen.
services: cognitive-services
author: mikedodaro
manager: ronakshah
ms.service: cognitive-services
ms.component: bing-visual-search
ms.topic: article
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: fed9bb396d72f140235a2743c1447076606bb87c
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36939453"
---
# <a name="tutorial-bing-visual-search-sdk-image-crop-area-and-results"></a>Zelfstudie: Gebied van Bing Visual Search SDK afbeelding bijsnijden en resultaten
De visuele Search SDK bevat een optie te selecteren van een gebied van een installatiekopie en online installatiekopieën die vergelijkbaar met het gebied van de grote afbeelding bijsnijden zijn te vinden.  In dit voorbeeld geeft gebied bijsnijden één persoon van een afbeelding met verschillende personen bevat die wordt weergegeven.  De code verzendt gebied bijsnijden en de URL van de grote afbeelding en Bing zoeken-URL's en van soortgelijke afbeeldingen gevonden online omvatten de resultaten geretourneerd.

## <a name="prerequisites"></a>Vereisten

U moet [Visual Studio 2017](https://www.visualstudio.com/downloads/) deze code die wordt uitgevoerd op Windows ophalen. (De editie free Community blijft werken.)

U moet hebben een [cognitieve Services API account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met Bing zoeken-API's. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) voldoende is voor deze snelstartgids. U moet de toegangssleutel die is opgegeven bij het activeren van uw gratis proefversie of u kunt de sleutel van een betaald abonnement van uw Azure-dashboard.

## <a name="application-dependencies"></a>Afhankelijkheden voor toepassingen
Als u een consoletoepassing met de Bing Web Search SDK instelt, blader naar de optie NuGet-pakketten beheren vanuit de Solution Explorer in Visual Studio. Het pakket Microsoft.Azure.CognitiveServices.Search.VisualSearch toevoegen.

Het pakket NuGet Web Search SDK installeert, installeert ook afhankelijkheden, met inbegrip van:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

## <a name="image-and-crop-area"></a>De installatiekopie en bijsnijden gebied
De volgende afbeelding toont de senior leiding van Microsoft.  Met behulp van de Visual Search SDK we een gebied bijsnijden van de installatiekopie uploaden en de andere afbeeldingen en webpagina's die de entiteit in het geselecteerde gebied van de grote afbeelding opnemen zoeken.  In dit geval wordt is de entiteit een persoon.

![Senior leiding van Microsoft](./media/MS_SrLeaders.jpg)

## <a name="specify-the-crop-area-as-imageinfo-in-visualsearchrequest"></a>Geef op het gebied bijsnijden als ImageInfo in VisualSearchRequest
In dit voorbeeld wordt een gebied bijsnijden van de vorige afbeelding die links bovenste geeft en het juiste coördinaten verlagen door percentage van de volledige afbeelding.  De volgende code maakt een `ImageInfo` object in het gebied bijsnijden en laadt de `ImageInfo` -object in een `VisualSearchRequest`.  De `ImageInfo` object bevat ook de URL van de afbeelding online.

```
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.25, left: (float)0.01, right: (float)0.2);
string imageURL = "http://windowsgeek.lk/wp-content/uploads/2016/04/cxc.png";
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```
## <a name="search-for-images-similar-to-crop-area"></a>Zoeken naar afbeeldingen die lijkt op het gebied bijsnijden
De `VisualSearchRequest` bijsnijden gebied informatie bevat over de installatiekopie en de bijbehorende URL.  De `VisualSearchMethodAsync` methode haalt de resultaten.
```
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>De URL-gegevens ophalen uit ImageModuleAction
Visual zoekresultaten zijn `ImageTag` objecten.  Elke tag bevat een lijst met `ImageAction` objecten.  Elke `ImageAction` bevat een `Data` veld met een lijst met waarden die afhankelijk van het type actie zijn:

U kunt de verschillende typen krijgen met de volgende code:
```
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);

```
De volledige toepassing geretourneerd:

* ActionType: PagesIncluding WebSearchURL:
* ActionType: MoreSizes WebSearchURL:
* ActionType: VisualSearch WebSearchURL:
* ActionType: ImageById WebSearchURL: 
* ActionType: RelatedSearches WebSearchURL:
* ActionType: Entiteit -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx, 5380.1
* ActionType: TopicResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx, 5382.1
* ActionType: ImageResults -> WebSearchUrl: https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx, 5384.1

Zoals wordt weergegeven in de voorgaande lijst de `Entity` `ActionType` bevat een Bing-zoekquery die informatie over een herkenbare persoon, plaats of object retourneert.  De `TopicResults` en `ImageResults` typen query's voor verwante installatiekopieën bevatten. De URL's in de lijst koppelen aan Bing zoekresultaten.


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

## <a name="complete-code"></a>Volledige code

De volgende code wordt de eerdere voorbeelden uitgevoerd. Verzendt hij een installatiekopie van een binaire in de hoofdtekst van de post-aanvraag, samen met een object cropArea en afdrukken de Bing zoeken URL's voor elke ActionType. Als de ActionType PagesIncluding is, haalt de code de items ImageObject in ImageObject gegevens.  De gegevens bevatten een lijst met waarden die de URL's van afbeeldingen op webpagina's.  Kopieer en plak de resulterende Visual URL's zoeken naar de browser om resultaten weer te geven. Kopieer en plak ContentUrl items naar de browser om afbeeldingen weer te geven.

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
                CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.25, left: (float)0.01, right: (float)0.2);
                
                // The ImageInfo struct specifies the crop area in the image and the URL of the larger image. 
                string imageURL = "http://windowsgeek.lk/wp-content/uploads/2016/04/cxc.png";
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
[Visual zoeken antwoord](https://docs.microsoft.com/en-us/azure/cognitive-services/bing-visual-search/overview#the-response)