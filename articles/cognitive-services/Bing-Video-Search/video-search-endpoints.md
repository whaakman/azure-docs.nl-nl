---
title: Video's zoeken-eindpunten - Bing video's zoeken
titlesuffix: Azure Cognitive Services
description: Samenvatting van de Bing video's zoeken-API-eindpunt.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 12/04/2017
ms.author: rosh
ms.openlocfilehash: db65f0ec857b2d69a2d36a22a1d9e3676af91683
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55190623"
---
# <a name="video-search-endpoints"></a>Video zoeken-eindpunten
De **video's zoeken-API** bevat drie eindpunten.  Eindpunt 1 retourneert video's van het Web op basis van een query. Eindpunt 2 retourneert inzichten over een video die op basis van de `modules` URL-parameter.  3-eindpunt retourneert trending afbeeldingen.

## <a name="endpoints"></a>Eindpunten
Video om resultaten te krijgen met behulp van de API voor Bing, verzendt een `GET` aanvraag naar een van de volgende eindpunten. De kop- en URL-parameters gebruiken om te definiëren verder specificaties.

**1:** Video's die relevant voor de zoekquery van de gebruiker gedefinieerd zijn door retourneert `?q=""`.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search
```

**2-eindpunt:** Retourneert informatie over een video, zoals gerelateerde video's. Bevatten de `modules` [queryparameter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference#query-parameters), dit is een door komma's gescheiden lijst met inzichten om aan te vragen.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/details
```

**3-eindpunt:** Retourneert-video's die populair zijn gebaseerd op zoekaanvragen anderen. De video's worden gescheiden in verschillende categorieën, bijvoorbeeld op basis van opmerkelijk personen of gebeurtenissen.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/trending
```

Voor meer informatie over kopteksten, parameters, markt codes, antwoordobjecten, fouten, enz., Zie de [Bing video's zoeken-API voor Bing versie 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-video-api-v7-reference) verwijzing.
## <a name="response-json"></a>JSON-antwoord
Het antwoord op een video's zoeken-aanvraag bevat de resultaten als JSON-objecten. Voor Zie voor voorbeelden van het parseren van de resultaten de [zelfstudie](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app) en [broncode](https://docs.microsoft.com/azure/cognitive-services/bing-video-search/tutorial-bing-video-search-single-page-app-source).

## <a name="next-steps"></a>Volgende stappen
De **Bing** API's ondersteunen zoekacties die retourneren van resultaten op basis van hun type. Alle eindpunten voor zoeken worden resultaten geretourneerd als JSON-antwoord-objecten.  Alle eindpunten ondersteuning voor query's die een specifieke taal en/of locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de referentiepagina's voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor meer voorbeelden van basic met behulp van de Video zoeken-API-aanvragen [snel starten van video's zoeken](https://docs.microsoft.com/azure/cognitive-services/bing-video-search).
