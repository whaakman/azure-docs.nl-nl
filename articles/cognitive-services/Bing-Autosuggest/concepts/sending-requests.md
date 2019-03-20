---
title: Verzenden van aanvragen naar de Bing Automatische suggestie-API
titlesuffix: Azure Cognitive Services
description: Meer informatie over het verzenden van aanvragen naar Bing Automatische suggestie-API.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-autosuggest
ms.topic: quickstart
ms.date: 02/20/2019
ms.author: scottwhi
ms.openlocfilehash: 597ef48fd7499a9d33b214b182d6dd1354756cdf
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57011580"
---
# <a name="sending-requests-to-the-bing-autosuggest-api"></a>Het verzenden van aanvragen naar de Bing Automatische suggestie-API.

Als uw toepassing query's verstuurt naar een van de API's van Bing Search, kunt u de Bing Automatische suggesties-API gebruiken om de ervaring van uw gebruikers te verbeteren. De Bing Automatische suggesties-API retourneert een lijst met voorgestelde query's op basis van de gedeeltelijke queryreeks in het zoekvak. Als de tekens worden ingevoerd in het zoekvak in uw toepassing, kunt u suggesties weergeven in een vervolgkeuzelijst. Gebruik dit artikel voor meer informatie over het verzenden van aanvragen naar deze API.

## <a name="bing-autosuggest-api-endpoint"></a>Bing Automatische suggestie-API-eindpunt

De **Bing Automatische suggestie-API** bevat één eindpunt die resulteert in een lijst met voorgestelde query's uit een gedeeltelijke zoekterm.

Als u de voorgestelde query's met behulp van de API voor Bing, verzendt een `GET` aanvraag naar het volgende eindpunt. De kop- en URL-parameters gebruiken om te definiëren verder specificaties.

**Eindpunt:** Retourneert zoeksuggesties als JSON-resultaten die relevant voor de invoer van de gebruiker gedefinieerd zijn door `?q=""`.

```http
GET https://api.cognitive.microsoft.com/bing/v7.0/Suggestions 
```

Voor meer informatie over kopteksten, parameters, markt codes, antwoordobjecten, fouten, enz., Zie de [Bing Automatische suggestie-API versie 7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference) verwijzing.

De **Bing** API's ondersteunen zoekacties die retourneren van resultaten op basis van hun type. Alle eindpunten voor zoeken worden resultaten geretourneerd als JSON-antwoord-objecten.
Alle eindpunten ondersteuning voor query's die een specifieke taal en/of locatie door lengtegraad, breedtegraad en zoeken radius retourneren.

Zie de referentiepagina's voor elk type voor volledige informatie over de parameters die worden ondersteund door elk eindpunt.
Zie voor meer voorbeelden van basic met behulp van de Automatische suggestie-API-aanvragen [Automatische suggestie-snelstartgidsen](https://docs.microsoft.com/azure/cognitive-services/Bing-Autosuggest).

## <a name="bing-autosuggest-api-requests"></a>Bing Automatische suggestie-API-aanvragen

> [!NOTE]
> Aanvragen voor de Automatische suggestie-API moeten de HTTPS-protocol gebruiken.

Het is raadzaam dat alle aanvragen afkomstig zijn van een server. De sleutel te distribueren als onderdeel van een clienttoepassing biedt meer kans kwaadwillende externe toegang. Daarnaast biedt aanroepen van een server een centraal punt voor de upgrade voor toekomstige updates.

De aanvraag moet de queryparameter [q](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query) bevatten, met daarin de gedeeltelijke zoekterm van de gebruiker. Hoewel dit optioneel is, moet de aanvraag ook de queryparameter [mkt](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#mkt) bevatten. Hiermee geeft u de markt aan waarvan de resultaten afkomstig moeten zijn. Zie [Queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#query-parameters) voor een lijst met optionele queryparameters. Alle waarden van queryparameter moeten als een URL zijn gecodeerd.

De aanvraag moet de header [Ocp-Apim-Subscription-Key](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#subscriptionkey) bevatten. Hoewel dit optioneel is, wordt u aangeraden ook altijd deze headers op te geven:

- [User-Agent](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#useragent)
- [X-MSEdge-ClientID](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientid)
- [X-Search-ClientIP](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#clientip)
- [X-Search-Location](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#location)

De headers ClientIP en Location zijn belangrijk voor het retourneren van locatiespecifieke inhoud.

Zie [Headers](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v5-reference#headers) voor een lijst met alle aanvraag- en antwoordheaders.

> [!NOTE]
> Wanneer u de Automatische suggestie-API vanuit JavaScript aanroepen, kunnen u de ingebouwde beveiligingsfuncties van uw browser toegang krijgen tot de waarden van deze headers niet.

U kunt dit oplossen kunt u de Automatische suggestie-API-aanvraag via een proxy CORS. Het antwoord van een dergelijke proxy heeft een `Access-Control-Expose-Headers`-header waardoor antwoordheaders worden opgenomen in de whitelist en beschikbaar gemaakt voor JavaScript.

Het is eenvoudig te installeren van een CORS-proxy om toe te staan onze [zelfstudie app](../tutorials/autosuggest.md) voor toegang tot de optionele client-headers. Als u [Node.js](https://nodejs.org/en/download/) nog niet hebt, moet u dit eerst installeren. Voer vervolgens de volgende opdracht in een opdrachtprompt in.

    npm install -g cors-proxy-server

Vervolgens het Bing Automatische suggestie-API-eindpunt in het HTML-bestand te wijzigen:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Start ten slotte de CORS-proxy met de volgende opdracht:

    cors-proxy-server

Laat het opdrachtvenster geopend terwijl u de zelfstudie-app gebruikt. Als u het venster sluit, wordt de proxy gestopt. In de uitbreidbare sectie met HTTP-headers onder de zoekresultaten ziet u nu (onder andere) de `X-MSEdge-ClientID`-header en kunt u controleren of deze voor elke aanvraag gelijk is.

Alle voorgestelde query-parameters en headers bevatten aanvragen. 

In het volgende voorbeeld ziet u een aanvraag die de voorgestelde queryreeksen voor *sail* retourneert.

> ```http
> GET https://api.cognitive.microsoft.com/bing/v7.0/suggestions?q=sail&mkt=en-us HTTP/1.1
> Ocp-Apim-Subscription-Key: 123456789ABCDE
> X-MSEdge-ClientIP: 999.999.999.999
> X-Search-Location: lat:47.60357;long:-122.3295;re:100
> X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>
> Host: api.cognitive.microsoft.com
> ```

Als dit de eerste keer is van u een van de Bing-API's aanroept, moet u de header met de client-id weglaten. Voeg de header met de client-id alleen toe als u eerder een Bing-API hebt aangeroepen en Bing een client-id heeft geretourneerd voor de combinatie van gebruiker en apparaat.

Hieronder wordt de reactie op de vorige aanvraag weergegeven. De reactie bevat een websuggestiegroep met een lijst met suggesties voor zoekquery's. Elke suggestie bevat de velden `displayText`, `query` en `url`.

Het veld `displayText` bevat de voorgestelde query die u gebruikt voor het vullen van de vervolgkeuzelijst van het zoekvak. U moet alle suggesties uit het antwoord weergeven, en in de opgegeven volgorde.  

Als de gebruiker een query in de vervolgkeuzelijst selecteert, kunt u deze gebruiken voor het aanroepen van een van de [Bing zoeken-API's](https://docs.microsoft.com/azure/cognitive-services/bing-web-search/bing-api-comparison?toc=%2Fen-us%2Fazure%2Fcognitive-services%2Fbing-autosuggest%2Ftoc.json&bc=%2Fen-us%2Fazure%2Fbread%2Ftoc.json) en zelf de resultaten weergeven of de gebruiker doorsturen naar de pagina met Bing resultaten met behulp van de geretourneerde `url` veld. Het volgende voorbeeld wordt de Bing webzoekopdrachten-API.

```json
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

- [Wat is Bing Automatische suggesties?](../get-suggested-search-terms.md)
- [Naslaghandleiding Bing Automatische suggesties-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-autosuggest-api-v7-reference)
- [Voorgestelde zoektermen ophalen uit de Automatische suggestie-API](get-suggestions.md)
