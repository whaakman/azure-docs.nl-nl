---
title: 'Zelfstudie: Een afbeelding met de Bing Visual Search SDK bijsnijden'
description: De Bing Visual Search SDK gebruiken om inzichten te verkrijgen van specifieke are op een afbeelding.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: article
ms.date: 06/20/2018
ms.author: rosh
ms.openlocfilehash: 6444a49a95ee53fb624501bc464f6378423e6e26
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55863020"
---
# <a name="tutorial-crop-an-image-with-the-bing-visual-search-sdk-for-c"></a>Zelfstudie: Een afbeelding met de Bing Visual Search SDK voor bijsnijdenC#

De Bing Visual Search SDK kunt u een installatiekopie voor online zoeken-installatiekopieën die vergelijkbaar zijn bijgesneden. Deze toepassing wordt bijgesneden één persoon uit een afbeelding met verschillende mensen en retourneert vervolgens zoekresultaten met vergelijkbare afbeeldingen online gevonden.

De volledige broncode voor deze toepassing is beschikbaar met extra foutafhandeling en aantekeningen op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchCropImage.cs).

In deze zelfstudie laat zien hoe u:

> [!div class="checklist"]
> * Een aanvraag met behulp van de Bing Visual Search SDK verzenden
> * Een gedeelte van de afbeelding om te zoeken met Bing visuele zoekopdrachten bijsnijden
> * Ontvangen en verwerken van het antwoord
> * De URL's van de actie-items niet vinden in de resposne

## <a name="prerequisites"></a>Vereisten

* Elke versie van [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* Als u Linux/MacOS gebruikt, kan deze toepassing worden uitgevoerd met behulp van [Mono](http://www.mono-project.com/).
- Het [NuGet-pakket Custom Search](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.CustomSearch/1.2.0) is geïnstalleerd. 
    - Klik in Solution Explorer in Visual Studio met de rechtermuisknop op uw project en selecteer `Manage NuGet Packages` in het menu. Installeer het `Microsoft.Azure.CognitiveServices.Search.CustomSearch`-pakket. Met de installatie van het NuGet-pakket Custom Search worden ook de volgende assembly's geïnstalleerd:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="specify-the-image-crop-area"></a>Geef de afbeelding bijsnijden gebied

Deze toepassing wordt een gebied van deze installatiekopie van het senior management-team van Microsoft. Dit gebied bijsnijden is gedefinieerd met behulp van de linker- en rechtsonder coördinaten, weergegeven als een percentage van de volledige afbeelding.  

![Senior leidinggevenden bij Microsoft](./media/MS_SrLeaders.jpg)

Deze afbeelding wordt bijgesneden door het maken van een `ImageInfo` object in het gebied bijsnijden en het laden van de `ImageInfo` -object in een `VisualSearchRequest`. De `ImageInfo` object bevat ook de URL van de afbeelding.

```csharp
CropArea CropArea = new CropArea(top: (float)0.01, bottom: (float)0.30, left: (float)0.01, right: (float)0.20);
string imageURL = "https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/media/ms_srleaders.jpg;
ImageInfo imageInfo = new ImageInfo(cropArea: CropArea, url: imageURL);

VisualSearchRequest visualSearchRequest = new VisualSearchRequest(imageInfo: imageInfo);
```

## <a name="search-for-images-similar-to-the-crop-area"></a>Zoeken naar afbeeldingen die vergelijkbaar is met het gebied bijsnijden

de variabele `VisualSearchRequest` bevat informatie over van de afbeelding bijsnijden vlak- en de url. Met de `VisualSearchMethodAsync()`-methode worden de resultaten opgehaald.

```csharp
Console.WriteLine("\r\nSending visual search request with knowledgeRequest that contains URL and crop area");
var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: visualSearchRequest).Result; 

```

## <a name="get-the-url-data-from-imagemoduleaction"></a>De URL-gegevens ophalen uit ImageModuleAction

Bing visuele zoekopdrachten-resultaten zijn `ImageTag` objecten.  Elke tag bevat een lijst met `ImageAction`-objecten.  Elke `ImageAction` bevat een `Data` veld met een lijst met waarden die afhankelijk van het type actie zijn.

U kunt de verschillende typen met de volgende code afdrukken:

```csharp
Console.WriteLine("\r\n" + "ActionType: " + i.ActionType + " -> WebSearchUrl: " + i.WebSearchUrl);
```

De volledige toepassing retourneert:


|ActionType  |URL  | |
|---------|---------|---------|
|PagesIncluding WebSearchURL     |         |         
|MoreSizes WebSearchURL     |         |         
|VisualSearch WebSearchURL    |         |         
|ImageById WebSearchURL     |         |         
|RelatedSearches WebSearchURL     |         |         
|Entiteit -> WebSearchUrl     | https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=BvvDoRtmZ35Xc_UZE4lZx6_eg7FHgcCkigU1D98NHQo&v=1&r=https%3a%2f%2fwww.bing.com%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5380.1        |         
|TopicResults WebSearchUrl ->    |  https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=3QGtxPb3W9LemuHRxAlW4CW7XN4sPkUYCUynxAqI9zQ&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fnadella%2bsatya&p=DevEx,5382.1        |         
|ImageResults WebSearchUrl ->    |  https://www.bing.com/cr?IG=E40D0E1A13404994ACB073504BC937A4&CID=03DCF882D7386A442137F49BD6596BEF&rd=1&h=l-WNHO89Kkw69AmIGe2MhlUp6MxR6YsJszgOuM5sVLs&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3dSatya%2bNadella&p=DevEx,5384.1        |         

Zoals hierboven, de `Entity` ActionType bevat een Bing-zoekquery die informatie over een herkenbare persoon, een plaats of een ding retourneert.  De typen `TopicResults` en `ImageResults` bevatten zoekopdrachten naar gerelateerde afbeeldingen. De URL's in de lijst zijn koppelingen naar Bing-zoekresultaten.


## <a name="get-urls-for-pagesincluding-actiontype-images"></a>URL's ophalen voor PagesIncluding ActionType afbeeldingen

Voor het ophalen van de werkelijke afbeeldings-URL's is een cast vereist die een `ActionType` leest als `ImageModuleAction`, die een `Data`-element met een waardelijst bevat.  Elke waarde is de URL van een afbeelding.  De volgende code cast het actietype `PagesIncluding` naar `ImageModuleAction` en leest de waarden.

```csharp
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Een web-app van één pagina maken](tutorial-bing-visual-search-single-page-app.md).

[Reactie van Visual Search](https://docs.microsoft.com/azure/cognitive-services/bing-visual-search/overview)
