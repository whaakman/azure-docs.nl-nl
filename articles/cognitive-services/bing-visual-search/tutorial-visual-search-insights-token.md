---
title: Vergelijkbare afbeeldingen uit eerdere zoekacties ImageInsightsToken - Bing visuele zoekopdrachten met zoeken
titleSuffix: Azure Cognitive Services
description: Gebruik de Bing Visual Search SDK om URL's van installatiekopieën die zijn opgegeven door ImageInsightsToken.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: article
ms.date: 04/05/2019
ms.author: rosh
ms.openlocfilehash: 39a95e877c766eb8f491c166edeb9d96f21db7dd
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/11/2019
ms.locfileid: "59493942"
---
# <a name="find-similar-images-from-previous-searches-using-imageinsightstoken"></a>Vergelijkbare afbeeldingen uit eerdere zoekacties met behulp van ImageInsightsToken zoeken

De Visual Search-SDK kunt u installatiekopieën online uit eerdere zoekacties die retourneren vindt u een `ImageInsightsToken`. Deze toepassing wordt een `ImageInsightsToken` en gebruikt het token in een toekomstige zoekopdracht. Verzendt vervolgens de `ImageInsightsToken` Bing en retourneert resultaten die zijn onder andere Bing zoeken-URL's en URL's van vergelijkbare afbeeldingen online gevonden.

De volledige broncode voor deze zelfstudie vindt u aanvullende foutafhandeling en aantekeningen op [GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/Tutorials/Bing-Visual-Search/BingVisualSearchInisghtsTokens.cs).

## <a name="prerequisites"></a>Vereisten

* Elke versie van [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* Als u van Linux/MacOS gebruikmaakt, kunt u uitvoeren gebruikt voor deze toepassing [Mono](https://www.mono-project.com/).
* De visuele zoekopdrachten-NuGet en afbeeldingen zoeken-pakketten.
    - Vanuit de Solution Explorer in Visual Studio, met de rechtermuisknop op uw project en selecteer **NuGet-pakketten beheren** in het menu. Installeer de `Microsoft.Azure.CognitiveServices.Search.CustomSearch` pakket, en de `Microsoft.Azure.CognitiveServices.Search.ImageSearch` pakket. Als u de NuGet-pakketten installeert, worden ook de volgende onderdelen geïnstalleerd:
        - Microsoft.Rest.ClientRuntime
        - Microsoft.Rest.ClientRuntime.Azure
        - Newtonsoft.Json


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../includes/cognitive-services-bing-visual-search-signup-requirements.md)]

## <a name="get-the-imageinsightstoken-from-the-bing-image-search-sdk"></a>De ImageInsightsToken ophalen uit de Bing afbeeldingen zoeken SDK

Deze toepassing gebruikt een `ImageInsightsToken` hebben verkregen via de [Bing afbeeldingen zoeken SDK](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/image-search-sdk-quickstart). Een nieuwe C# consoletoepassing, maakt u een client voor het aanroepen van de API kunnen doen met `ImageSearchAPI()`. Gebruik vervolgens `SearchAsync()` met uw query:

```csharp
var client = new ImageSearchAPI(new Microsoft.Azure.CognitiveServices.Search.ImageSearch.ApiKeyServiceClientCredentials(subKey));
var imageResults = client.Images.SearchAsync(query: "canadian rockies").Result;
Console.WriteLine("Search images for query \"canadian rockies\"");
```

De eerste zoekopdracht leiden tot met behulp van Store `imageResults.Value.First()`, en slaat u vervolgens de installatiekopie-inzicht `ImageInsightsToken`.

```csharp
String insightTok = "None";
if (imageResults.Value.Count > 0)
{
    var firstImageResult = imageResults.Value.First();
    insightTok = firstImageResult.ImageInsightsToken;
}
else
{
    insightTok = "None found";
    Console.WriteLine("Couldn't find image results!");
}
```

Dit `ImageInsightsToken` in een aanvraag wordt verzonden naar Bing visuele zoekopdrachten.

## <a name="add-the-imageinsightstoken-to-a-visual-search-request"></a>De ImageInsightsToken toevoegen aan een visuele zoekopdrachten-aanvraag

Geef de `ImageInsightsToken` voor een aanvraag voor een visueel zoeken door het maken van een `ImageInfo` object uit de `ImageInsightsToken` die is opgenomen in antwoorden van Bing visuele zoekopdrachten.

```csharp
ImageInfo ImageInfo = new ImageInfo(imageInsightsToken: insightsTok);
```

## <a name="use-bing-visual-search-to-find-images-from-an-imageinsightstoken"></a>Gebruik Bing visuele zoekopdrachten-installatiekopieën zoeken vanuit een ImageInsightsToken

De `VisualSearchRequest` object bevat informatie over de afbeelding in `ImageInfo` moet worden gezocht. Met de `VisualSearchMethodAsync()`-methode worden de resultaten opgehaald. U hoeft voor een afbeelding binary, zoals de installatiekopie wordt vertegenwoordigd door het token.

```csharp
VisualSearchRequest VisualSearchRequest = new VisualSearchRequest(ImageInfo);

var visualSearchResults = client.Images.VisualSearchMethodAsync(knowledgeRequest: VisualSearchRequest).Result;

```

## <a name="iterate-through-the-visual-search-results"></a>Doorlopen van de Visual Search-resultaten

Visual Search-resultaten zijn `ImageTag`-objecten. Elke tag bevat een lijst met `ImageAction`-objecten. Elke `ImageAction` bevat een `Data` veld, die een lijst met waarden die afhankelijk van het type actie zijn is. U kunt doorlopen de `ImageTag` objecten in `visualSearchResults.Tags`, voor het exemplaar en haal de `ImageAction` label binnen deze. Het onderstaande voorbeeld worden de details van afgedrukt `PagesIncluding` acties:

```csharp
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
```

### <a name="pagesincluding-actiontypes"></a>PagesIncluding ActionTypes

De werkelijke afbeelding-URL's ophalen uit actietypen vereist een cast nodig die leest een `ActionType` als `ImageModuleAction`, waarin een `Data` element met een lijst met waarden. Elke waarde is de URL van een afbeelding.  De volgende webcasts de `PagesIncluding` actietype naar `ImageModuleAction` en de waarden worden gelezen:

```csharp
    if (i.ActionType == "PagesIncluding")
    {
        foreach(ImageObject o in (i as ImageModuleAction).Data.Value)
        {
            Console.WriteLine("ContentURL: " + o.ContentUrl);
        }
    }
```

Zie [Afbeeldingen - Visual Search](https://docs.microsoft.com/rest/api/cognitiveservices/bingvisualsearch/images/visualsearch) voor meer informatie over deze gegevenstypen.

## <a name="returned-urls"></a>Geretourneerde URL 's

De volledige toepassing retourneert de volgende URL's:

|ActionType  |URL  | |
|---------|---------|---------|
|MoreSizes -> WebSearchUrl     |         |
|VisualSearch -> WebSearchUrl     |         |
|ImageById -> WebSearchUrl    |         |
|RelatedSearches -> WebSearchUrl:    |         |
|DocumentLevelSuggestions -> WebSearchUrl:     |         |
|TopicResults WebSearchUrl ->    | https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=BcQifmzdKFyyBusjLxxgO42kzq1Geh7RucVVqvH-900&v=1&r=https%3a%2f%2fwww.bing.com%2fdiscover%2fcanadian%2brocky&p=DevEx,5823.1       |
|ImageResults WebSearchUrl ->    |  https://www.bing.com/cr?IG=3E32CC6CA5934FBBA14ABC3B2E4651F9&CID=1BA795A21EAF6A63175699B71FC36B7C&rd=1&h=PV9GzMFOI0AHZp2gKeWJ8DcveSDRE3fP2jHDKMpJSU8&v=1&r=https%3a%2f%2fwww.bing.com%2fimages%2fsearch%3fq%3doutdoor&p=DevEx,5831.1       |

Zoals hierboven, de `TopicResults` en `ImageResults` typen query's voor gerelateerde afbeeldingen bevatten. De URL-koppeling naar Bing-zoekresultaten.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een visuele zoekopdrachten één pagina web-app maken](tutorial-bing-visual-search-single-page-app.md)
