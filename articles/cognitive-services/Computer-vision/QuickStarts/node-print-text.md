---
title: Snelstart voor OCR met behulp van de Computer Vision-API met Node.js | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: In deze snelstart extraheert u afgedrukte tekst uit een afbeelding met behulp van Computer Vision met Node.js in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 73fe11800d7141c9e6e7272a0d008d2bcdf6d6af
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770147"
---
# <a name="quickstart-extract-printed-text-ocr---rest-nodejs"></a>Snelstart: Afgedrukte tekst extraheren (OCR) - REST, Node.js

In deze snelstart gebruikt u Computer Vision om afgedrukte tekst uit een afbeelding te extraheren. Dit wordt optische tekenherkenning of OCR genoemd (optical character recognition).

## <a name="prerequisites"></a>Vereisten

Als u Computer Vision wilt gebruiken, moet u een abonnementssleutel hebben. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="ocr-request"></a>OCR-aanvraag

Met de [OCR-methode](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) kunt u tekst in een afbeelding detecteren en de herkende tekens naar een machinaal leesbare tekenstroom extraheren.

U kunt het voorbeeld uitvoeren aan de hand van de volgende stappen:

1. Kopieer de volgende code naar een editor.
1. Vervang `<Subscription Key>` door uw geldige abonnementssleutel.
1. Wijzig zo nodig de waarde `uriBase` in de locatie waar u de abonnementssleutels hebt verkregen.
1. Stel de waarde `imageUrl` eventueel in op de afbeelding die u wilt analyseren.
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
    'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/ocr';

const imageUrl = 'https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/' +
    'Atomist_quote_from_Democritus.png/338px-Atomist_quote_from_Democritus.png';

// Request parameters.
const params = {
    'language': 'unk',
    'detectOrientation': 'true',
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

## <a name="ocr-response"></a>OCR-antwoord

Als dit met succes is voltooid, bevatten de geretourneerde OCR-resultaten tekst, begrenzingsvakken voor regio's, lijnen en woorden, bijvoorbeeld:

```json
{
  "language": "en",
  "orientation": "Up",
  "textAngle": 0,
  "regions": [
    {
      "boundingBox": "21,16,304,451",
      "lines": [
        {
          "boundingBox": "28,16,288,41",
          "words": [
            {
              "boundingBox": "28,16,288,41",
              "text": "NOTHING"
            }
          ]
        },
        {
          "boundingBox": "27,66,283,52",
          "words": [
            {
              "boundingBox": "27,66,283,52",
              "text": "EXISTS"
            }
          ]
        },
        {
          "boundingBox": "27,128,292,49",
          "words": [
            {
              "boundingBox": "27,128,292,49",
              "text": "EXCEPT"
            }
          ]
        },
        {
          "boundingBox": "24,188,292,54",
          "words": [
            {
              "boundingBox": "24,188,292,54",
              "text": "ATOMS"
            }
          ]
        },
        {
          "boundingBox": "22,253,297,32",
          "words": [
            {
              "boundingBox": "22,253,105,32",
              "text": "AND"
            },
            {
              "boundingBox": "144,253,175,32",
              "text": "EMPTY"
            }
          ]
        },
        {
          "boundingBox": "21,298,304,60",
          "words": [
            {
              "boundingBox": "21,298,304,60",
              "text": "SPACE."
            }
          ]
        },
        {
          "boundingBox": "26,387,294,37",
          "words": [
            {
              "boundingBox": "26,387,210,37",
              "text": "Everything"
            },
            {
              "boundingBox": "249,389,71,27",
              "text": "else"
            }
          ]
        },
        {
          "boundingBox": "127,431,198,36",
          "words": [
            {
              "boundingBox": "127,431,31,29",
              "text": "is"
            },
            {
              "boundingBox": "172,431,153,36",
              "text": "opinion."
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de Computer Vision-API's die worden gebruikt om een afbeelding te analyseren, beroemdheden en oriëntatiepunten te detecteren, een miniatuur te maken en gedrukte en handgeschreven tekst te verkrijgen. Als u snel wilt experimenteren met de Computer Vision-API's, probeert u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [De Computer Vision-API's bekijken](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
