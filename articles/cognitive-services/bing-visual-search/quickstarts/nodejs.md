---
title: JavaScript-Snelstartgids voor Bing visuele zoekopdrachten-API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Laat zien hoe u een installatiekopie uploaden naar de Bing visuele zoekopdrachten-API en weer toegang krijgen van inzicht in de afbeelding.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 60b1dc9b8ea9eda258e9776b8967df38c97d964e
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2018
ms.locfileid: "39071699"
---
# <a name="your-first-bing-visual-search-query-in-javascript"></a>Uw eerste Bing visuele zoekopdrachten-query in JavaScript

Bing visuele zoekopdrachten-API retourneert informatie over een afbeelding die u opgeeft. U kunt de installatiekopie opgeven met behulp van de URL van de afbeelding, een insights token, of door een installatiekopie te uploaden. Zie voor meer informatie over deze opties [wat Bing visuele zoekopdrachten-API is?](../overview.md) In dit artikel ziet u een installatiekopie uploaden. Uploaden van een afbeelding kan nuttig zijn in mobiele scenario's waarbij u een afbeelding van een bekende oriëntatiepunt en informatie erover weer toegang krijgen. Bijvoorbeeld, kunnen de insights bevat ook algemene vragen over de oriëntatiepunt. 

Als u een lokale installatiekopie uploadt, ziet hieronder u de gegevens dat moet u in de hoofdtekst van het bericht opnemen. De gegevens moet de header Content-Disposition bevatten. De `name` parameter moet worden ingesteld op 'afbeelding' en de `filename` parameter kan worden ingesteld op een willekeurige tekenreeks. De inhoud van het formulier is het binaire bestand van de installatiekopie. De grootte van de maximale installatiekopie die u kunt uploaden is 1 MB. 

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

Dit artikel bevat een eenvoudige consoletoepassing die een Bing visuele zoekopdrachten-API-aanvraag verzendt en de JSON-zoekresultaten worden weergegeven. Terwijl deze toepassing is geschreven in JavaScript, de API is een RESTful-Web-compatibel is met elke programmeertaal die HTTP-aanvragen te parseren van JSON. 

## <a name="prerequisites"></a>Vereisten

U moet [Node.js 6](https://nodejs.org/en/download/) deze code uit te voeren.

In deze Quick Start kunt u een [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/?api=bing-web-search-api) abonnementssleutel of een betaald abonnement-sleutel.

## <a name="running-the-application"></a>De toepassing uitvoeren

Hieronder ziet u hoe u het bericht met FormData in Node.js te verzenden.

Om uit te voeren deze toepassing, de volgende stappen uit:

1. Maak een map voor uw project (of gebruik uw favoriete IDE of editor).
2. Navigeer naar de map die u zojuist hebt gemaakt vanaf een opdrachtprompt of terminal.
3. De aanvraag-modules installeren:  
  ```  
  npm install request  
  ```  
3. Installeer de formuliergegevens-modules:  
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
[Bing visuele zoekopdrachten-installatiekopie uploaden zelfstudie](../tutorial-visual-search-image-upload.md)
[Bing visuele zoekopdrachten-app met één pagina zelfstudie](../tutorial-bing-visual-search-single-page-app.md)  
[Bing visuele zoekopdrachten-overzicht](../overview.md)  
[Nu uitproberen](https://aka.ms/bingvisualsearchtryforfree)  
[Een gratis proefversie toegangssleutel ophalen](https://azure.microsoft.com/try/cognitive-services/?api=bing-visual-search-api)  
[Bing visuele zoekopdrachten-API-verwijzing](https://aka.ms/bingvisualsearchreferencedoc)
