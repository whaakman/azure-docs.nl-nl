---
title: Gebruik Custom Vision Service vanuit een C#-toepassing - Azure Cognitive Services | Microsoft Docs
description: Verken een eenvoudige C#-app die gebruikmaakt van de aangepaste Vision-API in Microsoft Cognitive Services. Maak een project, tags toevoegen, afbeeldingen uploaden, trainen van uw project en een voorspelling te maken met behulp van het eindpunt.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: d3c2ffb0fd9578458bd07241eed4a87cf70d3c3c
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/22/2018
ms.locfileid: "42617431"
---
# <a name="use-the-custom-vision-service-from-a-c35-application"></a>Gebruik de Custom Vision Service uit een C&#35; toepassing

Informatie over het gebruik van de Custom Vision Service vanuit een C#-toepassing. Nadat deze gemaakt, kunt u labels toevoegen, afbeeldingen uploaden, trainen van het project, verkrijgen van het project standaard voorspelling eindpunt-URL en het eindpunt voor het testen van een installatiekopie van een via een programma gebruiken. In dit voorbeeld open-source als sjabloon gebruiken voor het bouwen van uw eigen app voor Windows met behulp van de Custom Vision Service-API.

## <a name="prerequisites"></a>Vereisten

* Een versie van Visual Studio 2017 voor Windows.

## <a name="get-the-custom-vision-sdk-and-samples"></a>Ophalen van de aangepaste Vision SDK en voorbeelden
Als u wilt maken in dit voorbeeld, moet u de aangepaste Vision SDK NuGet-pakketten:

* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
* [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

U kunt downloaden van de installatiekopieën van samen met de [C#-voorbeelden](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples/tree/master/CustomVision).

## <a name="get-the-training-and-prediction-keys"></a>Ophalen van de sleutels trainen en voorspellen

Als u de sleutels in dit voorbeeld gebruikt, gaat u naar de [Custom Vision webpagina](https://customvision.ai) en selecteer de __tandwielpictogram__ in de rechterbovenhoek. In de __Accounts__ sectie, Kopieer de waarden van de __Training sleutel__ en __voorspelling sleutel__ velden.

![Afbeelding van de gebruikersinterface van de sleutels](./media/csharp-tutorial/training-prediction-keys.png)

## <a name="understand-the-code"></a>De code begrijpen

Open in Visual Studio het project zich in de `Samples/CustomVision.Sample/` map van de SDK-project.

Deze toepassing maakt gebruik van de training-sleutel die u eerder hebt opgehaald voor het maken van een nieuw project met de naam __mijn nieuwe Project__. Deze uploadt vervolgens trainen en testen van een classificatie van afbeeldingen. De classificatie geeft aan of een boomstructuur is een __Hemlock__ of een __Japanse Cherry__.

De volgende codefragmenten implementeren van de primaire functie van dit voorbeeld:

* __Maak een nieuw project voor Custom Vision Service__:

    ```csharp
     // Create a new project
    Console.WriteLine("Creating new project:");
    var project = trainingApi.CreateProject("My New Project");
    ```

* __-Tags maken in een project__:

    ```csharp
    // Make two tags in the new project
    var hemlockTag = trainingApi.CreateTag(project.Id, "Hemlock");
    var japaneseCherryTag = trainingApi.CreateTag(project.Id, "Japanese Cherry");
    ```

* __Uploaden en afbeeldingen labelen__:

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

* __Stel een iteratie standaard voor het eindpunt van de voorspelling__:

    ```csharp
    // The iteration is now trained. Make it the default project endpoint
    iteration.IsDefault = true;
    trainingApi.UpdateIteration(project.Id, iteration.Id, iteration);
    Console.WriteLine("Done!\n");
    ```

* __Maak een eindpunt voorspelling__:
 
    ```csharp
    // Create a prediction endpoint, passing in obtained prediction key
    PredictionEndpoint endpoint = new PredictionEndpoint() { ApiKey = predictionKey };
    ```
 
* __Een afbeelding verzenden naar het eindpunt voorspelling__:

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

1. Controleer de volgende wijzigingen aan de trainen en voorspellen sleutels toevoegen aan de toepassing:

    * Voeg uw __training sleutel__ naar de volgende regel:

        ```csharp
        string trainingKey = "<your key here>";
        ```

    * Voeg uw __voorspelling sleutel__ naar de volgende regel:

        ```csharp
        string predictionKey = "<your key here>";
        ```

2. Voer de toepassing uit. Als de toepassing wordt uitgevoerd, wordt de volgende uitvoer geschreven naar de console:

    ```
    Creating new project:
            Uploading images
            Training
    Done!

    Making a prediction:
            Hemlock: 95.0%
            Japanese Cherry: 0.0%
    ```

3. Op een willekeurige toets om de toepassing af te sluiten.
