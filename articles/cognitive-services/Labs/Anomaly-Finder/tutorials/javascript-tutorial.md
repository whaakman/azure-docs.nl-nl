---
title: 'Zelfstudie: Anomaliedetectie met Javascript'
titlesuffix: Azure Cognitive Services
description: Maak kennis met een Javascript-web-app die gebruikmaakt van de Anomaliedetectie-API. Verzend oorspronkelijke gegevenspunten naar de API en haal de verwachte waarde en anomaliepunten op.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: tutorial
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 9e66b24987b2318f3022404d951fbb911e7b592d
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55227825"
---
# <a name="tutorial-anomaly-detection-with-javascript-application"></a>Zelfstudie: Anomaliedetectie met Javascript-toepassing

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Maak kennis met een webtoepassing die gebruikmaakt van de REST API voor Anomaliedetectie om een anomalie te detecteren. In het voorbeeld worden de gegevens van de tijdreeks naar de Anomaliedetectie-API verzonden met uw abonnementssleutel en worden alle anomaliepunten en de verwachte waarde voor elk gegevenspunt uit de API opgehaald.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Deze zelfstudie is ontwikkeld met een eenvoudige teksteditor.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonneer u op Anomaliedetectie en haal een abonnementssleutel op 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Het voorbeeld downloaden en gebruiken

Deze zelfstudie bevat twee scenario's voor anomaliedetectie voor tijdreeksgegevens. Aan de slag.

<a name="Step1"></a> 
### <a name="download-the-tutorial-project"></a>Het zelfstudieproject downloaden

Kloon de [zelfstudie Cognitive Services JavaScript Anomaliedetectie](https://github.com/MicrosoftAnomalyDetection/javascript-sample) of download het zipbestand en pak het uit in een lege map.

<a name="Step2"></a>
### <a name="run-the-example"></a>Het voorbeeld uitvoeren

Er zijn twee scenario's die u kunt proberen als voorbeeld.
1. Stel uw **abonnementssleutel** in het veld Abonnementssleutel voor de detectiefunctie in op anomalydetection.html.
2. Geef het eindpunt van de Anomaliedetectie-API op en controleer of u de juiste regio gebruikt in Abonnementsregio.
3. Open het bestand **anomalydetection.html** in een webbrowser.

**Scenario 1: Wekelijkse tijdreeksgegevens detecteren**
1. Geef **7** op als periode in het vel Periode. 
2. Vervang de voorbeeldgegevens in het veld Punten door uw wekelijkse tijdreeksgegevenspunten (Json) of gebruik direct de voorbeeldgegevens.
3. Klik op de knop Anomaliedetectie en controleer het resultaat in het tekstvak Antwoord aan de rechterkant.

**Scenario 2: De tijdreeksgegevens detecteren zonder periode** 
1. Laat de periode leeg in het veld Periode, alsof u niet weet wat de periode van de tijdreeks is.
2. Maak gebruik van dezelfde tijdreeksgegevens als in scenario 1.
3. Klik op de knop Anomaliedetectie en controleer het veld Periode in het tekstvak Antwoord aan de rechterkant.

<a name="Step3"></a>
### <a name="read-the-result"></a>De resultaten lezen

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Bekijken en leren

U hebt nu een toepassing die wordt uitgevoerd. Nu gaan we kijken hoe de voorbeeld-app kan worden geïntegreerd met Cognitive Services-technologie. Deze stap maakt het eenvoudiger om door te gaan met bouwen aan deze app of om uw eigen app te ontwikkelen met Anomaliedetectie van Microsoft.
Deze voorbeeld-app maakt gebruik van het Restful-API-eindpunt van Anomaliedetectie.
Laten we een codefragment uit anomalydetection.html gebruiken om te zien hoe de Restful-API wordt gebruikt in de voorbeeldtoepassing.
```JavaScript
function anomalyDetection(url, subscriptionKey, points, period) {
    var obj = new Object();
    obj.Points = JSON.parse(points); // this points are read from text box.
    obj.Period = parseInt(period);//period=7 weekly period
    var tsData = JSON.stringify(obj);
    // Perform the REST API call.
    $.ajax({
        url: url, //Anomaly Detection API endpoint
        // Request headers.
        beforeSend: function (xhrObj) {
            xhrObj.setRequestHeader("Content-Type", "application/json");
            xhrObj.setRequestHeader("Ocp-Apim-Subscription-Key", subscriptionKey); // Replace your subscription key
        },
        type: "POST",
        // Request body.
        data: tsData, // json format
        })
        .done(function (data) {
            // Show formatted JSON on webpage.
            $("#responseTextArea").val(JSON.stringify(data, null, 2));
        })
        .fail(function (jqXHR, textStatus, errorThrown) {
            // Display error message.
            var errorString = (errorThrown === "") ? "Error. " : errorThrown + " (" + jqXHR.status + "): ";
            errorString += (jqXHR.responseText === "") ? "" : jQuery.parseJSON(jqXHR.responseText).message;
            $("#responseTextArea").val(errorString);           
        });
}

```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over REST API](https://dev.labs.cognitive.microsoft.com/docs/services/anomaly-detection/operations/post-anomalydetection)
