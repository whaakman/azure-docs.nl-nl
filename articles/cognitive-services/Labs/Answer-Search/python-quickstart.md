---
title: Python-Quick Start voor Microsoft cognitieve Services, Project antwoord zoeken | Microsoft Docs
description: Voorbeeld van Python aan de slag met zoeken in de Project-antwoord, cognitieve Microsoft-Services in Azure.
services: cognitive-services
author: mikedodaro
ms.service: cognitive-services
ms.technology: project-answer-search
ms.topic: article
ms.date: 04/13/2018
ms.author: rosh, v-gedod
ms.openlocfilehash: 9cb5406c616ed8e96d73c00c788a0d20f66dcabd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345257"
---
# <a name="project-answer-search-python-quickstart"></a>Project antwoord zoeken Python Quick Start

Het volgende voorbeeld met Python maakt en verzendt een aanvraag voor meer informatie over 'Steen Gibraltar'.

## <a name="prerequisites"></a>Vereisten

Ophalen van een toegangssleutel voor de gratis proefversie [cognitieve Services Labs](https://aka.ms/answersearchsubscription)

In dit voorbeeld maakt gebruik van Python 3.6.4

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

````
## <a name="next-steps"></a>Volgende stappen
- [Quick Start C#](c-sharp-quickstart.md)
- [Java-Quick Start](java-quickstart.md)
- [Knooppunt Quick Start](node-quickstart.md)