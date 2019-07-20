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
ms.date: 07/18/2019
ms.author: assafi
ms.openlocfilehash: 09713528f51675f6e9d7f3073b6c81b095d23631
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68356974"
---
# <a name="quickstart-use-the-net-sdk-and-c-to-call-the-text-analytics-service"></a>Quickstart: De .NET-SDK gebruiken C# en de Text Analytics-Service aanroepen
<a name="HOLTop"></a>

Deze Quick Start helpt u de Azure SDK voor .NET te gebruiken C# en om taal te analyseren. Hoewel de [Text Analytics](//go.microsoft.com/fwlink/?LinkID=759711) rest API compatibel is met de meeste programmeer talen, biedt de SDK een gemakkelijke manier om de service te integreren in uw toepassingen.

> [!NOTE]
> De broncode voor dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/TextAnalytics).

Raadpleeg de [Naslag informatie](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet)voor de SDK voor .net Text Analytics voor technische informatie.

## <a name="prerequisites"></a>Vereisten

* Elke versie van [Visual Studio 2017 of hoger]
* De Text Analytics [SDK voor .net](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

U hebt ook het [eind punt en de toegangs sleutel](../How-tos/text-analytics-how-to-access-key.md) nodig die voor u zijn gegenereerd tijdens het aanmelden.

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
        SentimentAnalysisExample(client).Wait();
        // DetectLanguageExample(client).Wait();
        // RecognizeEntitiesExample(client).Wait();
        // KeyPhraseExtractionExample(client).Wait();
        Console.ReadLine();
    }
```

In de volgende secties wordt beschreven hoe u elke service functie aanroept.

## <a name="perform-sentiment-analysis"></a>Sentiment-analyse uitvoeren

1. Maak een nieuwe functie `SentimentAnalysisExample()` die de-client gaat gebruiken die u eerder hebt gemaakt.
2. Genereer een lijst `MultiLanguageInput` met objecten die de documenten bevatten die u wilt analyseren.

    ```csharp
    public static async Task SentimentAnalysisExample(TextAnalyticsClient client)
    {
        // The documents to be analyzed. Add the language of the document. The ID can be any value.
        var inputDocuments = new MultiLanguageBatchInput(
            new List<MultiLanguageInput>
            {
                new MultiLanguageInput("en", "1", "I had the best day of my life.")
            });
        //...
    }
    ```

3. Roep `client.SentimentAsync()` en haal het resultaat op in dezelfde functie. Herhaal vervolgens de resultaten. Elke document-ID en sentiment-Score afdrukken. Een score die dicht bij 0 aangeeft, duidt op een negatieve sentiment, terwijl een score die dichter bij 1 staat, een positief sentiment aangeeft.

    ```csharp
    var result = await client.SentimentAsync(false, inputDocuments);

    // Printing sentiment results
    foreach (var document in result.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} , Sentiment Score: {document.Score:0.00}");
    }
    ```

### <a name="output"></a>Output

```console
Document ID: 1 , Sentiment Score: 0.87
```

## <a name="perform-language-detection"></a>Taal detectie uitvoeren

1. Maak een nieuwe functie `DetectLanguageExample()` die de-client gaat gebruiken die u eerder hebt gemaakt.
2. Genereer een lijst `LanguageInput` met objecten die uw documenten bevat.

    ```csharp
    public static async Task DetectLanguageExample(TextAnalyticsClient client)
    {

        // The documents to be submitted for language detection. The ID can be any value.
        var inputDocuments = new LanguageBatchInput(
                new List<LanguageInput>
                    {
                        new LanguageInput(id: "1", text: "This is a document written in English.")
                    });
        //...
    }
    ```

3. Roep `client.DetectLanguageAsync()` en haal het resultaat op in dezelfde functie. Herhaal vervolgens de resultaten. Elke document-ID en de eerste geretourneerde taal afdrukken.

    ```csharp
    var langResults = await client.DetectLanguageAsync(false, inputDocuments);

    // Printing detected languages
    foreach (var document in langResults.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} , Language: {document.DetectedLanguages[0].Name}");
    }
    ```

### <a name="output"></a>Output

```console
===== LANGUAGE EXTRACTION ======
Document ID: 1 , Language: English
```

## <a name="perform-entity-recognition"></a>Entiteits herkenning uitvoeren

1. Maak een nieuwe functie `RecognizeEntitiesExample()` die de-client gaat gebruiken die u eerder hebt gemaakt.
2. Genereer een lijst `MultiLanguageBatchInput` met objecten die uw documenten bevat.

    ```csharp
    public static async Task RecognizeEntitiesExample(TextAnalyticsClient client)
    {
        // The documents to be submitted for entity recognition. The ID can be any value.
        var inputDocuments = new MultiLanguageBatchInput(
            new List<MultiLanguageInput>
            {
                new MultiLanguageInput("en", "1", "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800.")
            });
        //...
    }
    ```

3. Roep `client.EntitiesAsync()` en haal het resultaat op in dezelfde functie. Herhaal vervolgens de resultaten. De ID van elk document afdrukken. Voor elke gedetecteerde entiteit drukt u de naam van de Wikipedia en het type en subtype (indien aanwezig) en de locaties in de oorspronkelijke tekst af.

    ```csharp
    var entitiesResult = await client.EntitiesAsync(false, inputDocuments);

    // Printing recognized entities
    foreach (var document in entitiesResult.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} ");

        Console.WriteLine("\t Entities:");
        foreach (var entity in document.Entities)
        {
            Console.WriteLine($"\t\tName: {entity.Name},\tType: {entity.Type ?? "N/A"},\tSub-Type: {entity.SubType ?? "N/A"}");
            foreach (var match in entity.Matches)
            {
                Console.WriteLine($"\t\t\tOffset: {match.Offset},\tLength: {match.Length},\tScore: {match.EntityTypeScore:F3}");
            }
        }
    }
    ```

### <a name="output"></a>Output

```console
Document ID: 1
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
2. Genereer een lijst `MultiLanguageBatchInput` met objecten die uw documenten bevat.

    ```csharp
    public static async Task KeyPhraseExtractionExample(TextAnalyticsClient client)
    {
        var inputDocuments = new MultiLanguageBatchInput(
                    new List<MultiLanguageInput>
                    {
                        new MultiLanguageInput("en", "1", "My cat might need to see a veterinarian.")
                    });
        //...
    }
    ```

3. Roep `client.KeyPhrasesAsync()` en haal het resultaat op in dezelfde functie. Herhaal vervolgens de resultaten. Elke document-ID en alle gedetecteerde sleutel zinnen afdrukken.

    ```csharp
    var kpResults = await client.KeyPhrasesAsync(false, inputDocuments);

    // Printing keyphrases
    foreach (var document in kpResults.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} ");

        Console.WriteLine("\t Key phrases:");

        foreach (string keyphrase in document.KeyPhrases)
        {
            Console.WriteLine($"\t\t{keyphrase}");
        }
    }
    ```

### <a name="output"></a>Output

```console
Document ID: 1
         Key phrases:
                cat
                veterinarian
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Text Analytics met Power BI](../tutorials/tutorial-power-bi-key-phrases.md)


* [Overzicht van Text Analytics](../overview.md)
* [Veelgestelde vragen](../text-analytics-resource-faq.md)
