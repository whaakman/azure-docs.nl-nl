---
title: Face-API Python-zelfstudie | Microsoft Docs
titleSuffix: Microsoft Cognitive Services
description: Informatie over het gebruik van de Face-API met de SDK voor Python menselijke vlakken in een installatiekopie in cognitieve Services detecteren.
services: cognitive-services
author: SteveMSFT
manager: corncar
ms.service: cognitive-services
ms.component: face-api
ms.topic: article
ms.date: 03/01/2018
ms.author: sbowles
ms.openlocfilehash: 90d74d8df2ed59e6f3313ef7c620284d1022a667
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049108"
---
# <a name="getting-started-with-face-api-in-python-tutorial"></a>Aan de slag met Face-API in Python-zelfstudie

In deze zelfstudie leert u Python SDK voor het detecteren van menselijke vlakken in een installatiekopie van de Face-API aanroepen.

## <a name="prerequisites"></a> Vereisten

Als u wilt gebruiken de zelfstudie, moet u het volgende doen:

- Python 2.7 + of Python 3.5 + installeren.
- Pip installeren.
- Installeer de Python-SDK voor de Face-API als volgt:

```bash
pip install cognitive_face
```

- Verkrijgen van een [abonnementssleutel](https://azure.microsoft.com/try/cognitive-services/) voor cognitieve Microsoft-Services. U kunt uw primaire of uw secundaire sleutel gebruiken in deze zelfstudie. (Houd er rekening mee dat als u wilt een Face-API gebruikt, u moet een geldig abonnementssleutel hebben.)

## <a name="sdk-example"></a> Detecteren van een gezicht in een installatiekopie

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

Hieronder volgt een voorbeeld-resultaat. Het is een `list` van gedetecteerde vlakken. Elk item in de lijst een `dict` instantie waar `faceId` is een unieke ID voor de gedetecteerde face en `faceRectangle` beschrijft de positie van de gedetecteerde gezicht. De ID van een gezicht verloopt binnen 24 uur.

```python
[{u'faceId': u'68a0f8cf-9dba-4a25-afb3-f9cdf57cca51', u'faceRectangle': {u'width': 89, u'top': 66, u'height': 89, u'left': 446}}]
```

## <a name="draw-rectangles-around-the-faces"></a>Rechthoeken rond de vlakken tekenen

U kunt met behulp van de json-coördinaten die u hebt ontvangen van de vorige opdracht rechthoeken tekenen op de installatiekopie naar elke face visueel te vertegenwoordigen. U moet `pip install Pillow` gebruiken de `PIL` installatiekopieën module.  Voeg het volgende toe aan de bovenkant van het bestand:

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

Vervolgens, nadat `print(faces)`, het volgende opnemen in uw script:

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

## <a name='further'></a> Aanvullende informatie

Als u wilt, kunt u verder verkennen de Face-API biedt deze zelfstudie een voorbeeld van een GUI. Uit te voeren, eerst installeren [wxPython](https://wxpython.org/pages/downloads/) Voer de onderstaande opdrachten.

```bash
git clone https://github.com/Microsoft/Cognitive-Face-Python.git
cd Cognitive-Face-Python
python sample
```

## <a name="summary"></a> Samenvatting

In deze zelfstudie hebt u geleerd het basisproces voor het gebruik van de Face-API via de SDK voor Python wordt aangeroepen. Raadpleeg voor meer informatie over API-details de instructies en [API Reference](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).

## <a name="related"></a> Verwante onderwerpen

- [Aan de slag met Face-API in CSharp](FaceAPIinCSharpTutorial.md)
- [Aan de slag met Face-API in Java voor Android](FaceAPIinJavaForAndroidTutorial.md)
