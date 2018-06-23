---
title: API-Quick Start voor Automatische suggestie | Microsoft Docs
description: Laat zien hoe u aan de slag met de API voor Automatische suggestie van Bing.
services: cognitive-services
author: swhite-msft
manager: ehansen
ms.assetid: 1482E781-7352-4A3F-B1D5-B896381348C4
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: article
ms.date: 04/15/2017
ms.author: scottwhi
ms.openlocfilehash: a7b54a1fb0b7c76eb72097357a6b51aa02e6e2fd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345473"
---
# <a name="making-your-first-autosuggest-query"></a>Uw eerste Automatische suggestie query maken

Voordat u uw eerste aanroep aanbrengen kunt, moet u een abonnement cognitieve Services code. Als u een sleutel, Zie [cognitieve Services probeer](https://azure.microsoft.com/try/cognitive-services/?api=autosuggest-api).

Als u de zoekresultaten, zou u een GET-aanvraag verzenden naar het volgende eindpunt:

```http
https://api.cognitive.microsoft.com/bing/v5.0/Suggestions
```

> [!NOTE]
> V7 Preview-eindpunt:
>
> ```http
> https://api.cognitive.microsoft.com/bing/v7.0/Suggestions
> ```

De aanvraag moet de HTTPS-protocol gebruiken.

We raden aan dat alle aanvragen zijn afkomstig uit een server. Het distribueren van de sleutel als onderdeel van een clienttoepassing biedt meer mogelijkheden om een schadelijke van derden om deze te openen. Het aanroepen van een server biedt ook een centraal punt voor de upgrade voor toekomstige versies van de API.

De aanvraag moet opgeven de [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query) queryparameter waarin de gebruiker gedeeltelijke zoekterm. Hoewel dit optioneel is, geef de aanvraag ook de [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#mkt) queryparameter waarmee de markt waar u de resultaten uit te komen. Zie voor een lijst van optionele queryparameters [queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query-parameters). Alle parameterwaarden moet URL zijn gecodeerd.

De aanvraag moet opgeven de [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#subscriptionkey) header. Hoewel dit optioneel, wordt u aangeraden ook de volgende headers opgeven:

- [Gebruikersagent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#useragent)
- [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientid)
- [X-Search-client-IP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientip)
- [X-Search-locatie](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#location)

De client-IP-adres en de locatie-headers zijn belangrijk voor het retourneren van locatie op de hoogte inhoud.

Zie voor een lijst van alle aanvraag- en reactieheaders [Headers](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#headers).

## <a name="the-request"></a>De aanvraag

De aanvraag moet bevatten alle voorgestelde queryparameters en -koppen. Telkens wanneer die de gebruiker een nieuwe teken in het zoekvak typt, zou u deze API aanroepen. De volledigheid van de query-tekenreeks gevolgen de toepasselijkheid van de voorgestelde query van de voorwaarden die de API retourneert. Hoe meer de queryreeks meer relevante dat de lijst met voorgestelde querytermen zijn voltooid. Bijvoorbeeld, de suggesties die de API kan retourneren voor *s* waarschijnlijk minder dan de query's die het resultaat voor relevante *Zeilsloepen*. 

Het volgende voorbeeld ziet u een aanvraag die als resultaat geeft de voorgestelde queryreeksen voor *voeren*.

```http
GET https://api.cognitive.microsoft.com/bing/v5.0/suggestions?q=sail&mkt=en-us HTTP/1.1  
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com  
```

> [!NOTE]
> V7 Preview-aanvraag:

> ```http
> GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE
> X-MSEdge-ClientIP: 999.999.999.999
> X-Search-Location: lat:47.60357;long:-122.3295;re:100
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
> Host: api.cognitive.microsoft.com
> ```

Als dit de eerste keer aanroepen van een van de Bing-API's, bevatten geen koptekst van de client-ID. Alleen de koptekst van de client-ID bevatten als u eerder een Bing-API hebt genoemd en Bing een client-ID voor de gebruiker en apparaat combinatie geretourneerd.

Hieronder ziet u het antwoord op de vorige aanvraag. Het antwoord bevat een web suggestie-groep die een lijst met query zoeksuggesties bevat. Elke suggestie bevat een `displayText`, `query`, en `url` veld.

De `displayText` veld bevat de voorgestelde query die u gebruiken wilt voor het vullen van de vervolgkeuzelijst het zoekvak. U moet alle suggesties die het antwoord bevat, worden weergegeven in de opgegeven volgorde.  

Als de gebruiker een query in de vervolgkeuzelijst selecteert, mag u de query-tekenreeks in de `query` veld aan te roepen de [Bing zoeken-API](../bing-web-search/search-the-web.md) en de resultaten weergeven zelf. Of u kunt de URL in de `url` resultatenpagina veld voor het verzenden van de gebruiker de Bing-zoekopdracht.

```  
BingAPIs-TraceId: 76DD2C2549B94F9FB55B4BD6FEB6AC
X-MSEdge-ClientID: 1C3352B306E669780D58D607B96869
BingAPIs-Market: en-US

{
    "_type" : "Suggestions",
    "queryContext" : {
        "originalQuery" : "sail"
    },
    "suggestionGroups" : [{
        "name" : "Web",
        "searchSuggestions" : [{
            "url" : "https:\/\/www.bing.com\/search?q=sailing+lessons+seattle&FORM=USBAPI",
            "displayText" : "sailing lessons seattle",
            "query" : "sailing lessons seattle",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+moon+news&FORM=USBAPI",
            "displayText" : "sailor moon news",
            "query" : "sailor moon news",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailor+jack%27s+lincoln+city&FORM=USBAPI",
            "displayText" : "sailor jack's lincoln city",
            "query" : "sailor jack's lincoln city",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailing+anarchy&FORM=USBAPI",
            "displayText" : "sailing anarchy",
            "query" : "sailing anarchy",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale&FORM=USBAPI",
            "displayText" : "sailboats for sale",
            "query" : "sailboats for sale",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailstn.mylabsplus.com&FORM=USBAPI",
            "displayText" : "sailstn.mylabsplus.com",
            "query" : "sailstn.mylabsplus.com",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailusfood&FORM=USBAPI",
            "displayText" : "sailusfood",
            "query" : "sailusfood",
            "searchKind" : "WebSearch"
        },
        {
            "url" : "https:\/\/www.bing.com\/search?q=sailboats+for+sale+seattle&FORM=USBAPI",
            "displayText" : "sailboats for sale seattle",
            "query" : "sailboats for sale seattle",
            "searchKind" : "WebSearch"
        }]
    }]
}
```

## <a name="next-steps"></a>Volgende stappen

De API uitproberen. Ga naar [API testen van de Console voor Automatische suggestie](https://dev.cognitive.microsoft.com/docs/services/56c7694ecf5ff801a090fbd1/operations/56c769a2cf5ff801a090fbd2).

Zie voor meer informatie over het gebruiken van de objecten antwoord [voorgestelde zoektermen ophalen](./get-suggested-search-terms.md).
