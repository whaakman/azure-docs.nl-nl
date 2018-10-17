---
title: 'Zelfstudie: Anomaliedetectie, Python'
titlesuffix: Azure Cognitive Services
description: Maak kennis met een Python-notebook dat gebruikmaakt van de Anomaliedetectie-API. Verzend oorspronkelijke gegevenspunten naar de API en haal de verwachte waarde en anomaliepunten op.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.component: anomaly-detection
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 98f83fbc4db8685526833c3d4f1d15903419ad3f
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887748"
---
# <a name="tutorial-anomaly-detection-with-python-application"></a>Zelfstudie: Anomaliedetectie met Python-toepassing

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

De zelfstudie laat zien hoe u de Anomaliedetectie-API in Python gebruikt en hoe u de resultaten kunt visualiseren met behulp van populaire bibliotheken. Gebruik Jupyter om de zelfstudie uit te voeren en probeer uw eigen gegevens uit met uw abonnementssleutel. Raadpleeg de [documentatie bij Jupyter](http://jupyter.readthedocs.io/en/latest/index.html) om te leren hoe u aan de slag kunt gaan met interactieve Jupyter-notitieblokken. 

## <a name="prerequisites"></a>Vereisten

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonneer u op Anomaliedetectie en haal de abonnementssleutel op 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="download-the-example-code"></a>Download de voorbeeldcode

1. Ga naar het [notitieblok van de zelfstudie in Github](https://github.com/MicrosoftAnomalyDetection/python-sample).
2. Klik op de groene knop om de zelfstudie te klonen of downloaden. 

## <a name="opening-the-tutorial-notebook-in-jupyter"></a>Het notitieblok van de zelfstudie openen in Jupyter

1. Open een opdrachtprompt en ga naar de map python-sample.
2. Voer de opdracht Jupyter notebook uit vanaf de opdrachtprompt om Jupyter te starten.
3. Klik in het venster van Jupyter op <em>Anomaly Detection API Example.ipynb</em> om het notitieblok van de zelfstudie te openen.   

## <a name="running-the-tutorial"></a>De zelfstudie uitvoeren

Om dit notitieblok te kunnen gebruiken, hebt u een abonnementssleutel voor de Anomaliedetectie-API nodig. Ga naar de pagina Subscription om u aan te melden. Gebruik op de pagina 'Sign in' uw Microsoft-account om u aan te melden. U kunt zich dan abonneren en sleutels ophalen. Nadat u het aanmeldingsproces hebt voltooid, plakt u uw sleutel in de sectie met variabelen van het notitieblok (hieronder weergegeven). De primaire of de secundaire sleutel werkt. Zet de sleutel tussen aanhalingstekens om er een tekenreeks van te maken.

```Python

            # Variables
            endpoint = 'https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection'
            subscription_key = None #Here you have to paste your primary key

```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
