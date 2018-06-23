---
title: Emotion-API Ruby snel aan de slag | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met de Emotion-API met Ruby in cognitieve Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: 7ac981966a303b3d197a3e27b6f3a1b20f34c17f
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344743"
---
# <a name="emotion-api-ruby-quick-start"></a>Emotion-API Ruby snel starten

> [!IMPORTANT]
> Video API Preview eindigt op 30 oktober 2017. Het nieuwe [Video indexeerfunctie API Preview](https://azure.microsoft.com/services/cognitive-services/video-indexer/) eenvoudig inzichten extraheren van video's en ervaringen van inhoud, zoals de lijst met zoekresultaten verbeteren doordat gesproken woorden, vlakken tekens en emoties. [Meer informatie](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

In dit artikel vindt u informatie en codevoorbeelden kunt u snel aan de slag met de [Emotion-API herkennen methode](https://dev.projectoxford.ai/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) met Ruby de emoties uitgedrukt in een of meer personen in een installatiekopie kan herkennen.

## <a name="prerequisite"></a>Vereiste
* Sleutel voor gratis abonnement ophalen [hier](https://azure.microsoft.com/try/cognitive-services/)

## <a name="recognize-emotions-ruby-example-request"></a>Herkent emoties Ruby Voorbeeldaanvraag

Wijzig de REST-URL voor het gebruik van de locatie waar u de sleutels van uw abonnement hebt verkregen, vervang de waarde 'Ocp-Apim-Subscription-Key' door uw sleutel geldig abonnement en een URL kunt toevoegen aan een foto naar de `body` variabele.

```ruby
require 'net/http'

# NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
#   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the 
#   URL below with "westcentralus".
uri = URI('https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize')
uri.query = URI.encode_www_form({
})

request = Net::HTTP::Post.new(uri.request_uri)
# Request headers
request['Content-Type'] = 'application/json'
# NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
request['Ocp-Apim-Subscription-Key'] = '13hc77781f7e4b19b5fcdd72a8df7156'
# Request body
request.body = "{\"url\":\"http://example.com/1.jpg\"}"

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

puts response.body

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
