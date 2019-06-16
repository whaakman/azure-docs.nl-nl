---
title: Informatie over Azure IoT Hub-taken | Microsoft Docs
description: Handleiding voor ontwikkelaars - plannen van taken om uit te voeren op meerdere apparaten worden verbonden met uw IoT-hub. Taken kunnen labels en gewenste eigenschappen bijwerken en directe methoden aanroepen op meerdere apparaten.
author: robinsh
manager: philmea
ms.author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/06/2019
ms.openlocfilehash: 147dd0f454bd85673bcba5cd6148c5da9716c580
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65409066"
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

{
    "jobId": "<jobId>",
    "type": "scheduleDeviceMethod",
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

Het volgende fragment toont de aanvraag en respons voor een taak is gepland om aan te roepen een rechtstreekse methode met de naam testMethod op alle apparaten in de contoso-hub-1:

```
PUT https://contoso-hub-1.azure-devices.net/jobs/v2/job01?api-version=2018-06-30 HTTP/1.1
Authorization: SharedAccessSignature sr=contoso-hub-1.azure-devices.net&sig=68iv------------------------------------v8Hxalg%3D&se=1556849884&skn=iothubowner
Content-Type: application/json; charset=utf-8
Host: contoso-hub-1.azure-devices.net
Content-Length: 317

{
    "jobId": "job01",
    "type": "scheduleDeviceMethod",
    "cloudToDeviceMethod": {
        "methodName": "testMethod",
        "payload": {},
        "responseTimeoutInSeconds": 30
    },
    "queryCondition": "*", 
    "startTime": "2019-05-04T15:53:00.077Z",
    "maxExecutionTimeInSeconds": 20
}

HTTP/1.1 200 OK
Content-Length: 65
Content-Type: application/json; charset=utf-8
Vary: Origin
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 03 May 2019 01:46:18 GMT

{"jobId":"job01","type":"scheduleDeviceMethod","status":"queued"}
```

## <a name="jobs-to-update-device-twin-properties"></a>Taken apparaatdubbeleigenschappen bijwerken

Het volgende fragment toont de details van de aanvraag HTTPS 1.1 voor het bijwerken van apparaatdubbeleigenschappen met behulp van een taak:

```
PUT /jobs/v2/<jobId>?api-version=2018-06-30

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8

{
    "jobId": "<jobId>",
    "type": "scheduleUpdateTwin",
    "updateTwin": <patch>                 // Valid JSON object
    "queryCondition": "<queryOrDevices>", // query condition
    "startTime": <jobStartTime>,          // as an ISO-8601 date string
    "maxExecutionTimeInSeconds": <maxExecutionTimeInSeconds>
}
```

> [!NOTE]
> De *updateTwin* eigenschap moet een geldige etag-overeenkomst, bijvoorbeeld `etag="*"`.

Het volgende fragment toont de aanvraag en respons voor een taak is gepland om bij te werken apparaatdubbeleigenschappen voor test-apparaat op contoso-hub-1:

```
PUT https://contoso-hub-1.azure-devices.net/jobs/v2/job02?api-version=2018-06-30 HTTP/1.1
Authorization: SharedAccessSignature sr=contoso-hub-1.azure-devices.net&sig=BN0U-------------------------------------RuA%3D&se=1556925787&skn=iothubowner
Content-Type: application/json; charset=utf-8
Host: contoso-hub-1.azure-devices.net
Content-Length: 339

{
    "jobId": "job02",
    "type": "scheduleUpdateTwin",
    "updateTwin": {
      "properties": {
        "desired": {
          "test1": "value1"
        }
      },
     "etag": "*"
     },
    "queryCondition": "deviceId = 'test-device'",
    "startTime": "2019-05-08T12:19:56.868Z",
    "maxExecutionTimeInSeconds": 20
}

HTTP/1.1 200 OK
Content-Length: 63
Content-Type: application/json; charset=utf-8
Vary: Origin
Server: Microsoft-HTTPAPI/2.0
Date: Fri, 03 May 2019 22:45:13 GMT

{"jobId":"job02","type":"scheduleUpdateTwin","status":"queued"}
```

## <a name="querying-for-progress-on-jobs"></a>Query's uitvoeren voor de voortgang van taken

Het volgende fragment toont de details van de aanvraag HTTPS 1.1 voor het uitvoeren van query's voor taken:

```
GET /jobs/v2/query?api-version=2018-06-30[&jobType=<jobType>][&jobStatus=<jobStatus>][&pageSize=<pageSize>][&continuationToken=<continuationToken>]

Authorization: <config.sharedAccessSignature>
Content-Type: application/json; charset=utf-8
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
| | **scheduleUpdateTwin**: Een taak die wordt gebruikt voor het bijwerken van een set van gewenste eigenschappen of labels. |
| | **scheduleDeviceMethod**: Een taak die wordt gebruikt om aan te roepen een apparaatmethode op een set dubbele apparaten. |
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