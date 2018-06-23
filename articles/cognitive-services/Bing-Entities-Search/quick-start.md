---
title: Entiteiten Search API snel aan de slag | Microsoft Docs
description: Laat zien hoe u aan de slag met de Bing entiteiten Search API.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: B206A254-B7E9-49FF-AFD5-87B1E4D6D30B
ms.service: cognitive-services
ms.component: bing-entity-search
ms.topic: article
ms.date: 07/06/2017
ms.author: scottwhi
ms.openlocfilehash: 12031d2447920c7e2d6180f35cf4fb29aa1b6150
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344455"
---
# <a name="making-your-first-entities-request"></a>Uw eerste entiteiten aanvragen maken

De entiteit Search API stuurt een zoekquery naar Bing en entiteiten en locaties omvatten de resultaten opgehaald. Plaats resultaten bevatten restaurant, hotel of andere lokale bedrijven. De query de naam van de lokale business kunt opgeven voor ruimten, of dit kunt vragen voor een lijst (bijvoorbeeld restaurant dichtbij). Resultaten van de entiteit zijn personen, plaatsen of dingen. Plaats in deze context is Sport attracties, statussen, landen, enzovoort. 

## <a name="first-steps"></a>Eerste stappen

Voordat u uw eerste aanroep aanbrengen kunt, moet u een abonnement cognitieve Services code. Als u een sleutel, Zie [cognitieve Services probeer](https://azure.microsoft.com/try/cognitive-services/?api=bing-entities-search-api). (Als de entiteiten Search API niet worden weergegeven aan de bovenkant, klikt u op de **Search** tabblad en schuif omlaag totdat u het ziet.)

## <a name="the-endpoint"></a>Het eindpunt

Om entiteit en zoekresultaten plaatsen, een aanvraag voor ophalen naar het volgende eindpunt te verzenden:  
  
```
https://api.cognitive.microsoft.com/bing/v7.0/entities
```

De aanvraag moet de HTTPS-protocol gebruiken.

We raden aan dat alle aanvragen zijn afkomstig uit een server. Het distribueren van de sleutel als onderdeel van een clienttoepassing biedt meer mogelijkheden om een schadelijke van derden om deze te openen. Het aanroepen van een server biedt ook een centraal punt voor de upgrade voor toekomstige versies van de API.

## <a name="specifying-query-parameters-and-headers"></a>Query-parameters en -koppen opgeven

De aanvraag moet opgeven de [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query) queryparameter waarin de gebruiker zoekterm. De aanvraag moet ook opgeven de [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#mkt) queryparameter waarmee de markt waar u de resultaten uit te komen. Zie voor een lijst van optionele queryparameters [queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#query-parameters). URL-codering alle queryparameters.  
  
De aanvraag moet opgeven de [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#subscriptionkey) header. Hoewel dit optioneel, wordt u aangeraden ook de volgende headers opgeven:  
  
-   [Gebruikersagent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#useragent)  
-   [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#clientid)  
-   [X-MSEdge-client-IP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#clientip)  
-   [X-Search-locatie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#location)  

De client-IP-adres en de locatie-headers zijn belangrijk voor het retourneren van locatie op de hoogte inhoud.  

Zie voor een lijst van alle aanvraag- en reactieheaders [Headers](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#headers).

## <a name="the-request"></a>De aanvraag

Hieronder ziet u een aanvraag voor entiteiten met alle voorgestelde queryparameters en -koppen. 

```  
GET https://api.cognitive.microsoft.com/bing/v7.0/entities?q=mount+rainier&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)  
X-Search-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```  

Als dit de eerste keer aanroepen van een van de Bing-API's, bevatten geen koptekst van de client-ID. Alleen de client-ID bevatten als u eerder een Bing-API hebt genoemd en Bing een client-ID voor de gebruiker en apparaat combinatie geretourneerd.

## <a name="the-response"></a>Het antwoord

Hieronder ziet u het antwoord op de vorige aanvraag. Het voorbeeld ziet ook de Bing-specifieke antwoordheaders. Zie voor meer informatie over het object response [SearchResponse](https://docs.microsoft.com/rest/api/cognitiveservices/bing-entities-api-v7-reference#searchresponse).

```
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "SearchResponse",
    "queryContext" : {
        "originalQuery" : "mount rainier"
    },
    "entities" : {
        "queryScenario" : "DominantEntity",
        "value" : [{
            "contractualRules" : [{
                "_type" : "ContractualRules\/LicenseAttribution",
                "targetPropertyName" : "description",
                "mustBeCloseToContent" : true,
                "license" : {
                    "name" : "CC-BY-SA",
                    "url" : "http:\/\/creativecommons.org\/licenses\/by-sa\/3.0\/"
                },
                "licenseNotice" : "Text under CC-BY-SA license"
            },
            {
                "_type" : "ContractualRules\/LinkAttribution",
                "targetPropertyName" : "description",
                "mustBeCloseToContent" : true,
                "text" : "en.wikipedia.org",
                "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
            },
            {
                "_type" : "ContractualRules\/MediaAttribution",
                "targetPropertyName" : "image",
                "mustBeCloseToContent" : true,
                "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
            }],
            "webSearchUrl" : "https:\/\/www.bing.com\/search?q=Mount%20Rainier...",
            "name" : "Mount Rainier",
            "image" : {
                "name" : "Mount Rainier",
                "thumbnailUrl" : "https:\/\/www.bing.com\/th?id=A21890c0e1f...",
                "provider" : [{
                    "_type" : "Organization",
                    "url" : "http:\/\/en.wikipedia.org\/wiki\/Mount_Rainier"
                }],
                "hostPageUrl" : "http:\/\/upload.wikimedia.org\/wikipedia...",
                "width" : 110,
                "height" : 110
            },
            "description" : "Mount Rainier, Mount Tacoma, or Mount Tahoma is the highest...",
            "entityPresentationInfo" : {
                "entityScenario" : "DominantEntity",
                "entityTypeHints" : ["Attraction"],
                "entityTypeDisplayHint" : "Mountain"
            },
            "bingId" : "9ae3e6ca-81ea-6fa1-ffa0-42e1d78906"
        }]
    }
}
```



## <a name="next-steps"></a>Volgende stappen

De API uitproberen. Ga naar [entiteiten zoeken API testen Console](https://dev.cognitive.microsoft.com/docs/services/7a3fb374be374859a823b79fd938cc65/). 

Zie voor meer informatie over het gebruiken van de objecten antwoord [zoeken op het Web voor entiteiten en locaties](./search-the-web.md).

Lees [Bing gebruiken en de vereisten van de weergave](./use-display-requirements.md) zodat u de regels over het gebruik van de zoekresultaten niet verbreken.
