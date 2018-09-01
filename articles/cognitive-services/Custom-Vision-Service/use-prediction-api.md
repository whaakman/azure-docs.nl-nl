---
title: Gebruik de Custom Vision Service voorspelling endpoint - Azure Cognitive Services | Microsoft Docs
description: Informatie over het gebruik van de API voor het testen van installatiekopieën met de classificatie Custom Vision Service via een programma.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: d7f9b90db06811e16cd0cd6ad2b32a27912cfee5
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43341790"
---
# <a name="use-the-prediction-endpoint-to-test-images-programmatically-with-a-custom-vision-service-classifier"></a>Het eindpunt voorspelling gebruiken voor het testen van installatiekopieën via een programma met een classificatie voor Custom Vision Service

Nadat u uw model te trainen, kunt u installatiekopieën programmatisch testen door indienen bij de Voorspellings-API. 

> [!NOTE]
> Dit document wordt gedemonstreerd met behulp van C# om in te dienen een installatiekopie voor de voorspelling API. Zie voor meer informatie en voorbeelden van het gebruik van de API, de [voorspelling API-verwijzing](https://go.microsoft.com/fwlink/?linkid=865445).

## <a name="get-the-url-and-prediction-key"></a>De URL- en voorspellingsgegevens sleutel ophalen

Uit de [Custom Vision webpagina](https://customvision.ai), selecteert u uw project en selecteer vervolgens de __prestaties__ tabblad. Om weer te geven informatie over het gebruik van de Voorspellings-API, met inbegrip van de __voorspelling-sleutel__, selecteer __voorspelling URL__. Voor projecten die zijn gekoppeld aan een Azure-Resource, uw __voorspelling-sleutel__ ook te vinden de [Azure Portal](https://portal.azure.com) -pagina voor gekoppelde Azure-Resource onder __sleutels__. Kopieer de volgende informatie voor gebruik in de toepassing:

* __URL__ voor het gebruik van een __afbeeldingsbestand__.
* __Voorspelling-sleutel__ waarde.

> [!TIP]
> Als u meerdere pogingen hebt, kunt u bepalen welke waarde wordt gebruikt door ingesteld als standaard. Selecteer de herhaling van de __iteraties__ sectie en selecteer vervolgens __standaard__ aan de bovenkant van de pagina.

![Het tabblad prestaties wordt weergegeven met een rode rechthoek rond de voorspelling-URL.](./media/use-prediction-api/prediction-url.png)

## <a name="create-the-application"></a>De toepassing maken

1. Vanuit Visual Studio, maak een nieuwe C#-consoletoepassing.

2. Gebruik de volgende code als de hoofdtekst van de __Program.cs__ bestand.

    > [!IMPORTANT]
    > Wijzig de volgende informatie:
    >
    > * Stel de __naamruimte__ op de naam van uw project.
    > * Stel de __voorspelling-sleutel__ waarde die u eerder in de regel die met begint ontvangen `client.DefaultRequestHeaders.Add("Prediction-Key",`.
    > * Stel de __URL__ waarde die u eerder in de regel die met begint ontvangen `string url =`.

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

## <a name="use-the-application"></a>Gebruik van de toepassing

Wanneer de toepassing wordt uitgevoerd, voert u het pad naar een afbeeldingsbestand. De afbeelding wordt verzonden naar de API en de resultaten worden geretourneerd als een JSON-document. De volgende JSON wordt een voorbeeld van het antwoord

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

[Exporteer het model voor mobiel gebruik](export-your-model.md)
