---
title: Het gebruik van de Afwijkingsdetectie zoeken-API met C# - cognitieve Microsoft-Services | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met C# en de Afwijkingsdetectie zoeken-API in cognitieve Services.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 867ce4d0262c94de8da0dadeb8de71c28a8295d5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345058"
---
# <a name="use-the-anomaly-finder-api-with-c"></a>Gebruik de zoekfunctie Afwijkingsdetectie API met C#

In dit artikel bevat informatie en codevoorbeelden kunt u snel aan de slag met behulp van de Afwijkingsdetectie zoeken-API met C# taak van het resultaat van de afwijkingsdetectie reeks gegevens ophalen uit te voeren.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="getting-anomaly-points-with-anomaly-finder-api-using-c"></a>Afwijkingsdetectie punten ophalen met Afwijkingsdetectie zoeken-API met C#

[!INCLUDE [DataContract](../includes/datacontract.md)]

### <a name="example-of-time-series-data-points"></a>Voorbeeld van gegevenspunten van reeksen tijd

In het voorbeeld van de gegevenspunten van reeksen tijd is als volgt.
[!INCLUDE [Request](../includes/request.md)]

### <a name="analyze-data-and-get-anomaly-points-c-example"></a>Gegevens analyseren en ophalen van de afwijkingsdetectie punten C#-voorbeeld

De stappen van het gebruik van het voorbeeld zijn als volgt.

1. Maak een nieuwe Console-oplossing in Visual Studio.
2. Program.cs vervangen door de volgende code en de verwijzing naar System.Net.Http toevoegen.
3. Vervang `[YOUR_SUBSCRIPTION_KEY]` waarde met de sleutel geldig abonnement.
4. Vervang `[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]` met uw gegevenspunten.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Text;
using System.Text.RegularExpressions;
using System.Threading.Tasks;

namespace Console
{
    class Program
    {
        // **********************************************
        // *** Update or verify the following values. ***
        // **********************************************

        // Replace the subscriptionKey string value with your valid subscription key.
        const string subscriptionKey = "[YOUR_SUBSCRIPTION_KEY]";

        const string endpoint = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

        // Replace the request data with your real data.
        const string requestData = "[REPLACE_WITH_THE_EXAMPLE_OR_YOUR_OWN_DATA_POINTS]";
        static void Main(string[] args)
        {
            var match = Regex.Match(endpoint, "(?<BaseAddress>https://[^/]+)(?<Url>/*.+)");
            if (match.Success)
            {
                var res = Request(
                    match.Groups["BaseAddress"].Value,
                    match.Groups["Url"].Value,
                    subscriptionKey,
                    requestData).Result;
                System.Console.Write(res);
            }
            else
            {
                System.Console.Write("Incorrect endpoint.");
            }
        }

        /// <summary>
        /// Call API to detect the anomaly points
        /// </summary>
        /// <param name="baseAddress">Base address of the API endpoint.</param>
        /// <param name="endpoint">The endpoint of the API</param>
        /// <param name="subscriptionKey">The subscription key applied  </param>
        /// <param name="requestData">The JSON string for requet data points</param>
        /// <returns>The JSON string for anomaly points and expected values.</returns>
        static async Task<string> Request(string baseAddress, string endpoint, string subscriptionKey, string requestData)
        {
            using (HttpClient client = new HttpClient { BaseAddress = new Uri(baseAddress) })
            {
                client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
                client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

                var content = new StringContent(requestData, Encoding.UTF8, "application/json");
                var res = await client.PostAsync(endpoint, content);
                if (res.IsSuccessStatusCode)
                {
                    return await res.Content.ReadAsStringAsync();
                }
                else
                {
                    return $"ErrorCode: {res.StatusCode}";
                }
            }
        }
    }
}

```

### <a name="example-response"></a>Voorbeeld van een antwoord

Een geslaagde reactie wordt geretourneerd als JSON. De voorbeeld-antwoord is als volgt.
[!INCLUDE [Response](../includes/response.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [C#-app](../tutorials/csharp-tutorial.md)
