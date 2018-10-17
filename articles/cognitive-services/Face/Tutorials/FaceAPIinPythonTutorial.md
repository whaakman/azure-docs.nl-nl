---
title: 'Zelfstudie: Gezichten in een afbeelding detecteren en omlijsten - Face-API, Python'
titleSuffix: Azure Cognitive Services
description: Meer informatie over het gebruik van de Face-API met de Python-SDK voor het detecteren van menselijke gezichten in een afbeelding.
services: cognitive-services
author: SteveMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: face-api
ms.topic: tutorial
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 6cc3ac25d2196c0275b445503b79b9ac06a791d3
ms.sourcegitcommit: f10653b10c2ad745f446b54a31664b7d9f9253fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "46127734"
---
# <a name="tutorial-detect-and-frame-faces-with-the-face-api-and-python"></a>Zelfstudie: Gezichten in een afbeelding detecteren en omlijsten met de Face-API en Python 

In deze zelfstudie leert u om de Face-API aan te roepen via de Python-SDK voor het detecteren van menselijke gezichten in een afbeelding.

## <a name="prerequisites"></a>Vereisten

Als u de zelfstudie wilt gebruiken, moet u het volgende doen:

- Installeer Python 2.7 + of Python 3.5 +.
- Installeer pip.
- Installeer de Python-SDK voor de Face-API als volgt:

```bash
pip install cognitive_face
```

- Haal een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/) op voor Microsoft Cognitive Services. U kunt uw primaire of uw secundaire sleutel gebruiken in deze zelfstudie. (Houd er rekening mee dat voor het gebruik van Face-API's, u een geldig abonnementssleutel moet hebben.)

## <a name="detect-a-face-in-an-image"></a>Een gezicht detecteren in een afbeelding

```python
import cognitive_face as CF

KEY = '<Subscription Key>'  # Replace with a valid subscription key (keeping the quotes in place).
CF.Key.set(KEY)

BASE_URL = 'https://westus.api.cognitive.microsoft.com/face/v1.0/'  # Replace with your regional Base URL
CF.BaseUrl.set(BASE_URL)

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
faces = CF.face.detect(img_url)
print(faces)
```

Hieronder volgt een voorbeeld van resultaat. Het is een `list` van gedetecteerde gezichten. Elk item in de lijst is een `dict` instantie waar `faceId` een unieke ID is voor het gedetecteerde gezicht en `faceRectangle` beschrijft de positie van het gedetecteerde gezicht. Een gezichts-ID verloopt binnen 24 uur.

```python
[{u'faceId': u'68a0f8cf-9dba-4a25-afb3-f9cdf57cca51', u'faceRectangle': {u'width': 89, u'top': 66, u'height': 89, u'left': 446}}]
```

## <a name="draw-rectangles-around-the-faces"></a>Rechthoeken rond de gezichten tekenen

U kunt met behulp van de json-coördinaten die u hebt ontvangen van de vorige opdracht, rechthoeken tekenen op de afbeelding om elk gezicht visueel te vertegenwoordigen. U hebt `pip install Pillow` nodig om de `PIL` beeldmodule te gebruiken.  Voeg het volgende toe aan de bovenkant van het bestand:

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

Vervolgens, nadat `print(faces)`, voeg het volgende toe in uw script:

```python
#Convert width height to a point in a rectangle
def getRectangle(faceDictionary):
    rect = faceDictionary['faceRectangle']
    left = rect['left']
    top = rect['top']
    bottom = left + rect['height']
    right = top + rect['width']
    return ((left, top), (bottom, right))

#Download the image from the url
response = requests.get(img_url)
img = Image.open(BytesIO(response.content))

#For each face returned use the face rectangle and draw a red box.
draw = ImageDraw.Draw(img)
for face in faces:
    draw.rectangle(getRectangle(face), outline='red')

#Display the image in the users default image browser.
img.show()
```

## <a name="further-exploration"></a>Verder verkennen

Deze zelfstudie bevat een voorbeeld van een GUI om u verder te helpen de Face-API te verkennen. Installeer eerst [wxPython](https://wxpython.org/pages/downloads/) om het uit te voeren en voer vervolgens de onderstaande opdrachten uit.

```bash
git clone https://github.com/Microsoft/Cognitive-Face-Python.git
cd Cognitive-Face-Python
python sample
```

## <a name="summary"></a>Samenvatting

In deze zelfstudie hebt u het basisproces voor het gebruik van de Face-API via het aanroepen van de Python-SDK geleerd. Raadpleeg voor meer informatie over API-details, de instructies en [API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related-topics"></a>Verwante onderwerpen

- [Aan de slag met de Face-API in CSharp](FaceAPIinCSharpTutorial.md)
- [Aan de slag met de Face-API in Java voor Android](FaceAPIinJavaForAndroidTutorial.md)
