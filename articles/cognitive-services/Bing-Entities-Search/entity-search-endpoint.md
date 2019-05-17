---
title: De Bing Entity Search API-eindpunt
titlesuffix: Azure Cognitive Services
description: Meer informatie over de Bing Entity Search API-eindpunt en verzenden van aanvragen naar deze.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-entity-search
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: aahi
ms.openlocfilehash: b29e568d6b3b1382b5be434500014f10740b58f8
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65788549"
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
> [Wat is de Bing Entiteiten zoeken-API?](overview.md)

## <a name="see-also"></a>Zie ook 

Zie voor meer informatie over kopteksten, parameters, markt codes, antwoordobjecten, aan fouten en meer, de [Bing Entity Search API voor Bing versie 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) naslagartikel.
