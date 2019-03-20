---
title: 'Quickstart: Een externe afbeelding analyseren - REST, PHP'
titleSuffix: Azure Cognitive Services
description: In deze snelstart analyseert u een afbeelding met behulp van de Computer Vision-API met PHP.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 03/11/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: c4c5c53f19831946675aca74a631c3e582690308
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57897687"
---
# <a name="quickstart-analyze-a-remote-image-using-the-rest-api-and-php-in-computer-vision"></a>Quickstart: Een externe afbeelding analyseren met behulp van de REST API en PHP in Computer Vision

In deze snelstart analyseert u een extern opgeslagen afbeelding om visuele kenmerken te verkrijgen met behulp van de REST-API van Computer Vision. Met de [Analyze Image](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa)-methode kunt u visuele kenmerken verkrijgen op basis van de afbeeldingsinhoud.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

- U moet [PHP](https://secure.php.net/downloads.php) hebben geïnstalleerd.
- U moet [Pear](https://pear.php.net) hebben geïnstalleerd.
- U moet beschikken over een abonnementssleutel voor Computer Vision. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md) voor meer informatie over het verkrijgen van een abonnementssleutel.

## <a name="create-and-run-the-sample"></a>Het voorbeeld maken en uitvoeren

U kunt het voorbeeld maken en uitvoeren aan de hand van de volgende stappen:

1. Installeer het PHP5-pakket [`HTTP_Request2`](https://pear.php.net/package/HTTP_Request2).
   1. Open een opdrachtpromptvenster als beheerder.
   1. Voer de volgende opdracht uit:

      ```console
      pear install HTTP_Request2
      ```

   1. Sluit het opdrachtpromptvenster nadat het pakket is geïnstalleerd.

1. Kopieer de volgende code naar een teksteditor.
1. Breng waar nodig de volgende wijzigingen in code aan:
    1. Vervang de waarde van `subscriptionKey` door uw abonnementssleutel.
    1. Vervang de waarde van `uriBase` door de eindpunt-URL van de methode [Afbeelding analyseren](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) uit de Azure-regio waar u uw abonnementssleutels hebt verkregen (indien nodig).
    1. Vervang eventueel de waarde van `imageUrl` door de URL van een andere afbeelding die u wilt analyseren.
    1. Vervang eventueel de waarde van de aanvraagparameter `language` door een andere taal.
1. Sla de code op als een bestand met de extensie `.php`. Bijvoorbeeld `analyze-image.php`.
1. Open een browservenster met PHP-ondersteuning.
1. Sleep het bestand naar het browservenster.

```php
<html>
<head>
    <title>Analyze Image Sample</title>
</head>
<body>
<?php
// Replace <Subscription Key> with a valid subscription key.
$ocpApimSubscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to obtain
// your subscription keys. For example, if you obtained your subscription keys
// from westus, replace "westcentralus" in the URL below with "westus".
$uriBase = 'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/';

$imageUrl = 'https://upload.wikimedia.org/wikipedia/commons/3/3c/Shaki_waterfall.jpg';

require_once 'HTTP/Request2.php';

$request = new Http_Request2($uriBase . '/analyze');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',
    'Ocp-Apim-Subscription-Key' => $ocpApimSubscriptionKey
);
$request->setHeader($headers);

$parameters = array(
    // Request parameters
    'visualFeatures' => 'Categories,Description',
    'details' => '',
    'language' => 'en'
);
$url->setQueryVariables($parameters);

$request->setMethod(HTTP_Request2::METHOD_POST);

// Request body parameters
$body = json_encode(array('url' => $imageUrl));

// Request body
$request->setBody($body);

try
{
    $response = $request->send();
    echo "<pre>" .
        json_encode(json_decode($response->getBody()), JSON_PRETTY_PRINT) . "</pre>";
}
catch (HttpException $ex)
{
    echo "<pre>" . $ex . "</pre>";
}
?>
</body>
</html>
```

## <a name="examine-the-response"></a>Het antwoord bekijken

Een geslaagd antwoord wordt geretourneerd in JSON-indeling. De voorbeeldwebsite parseert en geeft een geslaagd antwoord weer in het browservenster dat vergelijkbaar is met het volgende voorbeeld:

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
  "requestId": "ebf5a1bc-3ba2-4c56-99b4-bbd20ba28705",
  "metadata": {
    "height": 959,
    "width": 1280,
    "format": "Jpeg"
  }
}
```

## <a name="clean-up-resources"></a>Resources opschonen

Verwijder het bestand wanneer u het niet meer nodig hebt en verwijder het PHP5-pakket `HTTP_Request2`. Voer de volgende stappen uit om het pakket te verwijderen:

1. Open een opdrachtpromptvenster als beheerder.
2. Voer de volgende opdracht uit:

   ```console
   pear uninstall HTTP_Request2
   ```

3. Sluit het opdrachtpromptvenster nadat het pakket is verwijderd.

## <a name="next-steps"></a>Volgende stappen

Bekijk de Computer Vision-API die wordt gebruikt om een afbeelding te analyseren, beroemdheden en oriëntatiepunten te detecteren, een miniatuur te maken en gedrukte en handgeschreven tekst te verkrijgen. Als u snel wilt experimenteren met de Computer Vision-API, gebruikt u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [De Computer Vision-API verkennen](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
