---
title: Snelstart voor het maken van een miniatuur met de Computer Vision-API met Node.js | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: In deze snelstart maakt u een miniatuur van een afbeelding met behulp van Computer Vision met Node.js in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: c7038a294cbb273e21e892956a0b9c9e5fbfc38a
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770283"
---
# <a name="quickstart-generate-a-thumbnail---rest-nodejs"></a>Snelstart: een miniatuur maken - REST, Node.js

In deze snelstart maakt u een miniatuur van een afbeelding met behulp van Computer Vision.

## <a name="prerequisites"></a>Vereisten

Als u Computer Vision wilt gebruiken, moet u een abonnementssleutel hebben. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="get-thumbnail-request"></a>Get Thumbnail-aanvraag

Met de methode [Get Thumbnail](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) (miniatuur ophalen) kunt u een miniatuur van een afbeelding genereren. U geeft de hoogte en breedte op. Deze mag afwijken van de hoogte-breedteverhouding van de ingevoerde afbeelding. Computer Vision maakt gebruik van slim bijsnijden om op intelligente wijze het interessegebied te bepalen en coördinaten voor het bijsnijden te genereren op basis van dat gebied.

U kunt het voorbeeld uitvoeren aan de hand van de volgende stappen:

1. Kopieer de volgende code in een editor.
1. Vervang `<Subscription Key>` door uw geldige abonnementssleutel.
1. Wijzig zo nodig de waarde `uriBase` in de locatie waar u de abonnementssleutels hebt verkregen.
1. Stel de waarde `imageUrl` desgewenst in op de afbeelding die u wilt analyseren.
1. Sla het bestand op met de extensie `.js`.
1. Open de Node.js-opdrachtprompt en voer het bestand uit, bijvoorbeeld: `node myfile.js`.

In dit voorbeeld wordt gebruikgemaakt van het npm-[aanvraag](https://www.npmjs.com/package/request)-pakket.

```nodejs
'use strict';

const request = require('request');

// Replace <Subscription Key> with your valid subscription key.
const subscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to get your
// subscription keys. For example, if you got your subscription keys from
// westus, replace "westcentralus" in the URL below with "westus".
const uriBase =
    'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail';

const imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg';

// Request parameters.
const params = {
    'width': '100',
    'height': '100',
    'smartCropping': 'true'
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
});
```

## <a name="get-thumbnail-response"></a>Get Thumbnail-antwoord

Een geslaagd antwoord bevat het binaire bestand voor de miniatuurafbeelding. Als de aanvraag mislukt, bevat het antwoord een foutcode en een bericht om u te helpen bepalen wat er mis is gegaan.

## <a name="next-steps"></a>Volgende stappen

Bekijk de Computer Vision-API's die worden gebruikt om een afbeelding te analyseren, beroemdheden en oriëntatiepunten te detecteren, een miniatuur te maken en gedrukte en handgeschreven tekst te verkrijgen. Als u snel wilt experimenteren met de Computer Vision-API's, probeert u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [De Computer Vision-API's bekijken](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
