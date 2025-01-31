---
title: 'Quickstart: Een miniatuur maken - REST, Python'
titleSuffix: Azure Cognitive Services
description: In deze snelstart maakt u een miniatuur van een afbeelding met behulp van de Computer Vision-API en Python.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 07/03/2019
ms.author: pafarley
ms.custom: seodec18
ms.openlocfilehash: 5203467eadadf1180eabf7b5a51b113ae273ee7c
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67603538"
---
# <a name="quickstart-generate-a-thumbnail-using-the-computer-vision-rest-api-and-python"></a>Quickstart: Genereer een miniatuur met behulp van de Computer Vision-API voor REST en Python

In deze quickstart maakt u een miniatuur van een afbeelding met behulp van de REST API van Computer Vision. Met de methode [Get Thumbnail](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fb) kunt u de gewenste hoogte en breedte opgeven. Computer Vision maakt gebruik van slim bijsnijden om op intelligente wijze het interessegebied te bepalen en coördinaten voor het bijsnijden te genereren op basis van dat gebied.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/try/cognitive-services/) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

- U moet beschikken over een abonnementssleutel voor Computer Vision. U krijgt een gratis proefversie sleutel van [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=computer-vision). Of, volg de instructies in [een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) abonneren op de Computer Vision en haal uw sleutel.
- Een code-editor zoals [Visual Studio Code](https://code.visualstudio.com/download)

## <a name="create-and-run-the-sample"></a>Het voorbeeld maken en uitvoeren

Als u het voorbeeld wilt maken en uitvoeren, kopieert u de volgende code naar de code-editor. 

```python
import requests
# If you are using a Jupyter notebook, uncomment the following line.
# %matplotlib inline
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
# Free trial subscription keys are generated in the "westcentralus" region.
# If you use a free trial subscription key, you shouldn't need to change
# this region.
vision_base_url = "https://westcentralus.api.cognitive.microsoft.com/vision/v2.0/"

thumbnail_url = vision_base_url + "generateThumbnail"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/9/94/Bloodhound_Puppy.jpg"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params = {'width': '50', 'height': '50', 'smartCropping': 'true'}
data = {'url': image_url}
response = requests.post(thumbnail_url, headers=headers,
                         params=params, json=data)
response.raise_for_status()

thumbnail = Image.open(BytesIO(response.content))

# Display the thumbnail.
plt.imshow(thumbnail)
plt.axis("off")

# Verify the thumbnail size.
print("Thumbnail is {0}-by-{1}".format(*thumbnail.size))
```

Ga daarna als volgt te werk:
1. Vervang de waarde van `subscription_key` door uw abonnementssleutel.
1. Vervang de waarde van `vision_base_url` door de eindpunt-URL voor de Computer Vision-bron in de Azure-regio waar u uw abonnementssleutels hebt verkregen (indien nodig).
1. Vervang optioneel de waarde van `image_url` door de URL van een andere afbeelding waar u een miniatuur van wilt maken.
1. Sla de code op als een bestand met de extensie `.py`. Bijvoorbeeld `get-thumbnail.py`.
1. Open een opdrachtpromptvenster.
1. Typ bij de prompt de opdracht `python` om het voorbeeld uit te voeren. Bijvoorbeeld `python get-thumbnail.py`.

## <a name="examine-the-response"></a>Het antwoord bekijken

Een geslaagd antwoord wordt geretourneerd als binaire gegevens, die staan voor de afbeeldingsgegevens van de miniatuur. Deze afbeelding moet worden weergegeven in het voorbeeld. Als de aanvraag mislukt, wordt het antwoord weergegeven in het opdrachtpromptvenster en bevat het antwoord een foutcode.

## <a name="run-in-jupyter-optional"></a>Uitvoeren in Jupyter-(optioneel)

U kunt deze quickstart desgewenst stapsgewijs uitvoeren met behulp van een Jupyter Notebook op [MyBinder](https://mybinder.org). Selecteer de volgende knop om Binder te starten:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="next-steps"></a>Volgende stappen

Hieronder vindt u meer gedetailleerde informatie over de functie voor het genereren van miniaturen.

> [!div class="nextstepaction"]
> [Miniaturen genereren](../concept-generating-thumbnails.md)
