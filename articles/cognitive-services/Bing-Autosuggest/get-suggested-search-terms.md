---
title: Wat is Bing Automatische suggesties?
titlesuffix: Azure Cognitive Services
description: Lees hier hoe u de Automatische suggestie-API voor Bing gebruikt.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: overview
ms.date: 09/12/2017
ms.author: scottwhi
ms.openlocfilehash: e1e1ec2a9f5329f5d7331b7ce3ced4924d001a49
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55174173"
---
# <a name="what-is-bing-autosuggest"></a>Wat is Bing Automatische suggesties?

Als u query's verstuurt naar een van de API's van Bing Zoeken, kunt u de Automatische suggestie-API gebruiken om de ervaring met het zoekvak te verbeteren. De Automatische suggestie-API retourneert een lijst met voorgestelde query's op basis van de gedeeltelijke queryreeks die de gebruiker invoert in het zoekvak. De suggesties worden weergegeven in de vervolgkeuzelijst van het zoekvak. De voorgestelde termen zijn gebaseerd op voorgestelde query's die andere gebruikers hebben uitgevoerd en op de bedoeling van gebruikers.

Normaal gesproken zou u deze API aanroepen telkens wanneer de gebruiker een nieuw teken in het zoekvak typt. De volledigheid van de queryreeks is van invloed op de relevantie van de voorgestelde querytermen die de API retourneert. Hoe vollediger de queryreeks, des te relevanter de lijst met voorgestelde querytermen. De suggesties die de API bijvoorbeeld kan retourneren voor *s* zijn waarschijnlijk minder relevant zijn dan de query's die worden geretourneerd voor *sailing dinghies*.

## <a name="getting-suggested-search-terms"></a>Voorgestelde zoektermen ophalen

In het volgende voorbeeld ziet u een aanvraag die de voorgestelde queryreeksen voor *sail* retourneert. Vergeet niet om de gedeeltelijke zoekterm van de gebruiker als URL te coderen wanneer u de parameter [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference#query) instelt. Als de gebruiker bijvoorbeeld *sailing les* invoert, stelt u `q` in op `sailing+les` of `sailing%20les`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
Ocp-Apim-Subscription-Key: 123456789ABCDE
X-MSEdge-ClientIP: 999.999.999.999
X-Search-Location: lat:47.60357;long:-122.3295;re:100
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
Host: api.cognitive.microsoft.com
```

Het volgende antwoord bevat een lijst met [SearchAction](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference#searchaction)-objecten die de voorgestelde querytermen bevatten.

```json
{
    "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
    "displayText" : "sailing lessons seattle",
    "query" : "sailing lessons seattle",
    "searchKind" : "WebSearch"
}, ...
```

Elke suggestie bevat een veld `displayText`, `query` en `url`. Het veld `displayText` bevat de voorgestelde query die u gebruikt voor het vullen van de vervolgkeuzelijst van het zoekvak. U moet alle suggesties uit het antwoord weergeven, en in de opgegeven volgorde.

Hieronder ziet u een voorbeeld van de vervolgkeuzelijst van een zoekvak met voorgestelde zoektermen.

![Vervolgkeuzelijst voor zoekvak met automatische suggesties](./media/cognitive-services-bing-autosuggest-api/bing-autosuggest-drop-down-list.PNG)

Als de gebruiker een voorgestelde query selecteert in de vervolgkeuzelijst, gebruikt u de zoekterm in het veld `query` om de [Bing Webzoekopdrachten-API](../bing-web-search/search-the-web.md) aan te roepen en zelf de resultaten weer te geven. Een alternatief is om de gebruiker via de URL in het veld `url` om te leiden naar de pagina met zoekresultaten van Bing.

## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Volgende stappen

Zie [Uw eerste query maken](quickstarts/csharp.md) om snel aan de slag te gaan met uw eerste aanvraag.

Lees de [naslaghandleiding over de Automatische suggestie-API voor Bing v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference) om goed voorbereid aan de slag te gaan. De handleiding bevat de lijst met eindpunten, headers en queryparameters die u nodig hebt om voorgestelde querytermen op te vragen, en de definities van de antwoordobjecten.

Leer hoe u kunt zoeken op internet met behulp van de [Bing Webzoekopdrachten-API](../bing-web-search/search-the-web.md).

Lees [Gebruiks- en weergavevereisten voor Bing](./useanddisplayrequirements.md) om er zeker van te zijn dat u alle regels voor het gebruik van de zoekresultaten volgt.
