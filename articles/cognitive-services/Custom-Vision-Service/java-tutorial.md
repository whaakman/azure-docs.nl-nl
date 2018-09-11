---
title: Maken van een zelfstudie voor Custom Vision Service Java - Azure Cognitive Services | Microsoft Docs
description: Verken een eenvoudige Java-app die gebruikmaakt van de aangepaste Vision-API in Microsoft Cognitive Services. Een project maakt, tags toevoegen, afbeeldingen uploaden, trainen van uw project en een voorspelling met behulp van de standaardeindpunt.
services: cognitive-services
author: areddish
manager: chbuehle
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: a83a2f5cac9281a4cd79c1a0cead0f2af82d73df
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44305701"
---
# <a name="use-custom-vision-api-to-build-an-image-classification-project-with-java"></a>Custom Vision-API gebruiken om een installatiekopie van classificatie-project met behulp van Java te bouwen

Informatie over het maken van een installatiekopie van classificatie-project met de Custom Vision Service met behulp van Java. Nadat deze gemaakt, kunt u labels toevoegen, afbeeldingen uploaden, trainen van het project, van het project standaard voorspelling eindpunt-URL ophalen en gebruiken om te testen via een programma van een installatiekopie. In dit voorbeeld open-source als sjabloon gebruiken voor het bouwen van uw eigen app met behulp van de aangepaste Vision-API.

## <a name="prerequisites"></a>Vereiste onderdelen

- JDK 7 of 8 is geïnstalleerd.
- Maven is geïnstalleerd.

## <a name="get-the-training-and-prediction-keys"></a>Ophalen van de sleutels trainen en voorspellen

Als u de sleutels in dit voorbeeld gebruikt, gaat u naar de [Custom Vision webpagina](https://customvision.ai) en selecteer de __tandwielpictogram__ in de rechterbovenhoek. In de __Accounts__ sectie, Kopieer de waarden van de __Training sleutel__ en __voorspelling sleutel__ velden.

![Afbeelding van de gebruikersinterface van de sleutels](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>De Custom Vision Service SDK installeren

U kunt de aangepaste Vision-SDK installeren vanaf maven centrale opslagplaats:
* [Training SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
* [Voorspelling SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

## <a name="understand-the-code"></a>De code begrijpen

Het volledige project, met inbegrip van afbeeldingen, is beschikbaar via de [Custom Vision Azure-voorbeelden voor Java-opslagplaats](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). 

Uw favoriete IDE voor Java gebruiken om te openen de `Vision/CustomVision` project. 

Deze toepassing maakt gebruik van de training-sleutel die u eerder hebt opgehaald voor het maken van een nieuw project met de naam __Java-voorbeeldproject__. Deze uploadt vervolgens trainen en testen van een classificatie van afbeeldingen. De classificatie geeft aan of een boomstructuur is een __Hemlock__ of een __Japanse Cherry__.

De volgende codefragmenten implementeren van de primaire functie van dit voorbeeld:

## <a name="create-a-custom-vision-service-project"></a>Maak een project Custom Vision Service

> [!IMPORTANT]
> Stel de `trainingApiKey` aan de training-sleutelwaarde die u eerder hebt opgehaald.

```java
final String trainingApiKey = "insert your training key here";
TrainingApi trainClient = CustomVisionTrainingManager.authenticate(trainingApiKey);

Trainings trainer = trainClient.trainings();

System.out.println("Creating project...");
Project project = trainer.createProject()
            .withName("Sample Java Project")
            .execute();
```

## <a name="add-tags-to-your-project"></a>Tags toevoegen aan uw project

```java
// create hemlock tag
Tag hemlockTag = trainer.createTag()
    .withProjectId(project.id())
    .withName("Hemlock")
    .execute();

// create cherry tag
Tag cherryTag = trainer.createTag()
    .withProjectId(project.id())
    .withName("Japanese Cherry")
    .execute();
```

## <a name="upload-images-to-the-project"></a>Afbeeldingen uploaden naar het project

Het voorbeeld is ingesteld om op te nemen van de afbeeldingen in het definitieve pakket. Afbeeldingen worden gelezen uit de sectie met resources van de jar en geüpload naar de service.

```java
System.out.println("Adding images...");
for (int i = 1; i <= 10; i++) {
    String fileName = "hemlock_" + i + ".jpg";
    byte[] contents = GetImage("/Hemlock", fileName);
    AddImageToProject(trainer, project, fileName, contents, hemlockTag.id(), null);
}

for (int i = 1; i <= 10; i++) {
    String fileName = "japanese_cherry_" + i + ".jpg";
    byte[] contents = GetImage("/Japanese Cherry", fileName);
    AddImageToProject(trainer, project, fileName, contents, cherryTag.id(), null);
}
```

De code van de vorige fragment maakt gebruik van twee hulpfuncties die de afbeeldingen als resource stromen ophalen en ze uploaden naar de service.

```java
private static void AddImageToProject(Trainings trainer, Project project, String fileName, byte[] contents, UUID tag)
{
    System.out.println("Adding image: " + fileName);

    ImageFileCreateEntry file = new ImageFileCreateEntry()
        .withName(fileName)
        .withContents(contents);

    ImageFileCreateBatch batch = new ImageFileCreateBatch()
        .withImages(Collections.singletonList(file));

    batch = batch.withTagIds(Collections.singletonList(tag));

    trainer.createImagesFromFiles(project.id(), batch);
}

private static byte[] GetImage(String folder, String fileName)
{
    try {
        return ByteStreams.toByteArray(CustomVisionSamples.class.getResourceAsStream(folder + "/" + fileName));
    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }

    return null;
}
```

## <a name="train-the-project"></a>Het project trainen

Hiermee maakt u de eerste versie in het project en markeert deze herhaling als de standaard-iteratie. 

```java
System.out.println("Training...");
Iteration iteration = trainer.trainProject(project.id());

while (iteration.status().equals("Training"))
{
    System.out.println("Training Status: "+ iteration.status());
    Thread.sleep(1000);
    iteration = trainer.getIteration(project.id(), iteration.id());
}

System.out.println("Training Status: "+ iteration.status());
trainer.updateIteration(project.id(), iteration.id(), iteration.withIsDefault(true));
```

## <a name="get-and-use-the-default-prediction-endpoint"></a>Het standaardeindpunt voor de voorspelling gebruiken

> [!IMPORTANT]
> Stel de `predictionApiKey` voor de voorspelling-sleutelwaarde die u eerder hebt opgehaald.

```java
final String predictionApiKey = "insert your prediction key here";
PredictionEndpoint predictClient = CustomVisionPredictionManager.authenticate(predictionApiKey);

// Use below for predictions from a url
// String url = "some url";
// ImagePrediction results = predictor.predictions().predictImage()
//                         .withProjectId(project.id())
//                         .withUrl(url)
//                         .execute();

// load test image
byte[] testImage = GetImage("/Test", "test_image.jpg");

// predict
ImagePrediction results = predictor.predictions().predictImage()
    .withProjectId(project.id())
    .withImageData(testImage)
    .execute();

for (Prediction prediction: results.predictions())
{
    System.out.println(String.format("\t%s: %.2f%%", prediction.tagName(), prediction.probability() * 100.0f));
}
```

## <a name="run-the-example"></a>Het voorbeeld uitvoeren

Compileren en uitvoeren van de oplossing met behulp van maven:

```
mvn compile exec:java
```

De uitvoer van de toepassing is vergelijkbaar met de volgende tekst:

```
Creating project...
Adding images...
Adding image: hemlock_1.jpg
Adding image: hemlock_2.jpg
Adding image: hemlock_3.jpg
Adding image: hemlock_4.jpg
Adding image: hemlock_5.jpg
Adding image: hemlock_6.jpg
Adding image: hemlock_7.jpg
Adding image: hemlock_8.jpg
Adding image: hemlock_9.jpg
Adding image: hemlock_10.jpg
Adding image: japanese_cherry_1.jpg
Adding image: japanese_cherry_2.jpg
Adding image: japanese_cherry_3.jpg
Adding image: japanese_cherry_4.jpg
Adding image: japanese_cherry_5.jpg
Adding image: japanese_cherry_6.jpg
Adding image: japanese_cherry_7.jpg
Adding image: japanese_cherry_8.jpg
Adding image: japanese_cherry_9.jpg
Adding image: japanese_cherry_10.jpg
Training...
Training status: Training
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```