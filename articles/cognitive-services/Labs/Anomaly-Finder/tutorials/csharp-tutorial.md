---
title: 'Zelfstudie: Anomaliedetectie, C#'
titlesuffix: Azure Cognitive Services
description: Maak kennis met een C#-app die gebruikmaakt van de Anomaliedetectie-API. Verzend oorspronkelijke gegevenspunten naar de API en haal de verwachte waarde en anomaliepunten op.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-detection
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: f99ce765c1d9417fd5ca88b49214eca8a3b0bf49
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887646"
---
# <a name="tutorial-anomaly-detection-with-c-application"></a>Zelfstudie: Anomaliedetectie met C#-toepassing

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Maak kennis met een eenvoudige Windows-toepassing die gebruikmaakt van de Anomaliedetectie-API voor het detecteren van afwijkingen ten opzichte van de invoer. In het voorbeeld worden de gegevens van de tijdreeks naar de Anomaliedetectie-API verzonden met uw abonnementssleutel en worden alle anomaliepunten en de verwachte waarde voor elk gegevenspunt uit de API opgehaald.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Het voorbeeld is ontwikkeld voor het .NET Framework met behulp van [Visual Studio 2017, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs). 

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonneer u op Anomaliedetectie en haal de abonnementssleutel op 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Het voorbeeld downloaden en gebruiken

U kunt de voorbeeldtoepassing van Anomaliedetectie naar uw computer klonen vanaf [Github](https://github.com/MicrosoftAnomalyDetection/csharp-sample.git). 
<a name="Step1"></a>
### <a name="install-the-example"></a>Het voorbeeld installeren

Open Sample\AnomalyDetectionSample.sln op het bureaublad van GitHub.

<a name="Step2"></a>
### <a name="build-the-example"></a>Het voorbeeld bouwen

Druk op Ctrl+Shift+B of klik op Build in het lintmenu en selecteer vervolgens Build Solution.

<a name="Step3"></a>
### <a name="run-the-example"></a>Het voorbeeld uitvoeren

1. Nadat de build is voltooid, drukt u op **F5** of klikt u op **Start** op het lintmenu om het voorbeeld uit te voeren.
2. Ga naar het venster van de Anomaliedetectie-gebruikersinterface waarin het tekstinvoervak met de tekst '{your_subscription_key}' wordt weergegeven.
3. Vervang het bestand request.json, dat de voorbeeldgegevens bevat, door uw eigen gegevens en klik op de knop 'Verzenden'. Microsoft ontvangt de gegevens die u uploadt en gebruikt deze om daarin aanwezige anomaliepunten te detecteren. De gegevens die u laadt, worden niet permanent opgeslagen in de Microsoft-server. Om het anomaliepunt weer te kunnen detecteren, moet u de gegevens opnieuw uploaden.
4. Als de gegevens in orde zijn, vindt u het resultaat van de anomaliedetectie in het veld 'Response'. Als er een fout optreedt, worden de foutgegevens eveneens in het veld 'Response' weergegeven.

<a name="Review"></a>
### <a name="read-the-result"></a>De resultaten lezen

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Bekijken en leren

Nu u een werkende toepassing hebt, gaan we kijken hoe de voorbeeld-app kan worden geïntegreerd met Cognitive Services-technologie. Deze stap maakt het eenvoudiger om door te gaan met bouwen aan deze app of om uw eigen app te ontwikkelen met Anomaliedetectie van Microsoft.

Deze voorbeeldapp maakt gebruik van het Restful-API-eindpunt van Anomaliedetectie.

Laten we een codefragment uit **AnomalyDetectionClient.cs** gebruiken om te zien hoe de Restful-API wordt gebruikt in de voorbeeldtoepassing. Het bestand bevat codeopmerkingen die 'KEY SAMPLE CODE STARTS HERE' en 'KEY SAMPLE CODE ENDS HERE' aangeven om u te helpen bij het zoeken naar de codefragmenten die u hieronder ziet.

```csharp
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Set http request header
            // ---------------------------------------------------------------------- 
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

```
### <a name="request"></a>**Aanvraag**
Het onderstaande codefragment laat zien hoe de HttpClient wordt gebruikt bij het verzenden van uw abonnementssleutel en gegevenspunten naar het eindpunt van de Anomaliedetectie-API.

```csharp
    public async Task<string> Request(string baseAddress, string endpoint, string subscriptionKey, string requestData)
    {
        using (HttpClient client = new HttpClient { BaseAddress = new Uri(baseAddress) })
        {
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Set http request header
            // ---------------------------------------------------------------------- 
            client.DefaultRequestHeaders.Accept.Add(new MediaTypeWithQualityHeaderValue("application/json"));
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", subscriptionKey);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Build the request content
            // ---------------------------------------------------------------------- 
            var content = new StringContent(requestData, Encoding.UTF8, "application/json");
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------

            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE STARTS HERE
            // Send the request content with POST method.
            // ---------------------------------------------------------------------- 
            var res = await client.PostAsync(endpoint, content);
            // ----------------------------------------------------------------------
            // KEY SAMPLE CODE ENDS HERE 
            // ----------------------------------------------------------------------
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
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
