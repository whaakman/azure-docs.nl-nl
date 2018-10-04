---
title: Detectie van afwijkingen Python-app - Microsoft Cognitive Services | Microsoft Docs
description: Verken een Python-notebook die gebruikmaakt van de API voor Afwijkingsdetectie in Microsoft Cognitive Services. Oorspronkelijke gegevenspunten verzenden naar API en de verwachte waarde en anomaliedetectie punten.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: 87cd9e976d231291ad13acecf188cfd668d692b6
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248224"
---
# <a name="anomaly-detection-python-application"></a>Anomaly Detection Python-toepassing

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

De zelfstudie laat zien hoe u de API voor Afwijkingsdetectie in Python en hoe u kunt uw resultaten met behulp van populaire bibliotheken te visualiseren. Met behulp van Jupyter de zelfstudie en uw eigen gegevens met uw abonnementssleutel probeert uit te voeren. Raadpleeg voor meer informatie over aan de slag met interactieve Jupyter notebooks, [Jupyter documentatie](http://jupyter.readthedocs.io/en/latest/index.html). 

## <a name="prerequisites"></a>Vereisten

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonneer u op de detectie van afwijkingen en de abonnementssleutel van een ophalen 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="download-the-example-code"></a>De voorbeeldcode downloaden

1. Navigeer naar de [zelfstudie notebook in Github](https://github.com/MicrosoftAnomalyDetection/python-sample).
2. Klik op de groene knop klonen of downloaden van de zelfstudie. 

## <a name="opening-the-tutorial-notebook-in-jupyter"></a>De zelfstudie notebook openen in Jupyter

1. Open een opdrachtprompt en Ga naar de map python-voorbeeld.
2. Voer de opdracht Jupyter-notebook vanaf de opdrachtprompt die Jupyter wordt gestart.
3. Klik in het venster Jupyter op <em>Anomaly Detection API Example.ipynb</em> om de zelfstudie notitieblok te openen.   

## <a name="running-the-tutorial"></a>De zelfstudie

Als u wilt dit notitieblok gebruiken, moet u een abonnementssleutel voor de API voor Afwijkingsdetectie. Ga naar de pagina abonnement om u te registreren. Gebruik uw Microsoft-account aan te melden bij op de pagina 'Aanmelden' en kunt u zich kunt abonneren en uw sleutels ophalen. Na het voltooien van het aanmeldingsproces, plak uw sleutel in de sectie met variabelen van de notebook (hieronder). De primaire of secundaire sleutel werkt. Zorg ervoor dat de sleutel tussen de aanhalingstekens zodat deze een tekenreeks.

```Python

            # Variables
            endpoint = 'https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection'
            subscription_key = None #Here you have to paste your primary key

```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
