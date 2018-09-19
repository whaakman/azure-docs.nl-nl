---
title: Verzenden van de installatiekopie van query's, Bing afbeeldingen zoeken-API
titleSuffix: Azure Cognitive Services
description: Meer informatie over het aanpassen van zoekquery's verzonden naar de Bing afbeeldingen zoeken-API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: C2862E98-8BCC-423B-9C4A-AC79A287BE38
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 8/8/2018
ms.author: aahi
ms.openlocfilehash: 5d2dcc60a8707394ec07a76e3286929db365c651
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46296512"
---
# <a name="send-queries-to-the-bing-image-search-api"></a>Query's verzenden naar de Bing afbeeldingen zoeken-API

De Bing afbeeldingen zoeken-API biedt een ervaring die vergelijkbaar is met Bing.com/Images. U kunt het verzenden van een zoekopdracht naar Bing, waarna een lijst met relevante installatiekopieën gebruiken.

## <a name="use-and-suggest-search-terms"></a>Gebruik en zoektermen voorstellen

Nadat een zoekterm invoert, URL-codering de term voordat u stelt de [ **q** ](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query) queryparameter. Als u bijvoorbeeld *Zeilsloepen*, stel `q` naar `sailing+dinghies` of `sailing%20dinghies`.

Als uw app een zoekvak waarin zoektermen zijn ingevoerd heeft, kunt u de [Bing Automatische suggestie-API](../../bing-autosuggest/get-suggested-search-terms.md) om de ervaring te verbeteren. De API kunt voorgestelde zoektermen weergeven in realtime. De API retourneert voorgestelde queryreeksen op basis van gedeeltelijke zoektermen en Cognitive Services.

## <a name="pivot-the-query"></a>De query van draaipunt

Als de oorspronkelijke query, de geretourneerde door Bing segmenteren kunt [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) -object bevat `pivotSuggestions`. Pivot suggesties kunnen worden weergegeven als optionele zoektermen aan de gebruiker. Bijvoorbeeld, als de oorspronkelijke query is *Microsoft Surface*, Bing mogelijk in de query in segmenten *Microsoft* en *Surface* en voorgestelde draaigrafieken voor elk ervan opgeven. Deze suggesties kunnen worden weergegeven als optionele querytermen aan de gebruiker.

Het volgende voorbeeld ziet de pivot suggesties voor *Microsoft Surface*:  

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

Het veld `pivotSuggestions` bevat de lijst met segmenten (draaipunten) waarin de oorspronkelijke query is onderverdeeld. Voor elk draaipunt bevat de respons een lijst met [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj)-objecten met voorgestelde query's. De `text` veld bevat de voorgestelde query. De `displayText` veld bevat de term die wordt vervangen door de draaitabel in de oorspronkelijke query. Een voorbeeld is versie datum van aanvallen.

Als de pivot-queryreeks wat de gebruiker zoekt is, gebruikt u de `text` en `thumbnail` velden om weer te geven van de draaitabel query uitvoeren op tekenreeksen. De miniatuur en tekst geklikt te maken met behulp van de `webSearchUrl` URL of de `searchLink` URL. Gebruik `webSearchUrl` aan de gebruiker doorsturen naar de Bing-zoekresultaten. Als u uw eigen pagina met zoekresultaten opgeven, gebruikt u `searchLink`.

<!-- Need a sanitized version of the image
The following shows an example of the pivot queries.

![Pivot suggestions](./media/cognitive-services-bing-images-api/bing-image-pivotsuggestion.GIF)
-->

## <a name="expand-the-query"></a>Vouw de query

Als Bing de query kan uitbreiden om de oorspronkelijke zoekopdracht te beperken, bevat het object [Images](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) het veld `queryExpansions`. Bijvoorbeeld, als de query is *Microsoft Surface*, de uitgebreide query's mogelijk:
- Microsoft Surface **Pro 3**.
- Microsoft Surface **RT**.
- Microsoft Surface **Phone**.
- Microsoft Surface **Hub**.

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

Het veld `queryExpansions` bevat een lijst met [Query](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query_obj)-objecten. De `text` veld bevat de uitgebreide query. De `displayText` veld bevat de term uitbreiding. Als de uitgebreide query-tekenreeks wat de gebruiker zoekt is, gebruikt u de `text` en `thumbnail` velden om de querytekenreeksen uitgebreide weer te geven. De miniatuur en tekst geklikt te maken met behulp van de `webSearchUrl` URL of de `searchLink` URL. Gebruik `webSearchUrl` aan de gebruiker doorsturen naar de Bing-zoekresultaten. Als u uw eigen pagina met zoekresultaten opgeven, gebruikt u `searchLink`.

<!-- Removing until we can replace with a sanitized image.
The following shows an example Bing implementation that uses expanded queries. If the user clicks the Microsoft Surface Pro 3 link, they're taken to the Bing search results page, which shows them images of the Pro 3.

![Query expansion suggestions](./media/cognitive-services-bing-images-api/bing-image-queryexpansion.GIF)
-->


## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Volgende stappen

Als u de Bing afbeeldingen zoeken-API voordat u dit nog niet hebt geprobeerd, probeert u een [snelstartgids](../quickstarts/csharp.md). Als u ingewikkeldere zoekt, probeert u de zelfstudie voor het maken van een [één pagina web-app](../tutorial-bing-image-search-single-page-app.md).
