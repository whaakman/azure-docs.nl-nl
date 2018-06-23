---
title: Emotion-API cURL snel aan de slag | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met de Emotion-API met cURL in cognitieve Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: d36a191d3589702d676e694715f34ea1c924e2d3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344742"
---
# <a name="emotion-api-curl-quick-start"></a>Emotion-API cURL snel starten

> [!IMPORTANT]
> Video API Preview eindigt op 30 oktober 2017. Het nieuwe [Video indexeerfunctie API Preview](https://azure.microsoft.com/services/cognitive-services/video-indexer/) eenvoudig inzichten extraheren van video's en ervaringen van inhoud, zoals de lijst met zoekresultaten verbeteren doordat gesproken woorden, vlakken tekens en emoties. [Meer informatie](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

In dit artikel vindt u informatie en codevoorbeelden kunt u snel aan de slag met de [Emotion-API herkennen methode](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) met cURL de emoties uitgedrukt in een of meer personen in een installatiekopie kan herkennen. 

## <a name="prerequisite"></a>Vereiste
* Sleutel voor gratis abonnement ophalen [hier](https://azure.microsoft.com/try/cognitive-services/)

## <a name="recognize-emotions-curl-example-request"></a>Herkent dat emoties cURL voorbeeld aanvragen

> [!NOTE]
> U moet dezelfde locatie gebruiken in uw REST-aanroep als u gebruikt voor het verkrijgen van de sleutels van uw abonnement. Bijvoorbeeld, als u de sleutels van uw abonnement hebt verkregen via westcentralus, vervangen door 'westus' in de onderstaande URL 'westcentralus'.

```json
@ECHO OFF

curl -v -X POST "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: {subscription key}"

--data-ascii "{body}" 
```

## <a name="recognize-emotions-sample-response"></a>Voorbeeldreactie emoties herkennen
Een geslaagde aanroep retourneert een matrix met face-vermeldingen en hun bijbehorende emotion-scores, gerangschikt op face rechthoek grootte in aflopende volgorde. Een leeg antwoord geeft aan dat er geen vlakken zijn gedetecteerd. Een emotion-vermelding bevat de volgende velden:
* faceRectangle - locatie van de rechthoek van gezicht in de installatiekopie.
* scores - Emotion scores voor elk vlak in de installatiekopie. 

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

