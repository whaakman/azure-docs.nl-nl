---
title: De Afwijkingsdetectie zoeken-API gebruiken met cURL - cognitieve Microsoft-Services | Microsoft Docs
description: Get-informatie om u te helpen snel aan de slag met cURL en de Afwijkingsdetectie zoeken-API in cognitieve Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 3c1d791b8c0478715b4ffa93cd7dfa43f9be4586
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345052"
---
# <a name="use-the-anomaly-finder-api-with-curl"></a>De Afwijkingsdetectie zoeken-API gebruiken met cURL

In dit artikel bevat informatie en codevoorbeelden kunt u snel aan de slag met behulp van de Afwijkingsdetectie zoeken-API met cURL taak van het resultaat van de afwijkingsdetectie reeks gegevens ophalen uit te voeren.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-the-anomaly-finder-api-using-curl"></a>Afwijkingsdetectie punten ophalen met de Afwijkingsdetectie zoeken-API met cURL 

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Voorbeeld van een reeksgegevens

In het voorbeeld van de gegevenspunten van reeksen tijd is als volgt.

[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-curl-example"></a>Gegevens analyseren en afwijkingsdetectie punten cURL voorbeeld

De stappen van het gebruik van het voorbeeld zijn als volgt.

1. Vervang de `[YOUR_SUBSCRIPTION_KEY]` waarde met de sleutel geldig abonnement.
2. Vervang de `[YOUR_REGION]` gebruik van de locatie waar u de sleutels van uw abonnement hebt verkregen.
3. Vervang de `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` met het voorbeeld of uw eigen gegevenspunten.
4. Uitvoeren en controleren van het antwoord.

```cURL

curl -v -X POST "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection"
-H "Content-Type: application/json"
-H "Ocp-Apim-Subscription-Key: [YOUR_SUBSCRIPTION_KEY]"
--data-ascii "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]" 

```

### <a name="example-response"></a>Voorbeeld van een antwoord
Een geslaagde reactie wordt geretourneerd als JSON. Voorbeeld van een antwoord is als volgt: [!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
