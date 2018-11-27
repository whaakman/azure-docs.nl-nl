---
title: 'Snelstartgids: Bing Spellingcontrole-API, Python'
titlesuffix: Azure Cognitive Services
description: Haal informatie en codevoorbeelden op om u te helpen snel aan de slag te gaan met de Bing Spellingcontrole-API.
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: quickstart
ms.date: 09/14/2017
ms.author: aahi
ms.openlocfilehash: d77fd2637e7516dbae361518c4b729ca1217e6e6
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52160209"
---
# <a name="quickstart-for-bing-spell-check-api-with-python"></a>Snelstartgids voor Bing Spellingcontrole-API met Python 

In dit artikel leest u hoe u de [Bing Spellingcontrole-API](https://azure.microsoft.com/services/cognitive-services/spell-check/) gebruikt in combinatie met Python. De Spellingcontrole-API retourneert een lijst met woorden die niet worden herkend, samen met voorgestelde vervangingen. Normaal gesproken zou u tekst naar deze API sturen en vervolgens de voorgestelde vervangingen in de tekst doorvoeren of deze weergeven aan de gebruikers van uw toepassing, zodat ze zelf kunnen beslissen of de vervangingen moeten worden doorgevoerd. Dit artikel laat zien hoe een aanvraag met de tekst "Hollo, wrld!" wordt verzonden. De voorgestelde vervangingen zijn "Hello" en "world".

## <a name="prerequisites"></a>Vereisten

U hebt [Python 3.x](https://www.python.org/downloads/) nodig om deze code uit te voeren.

U moet een [Cognitive Services-API-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met **Bing Spellingcontrole-API voor Bing versie 7** hebben. De [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/#lang) is voldoende voor deze snelstartgids. U hebt de toegangssleutel nodig die wordt verstrekt bij het activeren van uw gratis proefversie of u gebruikt de sleutel van een betaald abonnement vanuit uw Azure-dashboard.

## <a name="get-spell-check-results"></a>Resultaten van spellingcontrole ophalen

1. Maak een nieuw Python-project in uw favoriete IDE.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `subscriptionKey` door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

```python
import http.client, urllib.parse, json

text = 'Hollo, wrld!'

data = {'text': text}

# NOTE: Replace this example key with a valid subscription key.
key = 'ENTER KEY HERE'

host = 'api.cognitive.microsoft.com'
path = '/bing/v7.0/spellcheck?'
params = 'mkt=en-us&mode=proof'

headers = {'Ocp-Apim-Subscription-Key': key,
'Content-Type': 'application/x-www-form-urlencoded'}

# The headers in the following example 
# are optional but should be considered as required:
#
# X-MSEdge-ClientIP: 999.999.999.999  
# X-Search-Location: lat: +90.0000000000000;long: 00.0000000000000;re:100.000000000000
# X-MSEdge-ClientID: <Client ID from Previous Response Goes Here>

conn = http.client.HTTPSConnection(host)
body = urllib.parse.urlencode (data)
conn.request ("POST", path + params, body, headers)
response = conn.getresponse ()
output = json.dumps(json.loads(response.read()), indent=4)
print (output)
```

**Antwoord**

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u kunt zien in het volgende voorbeeld: 

```json
{
   "_type": "SpellCheck",
   "flaggedTokens": [
      {
         "offset": 0,
         "token": "Hollo",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "Hello",
               "score": 0.9115257530801
            },
            {
               "suggestion": "Hollow",
               "score": 0.858039839213461
            },
            {
               "suggestion": "Hallo",
               "score": 0.597385084464481
            }
         ]
      },
      {
         "offset": 7,
         "token": "wrld",
         "type": "UnknownToken",
         "suggestions": [
            {
               "suggestion": "world",
               "score": 0.9115257530801
            }
         ]
      }
   ]
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie over Bing Spellingcontrole](../tutorials/spellcheck.md)

## <a name="see-also"></a>Zie ook

- [Overzicht van Bing Spellingcontrole](../proof-text.md)
- [Referentie voor de Bing Spellingcontrole-API v7](https://docs.microsoft.com/rest/api/cognitiveservices/bing-spell-check-api-v7-reference)
