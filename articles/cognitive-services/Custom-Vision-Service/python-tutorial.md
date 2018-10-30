---
title: 'Zelfstudie: Een afbeeldingsclassificatieproject maken met de Custom Vision-SDK voor Python'
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
ms.openlocfilehash: 96125ba1c54f742bb9ddf32a1588173217be0766
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49953109"
---
# <a name="tutorial-create-an-image-classification-project-with-the-custom-vision-sdk-for-python"></a>Zelfstudie: Een afbeeldingsclassificatieproject maken met de Custom Vision-SDK voor Python

Lees hoe u een afbeeldingsclassificatieproject kunt maken met behulp van de Custom Vision Service en een eenvoudig Python-script. Wanneer u het project hebt gemaakt, kunt u tags toevoegen, afbeeldingen uploaden, het project trainen, de standaardeindpunt-URL voor voorspellingen ophalen en deze gebruiken om afbeeldingen programmatisch te testen. Gebruik dit open-sourcevoorbeeld als sjabloon voor het bouwen van uw eigen app met behulp van de Custom Vision-API.



## <a name="prerequisites"></a>Vereisten

- Python 2.7+ of Python 3.5+.
- Het pip-hulpprogramma.

## <a name="get-the-training-and-prediction-keys"></a>De training en voorspellingssleutels ophalen

Als u de sleutels wilt ophalen die in dit voorbeeld worden gebruikt, gaat u naar de [Custom Vision-webpagina](https://customvision.ai) en selecteert u het __tandwielpictogram__ in de rechterbovenhoek. In het sectie __Accounts__ kopieert u de waarden uit de velden __Trainingssleutel__ en __Voorspellingssleutel__.

![Afbeelding van de gebruikersinterface van de sleutels](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>De Custom Vision Service-SDK installeren

Als u de Custom Vision Service-SDK wilt installeren, gebruikt u de volgende opdracht:

```
pip install azure-cognitiveservices-vision-customvision
```

## <a name="get-example-images"></a>Voorbeeldafbeeldingen

In dit voorbeeld worden de afbeeldingen uit de `Samples/Images`-directory van het [https://github.com/Microsoft/Cognitive-CustomVision-Windows](https://github.com/Microsoft/Cognitive-CustomVision-Windows/tree/master/Samples/Images)-project gebruikt. Kloon of download en extraheer het project in uw ontwikkelingsomgeving.

## <a name="create-a-custom-vision-service-project"></a>Een Custom Vision Service-project maken

Als u een nieuw Custom Vision Service-project wilt maken, maakt u een nieuw bestand met de naam `sample.py`. Gebruik de volgende code als de bestandsinhoud:

> [!IMPORTANT]
> Stel de `training_key` in op de trainingssleutelwaarde die u eerder hebt opgehaald.
>
> Stel de `prediction_key` in op de voorspellingssleutelwaarde die u eerder hebt opgehaald.

```python
from azure.cognitiveservices.vision.customvision.training import training_api
from azure.cognitiveservices.vision.customvision.training.models import ImageUrlCreateEntry

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"

trainer = training_api.TrainingApi(training_key)

# Create a new project
print ("Creating project...")
project = trainer.create_project("My Project")
```

## <a name="add-tags-to-your-project"></a>Tags toevoegen aan uw project

Voeg de volgende code toe aan het eind van het bestand `sample.py` om tags aan uw project toe te voegen:

```python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

## <a name="upload-images-to-the-project"></a>Afbeeldingen uploaden naar het project

Als u de voorbeeldafbeeldingen aan het project wilt toevoegen, voegt u de volgende code in nadat u de tag hebt gemaakt. Met deze code wordt de afbeelding met de overeenkomstige tag geüpload:

> [!IMPORTANT]
>
> Wijzig het pad naar de afbeeldingen op basis van de locatie waar u het Cognitive-CustomVision-Windows-project eerder hebt gedownload.

```python
base_image_url = "https://raw.githubusercontent.com/Microsoft/Cognitive-CustomVision-Windows/master/Samples/"

print ("Adding images...")
for image_num in range(1,10):
    image_url = base_image_url + "Images/Hemlock/hemlock_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(url=image_url, tag_ids=[ hemlock_tag.id ] ) ])

for image_num in range(1,10):
    image_url = base_image_url + "Images/Japanese Cherry/japanese_cherry_{}.jpg".format(image_num)
    trainer.create_images_from_urls(project.id, [ ImageUrlCreateEntry(url=image_url, tag_ids=[ cherry_tag.id ] ) ])


# Alternatively, if the images were on disk in a folder called Images alongside the sample.py, then
# they can be added by using the following:
#
#import os
#hemlock_dir = "Images\\Hemlock"
#for image in os.listdir(os.fsencode("Images\\Hemlock")):
#    with open(hemlock_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data, [ hemlock_tag.id ])
#
#cherry_dir = "Images\\Japanese Cherry"
#for image in os.listdir(os.fsencode("Images\\Japanese Cherry")):
#    with open(cherry_dir + "\\" + os.fsdecode(image), mode="rb") as img_data: 
#        trainer.create_images_from_data(project.id, img_data, [ cherry_tag.id ])
```

## <a name="train-the-project"></a>Het project trainen

Voeg de volgende code toe aan het eind van het bestand `sample.py` om de classificatie te trainen:

```python
import time

print ("Training...")
iteration = trainer.train_project(project.id)
while (iteration.status != "Completed"):
    iteration = trainer.get_iteration(project.id, iteration.id)
    print ("Training status: " + iteration.status)
    time.sleep(1)

# The iteration is now trained. Make it the default project endpoint
trainer.update_iteration(project.id, iteration.id, is_default=True)
print ("Done!")
```

## <a name="get-and-use-the-default-prediction-endpoint"></a>Het standaardeindpunt voor voorspellingen ophalen en gebruiken

Als u een afbeelding naar het voorspellingseindpunt wilt verzenden en de voorspelling wilt ophalen, voegt u de volgende code toe aan het einde van het `sample.py`-bestand:

```python
from azure.cognitiveservices.vision.customvision.prediction import prediction_endpoint
from azure.cognitiveservices.vision.customvision.prediction.prediction_endpoint import models

# Now there is a trained endpoint that can be used to make a prediction

predictor = prediction_endpoint.PredictionEndpoint(prediction_key)

test_img_url = base_image_url + "Images/Test/test_image.jpg"
results = predictor.predict_image_url(project.id, iteration.id, url=test_img_url)

# Alternatively, if the images were on disk in a folder called Images alongside the sample.py, then
# they can be added by using the following.
#
# Open the sample image and get back the prediction results.
# with open("Images\\test\\test_image.jpg", mode="rb") as test_data:
#     results = predictor.predict_image(project.id, test_data, iteration.id)

# Display the results.
for prediction in results.predictions:
    print ("\t" + prediction.tag_name + ": {0:.2f}%".format(prediction.probability * 100))
```

## <a name="run-the-example"></a>Het voorbeeld uitvoeren

Voer de oplossing uit. De voorspellingsresultaten worden in de console weergegeven.

```
python sample.py
```

De uitvoer van de toepassing is vergelijkbaar met de volgende tekst:

```
Creating project...
Adding images...
Training...
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```