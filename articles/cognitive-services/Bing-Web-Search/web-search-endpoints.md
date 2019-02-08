---
title: Eindpunt op het web zoeken
titleSuffix: Azure Cognitive Services
description: Samenvatting van het Web search API-eindpunt.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: article
ms.date: 11/14/2018
ms.author: v-gedod
ms.openlocfilehash: 8c3fd0fc091edbc4323315f636ed2f4fea7d822a
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55880785"
---
# <a name="web-search-endpoint"></a>Eindpunt op het web zoeken

De **webzoekopdrachten-API** retourneert webpagina's, nieuws, afbeeldingen, video's, en [entiteiten](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Entiteiten hebben overzichtsinformatie over een persoon, plaats of onderwerp.

## <a name="endpoint"></a>Eindpunt

Als u zoekresultaten voor webpagina's met behulp van de API voor Bing, verzendt een `GET` aanvraag naar het volgende eindpunt. De kop- en URL-parameters definiëren verder specificaties.

**Eindpunt**: Retourneert resultaten die relevant voor de zoekquery van de gebruiker gedefinieerd zijn door `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```

Eindpunt: Zie voor meer informatie over kopteksten, parameters, markt codes, antwoordobjecten, fouten en meer de [Web-API voor Bing versie 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) verwijzing.

## <a name="response-json"></a>JSON-antwoord

Het antwoord op een webverzoek zoeken bevat alle resultaten als JSON-objecten. Het parseren van het resultaat is procedures voor het verwerken van de elementen van elk type vereist. Zie de [zelfstudie](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) en [broncode](https://github.com/Azure-Samples/cognitive-services-REST-api-samples/tree/master/Tutorials/Bing-Web-Search) voor voorbeelden.

## <a name="next-steps"></a>Volgende stappen

De **Bing** API's ondersteunen zoekacties die retourneren van resultaten op basis van hun type. Alle eindpunten voor zoeken worden resultaten geretourneerd als JSON-antwoord-objecten.  Alle eindpunten ondersteuning voor query's die een specifieke taal en locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de referentiepagina's voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor meer voorbeelden van basic met behulp van de webzoekopdrachten-API-aanvragen [snel starten van het Web zoeken](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web).
