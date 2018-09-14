---
title: Snelstart voor OCR met behulp van de Computer Vision-API met Python | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: In deze snelstart extraheert u afgedrukte tekst uit een afbeelding met behulp van Computer Vision met Python in Cognitive Services.
services: cognitive-services
author: noellelacharite
manager: nolachar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: v-deken
ms.openlocfilehash: 60d0435083bdb38b3aee4ff8a8f022e76d06407f
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43770091"
---
# <a name="quickstart-extract-printed-text-ocr---rest-python"></a>Snelstart: Gedrukte tekst extraheren (OCR) - REST, Python

In deze snelstart gebruikt u Computer Vision om afgedrukte tekst uit een afbeelding te extraheren. Dit wordt optische tekenherkenning of OCR genoemd (optical character recognition).

U kunt deze snelstart stapsgewijs uitvoeren met behulp van een Jupyter-notitieblok op [MyBinder](https://mybinder.org). Selecteer de volgende knop om Binder te starten:

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=VisionAPI.ipynb)

## <a name="prerequisites"></a>Vereisten

Als u Computer Vision wilt gebruiken, moet u een abonnementssleutel hebben. Zie [Abonnementssleutels verkrijgen](../Vision-API-How-to-Topics/HowToSubscribe.md).

## <a name="extract-printed-text"></a>Afgedrukte tekst extraheren

Met de [OCR-methode](https://westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fc) kunt u tekst in een afbeelding detecteren en de herkende tekens naar een machinaal leesbare tekenstroom extraheren.

U kunt het voorbeeld uitvoeren aan de hand van de volgende stappen:

1. Kopieer de volgende code naar een nieuw Python-scriptbestand.
1. Vervang `<Subscription Key>` door uw geldige abonnementssleutel.
1. Wijzig indien nodig de `vision_base_url`-waarde in de locatie waar u uw abonnementssleutels hebt verkregen.
1. Wijzig eventueel de waarde `image_url` in een andere afbeelding.
1. Voer het script uit.

In de volgende code wordt de Python `requests`-bibliotheek gebruikt om de Computer Vision Analyze Image API aan te roepen. De resultaten worden geretourneerd als JSON-object. De API-sleutel wordt doorgegeven via de `headers`-woordenlijst.

## <a name="ocr-request"></a>OCR-aanvraag

```python
import requests
# If you are using a Jupyter notebook, uncomment the following line.
#%matplotlib inline
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle
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

ocr_url = vision_base_url + "ocr"

# Set image_url to the URL of an image that you want to analyze.
image_url = "https://upload.wikimedia.org/wikipedia/commons/thumb/a/af/" + \
    "Atomist_quote_from_Democritus.png/338px-Atomist_quote_from_Democritus.png"

headers = {'Ocp-Apim-Subscription-Key': subscription_key}
params  = {'language': 'unk', 'detectOrientation': 'true'}
data    = {'url': image_url}
response = requests.post(ocr_url, headers=headers, params=params, json=data)
response.raise_for_status()

analysis = response.json()

# Extract the word bounding boxes and text.
line_infos = [region["lines"] for region in analysis["regions"]]
word_infos = []
for line in line_infos:
    for word_metadata in line:
        for word_info in word_metadata["words"]:
            word_infos.append(word_info)
word_infos

# Display the image and overlay it with the extracted text.
plt.figure(figsize=(5, 5))
image = Image.open(BytesIO(requests.get(image_url).content))
ax = plt.imshow(image, alpha=0.5)
for word in word_infos:
    bbox = [int(num) for num in word["boundingBox"].split(",")]
    text = word["text"]
    origin = (bbox[0], bbox[1])
    patch  = Rectangle(origin, bbox[2], bbox[3], fill=False, linewidth=2, color='y')
    ax.axes.add_patch(patch)
    plt.text(origin[0], origin[1], text, fontsize=20, weight="bold", va="top")
plt.axis("off")
```

## <a name="ocr-response"></a>OCR-antwoord

Een geslaagd antwoord wordt geretourneerd in de JSON-indeling, bijvoorbeeld:

```json
{
  "language": "en",
  "orientation": "Up",
  "textAngle": 0,
  "regions": [
    {
      "boundingBox": "21,16,304,451",
      "lines": [
        {
          "boundingBox": "28,16,288,41",
          "words": [
            {
              "boundingBox": "28,16,288,41",
              "text": "NOTHING"
            }
          ]
        },
        {
          "boundingBox": "27,66,283,52",
          "words": [
            {
              "boundingBox": "27,66,283,52",
              "text": "EXISTS"
            }
          ]
        },
        {
          "boundingBox": "27,128,292,49",
          "words": [
            {
              "boundingBox": "27,128,292,49",
              "text": "EXCEPT"
            }
          ]
        },
        {
          "boundingBox": "24,188,292,54",
          "words": [
            {
              "boundingBox": "24,188,292,54",
              "text": "ATOMS"
            }
          ]
        },
        {
          "boundingBox": "22,253,297,32",
          "words": [
            {
              "boundingBox": "22,253,105,32",
              "text": "AND"
            },
            {
              "boundingBox": "144,253,175,32",
              "text": "EMPTY"
            }
          ]
        },
        {
          "boundingBox": "21,298,304,60",
          "words": [
            {
              "boundingBox": "21,298,304,60",
              "text": "SPACE."
            }
          ]
        },
        {
          "boundingBox": "26,387,294,37",
          "words": [
            {
              "boundingBox": "26,387,210,37",
              "text": "Everything"
            },
            {
              "boundingBox": "249,389,71,27",
              "text": "else"
            }
          ]
        },
        {
          "boundingBox": "127,431,198,36",
          "words": [
            {
              "boundingBox": "127,431,31,29",
              "text": "is"
            },
            {
              "boundingBox": "172,431,153,36",
              "text": "opinion."
            }
          ]
        }
      ]
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen

Een Python-toepassing verkennen die Computer Vision gebruikt om optische tekenherkenning (OCR) uit te voeren; slim bijgesneden miniaturen maken; plus visuele kenmerken, inclusief gezichten, in een afbeelding detecteren, categoriseren, labelen en beschrijven. Als u snel wilt experimenteren met de Computer Vision-API's, probeert u de [Open API-testconsole](https://westcentralus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa/console).

> [!div class="nextstepaction"]
> [Zelfstudie voor de Computer Vision-API met Python](../Tutorials/PythonTutorial.md)
