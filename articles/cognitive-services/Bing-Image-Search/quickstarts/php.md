---
title: "Snelstartgids: Verzenden zoekquery's met de REST-API voor de Bing afbeeldingen zoeken-API met PHP"
description: In deze Quick Start stuurt u zoekopdrachten naar de Bing webzoekopdrachten-API om een lijst met relevante afbeeldingen met behulp van PHP.
services: cognitive-services
documentationcenter: ''
author: v-jerkin
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 9/21/2017
ms.author: v-jerkin
ms.openlocfilehash: d91021c4bd5e0f78e518811f3794055b397c1a39
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "41994384"
---
# <a name="quickstart-send-search-queries-using-the-rest-api-and-php"></a>Snelstartgids: Verzenden zoekquery's met behulp van de REST-API en PHP

De Bing afbeeldingen zoeken-API biedt een ervaring die vergelijkbaar is met Bing.com/Images doordat u kunt een zoekquery voor de gebruiker naar Bing te verzenden en een lijst met relevante afbeeldingen teruggaan.

Dit artikel bevat een eenvoudige consoletoepassing die een Bing afbeeldingen zoeken-API-query en de zoekresultaten geretourneerde onbewerkte, die zich in de JSON-indeling worden weergegeven. Terwijl deze toepassing is geschreven in PHP, de API is een RESTful-Web-compatibel is met elke programmeertaal die HTTP-aanvragen te parseren van JSON. 

## <a name="prerequisites"></a>Vereisten

U moet [PHP 5.6.x](http://php.net/downloads.php) deze code uit te voeren.

[!INCLUDE [cognitive-services-bing-image-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="running-the-application"></a>De toepassing uitvoeren

Volg deze stappen om uit te voeren deze toepassing.

1. Zorg ervoor dat beveiligde HTTP-ondersteuning is ingeschakeld in uw `php.ini` zoals beschreven in de commentaar bij code. Op Windows, wordt dit bestand is in `C:\windows`.
2. Maak een nieuwe PHP-project in uw favoriete IDE of editor.
3. De opgegeven code toevoegen.
4. Vervang de `accessKey` waarde met een geldige toegangssleutel voor uw abonnement.
5. Voer het programma.

```php
<?php

// NOTE: Be sure to uncomment the following line in your php.ini file.
// ;extension=php_openssl.dll

// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the accessKey string value with your valid access key.
$accessKey = 'enter key here';

// Verify the endpoint URI.  At this writing, only one endpoint is used for Bing
// search APIs.  In the future, regional endpoints may be available.  If you
// encounter unexpected authorization errors, double-check this value against
// the endpoint for your Bing Search instance in your Azure dashboard.
$endpoint = 'https://api.cognitive.microsoft.com/bing/v7.0/images/search';

$term = 'puppies';

function BingImageSearch ($url, $key, $query) {
    // Prepare HTTP request
    // NOTE: Use the key 'http' even if you are making an HTTPS request. See:
    // http://php.net/manual/en/function.stream-context-create.php
    $headers = "Ocp-Apim-Subscription-Key: $key\r\n";
    $options = array ( 'http' => array (
                           'header' => $headers,
                           'method' => 'GET' ));

    // Perform the Web request and get the JSON response
    $context = stream_context_create($options);
    $result = file_get_contents($url . "?q=" . urlencode($query), false, $context);

    // Extract Bing HTTP headers
    $headers = array();
    foreach ($http_response_header as $k => $v) {
        $h = explode(":", $v, 2);
        if (isset($h[1]))
            if (preg_match("/^BingAPIs-/", $h[0]) || preg_match("/^X-MSEdge-/", $h[0]))
                $headers[trim($h[0])] = trim($h[1]);
    }

    return array($headers, $result);
}

if (strlen($accessKey) == 32) {

    print "Searching images for: " . $term . "\n";
    
    list($headers, $json) = BingImageSearch($endpoint, $accessKey, $term);
    
    print "\nRelevant Headers:\n\n";
    foreach ($headers as $k => $v) {
        print $k . ": " . $v . "\n";
    }
    
    print "\nJSON Response:\n\n";
    echo json_encode(json_decode($json), JSON_PRETTY_PRINT);

} else {

    print("Invalid Bing Search API subscription key!\n");
    print("Please paste yours into the source code.\n");

}
?>
```

## <a name="json-response"></a>JSON-antwoord

Er volgt een voorbeeldantwoord. Als u wilt beperken de lengte van de JSON, slechts één resultaat wordt weergegeven en andere onderdelen van het antwoord is afgekapt. 

```json
{
  "_type": "Images",
  "instrumentation": {},
  "readLink": "https://api.cognitive.microsoft.com/api/v7/images/search?q=puppies",
  "webSearchUrl": "https://www.bing.com/images/search?q=puppies&FORM=OIIARP",
  "totalEstimatedMatches": 955,
  "nextOffset": 1,
  "value": [
    {
      "webSearchUrl": "https://www.bing.com/images/search?view=detailv...",
      "name": "So cute - Puppies Wallpaper",
      "thumbnailUrl": "https://tse3.mm.bing.net/th?id=OIP.jHrihoDNkXGS1t...",
      "datePublished": "2014-02-01T21:55:00.0000000Z",
      "contentUrl": "http://images4.contoso.com/image/photos/14700000/So-cute-puppies...",
      "hostPageUrl": "http://www.contoso.com/clubs/puppies/images/14749028/...",
      "contentSize": "394455 B",
      "encodingFormat": "jpeg",
      "hostPageDisplayUrl": "www.contoso.com/clubs/puppies/images/14749...",
      "width": 1600,
      "height": 1200,
      "thumbnail": {
        "width": 300,
        "height": 225
      },
      "imageInsightsToken": "ccid_jHrihoDN*mid_F68CC526226E163FD1EA659747AD...",
      "insightsMetadata": {
        "recipeSourcesCount": 0
      },
      "imageId": "F68CC526226E163FD1EA659747ADCB8F9FA36",
      "accentColor": "8D613E"
    }
  ],
  "queryExpansions": [
    {
      "text": "Shih Tzu Puppies",
      "displayText": "Shih Tzu",
      "webSearchUrl": "https://www.bing.com/images/search?q=Shih+Tzu+Puppies...",
      "searchLink": "https://api.cognitive.microsoft.com/api/v7/images/search?q=Shih...",
      "thumbnail": {
        "thumbnailUrl": "https://tse2.mm.bing.net/th?q=Shih+Tzu+Puppies&pid=Api..."
      }
    }
  ],
  "pivotSuggestions": [
    {
      "pivot": "puppies",
      "suggestions": [
        {
          "text": "Dog",
          "displayText": "Dog",
          "webSearchUrl": "https://www.bing.com/images/search?q=Dog&tq=%7b%22pq%...",
          "searchLink": "https://api.cognitive.microsoft.com/api/v7/images/search?q=Dog...",
          "thumbnail": {
            "thumbnailUrl": "https://tse1.mm.bing.net/th?q=Dog&pid=Api&mkt=en-US..."
          }
        }
      ]
    }
  ],
  "similarTerms": [
    {
      "text": "cute",
      "displayText": "cute",
      "webSearchUrl": "https://www.bing.com/images/search?q=cute&FORM=...",
      "thumbnail": {
        "url": "https://tse2.mm.bing.net/th?q=cute&pid=Api&mkt=en-US..."
      }
    }
  ],
  "relatedSearches": [
    {
      "text": "Cute Puppies",
      "displayText": "Cute Puppies",
      "webSearchUrl": "https://www.bing.com/images/search?q=Cute+Puppies",
      "searchLink": "https://api.cognitive.microsoft.com/api/v7/images/sear...",
      "thumbnail": {
        "thumbnailUrl": "https://tse4.mm.bing.net/th?q=Cute+Puppies&pid=..."
      }
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie voor Bing afbeeldingen zoeken-app met één pagina](../tutorial-bing-image-search-single-page-app.md)

## <a name="see-also"></a>Zie ook 

[Overzicht van de Bing afbeeldingen zoeken](../overview.md)  
[Nu uitproberen](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/)  
[Een gratis proefversie toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-image-search-api)  
[Bing afbeeldingen zoeken-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
