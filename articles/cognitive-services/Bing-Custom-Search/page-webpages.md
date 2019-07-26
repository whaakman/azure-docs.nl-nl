---
title: Pagina via beschik bare webpagina's-Bing Aangepaste zoekopdrachten
titleSuffix: Azure Cognitive Services
description: Laat zien hoe u door alle webpagina's kunt bladeren die Bing Aangepaste zoekopdrachten kunnen retour neren.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: maheshb
ms.openlocfilehash: 13b4cef624c636b8935897338badf3349f27c7f5
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68405037"
---
# <a name="paging-webpages"></a>Webpagina's pagineren 

Wanneer u de Aangepaste zoekopdrachten-API aanroept, retourneert Bing een lijst met resultaten. De lijst is een subset van het totale aantal resultaten dat relevant kan zijn voor de query. Als u het geschatte totale aantal beschik bare resultaten wilt ophalen, opent u het veld [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#totalestimatedmatches) van het antwoord object.  
  
In het volgende voor beeld `totalEstimatedMatches` wordt het veld weer gegeven dat een webantwoord bevat.  
  
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
  
Als u door de beschik bare webpagina's wilt bladeren, gebruikt u de query parameters [aantal](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#count) en [Offset](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-custom-search-api-v7-reference#offset) .  
  
De `count` para meter bepaalt het aantal resultaten dat moet worden geretourneerd in het antwoord. Het maximum aantal resultaten dat u in het antwoord kunt aanvragen is 50. De standaard waarde is 10. Het daad werkelijke aantal dat kan worden geleverd, kan kleiner zijn dan het aangevraagde nummer.

De `offset` para meter bepaalt het aantal resultaten dat moet worden overgeslagen. De `offset` is op nul gebaseerd en moet kleiner zijn dan (`totalEstimatedMatches` - `count`).  
  
Als u 15 webpagina's per pagina wilt weer geven, stelt `count` u in op 15 en `offset` 0 om de eerste pagina met resultaten op te halen. Voor elke volgende pagina verhoogt `offset` u 15 (bijvoorbeeld 15, 30).  
  
Hieronder ziet u een voor beeld waarin 15 webpagina's worden aangevraagd, te beginnen bij offset 45.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&count=15&offset=45&mkt=en-us&customConfig=123456 HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>
Host: api.cognitive.microsoft.com  
```  

Als de standaard `count` waarde voor uw implementatie werkt, hoeft u alleen de `offset` query parameter op te geven.  
  
```  
GET https://api.cognitive.microsoft.com/bingcustomsearch/v7.0/search?q=sailing+dinghies&offset=45&mkt=en-us&customConfig=123456 HTTP/1.1  
Ocp-Apim-Subscription-Key: <subscription ID>  
Host: api.cognitive.microsoft.com  
```  

> [!NOTE]
> Het `TotalEstimatedAnswers` veld is een schatting van het totale aantal Zoek resultaten dat u voor de huidige query kunt ophalen.  Wanneer u en `count` `offset` para meters instelt `TotalEstimatedAnswers` , kan het aantal worden gewijzigd. 

