---
title: Afwijkingsdetectie detectie C#-app - cognitieve Services van Microsoft | Microsoft Docs
description: Verken een C#-app die gebruikmaakt van de Afwijkingsdetectie Detection-API in cognitieve Microsoft-Services. Oorspronkelijke gegevenspunten verzenden naar API en de verwachte waarde en afwijkingsdetectie punten.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 2e4100fd7d8e85a6b103c31000176aaaeb3d7151
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345067"
---
# <a name="anomaly-detection-c-application"></a>Afwijkingsdetectie detectie C#-toepassing

Gebruik een eenvoudige Windows-toepassing die gebruikmaakt van Afwijkingsdetectie Detection-API voor het detecteren van afwijkingen van de invoer. In het voorbeeld verzendt de gegevens van de reeks tijd voor de Afwijkingsdetectie Detection-API met de abonnementssleutel van uw en vervolgens alle punten afwijkingsdetectie en de verwachte waarde opgehaald voor elk gegevenspunt van de API.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

In het voorbeeld voor .NET Framework is ontwikkeld met behulp van [2017 voor Visual Studio, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs). 

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonneer u op de Anomaliedetectie en een abonnementssleutel ophalen 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Halen en gebruik het voorbeeld

U kunt de voorbeeldtoepassing Afwijkingsdetectie op uw computer uit klonen [Github](https://github.com/MicrosoftAnomalyDetection/csharp-sample.git). 
<a name="Step1"></a>
### <a name="install-the-example"></a>Installeren in het voorbeeld

Open in uw GitHub-bureaublad Sample\AnomalyDetectionSample.sln.

<a name="Step2"></a>
### <a name="build-the-example"></a>In het voorbeeld bouwen

Druk op Ctrl + Shift + B of Build Klik op het lintmenu en selecteer vervolgens oplossing bouwen.

<a name="Step3"></a>
### <a name="run-the-example"></a>Het voorbeeld uitvoert

1. Nadat de build is voltooid, drukt u op **F5** of klik op **Start** in het lintmenu om het voorbeeld uitvoert.
2. Zoek het venster Afwijkingsdetectie gebruikersinterface met de tekstinvoervak lezen '{your_subscription_key}'.
3. Het bestand request.json, waarin de voorbeeldgegevens vervangen door uw eigen gegevens en klik op de knop 'Verzenden'. Microsoft ontvangt de gegevens die u uploadt en deze gebruiken voor het detecteren van alle punten afwijkingsdetectie tussen vervolgens. De gegevens die u hebt bijgewerkt zal niet worden vastgehouden in Microsoft server. Om te detecteren van het punt afwijkingsdetectie opnieuw, moet u uploaden van de gegevens opnieuw.
4. Als de gegevens gemaakt is, vindt u het resultaat van de detectie afwijkingsdetectie in het veld "Antwoord". Als er een fout optreedt, wordt de informatie over de fout weergegeven in het veld antwoord ook.

<a name="Review"></a>
### <a name="read-the-result"></a>Lezen van het resultaat

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Bekijk en meer informatie

Nu dat u een actieve toepassing hebt, gaan we controleren hoe de voorbeeld-app kan worden geïntegreerd met cognitieve Services-technologie. Deze stap wordt het eenvoudiger te gaan gebouw naar deze app of uw eigen app met behulp van Microsoft-Afwijkingsdetectie ontwikkelen.

Deze voorbeeld-app wordt gebruikgemaakt van de Afwijkingsdetectie detectie Restful-API eindpunt.

Bekeken hoe de Restful-API wordt gebruikt in de voorbeeldtoepassing, bekijken we een codefragment uit **AnomalyDetectionClient.cs**. Het bestand bevat codeopmerkingen die aangeeft 'Sleutel voorbeeld CODE BEGINT hier' en 'Sleutel voorbeeld CODE EINDIGT hier' om te vinden van de code codefragmenten hieronder.

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
**Request(...)**  Onder code fragment toont hoe u de HttlClient verzenden van uw abonnement sleutel en gegevens verwijst naar het eindpunt van de Afwijkingsdetectie Detection-API.

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
