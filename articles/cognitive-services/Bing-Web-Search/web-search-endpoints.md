---
title: Search webeindpunt | Microsoft Docs
description: Samenvatting van het eindpunt van de Web zoeken-API.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: 72fbe1a0eb4379ad032e649f7299e37a0214adfb
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344459"
---
# <a name="web-search-endpoint"></a>Webeindpunt zoeken
De **Web Search API** retourneert webpagina's, nieuws, afbeeldingen, video's, en [entiteiten](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Entiteiten bevatten overzichtsgegevens van een persoon, plaats of onderwerp.
## <a name="endpoint"></a>Eindpunt
Voor zoekresultaten met de Bing-API, stuurt u een `GET` aanvraag voor het volgende eindpunt. De kop- en URL-parameters definiëren verdere specificaties.

**Eindpunt**: Web retourneert resultaten die relevant voor de gebruiker zijn zoeken query gedefinieerd door `?q=""`.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```
Eindpunt: Voor meer informatie over kopteksten, parameters, de markt codes, antwoord objecten, fouten, enz., Zie de [Web-API van Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) verwijzing.

## <a name="response-json"></a>Antwoord JSON
Het antwoord op een zoekaanvraag Web bevat alle resultaten als JSON-objecten. Procedures voor het verwerken van de elementen van elk type bij het parseren van het resultaat is vereist. Zie de [zelfstudie](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) en [broncode](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app-source) voor voorbeelden.

## <a name="next-steps"></a>Volgende stappen
De **Bing** API's ondersteunen zoekacties die resultaten volgens hun type retourneren. Alle zoeken eindpunten retourneren resultaten als JSON-antwoord-objecten.  Alle eindpunten ondersteuning query's die een specifieke taal en/of de locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de pagina's met naslaginformatie voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor voorbeelden van basic-aanvragen via de search Web API [zoeken van de Web-Quick-gestart](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web).