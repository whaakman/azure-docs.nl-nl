---
title: 'Quickstart: Een Visual Search-query maken, Node.js - Bing Visual Search'
titleSuffix: Azure Cognitive Services
description: Een afbeelding uploaden naar Bing Visual Search-API en inzichten over de afbeelding terugkrijgen.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: b13738c5bfd8fc75224bf934ae8be56e7c2edd69
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47225494"
---
# <a name="quickstart-your-first-bing-visual-search-query-in-javascript"></a>Quickstart: Uw eerste Bing Visual Search-query in JavaScript

Bing Visual Search-API retourneert informatie over een afbeelding die u opgeeft. U kunt de afbeelding opgeven met behulp van de URL van de afbeelding, een inzichttoken of door een afbeelding te uploaden. Zie [Wat is Bing Visual Search-API?](../overview.md) voor informatie over deze opties Dit artikel demonstreert het uploaden van een afbeelding. Het uploaden van een afbeelding kan handig zijn in mobiele scenario's waarbij u een foto neemt van een bekend oriëntatiepunt en er informatie over terugkrijgt. De inzichten kunnen bijvoorbeeld trivia bevatten over het oriëntatiepunt. 

Als u een lokale afbeelding uploadt, toont het volgende de formuliergegevens die u moet opnemen in de hoofdtekst van de POST. De formuliergegevens moeten de header Content-Disposition bevatten. De parameter `name` moet worden ingesteld op ‘image’ en de parameter `filename` kan op een willekeurige tekenreeks worden ingesteld. De inhoud van het formulier is het binaire bestand van de afbeelding. De maximale afbeeldingsgrootte die u kunt uploaden, is 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Dit artikel bevat een eenvoudige consoletoepassing waarmee een Bing Visual Search-API-aanvraag wordt verzonden en de JSON-zoekresultaten worden weergegeven. Hoewel deze toepassing is geschreven in JavaScript, is de API een RESTful-webservice die compatibel is met elke programmeertaal waarmee HTTP-aanvragen kunnen worden ingediend en JSON kan worden geparseerd. 

## <a name="prerequisites"></a>Vereisten

U hebt [Node.js 6](https://nodejs.org/en/download/) nodig om deze code uit te voeren.

In deze quickstart kunt u een abonnementssleutel van een [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) of een betaalde abonnementssleutel gebruiken.

## <a name="running-the-application"></a>De toepassing uitvoeren

Hieronder ziet u hoe u het bericht met behulp van formuliergegevens kunt verzenden in Node.js.

Volg deze stappen voor het uitvoeren van de toepassing:

1. Maak een map voor het project (of gebruik uw favoriete IDE of editor).
2. Navigeer vanuit een opdrachtprompt of terminal naar de map die u zojuist hebt gemaakt.
3. Installeer de aanvraagmodules:  
  ```  
  npm install request  
  ```  
3. Installeer de formuliergegevensmodules:  
  ```  
  npm install form-data  
  ```  
4. Maak een bestand met de naam GetVisualInsights.js en voeg de volgende code toe.
5. Vervang de waarde `subscriptionKey` door de abonnementscode.
6. Vervang de waarde `imagePath` door het pad van de afbeelding die moet worden geüpload.
7. Voer het programma uit.  
  ```
  node GetVisualInsights.js
  ```

```javascript
var request = require('request');
var FormData = require('form-data');
var fs = require('fs');

var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
var subscriptionKey = '<yoursubscriptionkeygoeshere>';
var imagePath = "<pathtoyourimagegoeshere>";

var form = new FormData();
form.append("image", fs.createReadStream(imagePath));

form.getLength(function(err, length){
  if (err) {
    return requestCallback(err);
  }

  var r = request.post(baseUri, requestCallback);
  r._form = form; 
  r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
});

function requestCallback(err, res, body) {
    console.log(JSON.stringify(JSON.parse(body), null, '  '))
}
```


## <a name="next-steps"></a>Volgende stappen

[Inzichten krijgen over een afbeelding met behulp van een inzichttoken](../use-insights-token.md)  
[Zelfstudie: Afbeelding uploaden naar Bing Visual Search](../tutorial-visual-search-image-upload.md)
[Zelfstudie: Bing Visual Search-app met één pagina](../tutorial-bing-visual-search-single-page-app.md)  
[Overzicht van Bing Visual Search ](../overview.md)  
[Proberen](https://aka.ms/bingvisualsearchtryforfree)  
[Een toegangscode voor een gratis proefversie aanvragen](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Naslaginformatie over Bing Visual Search-API](https://aka.ms/bingvisualsearchreferencedoc)
