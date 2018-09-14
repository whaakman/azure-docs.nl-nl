---
title: Zinslengten ophalen in Translator Text met C# | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: In deze snelstart vindt u de lengte van zinnen in tekst met behulp van de Translator Text-API met C# in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: translator-text
ms.topic: quickstart
ms.date: 06/15/2018
ms.author: nolachar
ms.openlocfilehash: b3e7f1099b1a7584435646fe3fae237cd458967f
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "43768928"
---
# <a name="quickstart-get-sentence-lengths-with-c35"></a>Snelstart: Zinslengten ophalen met C&#35;

In deze snelstart vindt u de lengte van zinnen in tekst met behulp van de Translator Text-API.

## <a name="prerequisites"></a>Vereisten

U hebt [Visual Studio 2017](https://www.visualstudio.com/downloads/) nodig om deze code op Windows uit te voeren. (De gratis Community-editie volstaat.)

Als u de Translator Text-API wilt gebruiken, moet u ook een abonnementssleutel hebben. Lees hoe u zich kunt [registreren voor de Translator Text-API](translator-text-how-to-signup.md).

## <a name="breaksentence-request"></a>BreakSentence-aanvraag

Met de volgende code wordt de brontekst in zinnen verdeeld met behulp van de methode [BreakSentence](./reference/v3-0-break-sentence.md).

1. Maak een nieuw C#-project in uw favoriete IDE.
2. Voeg de onderstaande code toe.
3. Vervang de waarde `key` door een geldige toegangssleutel voor uw abonnement.
4. Voer het programma uit.

```csharp
using System;
using System.Net.Http;
using System.Text;
// NOTE: Install the Newtonsoft.Json NuGet package.
using Newtonsoft.Json;

namespace TranslatorTextQuickStart
{
    class Program
    {
        static string host = "https://api.cognitive.microsofttranslator.com";
        static string path = "/breaksentence?api-version=3.0";

        static string uri = host + path;

        // NOTE: Replace this example key with a valid subscription key.
        static string key = "ENTER KEY HERE";

        static string text = "How are you? I am fine. What did you do today?";

        async static void Break()
        {
            System.Object[] body = new System.Object[] { new { Text = text } };
            var requestBody = JsonConvert.SerializeObject(body);

            using (var client = new HttpClient())
            using (var request = new HttpRequestMessage())
            {
                request.Method = HttpMethod.Post;
                request.RequestUri = new Uri(uri);
                request.Content = new StringContent(requestBody, Encoding.UTF8, "application/json");
                request.Headers.Add("Ocp-Apim-Subscription-Key", key);

                var response = await client.SendAsync(request);
                var responseBody = await response.Content.ReadAsStringAsync();
                var result = JsonConvert.SerializeObject(JsonConvert.DeserializeObject(responseBody), Formatting.Indented);

                Console.OutputEncoding = UnicodeEncoding.UTF8;
                Console.WriteLine(result);
            }
        }

        static void Main(string[] args)
        {
            Break();
            Console.ReadLine();
        }
    }
}
```

## <a name="breaksentence-response"></a>BreakSentence-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, zoals u in het volgende voorbeeld kunt zien:

```json
[
  {
    "detectedLanguage": {
      "language": "en",
      "score": 1.0
    },
    "sentLen": [
      13,
      11,
      22
    ]
  }
]
```

## <a name="next-steps"></a>Volgende stappen

Bekijk de voorbeeldcode voor deze snelstart en andere, zoals vertaling en transliteratie, evenals andere Translator Text-voorbeeldprojecten in GitHub.

> [!div class="nextstepaction"]
> [C#-voorbeelden op GitHub bekijken](https://aka.ms/TranslatorGitHub?type=&language=c%23)
