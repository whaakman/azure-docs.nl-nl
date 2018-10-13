---
title: 'Snelstartgids: Controleer de tekstinhoud in C# - Content Moderator'
titlesuffix: Azure Cognitive Services
description: Het controleren van de tekst bevat met de inhoud Moderator SDK voor C#
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: sajagtap
ms.openlocfilehash: ae795ad823c32bc83669d5e98e3fd922500741d4
ms.sourcegitcommit: 3a02e0e8759ab3835d7c58479a05d7907a719d9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2018
ms.locfileid: "49309209"
---
# <a name="quickstart-check-text-content-in-c"></a>Snelstartgids: Controleer de tekstinhoud in C# 

In dit artikel vindt u informatie en codevoorbeelden om aan de slag te gaan met de [Content Moderator SDK voor .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) en het volgende te doen:

- Detecteren van potentieel grof taalgebruik in tekst met filteren op basis van een term
- Gebruik op basis van machine-learning-modellen [classificeren van de tekst](text-moderation-api.md#classification) in drie categorieën.
- Persoonlijk identificeerbare informatie (PII) zoals VS en UK telefoonnummers, e-mailadressen en VS postadressen detecteren.
- Tekst en automatisch corrigeren typfouten normaliseren

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="sign-up-for-content-moderator-services"></a>Registreren voor Content Moderator-services

Voordat u de Content Moderator-services via de REST-API of de SDK gebruiken kunt, moet u de abonnementssleutel van een. Abonneer u op de Content Moderator-service in de [Azure-portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesContentModerator) een ophalen.

## <a name="create-your-visual-studio-project"></a>Het Visual Studio-project maken

1. Voeg een nieuw project van het type **Console app (.NET Framework)** toe aan uw oplossing.

   Noem het project in de voorbeeldcode **TextModeration**.

1. Selecteer dit project als het enige opstartproject voor de oplossing.

### <a name="install-required-packages"></a>De vereiste pakketten installeren

Installeer de volgende NuGet-pakketten:

- Microsoft.Azure.CognitiveServices.ContentModerator
- Microsoft.Rest.ClientRuntime
- Newtonsoft.Json

### <a name="update-the-programs-using-statements"></a>De using-instructies van het programma bijwerken

Voeg de volgende `using` instructies. 

```csharp
using Microsoft.Azure.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator;
using Microsoft.CognitiveServices.ContentModerator.Models;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.IO;
using System.Threading;
```

### <a name="create-the-content-moderator-client"></a>De Content Moderator-client maken

Voeg de volgende code toe om een Content Moderator-client voor uw abonnement te maken.

> [!IMPORTANT]
> Werk de velden **AzureRegion** en **CMSubscriptionKey** bij met de waarden van uw regio-id en de abonnementssleutel.

```csharp
/// <summary>
/// Wraps the creation and configuration of a Content Moderator client.
/// </summary>
/// <remarks>This class library contains insecure code. If you adapt this 
/// code for use in production, use a secure method of storing and using
/// your Content Moderator subscription key.</remarks>
public static class Clients
{
    /// <summary>
    /// The region/location for your Content Moderator account, 
    /// for example, westus.
    /// </summary>
    private static readonly string AzureRegion = "YOUR API REGION";

    /// <summary>
    /// The base URL fragment for Content Moderator calls.
    /// </summary>
    private static readonly string AzureBaseURL =
        $"https://{AzureRegion}.api.cognitive.microsoft.com";

    /// <summary>
    /// Your Content Moderator subscription key.
    /// </summary>
    private static readonly string CMSubscriptionKey = "YOUR API KEY";

    /// <summary>
    /// Returns a new Content Moderator client for your subscription.
    /// </summary>
    /// <returns>The new client.</returns>
    /// <remarks>The <see cref="ContentModeratorClient"/> is disposable.
    /// When you have finished using the client,
    /// you should dispose of it either directly or indirectly. </remarks>
    public static ContentModeratorClient NewClient()
    {
        // Create and initialize an instance of the Content Moderator API wrapper.
        ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

        client.Endpoint = AzureBaseURL;
        return client;
    }
}
```

### <a name="initialize-application-specific-settings"></a>Toepassingsspecifieke instellingen initialiseren

Voeg de volgende statische velden toe aan de klasse **Program** in Program.cs.

```csharp
/// <summary>
/// The name of the file that contains the text to evaluate.
/// </summary>
/// <remarks>You will need to create an input file and update this path
/// accordingly. Relative paths are relative to the execution directory.</remarks>
private static string TextFile = "TextFile.txt";

/// <summary>
/// The name of the file to contain the output from the evaluation.
/// </summary>
/// <remarks>Relative paths are relative to the execution directory.</remarks>
private static string OutputFile = "TextModerationOutput.txt";
```

We de volgende tekst gebruikt als invoer voor deze Quick Start.

> [!NOTE]
> Ongeldige sociaal-fiscaal nummer in het volgende voorbeeldtekst is opzettelijk. Het doel is het overbrengen van de Voorbeeldinvoer en indeling van uitvoer.

```
Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 
0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).
```

## <a name="add-code-to-load-and-evaluate-the-input-text"></a>Code toevoegen om te laden en de invoertekst evalueren

Voeg de volgende code aan de methode **Hoofd**.

```csharp
// Load the input text.
string text = File.ReadAllText(TextFile);
Console.WriteLine("Screening {0}", TextFile);

text = text.Replace(System.Environment.NewLine, " ");

// Save the moderation results to a file.
using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
{
    // Create a Content Moderator client and evaluate the text.
    using (var client = Clients.NewClient())
    {
        // Screen the input text: check for profanity, classify the text into three categories,
        // do autocorrect text, and check for personally identifying
        // information (PII)
        outputWriter.WriteLine("Autocorrect typos, check for matching terms, PII, and classify.");
        var screenResult =
        client.TextModeration.ScreenText("eng", "text/plain", text, true, true, null, true);
        outputWriter.WriteLine(
                JsonConvert.SerializeObject(screenResult, Formatting.Indented));
    }
    outputWriter.Flush();
    outputWriter.Close();
}
```

> [!NOTE]
> De sleutel van uw Content Moderator-service heeft een limiet voor het aantal aanvragen per seconde (RPS). Als u die limiet overschrijdt, genereert de SDK een uitzondering met foutcode 429. Wanneer u een gratis laag-sleutel gebruikt, is het aantal aanvragen is beperkt tot één aanvraag per seconde.

## <a name="run-the-program-and-review-the-output"></a>Het programma uitvoeren en de uitvoer controleren

Het voorbeeld voor het programma-uitvoer geschreven naar het logboekbestand is:

```json
Autocorrect typos, check for matching terms, PII, and classify.
{
"OriginalText": "\"Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).\"",
"NormalizedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"AutoCorrectedText": "\" Is this a garbage or crap email abide@ abed. com, phone: 6657789887, IP: 255. 255. 255. 255, 1 Microsoft Way, Redmond, WA 98052. These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 0800 820 3300. Also, 999-99-9999 looks like a social security number ( SSN) . \"",
"Misrepresentation": null,
    
"Classification": {
        "Category1": {
        "Score": 1.5113095059859916E-06
        },
        "Category2": {
        "Score": 0.12747249007225037
        },
        "Category3": {
        "Score": 0.98799997568130493
        },
        "ReviewRecommended": true
  },
  "Status": {
    "Code": 3000,
    "Description": "OK",
    "Exception": null
  },
  "PII": {
        "Email": [
            {
            "Detected": "abcdef@abcd.com",
            "SubType": "Regular",
            "Text": "abcdef@abcd.com",
            "Index": 33
            }
            ],
        "IPA": [
            {
            "SubType": "IPV4",
            "Text": "255.255.255.255",
            "Index": 73
            }
            ],
        "Phone": [
            {
            "CountryCode": "US",
            "Text": "6657789887",
            "Index": 57
            },
            {
            "CountryCode": "US",
            "Text": "870 608 4000",
            "Index": 211
            },
            {
            "CountryCode": "UK",
            "Text": "+44 870 608 4000",
            "Index": 207
            },
            {
            "CountryCode": "UK",
            "Text": "0344 800 2400",
            "Index": 227
            },
            {
        "CountryCode": "UK",
            "Text": "0800 820 3300",
            "Index": 244
            }
            ],
         "Address": [{
             "Text": "1 Microsoft Way, Redmond, WA 98052",
             "Index": 89
                }]
    },
  "Language": "eng",
  "Terms": [
    {
        "Index": 22,
        "OriginalIndex": 22,
        "ListId": 0,
        "Term": "crap"
    }
  ],
  "TrackingId": "9392c53c-d11a-441d-a874-eb2b93d978d3"
}
```

## <a name="next-steps"></a>Volgende stappen

Download de [Content Moderator .NET SDK](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/) en de [Visual Studio-oplossing](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/ContentModerator) voor deze en andere snelstarts over Content Moderator voor .NET en begin met de integratie.
