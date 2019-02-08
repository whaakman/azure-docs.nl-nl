---
title: Automatische suggestie zoektermen - Bing webzoekopdrachten-API
titleSuffix: Azure Cognitive Services
description: De Bing webzoekopdrachten-API met de Bing Automatische suggestie-API, zodat gebruikers beschikken over een verbeterde zoekervaring worden gekoppeld.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 8/13/2018
ms.author: aahi
ms.openlocfilehash: 540c8c19d1c5ab371588b8a2092b72187382488f
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55874359"
---
# <a name="autosuggest-bing-search-terms-in-your-application"></a>Automatische suggestie van Bing zoektermen in uw toepassing

Als u een zoekvak aanbiedt waarin de gebruiker een zoekterm invoert, kunt u de [Automatische suggestie-API voor Bing](../bing-autosuggest/get-suggested-search-terms.md) gebruiken om de ervaring te verbeteren. De API retourneert voorgestelde queryreeksen op basis van gedeeltelijke zoektermen terwijl de gebruiker typt.

Nadat de gebruiker een zoekterm invoert, moet het URL-codering voordat de [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#query) queryparameter is ingesteld. Als de gebruiker bijvoorbeeld *sailing dinghies* invoert, stelt u `q` in op `sailing+dinghies` of `sailing%20dinghies`.

Als de queryterm een fout spelling bevat, antwoord van de zoekactie bevat een [QueryContext](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#querycontext) object. In het object worden de oorspronkelijke spelling weergegeven en de gecorrigeerde spelling die Bing heeft gebruikt voor de zoekopdracht.

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
