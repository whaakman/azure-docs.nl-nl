---
title: 'Bing aangepaste zoekactie: Profiteer van tips voor Automatische suggestie van aangepaste | Microsoft Docs'
description: Hierin wordt beschreven hoe u aangepaste automatische suggestie suggesties ophalen
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.technology: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 73059038018602f7aa76377bf7c98d4658576576
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344900"
---
# <a name="get-custom-suggestions"></a>Aangepaste suggesties ophalen
Voordat u query's naar Bing aangepaste zoekactie verstuurt, roept u de aangepaste voor Automatische suggestie API om suggesties voor de zoekterm en verbeteren van de zoekfunctie. De aangepaste voor Automatische suggestie API retourneert een lijst met voorgestelde query's op basis van een gedeeltelijke queryreeks waarmee de gebruiker. Alle relevante aangepaste query-termen die u opgeeft, verschijnen vóór de suggesties dat Automatische suggestie genereert. Zie voor meer informatie [aangepaste zoeksuggesties definiëren](define-custom-suggestions.md).

## <a name="endpoint"></a>Eindpunt
Voor voorgestelde query's met de Bing aangepaste Search API, stuurt u een `GET` aanvraag voor het volgende eindpunt.

```
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/Suggestions 
```

## <a name="response-json"></a>Antwoord JSON
Het antwoord bevat een lijst met SearchAction-objecten die de voorgestelde querytermen bevatten.

```
        {  
            "displayText" : "sailing lessons seattle",  
            "query" : "sailing lessons seattle",  
            "searchKind" : "CustomSearch"  
        },  
```

Elke suggestie bevat een `displayText` en `query` veld. De `displayText` veld bevat de voorgestelde query die u gebruikt voor het vullen van de vervolgkeuzelijst het zoekvak.

Als de gebruiker een voorgestelde query in de vervolgkeuzelijst selecteert, gebruikt u de zoekterm in het `query` veld bij het aanroepen van de [Bing-API van zoekservice aangepaste](overview.md).

## <a name="throttling-requests"></a>Aanvraagbeperkingen

[!INCLUDE [cognitive-services-bing-throttling-requests](../../../includes/cognitive-services-bing-throttling-requests.md)]

## <a name="next-steps"></a>Volgende stappen

- [Uw aangepaste zoekopdracht aanroepen](./search-your-custom-view.md)
- [Configureer uw gehoste UI-mogelijkheden](./hosted-ui.md)