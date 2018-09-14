---
title: Knowledge base publiceren, Python-snelstart - Azure Cognitive Services | Microsoft Docs
description: Een knowledge base bijwerken in Python voor QnA Maker.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.technology: qna-maker
ms.topic: quickstart
ms.date: 06/18/2018
ms.author: nolachar
ms.openlocfilehash: 0b4c08f2c0f5921275ac191f684ce0c4cbdf1eaf
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/22/2018
ms.locfileid: "43769031"
---
# <a name="publish-a-knowledge-base-in-python"></a>Een knowledge base publiceren in Python

Met de volgende code wordt een bestaande knowledge base gepubliceerd met behulp van de methode [Publish](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fe).

1. Maak een nieuw Python-project in uw favoriete IDE.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `key` door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

```python
# -*- coding: utf-8 -*-

import http.client, urllib.parse, json, time

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace this with a valid subscription key.
subscriptionKey = 'ENTER KEY HERE'

# Replace this with a valid knowledge base ID.
kb = 'ENTER ID HERE';

host = 'westus.api.cognitive.microsoft.com'
service = '/qnamaker/v4.0'
method = '/knowledgebases/'

def pretty_print (content):
# Note: We convert content to and from an object so we can pretty-print it.
    return json.dumps(json.loads(content), indent=4)

def publish_kb (path, content):
    print ('Calling ' + host + path + '.')
    headers = {
        'Ocp-Apim-Subscription-Key': subscriptionKey,
        'Content-Type': 'application/json',
        'Content-Length': len (content)
    }
    conn = http.client.HTTPSConnection(host)
    conn.request ("POST", path, content, headers)
    response = conn.getresponse ()

    if response.status == 204:
        return json.dumps({'result' : 'Success.'})
    else:
        return response.read ()

path = service + method + kb
result = publish_kb (path, '')
print (pretty_print(result))
```

## <a name="the-publish-a-knowledge-base-response"></a>Het antwoord op Een knowledge base publiceren

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u kunt zien in het volgende voorbeeld:

```json
{
  "result": "Success."
}
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API voor QnA Maker (V4)](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75ff)

[Overzicht van QnA Maker](../Overview/overview.md)