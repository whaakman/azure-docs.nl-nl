---
title: 'Quickstart: Afbeeldingsinzichten krijgen met behulp van de Bing Visual Search REST-API en Node.js'
titleSuffix: Azure Cognitive Services
description: Leer hoe u een afbeelding uploadt naar de Bing Visual Search-API en inzichten in de afbeelding verkrijgt.
services: cognitive-services
author: swhite-msft
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 4/02/2019
ms.author: scottwhi
ms.openlocfilehash: 9414bac220d928618b403aa2f7df7748772e0e9a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60832605"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Quickstart: Afbeeldingsinzichten krijgen met behulp van de Bing Visual Search REST-API en Node.js

Gebruik deze quickstart om voor het eerst de Bing Visual Search-API aan te roepen en de zoekresultaten te bekijken. Met deze eenvoudige JavaScript-toepassing wordt er een afbeelding naar de API geüpload, waarna de geretourneerde gegevens van de afbeelding worden weergegeven. Hoewel deze toepassing in JavaScript is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.

Tijdens het uploaden van een lokale installatiekopie, de gegevens moet bevatten de `Content-Disposition` header. U moet instellen de `name` parameter 'afbeelding', en de `filename` parameter kan worden ingesteld op een willekeurige tekenreeks. De inhoud van het formulier omvat de binaire gegevens van de installatiekopie. De maximale afbeeldingsgrootte die u kunt uploaden is 1 MB.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Vereisten

* [Node.js](https://nodejs.org/en/download/)
* De aanvraag-module voor JavaScript. U kunt `npm install request` opdracht om de module te installeren.
* De module gegevens. U kunt de `npm install form-data` opdracht om de module te installeren. 

[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]

## <a name="initialize-the-application"></a>De toepassing initialiseren

1. Maak een JavaScript-bestand in uw favoriete IDE of editor en stel de volgende vereisten:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. Variabelen voor uw API-eindpunt, de abonnementssleutel en het pad naar uw installatiekopie maken:

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. Maak een functie met de naam `requestCallback()` om het antwoord van de API af te drukken:

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>De zoekaanvraag samenstellen en verzenden

1. Maak een nieuwe **FormData** object met `FormData()`, en het pad van de installatiekopie toevoegen, met behulp van `fs.createReadStream()`:
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

2. De aanvraag-clientbibliotheek gebruiken om de afbeelding te uploaden en roep `requestCallback()` afdrukken van het antwoord. Zorg ervoor dat de abonnementssleutel van uw toevoegen aan de aanvraagheader:

    ```javascript
    form.getLength(function(err, length){
      if (err) {
        return requestCallback(err);
      }
      var r = request.post(baseUri, requestCallback);
      r._form = form; 
      r.setHeader('Ocp-Apim-Subscription-Key', subscriptionKey);
    });
    ```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een visuele zoekopdrachten één pagina web-app bouwen](../tutorial-bing-visual-search-single-page-app.md)
