---
title: 'Zelfstudie: Een afbeeldingsclassificatieproject maken met de Custom Vision-SDK voor Java'
titlesuffix: Azure Cognitive Services
description: Maak een project, voeg tags toe, upload afbeeldingen, train uw project en doe een voorspelling met behulp van het standaardeindpunt.
services: cognitive-services
author: areddish
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: tutorial
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: e302fc580d9c83d269f0deedd051a3ea23bd274e
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957206"
---
# <a name="tutorial-create-an-image-classification-project-with-the-custom-vision-sdk-for-java"></a>Zelfstudie: Een afbeeldingsclassificatieproject maken met de Custom Vision-SDK voor Java

Lees hoe u een afbeeldingsclassificatieproject kunt maken met behulp van de Custom Vision Service en Java. Wanneer u het project hebt gemaakt, kunt u tags toevoegen, afbeeldingen uploaden, het project trainen, de standaardeindpunt-URL voor voorspellingen ophalen en deze gebruiken om afbeeldingen programmatisch te testen. Gebruik dit open-sourcevoorbeeld als sjabloon voor het bouwen van uw eigen app met behulp van de Custom Vision-API.

## <a name="prerequisites"></a>Vereisten

- JDK 7 of 8 is geïnstalleerd.
- Maven is geïnstalleerd.

## <a name="get-the-training-and-prediction-keys"></a>De training en voorspellingssleutels ophalen

Als u de sleutels wilt ophalen die in dit voorbeeld worden gebruikt, gaat u naar de [Custom Vision-webpagina](https://customvision.ai) en selecteert u het __tandwielpictogram__ in de rechterbovenhoek. In het sectie __Accounts__ kopieert u de waarden uit de velden __Trainingssleutel__ en __Voorspellingssleutel__.

![Afbeelding van de gebruikersinterface van de sleutels](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>De Custom Vision Service-SDK installeren

U kunt de Custom Vision-SDK uit de centrale maven-opslagplaats installeren:
* [Training-SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-training)
* [Voorspelling-SDK](https://mvnrepository.com/artifact/com.microsoft.azure.cognitiveservices/azure-cognitiveservices-customvision-prediction)

## <a name="understand-the-code"></a>De code begrijpen

Het volledige project, inclusief afbeeldingen, is beschikbaar bij de [Custom Vision Azure-voorbeelden voor Java-opslagplaats](https://github.com/Azure-Samples/cognitive-services-java-sdk-samples/tree/master). 

Gebruik uw favoriete Java IDE om het `Vision/CustomVision`-project te openen. 

Deze toepassing gebruikt de trainingssleutel die u eerder hebt opgehaald om een nieuw project met de naam __Sample Java Project__ te maken. Vervolgens worden afbeeldingen geüpload om een classificatie te trainen en te testen. De classificatie identificeert of een boom een __Hemlockspar__ of een __Japanse kers__ is.

Met de volgende codefragmenten implementeert u de primaire functionaliteit van dit voorbeeld:

## <a name="create-a-custom-vision-service-project"></a>Een Custom Vision Service-project maken

> [!IMPORTANT]
> Stel de `trainingApiKey` in op de trainingssleutelwaarde die u eerder hebt opgehaald.

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

Het voorbeeld is zodanig opgezet dat de afbeeldingen worden opgenomen in het definitieve pakket. Afbeeldingen worden gelezen uit de sectie met resources van de jar en geüpload naar de service.

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

De vorige fragmentcode maakt gebruik van twee hulpfuncties die de afbeeldingen als resourcestreams ophalen en ze naar de service uploaden.

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

Hiermee wordt de eerste iteratie in het project gemaakt en wordt deze iteratie gemarkeerd als de standaarditeratie. 

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

## <a name="get-and-use-the-default-prediction-endpoint"></a>Het standaardeindpunt voor voorspellingen ophalen en gebruiken

> [!IMPORTANT]
> Stel de `predictionApiKey` in op de voorspellingssleutelwaarde die u eerder hebt opgehaald.

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

U kunt de oplossing als volgt compileren en uitvoeren met maven:

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