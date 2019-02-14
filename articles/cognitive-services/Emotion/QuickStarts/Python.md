---
title: 'Quickstart: Emoties herkennen op gezichten in een afbeelding - Emotion-API, Python'
description: Bekijk informatie en codevoorbeelden om snel aan de slag te gaan met de Emotion-API en Python.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: quickstart
ms.date: 02/05/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: bd267d40fe1f5a673f1d6b98b301665690b12f31
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56233469"
---
# <a name="quickstart-build-an-app-to-recognize-emotions-on-faces-in-an-image"></a>Quickstart: Een app bouwen voor het herkennen van emoties op een gezicht in een afbeelding.

> [!IMPORTANT]
> De Emotion-API wordt op 15 februari 2019 afgeschaft. De mogelijkheid voor de herkenning van emoties is nu algemeen beschikbaar als onderdeel van de [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/). 

In deze snelstartgids vindt u informatie en codevoorbeelden om met behulp van Python en de [methode Recognize van de Emotion-API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) de emoties te herkennen die worden uitgedrukt door een of meer personen in een afbeelding.

U kunt dit voorbeeld uitvoeren als een Jupyter-notebook op [MyBinder](https://mybinder.org) door te klikken op de badge launch binder: ![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=EmotionAPI.ipynb)


## <a name="prerequisite"></a>Vereiste
U kunt [hier](https://azure.microsoft.com/try/cognitive-services/) een gratis abonnementssleutel ophalen

## <a name="running-the-walkthrough"></a>De voorbeeldtoepassing uitvoeren
Als u wilt doorgaan met deze gids, moet u `subscription_key` vervangen door een geldige API-sleutel die u eerder hebt opgehaald.


```python
subscription_key = None
assert subscription_key
```

Vervolgens controleert u of de service-URL overeenkomt met de regio die u hebt gebruikt bij het instellen van de API-sleutel. Als u de sleutel van een proefversie gebruikt, hoeft u niets te wijzigen.


```python
emotion_recognition_url = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
```

In deze gids maakt u gebruik van afbeeldingen die op schijf zijn opgeslagen. U kunt ook afbeeldingen gebruiken die beschikbaar zijn via een openbaar toegankelijke URL. Zie de [documentatie van de REST-API](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) voor meer informatie.

Omdat de afbeeldingsgegevens worden doorgegeven als onderdeel van de hoofdtekst van de aanvraag, moet u de header `Content-Type` instellen op `application/octet-stream`. Als u een afbeelding doorgeeft via een URL, vergeet dan niet om de header in te stellen op:
```python
header = {'Ocp-Apim-Subscription-Key': subscription_key }
```
Maak een woordenlijst die de URL bevat:
```python
data = {'url': image_url}
```
en geef die door aan de bibliotheek `requests` met behulp van:
```python
requests.post(emotion_recognition_url, headers=headers, json=image_data)
```

Download eerst een paar voorbeeldafbeeldingen van de site van de [Emotion-API](https://azure.microsoft.com/services/cognitive-services/emotion/).


```bash
%%bash
mkdir -p images
curl -Ls https://aka.ms/csnb-emotion-1 -o images/emotion_1.jpg
curl -Ls https://aka.ms/csnb-emotion-2 -o images/emotion_2.jpg
```


```python
image_path = "images/emotion_1.jpg"
image_data = open(image_path, "rb").read()
```


```python
import requests
headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
response.raise_for_status()
analysis = response.json()
analysis
```




    [{'faceRectangle': {'height': 162, 'left': 130, 'top': 141, 'width': 162},
      'scores': {'anger': 9.29041e-06,
       'contempt': 0.000118981574,
       'disgust': 3.15619363e-05,
       'fear': 0.000589638,
       'happiness': 0.06630674,
       'neutral': 0.00555004273,
       'sadness': 7.44669524e-06,
       'surprise': 0.9273863}}]



Het geretourneerde JSON-object bevat de begrenzingsvakken van de gezichten die zijn herkend, samen met de gedetecteerde emoties. Elke emotie is gekoppeld aan een score tussen 0 en 1, waarbij een hogere score een betere indicatie van een emotie voorstelt dan een lagere score.

De volgende regels met code beschrijven de gedetecteerde emoties op de gezichten in de afbeelding met behulp van de bibliotheek `matplotlib`. Om het overzichtelijk te houden, worden alleen de duidelijkste drie emoties weergegeven.


```python
%matplotlib inline
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle
from io import BytesIO
from PIL import Image

plt.figure(figsize=(5,5))

image  = Image.open(BytesIO(image_data))
ax     = plt.imshow(image, alpha=0.6)

for face in analysis:
    fr = face["faceRectangle"]
    em = face["scores"]
    origin = (fr["left"], fr["top"])
    p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
    ax.axes.add_patch(p)
    ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
    plt.text(origin[0], origin[1], ct, fontsize=20)    
_ = plt.axis("off")
```

De functie `annotate_image` hierna kan worden gebruikt om emoties als overlay op een afbeeldingsbestand te leggen, op basis van het pad in het bestandssysteem. De functie is gebaseerd op de code voor het aanroepen van de Emotion-API die eerder is weergegeven.


```python
def annotate_image(image_path):    
    image_data = open(image_path, "rb").read()
    headers  = {'Ocp-Apim-Subscription-Key': subscription_key, "Content-Type": "application/octet-stream" }
    response = requests.post(emotion_recognition_url, headers=headers, data=image_data)
    response.raise_for_status()
    analysis = response.json()

    plt.figure()

    image  = Image.open(image_path)
    ax     = plt.imshow(image, alpha=0.6)

    for face in analysis:
        fr = face["faceRectangle"]
        em = face["scores"]
        origin = (fr["left"], fr["top"])
        p = Rectangle(origin, fr["width"], fr["height"], fill=False, linewidth=2, color='b')
        ax.axes.add_patch(p)
        ct = "\n".join(["{0:<10s}{1:>.4f}".format(k,v) for k, v in sorted(list(em.items()),key=lambda r: r[1], reverse=True)][:3])
        plt.text(origin[0], origin[1], ct, fontsize=20, va="bottom")    
    _ = plt.axis("off")
```

Ten slotte kan de functie `annotate_image` worden aangeroepen voor een afbeeldingsbestand, zoals wordt weergegeven op de volgende regel:


```python
annotate_image("images/emotion_2.jpg")
```
