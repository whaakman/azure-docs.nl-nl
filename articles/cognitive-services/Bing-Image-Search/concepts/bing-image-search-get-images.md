---
title: Ophalen van installatiekopieën van het web met de Bing afbeeldingen zoeken-API | Microsoft Docs
description: De Bing afbeeldingen zoeken-API gebruiken om te zoeken naar en ophalen van relevante afbeeldingen van het web.
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: AB1B9898-C94A-4B59-91A1-8623C94BA3D4
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 8/8/2018
ms.author: aahi
ms.openlocfilehash: c379cc2dfbe53f83e4d79500cd947879407c8d55
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "40082602"
---
# <a name="getting-images-from-the-web-with-the-bing-image-search-api"></a>Ophalen van installatiekopieën van het web met de Bing afbeeldingen zoeken-API

U kunt met behulp van de Bing afbeeldingen zoeken REST-API, afbeeldingen krijgen via het web die gerelateerd zijn aan de zoekterm van de gebruiker door de volgende GET-aanvraag te verzenden:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

> [!IMPORTANT]
> * Alle aanvragen moeten worden gemaakt van een server en niet van een client.
> * Als het de eerste keer aanroepen van een van de Bing zoeken-API's, niet de header van de client-ID bevatten. Als u voorheen een Bing-API die een client-ID voor de gebruiker en apparaat combinatie geretourneerd hebt genoemd, zijn alleen de client-ID.
> * Afbeeldingen moeten worden weergegeven in de volgorde die is opgegeven in het antwoord.

## <a name="getting-images-from-a-specific-web-domain"></a>Ophalen van afbeeldingen uit een specifieke web-domein

Als u afbeeldingen uit een bepaald domein wilt opvragen, gebruikt de query-operator [site:](http://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

> [!NOTE]
> Antwoorden op query's met behulp van de `site:` operator kan bevatten inhoud voor volwassenen ongeacht de [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) instelling. Gebruik alleen `site:` als u zich bewust bent van de inhoud op het domein.

Het volgende voorbeeld laat zien hoe u kleine afbeeldingen opvraagt van ContosoSailing.com die Bing de afgelopen week heeft gedetecteerd.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="filtering-images"></a>Afbeeldingen filteren

 De afbeeldingen zoeken-API wordt standaard alle installatiekopieën die relevant voor de query zijn worden geretourneerd. Als u wilt filteren van de installatiekopieën die Bing retourneert (bijvoorbeeld om terug te keren alleen afbeeldingen met een transparant achtergrond of een specifieke grootte), kunt u de volgende queryparameters:

* [aspect](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#aspect): filter afbeeldingen op hoogte-breedteverhouding (bijvoorbeeld standaardafbeeldingen of afbeeldingen voor een breed scherm)
* [color](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#color): filter afbeeldingen op overheersende kleur of zwart-wit
* [feshness](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#freshness): filter afbeeldingen op leeftijd (bijvoorbeeld afbeeldingen die in de afgelopen week door Bing zijn gedetecteerd)
* [height](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#height), [width](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#width): filter afbeeldingen op breedte en hoogte
* [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagecontent): filter afbeeldingen op inhoud (bijvoorbeeld afbeeldingen die alleen het gezicht van een persoon bevatten)
* [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype): filter afbeeldingen op type (zoals illustraties, GIF-animaties of doorzichtige achtergronden)
* [license](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license): filter afbeeldingen op het type licentie dat aan de site is gekoppeld
* [size](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#size): filter afbeeldingen op grootte, zoals kleine afbeeldingen tot 200x200 pixels

Als u afbeeldingen uit een bepaald domein wilt opvragen, gebruikt de query-operator [site:](http://msdn.microsoft.com/library/ff795613.aspx).

    > [!NOTE]
    > Responses to queries using the `site:` operator may include adult content regardless of the [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) setting. Only use `site:` if you are aware of the content on the domain.

Het volgende voorbeeld laat zien hoe u kleine afbeeldingen opvraagt van ContosoSailing.com die Bing de afgelopen week heeft gedetecteerd.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="bing-image-search-response-format"></a>Bing afbeeldingen zoeken-antwoordindeling

Het antwoordbericht van Bing bevat een [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) antwoord bevat een lijst met installatiekopieën die Azure cognitive services blijkt dat relevant is voor de query. Elke [installatiekopie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) object in de lijst bevat de volgende informatie over de installatiekopie: de URL, de grootte, de dimensies, de coderingsindeling, een URL naar een miniatuur van de installatiekopie en dimensies van de miniatuur.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=73118C8B4E3...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP.GNarK7m...",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=73118C8B4E3D4C3687...",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "en.contoso.org\/wiki\/File:Rich_Passage...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "imageInsightsToken": "ccid_GNarK7ma*mid_CCF85447ADA6...",
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    },
    "imageId": "CCF85447ADA6FFF9E96E7DF0B796F7A86E34593",
    "accentColor": "376094"
},
```

Wanneer u de Bing Afbeeldingen zoeken-API aanroept, retourneert Bing een lijst met resultaten. De lijst is een subset van het totale aantal resultaten die relevant zijn voor de query. Het veld `totalEstimatedMatches` in de respons bevat een schatting van het aantal afbeeldingen dat beschikbaar is om weer te geven. Zie [Afbeeldingen per pagina weergeven](../paging-images.md) voor meer informatie over het paginagewijs weergeven van de resterende afbeeldingen.

## <a name="next-steps"></a>Volgende stappen

Als u de Bing afbeeldingen zoeken-API voordat u dit nog niet hebt geprobeerd, probeert u een [snelstartgids](../quickstarts/csharp.md). Als u ingewikkeldere zoekt, probeert u de zelfstudie voor het maken van een [één pagina web-app](../tutorial-bing-image-search-single-page-app.md).
