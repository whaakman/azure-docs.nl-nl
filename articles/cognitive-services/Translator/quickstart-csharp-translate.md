---
title: 'Snelstart: Tekst vertalen, C# - Translator Text'
titleSuffix: Azure Cognitive Services
description: In deze snelstart vertaalt u tekst vanuit één taal naar een andere taal met de Translator Text-API met C#.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 11/20/2018
ms.author: erhopf
ms.openlocfilehash: 20e07120b80f335397bd7e907f62e14ad8b1d2ee
ms.sourcegitcommit: cd0a1514bb5300d69c626ef9984049e9d62c7237
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52679049"
---
# <a name="quickstart-translate-text-with-the-translator-text-rest-api-c"></a>Snelstart: Tekst vertalen met de Translator Text REST API (C#)

In deze quickstart leert u hoe u een tekenreeks van het Engels naar het Italiaans en Duits vertaalt met .NET Core en de Translator Text REST API.

Voor deze snelstart is een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met een Translator Text-resource vereist. Als u geen account hebt, kunt u de [gratis proefversie](https://azure.microsoft.com/try/cognitive-services/) gebruiken om een abonnementssleutel op te halen.

## <a name="prerequisites"></a>Vereisten

* [.NET SDK](https://www.microsoft.com/net/learn/dotnet/hello-world-tutorial)
* [Json.NET NuGet-pakket](https://www.nuget.org/packages/Newtonsoft.Json/)
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) of uw favoriete teksteditor
* Een Azure-abonnementssleutel voor Translator Text

## <a name="create-a-net-core-project"></a>Een .NET Core-project maken

Open een nieuwe opdrachtprompt (of een terminalsessie) en voer deze opdrachten uit:

```console
dotnet new console -o translate-sample
cd translate-sample
```

De eerste opdracht doet twee dingen. Hiermee wordt een nieuwe .NET-consoletoepassing gemaakt en een map met de naam `translate-sample`. Met de tweede opdracht gaat u naar de map voor het project.

Daarna moet u Json.Net installeren. Voer vanuit de map van het project het volgende uit:

```console
dotnet add package Newtonsoft.Json --version 11.0.2
```

## <a name="add-required-namespaces-to-your-project"></a>Vereiste naamruimten toevoegen aan het project

Met de opdracht `dotnet new console` die u eerder hebt uitgevoerd, is een project gemaakt, met daarin `Program.cs`. In dit bestand plaatst u de toepassingscode. Open `Program.cs` en vervang de bestaande using-instructies. Deze instructies zorgen ervoor dat u toegang hebt tot alle typen die zijn vereist voor het bouwen en uitvoeren van de voorbeeld-app.

```csharp
using System;
using System.Net.Http;
using System.Text;
using Newtonsoft.Json;
```

## <a name="create-a-function-to-translate-text"></a>Een functie maken om tekst te vertalen

Maak in de klasse `Program` een functie met de naam `TranslateText`. Deze klasse bevat de code die wordt gebruikt om de resource Vertalen aan te roepen en het resultaat weer te geven op de console.

```csharp
static void TranslateText()
{
  /*
   * The code for your call to the translation service will be added to this
   * function in the next few sections.
   */
}
```

## <a name="set-the-subscription-key-host-name-and-path"></a>Abonnementssleutel, hostnaam en pad instellen

Voeg deze regels toe aan de functie `TranslateText`. U ziet dat er naast de `api-version` twee extra parameters zijn toegevoegd aan de `route`. Deze parameters worden gebruikt om de uitvoer van de vertaling in te stellen. In dit voorbeeld is dit ingesteld op Duits (`de`) en Italiaans (`it`). Zorg dat u de waarde van de abonnementssleutel bijwerkt.

```csharp
string host = "https://api.cognitive.microsofttranslator.com";
string route = "/translate?api-version=3.0&to=de&to=it";
string subscriptionKey = "YOUR_SUBSCRIPTION_KEY";
```

Vervolgens moet u het JSON-object maken en serialiseren dat de tekst bevat die u wilt vertalen. Onthoud dat u meerdere objecten kunt doorgeven in de matrix `body`.

```csharp
System.Object[] body = new System.Object[] { new { Text = @"Hello world!" } };
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
// Set the method to POST
request.Method = HttpMethod.Post;

// Construct the full URI
request.RequestUri = new Uri(host + route);

// Add the serialized JSON object to your request
request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");

// Add the authorization header
request.Headers.Add("Ocp-Apim-Subscription-Key", subscriptionKey);

// Send request, get response
var response = client.SendAsync(request).Result;
var jsonResponse = response.Content.ReadAsStringAsync().Result;

// Print the response
Console.WriteLine(jsonResponse);
Console.WriteLine("Press any key to continue.");
```

## <a name="put-it-all-together"></a>Alles samenvoegen

In de laatste stap roept u `TranslateText()` aan in de functie `Main`. Zoek `static void Main(string[] args)` en voeg deze regels toe:

```csharp
TranslateText();
Console.ReadLine();
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Nu kunt u de voorbeeld-app gaan uitvoeren. Ga vanaf de opdrachtregel (of terminalsessie) naar de projectmap en voer het volgende uit:

```console
dotnet run
```

## <a name="sample-response"></a>Voorbeeldantwoord

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "translations": [
      {
        "text": "Hallo Welt!",
        "to": "de"
      },
      {
        "text": "Salve, mondo!",
        "to": "it"
      }
    ]
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

* [Tekst transcriberen](quickstart-csharp-transliterate.md)
* [Een taal identificeren op basis van de invoer](quickstart-csharp-detect.md)
* [Alternatieve vertalingen verkrijgen](quickstart-csharp-dictionary.md)
* [Een lijst ophalen van ondersteunde talen](quickstart-csharp-languages.md)
* [De zinlengte in invoer bepalen](quickstart-csharp-sentences.md)
