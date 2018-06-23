---
title: Afwijkingsdetectie Javascript-app - cognitieve Services van Microsoft | Microsoft Docs
description: Verken een Javascript-Web-app die gebruikmaakt van de Afwijkingsdetectie Detection-API in cognitieve Microsoft-Services. Oorspronkelijke gegevenspunten verzenden naar API en de verwachte waarde en afwijkingsdetectie punten.
services: cognitive-services
author: wenya
manager: bix
ms.service: cognitive-services
ms.technology: anomaly-detection
ms.topic: article
ms.date: 05/01/2018
ms.author: wenya
ms.openlocfilehash: 42c3941a05efe8b74f818cd99f3606b3073892a9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345059"
---
# <a name="anomaly-detection-javascript-application"></a>Detectie van Javascript-toepassing voor afwijkingsdetectie

Een webtoepassing die gebruikmaakt van de Afwijkingsdetectie detectie REST-API voor het detecteren van een afwijkingsdetectie verkennen. In het voorbeeld verzendt de gegevens van de reeks tijd voor de Afwijkingsdetectie Detection-API met de abonnementssleutel van uw en vervolgens alle punten van de anomaliedetectie en de verwachte waarde opgehaald voor elk gegevenspunt van de API.

## <a name="prerequisites"></a>Vereisten

### <a name="platform-requirements"></a>Platformvereisten

Deze zelfstudie is ontwikkeld met een eenvoudige teksteditor.

### <a name="subscribe-to-anomaly-detection-and-get-a-subscription-key"></a>Abonneer u op de Anomaliedetectie en een abonnementssleutel ophalen 

[!INCLUDE [GetSubscriptionKey](../includes/get-subscription-key.md)]

## <a name="get-and-use-the-example"></a>Halen en gebruik het voorbeeld

Deze zelfstudie bestaat uit twee scenario's voor afwijkingsdetectie voor time series-gegevens. Aan de slag.

<a name="Step1"></a> 
### <a name="download-the-tutorial-project"></a>De zelfstudie project downloaden

Kloon de [cognitieve Services JavaScript Afwijkingsdetectie detectie zelfstudie](https://github.com/MicrosoftAnomalyDetection/javascript-sample), of het ZIP-bestand downloaden en pak het een lege map.

<a name="Step2"></a>
### <a name="run-the-example"></a>Het voorbeeld uitvoert

Er zijn twee scenario's kunt u proberen het voorbeeld.
1. Plaats uw **abonnementssleutel** detecteren in het veld Abonnementssleutel op de functie op anomalydetection.html.
2. Afwijkingsdetectie detectie API-eindpunt te plaatsen en controleer of dat u van de juiste regio in abonnement regio gebruikmaakt.
3. Open de **anomalydetection.html** bestand in een webbrowser.

**Scenario 1 detecteren wekelijkse tijd reeksgegevens**
1. Voer in het veld periode periode **7**. 
2. De voorbeeldgegevens vervangen door uw wekelijkse tijd gegevenspunten van reeksen (Json) in het veld punten of de voorbeeldgegevens rechtstreeks gebruiken.
3. Klik op de knop Afwijkingsdetectie en controleer of het resultaat in het juiste antwoord-tekstvak.

**Scenario 2 detecteren, de tijd reeksgegevens zonder punt**
1. Laat de leeg in de periode is ingediend, wordt ervan uitgegaan dat u de periode van de tijdreeks niet kent.
2. Gebruik dezelfde time series gegevens als het scenario 1.
3. Klik op de knop Afwijkingsdetectie en controleer of het veld periode in het juiste antwoord-tekstvak.

<a name="Step3"></a>
### <a name="read-the-result"></a>Lezen van het resultaat

[!INCLUDE [diagrams](../includes/diagrams.md)]

<a name="Review"></a>
### <a name="review-and-learn"></a>Bekijk en meer informatie

Nu u een actieve toepassing af. We controleren hoe de voorbeeld-app kan worden geïntegreerd met cognitieve Services-technologie. Deze stap wordt het eenvoudiger te gaan bouwen op deze app of uw eigen app met behulp van Microsoft-Afwijkingsdetectie ontwikkelen.
Deze voorbeeld-app wordt gebruikgemaakt van de Afwijkingsdetectie detectie Restful-API eindpunt.
Bekeken hoe de Restful-API wordt gebruikt in de voorbeeldtoepassing, bekijken we een codefragment van anomalydetection.html.
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
