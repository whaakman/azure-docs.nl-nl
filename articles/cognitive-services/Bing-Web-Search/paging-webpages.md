---
title: 'Pagina met zoekresultaten: Bing webzoekopdrachten-API'
titleSuffix: Azure Cognitive Services
description: Leer hoe u op de pagina met zoekresultaten van de Bing webzoekopdrachten-API.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.assetid: 26CA595B-0866-43E8-93A2-F2B5E09D1F3B
ms.service: cognitive-services
ms.subservice: bing-web-search
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: aahi
ms.openlocfilehash: e46c3c5722be6448999c4ed7f98b8babe609cd2c
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55734365"
---
# <a name="how-to-page-through-results-from-the-bing-web-search-api"></a>Pagina met resultaten van de Bing webzoekopdrachten-API

Wanneer u de webzoekopdrachten-API aanroept, retourneert Bing een lijst met resultaten. De lijst is een subset van het totale aantal resultaten die mogelijk relevant zijn voor de query. Als u de geschatte totale aantal beschikbare resultaten, toegang krijgen tot de antwoord-object [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference) veld.  

Het volgende voorbeeld wordt de `totalEstimatedMatches` veld met een Web-antwoord.  

```
{
    "_type" : "SearchResponse",
    "webPages" : {
        "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=3A43CA...",
        "totalEstimatedMatches" : 262000,
        "value" : [...]
    }
}  
```

Pagina via de webpagina's die beschikbaar is, gebruikt u de [aantal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#count) en [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#offset) queryparameters.  

De `count` parameter geeft u het aantal resultaten dat moet worden geretourneerd in het antwoord. Het maximum aantal resultaten die u in het antwoord aanvragen kunt is 50. De standaardwaarde is 10. Het werkelijke getal geleverd mogelijk lager dan aangevraagd.

De `offset` parameter geeft u het aantal resultaten dat moet worden overgeslagen. De `offset` is gebaseerd op nul en moet kleiner dan (`totalEstimatedMatches` - `count`).  

Als u weergeven van 15 webpagina's per pagina wilt, stelt u `count` tot en met 15 en `offset` op 0 om de eerste pagina van de resultaten. Voor elke volgende pagina opgehoogd `offset` 15 (bijvoorbeeld: 15, 30).  

Het volgende voorbeeld vraagt om 15 webpagina's die beginnen bij verschuiving 45.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

Als de standaard `count` waarde werkt voor uw implementatie, u hoeft alleen te geven de `offset` queryparameter.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```

De webzoekopdrachten-API retourneert resultaten die webpagina's bevatten en kunnen afbeeldingen, video's en nieuws. Wanneer u de lijst met zoekresultaten, u het wisselbestand worden de [WebAnswer](https://docs.microsoft.com/rest/api/cognitiveservices/bing-web-api-v7-reference#webanswer) antwoord en de andere antwoorden, zoals afbeeldingen of nieuws. Als u bijvoorbeeld `count` tot 50, je weer toegang krijgen 50 webpagina-resultaten, maar het antwoord kan resultaten voor de andere antwoorden ook bevatten. Het antwoord kan bijvoorbeeld 15 afbeeldingen en 4 nieuwsartikelen bevatten. Het is ook mogelijk dat de resultaten nieuws op de eerste pagina, maar niet de tweede pagina kunnen bevatten of vice versa.   

Als u opgeeft de `responseFilter` queryparameter en omvatten geen webpagina's in de filterlijst, gebruikt u niet de `count` en `offset` parameters. 

> [!NOTE]
> De `TotalEstimatedAnswers` veld is een schatting van het totale aantal zoekresultaten kunt u voor de huidige query ophalen.  Als u instelt `count` en `offset` parameters, de `TotalEstimatedAnswers` getal kan worden gewijzigd. 
