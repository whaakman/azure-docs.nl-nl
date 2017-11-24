---
title: Plannen van taken met Azure IoT Hub (knooppunt) | Microsoft Docs
description: Klik hier voor meer informatie over het plannen van een taak Azure IoT Hub een directe methode op meerdere apparaten aan te roepen. U kunt Azure IoT SDK's voor Node.js gebruiken voor het implementeren van het gesimuleerde apparaat-apps en een app service de taak uit te voeren.
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/06/2017
ms.author: juanpere
ms.openlocfilehash: e607f5db8b4f2a974cb172d4581dadefe7851275
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="schedule-and-broadcast-jobs-node"></a>Planning en broadcast-taken (knooppunt)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub is een volledig beheerde service waarmee u een back-end-app maken en bijhouden van taken die gepland en miljoenen apparaten werken.  Taken kunnen worden gebruikt voor de volgende acties:

* Gewenste eigenschappen bijwerken
* Labels bijwerken
* Directe methoden aanroepen

Conceptueel gezien een taak een van deze acties worden verpakt en de voortgang van de uitvoering op basis van een verzameling apparaten, dit wordt gedefinieerd door een apparaat twin query.  Een back-endserver voor apps kunt bijvoorbeeld een taak een methode voor opnieuw opstarten op 10.000 apparaten, is opgegeven door een apparaat twin query en gepland op een later tijdstip aan te roepen.  Deze toepassing kunt vervolgens voortgang bijhouden als elk van deze apparaten ontvangen en de methode voor opnieuw opstarten worden uitgevoerd.

Meer informatie over elk van deze mogelijkheden in deze artikelen:

* Apparaat-twin en eigenschappen: [aan de slag met apparaat horende] [ lnk-get-started-twin] en [zelfstudie: het gebruik van twin apparaateigenschappen][lnk-twin-props]
* directe methoden: [IoT Hub developer guide - rechtstreekse methoden] [ lnk-dev-methods] en [zelfstudie: directe methoden][lnk-c2d-methods]

In deze handleiding ontdekt u hoe u:

* Een Node.js gesimuleerd apparaat-app met een directe methode waarmee maken **lockDoor**, die kan worden aangeroepen door de back-end oplossing.
* Maken van een Node.js-consoletoepassing roept de **lockDoor** directe methode in de gesimuleerde apparaattoepassing een taak en updates met de gewenste eigenschappen met behulp van een taak van het apparaat.

Aan het einde van deze zelfstudie hebt u twee Node.js-apps:

**simDevice.js**, die is verbonden met uw IoT-hub aan de apparaat-id en ontvangt een **lockDoor** directe methode.

**scheduleJobService.js**, die een directe methode wordt aangeroepen in de gesimuleerde apparaattoepassing en updates van de apparaat-twin gewenst eigenschappen met behulp van een taak.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Node.js-versie 4.0.x of hoger <br/>  [Uw ontwikkelingsomgeving voorbereiden] [ lnk-dev-setup] wordt beschreven hoe u Node.js voor deze zelfstudie installeren op Windows of Linux.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie maakt u een Node.js-consoletoepassing dat overeenkomt met een directe methode is aangeroepen door de cloud, die een gesimuleerde activeert **lockDoor** methode.

1. Maak een nieuwe lege map genaamd **simDevice**.  In de **simDevice** map, maakt u een package.json-bestand met de volgende opdracht achter de opdrachtprompt.  Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de **simDevice** map, voer de volgende opdracht voor het installeren van de **azure-iot-device** apparaat-SDK-pakket en **azure-iot-device-mqtt** pakket:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Start een teksteditor en maak een nieuwe **simDevice.js** bestand de **simDevice** map.
4. Voeg de volgende 'vereist' instructies aan het begin van de **simDevice.js** bestand:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Voeg een **connectionString**-variabele toe en gebruik deze om een **client**exemplaar te maken.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Voeg de volgende functie voor het afhandelen van de **lockDoor** methode.
   
    ```
    var onLockDoor = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```
7. Voeg de volgende code voor het registreren van de handler voor de **lockDoor** methode.
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub. Register handler for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```
8. Sla op en sluit de **simDevice.js** bestand.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u een beleid voor opnieuw proberen implementeren (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel [Transient Fault Handling][lnk-transient-faults] (Afhandeling van tijdelijke fouten).
> 
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Plannen van taken voor het aanroepen van een directe methode en de eigenschappen voor een apparaat-twin bijwerken
In deze sectie maakt u een Node.js-consoletoepassing die een externe Start **lockDoor** op een apparaat met een directe methode en de eigenschappen van de apparaat-twin bijwerken.

1. Maak een nieuwe lege map genaamd **scheduleJobService**.  In de **scheduleJobService** map, maakt u een package.json-bestand met de volgende opdracht achter de opdrachtprompt.  Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de **scheduleJobService** map, voer de volgende opdracht voor het installeren van de **azure-iothub** apparaat-SDK-pakket en **azure-iot-device-mqtt** pakket:
   
    ```
    npm install azure-iothub uuid --save
    ```
3. Start een teksteditor en maak een nieuwe **scheduleJobService.js** bestand de **scheduleJobService** map.
4. Voeg de volgende 'vereist' instructies aan het begin van de **dmpatterns_gscheduleJobServiceetstarted_service.js** bestand:
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```
5. Voeg de volgende variabele declaraties en vervang de tijdelijke aanduiding voor waarden:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  300;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
6. Voeg de volgende functie die wordt gebruikt voor het bewaken van de uitvoering van de taak toe:
   
    ```
    function monitorJob (jobId, callback) {
        var jobMonitorInterval = setInterval(function() {
            jobClient.getJob(jobId, function(err, result) {
            if (err) {
                console.error('Could not get job status: ' + err.message);
            } else {
                console.log('Job: ' + jobId + ' - status: ' + result.status);
                if (result.status === 'completed' || result.status === 'failed' || result.status === 'cancelled') {
                clearInterval(jobMonitorInterval);
                callback(null, result);
                }
            }
            });
        }, 5000);
    }
    ```
7. Voeg de volgende code voor het plannen van de taak die de apparaat-methode aanroept:
   
    ```
    var methodParams = {
        methodName: 'lockDoor',
        payload: null,
        responseTimeoutInSeconds: 15 // Timeout after 15 seconds if device is unable to process method
    };
   
    var methodJobId = uuid.v4();
    console.log('scheduling Device Method job with id: ' + methodJobId);
    jobClient.scheduleDeviceMethod(methodJobId,
                                queryCondition,
                                methodParams,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule device method job: ' + err.message);
        } else {
            monitorJob(methodJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor device method job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
8. Voeg de volgende code om de taak voor het bijwerken van het apparaat twin plannen:
   
    ```
    var twinPatch = {
       etag: '*', 
       properties: {
           desired: {
               building: '43', 
               floor: 3
           }
       }
    };
   
    var twinJobId = uuid.v4();
   
    console.log('scheduling Twin Update job with id: ' + twinJobId);
    jobClient.scheduleTwinUpdate(twinJobId,
                                queryCondition,
                                twinPatch,
                                startTime,
                                maxExecutionTimeInSeconds,
                                function(err) {
        if (err) {
            console.error('Could not schedule twin update job: ' + err.message);
        } else {
            monitorJob(twinJobId, function(err, result) {
                if (err) {
                    console.error('Could not monitor twin update job: ' + err.message);
                } else {
                    console.log(JSON.stringify(result, null, 2));
                }
            });
        }
    });
    ```
9. Sla op en sluit de **scheduleJobService.js** bestand.

## <a name="run-the-applications"></a>De toepassingen uitvoeren
U kunt nu de toepassingen gaan uitvoeren.

1. Bij de opdrachtprompt in de **simDevice** map, voer de volgende opdracht om te beginnen met luisteren naar de directe methode voor opnieuw opstarten.
   
    ```
    node simDevice.js
    ```
2. Bij de opdrachtprompt in de **scheduleJobService** map, voer de volgende opdracht voor het activeren van de taken voor het vergrendelen van de deur en bijwerken van de twin
   
    ```
    node scheduleJobService.js
    ```
3. U ziet de reactie van het apparaat aan de directe methode in de console.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie gebruikt u een taak om een directe methode voor een apparaat en het bijwerken van de eigenschappen van de apparaat-twin plannen.

Zie het volgende om door te gaan aan de slag met IoT Hub en device management patronen, zoals extern via de lucht firmware-update:

[Zelfstudie: Hoe u een firmware-update][lnk-fwupdate]

Zie het volgende om door te gaan aan de slag met IoT Hub [aan de slag met Azure IoT rand][lnk-iot-edge].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
