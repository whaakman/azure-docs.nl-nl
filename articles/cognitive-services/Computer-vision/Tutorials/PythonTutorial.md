---
title: 'Zelfstudie: Computer Vision-API met Python'
titlesuffix: Azure Cognitive Services
description: Informatie over het gebruik van de Computer Vision-API met Python met behulp van Jupyter-notebooks. Visualiseer uw resultaten met behulp van populaire bibliotheken.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: tutorial
ms.date: 02/25/2017
ms.author: kefre
ms.openlocfilehash: 59e88ecb253bc6da803ddf34a4a02229b99e259d
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45981708"
---
# <a name="tutorial-computer-vision-api-python"></a>Zelfstudie: Computer Vision-API met Python

Deze zelfstudie laat zien hoe u de Computer Vision-API in Python gebruikt en hoe u de resultaten kunt visualiseren met behulp van populaire bibliotheken. Gebruik Jupyter om de zelfstudie uit te voeren. Raadpleeg de [documentatie bij Jupyter](http://jupyter.readthedocs.io/en/latest/index.html) om te leren hoe u aan de slag kunt gaan met interactieve Jupyter-notitieblokken. 

### <a name="opening-the-tutorial-notebook-in-jupyter"></a>De zelfstudie-notebook openen in Jupyter 

1. Ga naar de [zelfstudie-notebook in Github](https://github.com/Microsoft/Cognitive-Vision-Python). 
2. Klik op de groene knop om de zelfstudie te klonen of downloaden. 
3. Open een opdrachtprompt en ga naar de map _Cognitive-Vision-Python-master\Jupyter Notebook_. 
4. Voer de opdracht **jupyter notebook** uit vanaf de opdrachtprompt. Hiermee wordt Jupyter gestart.
5. Klik in het venster van Jupyter op _Computer Vision API Example.ipynb_ om zelfstudie-notebook te openen. 

### <a name="running-the-tutorial"></a>De zelfstudie uitvoeren

Als u dit notebook wilt gebruiken, moet u een abonnementssleutel voor de Computer Vision-API hebben. Ga naar de [pagina Abonnement](https://azure.microsoft.com/try/cognitive-services/) om u aan te melden. Gebruik op de pagina 'Aanmelden' uw Microsoft-account om u aan te melden. U kunt zich dan abonneren en gratis sleutels ophalen. Nadat u het aanmeldingsproces hebt voltooid, plakt u uw sleutel in de sectie met variabelen van het notitieblok (hieronder weergegeven). De primaire of de secundaire sleutel werkt. Zet de sleutel tussen aanhalingstekens om er een tekenreeks van te maken.

```python
# Variables

_url = 'https://westcentralus.api.cognitive.microsoft.com/vision/v1/analyses'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```
