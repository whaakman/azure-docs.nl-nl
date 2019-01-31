---
title: 'Snelstart: Afbeeldingsinzichten krijgen met behulp van de Bing Visual Search REST-API en Node.js'
titleSuffix: Azure Cognitive Services
description: Leer hoe u een afbeelding uploadt naar de Bing Visual Search-API en inzichten in de afbeelding verkrijgt.
services: cognitive-services
author: swhite-msft
manager: cgronlun
ms.service: cognitive-services
ms.subservice: bing-visual-search
ms.topic: quickstart
ms.date: 5/16/2018
ms.author: scottwhi
ms.openlocfilehash: 23c3e167f2fd1544a42c98946f4ae95d3451dafe
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55180616"
---
# <a name="quickstart-get-image-insights-using-the-bing-visual-search-rest-api-and-nodejs"></a>Quickstart: Afbeeldingsinzichten krijgen met behulp van de Bing Visual Search REST-API en Node.js

Gebruik deze quickstart om voor het eerst de Bing Visual Search-API aan te roepen en de zoekresultaten te bekijken. Met deze eenvoudige JavaScript-toepassing wordt er een afbeelding naar de API geüpload, waarna de geretourneerde gegevens van de afbeelding worden weergegeven. Hoewel deze toepassing in JavaScript is geschreven, is de API een RESTful-webservice die compatibel is met vrijwel elke programmeertaal.

Bij het uploaden van een lokale afbeelding, moeten de formuliergegevens de header Content-Disposition bevatten. De parameter `name` moet worden ingesteld op "image" en de parameter `filename` kan op een willekeurige tekenreeks worden ingesteld. De inhoud van het formulier is het binaire bestand van de afbeelding. De maximale afbeeldingsgrootte die u kunt uploaden is 1 MB.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

## <a name="prerequisites"></a>Vereisten

* [Node.js](https://nodejs.org/en/download/)
* De Request-module voor JavaScript
    * U kunt deze module installeren met behulp van `npm install request`
* De form-data-module
    * U kunt deze module installeren met behulp van `npm install form-data`


[!INCLUDE [cognitive-services-bing-visual-search-signup-requirements](../../../../includes/cognitive-services-bing-image-search-signup-requirements.md)]


## <a name="initialize-the-application"></a>De toepassing initialiseren

1. Maak een nieuw JavaScript-bestand in uw favoriete IDE of editor en stel de volgende vereisten in:

    ```javascript
    var request = require('request');
    var FormData = require('form-data');
    var fs = require('fs');
    ```

2. Maak variabelen voor uw API-eindpunt, abonnementssleutel en het pad naar uw afbeelding.

    ```javascript
    var baseUri = 'https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch';
    var subscriptionKey = 'your-api-key';
    var imagePath = "path-to-your-image";
    ```

3. Maak een functie met de naam `requestCallback()` om het antwoord van de API weer te geven.

    ```javascript
    function requestCallback(err, res, body) {
        console.log(JSON.stringify(JSON.parse(body), null, '  '))
    }
    ```

## <a name="construct-and-send-the-search-request"></a>De zoekaanvraag samenstellen en verzenden

1. Maak een nieuwe form-data met behulp van `FormData()` en voeg het pad naar de afbeelding eraan toe met behulp van `fs.createReadStream()`.
    
    ```javascript
    var form = new FormData();
    form.append("image", fs.createReadStream(imagePath));
    ```

2. Gebruik de aanvraagbibliotheek om de afbeelding te uploaden en roep `requestCallback()` aan om het antwoord weer te geven. Vergeet niet om de abonnementssleutel toe te voegen aan de aanvraagheader. 

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
> [Een web-app voor aangepaste zoekopdrachten bouwen](../tutorial-bing-visual-search-single-page-app.md)
