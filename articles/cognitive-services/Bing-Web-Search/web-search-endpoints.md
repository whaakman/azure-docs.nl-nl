---
title: Eindpunt op het web zoeken
titleSuffix: Azure Cognitive Services
description: Samenvatting van het Web search API-eindpunt.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: article
ms.date: 11/28/2017
ms.author: v-gedod
ms.openlocfilehash: 3058ca6cf0eb99486dd4c269d43b274fb367f7a9
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46125456"
---
# <a name="web-search-endpoint"></a>Eindpunt op het web zoeken
De **webzoekopdrachten-API** retourneert webpagina's, nieuws, afbeeldingen, video's, en [entiteiten](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/search-the-web). Entiteiten bevat overzichtsinformatie over een persoon, plaats of onderwerp.
## <a name="endpoint"></a>Eindpunt
Als u zoekresultaten voor webpagina's met behulp van de API voor Bing, verzendt een `GET` aanvraag naar het volgende eindpunt. De kop- en URL-parameters definiëren verder specificaties.

**Eindpunt**: retourneert resultaten die relevant voor de gebruiker zijn gedefinieerd door zoekquery `?q=""`.
```
GET https://api.cognitive.microsoft.com/bing/v7.0/search
```
Eindpunt: Voor meer informatie over kopteksten, parameters, markt codes, antwoordobjecten, fouten, enz., Zie de [Web-API voor Bing versie 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) verwijzing.

## <a name="response-json"></a>JSON-antwoord
Het antwoord op een webverzoek zoeken bevat alle resultaten als JSON-objecten. Het parseren van het resultaat is procedures voor het verwerken van de elementen van elk type vereist. Zie de [zelfstudie](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app) en [broncode](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/tutorial-bing-web-search-single-page-app-source) voor voorbeelden.

## <a name="next-steps"></a>Volgende stappen
De **Bing** API's ondersteunen zoekacties die retourneren van resultaten op basis van hun type. Alle eindpunten voor zoeken worden resultaten geretourneerd als JSON-antwoord-objecten.  Alle eindpunten ondersteuning voor query's die een specifieke taal en/of locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de referentiepagina's voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor meer voorbeelden van basic met behulp van de webzoekopdrachten-API-aanvragen [snel starten van het Web zoeken](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/search-the-web).
