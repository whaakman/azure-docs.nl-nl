---
title: Query's verzenden naar de Bing afbeeldingen zoeken-API | Microsoft Docs
description: Meer informatie over het verzenden en aanpassen van zoekquery's verzonden naar de Bing afbeeldingen zoeken-API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.assetid: C2862E98-8BCC-423B-9C4A-AC79A287BE38
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 8/8/2018
ms.author: aahi
ms.openlocfilehash: d74f59ffcf095e639686a3ada3b09dac988fc544
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2018
ms.locfileid: "40082603"
---
# <a name="sending-queries-to-the-bing-image-search-api"></a>Query's verzenden naar de Bing afbeeldingen zoeken-API

De Bing afbeeldingen zoeken-API biedt een ervaring die vergelijkbaar is met Bing.com/Images doordat u kunt een zoekquery voor de gebruiker naar Bing te verzenden en een lijst met relevante afbeeldingen teruggaan.

## <a name="using-and-suggesting-search-terms"></a>Met behulp van en zoektermen voorstellen

Nadat u een zoekterm invoert, URL-coderen van de term voordat u de [ **q** ](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#query) queryparameter. Bijvoorbeeld, als een gebruiker invoert *Zeilsloepen*, stel `q` naar `sailing+dinghies` of `sailing%20dinghies`.

Als uw app een zoekvak waarin zoektermen zijn ingevoerd heeft, kunt u de [Bing Automatische suggestie-API](../../bing-autosuggest/get-suggested-search-terms.md) op de ervaring te verbeteren door voorgestelde zoektermen in realtime weer te geven. De API retourneert voorgestelde query tekenreeksen op basis van gedeeltelijke zoektermen en Azure cognitive services.

## <a name="pivoting-the-query"></a>De query draaien

Als de oorspronkelijke query, de geretourneerde door Bing segmenteren kunt [installatiekopieën](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#images) object bevat `pivotSuggestions` die kunnen worden weergegeven als optionele zoektermen aan de gebruiker. Bijvoorbeeld, als de oorspronkelijke query is *Microsoft Surface*, Bing mogelijk in de query in segmenten *Microsoft* en *Surface* en voorgestelde draaigrafieken voor elk ervan opgeven. Deze kunnen worden weergegeven als optionele querytermen aan de gebruiker.

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

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Volgende stappen

Als u de Bing afbeeldingen zoeken-API voordat u dit nog niet hebt geprobeerd, probeert u een [snelstartgids](../quickstarts/csharp.md). Als u ingewikkeldere zoekt, probeert u de zelfstudie voor het maken van een [één pagina web-app](../tutorial-bing-image-search-single-page-app.md).
