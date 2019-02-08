---
title: Bing Nieuws zoeken-eindpunten
titlesuffix: Azure Cognitive Services
description: Samenvatting van het nieuws zoeken-API-eindpunt.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-news-search
ms.topic: conceptual
ms.date: 1/10/2019
ms.author: v-gedod
ms.openlocfilehash: a3badf7772c6be6509608fbdb305dc3bc225acc0
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55879986"
---
# <a name="bing-news-search-api-endpoints"></a>Bing nieuws zoeken-API-eindpunten

De **nieuws zoeken-API** retourneert nieuws artikelen, webpagina's, afbeeldingen, video's, en [entiteiten](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Entiteiten bevat overzichtsinformatie over een persoon, plaats of onderwerp.

## <a name="endpoints"></a>Eindpunten

Nieuws om zoekresultaten te krijgen met behulp van de Bing nieuws zoeken-API, verzendt een `GET` aanvraag naar een van de volgende eindpunten. De kop- en URL-parameters definiëren verder specificaties.

### <a name="news-items-by-search-query"></a>Nieuwsitems door zoekquery

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search
```

Retourneert de nieuwsitems op basis van een zoekquery. Als de query leeg is, wordt de API bovenste nieuwsartikelen geretourneerd uit verschillende categorieën. Verzenden van een query door de url van uw zoekterm codering en u dit toevoegt aan de`q=""` parameter. Zie voor beschikbaarheid, [ondersteunde landen/regio's en markten](language-support.md#supported-markets-for-news-search-endpoint).

### <a name="top-news-items-by-category"></a>Nieuwsitems per categorie

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news  
```

Retourneert de nieuws-items per categorie. U kunt de beste zakelijke, sport of entertainment artikelen zijn die specifiek aanvragen `category=business`, `category=sports`, of `category=entertainment`.  De `category` parameter kan alleen worden gebruikt met de `/news` URL. Er zijn een paar formele vereisten voor het opgeven van categorieën; Raadpleeg `category` in de [queryparameter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query-parameters) documentatie. Verzenden van een query door de url van uw zoekterm codering en u dit toevoegt aan de`q=""` parameter. Zie voor beschikbaarheid, [ondersteunde landen/regio's en markten](language-support.md#supported-markets-for-news-endpoint).

### <a name="trending-news-topics"></a>Actuele nieuwsonderwerpen op 

```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/trendingtopics
```

Nieuwsonderwerpen retourneert die momenteel zijn populair op sociale netwerken. Wanneer de `/trendingtopics` optie is opgenomen, zoeken in Bing verschillende andere parameters worden genegeerd, zoals `freshness` en `?q=""`. Zie voor beschikbaarheid, [ondersteunde landen/regio's en markten](language-support.md#supported-markets-for-news-trending-endpoint).

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over kopteksten, parameters, markt codes, antwoordobjecten, fouten, enz., Zie de [zoeken in Bing nieuws-API versie 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference) verwijzing.

Zie de referentiepagina's voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor meer voorbeelden van basic met behulp van het nieuws zoeken-API-aanvragen [snel starten van zoeken in Bing nieuws](https://docs.microsoft.com/azure/cognitive-services/bing-news-search).
