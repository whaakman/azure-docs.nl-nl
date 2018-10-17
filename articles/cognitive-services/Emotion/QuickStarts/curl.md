---
title: 'Snelstart: emoties op gezichten in een afbeelding herkennen - Emotion-API, cURL'
titlesuffix: Azure Cognitive Services
description: Bekijk informatie en codevoorbeelden om snel aan de slag te gaan met de Emotion-API en cURL.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: dfdaa89c9d29e419539f385f601dc7f264bf838e
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48237072"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Snelstart: een app bouwen voor het herkennen van emoties op gezichten in een afbeelding.

> [!IMPORTANT]
> De Emotion-API wordt op 15 februari 2019 afgeschaft. De mogelijkheid voor de herkenning van emoties is nu algemeen beschikbaar als onderdeel van de [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/).

In dit artikel vindt u informatie en codevoorbeelden om met behulp van cURL en de [methode Recognize van de Emotion-API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) de emoties te herkennen die worden uitgedrukt door een of meer personen in een afbeelding.

## <a name="prerequisite"></a>Vereiste
* U kunt [hier](https://azure.microsoft.com/try/cognitive-services/) een gratis abonnementssleutel ophalen

## <a name="recognize-emotions-curl-example-request"></a>Voorbeeld van cURL-aanvraag voor herkennen van emoties

> [!NOTE]
> U moet in uw REST-aanroep dezelfde locatie gebruiken waar u uw abonnementssleutels hebt verkregen. Als u bijvoorbeeld uw abonnementssleutels van westcentralus hebt verkregen, vervangt u 'westus' in de onderstaande URL door 'westcentralus'.

```json
@ECHO OFF

curl -v -X POST "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}"
```

## <a name="recognize-emotions-sample-response"></a>Voorbeeldantwoord voor herkennen van emoties
Een geslaagde aanroep retourneert een matrix van gezichtsvermeldingen en de bijbehorende emotiescores, in aflopende volgorde gerangschikt op grootte van gezichtsrechthoek. Een leeg antwoord geeft aan dat er geen gezichten zijn gedetecteerd. Een emotievermelding bevat de volgende velden:
* faceRectangle: locatie van de rechthoek met het gezicht in de afbeelding.
* scores: emotiescores voor elk gezicht in de afbeelding.

```json
application/json
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]
