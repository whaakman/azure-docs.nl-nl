---
title: Anomaliedetectie-app voor detectie van C# - Microsoft Cognitive Services | Microsoft Docs
description: Een C#-app die gebruikmaakt van de API voor Afwijkingsdetectie in Microsoft Cognitive Services verkennen. Oorspronkelijke gegevenspunten verzenden naar API en de verwachte waarde en anomaliedetectie punten.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 2e4100fd7d8e85a6b103c31000176aaaeb3d7151
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38317676"
---
# <a name="anomaly-detection-c-application"></a>Anomaly Detection C#-toepassing

Verken een eenvoudige Windows-toepassing die gebruikmaakt van de API voor Afwijkingsdetectie voor het detecteren van afwijkingen van de invoer. Het voorbeeld verzendt de time series-gegevens naar de API voor Afwijkingsdetectie met uw abonnementssleutel en vervolgens worden de anomaliedetectie-punten en verwachte waarde opgehaald voor elk gegevenspunt van de API.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Het voorbeeld is ontwikkeld voor .NET Framework met behulp van [Visual Studio 2017, Community Edition](https://www.visualstudio.com/products/visual-studio-community-vs). 

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonneer u op de detectie van afwijkingen en de abonnementssleutel van een ophalen 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Halen en gebruik het voorbeeld

U kunt de detectie van afwijkingen voorbeeld van de toepassing naar uw computer vanuit klonen [Github](https://github.com/MicrosoftAnomalyDetection/csharp-sample.git). 
<a name="Step1"></a>
### <a name="install-the-example"></a>Installeren in het voorbeeld

Open in uw GitHub-Desktop Sample\AnomalyDetectionSample.sln.

<a name="Step2"></a>
### <a name="build-the-example"></a>Bouw het voorbeeld

Druk op Ctrl + Shift + B, of klikt u op Build in het lintmenu en selecteer vervolgens oplossing bouwen.

<a name="Step3"></a>
### <a name="run-the-example"></a>Het voorbeeld uitvoeren

1. Nadat de build is voltooid, drukt u op **F5** of klik op **Start** op het lintmenu om uit te voeren in het voorbeeld.
2. Ga naar het venster voor detectie van afwijkingen gebruikersinterface met het tekstinvoervak lezen '{your_subscription_key}'.
3. Het bestand request.json, waarin de voorbeeldgegevens, vervangen door uw eigen gegevens en klik op de knop 'Verzenden'. Microsoft ontvangt de gegevens die u uploaden en deze gebruiken voor het detecteren van afwijkingen points tussen vervolgens. De gegevens die u hebt bijgewerkt wordt niet permanent worden opgeslagen in de Microsoft-server. Voor het detecteren van de punt van afwijkingen, u moet de gegevens uploaden opnieuw.
4. Als de gegevens goed is, vindt u het resultaat van de detectie van afwijkingen in het veld 'Response'. Als er een fout optreedt, worden gegevens van de fout wordt weergegeven in het veld antwoord ook.

<a name="Review"></a>
### <a name="read-the-result"></a>Het resultaat lezen

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Controleer en meer

Nu dat u een toepassing die wordt uitgevoerd hebt, laten we bekijken hoe de voorbeeld-app kan worden geïntegreerd met Cognitive Services-technologie. Deze stap maakt het eenvoudiger om door te gaan met bouwen naar deze app of uw eigen app ontwikkelen met Microsoft-Anomaliedetectie.

Deze voorbeeldapp maakt gebruik van de Restful API voor Afwijkingsdetectie eindpunt.

Laten we een codefragment van bekeken hoe de Restful-API wordt gebruikt in de voorbeeldtoepassing, kijken **AnomalyDetectionClient.cs**. Het bestand bevat codeopmerkingen waarmee wordt aangegeven "Sleutel voorbeeld CODE BEGINT hier" en 'Sleutel voorbeeld CODE EINDIGT hier' om te zoeken naar de code codefragmenten hieronder ziet.

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
**Request(...)**  Onder code fragment toont hoe u gebruik van de HttlClient indienen van uw abonnement sleutel en gegevens verwijst naar het eindpunt van de API voor Afwijkingsdetectie.

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
