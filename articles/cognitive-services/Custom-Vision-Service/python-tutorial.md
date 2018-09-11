---
title: Maak een Custom Vision Service Python-zelfstudie - Azure Cognitive Services | Microsoft Docs
description: Verken een Python-app die gebruikmaakt van de aangepaste Vision-API in Microsoft Cognitive Services. Een project maakt, tags toevoegen, afbeeldingen uploaden, trainen van uw project en een voorspelling met behulp van de standaardeindpunt.
services: cognitive-services
author: areddish
manager: chbuehle
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 08/28/2018
ms.author: areddish
ms.openlocfilehash: df0bdc0bbd2768566336323851f366c9ae280a88
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44301596"
---
# <a name="custom-vision-api-python-tutorial"></a>Aangepaste Vision API Python-zelfstudie

Informatie over het maken van een installatiekopie van classificatie-project met de Custom Vision Service en een eenvoudige Python-script. Nadat deze gemaakt, kunt u labels toevoegen, afbeeldingen uploaden, trainen van het project, van het project standaard voorspelling eindpunt-URL ophalen en gebruiken om te testen via een programma van een installatiekopie. In dit voorbeeld open-source als sjabloon gebruiken voor het bouwen van uw eigen app met behulp van de aangepaste Vision-API.



## <a name="prerequisites"></a>Vereiste onderdelen

- Python 2.7 + of Python 3.5 +.
- De pip-hulpprogramma.

## <a name="get-the-training-and-prediction-keys"></a>Ophalen van de sleutels trainen en voorspellen

Als u de sleutels in dit voorbeeld gebruikt, gaat u naar de [Custom Vision webpagina](https://customvision.ai) en selecteer de __tandwielpictogram__ in de rechterbovenhoek. In de __Accounts__ sectie, Kopieer de waarden van de __Training sleutel__ en __voorspelling sleutel__ velden.

![Afbeelding van de gebruikersinterface van de sleutels](./media/python-tutorial/training-prediction-keys.png)

## <a name="install-the-custom-vision-service-sdk"></a>De Custom Vision Service SDK installeren

Gebruik de volgende opdracht voor het installeren van de Custom Vision Service SDK:

```
pip install azure-cognitiveservices-vision-customvision
```

## <a name="get-example-images"></a>Voorbeeld van de afbeeldingen ophalen

In dit voorbeeld wordt de installatiekopieën van de `Samples/Images` map van de [ https://github.com/Microsoft/Cognitive-CustomVision-Windows ](https://github.com/Microsoft/Cognitive-CustomVision-Windows/tree/master/Samples/Images) project. Klonen of downloaden en uitpakken van het project naar uw ontwikkelomgeving.

## <a name="create-a-custom-vision-service-project"></a>Maak een project Custom Vision Service

Voor het maken van een nieuw project voor Custom Vision Service, maakt u nieuwe bestand met de naam `sample.py`. Gebruik de volgende code als de inhoud van het bestand:

> [!IMPORTANT]
> Stel de `training_key` aan de training-sleutelwaarde die u eerder hebt opgehaald.
>
> Stel de `prediction_key` voor de voorspelling-sleutelwaarde die u eerder hebt opgehaald.

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

Tags toevoegen aan uw project, kunt u de volgende code toevoegen aan het einde van de `sample.py` bestand:

```python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

## <a name="upload-images-to-the-project"></a>Afbeeldingen uploaden naar het project

Als u wilt de voorbeeldafbeeldingen toevoegen aan het project, voeg de volgende code na het maken van de tag. Deze code wordt de installatiekopie met de bijbehorende code geüpload:

> [!IMPORTANT]
>
> Pad naar de installatiekopieën op basis van waar u het Cognitive-CustomVision-Windows-project eerder hebt gedownload wijzigen.

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

Als u wilt de classificatie trainen, kunt u de volgende code toevoegen aan het einde van de `sample.py` bestand:

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

## <a name="get-and-use-the-default-prediction-endpoint"></a>Het standaardeindpunt voor de voorspelling gebruiken

Een installatiekopie naar het eindpunt van de voorspelling verzenden en ophalen van de voorspelling, kunt u de volgende code toevoegen aan het einde van de `sample.py` bestand:

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

Voer de oplossing uit. De voorspellingsresultaten op die worden weergegeven in de console.

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