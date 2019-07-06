---
title: Bewerkingen op een afbeelding uitvoeren - Python
titlesuffix: Azure Cognitive Services
description: Informatie over het gebruik van de Computer Vision-API met Python met behulp van Jupyter-notebooks. Visualiseer uw resultaten met behulp van populaire bibliotheken.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: b18f41db772c1c214bdf48c9bb765b1ce7e6d2c2
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604162"
---
# <a name="computer-vision-api-jupyter-notebook"></a>Computer Vision-API Jupyter-notebook

Deze handleiding leest u hoe u de Computer Vision-API in Python en voor het visualiseren van uw resultaten met behulp van populaire bibliotheken. U gebruikt Jupyter om de zelfstudie uit te voeren. Raadpleeg de (Engelstalige) [documentatie bij Jupyter](https://jupyter.readthedocs.io/en/latest/index.html) om te leren hoe u aan de slag kunt gaan met interactieve Jupyter-notitieblokken.

## <a name="prerequisites"></a>Vereisten

- [Python 2.7+ of 3.5+](https://www.python.org/downloads/)
- [pip](https://pip.pypa.io/en/stable/installing/)-hulpprogramma
- Installatie van [Jupyter Notebook](https://jupyter.org/install)

## <a name="open-the-notebook-in-jupyter"></a>Open het notitieblok in Jupyter 

1. Ga naar de GitHub-opslagplaats van [Cognitive Vision Python](https://github.com/Microsoft/Cognitive-Vision-Python). 
2. Klik op de groene knop om de opslagplaats te klonen of downloaden. 
3. Open een opdrachtprompt en ga naar de map **Cognitive-Vision-Python\Jupyter Notebook**.
1. Zorg ervoor dat u alle vereiste bibliotheken hebt door opdracht `pip install requests opencv-python numpy matplotlib` uit te voeren vanaf de opdrachtprompt.
1. Start Jupyter door opdracht `jupyter notebook` uit te voeren vanaf de opdrachtprompt.
1. Klik in het venster van Jupyter op _Computer Vision API Example.ipynb_ om het zelfstudie-notebook te openen.

## <a name="run-the-notebook"></a>Het notitieblok uitvoeren

Als u dit notebook wilt gebruiken, moet u een abonnementssleutel voor de Computer Vision-API hebben. Ga naar de [pagina Abonnement](https://azure.microsoft.com/try/cognitive-services/) om u aan te melden. Gebruik op de pagina **Aanmelden** uw Microsoft-account om u aan te melden. U kunt zich dan abonneren en gratis sleutels ophalen. Nadat u het aanmeldingsproces hebt voltooid, plakt u uw sleutel in de sectie `Variables` van het notebook (hieronder weergegeven). De primaire of de secundaire sleutel werkt. Zet de sleutel tussen aanhalingstekens om er een tekenreeks van te maken.

Controleer of het veld `_region` overeenkomt met de regio die met uw abonnement correspondeert.

```python
# Variables
_region = 'westcentralus'  # Here you enter the region of your subscription
_url = 'https://{}.api.cognitive.microsoft.com/vision/v2.0/analyze'.format(
    _region)
_key = None  # Here you have to paste your primary key
_maxNumRetries = 10
```

Als u de zelfstudie uitvoert, kunt u afbeeldingen toevoegen om te analyseren, zowel via een URL als lokale opslag. In het script worden de afbeeldingen en analysegegevens in het notebook weergegeven.
