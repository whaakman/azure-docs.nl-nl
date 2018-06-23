---
title: Het bladeren door de beschikbare installatiekopieën | Microsoft Docs
description: Laat zien hoe pagina via alle installatiekopieën die Bing kan retourneren.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 3C8423F8-41E0-4F89-86B6-697E840610A7
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: a74ee817e84be5bb563c5fdaf25afc1dc14732e5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344409"
---
# <a name="paging-results"></a>Resultaten pagineren

Wanneer u de installatiekopie zoeken-API aanroept, retourneert Bing een lijst met resultaten. De lijst is een subset van het totale aantal resultaten die relevant voor de query zijn. Als u het geschatte aantal beschikbare resultaten, toegang krijgen tot het antwoord-object [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#totalestimatedmatches) veld.  
  
Het volgende voorbeeld wordt de `totalEstimatedMatches` veld dat een antwoord installatiekopieën omvat.  
  
```json
{
    "_type" : "Images",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8...",
    "totalEstimatedMatches" : 838,
    "value" : [...]  
}  
```  
  
Pagina via de beschikbare installatiekopieën, gebruiken de [aantal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#count) en [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offset) queryparameters.  
  
De `count` parameter geeft u het aantal resultaten te retourneren in het antwoord. Het maximum aantal resultaten dat u in het antwoord aanvragen mogelijk is 150. De standaardwaarde is 35. Het werkelijke aantal geleverde mogelijk minder dan aangevraagd.

De `offset` parameter geeft u het aantal resultaten om over te slaan. De `offset` is gebaseerd op nul zijn en moet kleiner zijn dan (`totalEstimatedMatches` - `count`).  
  
Als u 20 afbeeldingen per pagina weergegeven wilt, stelt u `count` 20 en `offset` op 0 om op te halen van de eerste pagina van de resultaten. Hieronder ziet u een voorbeeld van 20 afbeeldingen beginnen bij verschuiving 40 aanvragen.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Als de standaard `count` waarde werkt voor uw implementatie, moet u alleen om op te geven de `offset` queryparameter.  
  
```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  
  
U zou verwachten dat als u installatiekopieën van 35 pagina tegelijk, u stelt de `offset` parameterwaarde 0 op de eerste aanvraag opvragen en verhoog vervolgens `offset` door 35 bij elke volgende aanvraag. Sommige van de resultaten in het volgende antwoord zijn echter duplicaten zijn van het vorige antwoord. Bijvoorbeeld kunnen de eerste twee afbeeldingen in het antwoord niet hetzelfde als de laatste twee installatiekopieën van het vorige antwoord.

Om te voorkomen dubbele resultaten, gebruiken de [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#nextoffset) veld van de `Images` object. De `nextOffset` veld vertelt u de `offset` moet worden gebruikt voor de volgende aanvraag. Bijvoorbeeld, als u pagina 30 installatiekopieën op een tijdstip wilt, stelt u `count` tot en met 30 en `offset` in 0 op de eerste aanvraag. In de volgende aanvraag, stelt u `count` tot en met 30 en `offset` aan de waarde van het vorige antwoord `nextOffset`. We raden een stack van de vorige verschuivingen onderhoud en de meest recente verschijnen voor de vorige pagina.

> [!NOTE]
> Paginering geldt alleen voor het zoeken naar afbeelding (installatiekopieën/search) en niet op de installatiekopie insights of trends afbeeldingen (afbeeldingen/trends).