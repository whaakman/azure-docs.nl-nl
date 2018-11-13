---
title: 'Snelstart: tekstinhoud op ongewenst materiaal analyseren in C#'
titlesuffix: Azure Cognitive Services
description: Tekstinhoud analyseren op diverse soorten ongewenst materiaal met behulp van de Content Moderator SDK voor .NET
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: quickstart
ms.date: 10/31/2018
ms.author: sajagtap
ms.openlocfilehash: 0540a81db93570928dd33b66a69b6883b2df0cd9
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51007685"
---
# <a name="quickstart-analyze-text-content-for-objectionable-material-in-c"></a>Snelstart: tekstinhoud op ongewenst materiaal analyseren in C# 

In dit artikel vindt u informatie en codevoorbeelden om aan de slag te gaan met de [Content Moderator SDK voor .NET](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.ContentModerator/). U leert hoe u kunt filteren op termen en tekstinhoud classificeren met het doel toezicht te houden op mogelijk ongewenst materiaal.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="prerequisites"></a>Vereisten
- Een abonnementssleutel voor Content Moderator. Volg de instructies in [een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) om u te abonneren op Content Moderator en uw sleutel op te halen.
- Een versie van [Visual Studio 2015 of 2017](https://www.visualstudio.com/downloads/)

> [!NOTE]
> In deze handleiding wordt een gratis Content Moderator-abonnement gebruikt. Raadpleeg de pagina [Prijzen en beperkingen](https://azure.microsoft.com/pricing/details/cognitive-services/content-moderator/) voor informatie over wat bij elke abonnementslaag wordt geleverd.

## <a name="create-the-visual-studio-project"></a>Het Visual Studio-project maken

1. Maak in Visual Studio een nieuw **Console-app (.NET Framework)**-project en noem het **TextModeration**. 
1. Als uw oplossing nog meer projecten bevat, selecteert u deze als het enkele opstartproject.
1. Download de vereiste NuGet-pakketten. Klik met de rechtermuisknop op uw project in Solution Explorer en selecteer **NuGet-pakketten beheren**. Zoek en installeer vervolgens de volgende pakketten:
    - Microsoft.Azure.CognitiveServices.ContentModerator
    - Microsoft.Rest.ClientRuntime
    - Newtonsoft.Json

## <a name="add-text-moderation-code"></a>Teksttoezichtcode toevoegen

Vervolgens kopieert en plakt u de code uit deze handleiding in uw project om een eenvoudig scenario voor inhoudstoezicht te implementeren.

### <a name="include-namespaces"></a>Naamruimten opnemen

Voeg aan het begin van het bestand *Program.cs* de volgende `using`-instructies toe.

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

Voeg de volgende code toe aan het bestand *Program.cs* om een Content Moderator-clientprovider voor uw abonnement te maken. Voeg de code toe naast de klasse **Program**, in dezelfde naamruimte. U moet de velden **AzureRegion** en **CMSubscriptionKey** bijwerken met de waarden van uw regio-id en de abonnementssleutel.

```csharp
// Wraps the creation and configuration of a Content Moderator client.
public static class Clients
{
    // The region/location for your Content Moderator account, 
    // for example, westus.
    private static readonly string AzureRegion = "YOUR API REGION";

    // The base URL fragment for Content Moderator calls.
    private static readonly string AzureBaseURL =
        $"https://{AzureRegion}.api.cognitive.microsoft.com";

    // Your Content Moderator subscription key.
    private static readonly string CMSubscriptionKey = "YOUR API KEY";

    // Returns a new Content Moderator client for your subscription.
    public static ContentModeratorClient NewClient()
    {
        // Create and initialize an instance of the Content Moderator API wrapper.
        ContentModeratorClient client = new ContentModeratorClient(new ApiKeyServiceClientCredentials(CMSubscriptionKey));

        client.Endpoint = AzureBaseURL;
        return client;
    }
}
```

### <a name="set-up-input-and-output-targets"></a>Invoer- en uitvoerdoelen instellen

Voeg de volgende statische velden toe aan de klasse **Program** in _Program.cs_. Hiermee worden de bestanden voor ingevoerde tekstinhoud en uitgevoerde JSON-inhoud opgegeven.

```csharp
// The name of the file that contains the text to evaluate.
private static string TextFile = "TextFile.txt";

// The name of the file to contain the output from the evaluation.
private static string OutputFile = "TextModerationOutput.txt";
```

U moet het invoerbestand *TextFile.txt* maken en het pad dienovereenkomstig bijwerken (relatieve paden zijn relatief ten opzichte van de uitvoeringsdirectory). Open _TextFile.txt_ en voeg de tekst toe waarop u toezicht wilt houden. In deze snelstart wordt de volgende voorbeeldtekst gebruikt:

```
Is this a grabage or crap email abcdef@abcd.com, phone: 6657789887, IP: 255.255.255.255, 1 Microsoft Way, Redmond, WA 98052.
These are all UK phone numbers, the last two being Microsoft UK support numbers: +44 870 608 4000 or 0344 800 2400 or 
0800 820 3300. Also, 999-99-9999 looks like a social security number (SSN).
```

### <a name="load-the-input-text"></a>De invoertekst laden

Voeg de volgende code aan de methode **Hoofd**. De **ScreenText**-methode is de essentiële bewerking. De parameters ervan bepalen welke bewerkingen voor inhoudtoezicht worden uitgevoerd. In dit voorbeeld wordt de methode geconfigureerd om het volgende te doen:
- Mogelijk grof taalgebruik in de tekst te detecteren.
- De tekst te normaliseren en typfouten automatisch te corrigeren.
- Persoonlijk identificeerbare informatie (PII) zoals telefoonnummers in de VS en het Verenigd Koninkrijk, e-mailadressen en postadressen in de VS detecteren.
- Machine-learningmodellen gebruiken om de tekst onder te verdelen in drie categorieën.

Als u meer wilt weten over wat deze bewerkingen doen, volgt u de koppeling in de sectie [Volgende stappen](#next-steps).

```csharp
// Load the input text.
string text = File.ReadAllText(TextFile);
Console.WriteLine("Screening {0}", TextFile);

text = text.Replace(System.Environment.NewLine, " ");
byte[] byteArray = System.Text.Encoding.UTF8.GetBytes(text);
MemoryStream stream = new MemoryStream(byteArray);

// Save the moderation results to a file.
using (StreamWriter outputWriter = new StreamWriter(OutputFile, false))
{
    // Create a Content Moderator client and evaluate the text.
    using (var client = Clients.NewClient())
    {
        // Screen the input text: check for profanity,
        // autocorrect text, check for personally identifying
        // information (PII), and classify the text into three categories
        outputWriter.WriteLine("Autocorrect typos, check for matching terms, PII, and classify.");
        var screenResult =
        client.TextModeration.ScreenText("text/plain", stream, "eng", true, true, null, true);
        outputWriter.WriteLine(
                JsonConvert.SerializeObject(screenResult, Formatting.Indented));
    }
    outputWriter.Flush();
    outputWriter.Close();
}
```

## <a name="run-the-program"></a>Het programma uitvoeren

Het programma schrijft JSON-tekenreeksgegevens naar het bestand _TextModerationOutput.txt_. De voorbeeldtekst die wordt gebruikt in deze snelstart geeft de volgende uitvoer:

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

In deze snelstart hebt u een eenvoudige .NET- toepassing ontwikkeld die de Content Moderator-service gebruikt om relevante informatie over een bepaald tekstvoorbeeld te retourneren. Nu leert u meer over wat de verschillende vlaggen en classificaties betekenen, zodat u kunt beslissen welke gegevens u nodig hebt en hoe uw app ermee om moet gaan.

> [!div class="nextstepaction"]
> [Teksttoezichthandleiding](text-moderation-api.md)
