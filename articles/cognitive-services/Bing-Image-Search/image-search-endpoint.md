---
title: Afbeelding zoeken eindpunten | Microsoft Docs
description: Samenvatting van het eindpunt van de afbeelding zoeken-API.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 11/30/2017
ms.author: v-gedod
ms.openlocfilehash: 0d5f28bfdb45b27b04df068f75e8a20d0235d12b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344413"
---
# <a name="image-search-endpoints"></a>Afbeelding zoeken eindpunten
De **installatiekopie Search API** omvat drie eindpunten.  Eindpunt 1 retourneert installatiekopieën van het Web op basis van een query. Eindpunt 2 retourneert [ImageInsights](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#imageinsightsresponse).  Eindpunt 3 retourneert trends installatiekopieën.
## <a name="endpoints"></a>Eindpunten
Als u de resultaten van de installatiekopie met de Bing-API, een aanvraag te verzenden naar een van de volgende eindpunten. De kop- en URL-parameters gebruiken voor het definiëren van verdere specificaties.

**Eindpunt 1:** retourneert installatiekopieën die relevant voor de zoekopdracht van de gebruiker gedefinieerd zijn door `?q=""`.
``` 
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search
```

**Eindpunt 2:** retourneert inzicht in een afbeelding, met behulp van `GET` of `POST`.
``` 
 GET or POST https://api.cognitive.microsoft.com/bing/v7.0/images/details
```
Een aanvraag voor ophalen retourneert inzicht in een installatiekopie, zoals webpagina's met de installatiekopie. Bevatten de [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#insightstoken) parameter met een `GET` aanvraag.

Of u kunt een binaire opnemen in de hoofdtekst van een `POST` aanvragen en stel de [modules](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#modulesrequested) -parameter voor `RecognizedEntities`. Hiermee herstelt u een [insightsToken](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v5-reference#insightstoken) moet worden gebruikt als een parameter in een toekomstige `GET` -verzoek informatie over mensen in de installatiekopie retourneert.  Ingesteld `modules` naar `All` om alle inzichten, behalve `RecognizedEntities` in de resultaten van de `POST` zonder maken met behulp van een andere aanroep van de `insightsToken`. 


**3-eindpunt:** retourneert de installatiekopieën die zijn trends op basis van de search-aanvragen die door anderen zijn aangebracht. De afbeeldingen zijn verdeeld in verschillende categorieën, bijvoorbeeld op basis van opmerkelijk personen of gebeurtenissen.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/images/trending
```

Zie voor een lijst van markten die ondersteuning bieden voor installatiekopieën van trends [trends installatiekopieën](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/trending-images).

Voor meer informatie over kopteksten, parameters, markt codes, antwoord objecten, fouten, enz., Zie de [Bing-API van zoekservice installatiekopie v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference) verwijzing.
## <a name="response-json"></a>Antwoord JSON
Het antwoord op een zoekaanvraag installatiekopie bevat de resultaten als JSON-objecten. Voor Zie voor voorbeelden van het parseren van de resultaten de [zelfstudie](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app) en [broncode](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/tutorial-bing-image-search-single-page-app-source).

## <a name="next-steps"></a>Volgende stappen
De **Bing** API's ondersteunen zoekacties die resultaten volgens hun type retourneren. Alle zoeken eindpunten retourneren resultaten als JSON-antwoord-objecten.  Alle eindpunten ondersteuning query's die een specifieke taal en/of de locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de pagina's met naslaginformatie voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor voorbeelden van basic-aanvragen via de zoekopdracht installatiekopie API [installatiekopie Search Quick-Start](https://docs.microsoft.com/azure/cognitive-services/bing-image-search/search-the-web).