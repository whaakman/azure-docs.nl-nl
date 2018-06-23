---
title: De Afwijkingsdetectie zoeken-API gebruiken met Ruby - cognitieve Microsoft-Services | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met Ruby en de Afwijkingsdetectie zoeken-API in cognitieve Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: ca4754514ba5012f7e9e28981d0869d174561fb3
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345055"
---
# <a name="use-the-anomaly-finder-api-with-ruby"></a>De zoekfunctie Afwijkingsdetectie API gebruiken met Ruby

In dit artikel bevat informatie en codevoorbeelden kunt u snel aan de slag met de Afwijkingsdetectie zoeken-API met Ruby taak van de afwijkingsdetectie detectieresultaat reeks gegevens ophalen uit te voeren.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-ruby"></a>Afwijkingsdetectie punten ophalen met Afwijkingsdetectie zoeken-API met Ruby 
[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data"></a>Voorbeeld van een reeksgegevens
Het voorbeeld van de gegevenspunten van reeksen tijd is als volgt [!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-ruby-example"></a>Gegevens analyseren en afwijkingsdetectie punten Ruby voorbeeld

De stappen van het gebruik van het voorbeeld zijn als volgt.

1. Installeer [rest-client](https://github.com/rest-client/rest-client) door het uitvoeren van 'gem installeren rest-client'.
2. Sla de onderstaande code als een .rb-bestand.
3. Vervang de `[YOUR_SUBSCRIPTION_KEY]` waarde met de sleutel geldig abonnement.
4. Vervang de `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` met het voorbeeld of uw eigen gegevenspunten.
5. Uitvoeren en controleren van het antwoord.

```ruby
# https://github.com/rest-client/rest-client
require 'rest_client'

# **********************************************
# *** Update or verify the following values. ***
# **********************************************

# Replace the subscriptionKey string value with your valid subscription key.
subscription_key = '[YOUR_SUBSCRIPTION_KEY]';

endpoint = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

# Replace the request data with your real data.
requestData = '[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]';

header = {
    :content_type => 'application/json',
    :'Ocp-Apim-Subscription-Key' => subscription_key
}

response = RestClient::Request.execute(
    :url => endpoint,
    :method => :post,
    :verify_ssl => true,
    :payload => requestData,
    :header => header)

# You will see the response with anomaly results
puts response.body
```

### <a name="example-response"></a>Voorbeeld van een antwoord

Een geslaagde reactie wordt geretourneerd als JSON. Voorbeeldreactie is als volgt.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
