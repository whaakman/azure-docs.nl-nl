---
title: "Snelstartgids: Verzenden zoekquery's met de Bing afbeeldingen zoeken-API en PHP"
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
ms.openlocfilehash: ebf8c0269e070c9047d730e58b8e2d3824124e1a
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45574195"
---
# <a name="quickstart-send-search-queries-using-the-bing-image-search-rest-api-and-php"></a>Snelstartgids: Verzenden zoekquery's met behulp van de Bing afbeeldingen zoeken-REST-API en PHP

Gebruik deze Quick Start voor het maken van de eerste aanroep van de Bing afbeeldingen zoeken-API en een JSON-antwoord ontvangen. De eenvoudige toepassing in dit artikel verzendt een zoekquery en geeft de onbewerkte resultaten weer.

Terwijl deze toepassing is geschreven in PHP, de API is een RESTful-Web-compatibel is met elke programmeertaal die HTTP-aanvragen te parseren van JSON.

De broncode voor dit voorbeeld is beschikbaar [op GitHub](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/blob/master/java/Search/BingImageSearchv7.java).

## <a name="prerequisites"></a>Vereisten

* [PHP 5.6.x of hoger](http://php.net/downloads.php).

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>Maken en initialiseren van de toepassing

Volg deze stappen om uit te voeren deze toepassing.

1. Zorg ervoor dat beveiligde HTTP-ondersteuning is ingeschakeld in uw `php.ini` bestand. Op Windows, dit bestand bevindt zich `C:\windows`.
2. Maak een nieuwe PHP-project in uw favoriete IDE of editor.
3. Definieer het API-eindpunt, uw abonnementssleutel en zoekterm.

    ```php
    $endpoint = 'https://api.cognitive.microsoft.com/bing/v7.0/images/search';
    // Replace the accessKey string value with your valid access key.
    $accessKey = 'enter key here';
    $term = 'tropical ocean';
    ```
## <a name="construct-and-perform-a-http-request"></a>Bouwen en uitvoeren van een HTTP-aanvraag

1. De variabelen in de vorige stap gebruiken om voor te bereiden van een HTTP-aanvraag naar de afbeeldingen zoeken-API.

    ```php
    $headers = "Ocp-Apim-Subscription-Key: $key\r\n";
    $options = array ( 'http' => array (
                            'header' => $headers,
                            'method' => 'GET' ));
    ```
2. Uitvoeren van de webaanvraag en ophalen van het JSON-antwoord.

    ```php
    $context = stream_context_create($options);
    $result = file_get_contents($url . "?q=" . urlencode($query), false, $context);
    ```

## <a name="process-and-print-the-json"></a>Verwerken en de JSON afdrukken

Verwerk en afdrukken van het geretourneerde JSON-antwoord.

    ```php
    $headers = array();
        foreach ($http_response_header as $k => $v) {
            $h = explode(":", $v, 2);
            if (isset($h[1]))
                if (preg_match("/^BingAPIs-/", $h[0]) || preg_match("/^X-MSEdge-/", $h[0]))
                    $headers[trim($h[0])] = trim($h[1]);
        }
        return array($headers, $result);
    ```

## <a name="sample-json-response"></a>Voorbeeld van JSON-antwoord

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