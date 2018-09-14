---
title: "Snelstartgids: Verzenden zoeken query's met de Bing afbeeldingen zoeken-API en"
titleSuffix: Azure Cognitive Services
description: Gebruik deze Quick Start om te zoeken en -installatiekopieën zoeken op Internet met behulp van de Bing webzoekopdrachten-API.
services: cognitive-services
documentationcenter: ''
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 9/07/2018
ms.author: aahi
ms.openlocfilehash: 22eb54f6adec0335dd89a5ae906117542bb11717
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45580409"
---
# <a name="quickstart-send-search-queries-using-the-bing-image-search-api-and-c"></a>Snelstartgids: Verzenden zoekquery's met behulp van de Bing afbeeldingen zoeken-API en C#

Gebruik deze Quick Start uit te voeren van de eerste aanroep van de Bing afbeeldingen zoeken-API en weergeven van een zoekresultaat uit het JSON-antwoord. Deze eenvoudige C#-toepassing verzendt een zoekquery voor HTTP-installatiekopie naar de API en wordt de URL van de eerste afbeelding geretourneerd.

Terwijl deze toepassing is geschreven in C#, de API is een RESTful-Web-compatibel is met de meeste moderne programmeertalen.

De broncode voor dit voorbeeld is beschikbaar [op GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/dotnet/Search/BingImageSearchv7Quickstart.cs) met extra foutafhandeling en code-aantekeningen.

## <a name="prerequisites"></a>Vereisten

* Een versie van [Visual Studio 2017](https://www.visualstudio.com/downloads/).
* De [Json.NET](https://www.newtonsoft.com/json) framework, beschikbaar als een NuGet-pakket. 
* Als u van Linux/MacOS gebruikmaakt, deze toepassing kan worden uitgevoerd met [Mono](http://www.mono-project.com/).

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-a-project"></a>Maken en een project initialiseren

1. Maak een nieuwe console-oplossing met de naam `BingSearchApisQuickStart` in Visual Studio. Voeg de volgende naamruimten in de belangrijkste codebestand.

    ```csharp
    using System;
    using System.Net;
    using System.IO;
    using System.Collections.Generic;
    using Newtonsoft.Json.Linq;
    ```

2. Variabelen voor de API-eindpunt, uw abonnementssleutel maken en zoekterm.

    ```csharp
    //...
    namespace BingSearchApisQuickstart
    {
        class Program
        {
        // Replace the this string with your valid access key.
        const string subscriptionKey = "enter your key here";
        const string uriBase = "https://api.cognitive.microsoft.com/bing/v7.0/images/search";
        const string searchTerm = "tropical ocean";
    //...
    ```

## <a name="create-a-struct-to-format-the-bing-image-search-response"></a>Maken van een structuur voor het opmaken van het antwoord van de Bing afbeeldingen zoeken

Definieer een `SearchResult` structuur om de zoekresultaten voor installatiekopieën en JSON-headerinformatie bevatten.
    
```csharp
    namespace BingSearchApisQuickstart
    {
        class Program
        {
        //...
            struct SearchResult
            {
                public String jsonResult;
                public Dictionary<String, String> relevantHeaders;
            }
//...
```

## <a name="create-a-method-to-send-search-requests"></a>Een methode voor het verzenden van zoekaanvragen maken

Maken van een methode met de naam `BingImageSearch` het uitvoeren van de aanroep naar de API en het retourtype ingesteld op de `SearchResult` struct eerder hebt gemaakt.

```csharp
//...
namespace BingSearchApisQuickstart
{
    //...
    class Program
    {
        //...
        static SearchResult BingImageSearch(string searchTerm)
        {
        }
//...
```

## <a name="create-and-handle-an-image-search-request"></a>Maken en een afbeelding zoeken-aanvragen worden verwerkt
 
In de `BingImageSearch` methode, voer de volgende stappen uit.

1. De URI voor de zoekopdracht maken. Houd er rekening mee dat de zoekterm `toSearch` moet zijn geformatteerd voordat het wordt toegevoegd aan de tekenreeks. 

    ```csharp
    static SearchResult BingImageSearch(string toSearch){

        var uriQuery = uriBase + "?q=" + Uri.EscapeDataString(toSearch);
    //...
    ```

2. De webaanvraag uitvoeren en een antwoord als een JSON-tekenreeks.

    ```csharp
    WebRequest request = WebRequest.Create(uriQuery);
    request.Headers["Ocp-Apim-Subscription-Key"] = subscriptionKey;
    HttpWebResponse response = (HttpWebResponse)request.GetResponseAsync().Result;
    string json = new StreamReader(response.GetResponseStream()).ReadToEnd();
    ```

3. Maken van het resultaatobject zoeken en extraheren van de Bing-HTTP-headers. Ga vervolgens terug `searchResult`.

    ```csharp
    // Create the result object for return
    var searchResult = new SearchResult()
    {
        jsonResult = json,
        relevantHeaders = new Dictionary<String, String>()
    };

    // Extract Bing HTTP headers
    foreach (String header in response.Headers)
    {
        if (header.StartsWith("BingAPIs-") || header.StartsWith("X-MSEdge-"))
            searchResult.relevantHeaders[header] = response.Headers[header];
    }
    return searchResult;
    ```

## <a name="process-and-view-the-response"></a>Proces en bekijk het antwoord

1. In de belangrijkste methode aanroepen `BingImageSearch()` en het geretourneerde antwoord opslaan. Vervolgens de JSON naar een object te deserialiseren.

    ```csharp
    SearchResult result = BingImageSearch(searchTerm);
    //deserialize the JSON response from the Bing Image Search API
    dynamic jsonObj = Newtonsoft.Json.JsonConvert.DeserializeObject(result.jsonResult);
    ```

2. Ophalen van de installatiekopie van het eerste geretourneerde van `jsonObj`, en de titel en een URL naar de installatiekopie van het afdrukken. 
    ```csharp
    var firstJsonObj = jsonObj["value"][0];
    Console.WriteLine("Title for the first image result: " + firstJsonObj["name"]+"\n");
    //After running the application, copy the output URL into a browser to see the image. 
    Console.WriteLine("URL for the first image result: " + firstJsonObj["webSearchUrl"]+"\n");
    ```  
       
3. Zorg ervoor dat uw abonnementssleutel verwijderen uit de code van de toepassing.

## <a name="json-response"></a>JSON-antwoord

Antwoorden van de Bing afbeeldingen zoeken-API worden geretourneerd als JSON. Dit voorbeeldantwoord is ingekort zodat één resultaat wordt weergegeven.

```json
{
"_type":"Images",
"instrumentation":{
    "_type":"ResponseInstrumentation"
},
"readLink":"images\/search?q=tropical ocean",
"webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=tropical ocean&FORM=OIIARP",
"totalEstimatedMatches":842,
"nextOffset":47,
"value":[
    {
        "webSearchUrl":"https:\/\/www.bing.com\/images\/search?view=detailv2&FORM=OIIRPO&q=tropical+ocean&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&simid=608027248313960152",
        "name":"My Life in the Ocean | The greatest WordPress.com site in ...",
        "thumbnailUrl":"https:\/\/tse3.mm.bing.net\/th?id=OIP.fmwSKKmKpmZtJiBDps1kLAHaEo&pid=Api",
        "datePublished":"2017-11-03T08:51:00.0000000Z",
        "contentUrl":"https:\/\/mylifeintheocean.files.wordpress.com\/2012\/11\/tropical-ocean-wallpaper-1920x12003.jpg",
        "hostPageUrl":"https:\/\/mylifeintheocean.wordpress.com\/",
        "contentSize":"897388 B",
        "encodingFormat":"jpeg",
        "hostPageDisplayUrl":"https:\/\/mylifeintheocean.wordpress.com",
        "width":1920,
        "height":1200,
        "thumbnail":{
        "width":474,
        "height":296
        },
        "imageInsightsToken":"ccid_fmwSKKmK*mid_8607ACDACB243BDEA7E1EF78127DA931E680E3A5*simid_608027248313960152*thid_OIP.fmwSKKmKpmZtJiBDps1kLAHaEo",
        "insightsMetadata":{
        "recipeSourcesCount":0,
        "bestRepresentativeQuery":{
            "text":"Tropical Beaches Desktop Wallpaper",
            "displayText":"Tropical Beaches Desktop Wallpaper",
            "webSearchUrl":"https:\/\/www.bing.com\/images\/search?q=Tropical+Beaches+Desktop+Wallpaper&id=8607ACDACB243BDEA7E1EF78127DA931E680E3A5&FORM=IDBQDM"
        },
        "pagesIncludingCount":115,
        "availableSizesCount":44
        },
        "imageId":"8607ACDACB243BDEA7E1EF78127DA931E680E3A5",
        "accentColor":"0050B2"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie voor Bing afbeeldingen zoeken-app met één pagina](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Zie ook 

* [Wat is de Bing afbeeldingen zoeken?](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/overview)  
* [Probeer een online interactieve demo](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
* [Een gratis Cognitive Services-toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
* [Documentatie voor Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services)
* [Bing afbeeldingen zoeken-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)