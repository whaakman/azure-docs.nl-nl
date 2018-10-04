---
title: Detectie van afwijkingen Javascript-app - Microsoft Cognitive Services | Microsoft Docs
description: Verken een Javascript-Web-app die gebruikmaakt van de API voor Afwijkingsdetectie in Microsoft Cognitive Services. Oorspronkelijke gegevenspunten verzenden naar API en de verwachte waarde en anomaliedetectie punten.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 5bb123648a683454597b0561f9f82dffb70eab04
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248360"
---
# <a name="anomaly-detection-javascript-application"></a>Detectie van Javascript-toepassing voor anomaliedetectie

[!INCLUDE [PrivatePreviewNote](../../../../../includes/cognitive-services-anomaly-finder-private-preview-note.md)]

Een webtoepassing die gebruikmaakt van de REST API voor Afwijkingsdetectie voor het detecteren van een anomalie verkennen. Het voorbeeld verzendt de time series-gegevens naar de API voor Afwijkingsdetectie met uw abonnementssleutel en vervolgens worden alle punten in de anomaliedetectie en de verwachte waarde opgehaald voor elk gegevenspunt van de API.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

In deze zelfstudie is ontwikkeld met behulp van een eenvoudige teksteditor.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonneer u op de detectie van afwijkingen en de abonnementssleutel van een ophalen 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Halen en gebruik het voorbeeld

Deze zelfstudie bevat twee scenario's voor afwijkingsdetectie voor time series-gegevens. Laten we beginnen.

<a name="Step1"></a> 
### <a name="download-the-tutorial-project"></a>Het zelfstudieproject downloaden

Kloon de [Cognitive Services JavaScript Anomaly Detection zelfstudie](https://github.com/MicrosoftAnomalyDetection/javascript-sample), of het ZIP-bestand downloaden en uitpakken naar een lege map.

<a name="Step2"></a>
### <a name="run-the-example"></a>Het voorbeeld uitvoeren

Er zijn twee scenario's kunt u proberen het voorbeeld.
1. Plaats uw **abonnementssleutel** detecteren functie op anomalydetection.html in het veld Abonnementssleutel op.
2. Plaats anomaly detection-API-eindpunt en controleer of u de juiste regio in de regio van het abonnement.
3. Open de **anomalydetection.html** bestand in een webbrowser.

**Scenario 1 analyse wekelijkse time series-gegevens**
1. Voer in het veld periode periode **7**. 
2. De voorbeeldgegevens vervangen door uw wekelijkse tijd gegevenspunten in reeks (Json) in het veld punten of rechtstreeks gebruik van de voorbeeldgegevens.
3. Klik op de knop Anomaliedetectie en controleer of het resultaat in het juiste antwoord tekstvak in.

**Scenario 2 detecteren, de time series-gegevens zonder een periode**
1. Laat de periode die leeg zijn in de periode is ingediend, wordt ervan uitgegaan dat u niet weet wat de periode van de tijdreeks.
2. Met behulp van de dezelfde time series-gegevens als het scenario 1.
3. Klik op de knop Anomaliedetectie en controleer of het veld periode in het juiste antwoord tekstvak in.

<a name="Step3"></a>
### <a name="read-the-result"></a>Het resultaat lezen

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Controleer en meer

U krijgt nu een toepassing die wordt uitgevoerd. Laten we bekijken hoe de voorbeeld-app kan worden geïntegreerd met Cognitive Services-technologie. Deze stap maakt het eenvoudiger om te blijven ontwikkelen op deze app of uw eigen app ontwikkelen met Microsoft-Anomaliedetectie.
Deze voorbeeldapp maakt gebruik van de Restful API voor Afwijkingsdetectie eindpunt.
Bekijk hoe de Restful-API wordt gebruikt in het voorbeeld van de toepassing en we kijken naar een codefragment van anomalydetection.html.
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
