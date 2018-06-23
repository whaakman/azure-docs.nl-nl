---
title: Gebruik de aangepaste visie Service voorspelling endpoint - cognitieve Azure-Services | Microsoft Docs
description: Informatie over het gebruik van de API voor het testen van afbeeldingen met de classificatie van uw aangepaste visie Service via een programma.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 54f9d9fec1f40c167341dec6a8699b6a558419da
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345048"
---
# <a name="use-the-prediction-endpoint-to-test-images-programmatically-with-a-custom-vision-service-classifier"></a>Het eindpunt van de voorspelling gebruiken voor het testen van afbeeldingen via een programma met een classificatie aangepaste visie Service

Nadat u uw model training, kunt u installatiekopieën programmatisch testen door indienen bij de voorspelling-API. 

> [!NOTE]
> Dit document wordt gedemonstreerd met C# verzenden van een afbeelding voor de voorspelling-API. Zie voor meer informatie en voorbeelden van het gebruik van de API de [voorspelling API-referentiemateriaal](https://go.microsoft.com/fwlink/?linkid=865445).

## <a name="get-the-url-and-prediction-key"></a>De URL en voorspelling sleutel ophalen

Van de [aangepaste visie webpagina](https://customvision.ai), selecteer uw project en selecteer vervolgens de __prestaties__ tabblad. Als u wilt weergeven van informatie over het gebruik van de voorspelling API, selecteer __voorspelling URL__. Kopieer de volgende informatie voor gebruik in de toepassing:

* __URL__ voor het gebruik van een __installatiekopiebestand__.
* __Voorspelling sleutel__ waarde.

> [!TIP]
> Als u meerdere pogingen hebt, kunt u bepalen welke regel wordt gebruikt door ingesteld als standaard. Selecteer de herhaling van de __iteraties__ sectie en selecteer vervolgens __instellen als standaardbrowser__ boven aan de pagina.

![Het tabblad prestaties wordt weergegeven met een rode rechthoek met daarin de URL van de voorspelling.](./media/use-prediction-api/prediction-url.png)

## <a name="create-the-application"></a>De toepassing maken

1. Maak een nieuwe C#-consoletoepassing vanuit Visual Studio.

2. De volgende code gebruiken als de hoofdtekst van de __Program.cs__ bestand.

    > [!IMPORTANT]
    > Wijzigen van de volgende informatie:
    >
    > * Stel de __naamruimte__ op de naam van uw project.
    > * Stel de __voorspelling sleutel__ u eerder in de regel die met de begint ontvangen waarde `client.DefaultRequestHeaders.Add("Prediction-Key",`.
    > * Stel de __URL__ u eerder in de regel die met de begint ontvangen waarde `string url =`.

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

Wanneer de toepassing wordt uitgevoerd, voert u het pad naar een afbeelding. De afbeelding wordt verzonden naar de API en de resultaten worden geretourneerd als een JSON-document. De volgende JSON is een voorbeeld van het antwoord

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

[Het model voor mobiel gebruik exporteren](export-your-model.md)