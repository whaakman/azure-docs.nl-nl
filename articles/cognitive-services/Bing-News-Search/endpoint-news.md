---
title: Bing nieuws zoeken-eindpunten | Microsoft Docs
description: Samenvatting van het nieuws zoeken-API-eindpunt.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-news-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: ab892e947566adf025499382b213a52ed3e96e35
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47433681"
---
# <a name="news-search-endpoints"></a>Nieuws zoeken-eindpunten
De **nieuws zoeken-API** retourneert nieuws artikelen, webpagina's, afbeeldingen, video's, en [entiteiten](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Entiteiten bevat overzichtsinformatie over een persoon, plaats of onderwerp.
## <a name="endpoints"></a>Eindpunten
Als u met behulp van de API voor Bing zoekresultaten voor nieuws, verzendt een `GET` aanvraag naar een van de volgende eindpunten. De kop- en URL-parameters definiëren verder specificaties.

**Eindpunt 1:** nieuwsitems op basis van de zoekquery van de gebruiker geretourneerd. Als de query leeg is, retourneert de aanroep van de bovenste nieuwsartikelen. De query `?q=""` optie kan ook worden gebruikt met de `/news` URL. Zie voor beschikbaarheid, [ondersteunde landen en markten](language-support.md#supported-markets-for-news-search-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/search
```

**Eindpunt 2:** retourneert de nieuws-items per categorie. U kunt de beste zakelijke, sport of entertainment artikelen zijn die specifiek aanvragen `category=business`, `category=sports`, of `category=entertainment`.  De `category` parameter kan alleen worden gebruikt met de `/news` URL. Er zijn een paar formele vereisten voor het opgeven van categorieën; Raadpleeg `category` in de [queryparameter](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference#query-parameters) documentatie. Zie voor beschikbaarheid, [ondersteunde landen en markten](language-support.md#supported-markets-for-news-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news  
```

**3-eindpunt:** nieuwsonderwerpen retourneert die momenteel zijn populair op sociale netwerken. Wanneer de `/trendingtopics` optie is opgenomen, zoeken in Bing verschillende andere parameters worden genegeerd, zoals `freshness` en `?q=""`. Zie voor beschikbaarheid, [ondersteunde landen en markten](language-support.md#supported-markets-for-news-trending-endpoint).
```
GET https://api.cognitive.microsoft.com/bing/v7.0/news/trendingtopics
```

Voor meer informatie over kopteksten, parameters, markt codes, antwoordobjecten, fouten, enz., Zie de [zoeken in Bing nieuws-API versie 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-news-api-v7-reference) verwijzing.
## <a name="response-json"></a>JSON-antwoord
Het antwoord op een zoekaanvraag nieuws neemt de resultaten op als JSON-objecten. Procedures voor het verwerken van de elementen van elk type parseren van de resultaten worden vereist. Zie de [zelfstudie](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app) en [broncode](https://docs.microsoft.com/azure/cognitive-services/bing-news-search/tutorial-bing-news-search-single-page-app-source) voor voorbeelden.

## <a name="next-steps"></a>Volgende stappen
De **Bing** API's ondersteunen zoekacties die retourneren van resultaten op basis van hun type. Alle eindpunten voor zoeken worden resultaten geretourneerd als JSON-antwoord-objecten.  Alle eindpunten ondersteuning voor query's die een specifieke taal en/of locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de referentiepagina's voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor meer voorbeelden van basic met behulp van het nieuws zoeken-API-aanvragen [snel starten van zoeken in Bing nieuws](https://docs.microsoft.com/azure/cognitive-services/bing-news-search).
