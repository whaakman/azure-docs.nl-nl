---
title: 'Snelstartgids: Een miniatuur genereren - REST, PHP - Computer Vision'
titleSuffix: Azure Cognitive Services
description: In deze snelstart maakt u een miniatuur van een afbeelding met behulp van de Computer Vision-API en PHP.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: pafarley
ms.openlocfilehash: 171893dd3017d46c405d2269506fe37896155e55
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52964506"
---
# <a name="quickstart-generate-a-thumbnail-using-the-rest-api-and-php-in-computer-vision"></a>Snelstartgids: Een miniatuur maken met de REST API en PHP in Computer Vision

In deze snelstart maakt u een miniatuur van een afbeelding met behulp van de REST API van Computer Vision. Met de methode [Get Thumbnail](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) kunt u een miniatuur van een afbeelding genereren. U geeft de hoogte en breedte op. Deze waarden mogen afwijken van de hoogte-breedteverhouding van de invoerafbeelding. Computer Vision maakt gebruik van slim bijsnijden om op intelligente wijze het interessegebied te bepalen en coördinaten voor het bijsnijden te genereren op basis van dat gebied.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/ai/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=cognitive-services) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

- U moet [PHP](https://secure.php.net/downloads.php) hebben geïnstalleerd.
- U moet [Pear](https://pear.php.net) hebben geïnstalleerd.
- U moet beschikken over een abonnementssleutel voor Computer Vision. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md) voor meer informatie over het verkrijgen van een abonnementssleutel.

## <a name="create-and-run-the-sample"></a>Het voorbeeld maken en uitvoeren

U kunt het voorbeeld maken en uitvoeren aan de hand van de volgende stappen:

1. Installeer het PHP5-pakket [`HTTP_Request2`](http://pear.php.net/package/HTTP_Request2).
   1. Open een opdrachtpromptvenster als beheerder.
   1. Voer de volgende opdracht uit:

      ```console
      pear install HTTP_Request2
      ```

   1. Sluit het opdrachtpromptvenster nadat het pakket is geïnstalleerd.

1. Kopieer de volgende code naar een teksteditor.
1. Breng waar nodig de volgende wijzigingen in code aan:
    1. Vervang de waarde van `subscriptionKey` door uw abonnementssleutel.
    1. Vervang de waarde van `uriBase` door de eindpunt-URL van de methode [Miniatuur ophalen](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) uit de Azure-regio waar u uw abonnementssleutels hebt verkregen (indien nodig).
    1. Vervang optioneel de waarde van `imageUrl` door de URL van een andere afbeelding waar u een miniatuur van wilt maken.
1. Sla de code op als een bestand met de extensie `.php`. Bijvoorbeeld `get-thumbnail.php`.
1. Open een browservenster met PHP-ondersteuning.
1. Sleep het bestand naar het browservenster.

```php
<html>
<head>
    <title>Get Thumbnail Sample</title>
</head>
<body>
<?php
// Replace <Subscription Key> with a valid subscription key.
$ocpApimSubscriptionKey = '<Subscription Key>';

// You must use the same location in your REST call as you used to obtain
// your subscription keys. For example, if you obtained your subscription keys
// from westus, replace "westcentralus" in the URL below with "westus".
$uriBase = 'https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/';

$imageUrl =
    'https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg';

require_once 'HTTP/Request2.php';

$request = new Http_Request2($uriBase . 'generateThumbnail');
$url = $request->getUrl();

$headers = array(
    // Request headers
    'Content-Type' => 'application/json',
    'Ocp-Apim-Subscription-Key' => $ocpApimSubscriptionKey
);
$request->setHeader($headers);

$parameters = array(
    // Request parameters
    'width' => '100',  // Width of the thumbnail.
    'height' => '100', // Height of the thumbnail.
    'smartCropping' => 'true',
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

Een geslaagd antwoord wordt geretourneerd als binaire gegevens - deze staan voor de afbeeldingsgegevens van de miniatuur. Als de aanvraag mislukt, wordt het antwoord weergegeven in het browservenster. De reactie op de mislukte aanvraag bevat een foutcode en een bericht om u te helpen bepalen wat er mis is gegaan.

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
