---
title: 'Quickstart: Spellingcontrole met de Bing Spellingcontrole-SDK voor C#'
titlesuffix: Azure Cognitive Services
description: Aan de slag met de Bing Spellingcontrole-REST-API om de spelling en grammatica te controleren.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: quickstart
ms.date: 02/20/2019
ms.author: v-gedod
ms.openlocfilehash: 3487656f72f315aa15ce003a8dfd279c45d1f992
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58105740"
---
# <a name="quickstart-check-spelling-with-the-bing-spell-check-sdk-for-c"></a>Quickstart: Spellingcontrole met de Bing Spellingcontrole-SDK voor C#

Gebruik deze quickstart om te beginnen met de Bing Spellingcontrole-SDK voor C#. Hoewel de Bing Spellingcontrole een REST-API heeft die compatibel is met de meeste programmeertalen, biedt de SDK een eenvoudige manier om de service in uw toepassingen te integreren. De broncode voor dit voorbeeld is te vinden op [GitHub](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/samples/SpellCheck).

## <a name="application-dependencies"></a>Afhankelijkheden van de toepassing

* Elke versie van [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/).
* [NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Language.SpellCheck) voor de Bing Spellingcontrole

Als u de Bing Spellingcontrole-SDK aan uw project wilt toevoegen, klikt u op `Manage NuGet Packages` vanuit Solution Explorer in Visual Studio. Voeg het pakket `Microsoft.Azure.CognitiveServices.Language.SpellCheck` toe. Dit pakket installeert ook de volgende afhankelijkheden:

* Microsoft.Rest.ClientRuntime
* Microsoft.Rest.ClientRuntime.Azure
* Newtonsoft.Json

[!INCLUDE [cognitive-services-bing-spell-check-signup-requirements](../../../includes/cognitive-services-bing-spell-check-signup-requirements.md)]

## <a name="create-and-initialize-the-application"></a>De toepassing maken en initialiseren

1. Maak een nieuwe C#-console-oplossing in Visual Studio. Voeg daarna de volgende `using`-instructie toe.
    
    ```csharp
    using System;
    using System.Linq;
    using System.Threading.Tasks;
    using Microsoft.Azure.CognitiveServices.Language.SpellCheck;
    using Microsoft.Azure.CognitiveServices.Language.SpellCheck.Models;
    ```

2. Maak een nieuwe klasse. Maak vervolgens een asynchrone functie met de naam `SpellCheckCorrection()` die een abonnementssleutel vereist en de aanvraag van de spellingcontrole verstuurt.

3. Maak een instantie van de client door een nieuw `ApiKeyServiceClientCredentials`-object te maken. 

    ```csharp
    public static class SpellCheckSample{
        public static async Task SpellCheckCorrection(string key){
            var client = new SpellCheckClient(new ApiKeyServiceClientCredentials(key));
        }
        //...
    }
    ```

## <a name="send-the-request-and-read-the-response"></a>De aanvraag verzenden en het antwoord lezen

1. Voer in de hierboven gemaakte functie de volgende stappen uit. Verstuur de aanvraag van de spellingcontrole met de client. Voeg de te controleren tekst toe aan de `text`-parameter en stel de modus in op `proof`.  
    
    ```csharp
    var result = await client.SpellCheckerWithHttpMessagesAsync(text: "Bill Gatas", mode: "proof");
    ```

2. Haal het eerste resultaat van de spellingcontrole op, als dat beschikbaar is. Druk het eerste verkeerd gespelde woord (token) af, het type token en het aantal suggesties.

    ```csharp
    if (firstspellCheckResult != null){
        var firstspellCheckResult = result.Body.FlaggedTokens.FirstOrDefault();
    
        Console.WriteLine("SpellCheck Results#{0}", result.Body.FlaggedTokens.Count);
        Console.WriteLine("First SpellCheck Result token: {0} ", firstspellCheckResult.Token);
        Console.WriteLine("First SpellCheck Result Type: {0} ", firstspellCheckResult.Type);
        Console.WriteLine("First SpellCheck Result Suggestion Count: {0} ", firstspellCheckResult.Suggestions.Count);
    }
    ```

3. Haal de eerste voorgestelde correctie op, als deze beschikbaar is. Druk de voorgestelde score en het voorgestelde woord af. 

    ```csharp
            var suggestions = firstspellCheckResult.Suggestions;

            if (suggestions?.Count > 0)
            {
                var firstSuggestion = suggestions.FirstOrDefault();
                Console.WriteLine("First SpellCheck Suggestion Score: {0} ", firstSuggestion.Score);
                Console.WriteLine("First SpellCheck Suggestion : {0} ", firstSuggestion.Suggestion);
            }
   }

## Next steps

> [!div class="nextstepaction"]
> [Create a single page web-app](tutorials/spellcheck.md)

- [What is the Bing Spell Check API?](overview.md)
- [Bing Spell Check C# SDK reference guide](https://docs.microsoft.com/dotnet/api/overview/azure/cognitiveservices/client/bingspellcheck?view=azure-dotnet)