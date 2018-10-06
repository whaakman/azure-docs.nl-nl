---
title: Bing Entiteiten zoeken-eindpunt
titlesuffix: Azure Cognitive Services
description: Samenvatting van de Bing Entity Search API-eindpunt.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: d781a4b3cd0119f5624b4dd20b514894ea339414
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816579"
---
# <a name="entity-search-endpoint"></a>Entiteit zoeken-eindpunten
De **Entity Search API** bevat één eindpunt waarmee entiteiten van het Web op basis van een query wordt geretourneerd.

## <a name="endpoint"></a>Eindpunt
Resultaten te verkrijgen van de entiteit met behulp van de **-API voor Bing**, verzendt een `GET` aanvraag naar het volgende eindpunt. De kop- en URL-parameters gebruiken om te definiëren verder specificaties.

**Eindpunt:** retourneert entiteiten die relevant voor de zoekquery van de gebruiker gedefinieerd zijn door `?q=""`.
```
 GET https://api.cognitive.microsoft.com/bing/v7.0/entities
```

Voor meer informatie over kopteksten, parameters, markt codes, antwoordobjecten, fouten, enz., Zie de [Bing Entity Search API voor Bing versie 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference) verwijzing.

## <a name="response-json"></a>JSON-antwoord
Het antwoord op een zoekaanvraag entiteit bevat resultaten als JSON-objecten. Zie voor meer voorbeelden van de resultaten [aan de slag](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start).

## <a name="next-steps"></a>Volgende stappen
De **Bing** API's ondersteunen zoekacties die retourneren van resultaten op basis van hun type. Alle eindpunten voor zoeken worden resultaten geretourneerd als JSON-antwoord-objecten.  Alle eindpunten ondersteuning voor query's die een specifieke taal en/of locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de referentiepagina's voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor voorbeelden van het gebruik van de entiteiten zoeken-API, [aan de slag](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/quick-start) en [grootte wordt gewijzigd en de miniatuurafbeeldingen bijsnijden](https://docs.microsoft.com/azure/cognitive-services/bing-entities-search/resize-and-crop-thumbnails).