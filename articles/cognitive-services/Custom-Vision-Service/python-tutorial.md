---
title: 'Quickstart: Een afbeeldingsclassificatieproject maken met de Custom Vision-SDK voor Python'
titlesuffix: Azure Cognitive Services
description: Maak een project, voeg tags toe, upload afbeeldingen, train uw project en doe een voorspelling met behulp van de Python-SDK.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: areddish
ms.openlocfilehash: 47e2f2a03c08ae1e44dcba35b440880ce06f6f95
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484458"
---
# <a name="quickstart-create-an-image-classification-project-with-the-custom-vision-python-sdk"></a>Quickstart: Een afbeeldingsclassificatieproject maken met de Custom Vision-SDK met Python

Dit artikel biedt informatie en voorbeeldcode om u te helpen om met de Custom Vision-SDK met Python een afbeeldingsclassificatiemodel te maken. Nadat deze gemaakt, kunt u labels toevoegen, afbeeldingen uploaden, trainen van het project, verkrijgen van het project gepubliceerde voorspelling eindpunt-URL en het eindpunt voor het testen van een installatiekopie van een via een programma gebruiken. Gebruik dit voorbeeld als een sjabloon om uw eigen Python-toepassing te maken. Zie de [handleiding voor browsers](getting-started-build-a-classifier.md) als u het ontwikkelproces wilt doorlopen en een classificatiemodel wilt gebruiken _zonder_ code.

## <a name="prerequisites"></a>Vereisten

- [Python 2.7+ of 3.5+](https://www.python.org/downloads/)
- [pip](https://pip.pypa.io/en/stable/installing/)-hulpprogramma

## <a name="install-the-custom-vision-sdk"></a>De Custom Vision-SDK installeren

Als u de Custom Vision Service-SDK voor Python wilt installeren, voert u de volgende opdracht uit in PowerShell:

```powershell
pip install azure-cognitiveservices-vision-customvision
```

[!INCLUDE [get-keys](includes/get-keys.md)]

[!INCLUDE [python-get-images](includes/python-get-images.md)]


## <a name="add-the-code"></a>Code toevoegen

Maak een nieuw bestand met de naam *sample.py* in uw projectmap.

### <a name="create-the-custom-vision-service-project"></a>Het Custom Vision Service-project maken

Als u een nieuw Custom Vision Service-project wilt maken, voegt u de volgende code aan uw script toe. Voeg uw abonnementssleutels in de juiste definities in.

```Python
from azure.cognitiveservices.vision.customvision.training import CustomVisionTrainingClient
from azure.cognitiveservices.vision.customvision.training.models import ImageFileCreateEntry

ENDPOINT = "https://southcentralus.api.cognitive.microsoft.com"

# Replace with a valid key
training_key = "<your training key>"
prediction_key = "<your prediction key>"
prediction_resource_id = "<your prediction resource id>"

publish_iteration_name = "classifyModel"

trainer = CustomVisionTrainingClient(training_key, endpoint=ENDPOINT)

# Create a new project
print ("Creating project...")
project = trainer.create_project("My New Project")
```

### <a name="create-tags-in-the-project"></a>Labels maken in het project

Voeg de volgende code toe aan het eind van *sample.py* om classificatielabels voor uw project te maken:

```Python
# Make two tags in the new project
hemlock_tag = trainer.create_tag(project.id, "Hemlock")
cherry_tag = trainer.create_tag(project.id, "Japanese Cherry")
```

### <a name="upload-and-tag-images"></a>Afbeeldingen uploaden en labelen

Als u de voorbeeldafbeeldingen aan het project wilt toevoegen, voegt u de volgende code in nadat u de tag hebt gemaakt. Met deze code wordt elke afbeelding met de bijbehorende tag geüpload. U moet het basispad naar de afbeeldings-URL invoeren, gebaseerd op waar u het project Cognitive Services Python-SDK-voorbeelden hebt gedownload.

> [!NOTE]
> U moet het pad naar de afbeeldingen wijzigen, gebaseerd op waar u het project Cognitive Services Python-SDK-voorbeelden eerder hebt gedownload.

```Python
base_image_url = "<path to project>"

print("Adding images...")

image_list = []

for image_num in range(1, 11):
    file_name = "hemlock_{}.jpg".format(image_num)
    with open(base_image_url + "images/Hemlock/" + file_name, "rb") as image_contents:
        image_list.append(ImageFileCreateEntry(name=file_name, contents=image_contents.read(), tag_ids=[hemlock_tag.id]))

for image_num in range(1, 11):
    file_name = "japanese_cherry_{}.jpg".format(image_num)
    with open(base_image_url + "images/Japanese Cherry/" + file_name, "rb") as image_contents:
        image_list.append(ImageFileCreateEntry(name=file_name, contents=image_contents.read(), tag_ids=[cherry_tag.id]))

upload_result = trainer.create_images_from_files(project.id, images=image_list)
if not upload_result.is_batch_successful:
    print("Image batch upload failed.")
    for image in upload_result.images:
        print("Image status: ", image.status)
    exit(-1)
```

### <a name="train-the-classifier-and-publish"></a>De classificatie te trainen en publiceren

Deze code wordt de eerste versie in het project wordt gemaakt en vervolgens publiceert dat iteratie naar het eindpunt van de voorspelling. De naam van de gepubliceerde iteratie kan worden gebruikt om voorspelling aanvragen te verzenden. Een iteratie is niet beschikbaar in de voorspelling eindpunt totdat deze is gepubliceerd.

```Python
import time

print ("Training...")
iteration = trainer.train_project(project.id)
while (iteration.status != "Completed"):
    iteration = trainer.get_iteration(project.id, iteration.id)
    print ("Training status: " + iteration.status)
    time.sleep(1)

# The iteration is now trained. Publish it to the project endpoint
trainer.publish_iteration(project.id, iteration.id, publish_iteration_name, prediction_resource_id)
print ("Done!")
```

### <a name="get-and-use-the-published-iteration-on-the-prediction-endpoint"></a>Halen en gebruik de gepubliceerde iteratie op het eindpunt voor de voorspelling

Als u een afbeelding naar het voorspellingseindpunt wilt verzenden en de voorspelling wilt ophalen, voegt u de volgende code toe aan het einde van het bestand:

```python
from azure.cognitiveservices.vision.customvision.prediction import CustomVisionPredictionClient

# Now there is a trained endpoint that can be used to make a prediction
predictor = CustomVisionPredictionClient(prediction_key, endpoint=ENDPOINT)

with open(base_image_url + "images/Test/test_image.jpg", "rb") as image_contents:
    results = predictor.classify_image(project.id, publish_iteration_name, image_contents.read())

    # Display the results.
    for prediction in results.predictions:
        print ("\t" + prediction.tag_name + ": {0:.2f}%".format(prediction.probability * 100))
```

## <a name="run-the-application"></a>De toepassing uitvoeren

Voer *sample.py* uit.

```powershell
python sample.py
```

Als het goed is, is de uitvoer van de toepassing vergelijkbaar met de volgende tekst:

```console
Creating project...
Adding images...
Training...
Training status: Training
Training status: Completed
Done!
        Hemlock: 93.53%
        Japanese Cherry: 0.01%
```

U kunt vervolgens controleren of de testafbeelding (in **<base_image_url>/Images/Test/**) juist is getagd. U kunt altijd teruggaan naar de [Custom Vision-website](https://customvision.ai) en de huidige status bekijken van het nieuwe project dat u hebt gemaakt.

[!INCLUDE [clean-ic-project](includes/clean-ic-project.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt nu gezien hoe elke stap van het afbeeldingsclassificatieproces in code kan worden uitgevoerd. Met dit voorbeeld wordt één trainingsinteratie uitgevoerd, maar vaak zult u uw model meerdere keren willen trainen en testen om het nauwkeuriger te maken.

> [!div class="nextstepaction"]
> [Een model testen en opnieuw trainen](test-your-model.md)
