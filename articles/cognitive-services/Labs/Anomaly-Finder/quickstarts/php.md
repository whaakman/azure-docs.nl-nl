---
title: De Anomaliedetectie Finder-API gebruiken met PHP - Microsoft Cognitive Services | Microsoft Docs
description: Get-informatie en codevoorbeelden om u te helpen snel aan de slag met behulp van Afwijkingsdetectie met PHP in Cognitive Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: e5262de518e6cdf5a131fa9a84a3aa8ce9ce9236
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213681"
---
# <a name="use-the-anomaly-finder-api-with-php"></a>De API voor Afwijkingsdetectie gebruiken met PHP

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Dit artikel bevat informatie en voorbeelden van code om u te helpen snel aan de slag met behulp van de Anomaliedetectie Finder-API met PHP om uit te voeren van de taak van het ophalen van de anomaliedetectie-resultaat voor time series-gegevens.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-php"></a>Anomaliedetectie punten ophalen met afwijkingen Finder-API met behulp van PHP
[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Voorbeeld van time series-gegevens
Het voorbeeld van de time series-gegevens is als volgt.
[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-php-example"></a>Gegevens analyseren en anomaliedetectie punten voorbeeld van PHP
1. Vervang de waarde `[YOUR_SUBSCRIPTION_KEY]` door de geldige abonnementssleutel.
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

Een geslaagd antwoord wordt geretourneerd in JSON-indeling. Voorbeeldantwoord is als volgt.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
