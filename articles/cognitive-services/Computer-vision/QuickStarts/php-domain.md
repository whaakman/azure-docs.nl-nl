---
title: Snelstart voor een domeinmodel met behulp van de Computer Vision-API met PHP | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: In deze snelstart gebruikt u een domeinmodel om oriëntatiepunten in een afbeelding te identificeren met behulp van Computer Vision met PHP in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 335065b45781dd2712f5416fb03a2f8726182472
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770291"
---
# <a name="quickstart-use-a-domain-model---rest-php"></a>Snelstart: een domeinmodel gebruiken - REST, PHP

In deze snelstart gebruikt u een domeinmodel om oriëntatiepunten of beroemdheden in een afbeelding te identificeren met behulp van Computer Vision.

## <a name="prerequisites"></a>Vereisten

Als u Computer Vision wilt gebruiken, moet u een abonnementssleutel hebben. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="recognize-landmark-request"></a>Recognize Landmark-aanvraag

Met de methode [Domain Specific Content](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e200) kunt u een specifieke set objecten in een afbeelding identificeren. De twee domeinspecifieke modellen die momenteel beschikbaar zijn, zijn _beroemdheden_ en _oriëntatiepunten_.

U kunt het voorbeeld uitvoeren aan de hand van de volgende stappen:

1. Kopieer de volgende code in een editor.
1. Vervang `<Subscription Key>` door uw geldige abonnementssleutel.
1. Wijzig zo nodig `uriBase` om de locatie te gebruiken waar u de abonnementssleutels hebt verkregen.
1. Stel `imageUrl` desgewenst in op de afbeelding die u wilt analyseren.
1. Stel `domain` desgewenst in op `celebrities` om gebruik te maken van het beroemdhedenmodel.
1. Sla het bestand op met de extensie `.php`.
1. Open het bestand in een browservenster met PHP-ondersteuning.

In het volgende voorbeeld wordt een oriëntatiepunt in een afbeelding geïdentificeerd.

In dit voorbeeld wordt gebruikgemaakt van het PHP5 [HTTP_Request2](http://pear.php.net/package/HTTP_Request2)-pakket.

```php
<html>
<head>
    <title>Analyze Domain Model Sample</title>
</head>
<body>
<?php
// Replace <Subscription Key> with a valid subscription key.
$ocpApimSubscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to obtain
// your subscription keys. For example, if you obtained your subscription keys
// from westus, replace "westcentralus" in the URL below with "westus".
$uriBase = 'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/';

// Change 'landmarks' to 'celebrities' to use the Celebrities model.
$domain = 'landmarks';

$imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/2/23/Space_Needle_2011-07-04.jpg';

require_once 'HTTP/Request2.php';

$request = new Http_Request2($uriBase . 'models/' . $domain . '/analyze');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',
    'Ocp-Apim-Subscription-Key' => $ocpApimSubscriptionKey
);
$request->setHeader($headers);

$parameters = array(
    // Request parameters
    'model' => $domain
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

## <a name="recognize-landmark-response"></a>Recognize Landmark-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, bijvoorbeeld:

```json
{
    "result": {
        "landmarks": [
            {
                "name": "Space Needle",
                "confidence": 0.9998177886009216
            }
        ]
    },
    "requestId": "4d26587b-b2b9-408d-a70c-1f8121d84b0d",
    "metadata": {
        "height": 4132,
        "width": 2096,
        "format": "Jpeg"
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de Computer Vision-API's die worden gebruikt om een afbeelding te analyseren, beroemdheden en oriëntatiepunten te detecteren, een miniatuur te maken en gedrukte en handgeschreven tekst te verkrijgen. Als u snel wilt experimenteren met de Computer Vision-API's, probeert u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [De Computer Vision-API's bekijken](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
