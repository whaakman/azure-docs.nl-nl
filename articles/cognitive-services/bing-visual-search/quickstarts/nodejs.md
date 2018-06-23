---
title: JavaScript Quick Start voor Bing Visual zoeken-API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Laat zien hoe u een installatiekopie uploaden naar Bing Visual zoeken-API en terughalen van inzicht in de afbeelding.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: dd28c829d8d24980a746244dc6aca880d2d69224
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345644"
---
# <a name="your-first-bing-visual-search-query-in-javascript"></a>Uw eerste Bing Visual zoekopdracht in JavaScript

Bing Visual zoeken-API retourneert informatie over de installatiekopie die u opgeeft. Met behulp van de URL van de afbeelding, een insights token, of door een afbeelding te uploaden, kunt u de installatiekopie opgeven. Zie voor meer informatie over deze opties [wat Bing Visual zoeken-API is?](../overview.md) In dit artikel ziet u een afbeelding te uploaden. Een afbeelding te uploaden kan nuttig zijn in mobiele scenario's waarin u een afbeelding van een bekende kenmerkende en informatie over het terughalen. De insights kunnen bijvoorbeeld de algemene vragen over de kenmerkende bevatten. 

Als u een lokale installatiekopie uploadt, ziet hieronder u de gegevens dat moet u in de hoofdtekst van het bericht opnemen. De formuliergegevens moet de header Content-Disposition bevatten. De `name` parameter moet worden ingesteld op 'installatiekopie' en de `filename` parameter kan worden ingesteld op een willekeurige tekenreeks. De inhoud van het formulier is het binaire bestand van de afbeelding. De grootte van de maximale installatiekopie die u kan uploaden is 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Dit artikel bevat een eenvoudige consoletoepassing die verzendt een aanvraag Bing Visual zoeken-API en de JSON-zoekresultaten worden weergegeven. Terwijl deze toepassing is geschreven in JavaScript, is de API een compatibel is met elke programmeertaal die kunt maken van HTTP-aanvragen en parseren van JSON RESTful-Web-service. 

## <a name="prerequisites"></a>Vereisten

U moet [Node.js 6](https://nodejs.org/en/download/) deze code uit te voeren.

U kunt gebruiken voor deze snelstartgids een [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonnementssleutel of een betaald abonnement-sleutel.

## <a name="running-the-application"></a>De toepassing uitvoeren

Hieronder ziet u hoe u verzendt het bericht met FormData in Node.js.

Voor het uitvoeren van deze toepassing, de volgende stappen uit:

1. Maak een map voor uw project (of uw favoriete IDE of -editor gebruiken).
2. Navigeer naar de map die u zojuist hebt gemaakt vanaf een opdrachtprompt of terminal.
3. De aanvraag-modules installeren:  
  ```  
  npm install request  
  ```  
3. De formuliergegevens modules installeren:  
  ```  
  npm install form-data  
  ```  
4. Maak een bestand met de naam GetVisualInsights.js en voeg de volgende code toe.
5. Vervang de `subscriptionKey` waarde met de abonnementssleutel van uw.
6. Vervang de `imagePath` waarde met het pad van de afbeelding te uploaden.
7. Voer het programma.  
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

[Inzichten over een afbeelding met behulp van een token inzichten verkrijgen](../use-insights-token.md)  
[Bing Visual Search één pagina app-zelfstudie](../tutorial-bing-visual-search-single-page-app.md)  
[Overzicht van Bing Visual zoeken](../overview.md)  
[Probeer deze](https://aka.ms/bingvisualsearchtryforfree)  
[Een gratis proefversie toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing Visual Search API-verwijzingen](https://aka.ms/bingvisualsearchreferencedoc)
