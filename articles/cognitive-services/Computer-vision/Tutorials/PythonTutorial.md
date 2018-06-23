---
title: Computer Vision-API Python-zelfstudie | Microsoft Docs
description: Informatie over het gebruik van de Computer Vision-API met behulp van Python via Jupyter-notebooks in cognitieve Microsoft-Services. De resultaten met populaire bibliotheken visualiseren.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 02/25/2017
ms.author: kefre
ms.openlocfilehash: a093c2d066e70a8daf1fe1cd33ccf794ecb196af
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344739"
---
# <a name="computer-vision-api-python-tutorial"></a>Computer Vision-API Python-zelfstudie

Deze zelfstudie ziet u hoe u de Computer Vision-API in Python en hoe de resultaten met behulp van sommige populaire bibliotheken visualiseren. Jupyter gebruiken de zelfstudie uitvoeren. Als u wilt weten hoe u aan de slag met interactieve Jupyter-notebooks, Raadpleeg: [Jupyter Documementation](http://jupyter.readthedocs.io/en/latest/index.html). 

### <a name="opening-the-tutorial-notebook-in-jupyter"></a>Openen van de zelfstudie Notebook in Jupyter 

1. Navigeer naar de [zelfstudie laptop in GitHub](https://github.com/Microsoft/Cognitive-Vision-Python). 
2. Klik op de groene knop klonen of downloaden van de zelfstudie. 
3. Open een opdrachtprompt en Ga naar de map _Cognitive, visie, Python, master\Jupyter Notebook_. 
4. Voer de opdracht **jupyter-notebook** vanaf de opdrachtprompt. Hiermee wordt de Jupyter gestart.
5. Klik in het venster Jupyter op _Computer Vision-API Example.ipynb_ openen van de zelfstudie notebook 

### <a name="running-the-tutorial"></a>Uitvoeren van de zelfstudie

Voor het gebruik van deze laptop, moet u een abonnementssleutel voor de Computer Vision-API. Ga naar de [abonnementpagina](https://azure.microsoft.com/try/cognitive-services/) aan te melden. Op de pagina 'Aanmelden' gebruik je Microsoft-account aan te melden en u zich abonneren en gratis sleutels ophalen. Na het voltooien van het aanmeldingsproces, plak uw sleutel in het gedeelte variabelen van de laptop (hieronder). De primaire of secundaire sleutel werkt. Zorg ervoor dat de sleutel plaats tussen aanhalingstekens zodat deze een tekenreeks.

```python
# Variables

_url = 'https://westcentralus.api.cognitive.microsoft.com/vision/v1/analyses'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```
