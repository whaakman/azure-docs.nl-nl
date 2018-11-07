---
title: Afbeeldingen ophalen van de web - Bing afbeeldingen zoeken-API
titleSuffix: Azure Cognitive Services
description: De Bing afbeeldingen zoeken-API gebruiken om te zoeken naar en relevante afbeeldingen van het web.
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: AB1B9898-C94A-4B59-91A1-8623C94BA3D4
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 8/8/2018
ms.author: aahi
ms.openlocfilehash: 621c2844b6952e5ea1f6b9f7c8bc2ed170fee310
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51246783"
---
# <a name="get-images-from-the-web-with-the-bing-image-search-api"></a>Afbeeldingen ophalen via het web met de Bing afbeeldingen zoeken-API

Wanneer u de Bing afbeeldingen zoeken REST-API gebruikt, kunt u installatiekopieën kunt krijgen via het web die gerelateerd zijn aan de zoekterm door de volgende GET-aanvraag te verzenden:

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

## <a name="get-images-from-a-specific-web-domain"></a>Afbeeldingen ophalen uit een specifieke web-domein

Als u afbeeldingen uit een bepaald domein wilt opvragen, gebruikt de query-operator [site:](https://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

> [!NOTE]
> Antwoorden op query's met behulp van de `site:` operator advies inwinnen bij inhoud voor volwassenen ongeacht de [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) instelling. Gebruik alleen `site:` bent u op de hoogte van de inhoud van het domein.

Het volgende voorbeeld laat zien hoe u kleine afbeeldingen opvraagt van ContosoSailing.com die Bing de afgelopen week heeft gedetecteerd.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="filter-images"></a>Installatiekopieën filteren

 De afbeeldingen zoeken-API retourneert standaard alle installatiekopieën die relevant voor de query zijn. Als u wilt filteren van de installatiekopieën die Bing retourneert (bijvoorbeeld om terug te keren alleen afbeeldingen met een transparante achtergrond of een specifieke grootte), gebruikt u de volgende queryparameters:

* [aspect](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#aspect): filteren van installatiekopieën op de hoogte-breedteverhouding (bijvoorbeeld, standard of breed schermafbeeldingen).
* [kleur](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#color), installatiekopieën van de overheersende kleur of zwart-wit filteren.
* [nieuwheid](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#freshness): installatiekopieën filteren op leeftijd (bijvoorbeeld afbeeldingen door Bing gedetecteerd in de afgelopen week).
* [hoogte](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#height), [breedte](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#width)-installatiekopieën met breedte en hoogte filteren.
* [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagecontent): installatiekopieën filteren op de inhoud (bijvoorbeeld afbeeldingen die alleen van een persoon face weergeven).
* [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype)-installatiekopieën op type (bijvoorbeeld, illustraties, GIF-animaties of transparante achtergrond) filteren.
* [licentie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license): installatiekopieën filteren op het type licentie dat is gekoppeld aan de site.
* [grootte](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#size): Filter installatiekopieën op grootte, zoals de kleine afbeeldingen maximaal 200 x 200 pixels.

Als u afbeeldingen uit een bepaald domein wilt opvragen, gebruikt de query-operator [site:](https://msdn.microsoft.com/library/ff795613.aspx).

 > [!NOTE]
 > Antwoorden op query's met behulp van de `site:` operator advies inwinnen bij inhoud voor volwassenen ongeacht de [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch) instelling. Gebruik alleen `site:` bent u op de hoogte van de inhoud van het domein.

Het volgende voorbeeld ziet hoe u kleine afbeeldingen van ContosoSailing.com die Bing gedetecteerd in de afgelopen week.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="bing-image-search-response-format"></a>Bing afbeeldingen zoeken-antwoordindeling

Het antwoordbericht van Bing bevat een [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) antwoord bevat een lijst met installatiekopieën die Cognitive Services blijkt dat relevant is voor de query. Elke [installatiekopie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) object in de lijst bevat de volgende informatie over de installatiekopie: de URL, de grootte, de dimensies, de coderingsindeling, een URL naar een miniatuur van de installatiekopie en dimensies van de miniatuur.

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

Wanneer u de Bing Afbeeldingen zoeken-API aanroept, retourneert Bing een lijst met resultaten. De lijst is een subset van het totale aantal resultaten die relevant zijn voor de query. Het veld `totalEstimatedMatches` in de respons bevat een schatting van het aantal afbeeldingen dat beschikbaar is om weer te geven. Zie voor meer informatie over het bladeren door de rest van de installatiekopieën [installatiekopieën van het wisselbestand](../paging-images.md).

## <a name="next-steps"></a>Volgende stappen

Als u de Bing afbeeldingen zoeken-API voordat u dit nog niet hebt geprobeerd, probeert u een [snelstartgids](../quickstarts/csharp.md). Als u ingewikkeldere zoekt, probeert u de zelfstudie voor het maken van een [één pagina web-app](../tutorial-bing-image-search-single-page-app.md).
