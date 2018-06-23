---
title: Aangepaste zoeken eindpunt | Microsoft Docs
description: Samenvatting van het aangepaste Search API-eindpunt.
services: cognitive-services
author: mikedodaro
manager: rosh
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 12/05/2017
ms.author: v-gedod
ms.openlocfilehash: 8d9851f3687a783f52a80a8dffcf2580d4710551
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344905"
---
# <a name="custom-search"></a>Aangepast zoeken
Bing aangepaste zoeken kunt u voor het maken van de ervaringen op maat gemaakte zoeken naar onderwerpen die u interesseren. Uw gebruikers zien zoekresultaten wilt weergeven die zijn toegesneden op de ze in plaats van interesseren inhoud naar de pagina met zoekresultaten wilt weergeven die niet relevant inhoud bevatten.

## <a name="custom-search-endpoint"></a>Aangepaste zoekactie eindpunt
Voor resultaten met de Bing aangepaste Search API, stuurt u een `GET` aanvraag voor het volgende eindpunt. De kop- en URL-parameters gebruiken voor het definiëren van verdere specificaties.

Eindpunt: Retourneert zoeksuggesties als JSON-resultaten die relevant voor de gebruikersinvoer gedefinieerd zijn door `?q=""`.
```  
 GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search  
```

Zie voor voorbeelden waarin wordt beschreven hoe u aangepaste zoekactie bronnen instelt voor de [zelfstudie](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/tutorials/custom-search-web-page). Voor meer informatie over kopteksten, parameters, markt codes, antwoord objecten, fouten, enz., Zie de [Bing-API van zoekservice aangepaste v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference) verwijzing.

## <a name="custom-search-response-json"></a>Aangepaste zoekactie antwoord JSON
Een aangepaste zoekaanvraag retourneert resultaten als JSON-objecten, Zie [antwoord objecten](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#response-objects). 

## <a name="custom-autosuggest"></a>Voor Automatische suggestie van aangepaste
De aangepaste voor Automatische suggestie API kunt u een gedeeltelijke zoekterm query verzenden naar Bing en ophalen van een lijst met voorgestelde query's die u kunt configureren. Met Automatische suggestie van aangepaste suggesties geretourneerd door de API toevoegen en optioneel aangeven of suggesties gegenereerd door Bing bevatten.

## <a name="custom-autosuggest-endpoint"></a>Eindpunt voor Automatische suggestie van aangepaste
Als aangepaste Querysuggesties, verzenden naar een GET-aanvraag:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions
```  

Zie voor meer informatie over het definiëren van aangepaste suggesties [aangepaste zoeksuggesties definiëren](define-custom-suggestions.md).

## <a name="custom-image-search"></a>Aangepaste installatiekopie zoeken
De aangepaste installatiekopie Search API kunt u een zoekopdracht naar Bing te verzenden en een lijst met relevante afbeeldingen terughalen van uw aangepaste zoekactie-exemplaar.

## <a name="custom-image-search-endpoint"></a>Aangepaste installatiekopie zoeken eindpunt
Om aan te vragen installatiekopieën van het exemplaar van uw aangepaste zoekactie, een GET-aanvraag te verzenden naar de volgende URL:

```
https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/images/search
```

Zie voor meer informatie over het configureren van een aangepaste Zoekexemplaar [configureren van uw aangepaste zoekervaring](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/define-your-custom-view).

## <a name="next-steps"></a>Volgende stappen
De **Bing** API's ondersteunen zoekacties die resultaten volgens hun type retourneren. Alle zoeken eindpunten retourneren resultaten als JSON-antwoord-objecten.  Alle eindpunten ondersteuning query's die een specifieke taal en/of de locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de pagina's met naslaginformatie voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor voorbeelden van basic-aanvragen via de Search-API voor aangepaste [aangepaste Search Quick-Start](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/)