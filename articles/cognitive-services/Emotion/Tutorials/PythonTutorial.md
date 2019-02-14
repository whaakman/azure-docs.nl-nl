---
title: 'Zelfstudie: Emoties op een gezicht in een afbeelding herkennen - Emotion-API, Python'
titlesuffix: Azure Cognitive Services
description: Gebruik een Jupyter-notebook voor meer informatie over het gebruik van de Emotion-API met Python. Visualiseer uw resultaten met behulp van populaire bibliotheken.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: tutorial
ms.date: 05/23/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 9e03e5d534fa741def2010a21120edc3d7169964
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237464"
---
# <a name="tutorial-use-the-emotion-api-with-a-jupyter-notebook--python"></a>Zelfstudie: De Emotion-API gebruiken met een Jupyter-notebook en Python.

> [!IMPORTANT]
> De Emotion-API wordt op 15 februari 2019 afgeschaft. De mogelijkheid voor de herkenning van emoties is nu algemeen beschikbaar als onderdeel van de [Face-API](https://docs.microsoft.com/azure/cognitive-services/face/). 

Om eenvoudig aan de slag te gaan met de Emotion-API maken, laat de Jupyter-notebook waarnaar hieronder wordt verwezen u zien hoe u de API in Python gebruikt en hoe u uw resultaten met behulp van enkele populaire bibliotheken kunt visualiseren.

[Koppeling naar notebook in GitHub](https://github.com/Microsoft/Cognitive-Emotion-Python/blob/master/Jupyter%20Notebook/Emotion%20Analysis%20Example.ipynb)

### <a name="using-the-jupyter-notebook"></a>Jupyter Notebook gebruiken

Als u de notebook interactief wilt gebruiken, moet u deze klonen en uitvoeren in Jupyter. Volg de instructies in http://jupyter.readthedocs.org/en/latest/install.html om te leren hoe u aan de slag kunt gaan met interactieve Jupyter-notebooks.

Als u dit notebook wilt gebruiken, moet u een abonnementssleutel voor de Emotion-API hebben. Ga naar de [pagina Abonnement](https://azure.microsoft.com/try/cognitive-services/) om u aan te melden. Gebruik op de pagina 'Aanmelden' uw Microsoft-account om u aan te melden. U kunt zich dan abonneren en gratis sleutels ophalen. Nadat u het aanmeldingsproces hebt voltooid, plakt u uw sleutel in de onderstaande sectie met variabelen. De primaire of de secundaire sleutel werkt.

```
Python Example

#Variables

_url = 'https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10

```
