---
title: 'Snelstartgids: Gezichten in een afbeelding detecteren en omlijsten met de Python-SDK'
titleSuffix: Azure Cognitive Services
description: In deze Quick Start maakt u een python-script dat gebruikmaakt van de Face-API om gezichten in een externe installatie kopie te detecteren en te frame.
services: cognitive-services
author: SteveMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 08/08/2019
ms.author: sbowles
ms.openlocfilehash: 502523e3c62f993af4484c8ab922c36fa51a60f1
ms.sourcegitcommit: 124c3112b94c951535e0be20a751150b79289594
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/10/2019
ms.locfileid: "68945897"
---
# <a name="quickstart-create-a-python-script-to-detect-and-frame-faces-in-an-image"></a>Quickstart: Een Python-script maken om gezichten in een afbeelding te herkennen en te omlijsten

In deze Quick Start maakt u een python-script dat gebruikmaakt van Azure Face-API, met behulp van de python-SDK, om menselijke gezichten in een externe installatie kopie te detecteren. Er wordt een geselecteerde afbeelding weergegeven en een kader rond elk gedetecteerd gezicht getekend.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="prerequisites"></a>Vereisten

- Een Face-API-abonnementssleutel. U kunt een abonnementssleutel voor een gratis proefversie downloaden van [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=face-api). Of volg de instructies in [Een Cognitive Services-account maken](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) om u te abonneren op de Face-API-service en uw sleutel op te halen.
- [Python 2.7+ of 3.5+](https://www.python.org/downloads/)
- [pip](https://pip.pypa.io/en/stable/installing/)-hulpprogramma

## <a name="get-the-face-sdk"></a>De Face-SDK ophalen

Installeer de Python-SDK voor Face door de opdrachtprompt te openen en de volgende opdracht uit te voeren:

```shell
pip install azure-cognitiveservices-vision-face
```

## <a name="detect-faces-in-an-image"></a>Gezichten in een afbeelding detecteren

Maak een nieuw Python-script met de naam _FaceQuickstart.py_ en voeg de volgende code toe. Deze code verwerkt de kern functionaliteit van gezichts detectie. U dient `<Subscription Key>` te vervangen door de waarde van de sleutel. Mogelijk moet u ook de waarde van `BASE_URL` wijzigen om de juiste regio-id voor uw sleutel te gebruiken (zie de [documentatie bij de Face-API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) voor een lijst van alle regio-eindpunten). Abonnementssleutels voor een gratis proefversie worden gegenereerd in de regio **westus**. U kunt `img_url` eventueel instellen op een afbeelding die u wilt gebruiken.

Het script detecteert gezichten door de methode **FaceClient. Face. detect_with_url** aan te roepen, waarmee de [detectie](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) rest API wordt ingepakt en een lijst met gezichten wordt geretourneerd.

```python
from azure.cognitiveservices.vision.face import FaceClient

# Replace with a valid subscription key (keeping the quotes in place).
KEY = '<Subscription Key>'

# Replace with your regional Base URL
BASE_URL = 'https://westus.api.cognitive.microsoft.com/'

faceClient = FaceClient(BASE_URL, CognitiveServicesCredentials(KEY))

# You can use this example JPG or replace the URL below with your own URL to a JPEG image.
img_url = 'https://raw.githubusercontent.com/Microsoft/Cognitive-Face-Windows/master/Data/detection1.jpg'
faces = faceClient.face.detect_with_url(img_url)
print(faces)
```

### <a name="try-the-app"></a>De app uitproberen

Voer de app uit met de opdracht `python FaceQuickstart.py`. In het consolevenster moet een tekst als antwoord verschijnen, bijvoorbeeld:

```console
[{'faceId': '26d8face-9714-4f3e-bfa1-f19a7a7aa240', 'faceRectangle': {'top': 124, 'left': 459, 'width': 227, 'height': 227}}]
```

De uitvoer vertegenwoordigt een lijst met gedetecteerde gezichten. Elk item in de lijst is een **dict**-instantie waar `faceId` een unieke id is voor het gedetecteerde gezicht en `faceRectangle` de positie van het gedetecteerde gezicht beschrijft. 

> [!NOTE]
> Gezichts-id's verlopen na 24 uur. U dient gezichtsgegevens expliciet op te slaan als u ze voor langere tijd wilt bewaren.

## <a name="draw-face-rectangles"></a>Gezichtsrechthoeken tekenen

U kunt met behulp van de coördinaten die u hebt ontvangen van de vorige opdracht, rechthoeken tekenen op de afbeelding om elk gezicht visueel te representeren. U dient Pillow (`pip install pillow`) te installeren als u de Pillow-afbeeldingsmodule wilt gebruiken. Voeg boven aan *FaceQuickstart.py* de volgende code toe:

```python
import requests
from io import BytesIO
from PIL import Image, ImageDraw
```

Voeg vervolgens onder aan het script de volgende code toe. Deze code maakt een eenvoudige functie voor het parseren van de rechthoek coördinaten en gebruikt Pillow om rechthoeken te tekenen op de oorspronkelijke afbeelding. Vervolgens wordt de afbeelding in de standaardafbeeldingsviewer weergegeven.

```python
# Convert width height to a point in a rectangle


def getRectangle(faceDictionary):
    rect = faceDictionary['faceRectangle']
    left = rect['left']
    top = rect['top']
    bottom = left + rect['height']
    right = top + rect['width']
    return ((left, top), (bottom, right))


# Download the image from the url
response = requests.get(img_url)
img = Image.open(BytesIO(response.content))

# For each face returned use the face rectangle and draw a red box.
draw = ImageDraw.Draw(img)
for face in faces:
    draw.rectangle(getRectangle(face), outline='red')

# Display the image in the users default image browser.
img.show()
```

## <a name="run-the-app"></a>De app uitvoeren

Mogelijk wordt u gevraagd eerst een standaardafbeeldingsviewer te selecteren. Vervolgens moet een afbeelding worden weergegeven die lijkt op de volgende. In het consolevenster moeten ook de gegevens van de rechthoek te zien zijn.

![Een jonge vrouw met een rode rechthoek rond haar gezicht](../images/face-rectangle-result.png)

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u het basisproces geleerd voor het gebruik van de Python SDK van de Face-API en een script gemaakt om gezichten in een afbeelding te detecteren en omlijsten. Vervolgens gaat u het gebruik van de Python SDK in een complexer voorbeeld bestuderen. Ga naar het Cognitive Face Python-voorbeeld op GitHub, kloneer het naar uw projectmap en volg de aanwijzingen in het bestand _README.md_ .

> [!div class="nextstepaction"]
> [Cognitive Face Python-voorbeeld](https://github.com/Microsoft/Cognitive-Face-Python)
