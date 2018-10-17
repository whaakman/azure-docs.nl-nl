---
title: 'Quickstart: Bing Spellingcontrole-API'
titlesuffix: Azure Cognitive Services
description: Hier kunt u zien hoe u aan de slag kunt gaan met de Bing Spellingcontrole-API.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: quickstart
ms.date: 06/21/2016
ms.author: scottwhi
ms.openlocfilehash: 29ee7cb4ee648d20b425939553ba31cd9ac150f0
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48804188"
---
# <a name="quickstart-your-first-spell-check-request"></a>Quickstart: Uw eerste spellingcontroleaanvraag

Voordat u uw eerste aanroep kunt versturen, moet u een abonnementssleutel van Cognitive Services opvragen. Zie [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=spellcheck-api) voor meer informatie.

Om een stuk tekst te controleren op spel- en grammaticafouten, verzendt u een GET-aanvraag naar het volgende eindpunt:  
  
```
https://api.cognitive.microsoft.com/bing/v5.0/spellcheck
```

> [!NOTE]
> Eindpunt in de preview van versie 7:
> 
> ```
> https://api.cognitive.microsoft.com/bing/v7.0/spellcheck
> ```  
  
In de aanvraag moet u het HTTPS-protocol gebruiken.

Het is raadzaam dat alle aanvragen afkomstig zijn van een server. Het distribueren van de sleutel als onderdeel van een clienttoepassing biedt een kwaadwillende gebruiker namelijk meer mogelijkheden om de sleutel te onderscheppen. Als u ervoor kiest om alle aanroepen via een server te laten lopen, beschikt u bovendien over een centraal upgradepunt voor toekomstige versies van de API.

De aanvraag moet de queryparameter [text](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#text) bevatten, met daarin het te controleren stuk tekst. Hoewel dat optioneel is, moet in de aanvraag ook de [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#mkt)-queryparameter worden opgegeven, ter aanduiding van de markt waar u de resultaten vandaan wilt halen. Zie [Queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#query-parameters) voor een lijst met optionele queryparameters, zoals `mode`. Alle waarden van queryparameter moeten als een URL zijn gecodeerd.  
  
De aanvraag moet de header [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#subscriptionkey) bevatten. Hoewel dit optioneel is, wordt u aangeraden ook altijd deze headers op te geven:  
  
-   [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientid)  
-   [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#clientip)  
-   [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#location)  

Zie [Headers](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v5-reference#headers) voor een lijst van alle aanvraag- en antwoordheaders.

## <a name="the-request"></a>De aanvraag

Hieronder ziet u een aanvraag waarin alle voorgestelde queryparameters en headers zijn opgenomen. Als dit de eerste keer is van u een van de Bing-API's aanroept, moet u de header met de client-id weglaten. Voeg de client-id alleen toe als u eerder een Bing-API hebt aangeroepen en Bing een client-id heeft geretourneerd voor de combinatie van gebruiker en apparaat. 
  
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
> Aanvraag in de preview van versie 7:

> ```  
> GET https://api.cognitive.microsoft.com/bing/v7.0/spellcheck?text=when+its+your+turn+turn,+john,+come+runing&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE  
> X-MSEdge-ClientIP: 999.999.999.999  
> X-Search-Location: lat:47.60357;long:-122.3295;re:100  
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
> Host: api.cognitive.microsoft.com  
> ```  

Hieronder ziet u het antwoord op de vorige aanvraag. Het voorbeeld bevat ook de Bing-specifieke antwoordheaders.

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

Probeer de API uit. Ga naar [Testconsole voor Spellingcontrole-API](https://dev.cognitive.microsoft.com/docs/services/56e73033cf5ff80c2008c679/operations/57855119bca1df1c647bc358). 

Zie [Tekenreeksen voor spellingcontrole](./proof-text.md) voor meer informatie over het gebruiken van de antwoordobjecten.

