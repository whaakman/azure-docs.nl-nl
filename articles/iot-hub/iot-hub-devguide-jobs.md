---
title: Informatie over Azure IoT Hub-taken | Microsoft Docs
description: Handleiding voor ontwikkelaars - plannen van taken om uit te voeren op meerdere apparaten worden verbonden met uw IoT-hub. Taken kunnen labels en gewenste eigenschappen bijwerken en directe methoden aanroepen op meerdere apparaten.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: e2beec1308b9664d35ccd9d355403b7076567f2f
ms.sourcegitcommit: b5ac31eeb7c4f9be584bb0f7d55c5654b74404ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/23/2018
ms.locfileid: "42745841"
---
# <a name="schedule-jobs-on-multiple-devices"></a>Taken op meerdere apparaten plannen

Azure IoT Hub kunt u een aantal van bouwstenen zoals [apparaatdubbeleigenschappen- en -codes] [ lnk-twin-devguide] en [directe methoden][lnk-dev-methods]. Back-end-apps inschakelen normaal gesproken apparaatbeheerders en operators om te werken en te communiceren met IoT-apparaten bulksgewijs en op het geplande tijdstip.  Taken uitvoeren van apparaatdubbel werkt bij en directe methoden aan de hand van apparaten op het geplande tijdstip.  Een operator kan bijvoorbeeld een back-end-app die geïnitieerd en bijgehouden van een taak voor het opnieuw opstarten van een set met apparaten in het bouwen van 43 en floor 3 op een tijdstip dat niet zou verstorend voor de bewerkingen van het gebouw gebruiken.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Overweeg het gebruik van taken wanneer u nodig hebt voor het plannen en de voortgang volgen een van de volgende activiteiten op een set apparaten:

* Gewenste eigenschappen bijwerken
* Bijwerken van tags
* Directe methoden aanroepen

## <a name="job-lifecycle"></a>Levenscyclus van taak
Taken worden gestart door de back-end van de oplossing en beheerd door IoT Hub.  U kunt een taak via een URI service gerichte starten (`{iot hub}/jobs/v2/{device id}/methods/<jobID>?api-version=2016-11-14`) en query's uitvoeren voor de voortgang van een taak wordt uitgevoerd via een gerichte service-URI (`{iot hub}/jobs/v2/<jobId>?api-version=2016-11-14`). Als u wilt vernieuwen van de status van actieve taken wanneer een taak wordt gestart, moet u een taakquery uitvoeren.

> [!NOTE]
> Wanneer u een taak start, namen en waarden mag alleen US-ASCII-afdrukbare uit alfanumerieke tekens, met uitzondering in de volgende set: `$ ( ) < > @ , ; : \ " / [ ] ? = { } SP HT`.

## <a name="jobs-to-execute-direct-methods"></a>Taken voor het uitvoeren van de directe methoden
Het volgende fragment toont de details van de aanvraag HTTPS 1.1 voor het uitvoeren van een [directe methode] [ lnk-dev-methods] in een set van apparaten met behulp van een taak:

    PUT /jobs/v2/<jobId>?api-version=2016-11-14

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleDirectRequest', 
        cloudToDeviceMethod: {
            methodName: '<methodName>',
            payload: <payload>,                 
            responseTimeoutInSeconds: methodTimeoutInSeconds 
        },
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        
    }

De queryvoorwaarde kan ook worden op een enkel apparaat-ID of een lijst van apparaat-id's zoals weergegeven in de volgende voorbeelden:

```
queryCondition = "deviceId = 'MyDevice1'"
queryCondition = "deviceId IN ['MyDevice1','MyDevice2']"
queryCondition = "deviceId IN ['MyDevice1']
```
[IoT Hub-querytaal] [ lnk-query] bevat informatie over IoT Hub-querytaal in meer detail.

## <a name="jobs-to-update-device-twin-properties"></a>Taken apparaatdubbeleigenschappen bijwerken
Het volgende fragment toont de details van de aanvraag HTTPS 1.1 voor het bijwerken van apparaatdubbeleigenschappen met behulp van een taak:

    PUT /jobs/v2/<jobId>?api-version=2016-11-14
    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

    {
        jobId: '<jobId>',
        type: 'scheduleTwinUpdate', 
        updateTwin: <patch>                 // Valid JSON object
        queryCondition: '<queryOrDevices>', // query condition
        startTime: <jobStartTime>,          // as an ISO-8601 date string
        maxExecutionTimeInSeconds: <maxExecutionTimeInSeconds>        // format TBD
    }

## <a name="querying-for-progress-on-jobs"></a>Query's uitvoeren voor de voortgang van taken
Het volgende fragment toont de details van de aanvraag HTTPS 1.1 voor het uitvoeren van query's voor taken:

    GET /jobs/v2/query?api-version=2016-11-14[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

    Authorization: <config.sharedAccessSignature>
    Content-Type: application/json; charset=utf-8
    Request-Id: <guid>
    User-Agent: <sdk-name>/<sdk-version>

De continuationToken wordt geleverd door het antwoord.  

U kunt een query voor de status van de taak kan worden uitgevoerd op elk apparaat met behulp van de [IoT Hub-querytaal voor apparaatdubbels, taken en berichtroutering][lnk-query].

## <a name="jobs-properties"></a>Eigenschappen van taken
De volgende lijst bevat de eigenschappen en de bijbehorende beschrijvingen, die kunnen worden gebruikt voor het uitvoeren van query's voor taken of Taakresultaten.

| Eigenschap | Beschrijving |
| --- | --- |
| **taak-id** |Toepassing opgegeven ID voor de taak. |
| **startTime** |Toepassing opgegeven begintijd (ISO 8601-) voor de taak. |
| **endTime** |IoT Hub opgegeven datum (ISO 8601-) voor wanneer de taak is voltooid. Geldige pas nadat de taak de status 'voltooid' hebben bereikt. |
| **type** |Typen taken: |
| | **scheduledUpdateTwin**: een taak die wordt gebruikt voor het bijwerken van een set van gewenste eigenschappen of labels. |
| | **scheduledDeviceMethod**: een taak die wordt gebruikt om aan te roepen een apparaatmethode op een set dubbele apparaten. |
| **status** |Huidige status van de taak. Mogelijke waarden voor de status van: |
| | **in behandeling**: gepland en wachten op het door de taak service opgehaald. |
| | **geplande**: gepland voor een tijd in de toekomst. |
| | **met**: momenteel actieve taken. |
| | **geannuleerd**: taak is geannuleerd. |
| | **kan geen**: taak is mislukt. |
| | **voltooid**: taak is voltooid. |
| **deviceJobStatistics** |Statistieken over van de taak kan worden uitgevoerd. |
| | **deviceJobStatistics** eigenschappen: |
| | **deviceJobStatistics.deviceCount**: aantal apparaten in de taak. |
| | **deviceJobStatistics.failedCount**: aantal apparaten waarop de taak is mislukt. |
| | **deviceJobStatistics.succeededCount**: aantal apparaten waarop de taak is voltooid. |
| | **deviceJobStatistics.runningCount**: aantal apparaten dat de taak momenteel worden uitgevoerd. |
| | **deviceJobStatistics.pendingCount**: aantal apparaten dat in behandeling voor het uitvoeren van de taak zijn. |

### <a name="additional-reference-material"></a>Meer referentiemateriaal dat beschikbaar is
Er zijn andere onderwerpen met naslaginformatie in de IoT Hub developer guide:

* [IoT Hub-eindpunten] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont.
* [Bandbreedtebeperking en quota's] [ lnk-quotas] beschrijving van de quota die betrekking hebben op de IoT Hub-service en de beperkingsgedrag u kunt verwachten wanneer u de service gebruiken.
* [Azure IoT-apparaat en service-SDK's] [ lnk-sdks] geeft een lijst van de verschillende SDK's kunt u bij het ontwikkelen van apparaat- en service-apps die communiceren met IoT Hub-taal.
* [IoT Hub-querytaal voor apparaatdubbels, taken en berichtroutering] [ lnk-query] beschrijving van de IoT Hub-querytaal. Gebruik deze querytaal gegevens ophalen uit IoT Hub over uw apparaatdubbels en taken.
* [IoT Hub MQTT-ondersteuning] [ lnk-devguide-mqtt] vindt u meer informatie over IoT Hub-ondersteuning voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen
Als u wilt uitproberen enkele concepten die worden beschreven in dit artikel, raadpleegt u de volgende zelfstudie voor IoT-Hub:

* [Taken plannen en uitzenden][lnk-jobs-tutorial]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-jobs-tutorial]: iot-hub-node-node-schedule-jobs.md
[lnk-c2d-methods]: quickstart-control-device-node.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-devguide]: iot-hub-devguide-device-twins.md
