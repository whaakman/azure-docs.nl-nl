---
title: Video zoeken eindpunten | Microsoft Docs
description: Samenvatting van het eindpunt van de Video zoeken-API.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-video-search
ms.topic: article
ms.date: 12/04/2017
ms.author: v-gedod
ms.openlocfilehash: 9836d9928362ab37b0a81ff5043d99f9bf353f22
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344446"
---
# <a name="video-search-endpoints"></a>Video Search-eindpunten
De **Video-API van zoekservice** omvat drie eindpunten.  Eindpunt 1 retourneert video's via het Web op basis van een query. Eindpunt 2 retourneert informatie over een video op basis van de `modules` URL-parameter.  Eindpunt 3 retourneert trends installatiekopieën.

## <a name="endpoints"></a>Eindpunten
Als u video resultaten met de Bing-API, verzendt een `GET` aanvraag voor een van de volgende eindpunten. De kop- en URL-parameters gebruiken voor het definiëren van verdere specificaties.

**1:** retourneert video's die relevant voor de zoekopdracht van de gebruiker gedefinieerd zijn door `?q=""`.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```

**Eindpunt 2:** retourneert informatie over een video, zoals gerelateerde video's. Bevatten de `modules` [queryparameter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters), dit is een door komma's gescheiden lijst van inzicht om aan te vragen.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/details
```

**3-eindpunt:** retourneert video's die zijn trends op basis van de search-aanvragen die door anderen zijn aangebracht. De video's die zijn verdeeld in verschillende categorieën, bijvoorbeeld op basis van opmerkelijk personen of gebeurtenissen.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending
```

Voor meer informatie over kopteksten, parameters, markt codes, antwoord objecten, fouten, enz., Zie de [Bing-API van zoekservice Video v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) verwijzing.
## <a name="response-json"></a>Antwoord JSON
Het antwoord op een zoekaanvraag video's bevat resultaten als JSON-objecten. Voor Zie voor voorbeelden van het parseren van de resultaten de [zelfstudie](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app) en [broncode](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app-source).

## <a name="next-steps"></a>Volgende stappen
De **Bing** API's ondersteunen zoekacties die resultaten volgens hun type retourneren. Alle zoeken eindpunten retourneren resultaten als JSON-antwoord-objecten.  Alle eindpunten ondersteuning query's die een specifieke taal en/of de locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de pagina's met naslaginformatie voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor voorbeelden van basic-aanvragen via de Video-zoekopdracht API [Video Search Quick-Start](https://docs.microsoft.com/azure/cognitive-services/bing-video-search).