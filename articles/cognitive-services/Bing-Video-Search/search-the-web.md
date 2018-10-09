---
title: Wat is Bing Video's zoeken?
titlesuffix: Azure Cognitive Services
description: Lees hoe u de Bing Video's zoeken-API gebruikt om op internet te zoeken naar video's.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: overview
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: cf37db9bffa8b2a54a6327c29ec806e0eefc8c91
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225409"
---
# <a name="what-is-bing-video-search"></a>Wat is Bing Video's zoeken?

De Bing Video's zoeken-API biedt in grote lijnen dezelfde ervaring als [Bing Video's](https://www.bing.com/videos). Met de Bing Video's zoeken-API kunt u een zoekopdracht versturen naar Bing, waarna u een lijst met relevante video's retour krijgt.

Als u een pagina wilt samenstellen met zoekresultaten die alleen uit video's bestaan om video's te vinden die relevant zijn voor de zoekopdracht van de gebruiker, roept u deze API aan in plaats van de meer algemene [Bing Webzoekopdrachten-API](../bing-web-search/search-the-web.md). Als u video's en andere typen inhoud wilt vinden, zoals webpagina's, nieuws en afbeeldingen, is de Bing Webzoekopdrachten-API de aangewezen methode.

## <a name="suggesting--using-search-terms"></a>Zoektermen voorstellen en gebruiken

Als u een zoekvak aanbiedt waarin de gebruiker een zoekterm invoert, kunt u de [Automatische suggestie-API voor Bing](../bing-autosuggest/get-suggested-search-terms.md) gebruiken om de ervaring te verbeteren. De API retourneert voorgestelde queryreeksen op basis van gedeeltelijke zoektermen terwijl de gebruiker typt.

Nadat de gebruiker de zoekterm heeft ingevoerd, past u URL-codering toe op de term voordat u de queryparameter [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query) instelt. Als de gebruiker bijvoorbeeld *sailing dinghies* invoert, stelt u `q` in op `sailing+dinghies` of `sailing%20dinghies`.

## <a name="getting-videos"></a>Video's ophalen

Verstuur de volgende GET-aanvraag om video's op te halen van internet die gerelateerd zijn aan de zoekterm van de gebruiker:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-Search-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Alle aanvragen moeten worden verzonden vanaf een server.

Als dit de eerste keer is van u een van de Bing-API's aanroept, moet u de header met de client-id weglaten. Voeg de client-id alleen toe als u eerder een Bing-API hebt aangeroepen en Bing een client-id heeft geretourneerd voor de combinatie van gebruiker en apparaat.

Als u video's uit een bepaald domein wilt opvragen, gebruikt u de query-operator [site:](http://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

De respons bevat een antwoord [Videos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) dat bestaat uit een lijst met video's die volgens Bing relevant zijn voor de query. Elk [Video](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video)-object in de lijst bevat de URL van de video, de duur, de afmetingen en de coderingsindeling, plus nog verschillende andere kenmerken. Het Video-object bevat ook de URL van een miniatuur van de video en de afmetingen van de miniatuur.

```json
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545...",
    "totalEstimatedMatches" : 1000,
    "value" : [
        {
            "name" : "How to sail - What to Wear for Dinghy Sailing",
            "description" : "An informative video on what to wear when...",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7...",
            "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?id=OVP.DYW...",
            "datePublished" : "2014-03-04T11:51:53",
            "publisher" : [
                {
                    "name" : "Fabrikam"
                }
            ],
            "creator" : 
            {
                "name" : "Marcus Appel"
            },
            "contentUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjZ--g",
            "hostPageUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D569...",
            "encodingFormat" : "h264",
            "hostPageDisplayUrl" : "https:\/\/www.fabrikam.com\/watch?v=vzmPjZ--g",
            "width" : 1280,
            "height" : 720,
            "duration" : "PT2M47S",
            "motionThumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?id=OM.Y62...",
            "embedHtml" : "<iframe width=\"1280\" height=\"720\" src=\"https:...><\/iframe>",
            "allowHttpsEmbed" : true,
            "viewCount" : 8743,
            "thumbnail" : 
            {
                "width" : 300,
                "height" : 168
            },
            "videoId" : "6DB795E11A6E3CBAAD636DB795E113CBAAD63",
            "allowMobileEmbed" : true,
            "isSuperfresh" : false
        },
        ...
    ],
    "queryExpansions" : [...],
    "nextOffsetAddCount" : 0,
    "pivotSuggestions" : [...]
}
```

U kunt een collage weergeven met alle videominiaturen of een subset van de miniaturen. Als u kiest voor een subset, geef de gebruiker dan de gelegenheid om ook de overige video's weer te geven. U moet de video's weergeven in de volgorde waarin ze in het antwoord zijn opgenomen. Zie [Formaat van miniaturen wijzigen en miniaturen bijsnijden](./resize-and-crop-thumbnails.md) voor informatie over het aanpassen van het formaat van de miniatuur. 

Als de gebruiker de muisaanwijzer op een miniatuur plaatst, kunt u [motionThumbnailUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-motionthumbnailurl) gebruiken om een miniatuurversie van de video af te spelen. Vergeet niet om het kenmerk motionThumbnailUrl in te stellen wanneer u de miniatuur weergeeft.

<!-- Removing until the images can be sanitized.
![Motion thumbnail of a video](../bing-web-search/media/cognitive-services-bing-web-api/bing-web-video-motion-thumbnail.PNG)
-->

Als de gebruiker op de miniatuur klikt, zijn dit de opties voor het bekijken van de video:

- Gebruik [hostPageUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-hostpageurl) om de video weer te geven op de hostwebsite (bijvoorbeeld YouTube)
- Gebruik [webSearchUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-websearchurl) om de video weer te geven in de videobrowser van Bing
- Gebruik [embedHtml](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-embedhtml) om de video in te sluiten in uw eigen ervaring 

Vergeet niet om de kenmerken publisher en creator op te geven bij het afspelen van de video.

Zie [Inzicht verkrijgen over een video](./video-insights.md) voor meer informatie over het gebruiken van [videoId](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video-videoid) om inzichten te verkrijgen over de video.

## <a name="filtering-videos"></a>Video's filteren

De Bing Video's zoeken-API retourneert standaard alle video's die relevant zijn voor de query. Als u alleen gratis video's wilt zien of video's die korter zijn dan vijf minuten, gebruikt u de volgende filterqueryparameters:

- [pricing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#pricing)&mdash;Video's filteren op prijs (bijvoorbeeld video's die gratis zijn of waarvoor u hebt betaald)
- [resolution](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#resolution)&mdash;Video's filteren op resolutie (bijvoorbeeld video's met een resolutie van 720 p of hoger)
- [videoLength](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videolength)&mdash;Video's filteren op videolengte (bijvoorbeeld video's die minder dan vijf minuten lang zijn)
- [freshness](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#freshness)&mdash;Video's filteren op leeftijd (bijvoorbeeld video's die in de afgelopen week door Bing zijn gedetecteerd)

Als u video's uit een bepaald domein wilt opvragen, gebruikt u de query-operator [site:](http://msdn.microsoft.com/library/ff795613.aspx) in de queryreeks.

> [!NOTE]
> Als u de operator `site:` gebruikt, is het afhankelijk van de query mogelijk dat het antwoord inhoud voor volwassenen bevat, ongeacht de instelling van [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#safesearch). Gebruik `site:` alleen als u zich bewust bent van de inhoud op de site en uw scenario de mogelijkheid van inhoud voor volwassenen ondersteunt.

Het volgende voorbeeld laat zien hoe u gratis video's van ContosoSailing.com ophaalt die een resolutie van 720p of hoger hebben en die Bing in de afgelopen maand heeft gedetecteerd.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies+site:contososailing.com&pricing=free&freshness=month&resolution=720p&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

## <a name="expanding-the-query"></a>Query uitbreiden

Als Bing de query kan uitbreiden om de oorspronkelijke zoekopdracht te beperken, bevat het object [Videos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) het veld `queryExpansions`. Als de query bijvoorbeeld *Cleaning Gutters* was, zijn dit voorbeelden van uitgebreide query's: Gutter Cleaning **Tools**, Cleaning Gutters **From the Ground**, Gutter Cleaning **Machine** en **Easy** Gutter Cleaning.

In het volgende voorbeeld ziet u de uitgebreide query's voor *Cleaning Gutters*.

```json
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FBC5...",
    "totalEstimatedMatches" : 1000,
    "value" : [...],
    "nextOffsetAddCount" : 4,
    "queryExpansions" : [
        {
            "text" : "Gutter Cleaning Tools",
            "displayText" : "Tools",
            "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FB....",
            "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v5...",
            "thumbnail" : {
                "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Gutter..."
            }
        },
        ...
    ]
    "pivotSuggestions" : [...],
}
```

Het veld `queryExpansions` bevat een lijst met [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query_obj)-objecten. Het veld `text` bevat de uitgebreide query en het veld `displayText` bevat de term na de uitbreiding. U kunt de velden text en thumbnail gebruiken om de uitgebreide queryreeksen weer te geven aan de gebruiker als de uitgebreide queryreeks is waar ze in feite naar op zoek zijn. Maak de miniatuur en tekst klikbaar met de URL `webSearchUrl` of `searchLink`. Gebruik `webSearchUrl` om de gebruiker om te leiden naar de zoekresultaten of `searchLink` als u een eigen resultatenpagina hebt.

## <a name="pivoting-the-query"></a>De query draaien

Als Bing de oorspronkelijke zoekopdracht kan segmenteren, bevat het object [Videos](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) het veld `pivotSuggestions`. Als de oorspronkelijke query bijvoorbeeld *Cleaning Gutters* was, kan Bing de query segmenteren in *Cleaning* en *Gutters*.

In het volgende voorbeeld ziet u de draaisuggesties voor *Cleaning Gutters*.

```json
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FBC...",
    "totalEstimatedMatches" : 1000,
    "value" : [...],
    "nextOffsetAddCount" : 0,
    "queryExpansions" : [...],
    "pivotSuggestions" : [
        {
            "pivot" : "cleaning",
            "suggestions" : [
                {
                    "text" : "Gutter Repair",
                    "displayText" : "Repair",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v5\/videos...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=Gutter..."
                    }
                },
                ...
            ]
        },
        {
            "pivot" : "gutters",
            "suggestions" : [
                {
                    "text" : "Window Cleaning",
                    "displayText" : "Window",
                    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=B52FBC59...",
                    "searchLink" : "https:\/\/api.cognitive.microsoft.com\/api\/v5...",
                    "thumbnail" : {
                        "thumbnailUrl" : "https:\/\/tse2.mm.bing.net\/th?q=Window..."
                    }
                },
                ...
            ]
        }
    ]
}
```

Voor elk draaipunt bevat de respons een lijst met [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query_obj)-objecten met voorgestelde query's. Het veld `text` bevat de voorgestelde query en het veld `displayText` de term die het draaipunt in de oorspronkelijke query vervangt, bijvoorbeeld Window Cleaning.

U kunt de velden `text` en `thumbnail` gebruiken om de uitgebreide queryreeksen weer te geven aan de gebruiker als de uitgebreide queryreeks is waar ze in feite naar op zoek zijn. Maak de miniatuur en tekst klikbaar met de URL `webSearchUrl` of `searchLink`. Gebruik `webSearchUrl` om de gebruiker om te leiden naar de zoekresultaten of `searchLink` als u een eigen resultatenpagina hebt.

## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Volgende stappen

Zie [Uw eerste aanvraag maken](./quick-start.md) om snel aan de slag te gaan met uw eerste aanvraag.

Zie [Abonnementssleutels verkrijgen](https://azure.microsoft.com/try/cognitive-services/?api=bing-video-search-api) voor meer informatie over het verkrijgen van een abonnementssleutel.

Lees voor meer informatie de [naslaghandleiding over de Bing Video's zoeken-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference). De handleiding bevat de lijst met eindpunten, headers en queryparameters die u nodig hebt om zoekresultaten op te vragen. Daarnaast vindt u hier definities van de responsobjecten. 

Zie [Automatische suggestie-API voor Bing](../bing-autosuggest/get-suggested-search-terms.md) om de gebruikerservaring van het zoekvak te verbeteren. Als de gebruiker een zoekterm invoert, kunt u deze API aanroepen om relevante querytermen weer te geven die door anderen zijn gebruikt.

Lees [Gebruiks- en weergavevereisten voor Bing](./useanddisplayrequirements.md) om er zeker van te zijn dat u alle regels voor het gebruik van de zoekresultaten volgt.

Wanneer u de Bing Video's zoeken-API aanroept, retourneert Bing een lijst met resultaten. De lijst is een subset van het totale aantal resultaten die relevant zijn voor de query. Het veld `totalEstimatedMatches` in de respons bevat een schatting van het aantal video's dat beschikbaar is om weer te geven. Zie [Video's per pagina weergeven](./paging-videos.md) voor meer informatie over het paginagewijs weergeven van de resterende video's.

Zie [Inzicht verkrijgen over een video](./video-insights.md) voor meer informatie over het verkrijgen van inzichten over een video.

Zie [Trending video's](./trending-videos.md) voor meer informatie over het ophalen van trending video's.