---
title: 'Voorbeeld: Gebruik een voorspellingseindpunt om afbeeldingen programmatisch te testen met classificatie: Custom Vision'
titlesuffix: Azure Cognitive Services
description: Lees hoe u de API gebruikt om afbeeldingen programmatisch te testen met uw Custom Vision Service-classificatie.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: article
ms.date: 03/26/2019
ms.author: anroth
ms.openlocfilehash: 715fa526c83608c9922315e3a0d89b67b31e0d16
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58472719"
---
#  <a name="use-your-model-with-the-prediction-api"></a>Het Model met de Voorspellings-API gebruiken

Nadat u uw model hebt getraind, kunt u afbeeldingen programmatisch testen door ze in te dienen bij de voorspellings-API.

> [!NOTE]
> In dit document ziet u hoe u een afbeelding bij de voorspellings-API kunt indienen met behulp van C#. Zie het document [Referentie voorspellings-API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15) voor meer informatie en voorbeelden van het gebruik van de API.

## <a name="publish-your-trained-iteration"></a>Uw getrainde iteratie publiceren

Op de [Custom Vision-webpagina](https://customvision.ai) selecteert u uw project en vervolgens selecteert u het tabblad __Prestaties__.

Om in te dienen afbeeldingen voor de voorspelling API, moet u eerst uw iteratie voor voorspelling, die kan worden uitgevoerd door het selecteren van publiceren __publiceren__ en een naam op voor de gepubliceerde iteratie op te geven. Hiermee schakelt u het model toegankelijk voor de voorspelling API van uw aangepaste Vision Azure-resource. 

![Het tabblad prestaties wordt weergegeven, met een rode rechthoek rond de knop publiceren.](./media/use-prediction-api/unpublished-iteration.png)

Wanneer het model is gepubliceerd, ziet u een 'Gepubliceerd' label weergegeven naast uw iteratie in de zijbalk links, evenals de naam van de gepubliceerde iteratie in de beschrijving van de iteratie.

![Het tabblad prestaties wordt weergegeven, met een rode rechthoek rond het label is gepubliceerd en de naam van de gepubliceerde iteratie.](./media/use-prediction-api/published-iteration.png)

## <a name="get-the-url-and-prediction-key"></a>De URL en voorspellingssleutel ophalen

Zodra het model is gepubliceerd, kunt u informatie over het gebruik van de Voorspellings-API door te selecteren ophalen __voorspelling URL__. Hiermee opent u een dialoogvenster zoals het voorbeeld hieronder met informatie voor het gebruik van de Voorspellings-API, met inbegrip van de __voorspelling URL__ en __voorspelling-sleutel__.

![Het tabblad prestaties wordt weergegeven met een rode rechthoek rond de voorspelling van URL-knop.](./media/use-prediction-api/published-iteration-prediction-url.png)

![Het tabblad prestaties wordt weergegeven met een rode rechthoek rond de voorspelling van URL-waarde voor het gebruik van een afbeeldingsbestand en de voorspelling-sleutel-waarde.](./media/use-prediction-api/prediction-api-info.png)

> [!TIP]
> Uw __voorspelling-sleutel__ ook te vinden de [Azure Portal](https://portal.azure.com) pagina voor de aangepaste Vision Azure-Resource die is gekoppeld aan uw project, onder __sleutels__. 

Kopieer de volgende informatie voor gebruik in de toepassing in het dialoogvenster:

* __Voorspelling URL__ voor het gebruik van een __afbeeldingsbestand__.
* __Voorspelling-sleutel__ waarde.

## <a name="create-the-application"></a>De toepassing maken

1. Maak in Visual Studio een nieuwe C#-consoletoepassing.

1. Gebruik de volgende code als de hoofdtekst van het bestand __Program.cs__.

    > [!IMPORTANT]
    > Voer de volgende informatie in:
    >
    > * Stel de __naamruimte__ in op de naam van uw project.
    > * Stel de __voorspelling-sleutel__ waarde die u eerder in de regel die met begint opgehaald `client.DefaultRequestHeaders.Add("Prediction-Key",`.
    > * Stel de __voorspelling URL__ waarde die u eerder in de regel die met begint opgehaald `string url =`.

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
                client.DefaultRequestHeaders.Add("Prediction-Key", "3b9dde6d1ae1453a86bfeb1d945300f2");

                // Prediction URL - replace this example URL with your valid Prediction URL.
                string url = "https://southcentralus.api.cognitive.microsoft.com/customvision/v3.0/Prediction/8622c779-471c-4b6e-842c-67a11deffd7b/classify/iterations/Cats%20vs.%20Dogs%20-%20Published%20Iteration%203/image";

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

## <a name="use-the-application"></a>De toepassing gebruiken

Wanneer de toepassing wordt uitgevoerd, voert u het pad naar een afbeelding in de console. De afbeelding wordt verzonden naar de Voorspellings-API en de voorspellingsresultaten worden geretourneerd als een JSON-document. De volgende JSON is een voorbeeld van het antwoord.

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

[Het model voor mobiele gebruik](export-your-model.md)

[Aan de slag met .NET SDK 's](csharp-tutorial.md)

[Aan de slag met Python-SDK 's](python-tutorial.md)

[Aan de slag met Java-SDK 's](java-tutorial.md)

[Aan de slag met Node-SDK 's](node-tutorial.md)

[Aan de slag met Go SDK 's](go-tutorial.md)
