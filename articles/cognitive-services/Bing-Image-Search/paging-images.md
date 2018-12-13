---
title: Blader door de installatiekopieën die zijn geretourneerd door de Bing afbeeldingen zoeken-API
titleSuffix: Azure Cognitive Services
description: Blader door verschillende pagina's van installatiekopieën die zijn geretourneerd door de Bing afbeeldingen zoeken-API.
services: cognitive-services
author: swhite-msft
manager: cgonlun
ms.assetid: 3C8423F8-41E0-4F89-86B6-697E840610A7
ms.service: cognitive-services
ms.component: bing-image-search
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: scottwhi
ms.custom: seodec2018
ms.openlocfilehash: 69829066e543e5ab3badc4d1d3f77ff3c1d16ffd
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53259746"
---
# <a name="page-through-the-images-results"></a>Pagina door de resultaten van afbeeldingen

Wanneer u de afbeeldingen zoeken-API aanroept, retourneert Bing een lijst met resultaten. De lijst is een subset van het totale aantal resultaten die relevant zijn voor de query. Als u de geschatte totale aantal beschikbare resultaten, toegang krijgen tot de antwoord-object [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#totalestimatedmatches) veld.  

Het volgende voorbeeld wordt de `totalEstimatedMatches` veld dat een antwoord afbeeldingen bevat.  

```json
{
    "_type" : "Images",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=73118C8...",
    "totalEstimatedMatches" : 838,
    "value" : [...]  
}  
```  

Pagina door de beschikbare installatiekopieën, gebruikt u de [aantal](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#count) en [offset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#offset) queryparameters.  

De `count` parameter geeft u het aantal resultaten dat moet worden geretourneerd in het antwoord. Het maximum aantal resultaten die u in het antwoord aanvragen kunt is 150. De standaardwaarde is 35. Het werkelijke getal geleverd mogelijk lager dan aangevraagd.

De `offset` parameter geeft u het aantal resultaten dat moet worden overgeslagen. De `offset` is gebaseerd op nul en moet kleiner dan (`totalEstimatedMatches` - `count`).  

Als u weergeven van 20 afbeeldingen per pagina wilt, stelt u `count` tot en met 20 en `offset` op 0 om de eerste pagina van de resultaten. Hieronder ziet u een voorbeeld waarin 20 afbeeldingen beginnen bij verschuiving 40 aanvragen.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Als de standaard `count` waarde werkt voor uw implementatie, u hoeft alleen te geven de `offset` queryparameter.  

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/images/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

U zou verwachten dat als u 35 afbeeldingen op een tijdstip pagina, u stelt de `offset` queryparameter op 0 op de eerste aanvraag, en vervolgens verhogen `offset` van 35 op elke volgende aanvraag. Maar mogelijk enkele van de resultaten in het volgende antwoord duplicaten zijn van het vorige antwoord. Bijvoorbeeld, de eerste twee afbeeldingen in het antwoord mogelijk hetzelfde als de laatste twee installatiekopieën uit het vorige antwoord.

Om te voorkomen dubbele resultaten, gebruikt u de [nextOffset](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference#nextoffset) veld van de `Images` object. De `nextOffset` veld ziet u de `offset` moet worden gebruikt voor de volgende aanvraag. Bijvoorbeeld, als u 30 afbeeldingen op een tijdstip pagina wilt, stelt u `count` tot en met 30 en `offset` in 0 op de eerste aanvraag. In de volgende aanvraag, stelt u `count` tot en met 30 en `offset` aan de waarde van het vorige antwoord `nextOffset`. Vorige pagina, het is raadzaam een stapel van de vorige verschuivingen onderhouden en de meest recente Apparaatpagina.

> [!NOTE]
> Wisselgeheugengebruik geldt alleen voor afbeeldingen zoeken (/ afbeeldingen/zoeken) en niet voor inzichten in afbeeldingen of afbeeldingen-trending (/ afbeeldingen/populair).

> [!NOTE]
> De `TotalEstimatedAnswers` veld is een schatting van het totale aantal zoekresultaten kunt u voor de huidige query ophalen.  Als u instelt `count` en `offset` parameters, de `TotalEstimatedAnswers` getal kan worden gewijzigd. 
