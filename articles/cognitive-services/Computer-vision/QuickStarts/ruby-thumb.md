---
title: 'Quickstart: Een miniatuur genereren - REST, Ruby'
titleSuffix: Azure Cognitive Services
description: In deze quickstart maakt u een miniatuur van een afbeelding met behulp van de Computer Vision-API en Ruby.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: e666c9371b4f38f20c5835b06d7b9959ad5718ad
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53578703"
---
# <a name="quickstart-generate-a-thumbnail-using-the-rest-api-and-ruby-in-computer-vision"></a>Quickstart: Een miniatuur maken met de REST API en Ruby in Computer Vision

In deze quickstart maakt u een miniatuur van een afbeelding met behulp van de REST API van Computer Vision. Met de methode [Get Thumbnail](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) kunt u een miniatuur van een afbeelding genereren. U geeft de hoogte en breedte op. Deze waarden mogen afwijken van de hoogte-breedteverhouding van de invoerafbeelding. Computer Vision maakt gebruik van slim bijsnijden om op intelligente wijze het interessegebied te bepalen en coördinaten voor het bijsnijden te genereren op basis van dat gebied.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

- U moet [Ruby](https://www.ruby-lang.org/en/downloads/) 2.4.x of later hebben geïnstalleerd.
- U moet beschikken over een abonnementssleutel voor Computer Vision. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md) voor meer informatie over het verkrijgen van een abonnementssleutel.

## <a name="create-and-run-the-sample"></a>Het voorbeeld maken en uitvoeren

U kunt het voorbeeld maken en uitvoeren aan de hand van de volgende stappen:

1. Kopieer de volgende code in een teksteditor.
1. Breng waar nodig de volgende wijzigingen in code aan:
    1. Vervang `<Subscription Key>` door uw abonnementssleutel.
    1. Vervang `https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/analyze` door de eindpunt-URL van de methode [Miniatuur ophalen](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) in de Azure-regio waar u uw abonnementssleutels hebt verkregen (indien nodig).
    1. Vervang eventueel `https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\` door de URL van een andere afbeelding waar u een miniatuur van wilt maken.
1. Sla de code op als een bestand met de extensie `.rb`. Bijvoorbeeld `get-thumbnail.rb`.
1. Open een opdrachtpromptvenster.
1. Typ bij de prompt de opdracht `ruby` om het voorbeeld uit te voeren. Bijvoorbeeld `ruby get-thumbnail.rb`.

```ruby
require 'net/http'

# You must use the same location in your REST call as you used to get your
# subscription keys. For example, if you got your subscription keys from westus,
# replace "westcentralus" in the URL below with "westus".
uri = URI('https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/generateThumbnail')
uri.query = URI.encode_www_form({
    # Request parameters
    'width' => '100',
    'height' => '100',
    'smartCropping' => 'true'
})

request = Net::HTTP::Post.new(uri.request_uri)

# Request headers
# Replace <Subscription Key> with your valid subscription key.
request['Ocp-Apim-Subscription-Key'] = '<Subscription Key>'
request['Content-Type'] = 'application/json'

request.body =
    "{\"url\": \"https://upload.wikimedia.org/wikipedia/commons/thumb/5/56/" +
    "Shorkie_Poo_Puppy.jpg/1280px-Shorkie_Poo_Puppy.jpg\"}"

response = Net::HTTP.start(uri.host, uri.port, :use_ssl => uri.scheme == 'https') do |http|
    http.request(request)
end

#puts response.body
```

## <a name="examine-the-response"></a>Het antwoord bekijken

Een geslaagd antwoord wordt geretourneerd als binaire gegevens - deze staan voor de afbeeldingsgegevens van de miniatuur. Als de aanvraag mislukt, wordt het antwoord weergegeven in het consolevenster. De reactie op de mislukte aanvraag bevat een foutcode en een bericht om u te helpen bepalen wat er mis is gegaan.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u het bestand niet meer nodig hebt, kunt u het verwijderen.

## <a name="next-steps"></a>Volgende stappen

Bekijk de Computer Vision-API die wordt gebruikt om een afbeelding te analyseren, beroemdheden en oriëntatiepunten te detecteren, een miniatuur te maken en gedrukte en handgeschreven tekst te verkrijgen. Als u snel wilt experimenteren met de Computer Vision-API, gebruikt u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [De Computer Vision-API verkennen](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
