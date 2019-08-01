---
title: 'Quickstart: Project URL Preview, Python'
titlesuffix: Azure Cognitive Services
description: Voorbeeldscript om snel aan de slag te gaan met voorbeelden van project-URL's met Python.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: quickstart
ms.date: 03/29/2018
ms.author: rosh
ms.openlocfilehash: 19a226fb580f3d4215b7c3f04f17c3f92505987e
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68697981"
---
# <a name="quickstart-url-preview-with-python"></a>Quickstart: URL Preview met Python

In het volgende voorbeeld van Python wordt er een URL-voorbeeld gemaakt voor de SwiftKey-website: https://swiftkey.com/en.

## <a name="prerequisites"></a>Vereisten

Vraag een toegangssleutel aan voor de gratis proefversie van [Cognitive Services Labs](https://labs.cognitive.microsoft.com/en-us/project-answer-search)

In dit voorbeeld wordt Python 3.6 gebruikt.

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
subscriptionKey = 'your-subscription-key'

host = 'api.labs.cognitive.microsoft.com'
path = '/urlpreview/v7.0/search'

query = 'https://SwiftKey.com'

params = '?q=' + urllib.parse.quote (query)

def get_preview ():
    headers = {'Ocp-Apim-Subscription-Key': subscriptionKey}
    conn = http.client.HTTPSConnection (host)
    conn.request ("GET", path + params, None, headers)
    response = conn.getresponse ()
    return response.read ()

result = get_preview ()
print (json.dumps(json.loads(result), indent=4))
```
## <a name="next-steps"></a>Volgende stappen
- [Snelstart voor C#](csharp.md)
- [Snelstart voor Java](java-quickstart.md)
- [Snelstart voor JavaScript](javascript.md)
- [Snelstartgids voor Node](node-quickstart.md)
