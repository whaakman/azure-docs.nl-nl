---
title: Object-detectie met Python en aangepaste Vision-API - cognitieve Azure-Services | Microsoft Docs
description: Gebruik een eenvoudige Windows-app die gebruikmaakt van de aangepaste Vision-API in cognitieve Microsoft-Services. Maak een project, labels toevoegen, afbeeldingen uploaden, training van uw project en maken van een voorspelling met behulp van het standaardeindpunt.
services: cognitive-services
author: areddish
manager: chbuehle
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 05/03/2018
ms.author: areddish
ms.openlocfilehash: b946265b431a7dcb16bf99e3bf78e09f2d0a7de3
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36301009"
---
# <a name="use-custom-vision-api-to-build-an-object-detection-project-with-python"></a>Aangepaste Vision-API gebruiken voor het bouwen van een object detectie-project met behulp van Python

Verken een basic pythonscript dat gebruikmaakt van de Computer Vision-API om een object detectie-project te maken. Nadat deze gemaakt, kunt u toevoegen met tags regio's, afbeeldingen uploaden, trainen van het project, verkrijgen van het project standaard voorspelling eindpunt-URL en het eindpunt voor het testen van een installatiekopie van een programmatisch gebruiken. In dit voorbeeld open source als sjabloon gebruiken voor het bouwen van uw eigen app met behulp van de aangepaste Vision-API.

## <a name="prerequisites"></a>Vereisten

Als u wilt gebruiken de zelfstudie, moet u het volgende doen:

- Python 2.7 + of Python 3.5 + installeren.
- Pip installeren.

### <a name="platform-requirements"></a>Platformvereisten
In dit voorbeeld is ontwikkeld voor Python.

### <a name="get-the-custom-vision-sdk"></a>De aangepaste visie SDK ophalen

Als u wilt maken in dit voorbeeld, moet u de SDK voor Python installeren voor de aangepaste Vision-API:

```
pip install azure-cognitiveservices-vision-customvision
```

U kunt de afbeeldingen met downloaden de [Python voorbeelden](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples).

## <a name="step-1-get-the-training-and-prediction-keys"></a>Stap 1: Haal de training en voorspelling sleutels

Als u de sleutels in dit voorbeeld gebruikt, gaat u naar de [aangepaste visie site](https://customvision.ai) en selecteer de __tandwielpictogram pictogram__ in de rechterbovenhoek. In de __Accounts__ sectie, Kopieer de waarden van de __Training sleutel__ en __voorspelling sleutel__ velden.

![Afbeelding van de gebruikersinterface van de sleutels](./media/python-tutorial/training-prediction-keys.png)

In dit voorbeeld wordt de installatiekopieën van [deze locatie](https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/tree/master/samples/vision/images).

## <a name="step-2-create-a-custom-vision-service-project"></a>Stap 2: Een aangepaste visie Service-project maken

Maak een nieuw aangepast visie Service-project, een scriptbestand sample.py maken en toevoegen van de volgende inhoud. Noteer het verschil tussen het maken van de detectie van een object en afbeelding classificatie project is het domein dat is opgegeven voor de create_project-aanroep.

```Python
from azure.cognitiveservices.vision.customvision.training import training_api
from azure.cognitiveservices.vision.customvision.training.models import ImageFileCreateEntry, Region

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"

trainer = training_api.TrainingApi(training_key)

# Find the object detection domain
obj_detection_domain = next(domain for domain in trainer.get_domains() if domain.type == "ObjectDetection")

# Create a new project
print ("Creating project...")
project = trainer.create_project("My Detection Project", domain_id=obj_detection_domain.id)
```

## <a name="step-3-add-tags-to-your-project"></a>Stap 3: Labels toevoegen aan uw project

Als u wilt labels toevoegen aan uw project, voeg de volgende code voor het maken van twee tags:

```Python
# Make two tags in the new project
fork_tag = trainer.create_tag(project.id, "fork")
scissors_tag = trainer.create_tag(project.id, "scissors")
```

## <a name="step-4-upload-images-to-the-project"></a>Stap 4: Afbeeldingen uploaden naar het project

Voor object detectie project dat u wilt uploaden installatiekopie, regio's en labels. De regio is in genormaliseerde coordiantes en geeft de locatie van het label object.

Als u wilt de installatiekopieën, regio en labels toevoegen aan het project, voeg de volgende code na het maken van het label. Houd er rekening mee dat voor deze zelfstudie de gebieden kunnen vastgelegd inline met de code worden. De regio's wordt het begrenzingsvak in genormaliseerde coördinaten opgeven.

```Python

fork_image_regions = {
    "fork_1": [ 0.145833328, 0.3509314, 0.5894608, 0.238562092 ],
    "fork_2": [ 0.294117659, 0.216944471, 0.534313738, 0.5980392 ],
    "fork_3": [ 0.09191177, 0.0682516545, 0.757352948, 0.6143791 ],
    "fork_4": [ 0.254901975, 0.185898721, 0.5232843, 0.594771266 ],
    "fork_5": [ 0.2365196, 0.128709182, 0.5845588, 0.71405226 ],
    "fork_6": [ 0.115196079, 0.133611143, 0.676470637, 0.6993464 ],
    "fork_7": [ 0.164215669, 0.31008172, 0.767156839, 0.410130739 ],
    "fork_8": [ 0.118872553, 0.318251669, 0.817401946, 0.225490168 ],
    "fork_9": [ 0.18259804, 0.2136765, 0.6335784, 0.643790841 ],
    "fork_10": [ 0.05269608, 0.282303959, 0.8088235, 0.452614367 ],
    "fork_11": [ 0.05759804, 0.0894935, 0.9007353, 0.3251634 ],
    "fork_12": [ 0.3345588, 0.07315363, 0.375, 0.9150327 ],
    "fork_13": [ 0.269607842, 0.194068655, 0.4093137, 0.6732026 ],
    "fork_14": [ 0.143382356, 0.218578458, 0.7977941, 0.295751631 ],
    "fork_15": [ 0.19240196, 0.0633497, 0.5710784, 0.8398692 ],
    "fork_16": [ 0.140931368, 0.480016381, 0.6838235, 0.240196079 ],
    "fork_17": [ 0.305147052, 0.2512582, 0.4791667, 0.5408496 ],
    "fork_18": [ 0.234068632, 0.445702642, 0.6127451, 0.344771236 ],
    "fork_19": [ 0.219362751, 0.141781077, 0.5919118, 0.6683006 ],
    "fork_20": [ 0.180147052, 0.239820287, 0.6887255, 0.235294119 ]
}

scissors_image_regions = {
    "scissors_1": [ 0.4007353, 0.194068655, 0.259803921, 0.6617647 ],
    "scissors_2": [ 0.426470578, 0.185898721, 0.172794119, 0.5539216 ],
    "scissors_3": [ 0.289215684, 0.259428144, 0.403186262, 0.421568632 ],
    "scissors_4": [ 0.343137264, 0.105833367, 0.332107842, 0.8055556 ],
    "scissors_5": [ 0.3125, 0.09766343, 0.435049027, 0.71405226 ],
    "scissors_6": [ 0.379901975, 0.24308826, 0.32107842, 0.5718954 ],
    "scissors_7": [ 0.341911763, 0.20714055, 0.3137255, 0.6356209 ],
    "scissors_8": [ 0.231617644, 0.08459154, 0.504901946, 0.8480392 ],
    "scissors_9": [ 0.170343131, 0.332957536, 0.767156839, 0.403594762 ],
    "scissors_10": [ 0.204656869, 0.120539248, 0.5245098, 0.743464053 ],
    "scissors_11": [ 0.05514706, 0.159754932, 0.799019635, 0.730392158 ],
    "scissors_12": [ 0.265931368, 0.169558853, 0.5061275, 0.606209159 ],
    "scissors_13": [ 0.241421565, 0.184264734, 0.448529422, 0.6830065 ],
    "scissors_14": [ 0.05759804, 0.05027781, 0.75, 0.882352948 ],
    "scissors_15": [ 0.191176474, 0.169558853, 0.6936275, 0.6748366 ],
    "scissors_16": [ 0.1004902, 0.279036, 0.6911765, 0.477124184 ],
    "scissors_17": [ 0.2720588, 0.131977156, 0.4987745, 0.6911765 ],
    "scissors_18": [ 0.180147052, 0.112369314, 0.6262255, 0.6666667 ],
    "scissors_19": [ 0.333333343, 0.0274019931, 0.443627447, 0.852941155 ],
    "scissors_20": [ 0.158088237, 0.04047389, 0.6691176, 0.843137264 ]
}

# Go through the data table above and create the images
print ("Adding images...")
tagged_images_with_regions = []

for file_name in fork_image_regions.keys():
    x,y,w,h = fork_image_regions[file_name]
    regions = [ Region(tag_id=fork_tag.id, left=x,top=y,width=w,height=h) ]

    with open("images/fork/" + file_name + ".jpg", mode="rb") as image_contents:
        tagged_images_with_regions.append(ImageFileCreateEntry(name=file_name, contents=image_contents.read(), regions=regions))

for file_name in scissors_image_regions.keys():
    x,y,w,h = scissors_image_regions[file_name]
    regions = [ Region(tag_id=scissors_tag.id, left=x,top=y,width=w,height=h) ]

    with open("images/scissors/" + file_name + ".jpg", mode="rb") as image_contents:
        tagged_images_with_regions.append(ImageFileCreateEntry(name=file_name, contents=image_contents.read(), regions=regions))


trainer.create_images_from_files(project.id, images=tagged_images_with_regions)
```

## <a name="step-5-train-the-project"></a>Stap 5: Het project trainen

Nu dat u kunt tags en afbeeldingen hebt toegevoegd aan het project, kunt u het kunt trainen: 

1. Voeg de volgende code. Hiermee maakt u de eerste iteratie in het project. 
2. Markeer deze herhaling als de standaard herhaling.

```Python
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

## <a name="step-6-get-and-use-the-default-prediction-endpoint"></a>Stap 6: Halen en gebruik het standaardeindpunt voorspelling

U bent nu klaar voor gebruik van het model voor de prognose: 

1. Het eindpunt dat is gekoppeld aan de standaard iteratie verkrijgen. 
2. Een testinstallatiekopie van de verzenden naar het project met dat eindpunt.

```Python
from azure.cognitiveservices.vision.customvision.prediction import prediction_endpoint
from azure.cognitiveservices.vision.customvision.prediction.prediction_endpoint import models

# Now there is a trained endpoint that can be used to make a prediction

predictor = prediction_endpoint.PredictionEndpoint(prediction_key)

# Open the sample image and get back the prediction results.
with open("images/test/test_image.jpg", mode="rb") as test_data:
    results = predictor.predict_image(project.id, test_data, iteration.id)

# Display the results.
for prediction in results.predictions:
    print ("\t" + prediction.tag_name + ": {0:.2f}%".format(prediction.probability * 100), prediction.bounding_box.left, prediction.bounding_box.top, prediction.bounding_box.width, prediction.bounding_box.height)
```

## <a name="step-7-run-the-example"></a>Stap 7: Het voorbeeld uitvoert

Voer de oplossing uit. De resultaten van de voorspelling weergegeven op de console.

```
python sample.py
```
