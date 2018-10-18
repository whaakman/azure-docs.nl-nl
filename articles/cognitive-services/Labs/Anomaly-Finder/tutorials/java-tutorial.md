---
title: 'Zelfstudie: Anomaliedetectie, Java'
titlesuffix: Azure Cognitive Services
description: Maak kennis met een Java-app die gebruikmaakt van de Anomaliedetectie-API. Verzend oorspronkelijke gegevenspunten naar de API en haal de verwachte waarde en anomaliepunten op.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.component: anomaly-detection
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 4b544e2e59a40cebf75042c4040b84bceebcecf7
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48887697"
---
# <a name="tutorial-anomaly-detection-with-java-application"></a>Zelfstudie: Anomaliedetectie met Java-toepassing

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

In dit artikel ziet u hoe u een eenvoudige Java-toepassing gebruikt om de Anomaliedetectie-API aan te roepen.  
In het voorbeeld worden de gegevens van de tijdreeks naar de Anomaliedetectie-API verzonden met uw abonnementssleutel en worden alle anomaliepunten en de verwachte waarde voor elk gegevenspunt uit de API opgehaald.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Deze zelfstudie is ontwikkeld met [IntelliJ IDEA](https://www.jetbrains.com/idea). En u moet ook de [Java Development Kit (JDK)](http://www.oracle.com/technetwork/java/javase/downloads/index.html) versie 1.8 + en een recent hulpprogramma voor het compileren van [Apache Maven](http://maven.apache.org/) installeren.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonneer u op Anomaliedetectie en haal een abonnementssleutel op 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]
 

## <a name="download-the-tutorial-project"></a>Het zelfstudieproject downloaden

1. Ga naar de [Java-opslagplaats](https://github.com/MicrosoftAnomalyDetection/java-sample) van MicrosoftAnomalyDetection.
2. Klik op de knop Klonen of Downloaden.
3. Klik op ZIP-bestand downloaden om een ZIP-bestand van het zelfstudieproject te downloaden.

<a name="Step1"></a>
### <a name="open-the-tutorial-project"></a>Het zelfstudieproject openen

1. Pak het ZIP-bestand van het zelfstudieproject uit.
2. Klik in IntelliJ IDEA op **Bestand > Openen**. Het dialoogvenster Bestand openen of het dialoogvenster Project wordt weergegeven.
3. Selecteer het pad naar de hoofdmap van het uitgepakte project en klik op OK.
4. Vouw **src > main > java** uit in het deelvenster Projecten.
5. Dubbelklik op com.microsoft.cognitiveservice.anomalydetection.Main.java voor het laden van het bestand in de editor.

<a name="Step2"></a>
### <a name="replace-subscriptionkey-and-uri-region"></a>Vervang subscriptionKey en de URI-regio

```
// **********************************************
// *** Update or verify the following values. ***
// **********************************************

// Replace the subscriptionKey string value with your valid subscription key.
public static final String subscriptionKey = "<Subscription Key>";

public static final String uriBase = "https://api.labs.cognitive.microsoft.com/anomalyfinder/v1.0/anomalydetection";

```

<a name="Step3"></a>
### <a name="build-and-run-the-tutorial-project"></a>Het zelfstudieproject compileren en uitvoeren

1. Open het menu met de rechtermuisknop op een willekeurige plaats in het tabblad com.microsoft.cognitiveservice.anomalydetection.Main.java-broncode. 
2. Selecteer 'Main.main()' uitvoeren
3. Het resultaat van de voorbeeldaanvraag wordt geretourneerd en weergegeven in de terminal.

### <a name="result-of-the-tutorial-project"></a>Resultaat van het zelfstudieproject

[!INCLUDE [diagrams](../includes/diagrams.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
