---
title: Entiteit zoeken eindpunt | Microsoft Docs
description: Samenvatting van de entiteit zoeken-API-eindpunt.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: a2557c6000445544b3b47a05d7d356ccaa9928b4
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344414"
---
# <a name="entity-search-endpoint"></a>Entiteit zoeken eindpunt
De **entiteit Search API** bevat een eindpunt dat entiteiten van het Web op basis van een query retourneert.

## <a name="endpoint"></a>Eindpunt
Scanresultaten entiteit ophalen met behulp van de **Bing-API**, verzendt een `GET` aanvraag voor het volgende eindpunt. De kop- en URL-parameters gebruiken voor het definiëren van verdere specificaties.

**Eindpunt:** retourneert entiteiten die relevant voor de zoekopdracht van de gebruiker gedefinieerd zijn door `?q=""`.
```
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

Voor meer informatie over kopteksten, parameters, markt codes, antwoord objecten, fouten, enz., Zie de [Bing-API van zoekservice entiteit v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) verwijzing.

## <a name="response-json"></a>Antwoord JSON
Het antwoord op een zoekaanvraag entiteit bevat resultaten als JSON-objecten. Zie voor voorbeelden van de resultaten [aan de slag](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start).

## <a name="next-steps"></a>Volgende stappen
De **Bing** API's ondersteunen zoekacties die resultaten volgens hun type retourneren. Alle zoeken eindpunten retourneren resultaten als JSON-antwoord-objecten.  Alle eindpunten ondersteuning query's die een specifieke taal en/of de locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de pagina's met naslaginformatie voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor voorbeelden van de entiteit zoekactie API [aan de slag](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start) en [miniatuurafbeeldingen bijsnijden en Resizing](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/resize-and-crop-thumbnails).