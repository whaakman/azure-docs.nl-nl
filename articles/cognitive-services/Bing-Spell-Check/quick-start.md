---
title: Snel starten spellen controleren Bing-API | Microsoft Docs
description: Laat zien hoe u aan de slag met de Bing-API voor het controleren van spellen.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: AF8EB1F0-386D-4555-9354-735611435F04
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: article
ms.date: 06/21/2016
ms.author: scottwhi
ms.openlocfilehash: cae8353e5be6e70eca90e5995b29b6774fc7d6a9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344447"
---
# <a name="your-first-spell-check-request"></a>Uw eerste spellingcontrole aanvraag

Voordat u uw eerste aanroep aanbrengen kunt, moet u een abonnement cognitieve Services code. Als u een sleutel, Zie [cognitieve Services probeer](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api).

Als u wilt controleren in een tekenreeks voor de spelling en grammaticafouten, zou u een GET-aanvraag verzenden naar het volgende eindpunt:  
  
```
https://api.cognitive.microsoft.com/bing/v5.0/spellcheck
```

> [!NOTE]
> V7 Preview-eindpunt:
> 
> ```
> https://api.cognitive.microsoft.com/bing/v7.0/spellcheck
> ```  
  
De aanvraag moet de HTTPS-protocol gebruiken.

We raden aan dat alle aanvragen zijn afkomstig uit een server. Het distribueren van de sleutel als onderdeel van een clienttoepassing biedt meer mogelijkheden om een schadelijke van derden om deze te openen. Het aanroepen van een server biedt ook een centraal punt voor de upgrade voor toekomstige versies van de API.

De aanvraag moet opgeven de [tekst](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#text) queryparameter die de teksttekenreeks bewijs bevat. Hoewel dit optioneel is, geef de aanvraag ook de [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#mkt) queryparameter waarmee de markt waar u de resultaten uit te komen. Query voor een lijst met optionele parameters zoals `mode`, Zie [queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#query-parameters). Alle parameterwaarden moet URL zijn gecodeerd.  
  
De aanvraag moet opgeven de [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#subscriptionkey) header. Hoewel dit optioneel, wordt u aangeraden ook de volgende headers opgeven:  
  
-   [Gebruikersagent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientid)  
-   [X-Search-client-IP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientip)  
-   [X-Search-locatie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#location)  

Zie voor een lijst van alle aanvraag- en reactieheaders [Headers](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#headers).

## <a name="the-request"></a>De aanvraag

Hieronder ziet u een aanvraag waarin alle voorgestelde queryparameters en -koppen. Als dit de eerste keer aanroepen van een van de Bing-API's, bevatten geen koptekst van de client-ID. Alleen de client-ID bevatten als u eerder een Bing-API hebt genoemd en Bing een client-ID voor de gebruiker en apparaat combinatie geretourneerd. 
  
```  
GET https://api.cognitive.microsoft.com/bing/v5.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

> [!NOTE]
> V7 Preview-aanvraag:

> ```  
> GET https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE  
> X-MSEdge-ClientIP: 999.999.999.999  
> X-Search-Location: lat:47.60357;long:-122.3295;re:100  
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
> Host: api.cognitive.microsoft.com  
> ```  

Hieronder ziet u het antwoord op de vorige aanvraag. Het voorbeeld ziet ook de Bing-specifieke antwoordheaders.

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{  
    "_type" : "SpellCheck",  
    "flaggedTokens" : [{  
        "offset" : 5,  
        "token" : "its",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "it's",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 25,  
        "token" : "john",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "John",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 19,  
        "token" : "turn",  
        "type" : "RepeatedToken",  
        "suggestions" : [{  
            "suggestion" : "",  
            "score" : 1  
        }]  
    },  
    {  
        "offset" : 35,  
        "token" : "runing",  
        "type" : "UnknownToken",  
        "suggestions" : [{  
            "suggestion" : "running",  
            "score" : 1  
        }]  
    }]  
}  
```  


## <a name="next-steps"></a>Volgende stappen

De API uitproberen. Ga naar [Spell selectievakje API voor testdoeleinden Console](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358). 

Zie voor meer informatie over het gebruiken van de objecten antwoord [spellen selectievakje tekenreeksen](./proof-text.md).

