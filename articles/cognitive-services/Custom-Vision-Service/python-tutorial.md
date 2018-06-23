---
title: Maak een aangepaste visie Service Python-zelfstudie - cognitieve Azure-Services | Microsoft Docs
description: Gebruik een eenvoudige Python-app die gebruikmaakt van de aangepaste Vision-API in cognitieve Microsoft-Services. Maak een project, labels toevoegen, afbeeldingen uploaden, training van uw project en maken van een voorspelling met behulp van het standaardeindpunt.
services: cognitive-services
author: areddish
manager: chbuehle
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/07/2018
ms.author: areddish
ms.openlocfilehash: 0359935bf266d4f2a5cf845dd0d23183f4f77b72
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345042"
---
# <a name="custom-vision-api-python-tutorial"></a>Aangepaste visie API Python-zelfstudie

Informatie over het maken van een installatiekopie van classificatie-project met de aangepaste visie-Service en een eenvoudige pythonscript. Nadat deze gemaakt, kunt u tags toevoegen, afbeeldingen uploaden trainen van het project, ophalen van het project standaard voorspelling eindpunt-URL en gebruikt voor het testen van een installatiekopie van een programmatisch. In dit voorbeeld open source als sjabloon gebruiken voor het bouwen van uw eigen app met behulp van de aangepaste Vision-API.



## <a name="prerequisites"></a>Vereisten

- Python 2.7 + of Python 3.5 +.
- Het hulpprogramma pip.

## <a name="get-the-training-and-prediction-keys"></a>De training en voorspelling sleutels ophalen

Als u de sleutels in dit voorbeeld gebruikt, gaat u naar de [aangepaste visie webpagina](https://customvision.ai) en selecteer de __tandwielpictogram pictogram__ in de rechterbovenhoek. In de __Accounts__ sectie, Kopieer de waarden van de __Training sleutel__ en __voorspelling sleutel__ velden.

![Afbeelding van de gebruikersinterface van de sleutels](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>De aangepaste visie Service SDK installeren

Gebruik de volgende opdracht voor het installeren van de aangepaste visie Service SDK:

```
pip install azure-cognitiveservices-vision-customvision
```

## <a name="get-example-images"></a>Voorbeeld afbeeldingen ophalen

In dit voorbeeld wordt de installatiekopieën van de `Samples/Images` map van de [ https://github.com/Microsoft/Cognitive-CustomVision-Windows ](https://github.com/Microsoft/Cognitive-CustomVision-Windows/tree/master/Samples/Images) project. Klonen of downloaden en uitpakken van het project op uw ontwikkelomgeving.

## <a name="create-a-custom-vision-service-project"></a>Een aangepaste visie Service-project maken

Voor het maken van een nieuwe aangepaste visie Service-project maken van nieuw bestand met de naam `sample.py`. De volgende code gebruiken als de inhoud van het bestand:

> [!IMPORTANT]
> Stel de `training_key` aan de sleutelwaarde van training u eerder hebt opgehaald.
>
> Stel de `prediction_key` voor de voorspelling sleutelwaarde dat u eerder hebt opgehaald.

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

## <a name="add-tags-to-your-project"></a>Labels toevoegen aan uw project

Als u wilt labels toevoegen aan uw project, kunt u de volgende code toevoegen aan het einde van de `sample.py` bestand:

```python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

## <a name="upload-images-to-the-project"></a>Uploaden van afbeeldingen aan het project

Als u wilt de installatiekopieën van het voorbeeld toevoegen aan het project, voeg de volgende code na het maken van het label. Deze code uploadt de installatiekopie met de bijbehorende code:

> [!IMPORTANT]
>
> Pad naar de installatiekopieën op basis van waar u eerder hebt gedownload het Cognitive-CustomVision-Windows-project wijzigen.

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

Als u wilt de classificatie training, kunt u de volgende code toevoegen aan het einde van de `sample.py` bestand:

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

## <a name="get-and-use-the-default-prediction-endpoint"></a>Het standaardeindpunt voorspelling gebruiken

Voor het verzenden van een installatiekopie van het eindpunt van de voorspelling en ophalen van de voorspelling, kunt u de volgende code toevoegen aan het einde van de `sample.py` bestand:

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

## <a name="run-the-example"></a>Het voorbeeld uitvoert

Voer de oplossing uit. De resultaten van de voorspelling weergegeven op de console.

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