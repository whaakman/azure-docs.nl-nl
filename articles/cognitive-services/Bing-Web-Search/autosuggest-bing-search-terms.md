---
title: Automatische suggestie zoektermen - Bing webzoekopdrachten-API
titleSuffix: Azure Cognitive Services
description: De Bing webzoekopdrachten-API met de Bing Automatische suggestie-API, zodat gebruikers beschikken over een verbeterde zoekervaring worden gekoppeld.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-web-search
ms.topic: conceptual
ms.date: 8/13/2018
ms.author: erhopf
ms.openlocfilehash: 475ddfc2bcd9acd1312348a6c85e3d4627e48a3d
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46128941"
---
# <a name="autosuggest-bing-search-terms-in-your-application"></a>Automatische suggestie van Bing zoektermen in uw toepassing

Als u een zoekvak aanbiedt waarin de gebruiker een zoekterm invoert, kunt u de [Automatische suggestie-API voor Bing](../bing-autosuggest/get-suggested-search-terms.md) gebruiken om de ervaring te verbeteren. De API retourneert voorgestelde queryreeksen op basis van gedeeltelijke zoektermen terwijl de gebruiker typt.

Nadat de gebruiker een zoekterm invoert, moet het URL-codering voordat de [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query) queryparameter is ingesteld. Als de gebruiker bijvoorbeeld *sailing dinghies* invoert, stelt u `q` in op `sailing+dinghies` of `sailing%20dinghies`.

Als de queryterm een fout spelling bevat, antwoord van de zoekactie bevat een [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#querycontext) object. Het object bevat de oorspronkelijke spelling en de gecorrigeerde spelling die Bing voor de zoekopdracht gebruikt.

```json
"queryContext": {
    "originalQuery": "sialing dingy for sale",
    "alteredQuery": "sailing dinghy for sale",
    "alterationOverrideQuery": "+sialing +dingy for sale"
}
```

U kunt deze informatie gebruiken zodat de gebruiker weten dat u de queryreeks gewijzigd wanneer u de lijst met zoekresultaten weergeven.

![Voorbeeld van UX-context](./media/cognitive-services-bing-web-api/bing-query-context.PNG)  

## <a name="next-steps"></a>Volgende stappen  

* Voorbeeld van de beoordeling [Bing webzoekopdrachten-API-reacties](search-responses.md).  

## <a name="see-also"></a>Zie ook  

* [Bing webzoekopdrachten-API-verwijzing](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference)
