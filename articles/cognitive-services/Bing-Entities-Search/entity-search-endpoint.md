---
title: De Bing Entity Search API-eindpunt
titlesuffix: Azure Cognitive Services
description: Meer informatie over de Bing Entity Search API-eindpunt en verzenden van aanvragen naar deze.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: v-gedod
ms.openlocfilehash: 3c2aa4b22c8e679f73692978d9e1f8009f11a46b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55875226"
---
# <a name="bing-entity-search-api-endpoint"></a>Bing Entity Search API-eindpunt


Bing Entity Search API heeft één eindpunt waarmee entiteiten van het Web op basis van een query wordt geretourneerd. Deze lijst met zoekresultaten worden geretourneerd in JSON.

## <a name="get-entity-results-from-the-endpoint"></a>Entiteitsresultaten van de van het eindpunt ophalen

Resultaten te verkrijgen van de entiteit met behulp van de **-API voor Bing**, verzendt een `GET` aanvraag naar het volgende eindpunt. Gebruik [headers](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#headers) en [queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query-parameters) voor het aanpassen van uw zoekopdracht. Search-aanvragen kunnen worden verzonden met behulp van de `?q=` parameter.

```cURL
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Wat is de Bing Entity Search API?](overview.md)

## <a name="see-also"></a>Zie ook 

Zie voor meer informatie over kopteksten, parameters, markt codes, antwoordobjecten, aan fouten en meer, de [Bing Entity Search API voor Bing versie 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) naslagartikel.
