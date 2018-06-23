---
title: Bing nieuws zoeken eindpunten | Microsoft Docs
description: Samenvatting van het eindpunt van de nieuws zoeken-API.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: b8ae99698926a818bf22b0b4027af8610413aaa6
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344450"
---
# <a name="news-search-endpoints"></a>Eindpunten van nieuws zoeken
De **nieuws Search API** retourneert nieuws artikelen, webpagina's, afbeeldingen, video's, en [entiteiten](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Entiteiten bevatten overzichtsgegevens van een persoon, plaats of onderwerp. 
## <a name="endpoints"></a>Eindpunten
Voor nieuws zoekresultaten wilt weergeven met de Bing-API, stuurt u een `GET` aanvraag voor een van de volgende eindpunten. De kop- en URL-parameters definiëren verdere specificaties.

**Eindpunt 1:** nieuwsitems op basis van de gebruiker zoekquery retourneert. Als de zoekopdracht leeg is, retourneert de aanroep van de artikelen nieuws. De query `?q=""` optie kan ook worden gebruikt met de `/news` URL. Zie voor beschikbaarheid, [ondersteunde landen en markten](supported-countries-markets.md#supported-markets-for-news-search-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search 
``` 

**Eindpunt 2:** retourneert de bovenste nieuwsitems per categorie. U kunt de bovenste business, sport of entertainment artikelen zijn die specifiek aanvragen `category=business`, `category=sports`, of `category=entertainment`.  De `category` parameter kan alleen worden gebruikt met de `/news` URL. Er zijn enkele formele vereisten voor het opgeven van categorieën; Raadpleeg `category` in de [queryparameter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query-parameters) documentatie. Zie voor beschikbaarheid, [ondersteunde landen en markten](supported-countries-markets.md#supported-markets-for-news-endpoint). 
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news  
```

**3-eindpunt:** retourneert nieuwsonderwerpen die momenteel worden trends op sociale netwerken. Wanneer de `/trendingtopics` optie is opgenomen, Bing-zoekopdracht verschillende andere parameters worden genegeerd, zoals `freshness` en `?q=""`. Zie voor beschikbaarheid, [ondersteunde landen en markten](supported-countries-markets.md#supported-markets-for-news-trending-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/trendingtopics 
``` 

Voor meer informatie over kopteksten, parameters, markt codes, antwoord objecten, fouten, enz., Zie de [Bing nieuws search API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference) verwijzing.
## <a name="response-json"></a>Antwoord JSON
Het antwoord op een zoekaanvraag nieuws bevat resultaten als JSON-objecten. Procedures voor het verwerken van de elementen van elk type bij het parseren van de resultaten worden vereist. Zie de [zelfstudie](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app) en [broncode](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app-source) voor voorbeelden.

## <a name="next-steps"></a>Volgende stappen
De **Bing** API's ondersteunen zoekacties die resultaten volgens hun type retourneren. Alle zoeken eindpunten retourneren resultaten als JSON-antwoord-objecten.  Alle eindpunten ondersteuning query's die een specifieke taal en/of de locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de pagina's met naslaginformatie voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor voorbeelden van basic-aanvragen via de zoekopdracht nieuws API [Bing nieuws Search Quick-Start](https://docs.microsoft.com/azure/cognitive-services/bing-news-search).