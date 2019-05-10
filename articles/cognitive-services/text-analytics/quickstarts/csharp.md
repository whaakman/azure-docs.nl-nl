---
title: 'Quickstart: C# gebruiken om de Text Analytics-API aan te roepen'
titleSuffix: Azure Cognitive Services
description: Bekijk informatie en codevoorbeelden om snel aan de slag te gaan met de Text Analytics-API.
services: cognitive-services
author: raymondl
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: quickstart
ms.date: 04/29/2019
ms.author: assafi
ms.openlocfilehash: c521be03f884227116a21c8c5396d47cdd1ae253
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/09/2019
ms.locfileid: "65466541"
---
# <a name="quickstart-using-c-to-call-the-text-analytics-cognitive-service"></a>Quickstart: C# gebruiken om de Text Analytics Cognitive Service aan te roepen
<a name="HOLTop"></a>

In deze snelstartgids gebruiken om te beginnen met het analyseren van de taal met de Text Analytics-SDK voor C#. Terwijl de [Tekstanalyse](//go.microsoft.com/fwlink/?LinkID=759711) REST-API is compatibel met de meeste moderne programmeertalen, de SDK biedt een eenvoudige manier om de service te integreren in uw toepassingen. De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/TextAnalytics).

Raadpleeg de [API-definities](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c7) voor technische documentatie voor de API's.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [cognitive-services-text-analytics-signup-requirements](../../../../includes/cognitive-services-text-analytics-signup-requirements.md)]

U moet ook de [eindpunt- en toegangssleutel](../How-tos/text-analytics-how-to-access-key.md) hebben die voor u is gegenereerd tijden de registratie.

> [!Tip]
>  Hoewel u de [HTTP-eindpunten](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics-v2-1/operations/56f30ceeeda5650db055a3c9) rechtstreeks vanuit C# kunt aanroepen, maakt de SDK Microsoft.Azure.CognitiveServices.Language het veel gemakkelijker om de service aan te roepen zonder dat u zich zorgen hoeft te maken over het serialiseren en deserialiseren van JSON.
>
> Een paar handige koppelingen:
> - [Pagina over het NuGet-pakket voor de SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.TextAnalytics)
> - [SDK-code](https://github.com/Azure/azure-sdk-for-net/tree/master/src/SDKs/CognitiveServices/dataPlane/Language/TextAnalytics)

## <a name="create-the-visual-studio-solution-and-install-the-sdk"></a>De Visual Studio-oplossing maken en installeer de SDK

1. Maak een nieuw project in de Console-App (.NET Core) [Visual Studio](https://visualstudio.microsoft.com/vs/).
1. Klik met de rechtermuisknop op de oplossing en klik op **Manage NuGet Packages for Solution**.
1. Selecteer het tabblad **Browse** en zoek naar **Microsoft.Azure.CognitiveServices.Language.TextAnalytics**

## <a name="authenticate-your-credentials"></a>Verificatie van uw referenties

1. Voeg de volgende `using` instructies toe aan het bestand main-klasse (`Program.cs` standaard).

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
    /// Allows authentication to the API using a basic apiKey mechanism
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

3. Update de `Program` klasse en het toevoegen van een constante lid op voor uw abonnementssleutel Text Analytics en een andere voor het service-eindpunt. Geef de juiste Azure-regio voor uw Text Analytics-abonnement.

    ```csharp
    private const string SubscriptionKey = "enter-your-key-here";

    private const string Endpoint = "enter-your-service-endpoint-here"; // For example: "https://westus.api.cognitive.microsoft.com";
    ```
> [!Tip]
> Voor de veilige distributie van geheimen in productiesystemen wordt u aangeraden [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/quick-create-net)
>

## <a name="create-a-text-analytics-client"></a>Een Text Analytics-client maken

In de `Main` functie van uw project, roept u de voorbeeldmethode die u wilt aanroepen en door te geven de `Endpoint` en `SubscriptionKey` parameters die u hebt gedefinieerd.

```csharp
    public static void Main(string[] args)
    {
        var credentials = new ApiKeyServiceClientCredentials(SubscriptionKey);
        var client = new TextAnalyticsClient(credentials)
        {
            //Replace 'westus' with the correct region for your Text Analytics subscription
            Endpoint = "https://westus.api.cognitive.microsoft.com"
        };

        // Change console encoding to display non-ASCII characters
        Console.OutputEncoding = System.Text.Encoding.UTF8;
        SentimentAnalysisExample(client).Wait();
        // DetectLanguageExample(client).Wait();
        // RecognizeEntitiesExample(client).Wait();
        // KeyPhraseExtractionExample(client).Wait();
        Console.ReadLine();
    }
```

Volgende secties wordt beschreven hoe u elk van de API-functies aanroepen.

## <a name="sentiment-analysis"></a>Stemmingsanalyse

1. Maak een nieuwe functie met de naam `SentimentAnalysisExample()` waarmee de client eerder hebt gemaakt.
2. Een lijst genereren `MultiLanguageInput` objecten, met de documenten die u wilt analyseren.

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

3. In de dezelfde functie aanroepen `client.SentimentAsync()` en het resultaat. Vervolgens doorlopen en de resultaten en afdrukken van elk document-ID en gevoelsscore. Een score dichter bij 0 geeft aan dat een negatief gevoel, terwijl een score dichter bij 1 geeft aan een positieve stemming dat.

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

## <a name="language-detection"></a>Taaldetectie

1. Maak een nieuwe functie met de naam `DetectLanguageExample()` waarmee de client eerder hebt gemaakt.
2. Een lijst genereren `LanguageInput` objecten, die uw documenten.

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

3. In de dezelfde functie aanroepen `client.DetectLanguageAsync()` en het resultaat. Vervolgens doorlopen en de resultaten en afdrukken van elk document-ID en de taal van de eerste geretourneerd.

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

## <a name="entity-recognition"></a>Herkenning van entiteit

1. Maak een nieuwe functie met de naam `RecognizeEntitiesExample()` waarmee de client eerder hebt gemaakt.
2. Een lijst genereren `MultiLanguageBatchInput` objecten, die uw documenten.

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

3. In de dezelfde functie aanroepen `client.EntitiesAsync()` en het resultaat. Vervolgens doorlopen en de resultaten en afdrukken van elk document-ID. Voor elke entiteit wordt gedetecteerd, de wikipedia-naam, het type en afdrukken subtypen (indien aanwezig) evenals de locaties in de oorspronkelijke tekst.

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

## <a name="key-phrase-extraction"></a>Sleuteltermextractie

1. Maak een nieuwe functie met de naam `KeyPhraseExtractionExample()` waarmee de client eerder hebt gemaakt.
2. Een lijst genereren `MultiLanguageBatchInput` objecten, die uw documenten.

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

3. In de dezelfde functie aanroepen `client.KeyPhrasesAsync()` en het resultaat. Vervolgens doorlopen en de resultaten en afdrukken van elk document-ID en eventuele gedetecteerde sleuteltermen.

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

