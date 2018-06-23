---
title: De Afwijkingsdetectie zoeken-API gebruiken met PHP - cognitieve Microsoft-Services | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met Afwijkingsdetectie zoeken met PHP in cognitieve Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: f81c99b77f931b5b259633fa8fcd0bf3e358e281
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345056"
---
# <a name="use-the-anomaly-finder-api-with-php"></a>Gebruik de zoekfunctie Afwijkingsdetectie API met PHP

In dit artikel bevat informatie en codevoorbeelden kunt u snel aan de slag met de Afwijkingsdetectie zoeken-API met PHP de taak van het ophalen van afwijkingsdetectie resultaat voor reeksgegevens tijd uit te voeren.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-php"></a>Afwijkingsdetectie punten ophalen met Afwijkingsdetectie zoeken-API met PHP
[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Voorbeeld van een reeksgegevens
Het voorbeeld van het gegevenstype van de reeks tijd is als volgt.
[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-php-example"></a>Gegevens analyseren en afwijkingsdetectie punten PHP-voorbeeld
1. Vervang de `[YOUR_SUBSCRIPTION_KEY]` waarde met de sleutel geldig abonnement.
2. Vervang de `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` met het voorbeeld of uw eigen gegevenspunten.
3. Uitvoeren en controleren van het antwoord.

```PHP
<?php
# This sample uses the Apache HTTP client from HTTP components (http://hc.apache.org/httpcomponents-client-ga/)
require_once 'HTTP/Request2.php';

$request = new HTTP_Request2('https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection');
$url = $request->getUrl();

$requestData = '[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]';

$headers = array(
    # Request headers
    'Content-Type' => 'application/json',
    # NOTE: Replace the "Ocp-Apim-Subscription-Key" value with a valid subscription key.
    'Ocp-Apim-Subscription-Key' => '[YOUR_SUBSCRIPTION_KEY]',
);

$request->setHeader($headers);

$request->setMethod(HTTP_Request2::METHOD_POST);

# Request body
$request->setBody($requestData);

try
{
    $response = $request->send();
    echo $response->getBody();
}
catch (HttpException $ex)
{
    echo $ex;
}
?>
```

### <a name="example-response"></a>Voorbeeld van een antwoord

Een geslaagde reactie wordt geretourneerd als JSON. Voorbeeldreactie is als volgt.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
