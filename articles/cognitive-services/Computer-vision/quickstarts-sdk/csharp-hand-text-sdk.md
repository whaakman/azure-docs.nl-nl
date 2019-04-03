---
title: 'Quickstart: handgeschreven tekst extraheren - SDK, C#'
titleSuffix: Azure Cognitive Services
description: In deze snelstart extraheert u tekst uit een afbeelding met behulp van de Computer Vision Windows C#-clientbibliotheek.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 03/26/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 2ae5cd0fd177f64bed5ed0705207c6a3e81a1b24
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58878193"
---
# <a name="quickstart-extract-handwritten-text-using-the-computer-vision-c-sdk"></a>Quickstart: Extraheer handgeschreven tekst met behulp van de Computer Vision C# SDK

In deze quickstart extraheert u handgeschreven of gedrukte tekst uit een afbeelding met behulp van de Computer Vision SDK voor C#. U kunt eventueel de code in deze handleiding downloaden als een volledige voorbeeld-app uit de [Cognitive Services Csharp Vision](https://github.com/Azure-Samples/cognitive-services-vision-csharp-sdk-quickstarts/tree/master/ComputerVision)-opslagplaats op GitHub.

## <a name="prerequisites"></a>Vereisten

* Als u Computer Vision wilt gebruiken, hebt u een abonnementssleutel nodig. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md) voor meer informatie.
* Een versie van [Visual Studio 2015 of 2017](https://www.visualstudio.com/downloads/).
* Het NuGet-pakket van de [Microsoft.Azure.CognitiveServices.Vision.ComputerVision](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.ComputerVision)-clientbibliotheek. U hoeft het pakket niet te downloaden. Hieronder vindt u de installatie-instructies.

## <a name="create-and-run-the-sample-app"></a>De voorbeeld-app maken en uitvoeren

U kunt het voorbeeld uitvoeren aan de hand van de volgende stappen:

1. Maak in Visual Studio een nieuwe Visual C#-console-app.
1. Installeer het NuGet-pakket van de Computer Vision-client-bibliotheek.
    1. Klik in het menu op **Extra**, selecteer **NuGet Package Manager** en vervolgens **NuGet-pakketten voor oplossing beheren**.
    1. Klik op het tabblad **Bladeren** en typ 'Microsoft.Azure.CognitiveServices.Vision.ComputerVision' in het vak **Zoeken**.
    1. Selecteer **Microsoft.Azure.CognitiveServices.Vision.ComputerVision** wanneer dit wordt weergegeven, klik op het selectievakje naast uw projectnaam en klik op **Installeren**.
1. Vervang `Program.cs` door de volgende code. De `BatchReadFileAsync` en `BatchReadFileInStreamAsync` methoden verpakken de [Batch lezen API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/2afb498089f74080d7ef85eb) voor lokale en externe afbeeldingen, respectievelijk. De `GetReadOperationResultAsync` methode wordt de [ophalen lezen bewerking resultaat API](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/5be108e7498a4f9ed20bf96d).

    ```csharp
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision;
    using Microsoft.Azure.CognitiveServices.Vision.ComputerVision.Models;

    using System;
    using System.IO;
    using System.Threading.Tasks;

    namespace ExtractText
    {
        class Program
        {
            // subscriptionKey = "0123456789abcdef0123456789ABCDEF"
            private const string subscriptionKey = "<Subscription key>";

            // For printed text, change to TextRecognitionMode.Printed
            private const TextRecognitionMode textRecognitionMode =
                TextRecognitionMode.Handwritten;

            // localImagePath = @"C:\Documents\LocalImage.jpg"
            private const string localImagePath = @"<LocalImage>";

            private const string remoteImageUrl =
                "https://upload.wikimedia.org/wikipedia/commons/thumb/d/dd/Cursive_Writing_on_Notebook_paper.jpg/800px-Cursive_Writing_on_Notebook_paper.jpg";

            private const int numberOfCharsInOperationId = 36;

            static void Main(string[] args)
            {
                ComputerVisionClient computerVision = new ComputerVisionClient(
                    new ApiKeyServiceClientCredentials(subscriptionKey),
                    new System.Net.Http.DelegatingHandler[] { });

                // You must use the same region as you used to get your subscription
                // keys. For example, if you got your subscription keys from westus,
                // replace "westcentralus" with "westus".
                //
                // Free trial subscription keys are generated in the westcentralus
                // region. If you use a free trial subscription key, you shouldn't
                // need to change the region.

                // Specify the Azure region
                computerVision.Endpoint = "https://westus.api.cognitive.microsoft.com";

                Console.WriteLine("Images being analyzed ...");
                var t1 = ExtractRemoteTextAsync(computerVision, remoteImageUrl);
                var t2 = ExtractLocalTextAsync(computerVision, localImagePath);

                Task.WhenAll(t1, t2).Wait(5000);
                Console.WriteLine("Press ENTER to exit");
                Console.ReadLine();
            }

            // Read text from a remote image
            private static async Task ExtractRemoteTextAsync(
                ComputerVisionClient computerVision, string imageUrl)
            {
                if (!Uri.IsWellFormedUriString(imageUrl, UriKind.Absolute))
                {
                    Console.WriteLine(
                        "\nInvalid remoteImageUrl:\n{0} \n", imageUrl);
                    return;
                }

                // Start the async process to read the text
                BatchReadFileHeaders textHeaders =
                    await computerVision.BatchReadFileAsync(
                        imageUrl, textRecognitionMode);

                await GetTextAsync(computerVision, textHeaders.OperationLocation);
            }

            // Recognize text from a local image
            private static async Task ExtractLocalTextAsync(
                ComputerVisionClient computerVision, string imagePath)
            {
                if (!File.Exists(imagePath))
                {
                    Console.WriteLine(
                        "\nUnable to open or read localImagePath:\n{0} \n", imagePath);
                    return;
                }

                using (Stream imageStream = File.OpenRead(imagePath))
                {
                    // Start the async process to recognize the text
                    BatchReadFileInStreamHeaders textHeaders =
                        await computerVision.BatchReadFileInStreamAsync(
                            imageStream, textRecognitionMode);

                    await GetTextAsync(computerVision, textHeaders.OperationLocation);
                }
            }

            // Retrieve the recognized text
            private static async Task GetTextAsync(
                ComputerVisionClient computerVision, string operationLocation)
            {
                // Retrieve the URI where the recognized text will be
                // stored from the Operation-Location header
                string operationId = operationLocation.Substring(
                    operationLocation.Length - numberOfCharsInOperationId);

                Console.WriteLine("\nCalling GetHandwritingRecognitionOperationResultAsync()");
                ReadOperationResult result =
                    await computerVision.GetReadOperationResultAsync(operationId);

                // Wait for the operation to complete
                int i = 0;
                int maxRetries = 10;
                while ((result.Status == TextOperationStatusCodes.Running ||
                        result.Status == TextOperationStatusCodes.NotStarted) && i++ < maxRetries)
                {
                    Console.WriteLine(
                        "Server status: {0}, waiting {1} seconds...", result.Status, i);
                    await Task.Delay(1000);

                    result = await computerVision.GetReadOperationResultAsync(operationId);
                }

                // Display the results
                Console.WriteLine();
                var recResults = result.RecognitionResults;
                foreach (TextRecognitionResult recResult in recResults)
                {
                    foreach (Line line in recResult.Lines)
                    {
                        Console.WriteLine(line.Text);
                    }
                }
                Console.WriteLine();
            }
        }
    }
    ```

1. Vervang `<Subscription Key>` door uw geldige abonnementssleutel.
1. Wijzig zo nodig `computerVision.Endpoint` in de Azure-regio die is gekoppeld aan uw abonnementssleutels.
1. Stel `textRecognitionMode` optioneel in op `TextRecognitionMode.Printed`.
1. Vervang `<LocalImage>` door het pad en de naam van een lokaal bestand.
1. Stel `remoteImageUrl` eventueel in op een andere afbeelding.
1. Voer het programma uit.

## <a name="examine-the-response"></a>Het antwoord bekijken

In een geslaagd antwoord worden de regels herkende tekst voor elke afbeelding afgedrukt.

```console
Calling GetHandwritingRecognitionOperationResultAsync()

Calling GetHandwritingRecognitionOperationResultAsync()
Server status: Running, waiting 1 seconds...
Server status: Running, waiting 1 seconds...

dog
The quick brown fox jumps over the lazy
Pack my box with five dozen liquor jugs
```

Zie [Quickstart: Handgeschreven tekst extraheren - REST, C#](../QuickStarts/CSharp-hand-text.md#examine-the-response) voor een voorbeeld van onbewerkte JSON-uitvoer van de API-aanroep.

## <a name="next-steps"></a>Volgende stappen

Bekijk de Computer Vision-API's die worden gebruikt om een afbeelding te analyseren, beroemdheden en oriëntatiepunten te detecteren, een miniatuur te maken en gedrukte en handgeschreven tekst te verkrijgen.

> [!div class="nextstepaction"]
> [Computer Vision-API verkennen](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44)
