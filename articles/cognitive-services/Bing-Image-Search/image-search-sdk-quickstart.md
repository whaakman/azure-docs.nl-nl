---
title: 'Snelstartgids: Zoeken voor afbeeldingen met behulp van de Bing afbeeldingen zoeken-SDK en C#'
description: Gebruik deze Quick Start om te zoeken en -installatiekopieën zoeken op Internet met behulp van de Bing afbeeldingen zoeken-SDK en C#.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 08/28/2018
ms.author: aahi
ms.openlocfilehash: 9e0decb29224b5ad684e1242b8f93e091e08e6b6
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45579247"
---
# <a name="quickstart-search-for-images-with-the-bing-image-search-sdk-and-c"></a>Snelstartgids: Zoeken voor afbeeldingen met de Bing afbeeldingen zoeken-SDK en C#

Met deze Quick Start kunt u uw eerste afbeeldingen zoeken met behulp van de Bing afbeeldingen zoeken SDK, die een wrapper voor de API en bevat de dezelfde functies. Deze eenvoudige C#-toepassing verzendt een zoekquery afbeelding, parseert de JSON-antwoord en Hiermee geeft u de URL van de eerste afbeelding geretourneerd.

De broncode voor dit voorbeeld is beschikbaar [op GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7/BingImageSearch) met extra foutafhandeling en aantekeningen.

## <a name="prerequisites"></a>Vereisten

* Een versie van [Visual Studio 2017](https://visualstudio.microsoft.com/vs/whatsnew/).
* De [cognitieve installatiekopie zoeken NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Search.ImageSearch/1.2.0).

Voor het installeren van de Bing afbeeldingen zoeken-SDK in visual studio, gebruikt u de `Manage NuGet Packages` optie vanuit de Solution Explorer in Visual Studio.

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]


## <a name="create-and-initialize-the-application"></a>Maken en initialiseren van de toepassing

Maak eerst een nieuwe C#-consoletoepassing in Visual Studio. De volgende pakketten toevoegen aan uw project.

```csharp
using System;
using System.Linq;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch;
using Microsoft.Azure.CognitiveServices.Search.ImageSearch.Models;
```

In de belangrijkste methode voor uw project, maakt u variabelen voor de sleutel geldig abonnement, de afbeeldingsresultaten moet worden geretourneerd door Bing en een zoekterm. Instantieer vervolgens de afbeelding zoeken-client met behulp van de sleutel.

```csharp
//IMPORTANT: replace this variable with your Cognitive Services subscription key
string subscriptionKey = "ENTER YOUR KEY HERE";
//stores the image results returned by Bing
Images imageResults = null;
// the image search term to be used in the query
string searchTerm = "canadian rockies";
//initialize the client
var client = new ImageSearchAPI(new ApiKeyServiceClientCredentials(subscriptionKey));
```

## <a name="send-a-search-query-using-the-client"></a>Verzenden van een zoekopdracht met behulp van de client

De client gebruiken om te zoeken met de tekst van een query:

```csharp
// make the search request to the Bing Image API, and get the results"
imageResults = client.Images.SearchAsync(query: searchTerm).Result; //search query
```

## <a name="parse-and-view-the-first-image-result"></a>Parseren en de eerste afbeelding resultaat weergeven

Parseren van de afbeeldingsresultaten in het antwoord geretourneerd.
Als het antwoord bevat een lijst met zoekresultaten, het eerste resultaat opslaan en afdrukken van de details, zoals een miniatuur-URL, wordt de oorspronkelijke URL, samen met het totale aantal installatiekopieën geretourneerd.  

```csharp
if (imageResults != null)
{
    //display the details for the first image result.
    var firstImageResult = imageResults.Value.First();
    Console.WriteLine($"\nTotal number of returned images: {imageResults.Value.Count}\n");
    Console.WriteLine($"Copy the following URLs to view these images on your browser.\n");
    Console.WriteLine($"URL to the first image:\n\n {firstImageResult.ContentUrl}\n");
    Console.WriteLine($"Thumbnail URL for the first image:\n\n {firstImageResult.ThumbnailUrl}");
    Console.ReadKey();
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie voor Bing afbeeldingen zoeken-app met één pagina](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app)

## <a name="see-also"></a>Zie ook 

* [Wat is de Bing afbeeldingen zoeken?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Probeer een online interactieve demo](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Een gratis Cognitive Services-toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [.NET-voorbeelden voor de SDK van Azure Cognitive Services](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/BingSearchv7)
* [Documentatie voor Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Bing afbeeldingen zoeken-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)