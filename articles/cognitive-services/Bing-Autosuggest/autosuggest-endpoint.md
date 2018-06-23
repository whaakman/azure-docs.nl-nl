---
title: Eindpunt voor Automatische suggestie | Microsoft Docs
description: Samenvatting van de Automatische suggestie van API-eindpunt.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: article
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: 5aaddd09006cb6f1812bb6ae213a2f5e6720fb1b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345444"
---
# <a name="autosuggest-endpoint"></a>Eindpunt voor Automatische suggestie

De **API voor Automatische suggestie van** bevat één eindpunt waarmee een lijst met voorgestelde query's uit een gedeeltelijke zoekterm retourneert.

## <a name="endpoint"></a>Eindpunt

Voor voorgestelde query's met de Bing-API, stuurt u een `GET` aanvraag voor het volgende eindpunt. De kop- en URL-parameters gebruiken voor het definiëren van verdere specificaties.

**Eindpunt:** retourneert zoeksuggesties als JSON-resultaten die relevant voor de gebruikersinvoer gedefinieerd zijn door `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/Suggestions 
```

Voor meer informatie over kopteksten, parameters, markt codes, antwoord objecten, fouten, enz., Zie de [voor Automatische suggestie van Bing-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference) verwijzing.

## <a name="response-json"></a>Antwoord JSON

Het antwoord op een aanvraag voor Automatische suggestie bevat resultaten als JSON-objecten. Voor Zie voor voorbeelden van het parseren van de resultaten de [zelfstudie](tutorials/autosuggest.md) en [broncode](tutorials/autosuggest-source.md).

## <a name="next-steps"></a>Volgende stappen

De **Bing** API's ondersteunen zoekacties die resultaten volgens hun type retourneren. Alle zoeken eindpunten retourneren resultaten als JSON-antwoord-objecten.
Alle eindpunten ondersteuning query's die een specifieke taal en/of de locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de pagina's met naslaginformatie voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor voorbeelden van basic-aanvragen via de API voor Automatische suggestie van [snelstartgidsen voor Automatische suggestie van](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest).