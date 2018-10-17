---
title: 'Voorbeeld: gebruik een voorspellingseindpunt om afbeeldingen programmatisch te testen met classificatie: Custom Vision'
titlesuffix: Azure Cognitive Services
description: Lees hoe u de API gebruikt om afbeeldingen programmatisch te testen met uw Custom Vision Service-classificatie.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: sample
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 3a81f3cef6aaeb5c98022d9fc93f4d84f3f58a6e
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46363646"
---
# <a name="use-the-prediction-endpoint-to-test-images-programmatically-with-a-custom-vision-service-classifier"></a>Het voorspellingseindpunt gebruiken om afbeeldingen programmatisch te testen met een Custom Vision Service-classificatie

Nadat u uw model hebt getraind, kunt u afbeeldingen programmatisch testen door ze in te dienen bij de voorspellings-API. 

> [!NOTE]
> In dit document ziet u hoe u een afbeelding bij de voorspellings-API kunt indienen met behulp van C#. Zie het document [Referentie voorspellings-API](https://go.microsoft.com/fwlink/?linkid=865445) voor meer informatie en voorbeelden van het gebruik van de API.

## <a name="get-the-url-and-prediction-key"></a>De URL en voorspellingssleutel ophalen

Op de [Custom Vision-webpagina](https://customvision.ai) selecteert u uw project en vervolgens selecteert u het tabblad __Prestaties__. Als u informatie over het gebruik van de voorspellings-API wilt weergeven, inclusief de __voorspellingssleutel__, selecteert u __Voorspellings-URL__. Voor projecten die aan een Azure-resource zijn gekoppeld, kunt u uw __voorspellingssleutel__ ook vinden op de [Azure Portal](https://portal.azure.com)-pagina voor bijbehorende Azure-resources onder __Sleutels__. Kopieer de volgende informatie voor gebruik in de toepassing:

* __URL__ voor het gebruik van een __afbeeldingsbestand__.
* De waarde __Voorspellingssleutel__.

> [!TIP]
> Als u meerdere iteraties hebt, kunt u regelen welke iteratie wordt gebruikt door deze als standaard in te stellen. Selecteer de iteratie in de sectie __Iteraties__ en selecteer vervolgens __Standaard maken__ bovenaan de pagina.

![Het tabblad Prestaties wordt weergegeven met een rode rechthoek rondom de voorspellings-URL.](./media/use-prediction-api/prediction-url.png)

## <a name="create-the-application"></a>De toepassing maken

1. Maak in Visual Studio een nieuwe C#-consoletoepassing.

2. Gebruik de volgende code als de hoofdtekst van het bestand __Program.cs__.

    > [!IMPORTANT]
    > Voer de volgende informatie in:
    >
    > * Stel de __naamruimte__ in op de naam van uw project.
    > * Stel in de regel die met `client.DefaultRequestHeaders.Add("Prediction-Key",` begint de waarde voor de __voorspellingssleutel__ in die u eerder hebt ontvangen.
    > * Stel in de regel die met `string url =` begint de __URL__-waarde in die u eerder hebt ontvangen.

    ```csharp
    using System;
    using System.IO;
    using System.Net.Http;
    using System.Net.Http.Headers;
    using System.Threading.Tasks;

    namespace CSPredictionSample
    {
        static class Program
        {
            static void Main()
            {
                Console.Write("Enter image file path: ");
                string imageFilePath = Console.ReadLine();

                MakePredictionRequest(imageFilePath).Wait();

                Console.WriteLine("\n\n\nHit ENTER to exit...");
                Console.ReadLine();
            }

            static byte[] GetImageAsByteArray(string imageFilePath)
            {
                FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
                BinaryReader binaryReader = new BinaryReader(fileStream);
                return binaryReader.ReadBytes((int)fileStream.Length);
            }

            static async Task MakePredictionRequest(string imageFilePath)
            {
                var client = new HttpClient();

                // Request headers - replace this example key with your valid subscription key.
                client.DefaultRequestHeaders.Add("Prediction-Key", "13hc77781f7e4b19b5fcdd72a8df7156");

                // Prediction URL - replace this example URL with your valid prediction URL.
                string url = "http://southcentralus.api.cognitive.microsoft.com/customvision/v1.0/prediction/d16e136c-5b0b-4b84-9341-6a3fff8fa7fe/image?iterationId=f4e573f6-9843-46db-8018-b01d034fd0f2";

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
        }
    }
    ```

## <a name="use-the-application"></a>De toepassing gebruiken

Wanneer u de toepassing uitvoert, voert u het pad naar een afbeeldingsbestand in. De afbeelding wordt ingediend bij de API en de resultaten worden geretourneerd als JSON-document. De volgende JSON is een voorbeeld van het antwoord

```json
{
    "Id":"3f76364c-b8ae-4818-a2b2-2794cfbe377a",
    "Project":"2277aca4-7aff-4742-8afb-3682e251c913",
    "Iteration":"84105bfe-73b5-4fcc-addb-756c0de17df2",
    "Created":"2018-05-03T14:15:22.5659829Z",
    "Predictions":[
        {"TagId":"35ac2ad0-e3ef-4e60-b81f-052a1057a1ca","Tag":"dog","Probability":0.102716163},
        {"TagId":"28e1a872-3776-434c-8cf0-b612dd1a953c","Tag":"cat","Probability":0.02037274}
    ]
}
```

## <a name="next-steps"></a>Volgende stappen

[Het model voor mobiele gebruik](export-your-model.md)
