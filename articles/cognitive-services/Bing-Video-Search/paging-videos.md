---
title: Bladeren door de beschik bare Video's-Bing Video Search
titleSuffix: Azure Cognitive Services
description: Meer informatie over het bladeren door alle Video's die worden geretourneerd door de Bing Video's zoeken-API.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-video-search
ms.topic: conceptual
ms.date: 01/31/2019
ms.author: scottwhi
ms.openlocfilehash: c3570d8772734595c6707ca8103006867a8eb47a
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68500708"
---
# <a name="paging-through-video-search-results"></a>Bladeren via video-Zoek resultaten

De Bing Video's zoeken-API retourneert een subset van alle zoek resultaten die voor uw query zijn gevonden. Door deze resultaten door te bladeren met de volgende aanroepen naar de API, kunt u deze in uw toepassing ophalen en weer geven.

> [!NOTE]
> Paging is alleen van toepassing op Video's zoeken (/Videos/Search) en niet op video inzichten (/videos/details) of trending video (/videos/trending).

## <a name="total-estimated-matches"></a>Totaal geschatte overeenkomsten

Om het geschatte aantal gevonden Zoek resultaten te krijgen, gebruikt u het veld [totalEstimatedMatches](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videos-totalestimatedmatches) in het JSON-antwoord.   
  
```json  
{
    "_type" : "Videos",
    "webSearchUrl" : "https:\/\/www.bing.com\/cr?IG=81EF7545D56...",
    "totalEstimatedMatches" : 1000,
    "value" : [...]
}  
```  
  
## <a name="paging-through-videos"></a>Bladeren via Video's

Als u door de beschik bare Video's wilt bladeren, gebruikt u de para meters voor [aantal](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#count) en [Offset](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#offset) query bij het verzenden van uw aanvraag.  
  

|Parameter  |Description  |
|---------|---------|
|`count`     | Hiermee geeft u het aantal resultaten op dat moet worden geretourneerd in het antwoord. Het maximum aantal resultaten dat u in het antwoord kunt aanvragen is 100. De standaard waarde is 10. Het daad werkelijke aantal dat kan worden geleverd, kan kleiner zijn dan het aangevraagde nummer.        |
|`offset`     | Hiermee geeft u het aantal resultaten dat moet worden overgeslagen. De `offset` is op nul gebaseerd en moet kleiner zijn dan (`totalEstimatedMatches` - `count`).          |

Als u bijvoorbeeld 20 artikelen per pagina wilt weer geven, stelt `count` u in op 20 en `offset` 0 om de eerste pagina met resultaten op te halen. Voor elke volgende pagina verhoogt `offset` u de 20 (bijvoorbeeld 20, 40).  
  
In het volgende voor beeld worden 20 Video's aangevraagd, te beginnen bij offset 40.  
  
```cURL  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&count=20&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Als u de standaard waarde voor de [telling](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#count)gebruikt, hoeft u alleen de `offset` query parameter op te geven, zoals in het volgende voor beeld.  
  
```cURL  
GET https://api.cognitive.microsoft.com/bing/v7.0/videos/search?q=sailing+dinghies&offset=40&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
Host: api.cognitive.microsoft.com  
```  

Als u 35 Video's tegelijk bladert, stelt u de query parameter in `offset` op 0 bij uw eerste aanvraag en 35 verhoogt `offset` u deze op elke volgende aanvraag. Sommige resultaten in het volgende antwoord kunnen echter dubbele video resultaten van het vorige antwoord bevatten. De eerste twee Video's in een reactie kunnen bijvoorbeeld hetzelfde zijn als de laatste twee Video's van het vorige antwoord.

Als u dubbele resultaten wilt verwijderen, [](https://docs.microsoft.com/rest/api/cognitiveservices-bingsearch/bing-video-api-v7-reference#videos-nextoffset) gebruikt u het veld `Videos` nextOffset van het object.

Als u bijvoorbeeld 30 Video's per keer wilt paginaeren, kunt u instellen `count` op 30 en `offset` 0 in uw eerste aanvraag. In uw volgende aanvraag stelt u de `offset` query parameter in op de `nextOffset` waarde.

> [!NOTE]
> Het `TotalEstimatedAnswers` veld is een schatting van het totale aantal Zoek resultaten dat u voor de huidige query kunt ophalen.  Wanneer u en `count` `offset` para meters instelt `TotalEstimatedAnswers` , kan het aantal worden gewijzigd. 
  
## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Inzicht verkrijgen over video](video-insights.md)
