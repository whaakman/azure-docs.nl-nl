---
title: 'Quickstart: De Text Analytics-service aanroepen met behulp van de Azure SDK voor .NET enC#'
titleSuffix: Azure Cognitive Services
description: Informatie en codevoorbeelden beginnen met de Text Analytics-service en C#.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 04/29/2019
ms.author: assafi
ms.openlocfilehash: 44d4a9427fcf6b582d44707127b87c262781520f
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65602364"
---
# <a name="quickstart-use-the-net-sdk-and-c-to-call-the-text-analytics-service"></a>Quickstart: De .NET SDK gebruiken en C# de Text Analytics-service aanroepen
<a name="HOLTop"></a>

Deze Quick Start kunt u beginnen met de Azure SDK voor .NET en C# voor het analyseren van taal. Hoewel de [Tekstanalyse](//go.microsoft.com/fwlink/?LinkID=759711) REST-API is compatibel met de meeste moderne programmeertalen, de SDK biedt een eenvoudige manier om de service te integreren in uw toepassingen.

> [!NOTE]
> De broncode voor dit voorbeeld is beschikbaar op [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/TextAnalytics).

Raadpleeg voor technische details voor de SDK voor .NET [Tekstanalyse verwijzen naar](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/textanalytics?view=azure-dotnet).

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

U moet ook de [eindpunt en de toegangssleutel](../How-tos/text-analytics-how-to-access-key.md) die voor u is gegenereerd tijdens de registratie.

> [!Tip]
>  Terwijl u kunt aanroepen de [HTTP-eindpunten](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) rechtstreeks vanuit C#, de SDK Microsoft.Azure.CognitiveServices.Language veel gemakkelijker om aan te roepen van de service zonder te serialiseren en deserialiseren van JSON.
>
> Een paar handige koppelingen:
> - [SDK NuGet-pagina](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)
> - [SDK-code](https://github.com/Azure/azure-sdk-for-net/tree/master/src/SDKs/CognitiveServices/dataPlane/Language/TextAnalytics)

## <a name="create-the-visual-studio-solution-and-install-the-sdk"></a>De Visual Studio-oplossing maken en installeer de SDK

1. Maak een nieuw project voor de console-app (.NET Core). [Access Visual Studio](https://visualstudio.microsoft.com/vs/).
1. Met de rechtermuisknop op de oplossing en selecteer **NuGet-pakketten beheren voor oplossing**.
1. Selecteer het tabblad **Browse**. Zoeken naar **Microsoft.Azure.CognitiveServices.Language.TextAnalytics**.

## <a name="authenticate-your-credentials"></a>Verificatie van uw referenties

1. Voeg de volgende `using` instructies toe aan de main-klasse-bestand (dit is Program.cs standaard).

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

2. Maak een nieuwe `ApiKeyServiceClientCredentials` klasse voor het opslaan van de referenties en toe te voegen voor elke aanvraag.

    ```csharp
    /// <summary>
    /// Allows authentication to the API by using a basic apiKey mechanism
    /// </summary>
    class ApiKeyServiceClientCredentials : ServiceClientCredentials
    {
        private readonly string subscriptionKey;

        /// <summary>
        /// Creates a new instance of the ApiKeyServiceClientCredentails class
        /// </summary>
        /// <param name="subscriptionKey">The subscription key to authenticate and authorize as</param>
        public ApiKeyServiceClientCredentials(string subscriptionKey)
        {
            this.subscriptionKey = subscriptionKey;
        }

        /// <summary>
        /// Add the Basic Authentication Header to each outgoing request
        /// </summary>
        /// <param name="request">The outgoing request</param>
        /// <param name="cancellationToken">A token to cancel the operation</param>
        public override Task ProcessHttpRequestAsync(HttpRequestMessage request, CancellationToken cancellationToken)
        {
            if (request == null)
            {
                throw new ArgumentNullException("request");
            }

            request.Headers.Add("Ocp-Apim-Subscription-Key", this.subscriptionKey);
            return base.ProcessHttpRequestAsync(request, cancellationToken);
        }
    }
    ```

3. Update de `Program` klasse. Een constante lid op voor uw abonnementssleutel Text Analytics en een andere voor het service-eindpunt toevoegen. Geef de juiste Azure-regio voor uw Text Analytics-abonnement.

    ```csharp
    private const string SubscriptionKey = "enter-your-key-here";

    private const string Endpoint = "enter-your-service-endpoint-here"; // For example: "https://westus.api.cognitive.microsoft.com";
    ```
> [!Tip]
> Om te verbeteren de beveiliging van geheimen in productiesystemen, raden wij aan dat u [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net).
>

## <a name="create-a-text-analytics-client"></a>Een Text Analytics-client maken

In de `Main` functie van uw project, roept u de voorbeeldmethode die u wilt aanroepen. Geeft de `Endpoint` en `SubscriptionKey` parameters die u hebt gedefinieerd.

```csharp
    public static void Main(string[] args)
    {
        var credentials = new ApiKeyServiceClientCredentials(SubscriptionKey);
        var client = new TextAnalyticsClient(credentials)
        {
            //Replace 'westus' with the correct region for your Text Analytics subscription.
            Endpoint = "https://westus.api.cognitive.microsoft.com"
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

De volgende secties wordt beschreven hoe u elke service-functie aanroepen.

## <a name="perform-sentiment-analysis"></a>Uitvoeren van sentimentanalyse

1. Maak een nieuwe functie `SentimentAnalysisExample()` waarmee de client die u eerder hebt gemaakt.
2. Een lijst genereren `MultiLanguageInput` objecten in de documenten die u wilt analyseren.

    ```csharp
    public static async Task SentimentAnalysisExample(TextAnalyticsClient client)
    {
        // The documents to be analyzed. Add the language of the document. The ID can be any value.
        var inputDocuments = new MultiLanguageBatchInput(
            new List<MultiLanguageInput>
            {
                new MultiLanguageInput("en", "1", "I had the best day of my life."),
                new MultiLanguageInput("en", "2", "This was a waste of my time. The speaker put me to sleep."),
                new MultiLanguageInput("es", "3", "No tengo dinero ni nada que dar..."),
                new MultiLanguageInput("it", "4", "L'hotel veneziano era meraviglioso. È un bellissimo pezzo di architettura."),
            });
        //...
    }
    ```

3. In de dezelfde functie aanroepen `client.SentimentAsync()` en het resultaat. Vervolgens de resultaten doorlopen. Afdrukken van elk document-ID en het gevoel score. Een score die zich in de buurt van 0 geeft aan dat een negatief gevoel, terwijl een score die dichter bij 1 geeft aan een positieve stemming dat.

    ```csharp
    var result = await client.SentimentAsync(false, inputDocuments);

    // Printing sentiment results
    foreach (var document in result.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} , Sentiment Score: {document.Score:0.00}");
    }
    ```

### <a name="output"></a>Uitvoer

```console
Document ID: 1 , Sentiment Score: 0.87
Document ID: 2 , Sentiment Score: 0.11
Document ID: 3 , Sentiment Score: 0.44
Document ID: 4 , Sentiment Score: 1.00
```

## <a name="perform-language-detection"></a>Taaldetectie uitvoeren

1. Maak een nieuwe functie `DetectLanguageExample()` waarmee de client die u eerder hebt gemaakt.
2. Een lijst genereren `LanguageInput` objecten die uw documenten bevat.

    ```csharp
    public static async Task DetectLanguageExample(TextAnalyticsClient client)
    {

        // The documents to be submitted for language detection. The ID can be any value.
        var inputDocuments = new LanguageBatchInput(
                new List<LanguageInput>
                    {
                        new LanguageInput(id: "1", text: "This is a document written in English."),
                        new LanguageInput(id: "2", text: "Este es un document escrito en Español."),
                        new LanguageInput(id: "3", text: "这是一个用中文写的文件")
                    });
        //...
    }
    ```

3. In de dezelfde functie aanroepen `client.DetectLanguageAsync()` en het resultaat. Vervolgens de resultaten doorlopen. Afdrukken van elk document-ID en de taal van de eerste geretourneerd.

    ```csharp
    var langResults = await client.DetectLanguageAsync(false, inputDocuments);

    // Printing detected languages
    foreach (var document in langResults.Documents)
    {
        Console.WriteLine($"Document ID: {document.Id} , Language: {document.DetectedLanguages[0].Name}");
    }
    ```

### <a name="output"></a>Uitvoer

```console
===== LANGUAGE EXTRACTION ======
Document ID: 1 , Language: English
Document ID: 2 , Language: Spanish
Document ID: 3 , Language: Chinese_Simplified
```

## <a name="perform-entity-recognition"></a>Herkenning van entity's uitvoeren

1. Maak een nieuwe functie `RecognizeEntitiesExample()` waarmee de client die u eerder hebt gemaakt.
2. Een lijst genereren `MultiLanguageBatchInput` objecten die uw documenten bevat.

    ```csharp
    public static async Task RecognizeEntitiesExample(TextAnalyticsClient client)
    {

        // The documents to be submitted for entity recognition. The ID can be any value.
        var inputDocuments = new MultiLanguageBatchInput(
            new List<MultiLanguageInput>
            {
                new MultiLanguageInput("en", "1", "Microsoft was founded by Bill Gates and Paul Allen on April 4, 1975, to develop and sell BASIC interpreters for the Altair 8800."),
                new MultiLanguageInput("es", "2", "La sede principal de Microsoft se encuentra en la ciudad de Redmond, a 21 kilómetros de Seattle.")
            });
        //...
    }
    ```

3. In de dezelfde functie aanroepen `client.EntitiesAsync()` en het resultaat. Vervolgens de resultaten doorlopen. Afdrukken van elk document-ID. Afdrukken voor elke gedetecteerde entiteit de Wikipedia-naam en het type en de subtypen (indien aanwezig) evenals de locaties in de oorspronkelijke tekst.

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

### <a name="output"></a>Uitvoer

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
Document ID: 2
         Entities:
                Name: Microsoft,        Type: Organization,     Sub-Type: N/A
                        Offset: 21,     Length: 9,      Score: 1.000
                Name: Redmond (Washington),     Type: Location, Sub-Type: N/A
                        Offset: 60,     Length: 7,      Score: 0.991
                Name: 21 kilómetros,    Type: Quantity, Sub-Type: Dimension
                        Offset: 71,     Length: 13,     Score: 0.800
                Name: Seattle,  Type: Location, Sub-Type: N/A
                        Offset: 88,     Length: 7,      Score: 1.000
```

## <a name="perform-key-phrase-extraction"></a>Sleuteltermextractie uitvoeren

1. Maak een nieuwe functie `KeyPhraseExtractionExample()` waarmee de client die u eerder hebt gemaakt.
2. Een lijst genereren `MultiLanguageBatchInput` objecten die uw documenten bevat.

    ```csharp
    public static async Task KeyPhraseExtractionExample(TextAnalyticsClient client)
    {
        var inputDocuments = new MultiLanguageBatchInput(
                    new List<MultiLanguageInput>
                    {
                        new MultiLanguageInput("ja", "1", "猫は幸せ"),
                        new MultiLanguageInput("de", "2", "Fahrt nach Stuttgart und dann zum Hotel zu Fu."),
                        new MultiLanguageInput("en", "3", "My cat might need to see a veterinarian."),
                        new MultiLanguageInput("es", "4", "A mi me encanta el fútbol!")
                    });
        //...
    }
    ```

3. In de dezelfde functie aanroepen `client.KeyPhrasesAsync()` en het resultaat. Vervolgens de resultaten doorlopen. Afdrukken van elk document-ID en eventuele gedetecteerde sleuteltermen.

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

### <a name="output"></a>Uitvoer

```console
Document ID: 1
         Key phrases:
                幸せ
Document ID: 2
         Key phrases:
                Stuttgart
                Hotel
                Fahrt
                Fu
Document ID: 3
         Key phrases:
                cat
                veterinarian
Document ID: 4
         Key phrases:
                fútbol
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Text Analytics met Power BI](../tutorials/tutorial-power-bi-key-phrases.md)


* [Overzicht van Text Analytics](../overview.md)
* [Veelgestelde vragen](../text-analytics-resource-faq.md)
