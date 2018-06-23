---
title: Snel starten van emotion-API Python | Microsoft Docs
description: Get-informatie en codevoorbeelden kunt u snel aan de slag met de Emotion-API met behulp van Python in cognitieve Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 02/05/2018
ms.author: anroth
ms.openlocfilehash: ff1f6b2ddc872d0ee63d9885b04b1f007bc86e33
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344490"
---
# <a name="emotion-api-python-quickstart"></a>Quick Start emotion-API Python

> [!IMPORTANT]
> Video API Preview beëindigd op 30 oktober 2017. Het nieuwe [Video indexeerfunctie API Preview](https://azure.microsoft.com/services/cognitive-services/video-indexer/) eenvoudig inzichten extraheren van video's en ervaringen van inhoud, zoals de lijst met zoekresultaten verbeteren doordat gesproken woorden, vlakken tekens en emoties. [Meer informatie](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

In dit scenario vindt u informatie en codevoorbeelden kunt u snel aan de slag met de [Emotion-API herkennen methode](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa) met behulp van Python de emoties uitgedrukt in een of meer personen in een installatiekopie kan herkennen. 

U kunt in dit voorbeeld uitvoeren als een Jupyter-notebook op [MyBinder](https://mybinder.org) door te klikken op de lancering Binder badge: [ ![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=EmotionAPI.ipynb)


## <a name="prerequisite"></a>Vereiste
Sleutel voor gratis abonnement ophalen [hier](https://azure.microsoft.com/try/cognitive-services/)

## <a name="running-the-walkthrough"></a>De procedure wordt uitgevoerd
Om door te gaan met deze stapsgewijze Kennismaking vervangen `subscription_key` met de API-sleutel die u eerder hebt verkregen.


```python
subscription_key = None
assert subscription_key
```

Controleer vervolgens of de service-URL overeenkomt met de regio die u hebt gebruikt bij het instellen van de API-sleutel. Als u een proefabonnement sleutel gebruikt, hoeft u geen wijzigingen aanbrengen.


```python
emotion_recognition_url = "https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize"
```

In dit scenario maakt gebruik van installatiekopieën die zijn opgeslagen op schijf. Ook kunt u installatiekopieën die beschikbaar zijn via een openbaar toegankelijke URL zijn. Zie voor meer informatie de [REST API-documentatie](https://westus.dev.cognitive.microsoft.com/docs/services/5639d931ca73072154c1ce89/operations/563b31ea778daf121cc3a5fa).

U ziet dat u moet instellen omdat de afbeeldingsgegevens is doorgegeven als onderdeel van de aanvraagtekst, de `Content-Type` koptekst tot `application/octet-stream`. Als u in een installatiekopie via een URL doorgeeft, moet u de header ingesteld op:
```python
header = {'Ocp-Apim-Subscription-Key': subscription_key }
```
een woordenlijst met de URL maken:
```python
data = {'url': image_url}
```
en geef die naar de `requests` bibliotheek gebruiken:
```python
requests.post(emotion_recognition_url, headers=headers, json=image_data)
```

Eerst een paar voorbeeldafbeeldingen downloaden van de [Emotion-API](https://azure.microsoft.com/services/cognitive-services/emotion/) site.


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



Het geretourneerde JSON-object bevat de omsluitende uit van de vlakken die samen met de gedetecteerde emoties zijn herkend. Elke emotion is gekoppeld aan een score tussen 0 en 1 waarbij een hogere score meer indicatief voor een emotion dan een lagere score. 

De volgende regels code de gedetecteerde emoties de reacties in de installatiekopie met de `matplotlib` bibliotheek. Als u wilt overzichtelijker, worden alleen de eerste drie emoties weergegeven.


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

De `annotate_image` functie weergegeven volgende kan worden gebruikt voor de overlay emoties bovenop een installatiekopiebestand krijgt het pad voor het bestandssysteem. Deze is gebaseerd op de code voor aanroepen in de eerder vermelde Emotion-API.


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

Ten slotte de `annotate_image` functie kan worden aangeroepen voor een installatiekopiebestand, zoals wordt weergegeven in de volgende regel:


```python
annotate_image("images/emotion_2.jpg")
```
