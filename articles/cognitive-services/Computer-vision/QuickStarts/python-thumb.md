---
title: Snelstart voor het maken van een miniatuur in Computer Vision met Python | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: In deze snelstart maakt u een miniatuur van een afbeelding met behulp van Computer Vision met Python in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: bc1d01cd4b8f15ba627d917825ee5205c34f5cdf
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770211"
---
# <a name="quickstart-generate-a-thumbnail---rest-python"></a>Snelstart: een miniatuur maken - REST, Python

In deze snelstart maakt u een miniatuur van een afbeelding met behulp van Computer Vision.

U kunt deze snelstart stapsgewijs uitvoeren met behulp van een Jupyter-notitieblok op [MyBinder](https://mybinder.org). Selecteer de volgende knop om Binder te starten:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="prerequisites"></a>Vereisten

Als u Computer Vision wilt gebruiken, moet u een abonnementssleutel hebben. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="intelligently-generate-a-thumbnail"></a>Intelligent een miniatuur genereren

Met de methode [Get Thumbnail](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) (miniatuur ophalen) kunt u een miniatuur van een afbeelding genereren. U geeft de hoogte en breedte op. Deze kan afwijken van de hoogte-breedteverhouding van de ingevoerde afbeelding. Computer Vision maakt gebruik van slim bijsnijden om op intelligente wijze het belangrijkste gebied te bepalen en coördinaten voor het bijsnijden te genereren op basis van dat gebied.

U kunt het voorbeeld uitvoeren aan de hand van de volgende stappen:

1. Kopieer de volgende code naar een nieuw Python-scriptbestand.
1. Vervang `<Subscription Key>` door uw geldige abonnementssleutel.
1. Wijzig indien nodig de `vision_base_url`-waarde in de locatie waar u uw abonnementssleutels hebt verkregen.
1. Wijzig eventueel de waarde `image_url` in een andere afbeelding.
1. Voer het script uit.

In de volgende code wordt de Python `requests`-bibliotheek gebruikt om de Computer Vision Analyze Image API aan te roepen. De API-sleutel wordt doorgegeven via de `headers`-woordenlijst. De grootte van de miniatuur wordt doorgegeven via de `params`-woordenlijst. De miniatuur wordt in het antwoord geretourneerd als een bytematrix.

## <a name="get-thumbnail-request"></a>Get Thumbnail-aanvraag

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

thumbnail_url = vision_base_url + "generateThumbnail"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params  = {'width': '50', 'height': '50', 'smartCropping': 'true'}
data    = {'url': image_url}
response = requests.post(thumbnail_url, headers=headers, params=params, json=data)
response.raise_for_status()

thumbnail = Image.open(BytesIO(response.content))

# Display the thumbnail.
plt.imshow(thumbnail)
plt.axis("off")

# Verify the thumbnail size.
print("Thumbnail is {0}-by-{1}".format(*thumbnail.size))
```

## <a name="next-steps"></a>Volgende stappen

Een Python-toepassing verkennen die Computer Vision gebruikt om optische tekenherkenning (OCR) uit te voeren; slim bijgesneden miniaturen maken; plus visuele kenmerken, inclusief gezichten, in een afbeelding detecteren, categoriseren, labelen en beschrijven. Als u snel wilt experimenteren met de Computer Vision-API's, probeert u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Zelfstudie voor de Computer Vision-API met Python](../Tutorials/PythonTutorial.md)
