---
title: Afwijkingsdetectie Python-app - cognitieve Services van Microsoft | Microsoft Docs
description: Verken een Python-notebook die gebruikmaakt van de Afwijkingsdetectie Detection-API in cognitieve Microsoft-Services. Oorspronkelijke gegevenspunten verzenden naar API en de verwachte waarde en afwijkingsdetectie punten.
services: cognitive-services
author: chliang
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: chliang
ms.openlocfilehash: d35f41ddab21aa155376ad52ff4084298dab8fc5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345259"
---
# <a name="anomaly-detection-python-application"></a>Afwijkingsdetectie detectie Python-toepassing

De zelfstudie laat zien hoe u de Afwijkingsdetectie Detection-API in Python en hoe u kunt uw resultaten met populaire bibliotheken visualiseren. Met Jupyter voor het uitvoeren van de zelfstudie en probeert uw eigen gegevens met de abonnementssleutel van uw. Raadpleeg voor meer informatie over aan de slag met interactieve Jupyter-notebooks, [Jupyter documentatie](http://jupyter.readthedocs.io/en/latest/index.html). 

## <a name="prerequisites"></a>Vereisten

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonneer u op de Anomaliedetectie en een abonnementssleutel ophalen 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="download-the-example-code"></a>De voorbeeldcode downloaden

1. Navigeer naar de [zelfstudie laptop in Github](https://github.com/MicrosoftAnomalyDetection/python-sample).
2. Klik op de groene knop klonen of downloaden van de zelfstudie. 

## <a name="opening-the-tutorial-notebook-in-jupyter"></a>Openen van de zelfstudie notebook in Jupyter

1. Open een opdrachtprompt en Ga naar de map python-voorbeeld.
2. Voer de opdracht Jupyter-notebook vanaf de opdrachtregel die Jupyter wordt gestart.
3. Klik in het venster Jupyter op <em>Afwijkingsdetectie detectie API Example.ipynb</em> openen van de zelfstudie notebook.   

## <a name="running-the-tutorial"></a>Uitvoeren van de zelfstudie

Voor het gebruik van deze laptop, moet u een abonnementssleutel voor de Afwijkingsdetectie Detection-API. Ga naar de pagina abonnement aan te melden. Op de pagina 'Aanmelden' gebruik van uw Microsoft-account aan te melden en u zich abonneren en ophalen van uw sleutels. Na het voltooien van het aanmeldingsproces, plak uw sleutel in het gedeelte variabelen van de laptop (hieronder). De primaire of secundaire sleutel werkt. Zorg ervoor dat de sleutel plaats tussen aanhalingstekens zodat deze een tekenreeks.

```Python

            # Variables
            endpoint = 'https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection'
            subscription_key = None #Here you have to paste your primary key

```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
