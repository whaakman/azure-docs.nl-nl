---
title: Emotion-API Python-zelfstudie | Microsoft Docs
description: Gebruik een Jupyter-notebook voor informatie over het gebruik van de cognitieve Services Emotion-API met behulp van Python. Uw resultaten visualiseren met behulp van populaire bibliotheken.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: 70c8ca48c651601f3d7cbb3717c32bfe112176fe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344746"
---
# <a name="emotion-api-using-python-tutorial"></a>Emotion-API met behulp van Python-zelfstudie

> [!IMPORTANT]
> Video API Preview eindigt op 30 oktober 2017. Het nieuwe [Video indexeerfunctie API Preview](https://azure.microsoft.com/services/cognitive-services/video-indexer/) eenvoudig inzichten extraheren van video's en ervaringen van inhoud, zoals de lijst met zoekresultaten verbeteren doordat gesproken woorden, vlakken tekens en emoties. [Meer informatie](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

Om het gemakkelijk om te beginnen met Emotion-API maken, ziet de Jupyter-notebook gekoppeld hieronder u het gebruik van de API in Python en hoe u kunt uw resultaten met behulp van sommige populaire bibliotheken visualiseren. 

[Koppelen aan een laptop in GitHub](https://github.com/Microsoft/Cognitive-Emotion-Python/blob/master/Jupyter%20Notebook/Emotion%20Analysis%20Example.ipynb)

### <a name="using-the-jupyter-notebook"></a>Met behulp van de Jupyter-Notebook

De notebook als interactief wilt gebruiken, moet u het klonen en voer deze in Jupyter. Als u wilt weten hoe u aan de slag met interactieve Jupyter-notebooks, volg de instructies voor http://jupyter.readthedocs.org/en/latest/install.html. 

Voor het gebruik van deze laptop, moet u een abonnementssleutel voor de Emotion-API. Ga naar de [abonnementpagina](https://azure.microsoft.com/try/cognitive-services/) aan te melden. Op de pagina 'Aanmelden' gebruik je Microsoft-account aan te melden en u zich abonneren en gratis sleutels ophalen. Plak uw sleutel in het gedeelte variabelen die hieronder wordt weergegeven na het voltooien van het aanmeldingsproces. De primaire of secundaire sleutel werkt.

```
Python Example 

#Variables

_url = 'https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10

```
