---
title: 'Quickstart: Roep de Text Analytics-service aan met behulp van de Azure SDK voor .NET enC#'
titleSuffix: Azure Cognitive Services
description: Informatie en code voorbeelden waarmee u kunt beginnen met het gebruik van de C#Text Analytics-service en.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 07/30/2019
ms.author: assafi
ms.openlocfilehash: 6bd3907392dad626c1eeb1823c929f1a35d544dd
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68697675"
---
# <a name="quickstart-use-the-net-sdk-and-c-to-call-the-text-analytics-service"></a>Quickstart: De .NET-SDK gebruiken C# en de Text Analytics-Service aanroepen
<a name="HOLTop"></a>

Deze Quick Start helpt u de Azure SDK voor .NET te gebruiken C# en om taal te analyseren. Hoewel de [Text Analytics](//go.microsoft.com/fwlink/?LinkID=759711) rest API compatibel is met de meeste programmeer talen, biedt de SDK een gemakkelijke manier om de service te integreren in uw toepassingen.

> [!NOTE]
> In de demo code in dit artikel wordt gebruikgemaakt van de synchrone methoden van de Text Analytics .NET SDK voor eenvoud. Voor productie scenario's kunt u echter het beste de batch-asynchrone methoden in uw eigen toepassingen gebruiken om ze schaalbaar en responsief te maken. U kunt bijvoorbeeld in plaats van `SentimentBatchAsync` `Sentiment`gebruiken.
>
> Een batch-asynchrone versie van dit voor beeld is te vinden op [github](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/TextAnalytics).

Raadpleeg de [Naslag informatie](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet)voor de SDK voor .net Text Analytics voor technische informatie.

## <a name="prerequisites"></a>Vereisten

* Elke versie van Visual Studio 2017 of hoger
* De Text Analytics [SDK voor .net](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

## <a name="create-the-visual-studio-solution-and-install-the-sdk"></a>De Visual Studio-oplossing maken en de SDK installeren

1. Maak een nieuw console-app-project (.NET core). [Open Visual Studio](https://visualstudio.microsoft.com/vs/).
1. Klik met de rechter muisknop op de oplossing en selecteer **NuGet-pakketten beheren voor oplossing**.
1. Selecteer het tabblad **Browse**. Zoek naar **micro soft. Azure. CognitiveServices. language. TextAnalytics**.

## <a name="authenticate-your-credentials"></a>Uw referenties verifiëren

1. Voeg de volgende `using` -instructies toe aan het bestand van de hoofd klasse (dit is standaard Program.cs).

    ```csharp
    using System;
    using System.Collections.Generic;
    using System.Net.Http;
    using System.Threading;
    using System.Threading.Tasks;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics;
    using Microsoft.Azure.CognitiveServices.Language.TextAnalytics.Models;
    using Microsoft.Rest;
    ```

2. Maak een nieuwe `ApiKeyServiceClientCredentials` klasse om de referenties op te slaan en toe te voegen voor elke aanvraag.

    ```csharp
    class ApiKeyServiceClientCredentials : ServiceClientCredentials
    {
        private readonly string apiKey;

        public ApiKeyServiceClientCredentials(string apiKey)
        {
            this.apiKey = apiKey;
        }

        public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            if (request == null)
            {
                throw new ArgumentNullException("request");
            }
            request.Headers.Add("Ocp-Apim-Subscription-Key", this.apiKey);
            return base.ProcessHttpRequestAsync(request, cancellationToken);
        }
    }
    ```

3. Werk de `Program` klasse bij. Voeg een constant lid toe voor uw Text Analytics-API sleutel en een ander voor het service-eind punt. Vergeet niet om de juiste Azure-locatie voor uw Text Analytics-resource te gebruiken.

    ```csharp
    //Enter your Text Analytics (TA) API Key (available in Azure Portal -> your TA resource -> Keys)
    private const string ApiKey = "enter-your-textanalytics-api-key-here";
    //You can get the resource location from Azure Portal -> your TA resource -> Overview
    private const string Endpoint = "enter-your-service-endpoint-here"; // For example: "https://<your-location>.api.cognitive.microsoft.com";
    ```

> [!Tip]
> Als u de beveiliging van geheimen in productie systemen wilt verbeteren, raden we u aan [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net)te gebruiken.
>

## <a name="create-a-text-analytics-client"></a>Een Text Analytics-client maken

In de `Main` functie van uw project roept u de voorbeeld methode op die u wilt aanroepen. Geef de `Endpoint` para `ApiKey` meters en op die u hebt gedefinieerd.

```csharp
    public static void Main(string[] args)
    {
        var credentials = new ApiKeyServiceClientCredentials(ApiKey);
        var client = new TextAnalyticsClient(credentials)
        {
            Endpoint = Endpoint
        };

        // Change the console encoding to display non-ASCII characters.
        Console.OutputEncoding = System.Text.Encoding.UTF8;
        SentimentAnalysisExample(client);
        // DetectLanguageExample(client);
        // RecognizeEntitiesExample(client);
        // KeyPhraseExtractionExample(client);
        Console.ReadLine();
    }
```

In de volgende secties wordt beschreven hoe u elke service functie aanroept.

## <a name="perform-sentiment-analysis"></a>Sentiment-analyse uitvoeren

1. Maak een nieuwe functie `SentimentAnalysisExample()` die de-client gaat gebruiken die u eerder hebt gemaakt.
2. Roep `client.Sentiment()` en haal het resultaat op in dezelfde functie. Het resultaat bevat de sentiment `Score` als dit is gelukt, `errorMessage` en als dat niet het geval is. Een score die dicht bij 0 aangeeft, duidt op een negatieve sentiment, terwijl een score die dichter bij 1 staat, een positief sentiment aangeeft.

    ```csharp
    var result = client.Sentiment("I had the best day of my life.", "en");

    // Printing sentiment results
    Console.WriteLine($"Sentiment Score: {result.Score:0.00}");
    ```

### <a name="output"></a>Output

```console
Sentiment Score: 0.87
```

## <a name="perform-language-detection"></a>Taal detectie uitvoeren

1. Maak een nieuwe functie `DetectLanguageExample()` die de-client gaat gebruiken die u eerder hebt gemaakt.
2. Roep `client.DetectLanguage()` en haal het resultaat op in dezelfde functie. Het resultaat bevat de lijst met gedetecteerde talen in `DetectedLanguages` als dat is gelukt `errorMessage` , en wel als dat niet het geval is. Druk vervolgens de eerste geretourneerde taal af.

    ```csharp
    var result = client.DetectLanguage("This is a document written in English.");

    // Printing detected languages
    Console.WriteLine($"Language: {result.DetectedLanguages[0].Name}");
    ```

> [!Tip]
> In sommige gevallen kan het lastig zijn om talen te dubbel zinnigheid op basis van de invoer. U kunt de `countryHint` para meter gebruiken om een land nummer van twee letters op te geven. Standaard gebruikt de API de standaard countryHint om dit gedrag te verwijderen, kunt u deze para meter opnieuw instellen door deze waarde in te stellen op een lege teken reeks `countryHint = ""` .

### <a name="output"></a>Output

```console
Language: English
```

## <a name="perform-entity-recognition"></a>Entiteits herkenning uitvoeren

1. Maak een nieuwe functie `RecognizeEntitiesExample()` die de-client gaat gebruiken die u eerder hebt gemaakt.
2. Roep `client.Entities()` en haal het resultaat op in dezelfde functie. Herhaal vervolgens de resultaten. Het resultaat bevat de lijst met gedetecteerde entiteiten in `Entities` als dat is gelukt `errorMessage` , en wel als dat niet het geval is. Voor elke gedetecteerde entiteit, het type, subtype, naam van de Wikipedia (indien aanwezig) afdrukken, evenals de locaties in de oorspronkelijke tekst.

    ```csharp
    var result = client.Entities("Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.");

    // Printing recognized entities
    Console.WriteLine("Entities:");
    foreach (var entity in result.Entities)
    {
        Console.WriteLine($"\tName: {entity.Name},\tType: {entity.Type ?? "N/A"},\tSub-Type: {entity.SubType ?? "N/A"}");
        foreach (var match in entity.Matches)
        {
            Console.WriteLine($"\t\tOffset: {match.Offset},\tLength: {match.Length},\tScore: {match.EntityTypeScore:F3}");
        }
    }
    ```

### <a name="output"></a>Output

```console
Entities:
    Name: Microsoft,        Type: Organization,     Sub-Type: N/A
        Offset: 0,      Length: 9,      Score: 1.000
    Name: Bill Gates,       Type: Person,   Sub-Type: N/A
        Offset: 25,     Length: 10,     Score: 1.000
    Name: Paul Allen,       Type: Person,   Sub-Type: N/A
        Offset: 40,     Length: 10,     Score: 0.999
    Name: April 4,  Type: Other,    Sub-Type: N/A
        Offset: 54,     Length: 7,      Score: 0.800
    Name: April 4, 1975,    Type: DateTime, Sub-Type: Date
        Offset: 54,     Length: 13,     Score: 0.800
    Name: BASIC,    Type: Other,    Sub-Type: N/A
        Offset: 89,     Length: 5,      Score: 0.800
    Name: Altair 8800,      Type: Other,    Sub-Type: N/A
        Offset: 116,    Length: 11,     Score: 0.800
```

## <a name="perform-key-phrase-extraction"></a>Extractie van sleutel woorden uitvoeren

1. Maak een nieuwe functie `KeyPhraseExtractionExample()` die de-client gaat gebruiken die u eerder hebt gemaakt.
2. Roep `client.KeyPhrases()` en haal het resultaat op in dezelfde functie. Het resultaat bevat de lijst met gedetecteerde sleutel zinnen in `KeyPhrases` als dit is geslaagd, `errorMessage` en een als dat niet zo is. Druk vervolgens een of meer gedetecteerde sleutel zinnen af.

    ```csharp
    var result = client.KeyPhrases("My cat might need to see a veterinarian.");

    // Printing key phrases
    Console.WriteLine("Key phrases:");

    foreach (string keyphrase in result.KeyPhrases)
    {
        Console.WriteLine($"\t{keyphrase}");
    }
    ```

### <a name="output"></a>Output

```console
Key phrases:
    cat
    veterinarian
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Text Analytics met Power BI](../tutorials/tutorial-power-bi-key-phrases.md)


* [Overzicht van Text Analytics](../overview.md)
* [Veelgestelde vragen](../text-analytics-resource-faq.md)
