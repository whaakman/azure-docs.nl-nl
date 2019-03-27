---
title: 'Quickstart: een objectdetectieproject maken met de Custom Vision-SDK voor C#'
titlesuffix: Azure Cognitive Services
description: Maak een project, voeg labels toe, upload afbeeldingen, train uw project en detecteer objecten met de .NET SDK met C#.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: areddish
ms.openlocfilehash: cc66630f57af32e18916e0662a400b38f27000a9
ms.sourcegitcommit: fbfe56f6069cba027b749076926317b254df65e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58472596"
---
# <a name="quickstart-create-an-object-detection-project-with-the-custom-vision-net-sdk"></a>Quickstart: een objectdetectieproject maken met de .NET SDK van Custom Vision

Dit artikel biedt informatie en voorbeeldcode om u op weg te helpen met de Custom Vision-SDK voor C# om een objectdetectiemodel te maken. Wanneer u het project hebt gemaakt, kunt u gelabelde regio's toevoegen, afbeeldingen uploaden, het project trainen, de standaardeindpunt-URL voor voorspellingen ophalen en het eindpunt gebruiken om afbeeldingen programmatisch te testen. Gebruik dit voorbeeld als een sjabloon om uw eigen .NET-toepassing te maken. 

## <a name="prerequisites"></a>Vereisten

- Een versie van [Visual Studio 2015 of 2017](https://www.visualstudio.com/downloads/)

## <a name="get-the-custom-vision-sdk-and-sample-code"></a>De Custom Vision-SDK en voorbeeldcode ophalen

Als u een .NET-app wilt schrijven die gebruikmaakt van Custom Vision, hebt u de NuGet-pakketten van Custom Vision nodig. Deze zijn opgenomen in het voorbeeldproject dat u gaat downloaden, maar u kunt ze hier afzonderlijk openen.

- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Training/)
- [Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction](https://www.nuget.org/packages/Microsoft.Azure.CognitiveServices.Vision.CustomVision.Prediction/)

Kloon of download het project [.NET-voorbeelden van Cognitive Services](https://github.com/Azure-Samples/cognitive-services-dotnet-sdk-samples). Ga naar de map **CustomVision/ObjectDetection** en open _ObjectDetection.csproj_ in Visual Studio.

Met dit Visual Studio-project wordt een nieuw Custom Vision-project gemaakt met de naam __My New Project__. Dit kan worden geopend via de [Custom Vision-website](https://customvision.ai/). Vervolgens worden afbeeldingen geüpload om een objectdetectiemodel te trainen en te testen. In dit project wordt het model getraind om vorken en scharen in afbeeldingen te detecteren.

[!INCLUDE [get-keys](includes/get-keys.md)]

## <a name="understand-the-code"></a>De code begrijpen

Open het bestand _Program.cs_ en inspecteer de code. Voeg uw abonnementssleutels in de juiste definities in in de methode **Main**.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=18-27)]

De parameter Endpoint moet verwijzen naar de regio waarin de Azure-resourcegroep met de Custom Vision-resources is gemaakt. In dit voorbeeld wordt uitgegaan van de regio US - zuid-centraal en gebruiken we:

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ImageClassification/Program.cs?range=14-14)]

### <a name="create-a-new-custom-vision-service-project"></a>Een nieuw Custom Vision Service-project maken

Met het volgende deel van de code maakt u een objectdetectieproject. Het project wordt weergegeven op de [Custom Vision-website](https://customvision.ai/), die u eerder hebt bezocht. 

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=29-35)]

### <a name="add-tags-to-the-project"></a>Label aan het project toevoegen

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=37-39)]

### <a name="upload-and-tag-images"></a>Afbeeldingen uploaden en labelen

Als u afbeeldingen labelt in objectdetectieprojecten, dient u de regio van elk gelabeld object op te geven met behulp van genormaliseerde coördinaten. Met de volgende code wordt elk voorbeeld van een afbeelding aan de bijbehorende gelabelde regio gekoppeld.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=41-84)]

Vervolgens wordt deze kaart met koppelingen gebruikt om elke voorbeeldafbeelding met de bijbehorende regiocoördinaten te uploaden.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=86-104)]

Op dit punt aangekomen, zijn alle voorbeeldafbeeldingen geüpload en hebben ze allemaal een label (**vork** of **schaar**) en een bijbehorend, uit pixels bestaande rechthoek voor dat label.

### <a name="train-the-project"></a>Het project trainen

Met deze code wordt de eerste trainingsiteratie in het project gemaakt.

[!code-csharp[](~/cognitive-services-dotnet-sdk-samples/CustomVision/ObjectDetection/Program.cs?range=106-117)]

### <a name="publish-the-current-iteration"></a>De huidige iteratie publiceren

De naam van de gepubliceerde iteratie kan worden gebruikt om voorspelling aanvragen te verzenden. Een iteratie is niet beschikbaar in de voorspelling eindpunt totdat deze is gepubliceerd.

```csharp
// The iteration is now trained. Publish it to the prediction end point.
var publishedModelName = "treeClassModel";
var predictionResourceId = "<target prediction resource ID>";
trainingApi.PublishIteration(project.Id, iteration.Id, publishedModelName, predictionResourceId);
Console.WriteLine("Done!\n");
```

### <a name="create-a-prediction-endpoint"></a>Een voorspellingseindpunt maken

```csharp
// Create a prediction endpoint, passing in the obtained prediction key
CustomVisionPredictionClient endpoint = new CustomVisionPredictionClient()
{
        ApiKey = predictionKey,
        Endpoint = SouthCentralUsEndpoint
};
```

### <a name="use-the-prediction-endpoint"></a>Voorspellingseindpunt gebruiken

In dit deel van het script wordt de testafbeelding geladen, wordt een query op het eindpunt van het model uitgevoerd en worden de gegevens van de voorspelling op de console weergegeven.

```csharp
// Make a prediction against the new project
Console.WriteLine("Making a prediction:");
var imageFile = Path.Combine("Images", "test", "test_image.jpg");
using (var stream = File.OpenRead(imageFile))
{
        var result = endpoint.DetectImage(project.Id, publishedModelName, File.OpenRead(imageFile));

        // Loop over each prediction and write out the results
        foreach (var c in result.Predictions)
        {
                Console.WriteLine($"\t{c.TagName}: {c.Probability:P1} [ {c.BoundingBox.Left}, {c.BoundingBox.Top}, {c.BoundingBox.Width}, {c.BoundingBox.Height} ]");
        }
}
```

## <a name="run-the-application"></a>De toepassing uitvoeren

Terwijl de toepassing wordt uitgevoerd, wordt een consolevenster geopend en wordt de volgende uitvoer geschreven:

```
Creating new project:
        Training
Done!

Making a prediction:
        fork: 98.2% [ 0.111609578, 0.184719115, 0.6607002, 0.6637112 ]
        scissors: 1.2% [ 0.112389535, 0.119195729, 0.658031344, 0.7023591 ]
```

Vervolgens kunt u controleren of de testafbeelding (gevonden in **Images/Test/**) op de juiste wijze wordt gelabeld en of de detectieregio juist is. Druk nu op een willekeurige toets om de toepassing af te sluiten.

[!INCLUDE [clean-od-project](includes/clean-od-project.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt nu gezien hoe elke stap van het objectdetectieproces in code kan worden uitgevoerd. Met dit voorbeeld wordt één trainingsinteratie uitgevoerd, maar vaak zult u uw model meerdere keren willen trainen en testen om het nauwkeuriger te maken. In de volgende handleiding wordt classificatie van afbeeldingen behandeld. De principes zijn soortgelijk aan die van objectdetectie.

> [!div class="nextstepaction"]
> [Een model testen en opnieuw trainen](test-your-model.md)
