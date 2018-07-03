---
title: Wat is Bing Afbeeldingen zoeken? | Microsoft Docs
description: Lees hoe u met de Bing Afbeeldingen zoeken-API op internet kunt zoeken naar afbeeldingen.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 1446AD8B-A685-4F5F-B4AA-74C8E9A40BE9
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: overview
ms.date: 10/11/2017
ms.author: scottwhi
ms.openlocfilehash: 457707065059b5cb83c9d2b81f5d073cf1c89b84
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36336516"
---
# <a name="what-is-bing-image-search"></a>Wat is Bing Afbeeldingen zoeken?

De Bing Afbeeldingen zoeken-API zoeken biedt een ervaring die vergelijkbaar is met [Bing Afbeeldingen](https://www.bing.com/images), namelijk dat zoekopdrachten van gebruikers kunnen worden verzonden naar Bing waarna er een lijst met relevante afbeeldingen wordt geretourneerd.

Als u een pagina samenstelt met zoekresultaten die alleen uit afbeeldingen bestaan om afbeeldingen te vinden die relevant zijn voor de zoekopdracht van de gebruiker, roept u deze API aan in plaats van de meer algemene [Bing Webzoekopdrachten-API](../bing-web-search/search-the-web.md). Als u afbeeldingen en andere typen inhoud wilt vinden, zoals webpagina's, nieuws en video's, is de Bing Webzoekopdrachten-API de aangewezen methode.

## <a name="suggesting--using-search-terms"></a>Zoektermen voorstellen en gebruiken

Als u een zoekvak aanbiedt waarin de gebruiker een zoekterm invoert, kunt u de [Automatische suggestie-API voor Bing](../bing-autosuggest/get-suggested-search-terms.md) gebruiken om de ervaring te verbeteren. De API retourneert voorgestelde queryreeksen op basis van gedeeltelijke zoektermen terwijl de gebruiker typt.

Nadat de gebruiker de zoekterm heeft ingevoerd, past u URL-codering toe op de term voordat u de queryparameter [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query) instelt. Als de gebruiker bijvoorbeeld *sailing dinghies* invoert, stelt u `q` in op `sailing+dinghies` of `sailing%20dinghies`.

## <a name="getting-images"></a>Afbeeldingen ophalen

Verstuur de volgende GET-aanvraag om afbeeldingen op te halen van internet die gerelateerd zijn aan de zoekterm van de gebruiker:

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Alle aanvragen moeten worden verzonden vanaf een server. U kunt geen aanroepen verzenden vanuit een client.

Als dit de eerste keer is van u een van de Bing-API's aanroept, moet u de header met de client-id weglaten. Voeg de client-id alleen toe als u eerder een Bing-API hebt aangeroepen en Bing een client-id heeft geretourneerd voor de combinatie van gebruiker en apparaat.

Als u afbeeldingen uit een bepaald domein wilt opvragen, gebruikt de query-operator [site:](http://msdn.microsoft.com/library/ff795613.aspx).

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&mkt=en-us HTTP/1.1
```

De respons bevat een antwoord [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) dat bestaat uit een lijst met afbeeldingen die volgens Bing relevant zijn voor de query. Elk object [Image](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image) in de lijst bevat de URL van de afbeelding, de grootte, de afmetingen en de coderingsindeling. Het Image-object bevat ook de URL van een miniatuur van de afbeelding en de afmetingen van de miniatuur.

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

U kunt een collage weergeven van alle miniaturen van de afbeeldingen of een subset van de miniaturen. Als u een subset weergeeft, geef de gebruiker dan de gelegenheid om ook de overige afbeeldingen weer te geven. U moet de afbeeldingen weergeven in de volgorde waarin ze in de respons zijn opgenomen.

U kunt de miniatuur ook uitbreiden op het moment dat de gebruiker er met de cursor naar wijst. Vergeet niet om een geschikt kenmerk toe wijzen aan de afbeelding als u dat wilt doen. U kunt bijvoorbeeld de host van [hostPageDisplayUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-hostpagedisplayurl) extraheren en deze weergeven onder de afbeelding. Zie [Formaat van miniaturen wijzigen en miniaturen bijsnijden](./resize-and-crop-thumbnails.md) voor informatie over het aanpassen van het formaat van de miniatuur.

<!-- Removing image until we can replace it with a sanatized version.
![Expanded view of thumbnail image](../bing-web-search/media/cognitive-services-bing-web-api/bing-web-image-thumbnail-expansion.PNG)
-->

Als de gebruiker op de miniatuur klikt, kunt u [contentUrl](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-contenturl) gebruiken om de afbeelding op volledige grootte weer te geven aan de gebruiker. Vergeet niet om een geschikt kenmerk toe te wijzen aan de afbeelding.

Als `shoppingSourcesCount` of `recipeSourcesCount` groter is dan nul, voeg dan een visueel element, zoals een winkelwagen, toe aan de miniatuur om aan te geven dat er artikelen of recepten bestaan voor het item in de afbeelding.

<!-- this is a sanitized version but we're removing all images for now until everything is sanitized.
![Shopping sources badge](./media/cognitive-services-bing-images-api/bing-images-shopping-source.PNG)
-->

Als u inzichten over de afbeelding wilt opvragen, zoals webpagina's die de afbeelding bevatten of personen die zijn herkend in de afbeelding, gebruikt u [imageInsightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#image-imageinsightstoken). Zie voor meer informatie [Inzichten over afbeeldingen](./image-insights.md).

## <a name="filtering-images"></a>Afbeeldingen filteren

 De Bing Afbeeldingen zoeken-API retourneert standaard alle afbeeldingen die relevant zijn voor de query. Maar als u alleen afbeeldingen met een doorzichtige achtergrond of afbeeldingen met een bepaalde grootte wilt weergeven, gebruikt u de volgende queryparameters om de afbeeldingen te filteren die Bing retourneert.  

- [aspect](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#aspect): filter afbeeldingen op hoogte-breedteverhouding (bijvoorbeeld standaardafbeeldingen of afbeeldingen voor een breed scherm)
- [color](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#color): filter afbeeldingen op overheersende kleur of zwart-wit
- [feshness](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#freshness): filter afbeeldingen op leeftijd (bijvoorbeeld afbeeldingen die in de afgelopen week door Bing zijn gedetecteerd)
- [height](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#height), [width](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#width): filter afbeeldingen op breedte en hoogte
- [imageContent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagecontent): filter afbeeldingen op inhoud (bijvoorbeeld afbeeldingen die alleen het gezicht van een persoon bevatten)
- [imageType](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imagetype): filter afbeeldingen op type (zoals illustraties, GIF-animaties of doorzichtige achtergronden)
- [license](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#license): filter afbeeldingen op het type licentie dat aan de site is gekoppeld
- [size](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#size): filter afbeeldingen op grootte, zoals kleine afbeeldingen tot 200x200 pixels

Als u afbeeldingen uit een bepaald domein wilt opvragen, gebruikt de query-operator [site:](http://msdn.microsoft.com/library/ff795613.aspx).

> [!NOTE]
> Als u de operator `site:` gebruikt, is het afhankelijk van de query mogelijk dat de respons inhoud voor volwassenen bevat, ongeacht de instelling van [safeSearch](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#safesearch). Gebruik `site:` alleen als u zich bewust bent van de inhoud op de site en uw scenario de mogelijkheid van inhoud voor volwassenen ondersteunt.

Het volgende voorbeeld laat zien hoe u kleine afbeeldingen opvraagt van ContosoSailing.com die Bing de afgelopen week heeft gedetecteerd.  

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies+site:contososailing.com&size=small&freshness=week&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

## <a name="pivoting-the-query"></a>De query draaien

Als Bing de oorspronkelijke zoekopdracht kan segmenteren, bevat het object [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) het veld `pivotSuggestions`. Als de oorspronkelijke query bijvoorbeeld *Microsoft Surface* was, kan Bing de query segmenteren in *Microsoft* en *Surface*.  

In het volgende voorbeeld ziet u de draaisuggesties voor *Microsoft Surface*.  

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface&FORM=OIIARP",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions": [...],
    "pivotSuggestions": [{
        "pivot": "microsoft",
        "suggestions": [{
            "text": "Contoso Surface",
            "displayText": "Contoso",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=OtterBox+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Contoso...",
                    "searchLink": "https:\/\/api.cognitive.microsoft.com\/api...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Contoso+Surface..."
            }
        },
        {
            "text": "Adatum Surface",
            "displayText": "Adatum",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Adatum+Surface&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Adatum+Surface&pid=Ap..."
            }
        },
        ...
        ]
    },
    {
        "pivot": "surface",
        "suggestions": [{
            "text": "Microsoft Surface4",
            "displayText": "Surface4",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface...",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft..."
            }
        },
        {
            "text": "Microsoft Tablet",
            "displayText": "Tablet",
            "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Tablet&FORM=IRQBPS",
            "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?...",
            "thumbnail": {
                "thumbnailUrl": "https:\/\/tse3.mm.bing.net\/th?q=Microsoft+Tablet..."
            }
        },
        ...
    ],
    "nextOffsetAddCount": 0
}
```

U kunt dan de optionele zoektermen weergeven aan de gebruiker als deze interessant zijn voor de gebruiker.

Het veld `pivotSuggestions` bevat de lijst met segmenten (draaipunten) waarin de oorspronkelijke query is onderverdeeld. Voor elk draaipunt bevat de respons een lijst met [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj)-objecten met voorgestelde query's. Het veld `text` bevat de voorgestelde query en het veld `displayText` de term die het draaipunt in de oorspronkelijke query vervangt, zoals Release Date of Surface.

U kunt de velden `text` en `thumbnail` gebruiken om de draaiqueryreeksen weer te geven aan de gebruiker als de queryreeks de informatie is waar ze in feite naar op zoek zijn. Maak de miniatuur en tekst klikbaar met de URL `webSearchUrl` of `searchLink`. Gebruik `webSearchUrl` om de gebruiker om te leiden naar de zoekresultaten of `searchLink` als u een eigen resultatenpagina hebt.

<!-- Need a sanitized version of the image
The following shows an example of the pivot queries.

![Pivot suggestions](./media/cognitive-services-bing-images-api/bing-image-pivotsuggestion.GIF)
-->

## <a name="expanding-the-query"></a>Query uitbreiden

Als Bing de query kan uitbreiden om de oorspronkelijke zoekopdracht te beperken, bevat het object [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) het veld `queryExpansions`. Als de query bijvoorbeeld *Microsoft Surface* was, kan de uitgebreide query er zo uitzien: Microsoft Surface **Pro 3**, Microsoft Surface **RT**, Microsoft Surface **Phone** en Microsoft Surface **Hub**.

In het volgende voorbeeld ziet u de uitgebreide query's voor *Microsoft Surface*.

```json
{
    "_type": "Images",
    "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=microsoft%20surface...",
    "totalEstimatedMatches": 1000,
    "value": [...],
    "queryExpansions":  [{
        "text": "Microsoft Surface Pro 3",
        "displayText": "Pro 3",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Pro+3...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=Microsoft...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Pro+3..."
        }
    },
    {
        "text": "Microsoft Surface RT",
        "displayText": "RT",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+RT...",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+RT..."
        }
    },
    {
        "text": "Microsoft Surface Phone",
        "displayText": "Phone",
        "webSearchUrl": "https:\/\/www.bing.com\/images\/search?q=Microsoft+Surface+Phone",
        "searchLink": "https:\/\/api.cognitive.microsoft.com\/api\/v7\/images\/search?q=...",
        "thumbnail": {
            "thumbnailUrl": "https:\/\/tse4.mm.bing.net\/th?q=Microsoft+Surface+Phone..."
        }
    }],
    "pivotSuggestions": [...],
    "nextOffsetAddCount": 0
}
```

Het veld `queryExpansions` bevat een lijst met [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj)-objecten. Het veld `text` bevat de uitgebreide query en het veld `displayText` bevat de term na de uitbreiding. U kunt de velden `text` en `thumbnail` gebruiken om de uitgebreide queryreeksen weer te geven aan de gebruiker als de uitgebreide queryreeks is waar ze in feite naar op zoek zijn. Maak de miniatuur en tekst klikbaar met de URL `webSearchUrl` of `searchLink`. Gebruik `webSearchUrl` om de gebruiker om te leiden naar de zoekresultaten of `searchLink` als u een eigen resultatenpagina hebt.

<!-- Removing until we can replace with a sanitized image.
The following shows an example Bing implementation that uses expanded queries. If the user clicks the Microsoft Surface Pro 3 link, they're taken to the Bing search results page, which shows them images of the Pro 3.

![Query expansion suggestions](./media/cognitive-services-bing-images-api/bing-image-queryexpansion.GIF)
-->

## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Volgende stappen

Zie [de snelstartgids over C#](quickstarts/csharp.md) om snel aan de slag met uw eerste aanvraag.

Lees de [naslaghandleiding over de Bing Afbeeldingen zoeken-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) om goed voorbereid aan de slag te gaan. De handleiding bevat de lijst met eindpunten, headers en queryparameters die u nodig hebt om zoekresultaten op te vragen. Daarnaast vindt u hier definities van de responsobjecten.

Zie [Automatische suggestie-API voor Bing](../bing-autosuggest/get-suggested-search-terms.md) om de gebruikerservaring van het zoekvak te verbeteren. Als de gebruiker een zoekterm invoert, kunt u deze API aanroepen om relevante querytermen weer te geven die door anderen zijn gebruikt.

Lees [Gebruiks- en weergavevereisten voor Bing](./useanddisplayrequirements.md) om er zeker van te zijn dat u alle regels voor het gebruik van de zoekresultaten volgt.

Wanneer u de Bing Afbeeldingen zoeken-API aanroept, retourneert Bing een lijst met resultaten. De lijst is een subset van het totale aantal resultaten die relevant zijn voor de query. Het veld `totalEstimatedMatches` in de respons bevat een schatting van het aantal afbeeldingen dat beschikbaar is om weer te geven. Zie [Afbeeldingen per pagina weergeven](./paging-images.md) voor meer informatie over het paginagewijs weergeven van de resterende afbeeldingen.