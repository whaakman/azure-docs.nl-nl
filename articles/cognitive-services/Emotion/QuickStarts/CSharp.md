---
title: 'Snelstart: Emoties op gezichten in een afbeelding herkennen - Emotion-API, C#'
titlesuffix: Azure Cognitive Services
description: Hier vindt u informatie en een codevoorbeeld om snel aan de slag te gaan met de Emotion-API en C#.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: quickstart
ms.date: 11/02/2017
ms.author: anroth
ROBOTS: NOINDEX
ms.openlocfilehash: 530d05887e585884b184635e01031c1332fad3fb
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48239367"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Snelstart: een app bouwen voor het herkennen van emoties op gezichten in een afbeelding.

> [!IMPORTANT]
> De Emotion-API wordt op 15 februari 2019 afgeschaft. De mogelijkheid voor de herkenning van emoties is nu algemeen beschikbaar als onderdeel van de [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/).

Dit artikel bevat informatie en een codevoorbeeld om snel aan de slag te gaan met de [Emotion-API-herkenningsmethode](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) en C#. U kunt deze gebruiken om de emoties van een of meer mensen in een afbeelding te herkennen.

## <a name="prerequisites"></a>Vereisten
* Haal de Cognitive Services [Emotion-API-Windows-SDK](https://www.nuget.org/packages/Microsoft.ProjectOxford.Emotion/) op.
* Haal uw gratis [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/) op.

## <a name="emotion-recognition-c-example-request"></a>Emotieherkenning - C#-voorbeeldaanvraag

Maak een nieuwe consoleoplossing in Visual Studio en vervang Program.cs dan door de volgende code. Wijzig `string uri` om de regio te gebruiken waar u de abonnementssleutels hebt verkregen. Vervang de waarde **Ocp-Apim-Subscription-Key** door een geldige abonnementssleutel. U vindt de abonnementssleutel in Azure Portal. In het navigatiedeelvenster links bladert u onder het gedeelte **Sleutels** naar uw Emotion-API-resource. Op vergelijkbare wijze kunt u de juiste verbindings-URI voor uw resource bij **Eindpunt** verkrijgen in het deelvenster **Overzicht**.

![Uw API-resourcesleutels](../../media/emotion-api/keys.png)

Voor het verwerken van het antwoord op uw aanvraag gebruikt u een bibliotheek zoals `Newtonsoft.Json`. Op deze manier kunt u een JSON-tekenreeks verwerken als een reeks beheerbare objecten, ook wel tokens genoemd. Als u deze bibliotheek wilt toevoegen aan uw pakket, klikt u in Solution Explorer met de rechtermuisknop op uw project en selecteert u **NuGet-pakketten beheren**. Zoek dan naar **Newtonsoft**. Het eerste resultaat moet **Newtonsoft.Json** zijn. Selecteer **Installeren**. U kunt nu in uw toepassing verwijzen naar deze bibliotheek.

![Newtonsoft.Json installeren](../../media/emotion-api/newtonsoft-nuget.png)

```csharp
using System;
using System.IO;
using System.Net.Http.Headers;
using System.Net.Http;
using Newtonsoft.Json.Linq;

namespace CSHttpClientSample
{
    static class Program
    {
        static void Main()
        {
            Console.Write("Enter the path to a JPEG image file:");
            string imageFilePath = Console.ReadLine();

            MakeRequest(imageFilePath);

            Console.WriteLine("\n\n\nWait for the result below, then hit ENTER to exit...\n\n\n");
            Console.ReadLine(); // wait for ENTER to exit program
        }

        static byte[] GetImageAsByteArray(string imageFilePath)
        {
            FileStream fileStream = new FileStream(imageFilePath, FileMode.Open, FileAccess.Read);
            BinaryReader binaryReader = new BinaryReader(fileStream);
            return binaryReader.ReadBytes((int)fileStream.Length);
        }

        static async void MakeRequest(string imageFilePath)
        {
            var client = new HttpClient();

            // Request headers - replace this example key with your valid key.
            client.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", "<your-subscription-key>"); //

            // NOTE: You must use the same region in your REST call as you used to obtain your subscription keys.
            //   For example, if you obtained your subscription keys from westcentralus, replace "westus" in the
            //   URI below with "westcentralus".
            string uri = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize?";
            HttpResponseMessage response;
            string responseContent;

            // Request body. Try this sample with a locally stored JPEG image.
            byte[] byteData = GetImageAsByteArray(imageFilePath);

            using (var content = new ByteArrayContent(byteData))
            {
                // This example uses content type "application/octet-stream".
                // The other content types you can use are "application/json" and "multipart/form-data".
                content.Headers.ContentType = new MediaTypeHeaderValue("application/octet-stream");
                response = await client.PostAsync(uri, content);
                responseContent = response.Content.ReadAsStringAsync().Result;
            }

            // A peek at the raw JSON response.
            Console.WriteLine(responseContent);

            // Processing the JSON into manageable objects.
            JToken rootToken = JArray.Parse(responseContent).First;

            // First token is always the faceRectangle identified by the API.
            JToken faceRectangleToken = rootToken.First;

            // Second token is all emotion scores.
            JToken scoresToken = rootToken.Last;

            // Show all face rectangle dimensions
            JEnumerable<JToken> faceRectangleSizeList = faceRectangleToken.First.Children();
            foreach (var size in faceRectangleSizeList) {
                Console.WriteLine(size);
            }

            // Show all scores
            JEnumerable<JToken> scoreList = scoresToken.First.Children();
            foreach (var score in scoreList) {
                Console.WriteLine(score);
            }
        }
    }
}
```

## <a name="recognize-emotions-sample-response"></a>Voorbeeldantwoord voor herkennen van emoties
Een geslaagde aanroep retourneert een matrix van gezichtsvermeldingen en de bijbehorende emotiescores. Deze worden gerangschikt op basis van de rechthoekgrootte van het gezicht (aflopende volgorde). Een leeg antwoord geeft aan dat er geen gezichten zijn gedetecteerd. Een emotievermelding bevat de volgende velden:

* faceRectangle: locatie van de rechthoek met het gezicht in de afbeelding
* scores: emotiescores voor elk gezicht in de afbeelding

```json
application/json
[
  {
    "faceRectangle": {
      "left": 68,
      "top": 97,
      "width": 64,
      "height": 97
    },
    "scores": {
      "anger": 0.00300731952,
      "contempt": 5.14648448E-08,
      "disgust": 9.180124E-06,
      "fear": 0.0001912825,
      "happiness": 0.9875571,
      "neutral": 0.0009861537,
      "sadness": 1.889955E-05,
      "surprise": 0.008229999
    }
  }
]
