---
title: 'Quickstart: Project Answer Search, Python'
titlesuffix: Azure Cognitive Services
description: Python-voorbeeld om aan de slag te gaan met Project Answer Search.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: quickstart
ms.date: 04/13/2018
ms.author: rosh
ms.openlocfilehash: c0da596e84ac827b55affd5545c516e7623980f5
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68698065"
---
# <a name="quickstart-project-answer-search-with-python"></a>Snelstart Project Answer Search met Python

Met het volgende Python-voorbeeld wordt een aanvraag voor informatie over 'Rock of Gibraltar' gemaakt en verzonden.

## <a name="prerequisites"></a>Vereisten

Vraag een toegangssleutel aan voor de gratis proefversie van [Cognitive Services Labs](https://labs.cognitive.microsoft.com/en-us/project-answer-search)

In dit voorbeeld wordt Python 3.6.4 gebruikt

## <a name="code-scenario"></a>Codescenario 

Met de volgende code wordt een URL-voorbeeld gemaakt.
De code wordt in de volgende stappen geïmplementeerd:
1. Declareer variabelen om het eindpunt op te geven met een host en pad.
2. Geef de query-URL op waarvan u een voorbeeld wilt maken en voeg de queryparameter toe.  
3. Stel de queryparameter in.
4. Definieer de Search-functie die de aanvraag maakt en de header *Ocp-Apim-Subscription-Key* toevoegt.
5. Stel de header *Ocp-Apim-Subscription-Key* in. 
6. Maak verbinding en verzend de aanvraag.
7. Geef de JSON-resultaten weer.

Dit is de volledige code voor deze demo:

```
import http.client, urllib.parse
import json

# Replace the subscriptionKey string value with your valid subscription key.
subscriptionKey = 'YOUR-SUBSCRIPTION-KEY'

host = 'https://api.labs.cognitive.microsoft.com'
path = '/answerSearch/v7.0/search '

query = 'Rock of Gibraltar'

params = '?q=' + urllib.parse.quote (query) + '&mkt=en-us'

def get_local():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_local()
print (json.dumps(json.loads(result), indent=4))

```
## <a name="next-steps"></a>Volgende stappen
- [Snelstart voor C#](c-sharp-quickstart.md)
- [Snelstart voor Java](java-quickstart.md)
- [Snelstart voor Node](node-quickstart.md)
