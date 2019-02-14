---
title: 'Quickstart: Emoties herkennen op gezichten in een afbeelding - Emotion-API, Ruby'
titlesuffix: Azure Cognitive Services
description: Bekijk informatie en codevoorbeelden om snel aan de slag te gaan met de Emotion-API en Ruby.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 05/23/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: b1bddbffa3a58158b3a87c4681557ff8f10a6334
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56232670"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Quickstart: Een app bouwen voor het herkennen van emoties op een gezicht in een afbeelding.

> [!IMPORTANT]
> De Emotion-API wordt op 15 februari 2019 afgeschaft. De mogelijkheid voor de herkenning van emoties is nu algemeen beschikbaar als onderdeel van de [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/). 

In dit artikel vindt u informatie en codevoorbeelden om met behulp van Ruby en de [methode Recognize van de Emotion-API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) de emoties te herkennen die worden uitgedrukt door een of meer personen in een afbeelding.

## <a name="prerequisite"></a>Vereiste
* U kunt [hier](https://azure.microsoft.com/try/cognitive-services/) een gratis abonnementssleutel ophalen

## <a name="recognize-emotions-ruby-example-request"></a>Voorbeeld van Ruby-aanvraag voor herkennen van emoties

Wijzig de REST-URL in de locatie waar u de abonnementssleutels hebt verkregen, vervang de waarde 'Ocp-Apim-Subscription-Key' door een geldige abonnementssleutel en voeg een URL naar een foto toe aan de variabele `body`.

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
