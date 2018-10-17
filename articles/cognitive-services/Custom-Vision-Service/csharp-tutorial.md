---
title: 'Zelfstudie: Een Windows-app bouwen voor Custom Vision Service met C#'
titlesuffix: Azure Cognitive Services
description: Maak een project, voeg tags toe, upload afbeeldingen, train uw project en doe een voorspelling met behulp van het standaardeindpunt.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 9e5ed71d4620f7ffeac8acb15f90d67964a86870
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46366638"
---
# <a name="tutorial-use-the-custom-vision-service-from-a-c-application"></a>Zelfstudie: De Custom Vision Service uit een C#-toepassing gebruiken

Lees hoe u de Custom Vision Service uit een C#-toepassing gebruikt. Wanneer u het project hebt gemaakt, kunt u tags toevoegen, afbeeldingen uploaden, het project trainen, de standaardeindpunt-URL voor voorspellingen ophalen en het eindpunt gebruiken om afbeeldingen programmatisch te testen. Gebruik dit open-sourcevoorbeeld als sjabloon voor het bouwen van uw eigen app voor Windows met behulp van de Custom Vision Service-API.

## <a name="prerequisites"></a>Vereisten

* Elke versie van Visual Studio 2017 voor Windows.

## <a name="get-the-custom-vision-sdk-and-samples"></a>De Custom Vision-SDK en voorbeelden ophalen
Als u dit voorbeeld wilt maken, hebt u de NuGet-pakketten van de Custom Vision-SDK nodig:

* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

U kunt de afbeeldingen tegelijk met de [C#-voorbeelden](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/CustomVision) downloaden.

## <a name="get-the-training-and-prediction-keys"></a>De training en voorspellingssleutels ophalen

Als u de sleutels wilt ophalen die in dit voorbeeld worden gebruikt, gaat u naar de [Custom Vision-webpagina](https://customvision.ai) en selecteert u het __tandwielpictogram__ in de rechterbovenhoek. In de sectie __Accounts__ kopieert u de waarden uit de velden __Trainingssleutel__ en __Voorspellingssleutel__.

![Afbeelding van de gebruikersinterface van de sleutels](./media/csharp-tutorial/training-prediction-keys.png)

## <a name="understand-the-code"></a>De code begrijpen

In Visual Studio opent u het project dat zich in de `Samples/CustomVision.Sample/`-directory van het SDK-project bevindt.

Deze toepassing gebruikt de trainingssleutel die u eerder hebt opgehaald om een nieuw project met de naam __Mijn nieuwe project__ te maken. Vervolgens worden afbeeldingen geüpload om een classificatie te trainen en te testen. De classificatie identificeert of een boom een __Hemlockspar__ of een __Japanse kers__ is.

Met de volgende codefragmenten implementeert u de primaire functionaliteit van dit voorbeeld:

* __Een nieuw Custom Vision Service-project maken__:

    ```csharp
     // Create a new project
    Console.WriteLine("Creating new project:");
    var project = trainingApi.CreateProject("My New Project");
    ```

* __Tags maken in een project__:

    ```csharp
    // Make two tags in the new project
    var hemlockTag = trainingApi.CreateTag(project.Id, "Hemlock");
    var japaneseCherryTag = trainingApi.CreateTag(project.Id, "Japanese Cherry");
    ```

* __Afbeeldingen uploaden en labellen__:

    ```csharp
    // Add some images to the tags
    Console.WriteLine("\tUploading images");
    LoadImagesFromDisk();

    // Images can be uploaded one at a time
    foreach (var image in hemlockImages)
    {
        using (var stream = new MemoryStream(File.ReadAllBytes(image)))
        {
            trainingApi.CreateImagesFromData(project.Id, stream, new List<string>() { hemlockTag.Id.ToString() });
        }
    }

    // Or uploaded in a single batch 
    var imageFiles = japaneseCherryImages.Select(img => new ImageFileCreateEntry(Path.GetFileName(img), File.ReadAllBytes(img))).ToList();
    trainingApi.CreateImagesFromFiles(project.Id, new ImageFileCreateBatch(imageFiles, new List<Guid>() { japaneseCherryTag.Id }));
    ```

* __De classificatie trainen__:

    ```csharp
    // Now there are images with tags start training the project
    Console.WriteLine("\tTraining");
    var iteration = trainingApi.TrainProject(project.Id);

    // The returned iteration will be in progress, and can be queried periodically to see when it has completed
    while (iteration.Status == "Completed")
    {
        Thread.Sleep(1000);

        // Re-query the iteration to get it's updated status
        iteration = trainingApi.GetIteration(project.Id, iteration.Id);
    }
    ```

* __Een standaarditeratie instellen voor het voorspellingseindpunt__:

    ```csharp
    // The iteration is now trained. Make it the default project endpoint
    iteration.IsDefault = true;
    trainingApi.UpdateIteration(project.Id, iteration.Id, iteration);
    Console.WriteLine("Done!\n");
    ```

* __Een eindpunt voor de queryvoorspelling maken__:
 
    ```csharp
    // Create a prediction endpoint, passing in obtained prediction key
    PredictionEndpoint endpoint = new PredictionEndpoint() { ApiKey = predictionKey };
    ```
 
* __Een afbeelding naar het voorspellingseindpunt verzenden__:

    ```csharp
    // Make a prediction against the new project
    Console.WriteLine("Making a prediction:");
    var result = endpoint.PredictImage(project.Id, testImage);

    // Loop over each prediction and write out the results
    foreach (var c in result.Predictions)
    {
        Console.WriteLine($"\t{c.TagName}: {c.Probability:P1}");
    }
    ```

## <a name="run-the-application"></a>De toepassing uitvoeren

1. Breng de volgende wijzigingen aan om de trainings- en voorspellingssleutels aan de toepassing toe te voegen:

    * Voeg uw __trainingssleutel__ toe aan de volgende regel:

        ```csharp
        string trainingKey = "<your key here>";
        ```

    * Voeg uw __voorspellingssleutel__ toe aan de volgende regel:

        ```csharp
        string predictionKey = "<your key here>";
        ```

2. Voer de toepassing uit. Wanneer de toepassing wordt uitgevoerd, wordt de volgende uitvoer naar de console geschreven:

    ```
    Creating new project:
            Uploading images
            Training
    Done!

    Making a prediction:
            Hemlock: 95.0%
            Japanese Cherry: 0.0%
    ```

3. Druk op een willekeurige toets om de toepassing af te sluiten.
