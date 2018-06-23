---
title: Afwijkingsdetectie Java-app - cognitieve Services van Microsoft | Microsoft Docs
description: Een Java-app die gebruikmaakt van de Afwijkingsdetectie Detection-API in Microsoft cognitieve Services verkennen. Oorspronkelijke gegevenspunten verzenden naar API en de verwachte waarde en afwijkingsdetectie punten.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 228d440da358eba1322e2228c54f21e925e36ecd
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345054"
---
# <a name="anomaly-detection-java-application"></a>Detectie van Java-toepassing voor afwijkingsdetectie

In dit artikel wordt gedemonstreerd met behulp van een eenvoudige Java-toepassing de Afwijkingsdetectie Detection-API aanroepen.  
In het voorbeeld verzendt de gegevens van de reeks tijd voor de Afwijkingsdetectie Detection-API met de abonnementssleutel van uw en vervolgens alle punten afwijkingsdetectie en de verwachte waarde opgehaald voor elk gegevenspunt van de API.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Deze zelfstudie is ontwikkeld met behulp van [IntelliJ IDEA](https://www.jetbrains.com/idea). En u moet ook installeren [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html) versie 1.8 +, en een recente [van Apache Maven](http://maven.apache.org/) hulpprogramma bouwen.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonneer u op de Anomaliedetectie en een abonnementssleutel ophalen 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]
 

## <a name="download-the-tutorial-project"></a>De zelfstudie project downloaden

1. Ga naar de MicrosoftAnomalyDetection [Java-opslagplaats](https://github.com/MicrosoftAnomalyDetection/java-sample).
2. Klik op de kloon of knop downloaden.
3. Klik op voor het downloaden van een ZIP-bestand van de zelfstudie project ZIP downloaden.

<a name="Step1"></a>
### <a name="open-the-tutorial-project"></a>Open het project zelfstudie

1. Pak het ZIP-bestand van de zelfstudie project.
2. Klik in de IntelliJ IDEA op **bestand > openen**, Open bestand of dialoogvenster wordt weergegeven.
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

1. Online zetten van het menu met de rechtermuisknop op een willekeurige plaats in het tabblad com.microsoft.cognitiveservice.anomalydetection.Main.java bron-code. 
2. Voer Main.main() selecteren
3. Het resultaat van het voorbeeld van een aanvraag wordt geretourneerd en wordt weergegeven in de terminal.

### <a name="result-of-the-tutorial-project"></a>Resultaat van de zelfstudie project

[!INCLUDE [diagrams](../includes/diagrams.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
