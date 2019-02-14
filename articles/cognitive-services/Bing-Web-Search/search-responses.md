---
title: Bing webzoekopdrachten-API-reactie structuur en -antwoordsessie-typen
titleSuffix: Azure Cognitive Services
description: Meer informatie over de antwoord-typen en -antwoorden die worden gebruikt door de Bing webzoekopdrachten-API.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: aahi
ms.custom: seodec2018
ms.openlocfilehash: 07fb655af25fe590effcb885e7b366346724b50a
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56232889"
---
# <a name="bing-web-search-api-response-structure-and-answer-types"></a>Bing webzoekopdrachten-API-reactie structuur en -antwoordsessie-typen  

Als u Bing webzoekopdrachten een zoekaanvraag verzendt, wordt een [ `SearchResponse` ](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse) object in de hoofdtekst van het antwoord. Het object bevat een veld voor elk antwoord dat Bing bepaald relevant zijn voor de query is. In dit voorbeeld wordt een antwoordobject als Bing geretourneerd alle antwoorden op:

```json
{
    "_type": "SearchResponse",
    "queryContext": {...},
    "webPages": {...},
    "images": {...},
    "relatedSearches": {...},
    "videos": {...},
    "news": {...},
    "spellSuggestion": {...},
    "computation": {...},
    "timeZone": {...},
    "rankingResponse": {...}
}, ...
```

Bing webzoekopdrachten retourneert doorgaans een subset van de antwoorden. Bijvoorbeeld, als de zoekterm is *Zeilsloepen*, het antwoord advies inwinnen `webPages`, `images`, en `rankingResponse`. Tenzij u bekend bent met [responseFilter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#responsefilter) wilt wegfilteren webpagina's, bevat het antwoord altijd de `webpages` en `rankingResponse` antwoorden.

## <a name="webpages-answer"></a>Antwoord van de webpagina 's

De [webpagina's](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webanswer) antwoord bevat een lijst met koppelingen naar webpagina's met Bing webzoekopdrachten bepaald zijn relevant zijn voor de query. Elke [webpagina](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webpage) in de lijst bevat: naam van de pagina, url, URL, een korte beschrijving van de inhoud en de datum Bing gevonden de inhoud wordt weergegeven.

```json
{
    "id": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/#WebPages.0",
    "name": "Dinghy sailing",
    "url": "https:\/\/www.bing.com\/cr?IG=3A43CA5...",
    "displayUrl": "https:\/\/en.contoso.com\/wiki\/Dinghy_sailing",
    "snippet": "Dinghy sailing is the activity of sailing small boats...",
    "dateLastCrawled": "2017-04-05T16:25:00"
}, ...
```

Gebruik `name` en `url` te maken van een hyperlink waarop de gebruiker naar de webpagina leidt.

<!-- Remove until this can be replaced with a sanitized version.
The following shows an example of how you might display the webpage in a search results page.

![Rendered webpage example](./media/cognitive-services-bing-web-api/bing-rendered-webpage-example.PNG)
-->

## <a name="images-answer"></a>Installatiekopieën van antwoord

De [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) antwoord bevat een lijst met installatiekopieën die Bing beschouwd zijn relevant zijn voor de query. Elke [installatiekopie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) in de lijst bevat de URL van de afbeelding, de grootte, de dimensies en de coderingsindeling. Het Image-object bevat ook de URL van een miniatuur van de afbeelding en de afmetingen van de miniatuur.

```json
{
    "name": "Rich Passage Sailing Dinghy",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=3A43CA5CA64...",
    "thumbnailUrl": "https:\/\/tse1.mm.bing.net\/th?id=OIP....",
    "datePublished": "2011-10-29T11:26:00",
    "contentUrl": "http:\/\/upload.contoso.com\/sailing\/...",
    "hostPageUrl": "http:\/\/www.bing.com\/cr?IG=3A43CA5CA6464....",
    "contentSize": "79239 B",
    "encodingFormat": "jpeg",
    "hostPageDisplayUrl": "http:\/\/en.contoso.com\/wiki\/File...",
    "width": 526,
    "height": 688,
    "thumbnail": {
        "width": 229,
        "height": 300
    },
    "insightsSourcesSummary": {
        "shoppingSourcesCount": 0,
        "recipeSourcesCount": 0
    }
}, ...
```

Afhankelijk van het apparaat van de gebruiker, zou u meestal een subset van de miniatuurweergaven, met een optie voor de gebruiker weer [pagina via](paging-webpages.md) de resterende afbeeldingen.

<!-- Remove until this can be replaced with a sanitized version.
![List of thumbnail images](./media/cognitive-services-bing-web-api/bing-web-image-thumbnails.PNG)
-->

U kunt de miniatuur ook uitbreiden op het moment dat de gebruiker er met de cursor naar wijst. Vergeet niet om een geschikt kenmerk toe wijzen aan de afbeelding als u dat wilt doen. Bijvoorbeeld, door te extraheren van de host van `hostPageDisplayUrl` en deze onder de afbeelding wordt weergegeven. Zie [Formaat van miniaturen wijzigen en miniaturen bijsnijden](./resize-and-crop-thumbnails.md) voor informatie over het aanpassen van het formaat van de miniatuur.

<!-- Remove until this can be replaced with a sanitized version.
![Expanded view of thumbnail image](./media/cognitive-services-bing-web-api/bing-web-image-thumbnail-expansion.PNG)
-->

Als de gebruiker klikt op de miniatuur, gebruikt u `webSearchUrl` te nemen van de gebruiker van de Bing pagina met zoekresultaten voor installatiekopieën, waarin een collage van de installatiekopieën.

Zie voor meer informatie over het antwoord van de installatiekopie en de afbeeldingen [afbeeldingen zoeken-API](../bing-image-search/search-the-web.md).

## <a name="related-searches-answer"></a>Gerelateerde zoekopdrachten antwoord

De [relatedSearches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#searchresponse-relatedsearches) antwoord bevat een lijst van de meest populaire gerelateerde query's die door andere gebruikers. Elke [query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query_obj) in de lijst met een queryreeks bevat (`text`), een query-tekenreeks met tekens markeren van treffers (`displayText`), en een URL (`webSearchUrl`) op de pagina met zoekresultaten van Bing voor deze query.

```json
{
    "text": "dinghy racing teams",
    "displayText": "dinghy racing teams",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=96C4CF214A0..."
}, ...
```

Gebruik de `displayText` querytekenreeks en de `webSearchUrl` URL naar een hyperlink maken die de gebruiker naar de Bing zoeken leidt-pagina voor de gerelateerde query resultaten. U kunt ook de `text` querytekenreeks in uw eigen query webzoekopdrachten-API en de resultaten weergeven zelf.

Voor informatie over het afhandelen van de markering markeringen in `displayText`, Zie [treffers markeren](./hit-highlighting.md).

Hieronder ziet u een voorbeeld van het gebruik van gerelateerde query's in op Bing.com.

![Voorbeeld van de verwante zoekopdrachten voor Bing](./media/cognitive-services-bing-web-api/bing-web-rendered-relatedsearches.GIF)

## <a name="videos-answer"></a>Antwoord van de video 's

De [video's](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#videos) antwoord bevat een lijst met video's die Bing beschouwd zijn relevant zijn voor de query. Elke [video](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#video) in de lijst bevat de URL van de video, de duur van de dimensies en de coderingsindeling. Het Video-object bevat ook de URL van een miniatuur van de video en de afmetingen van de miniatuur.

```json
{
    "name": "Sailing dinghy",
    "description": "Northwind Traders is a 12 foot gunter rigged...",
    "webSearchUrl": "https:\/\/www.bing.com\/cr?IG=1CAE739681D84...",
    "thumbnailUrl": "https:\/\/tse2.mm.bing.net\/th?id=OVP.wsKiL...",
    "datePublished": "2013-11-06T01:56:28",
    "publisher": [{
        "name": "Fabrikam"
    }],
    "contentUrl": "https:\/\/www.fabrikam.com\/watch?v=MrVBWZpJjX",
    "hostPageUrl": "https:\/\/www.bing.com\/cr?IG=1CAE739681D8400DB...",
    "encodingFormat": "mp4",
    "hostPageDisplayUrl": "https:\/\/www.fabrikam.com\/watch?v=MrBWZpJjXo",
    "width": 1280,
    "height": 720,
    "duration": "PT3M47S",
    "motionThumbnailUrl": "https:\/\/tse2.mm.bing.net\/th?id=OM.oa...",
    "embedHtml": "<iframe width=\"1280\" height=\"720\" src=\"http:\/\/www....><\/iframe>",
    "allowHttpsEmbed": true,
    "viewCount": 19089,
    "thumbnail": {
        "width": 300,
        "height": 168
    },
    "allowMobileEmbed": true,
    "isSuperfresh": false
}, ...
```

Afhankelijk van het apparaat van de gebruiker, zou u doorgaans een subset van de video's met een optie voor de gebruiker om weer te geven van de resterende video's weergeven. U zou een miniatuur van de video met een lengte van de video, geven een beschrijving (naam) en attribution (uitgever).

<!-- Remove until this can be replaced with a sanitized version.
![List of video thumbnails](./media/cognitive-services-bing-web-api/bing-web-video-thumbnails.PNG)
-->

Als de gebruiker de muisaanwijzer op de miniatuur kunt u `motionThumbnailUrl` een miniatuur versie van de video afspelen. Vergeet niet om het kenmerk motionThumbnailUrl in te stellen wanneer u de miniatuur weergeeft.

<!-- Remove until this can be replaced with a sanitized version.
![Motion thumbnail of a video](./media/cognitive-services-bing-web-api/bing-web-video-motion-thumbnail.PNG)
-->

Als de gebruiker op de miniatuur klikt, zijn dit de opties voor het bekijken van de video:

- Gebruik `hostPageUrl` om weer te geven van de video op de host-website (bijvoorbeeld YouTube)
- Gebruik `webSearchUrl` om weer te geven van de video in de Bing video-browser
- Gebruik `embedHtml` de video insluiten in uw eigen ervaring

Zie voor meer informatie over de video antwoord en video's [Video zoeken-API](../bing-video-search/search-the-web.md).

## <a name="news-answer"></a>Nieuwsantwoord

De [nieuws](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#news) antwoord bevat een lijst met nieuwsartikelen die Bing beschouwd zijn relevant zijn voor de query. Elk [NewsArticle](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#newsarticle) in de lijst bevat de naam en beschrijving van het artikel, plus de URL naar het artikel op de website van de host. Als in het artikel een afbeelding staat, bevat het object een miniatuur van de afbeelding.

```json
{
    "name": "WC Sailing Qualifies for America Trophy with...",
    "url": "http:\/\/www.bing.com\/cr?IG=3445EEF15DAF4FFFBF7...",
    "image": {
        "contentUrl": "http:\/\/www.contoso.com\/sports\/sail...",
        "thumbnail": {
            "contentUrl": "https:\/\/www.bing.com\/th?id=ON.1...",
            "width": 400,
            "height": 272
        }
    },
    "description": "The WC sailing team qualified for a...",
    "provider": [{
        "_type": "Organization",
        "name": "contoso.com"
    }],
    "datePublished": "2017-04-16T21:56:00"
}, ...
```

Afhankelijk van het apparaat van de gebruiker, zou u een subset van de nieuwsartikelen met een optie voor de gebruiker om de resterende artikelen weer te geven weergeven. Gebruik `name` en `url` om een hyperlink te maken waarmee de gebruiker naar het nieuwsartikel op de site van de host gaat. Als het artikel een afbeelding bevat, moet u de installatiekopie geklikt met `url`. Vergeet niet om met `provider` bronvermelding toe te passen voor het artikel.

<!-- Remove until this can be replaced with a sanitized version.
The following shows an example of how you might display articles in a search results page.

![List of news articles](./media/cognitive-services-bing-web-api/bing-web-news-list.PNG)
-->

Zie voor meer informatie over de nieuwsantwoord en nieuwsartikelen [nieuws zoeken-API](../bing-news-search/search-the-web.md).

## <a name="computation-answer"></a>Antwoord berekenen

Als de gebruiker een rekenkundige expressie of een query van de conversie unit voert, het antwoord kan bevatten een [berekening](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#computation) antwoord. De `computation` antwoord bevat de genormaliseerde expressie en het resultaat.

Een eenheid conversie-query is een query die één eenheid naar een andere converteert. Bijvoorbeeld, *hoeveel meter in 10 meter?* of *hoeveel eetlepels in een 1/4-cup?*

Het volgende bevat de `computation` beantwoorden voor *hoeveel meter in 10 meter?*

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
    "expression": "10 meters",
    "value": "32.808399 feet"
}, ...
```

Hieronder ziet u voorbeelden van wiskundige query's en de bijbehorende `computation` antwoorden.

```
Query: (5+3)(10/2)+8
Encoded query: %285%2B3%29%2810%2F2%29%2B8
```

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
    "expression": "((5+3)*(10\/2))+8",
    "value": "48"
}
```

```
Query: sqrt(4^2+8^2)
Encoded query: sqrt%284^2%2B8^2%29
```

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
    "expression": "sqrt((4^2)+(8^2))",
    "value": "8.94427191"
}
```

```
Query: 30 6/8 - 18 8/16
Encoded query: 30%206%2F8%20-%2018%208%2F16
```

```json
"computation": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#WolframAlpha",
    "expression": "30 6\/8-18 8\/16",
    "value": "12.25"
}
```

```
Query: 8^2+11^2-2*8*11*cos(37)
Encoded query: 8^2%2B11^2-2*8*11*cos%2837%29
```

```json
"computation": {
        "id": "https:\/\/www.bing.com\/api\/v7\/#Computation",
        "expression": "(8^2)+(11^2)-(2*8*11*cos(37))",
        "value": "44.4401502"
}
```

Een wiskundige expressie kan de volgende symbolen bevatten:

|Symbool|Description|
|------------|-----------------|
|+|Aanvulling|
|-|Aftrekken|
|/|deling|
|*|Vermenigvuldigen|
|^|Energiebeheer|
|!|Faculteit|
|.|Decimal|
|()|Prioriteit groeperen|
|[]|Function|

Een rekenkundige expressie mag de volgende constanten toe:

|Symbool|Description|
|------------|-----------------|
|PI|3.14159...|
|Mate|Mate|
|Ik|Denkbeeldige getal|
|e|e 2.71828...|
|GoldenRatio|Golden verhouding, 1.61803...|

Een rekenkundige expressie mag de volgende functies:

|Symbool|Description|
|------------|-----------------|
|Sorteren|Vierkantswortel|
|SIN [x], Cos [x], Tan [x]<br />Csc [x], [x] Sec Cot [x]|Trigonometrische functies (met argumenten in radialen)|
|ArcSin[x], ArcCos[x], ArcTan[x]<br />ArcCsc[x], ArcSec[x], ArcCot[x]|Inverse trigonometrische functies (waardoor de resultaten in radialen)|
|Exp[x], E^x|Exponentiële functie|
|Log[x]|Natuurlijk logaritme|
|SINH [x], [x] Cosh Tanh [x]<br />Csch [x], [x] Sech Coth [x]|Hyperbolische functies|
|ArcSinh[x], ArcCosh[x], ArcTanh[x]<br />ArcCsch[x], ArcSech[x], ArcCoth[x]|Inverse hyperbolische functies|

Wiskundige expressies die variabelen (voor bijvoorbeeld 4 x + 6 = 18, waarbij x staat voor de variabele) bevatten, worden niet ondersteund.

## <a name="timezone-answer"></a>Tijdzone-antwoord

Als de gebruiker een query voor datum of tijd voert, het antwoord kan bevatten een [tijdzone](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#timezone) antwoord. Dit antwoord ondersteunt impliciet of expliciet query's. Een impliciete query, zoals *welk moment is het?*, retourneert de lokale tijd op basis van de locatie van de gebruiker. Een explicit-query, zoals *welk moment is het in Seattle?*, retourneert de lokale tijd voor Seattle, Washington.

De `timeZone` antwoord bevat de naam van de locatie, de huidige UTC-datum en tijd op de opgegeven locatie en de UTC-offset. Als de grens van de locatie in meerdere tijdzones is, wordt het antwoord bevat de huidige UTC-datum en tijd van alle tijdzones binnen de grens. Omdat de status Florida binnen twee tijdzones valt, bevat het antwoord, bijvoorbeeld de lokale datum en tijd van beide tijdzones.  

Als de query vraagt om de tijd van een staat of land/regio, Bing bepaalt de primaire plaats binnen een geografische grens van de locatie en retourneert deze in de `primaryCityTime` veld. Als de grens meerdere tijdzones bevat, de resterende tijdzones worden geretourneerd in de `otherCityTimes` veld.

Het volgende voorbeeld van bevat query's dat rendement de `timeZone` antwoord.

```
Query: What time is it?

"timeZone": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
    "primaryCityTime": {
        "location": "Redmond, Washington, United States",
        "time": "2015-10-27T08:38:12.1189231Z",
        "utcOffset": "UTC-7"
    }
}

Query: What time is it in the Pacific time zone?

"timeZone": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
    "primaryCityTime": {
        "location": "Pacific Time Zone",
        "time": "2015-10-23T12:33:19.0728146Z",
        "utcOffset": "UTC-7"
    }
}

Query: Time in Florida?

"timeZone": {
        "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
        "primaryCityTime": {
            "location": "Tallahassee, Florida, United States",
            "time": "2015-10-23T13:04:56.6774389Z",
            "utcOffset": "UTC-4"
        },
        "otherCityTimes": [{
            "location": "Pensacola",
            "time": "2015-10-23T12:04:56.6664294Z",
            "utcOffset": "UTC-5"
        }]
}

Query: What time is it in the U.S.

"timeZone": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#TimeZone",
    "primaryCityTime": {
        "location": "Washington, D.C., United States",
        "time": "2015-10-23T15:27:59.8892745Z",
        "utcOffset": "UTC-4"
    },
    "otherCityTimes": [{
        "location": "Honolulu",
        "time": "2015-10-23T09:27:59.8892745Z",
        "utcOffset": "UTC-10"
    },
    {
        "location": "Anchorage",
        "time": "2015-10-23T11:27:59.8892745Z",
        "utcOffset": "UTC-8"
    },
    {
        "location": "Phoenix",
        "time": "2015-10-23T12:27:59.8892745Z",
        "utcOffset": "UTC-7"
    },
    {
        "location": "Los Angeles",
        "time": "2015-10-23T12:27:59.8942788Z",
        "utcOffset": "UTC-7"
    },
    {
        "location": "Denver",
        "time": "2015-10-23T13:27:59.8812681Z",
        "utcOffset": "UTC-6"
    },
    {
        "location": "Chicago",
        "time": "2015-10-23T14:27:59.8892745Z",
        "utcOffset": "UTC-5"
    }]
}
```

## <a name="spellsuggestion-answer"></a>SpellSuggestion antwoord

Als u Bing bepaalt dat de gebruiker kan zijn bedoeld om te zoeken naar een andere, het antwoord bevat een [SpellSuggestions](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#spellsuggestions) object. Bijvoorbeeld, als de gebruiker wordt gezocht naar *carlos pen*, Bing besluiten dat de gebruiker waarschijnlijk bedoeld om te zoeken naar Carlos Pena in plaats daarvan (op basis van afgelopen zoekopdrachten door anderen van *carlos pen*). Hieronder ziet u een voorbeeld van een antwoord spellingcontrole.

```json
"spellSuggestions": {
    "id": "https:\/\/www.bing.com\/api\/v7\/#SpellSuggestions",
    "value": [{
        "text": "carlos pena",
        "displayText": "carlos pena"
    }]
}, ...
```

Hieronder ziet u hoe Bing maakt gebruik van het voorstel spelling.

![Bing spellingcontrole suggestie voorbeeld](./media/cognitive-services-bing-web-api/bing-web-spellingsuggestion.GIF)  

## <a name="response-headers"></a>Antwoordheaders

Antwoorden van de Bing webzoekopdrachten-API kunnen de volgende headers bevatten:

|||
|-|-|
|`X-MSEdge-ClientID`|De unieke ID die Bing is toegewezen aan de gebruiker|
|`BingAPIs-Market`|De markt die is gebruikt om te voldoen aan de aanvraag|
|`BingAPIs-TraceId`|De vermelding op de server-API voor Bing voor deze aanvraag (voor ondersteuning)|

Dit is vooral belangrijk voor het behouden van de client-ID en retourneer het met de volgende aanvragen. Als u dit doet, wordt de zoekopdracht gebruikt voorbij context in volgorde van zoekresultaten en bieden ook een consistente gebruikerservaring.

Echter, wanneer u de Bing webzoekopdrachten-API vanuit JavaScript aanroepen, van uw browser ingebouwde beveiligingsfuncties (CORS) kunnen voorkomen dat u toegang tot de waarden van deze headers.

Om toegang te krijgen tot de headers, kunt u de Bing webzoekopdrachten-API-aanvraag via een proxy CORS. Het antwoord van een dergelijke proxy heeft een `Access-Control-Expose-Headers`-header waardoor antwoordheaders worden opgenomen in de whitelist en beschikbaar gemaakt voor JavaScript.

Het is eenvoudig te installeren van een CORS-proxy om toe te staan onze [zelfstudie app](tutorial-bing-web-search-single-page-app.md) voor toegang tot de optionele client-headers. Als u [Node.js](https://nodejs.org/en/download/) nog niet hebt, moet u dit eerst installeren. Voer de volgende opdracht achter de opdrachtprompt.

    npm install -g cors-proxy-server

Vervolgens de Bing webzoekopdrachten-API-eindpunt in de HTML-bestand te wijzigen:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/search

Start ten slotte de CORS-proxy met de volgende opdracht:

    cors-proxy-server

Laat het opdrachtvenster geopend terwijl u de zelfstudie-app gebruikt. Als u het venster sluit, wordt de proxy gestopt. In de uitbreidbare sectie met HTTP-headers onder de zoekresultaten ziet u nu (onder andere) de `X-MSEdge-ClientID`-header en kunt u controleren of deze voor elke aanvraag gelijk is.

## <a name="response-headers-in-production"></a>Antwoordheaders in productie

De CORS-proxy-benadering beschreven in het vorige antwoord is geschikt voor ontwikkeling, testen en leren.

In een productieomgeving, moet u een script op de server in hetzelfde domein als de webpagina die gebruikmaakt van de Bing webzoekopdrachten-API te hosten. Met dit script moet API-aanroepen op verzoek van de webpagina JavaScript en alle resultaten, met inbegrip van headers, terug naar de client. Aangezien de twee resources (pagina en script) een oorsprong delen, CORS niet wordt gebruikt en de speciale headers zijn toegankelijk voor de JavaScript op de webpagina wordt weergegeven.

Deze benadering ook voorkomt dat uw API-sleutel blootstelling aan het publiek, omdat alleen het script op de server nodig heeft. Het script kunt u een andere methode gebruiken om te controleren of dat de aanvraag is geautoriseerd.

Hieronder ziet u hoe Bing maakt gebruik van het voorstel spelling.

![Bing spellingcontrole suggestie voorbeeld](./media/cognitive-services-bing-web-api/bing-web-spellingsuggestion.GIF)  

## <a name="next-steps"></a>Volgende stappen  

* Beoordeling [aanvraagbeperking](throttling-requests.md) documentatie.  

## <a name="see-also"></a>Zie ook  

* [Bing webzoekopdrachten-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
