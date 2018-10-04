---
title: Detectie van afwijkingen Java-app - Microsoft Cognitive Services | Microsoft Docs
description: Een Java-app die gebruikmaakt van de API voor Afwijkingsdetectie in Microsoft Cognitive Services verkennen. Oorspronkelijke gegevenspunten verzenden naar API en de verwachte waarde en anomaliedetectie punten.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: ac26e29f4a839f69b123489600c2c83fe395c48a
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48247893"
---
# <a name="anomaly-detection-java-application"></a>Detectie van Java-toepassing voor anomaliedetectie

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

In dit artikel ziet u een eenvoudige Java-toepassing met de API voor Afwijkingsdetectie aanroepen.  
Het voorbeeld verzendt de time series-gegevens naar de API voor Afwijkingsdetectie met uw abonnementssleutel en vervolgens worden de anomaliedetectie-punten en verwachte waarde opgehaald voor elk gegevenspunt van de API.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

In deze zelfstudie is ontwikkeld met behulp van [IntelliJ IDEA](https://www.jetbrains.com/idea). En u moet ook installeren [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html) versie 1.8 +, en een recente [van Apache Maven](http://maven.apache.org/) hulpprogramma bouwen.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonneer u op de detectie van afwijkingen en de abonnementssleutel van een ophalen 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]
 

## <a name="download-the-tutorial-project"></a>Het zelfstudieproject downloaden

1. Ga naar de MicrosoftAnomalyDetection [Java opslagplaats](https://github.com/MicrosoftAnomalyDetection/java-sample).
2. Klik op de kloon of knop downloaden.
3. Klik op voor het downloaden van een ZIP-bestand van het zelfstudieproject ZIP downloaden.

<a name="Step1"></a>
### <a name="open-the-tutorial-project"></a>Open het project voor een tabellair

1. Pak het ZIP-bestand van het project voor een tabellair.
2. In IntelliJ IDEA gebruiken, klikt u op **bestand > openen**, in het dialoogvenster bestand openen of het Project wordt weergegeven.
3. Selecteer het pad naar de hoofdmap van het uitgepakte project en klik op OK.
4. Vouw in het deelvenster projecten **src > belangrijkste > java**.
5. Dubbelklik op com.microsoft.cognitiveservice.anomalydetection.Main.java voor het laden van het bestand in de editor.

<a name="Step2"></a>
### <a name="replace-subscriptionkey-and-uri-region"></a>Vervang subscriptionKey en URI regio

```
// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
public static final String subscriptionKey = "<Subscription Key>";

public static final String uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

```

<a name="Step3"></a>
### <a name="build-and-run-the-tutorial-project"></a>Bouwen en uitvoeren van de zelfstudie project

1. Open het menu met de rechtermuisknop op een willekeurige plaats in het tabblad com.microsoft.cognitiveservice.anomalydetection.Main.java bron-code. 
2. Selecteer 'Main.main()' uitvoeren
3. Het resultaat van het voorbeeld van een aanvraag wordt geretourneerd en wordt weergegeven in de terminal.

### <a name="result-of-the-tutorial-project"></a>Resultaat van het zelfstudieproject

[!INCLUDE [diagrams](../includes/diagrams.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
