---
title: Informatie over Azure IoT Hub-taken | Microsoft Docs
description: Handleiding voor ontwikkelaars - plannen van taken om uit te voeren op meerdere apparaten worden verbonden met uw IoT-hub. Taken kunnen labels en gewenste eigenschappen bijwerken en directe methoden aanroepen op meerdere apparaten.
author: robinsh
manager: philmea
ms.author: robin.shahan
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 10/09/2018
ms.openlocfilehash: c2b05b1854b4f1d7ee4ac65ebe635330ba8c604e
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57011361"
---
# <a name="schedule-jobs-on-multiple-devices"></a>Taken op meerdere apparaten plannen

Azure IoT Hub kunt u een aantal van bouwstenen zoals [apparaatdubbeleigenschappen- en -codes](iot-hub-devguide-device-twins.md) en [directe methoden](iot-hub-devguide-direct-methods.md). Back-end-apps inschakelen normaal gesproken apparaatbeheerders en operators om te werken en te communiceren met IoT-apparaten bulksgewijs en op het geplande tijdstip. Taken uitvoeren van apparaatdubbel werkt bij en directe methoden aan de hand van apparaten op het geplande tijdstip. Een operator kan bijvoorbeeld een back-end-app die geïnitieerd en bijgehouden van een taak voor het opnieuw opstarten van een set met apparaten in het bouwen van 43 en floor 3 op een tijdstip dat niet zou verstorend voor de bewerkingen van het gebouw gebruiken.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Overweeg het gebruik van taken wanneer u nodig hebt voor het plannen en de voortgang volgen een van de volgende activiteiten op een set apparaten:

* Gewenste eigenschappen bijwerken
* Bijwerken van tags
* Directe methoden aanroepen

## <a name="job-lifecycle"></a>Levenscyclus van taak

Taken worden gestart door de back-end van de oplossing en beheerd door IoT Hub. U kunt een taak via een URI service gerichte starten (`PUT https://<iot hub>/jobs/v2/<jobID>?api-version=2018-06-30`) en query's uitvoeren voor de voortgang van een taak wordt uitgevoerd via een gerichte service-URI (`GET https://<iot hub>/jobs/v2/<jobID?api-version=2018-06-30`). Als u wilt vernieuwen van de status van actieve taken wanneer een taak wordt gestart, moet u een taakquery uitvoeren.

> [!NOTE]
> Wanneer u een taak start, namen en waarden mag alleen US-ASCII-afdrukbare uit alfanumerieke tekens, behalve in het volgende instellen: `$ ( ) < > @ , ; : \ " / [ ] ? = { } SP HT`

## <a name="jobs-to-execute-direct-methods"></a>Taken voor het uitvoeren van de directe methoden

Het volgende fragment toont de details van de aanvraag HTTPS 1.1 voor het uitvoeren van een [directe methode](iot-hub-devguide-direct-methods.md) in een set van apparaten met behulp van een taak:

```
PUT /jobs/v2/<jobId>?api-version=2018-06-30

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8
Request-Id: <guid>
User-Agent: <sdk-name>/<sdk-version>

{
    "jobId": "<jobId>",
    "type": "scheduleDirectMethod",
    "cloudToDeviceMethod": {
        "methodName": "<methodName>",
        "payload": <payload>,
        "responseTimeoutInSeconds": methodTimeoutInSeconds
    },
    "queryCondition": "<queryOrDevices>", // query condition
    "startTime": <jobStartTime>,          // as an ISO-8601 date string
    "maxExecutionTimeInSeconds": <maxExecutionTimeInSeconds>
}
```

De queryvoorwaarde kan ook worden op een enkel apparaat-ID of een lijst van apparaat-id's zoals weergegeven in de volgende voorbeelden:

```
"queryCondition" = "deviceId = 'MyDevice1'"
"queryCondition" = "deviceId IN ['MyDevice1','MyDevice2']"
"queryCondition" = "deviceId IN ['MyDevice1']"
```

[IoT Hub-querytaal](iot-hub-devguide-query-language.md) bevat informatie over IoT Hub-querytaal in meer detail.

## <a name="jobs-to-update-device-twin-properties"></a>Taken apparaatdubbeleigenschappen bijwerken

Het volgende fragment toont de details van de aanvraag HTTPS 1.1 voor het bijwerken van apparaatdubbeleigenschappen met behulp van een taak:

```
PUT /jobs/v2/<jobId>?api-version=2018-06-30

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8
Request-Id: <guid>
User-Agent: <sdk-name>/<sdk-version>

{
    "jobId": "<jobId>",
    "type": "scheduleTwinUpdate",
    "updateTwin": <patch>                 // Valid JSON object
    "queryCondition": "<queryOrDevices>", // query condition
    "startTime": <jobStartTime>,          // as an ISO-8601 date string
    "maxExecutionTimeInSeconds": <maxExecutionTimeInSeconds>
}
```

## <a name="querying-for-progress-on-jobs"></a>Query's uitvoeren voor de voortgang van taken

Het volgende fragment toont de details van de aanvraag HTTPS 1.1 voor het uitvoeren van query's voor taken:

```
GET /jobs/v2/query?api-version=2018-06-30[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8
Request-Id: <guid>
User-Agent: <sdk-name>/<sdk-version>
```

De continuationToken wordt geleverd door het antwoord.

U kunt een query voor de status van de taak kan worden uitgevoerd op elk apparaat met behulp van de [IoT Hub-querytaal voor apparaatdubbels, taken en berichtroutering](iot-hub-devguide-query-language.md).

## <a name="jobs-properties"></a>Eigenschappen van taken

De volgende lijst bevat de eigenschappen en de bijbehorende beschrijvingen, die kunnen worden gebruikt voor het uitvoeren van query's voor taken of Taakresultaten.

| Eigenschap | Description |
| --- | --- |
| **jobId** |Toepassing opgegeven ID voor de taak. |
| **startTime** |Toepassing opgegeven begintijd (ISO 8601-) voor de taak. |
| **endTime** |IoT Hub opgegeven datum (ISO 8601-) voor wanneer de taak is voltooid. Geldige pas nadat de taak de status 'voltooid' hebben bereikt. |
| **type** |Typen taken: |
| | **scheduledUpdateTwin**: Een taak die wordt gebruikt voor het bijwerken van een set van gewenste eigenschappen of labels. |
| | **scheduledDeviceMethod**: Een taak die wordt gebruikt om aan te roepen een apparaatmethode op een set dubbele apparaten. |
| **status** |Huidige status van de taak. Mogelijke waarden voor de status van: |
| | **In behandeling**: Gepland en wachten op het door de taak service opgehaald. |
| | **Geplande**: Gepland voor een tijd in de toekomst. |
| | **Met**: Momenteel actieve taken. |
| | **Geannuleerd**: Taak is geannuleerd. |
| | **Kan geen**: Taak is mislukt. |
| | **Voltooid**: Taak is voltooid. |
| **deviceJobStatistics** |Statistieken over van de taak kan worden uitgevoerd. |
| | **deviceJobStatistics** eigenschappen: |
| | **deviceJobStatistics.deviceCount**: Het aantal apparaten in de taak. |
| | **deviceJobStatistics.failedCount**: Aantal apparaten waarop de taak is mislukt. |
| | **deviceJobStatistics.succeededCount**: Aantal apparaten waarop de taak is voltooid. |
| | **deviceJobStatistics.runningCount**: Het aantal apparaten dat de taak momenteel worden uitgevoerd. |
| | **deviceJobStatistics.pendingCount**: Het aantal apparaten dat in behandeling voor het uitvoeren van de taak zijn. |

### <a name="additional-reference-material"></a>Meer referentiemateriaal dat beschikbaar is

Er zijn andere onderwerpen met naslaginformatie in de IoT Hub developer guide:

* [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md) beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont.

* [Bandbreedtebeperking en quota's](iot-hub-devguide-quotas-throttling.md) beschrijving van de quota die betrekking hebben op de IoT Hub-service en de beperkingsgedrag u kunt verwachten wanneer u de service gebruiken.

* [Azure IoT-apparaat en service-SDK's](iot-hub-devguide-sdks.md) geeft een lijst van de verschillende SDK's kunt u bij het ontwikkelen van apparaat- en service-apps die communiceren met IoT Hub-taal.

* [IoT Hub-querytaal voor apparaatdubbels, taken en berichtroutering](iot-hub-devguide-query-language.md) beschrijving van de IoT Hub-querytaal. Gebruik deze querytaal gegevens ophalen uit IoT Hub over uw apparaatdubbels en taken.

* [IoT Hub MQTT-ondersteuning](iot-hub-mqtt-support.md) vindt u meer informatie over IoT Hub-ondersteuning voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Als u wilt uitproberen enkele concepten die worden beschreven in dit artikel, raadpleegt u de volgende zelfstudie voor IoT-Hub:

* [Taken plannen en uitzenden](iot-hub-node-node-schedule-jobs.md)