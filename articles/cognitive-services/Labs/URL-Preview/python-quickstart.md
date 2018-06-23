---
title: Python-Quick Start voor het voorbeeld in de Project-URL - cognitieve Microsoft-Services | Microsoft Docs
description: Het voorbeeldscript snel aan de slag met behulp van de evaluatieversie van de URL-Project in Microsoft cognitieve Services in Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-url-preview
ms.topic: article
ms.date: 03/29/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 78b2d83b02aa9ea32509029c7456e04e420b8572
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345285"
---
# <a name="url-preview-python-quickstart"></a>Quick Start-URL Preview Python

Het volgende Python-voorbeeld wordt een voorbeeld-Url voor de website van SwiftKey: https://swiftkey.com/en.

## <a name="prerequisites"></a>Vereisten

Ophalen van een toegangssleutel voor de gratis proefversie [cognitieve Services Labs](https://aka.ms/answersearchsubscription)

In dit voorbeeld maakt gebruik van Python 3.6.

## <a name="code-scenario"></a>Code-scenario 

De volgende code maakt een voorbeeld-URL.
Deze is geïmplementeerd in de volgende stappen uit:
1. Declareer de variabelen opgeven van het eindpunt van de host en pad.
2. Geef de URL van de query voor de preview en voeg de queryparameter.  
3. De queryparameter ingesteld.
4. Definieer de zoekfunctie die de aanvraag wordt gemaakt en wordt de *Ocp-Apim-Subscription-Key* header.
5. Stel de *Ocp-Apim-Subscription-Key* header. 
6. Controleer de verbinding en verzend de aanvraag.
7. De JSON-resultaten afdrukken.

De volledige code voor deze demo volgt:

````
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
````
## <a name="next-steps"></a>Volgende stappen
- [Quick Start C#](csharp.md)
- [Java-Quick Start](java-quickstart.md)
- [JavaScript Quick Start](javascript.md)
- [Quick Start-URL van knooppunt](node-quickstart.md)