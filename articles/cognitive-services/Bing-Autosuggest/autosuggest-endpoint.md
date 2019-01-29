---
title: Bing Automatische suggesties-eindpunt
titlesuffix: Azure Cognitive Services
description: Samenvatting van de Automatische suggestie-API-eindpunt.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: conceptual
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: 03f7ac11b08d9cad633e207337ff963114f2c68f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55149234"
---
# <a name="bing-autosuggest-endpoint"></a>Bing Automatische suggesties-eindpunt

De **Bing Automatische suggestie-API** bevat één eindpunt die resulteert in een lijst met voorgestelde query's uit een gedeeltelijke zoekterm.

## <a name="endpoint"></a>Eindpunt

Als u de voorgestelde query's met behulp van de API voor Bing, verzendt een `GET` aanvraag naar het volgende eindpunt. De kop- en URL-parameters gebruiken om te definiëren verder specificaties.

**Eindpunt:** Retourneert zoeksuggesties als JSON-resultaten die relevant voor de invoer van de gebruiker gedefinieerd zijn door `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/Suggestions 
```

Voor meer informatie over kopteksten, parameters, markt codes, antwoordobjecten, fouten, enz., Zie de [Bing Automatische suggestie-API versie 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference) verwijzing.

## <a name="response-json"></a>JSON-antwoord

Het antwoord op een automatische suggesties-aanvraag bevat de resultaten als JSON-objecten. Voor Zie voor voorbeelden van het parseren van de resultaten de [zelfstudie](tutorials/autosuggest.md) en [broncode](tutorials/autosuggest-source.md).

## <a name="next-steps"></a>Volgende stappen

De **Bing** API's ondersteunen zoekacties die retourneren van resultaten op basis van hun type. Alle eindpunten voor zoeken worden resultaten geretourneerd als JSON-antwoord-objecten.
Alle eindpunten ondersteuning voor query's die een specifieke taal en/of locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de referentiepagina's voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor meer voorbeelden van basic met behulp van de Automatische suggestie-API-aanvragen [Automatische suggestie-snelstartgidsen](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest).
