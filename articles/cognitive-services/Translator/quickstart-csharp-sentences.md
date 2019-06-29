---
title: 'Quickstart: Zinslengtes ophalen, C# - Translator Text-API'
titleSuffix: Azure Cognitive Services
description: In deze quickstart leert u hoe u zinlengtes bepaalt met behulp van .NET Core en de Translator Text-API.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: quickstart
ms.date: 06/13/2019
ms.author: swmachan
ms.openlocfilehash: 27f515c775d4b1a77563ac10338fc2255e42731c
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448171"
---
# <a name="quickstart-use-the-translator-text-api-to-determine-sentence-length-using-c"></a>Quickstart: De Translator Text-API gebruiken om de zinslengte te bepalen met C#

In deze snelstartgids leert u hoe u om te bepalen zin lengtes met behulp van .NET Core, C# 7.1 of hoger, en de Translator Text-API.

Voor deze snelstart is een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met een Translator Text-resource vereist. Als u geen account hebt, kunt u de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/) gebruiken om een abonnementssleutel op te halen.

## <a name="prerequisites"></a>Vereisten

* C#7.1 of hoger
* [.NET SDK](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Json.NET NuGet-pakket](https://www.nuget.org/packages/Newtonsoft.Json/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) of uw favoriete teksteditor
* Een Azure-abonnementssleutel voor Translator Text

## <a name="create-a-net-core-project"></a>Een .NET Core-project maken

Open een nieuwe opdrachtprompt (of een terminalsessie) en voer deze opdrachten uit:

```console
dotnet new console -o sentences-sample
cd sentences-sample
```

De eerste opdracht doet twee dingen. Hiermee wordt een nieuwe .NET-consoletoepassing gemaakt en een map met de naam `sentences-sample`. Met de tweede opdracht gaat u naar de map voor het project.

Daarna moet u Json.Net installeren. Voer vanuit de map van het project het volgende uit:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="select-the-c-language-version"></a>Selecteer de C# taalversie

Deze snelstartgids moet C# 7.1 of hoger. Er zijn een aantal manieren om te wijzigen de C# versie voor uw project. In deze handleiding leert u hoe u aan te passen de `sentences-sample.csproj` bestand. Voor alle beschikbare opties, zoals het wijzigen van de taal van Visual Studio, Zie [selecteert u de C# taalversie](https://docs.microsoft.com/dotnet/csharp/language-reference/configure-language-version).

Open het project en open vervolgens `sentences-sample.csproj`. Zorg ervoor dat `LangVersion` is ingesteld op 7.1 of hoger. Als er geen een groep met eigenschappen voor de taalversie, voegt u deze regels toe:

```xml
<PropertyGroup>
   <LangVersion>7.1</LangVersion>
</PropertyGroup>
```

## <a name="add-required-namespaces-to-your-project"></a>Vereiste naamruimten toevoegen aan het project

Met de opdracht `dotnet new console` die u eerder hebt uitgevoerd, is een project gemaakt, met daarin `Program.cs`. In dit bestand plaatst u de toepassingscode. Open `Program.cs` en vervang de bestaande using-instructies. Deze instructies zorgen ervoor dat u toegang hebt tot alle typen die zijn vereist voor het bouwen en uitvoeren van de voorbeeld-app.

```csharp
using System;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
// Install Newtonsoft.Json with NuGet
using Newtonsoft.Json;
```

## <a name="create-classes-for-the-json-response"></a>Maken van klassen voor de JSON-antwoord

Vervolgens gaan we een klasse die wordt gebruikt tijdens het deserialiseren van het JSON-antwoord geretourneerd door de Translator Text-API te maken.

```csharp
/// <summary>
/// The C# classes that represents the JSON returned by the Translator Text API.
/// </summary>
public class BreakSentenceResult
{
    public int[] SentLen { get; set; }
    public DetectedLanguage DetectedLanguage { get; set; }
}

public class DetectedLanguage
{
    public string Language { get; set; }
    public float Score { get; set; }
}
```

## <a name="create-a-function-to-determine-sentence-length"></a>Een functie maken om de zinlengte te bepalen

Maak in de klasse `Program` een functie met de naam `BreakSentence()`. Deze functie omvat vier argumenten: `subscriptionKey`, `host`, `route`, en `inputText`.

```csharp
static public async Task BreakSentenceRequest(string subscriptionKey, string host, string route, string inputText)
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="serialize-the-break-sentence-request"></a>De aanvraag van de zin break serialiseren

Vervolgens moet u het JSON-object maken en serialiseren dat de tekst bevat. Onthoud dat u meerdere objecten kunt doorgeven in de matrix `body`.

```csharp
object[] body = new object[] { new { Text = inputText } };
var requestBody = JsonConvert.SerializeObject(body);
```

## <a name="instantiate-the-client-and-make-a-request"></a>De client instantiëren en een aanvraag indienen

Met deze regels instantieert u een exemplaar van `HttpClient` en `HttpRequestMessage`:

```csharp
using (var client = new HttpClient())
using (var request = new HttpRequestMessage())
{
  // In the next few sections you'll add code to construct the request.
}
```

## <a name="construct-the-request-and-print-the-response"></a>De aanvraag maken en het antwoord afdrukken

In `HttpRequestMessage` doet u het volgende:

* De HTTP-methode declareren
* De aanvraag-URI samenstellen
* De aanvraagbody (geserialiseerd JSON-object) invoegen
* Vereiste headers toevoegen
* Een asynchrone aanvraag maken
* Het antwoord weergeven

Voeg deze code toe aan `HttpRequestMessage`:

```csharp
// Build the request.
// Set the method to POST
request.Method = HttpMethod.Post;
// Construct the URI and add headers.
request.RequestUri = new Uri(host + route);
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send the request and get response.
HttpResponseMessage response = await client.SendAsync(request).ConfigureAwait(false);
// Read response as a string.
string result = await response.Content.ReadAsStringAsync();
// Deserialize the response using the classes created earlier.
BreakSentenceResult[] deserializedOutput = JsonConvert.DeserializeObject<BreakSentenceResult[]>(result);
foreach (BreakSentenceResult o in deserializedOutput)
{
    Console.WriteLine("The detected language is '{0}'. Confidence is: {1}.", o.DetectedLanguage.Language, o.DetectedLanguage.Score);
    Console.WriteLine("The first sentence length is: {0}", o.SentLen[0]);
}
```

Als u een abonnement van Cognitive Services-meerdere services gebruikt, moet u ook de `Ocp-Apim-Subscription-Region` in de parameters van de aanvraag. [Meer informatie over verificatie met het abonnement op meerdere services](https://docs.microsoft.com/azure/cognitive-services/translator/reference/v3-0-reference#authentication). 

## <a name="put-it-all-together"></a>Alles samenvoegen

In de laatste stap roept u `BreakSentenceRequest()` aan in de functie `Main`. Zoek `static void Main(string[] args)` en vervang deze door deze code:

```csharp
static async Task Main(string[] args)
{
    // This is our main function.
    // Output languages are defined in the route.
    // For a complete list of options, see API reference.
    string subscriptionKey = "YOUR_TRANSLATOR_TEXT_KEY_GOES_HERE";
    string host = "https://api.cognitive.microsofttranslator.com";
    string route = "/breaksentence?api-version=3.0";
    // Feel free to use any string.
    string breakSentenceText = @"How are you doing today? The weather is pretty pleasant. Have you been to the movies lately?";
    await BreakSentenceRequest(subscriptionKey, host, route, breakSentenceText);
}
```

U kunt zien dat in `Main`, u bent declareren `subscriptionKey`, `host`, `route`, en de tekst om te evalueren `breakSentenceText`.

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Nu kunt u de voorbeeld-app gaan uitvoeren. Ga vanaf de opdrachtregel (of terminalsessie) naar de projectmap en voer het volgende uit:

```console
dotnet run
```

## <a name="sample-response"></a>Voorbeeldantwoord

Nadat u het voorbeeld uitvoert, ziet u het volgende afgedrukt op de terminal:

```bash
The detected language is \'en\'. Confidence is: 1.
The first sentence length is: 25
```

Dit bericht wordt samengesteld uit de onbewerkte JSON, die er als volgt:

```json
[
    {
        "detectedLanguage":
        {
            "language":"en",
            "score":1.0
        },
        "sentLen":[25,32,35]
    }
]
```

## <a name="clean-up-resources"></a>Resources opschonen

Denk eraan dat u eventuele vertrouwelijke informatie, zoals abonnementssleutels, verwijdert uit de broncode van de voorbeeld-app.

## <a name="next-steps"></a>Volgende stappen

Bekijk de voorbeeldcode voor deze snelstartgids en andere resources, met inbegrip van transcriptie en taalidentificatie, evenals andere Translator Text-voorbeeldprojecten op GitHub.

> [!div class="nextstepaction"]
> [C#-voorbeelden in GitHub bekijken](https://aka.ms/TranslatorGitHub?type=&language=c%23)

## <a name="see-also"></a>Zie ook

* [Tekst vertalen](quickstart-csharp-translate.md)
* [Tekst transcriberen](quickstart-csharp-transliterate.md)
* [Een taal identificeren op basis van de invoer](quickstart-csharp-detect.md)
* [Alternatieve vertalingen verkrijgen](quickstart-csharp-dictionary.md)
* [Een lijst ophalen van ondersteunde talen](quickstart-csharp-languages.md)
* [De zinlengte in invoer bepalen](quickstart-csharp-sentences.md)
