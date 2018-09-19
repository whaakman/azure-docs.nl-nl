---
title: Eindpunten in de afbeelding zoeken - Bing afbeeldingen zoeken-API
titleSuffix: Azure Cognitive Services
description: Een lijst met beschikbare eindpunten voor de Bing afbeeldingen zoeken-API.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 11/30/2017
ms.author: v-gedod
ms.openlocfilehash: ca38943908bf3eee04c40cf4decf81fd20b08a1f
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46295917"
---
# <a name="image-search-endpoints"></a>Afbeeldingen zoeken-eindpunten

De **afbeeldingen zoeken-API** bevat drie eindpunten.  Eindpunt 1 geeft als resultaat van installatiekopieën van het Web op basis van een query. Eindpunt 2 retourneert [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse).  3-eindpunt retourneert trending afbeeldingen.
## <a name="endpoints"></a>Eindpunten
Als u de resultaten van de installatiekopie met behulp van de Bing-API, een aanvraag te verzenden naar een van de volgende eindpunten. De kop- en URL-parameters gebruiken om te definiëren verder specificaties.

**Eindpunt 1:** retourneert installatiekopieën die relevant voor de zoekquery van de gebruiker gedefinieerd zijn door `?q=""`.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search
```

**Eindpunt 2:** retourneert inzichten over een afbeelding, met behulp van `GET` of `POST`.
```
 GET or POST https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
Een GET-aanvraag retourneert inzichten over een afbeelding, zoals webpagina's die de installatiekopie van het opnemen. Bevatten de [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) parameter met een `GET` aanvraag.

Of u kunt een binaire opnemen in de hoofdtekst van een `POST` aanvragen en stel de [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) parameter `RecognizedEntities`. Dit resulteert in een [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v5-reference#insightstoken) te gebruiken als een parameter in een latere `GET` aanvraag informatie over mensen in de afbeelding retourneert.  Instellen `modules` naar `All` alle om inzichten te verkrijgen, behalve `RecognizedEntities` in de resultaten van de `POST` zonder te maken met behulp van een andere aanroep van de `insightsToken`.


**3-eindpunt:** retourneert de installatiekopieën die populair zijn gebaseerd op zoekaanvragen anderen. De afbeeldingen worden gescheiden in verschillende categorieën, bijvoorbeeld op basis van opmerkelijk personen of gebeurtenissen.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending
```

Zie voor een lijst van markten die ondersteuning bieden voor trending afbeeldingen, [Trending afbeeldingen](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images).

Voor meer informatie over kopteksten, parameters, markt codes, antwoordobjecten, fouten, enz., Zie de [Bing afbeeldingen zoeken-API voor Bing versie 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) verwijzing.
## <a name="response-json"></a>JSON-antwoord
Het antwoord op een zoekaanvraag afbeelding bevat resultaten als JSON-objecten. Voor Zie voor voorbeelden van het parseren van de resultaten de [zelfstudie](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app) en [broncode](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app-source).

## <a name="next-steps"></a>Volgende stappen
De **Bing** API's ondersteunen zoekacties die retourneren van resultaten op basis van hun type. Alle eindpunten voor zoeken worden resultaten geretourneerd als JSON-antwoord-objecten.  Alle eindpunten ondersteuning voor query's die een specifieke taal en/of locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de referentiepagina's voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor meer voorbeelden van basic met behulp van de afbeeldingen zoeken-API-aanvragen [snel starten van afbeeldingen zoeken-](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/search-the-web).
