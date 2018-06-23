---
title: Aangepaste visie Service gebruiken vanuit een toepassing C# - cognitieve Azure-Services | Microsoft Docs
description: Gebruik een eenvoudige C# app die gebruikmaakt van de aangepaste Vision-API in cognitieve Microsoft-Services. Maak een project, labels toevoegen, afbeeldingen uploaden, training van uw project en een voorspelling te maken met behulp van het standaardeindpunt.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: anroth
ms.openlocfilehash: 80cb022808748ed2c60dff7c363d6020cb4043a8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344903"
---
# <a name="use-the-custom-vision-service-from-a-c35-application"></a>Gebruik van de aangepaste visie Service uit een C&#35; toepassing

Informatie over het gebruik van de aangepaste visie Service vanuit een C#-toepassing. Nadat deze gemaakt, kunt u tags toevoegen, afbeeldingen uploaden trainen van het project, verkrijgen van het project standaard voorspelling eindpunt-URL en het eindpunt voor het testen van een installatiekopie van een programmatisch gebruiken. In dit voorbeeld open source als sjabloon gebruiken voor het bouwen van uw eigen app voor Windows met behulp van de aangepaste visie Service API.

## <a name="prerequisites"></a>Vereisten

* Een versie van Visual Studio 2015 of 2017 voor Windows.

* De [aangepaste visie Service SDK](http://github.com/Microsoft/Cognitive-CustomVision-Windows/). Dit omvat de afbeeldingen in dit document gebruikt en voorbeelden.

## <a name="get-the-training-and-prediction-keys"></a>De training en voorspelling sleutels ophalen

Als u de sleutels in dit voorbeeld gebruikt, gaat u naar de [aangepaste visie webpagina](https://customvision.ai) en selecteer de __tandwielpictogram pictogram__ in de rechterbovenhoek. In de __Accounts__ sectie, Kopieer de waarden van de __Training sleutel__ en __voorspelling sleutel__ velden.

![Afbeelding van de gebruikersinterface van de sleutels](./media/csharp-tutorial/training-prediction-keys.png)

## <a name="understand-the-code"></a>De code begrijpen

In Visual Studio, opent u het project zich in de `Samples/CustomVision.Sample/` map van de SDK-project.

Deze toepassing gebruikt de trainings-sleutel die u eerder hebt opgehaald voor het maken van een nieuw project met de naam __mijn nieuw Project__. Deze geüpload en afbeeldingen trainen en te testen van een classificatie. De classificatie geeft aan of de structuur van een is een __Hemlock__ of een __Japanse Cherry__.

De volgende codefragmenten implementeren van de primaire functionaliteit van dit voorbeeld:

* __Maak een nieuw project voor de aangepaste visie Service__:

    ```csharp
     // Create a new project
    Console.WriteLine("Creating new project:");
    var project = trainingApi.CreateProject("My New Project");
    ```

* __Labels maken in een project__:

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

* __Stel de herhaling van een standaard voor het eindpunt van de voorspelling__:

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
 
* __Verzenden van een installatiekopie van het eindpunt van de voorspelling__:

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

1. Breng de volgende wijzigingen aan de training en voorspelling sleutels toevoegen aan de toepassing:

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

3. Druk op een willekeurige toets om de toepassing te sluiten.
