---
title: Snelstartgids voor het analyseren van afbeeldingen met behulp van de Computer Vision-API met Node.js | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: In deze snelstartgids analyseert u een afbeelding met behulp van Computer Vision met Node.js in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: dab6547e08b1b01a9090a817d728c86359c680f2
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770240"
---
# <a name="quickstart-analyze-a-remote-image---rest-nodejs"></a>Snelstartgids: een externe afbeelding analyseren - REST, Node.js

In deze snelstartgids analyseert u een afbeelding om visuele kenmerken te verkrijgen met behulp van Computer Vision.

## <a name="prerequisites"></a>Vereisten

Als u Computer Vision wilt gebruiken, moet u een abonnementssleutel hebben. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="analyze-image-request"></a>Aanvraag voor het analyseren van een afbeelding

Met de [methode Analyze Image](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) kunt u visuele kenmerken verkrijgen op basis van de afbeeldingsinhoud. U kunt een afbeelding uploaden of een afbeeldings-URL opgeven en kiezen welke kenmerken moeten worden geretourneerd, zoals:

* Een gedetailleerde lijst met tags die betrekking hebben op de afbeeldingsinhoud.
* Een beschrijving van de afbeeldingsinhoud in een volledige zin.
* De coördinaten, het geslacht en de leeftijd die bij de gezichten horen die in de afbeelding voorkomen.
* Het type afbeelding (illustratie of een lijntekening).
* De overheersende kleur, de accentkleur en of een afbeelding in zwart-wit is.
* De categorie die is gedefinieerd in deze [taxonomie](../Category-Taxonomy.md).
* Bevat de afbeelding erotische of seksueel suggestieve inhoud?

U kunt het voorbeeld uitvoeren aan de hand van de volgende stappen:

1. Kopieer de volgende code naar een editor.
1. Vervang `<Subscription Key>` door uw geldige abonnementssleutel.
1. Wijzig zo nodig de waarde `uriBase` in de locatie waar u de abonnementssleutels hebt verkregen.
1. Stel de waarde `imageUrl` eventueel in op de afbeelding die u wilt analyseren.
1. Wijzig eventueel de taal voor het antwoord (`'language': 'en'`).
1. Sla het bestand op met de extensie `.js`.
1. Open de Node.js-opdrachtprompt en voer het bestand uit, bijvoorbeeld: `node myfile.js`.

In dit voorbeeld wordt gebruikgemaakt van het npm-[aanvraag](https://www.npmjs.com/package/request)pakket.

```nodejs
'use strict';

const request = require('request');

// Replace <Subscription Key> with your valid subscription key.
const subscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to get your
// subscription keys. For example, if you got your subscription keys from
// westus, replace "westcentralus" in the URL below with "westus".
const uriBase =
    'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze';

const imageUrl =
    'http://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg';

// Request parameters.
const params = {
    'visualFeatures': 'Categories,Description,Color',
    'details': '',
    'language': 'en'
};

const options = {
    uri: uriBase,
    qs: params,
    body: '{"url": ' + '"' + imageUrl + '"}',
    headers: {
        'Content-Type': 'application/json',
        'Ocp-Apim-Subscription-Key' : subscriptionKey
    }
};

request.post(options, (error, response, body) => {
  if (error) {
    console.log('Error: ', error);
    return;
  }
  let jsonResponse = JSON.stringify(JSON.parse(body), null, '  ');
  console.log('JSON Response\n');
  console.log(jsonResponse);
});
```

## <a name="analyze-image-response"></a>Antwoord voor het analyseren van een afbeelding

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, bijvoorbeeld:

```json
{
  "categories": [
    {
      "name": "outdoor_water",
      "score": 0.9921875,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "nature",
      "water",
      "waterfall",
      "outdoor",
      "rock",
      "mountain",
      "rocky",
      "grass",
      "hill",
      "covered",
      "hillside",
      "standing",
      "side",
      "group",
      "walking",
      "white",
      "man",
      "large",
      "snow",
      "grazing",
      "forest",
      "slope",
      "herd",
      "river",
      "giraffe",
      "field"
    ],
    "captions": [
      {
        "text": "a large waterfall over a rocky cliff",
        "confidence": 0.916458423253597
      }
    ]
  },
  "color": {
    "dominantColorForeground": "Grey",
    "dominantColorBackground": "Green",
    "dominantColors": [
      "Grey",
      "Green"
    ],
    "accentColor": "4D5E2F",
    "isBwImg": false
  },
  "requestId": "81b4e400-e3c1-41f1-9020-e6871ad9f0ed",
  "metadata": {
    "height": 959,
    "width": 1280,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de Computer Vision-API's die worden gebruikt om een afbeelding te analyseren, beroemdheden en oriëntatiepunten te detecteren, een miniatuur te maken en gedrukte en handgeschreven tekst te verkrijgen. Als u snel wilt experimenteren met de Computer Vision-API's, probeert u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [De Computer Vision-API's bekijken](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
