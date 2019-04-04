---
title: 'Gebruik een voorspellingseindpunt om afbeeldingen programmatisch te testen met classificatie: Custom Vision'
titlesuffix: Azure Cognitive Services
description: Lees hoe u de API gebruikt om afbeeldingen programmatisch te testen met uw Custom Vision Service-classificatie.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: article
ms.date: 04/02/2019
ms.author: anroth
ms.openlocfilehash: 78ca1d7ceb9086e0d589f904b24b967d36b079a0
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58895610"
---
# <a name="use-your-model-with-the-prediction-api"></a>Het Model met de Voorspellings-API gebruiken

Nadat u hebt uw model te trainen, kunt u installatiekopieën programmatisch testen door indienen bij de voorspelling API-eindpunt.

> [!NOTE]
> In dit document ziet u hoe u een afbeelding bij de voorspellings-API kunt indienen met behulp van C#. Zie voor meer informatie en voorbeelden, de [voorspelling API-verwijzing](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15).

## <a name="publish-your-trained-iteration"></a>Uw getrainde iteratie publiceren

Op de [Custom Vision-webpagina](https://customvision.ai) selecteert u uw project en vervolgens selecteert u het tabblad __Prestaties__.

Om in te dienen afbeeldingen voor de voorspelling API, moet u eerst uw iteratie voor voorspelling, die kan worden uitgevoerd door het selecteren van publiceren __publiceren__ en een naam op voor de gepubliceerde iteratie op te geven. Dit maakt het model toegankelijk is voor de voorspelling API van uw aangepaste Vision Azure-resource.

![Het tabblad prestaties wordt weergegeven, met een rode rechthoek rond de knop publiceren.](./media/use-prediction-api/unpublished-iteration.png)

Zodra het model is gepubliceerd, ziet u een 'Gepubliceerd' label weergegeven naast uw iteratie in de zijbalk links en de naam wordt weergegeven in de beschrijving van de iteratie.

![Het tabblad prestaties wordt weergegeven, met een rode rechthoek rond het label is gepubliceerd en de naam van de gepubliceerde iteratie.](./media/use-prediction-api/published-iteration.png)

## <a name="get-the-url-and-prediction-key"></a>De URL en voorspellingssleutel ophalen

Zodra het model is gepubliceerd, kunt u de vereiste gegevens ophalen door het selecteren van __voorspelling URL__. Hiermee opent u een dialoogvenster met informatie over het gebruik van de Voorspellings-API, met inbegrip van de __voorspelling URL__ en __voorspelling-sleutel__.

![Het tabblad prestaties wordt weergegeven met een rode rechthoek rond de voorspelling van URL-knop.](./media/use-prediction-api/published-iteration-prediction-url.png)

![Het tabblad prestaties wordt weergegeven met een rode rechthoek rond de voorspelling van URL-waarde voor het gebruik van een afbeeldingsbestand en de voorspelling-sleutel-waarde.](./media/use-prediction-api/prediction-api-info.png)

> [!TIP]
> Uw __voorspelling-sleutel__ ook te vinden de [Azure Portal](https://portal.azure.com) pagina voor de aangepaste Vision Azure-Resource die is gekoppeld aan uw project, onder de __sleutels__ blade.

In deze handleiding vindt u een lokale installatiekopie gebruiken, dus kopieer de URL onder **hebt u een afbeeldingsbestand** naar een tijdelijke locatie. Kopieer de bijbehorende __voorspelling-sleutel__ waarde ook.

## <a name="create-the-application"></a>De toepassing maken

1. In Visual Studio, maak een nieuwe C# consoletoepassing.

1. Gebruik de volgende code als de hoofdtekst van het bestand __Program.cs__.

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace CVSPredictionSample
    {
        public static class Program
        {
            public static void Main()
            {
                Console.Write("Enter image file path: ");
                string imageFilePath = Console.ReadLine();

                MakePredictionRequest(imageFilePath).Wait();

                Console.WriteLine("\n\nHit ENTER to exit...");
                Console.ReadLine();
            }

            public static async Task MakePredictionRequest(string imageFilePath)
            {
                var client = new HttpClient();

                // Request headers - replace this example key with your valid Prediction-Key.
                client.DefaultRequestHeaders.Add("Prediction-Key", "<Your prediction key>");

                // Prediction URL - replace this example URL with your valid Prediction URL.
                string url = "<Your prediction URL>";

                HttpResponseMessage response;

                // Request body. Try this sample with a locally stored image.
                byte[] byteData = GetImageAsByteArray(imageFilePath);

                using (var content = new ByteArrayContent(byteData))
                {
                    content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                    response = await client.PostAsync(url, content);
                    Console.WriteLine(await response.Content.ReadAsStringAsync());
                }
            }

            private static byte[] GetImageAsByteArray(string imageFilePath)
            {
                FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }
        }
    }
    ```

1. Voer de volgende informatie in:
   * Stel de `namespace` veld de naam van uw project.
   * Vervang de tijdelijke aanduiding `<Your prediction key>` met de waarde van de sleutel die u eerder hebt opgehaald.
   * Vervang de tijdelijke aanduiding `<Your prediction URL>` met de URL die u eerder hebt opgehaald.

## <a name="run-the-application"></a>De toepassing uitvoeren

Wanneer u de toepassing uitvoert, wordt u gevraagd om in te voeren van een pad naar een afbeelding in de console. De afbeelding wordt vervolgens verzonden naar de Voorspellings-API en de voorspellingsresultaten worden geretourneerd als een tekenreeks in JSON-indeling. Hier volgt een voorbeeld van de reactie.

```json
{
    "Id":"7796df8e-acbc-45fc-90b4-1b0c81b73639",
    "Project":"8622c779-471c-4b6e-842c-67a11deffd7b",
    "Iteration":"59ec199d-f3fb-443a-b708-4bca79e1b7f7",
    "Created":"2019-03-20T16:47:31.322Z",
    "Predictions":[
        {"TagId":"d9cb3fa5-1ff3-4e98-8d47-2ef42d7fb373","TagName":"cat", "Probability":1.0},
        {"TagId":"9a8d63fb-b6ed-4462-bcff-77ff72084d99","TagName":"dog", "Probability":0.1087869}
    ]
}
```

## <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u geleerd hoe u om in te dienen afbeeldingen aan uw aangepaste installatiekopie classificatie/detector en een antwoord via een programma met de C# SDK. Hierna leert hoe u end-to-end scenario's met C#, of aan de slag met een andere taal SDK.

* [Quickstart: .NET SDK](csharp-tutorial.md)
* [Quickstart: Python-SDK](python-tutorial.md)
* [Quickstart: Java-SDK](java-tutorial.md)
* [Quickstart: Node SDK](node-tutorial.md)
* [Quickstart: Go-SDK](go-tutorial.md)
