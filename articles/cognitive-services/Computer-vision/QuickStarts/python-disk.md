---
title: Snelstart voor het analyseren van een lokale afbeelding met behulp van Computer Vision met Python | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: In deze snelstart analyseert u een lokale afbeelding met behulp van Computer Vision met Python in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: a1f3fce5a547f143f7c4884c6642e78f53d160e9
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770096"
---
# <a name="quickstart-analyze-a-local-image---rest-python"></a>Snelstart: Een lokale afbeelding analyseren - REST, Python

In deze snelstart analyseert u een lokale afbeelding met behulp van Computer Vision. Zie voor het analyseren van een externe afbeelding [Een externe afbeelding analyseren met Python](python-analyze.md).

U kunt deze snelstart stapsgewijs uitvoeren met behulp van een Jupyter-notitieblok op [MyBinder](https://mybinder.org). Selecteer de volgende knop om Binder te starten:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="prerequisites"></a>Vereisten

Als u Computer Vision wilt gebruiken, moet u een abonnementssleutel hebben. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="analyze-a-local-image"></a>Een lokale afbeelding analyseren

Dit voorbeeld is vergelijkbaar met [Een externe afbeelding analyseren met Python ](python-analyze.md), behalve dat de te analyseren afbeelding lokaal wordt gelezen vanaf schijf. Er zijn twee wijzigingen nodig:

- Voeg een `{"Content-Type": "application/octet-stream"}`-header toe aan de aanvraag.
- Voeg de afbeeldingsgegevens (bytematrix) toe aan de hoofdtekst van de aanvraag.

U kunt het voorbeeld uitvoeren aan de hand van de volgende stappen:

1. Kopieer de volgende code naar een nieuw Python-scriptbestand.
1. Vervang `<Subscription Key>` door uw geldige abonnementssleutel.
1. Wijzig indien nodig de `vision_base_url`-waarde in de locatie waar u uw abonnementssleutels hebt verkregen.
1. Wijzig de waarde `image_path` in het pad van een lokale afbeelding.
1. Voer het script uit.

In de volgende code wordt de Python `requests`-bibliotheek gebruikt om de Computer Vision Analyze Image API aan te roepen. De resultaten worden geretourneerd als JSON-object. De API-sleutel wordt doorgegeven via de `headers`-woordenlijst. De typen te herkennen kenmerken worden doorgegeven via de `params`-woordenlijst. De binaire gegevens worden aan `requests.post`doorgegeven via de parameter `data`.

## <a name="analyze-image-request"></a>Analyze Image-aanvraag

```python
import requests
# If you are using a Jupyter notebook, uncomment the following line.
#%matplotlib inline
import matplotlib.pyplot as plt
from PIL import Image
from io import BytesIO

# Replace <Subscription Key> with your valid subscription key.
subscription_key = "<Subscription Key>"
assert subscription_key

# You must use the same region in your REST call as you used to get your
# subscription keys. For example, if you got your subscription keys from
# westus, replace "westcentralus" in the URI below with "westus".
#
# Free trial subscription keys are generated in the westcentralus region.
# If you use a free trial subscription key, you shouldn't need to change
# this region.
vision_base_url = "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/"

analyze_url = vision_base_url + "analyze"

# Set image_path to the local path of an image that you want to analyze.
image_path = "C:/Documents/ImageToAnalyze.jpg"

# Read the image into a byte array
image_data = open(image_path, "rb").read()
headers    = {'Ocp-Apim-Subscription-Key': subscription_key,
              'Content-Type': 'application/octet-stream'}
params     = {'visualFeatures': 'Categories,Description,Color'}
response = requests.post(
    analyze_url, headers=headers, params=params, data=image_data)
response.raise_for_status()

# The 'analysis' object contains various fields that describe the image. The most
# relevant caption for the image is obtained from the 'description' property.
analysis = response.json()
print(analysis)
image_caption = analysis["description"]["captions"][0]["text"].capitalize()

# Display the image and overlay it with the caption.
image = Image.open(BytesIO(image_data))
plt.imshow(image)
plt.axis("off")
_ = plt.title(image_caption, size="x-large", y=-0.1)
```

## <a name="analyze-image-response"></a>Analyze Image-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, bijvoorbeeld:

```json
{
  "categories": [
    {
      "name": "outdoor_",
      "score": 0.00390625,
      "detail": {
        "landmarks": []
      }
    },
    {
      "name": "outdoor_street",
      "score": 0.33984375,
      "detail": {
        "landmarks": []
      }
    }
  ],
  "description": {
    "tags": [
      "building",
      "outdoor",
      "street",
      "city",
      "people",
      "busy",
      "table",
      "walking",
      "traffic",
      "filled",
      "large",
      "many",
      "group",
      "night",
      "light",
      "crowded",
      "bunch",
      "standing",
      "man",
      "sign",
      "crowd",
      "umbrella",
      "riding",
      "tall",
      "woman",
      "bus"
    ],
    "captions": [
      {
        "text": "a group of people on a city street at night",
        "confidence": 0.9122243847383961
      }
    ]
  },
  "color": {
    "dominantColorForeground": "Brown",
    "dominantColorBackground": "Brown",
    "dominantColors": [
      "Brown"
    ],
    "accentColor": "B54316",
    "isBwImg": false
  },
  "requestId": "c11894eb-de3e-451b-9257-7c8b168073d1",
  "metadata": {
    "height": 600,
    "width": 450,
    "format": "Jpeg"
  }
}
```

## <a name="next-steps"></a>Volgende stappen

Een Python-toepassing verkennen die Computer Vision gebruikt om optische tekenherkenning (OCR) uit te voeren; slim bijgesneden miniaturen maken; plus visuele kenmerken, inclusief gezichten, in een afbeelding detecteren, categoriseren, labelen en beschrijven. Als u snel wilt experimenteren met de Computer Vision-API's, probeert u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Zelfstudie voor de Computer Vision-API met Python](../Tutorials/PythonTutorial.md)
