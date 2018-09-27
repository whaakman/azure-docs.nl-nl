---
title: Plannen van taken met Azure IoT Hub (Node) | Microsoft Docs
description: Klik hier voor meer informatie over het plannen van een taak is Azure IoT Hub een rechtstreekse methode aanroepen op meerdere apparaten. U de Azure IoT SDK's voor Node.js gebruiken voor het implementeren van de gesimuleerde apparaat-apps en een app service de taak uit te voeren.
author: juanjperez
manager: cberlin
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 10/06/2017
ms.author: juanpere
ms.openlocfilehash: 69469d6b302f951301c92c0ee7984df95911624e
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47221295"
---
# <a name="schedule-and-broadcast-jobs-node"></a>Taken plannen en uitzenden (Node)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub is een volledig beheerde service waarmee u een back-end-app voor het maken en bijhouden van taken die plannen en bijwerken van miljoenen apparaten.  Taken kunnen worden gebruikt voor de volgende acties:

* Gewenste eigenschappen bijwerken
* Bijwerken van tags
* Directe methoden aanroepen

Conceptueel gezien, een taak een van deze acties wordt verpakt en wordt de voortgang van de uitvoering op basis van een set apparaten dat is gedefinieerd door een dubbele apparaatquery bijgehouden.  Een back-end-app kan bijvoorbeeld een taak gebruiken om aan te roepen een methode voor opnieuw opstarten op 10.000 apparaten, opgegeven door een dubbele apparaatquery en gepland op een later tijdstip.  Deze toepassing kunt vervolgens voortgang bijhouden als elk van deze apparaten ontvangen en de methode voor opnieuw opstarten worden uitgevoerd.

Meer informatie over elk van deze mogelijkheden in deze artikelen:

* Apparaatdubbel en eigenschappen: [aan de slag met apparaatdubbels] [ lnk-get-started-twin] en [zelfstudie: apparaatdubbeleigenschappen gebruiken][lnk-twin-props]
* Directe methoden: [het Ontwikkelaarshandleiding voor IoT Hub - directe methoden] [ lnk-dev-methods] en [zelfstudie: directe methoden][lnk-c2d-methods]

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze zelfstudie ontdekt u hoe u:

* Maken van een gesimuleerde apparaat-app voor Node.js met een rechtstreekse methode waarmee **lockDoor**, die kan worden aangeroepen in de back-end van de oplossing.
* Een Node.js-consoletoepassing die roept de **lockDoor** directe methode in het gesimuleerde apparaat-app met behulp van een taak en updates de gewenste eigenschappen met behulp van een apparaattaak.

Aan het einde van deze zelfstudie hebt u twee Node.js-apps:

**simDevice.js**, die verbinding maakt met uw IoT-hub aan de apparaat-id en ontvangt een **lockDoor** directe methode.

**scheduleJobService.js**, die een rechtstreekse methode aanroepen in het gesimuleerde apparaat-app en werkt het dubbele apparaat gewenste eigenschappen met behulp van een taak.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Node.js versie 4.0.x of hoger <br/>  [Uw ontwikkelomgeving voorbereiden] [ lnk-dev-setup] wordt beschreven hoe u Node.js voor deze zelfstudie installeren op Windows of Linux.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie maakt u een Node.js-consoletoepassing die reageert op een rechtstreekse methode aangeroepen door de cloud, waartoe een gesimuleerde activeert **lockDoor** methode.

1. Maak een nieuwe lege map genaamd **simDevice**.  In de **simDevice** map, maakt u een package.json-bestand met de volgende opdracht in uw opdrachtvenster.  Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. In het opdrachtprompt in de **simDevice** map, voer de volgende opdracht voor het installeren van de **azure-iot-device** Device SDK-pakket en **azure-iot-device-mqtt** pakket:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Maak met een teksteditor een nieuw **simDevice.js** -bestand in de **simDevice** map.
4. Voeg de volgende 'require' instructies toe aan het begin van de **simDevice.js** bestand:
   
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
8. Opslaan en sluiten de **simDevice.js** bestand.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals aangegeven in het artikel implementeren [afhandeling van tijdelijke fouten](/azure/architecture/best-practices/transient-faults).
> 
> 

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Taken plannen voor een rechtstreekse methode aanroepen en het bijwerken van eigenschappen van een apparaatdubbel
In deze sectie maakt u een Node.js-consoletoepassing die een externe initieert **lockDoor** op een apparaat met een rechtstreekse methode en eigenschappen van het dubbele apparaat bijwerken.

1. Maak een nieuwe lege map genaamd **scheduleJobService**.  In de **scheduleJobService** map, maakt u een package.json-bestand met de volgende opdracht in uw opdrachtvenster.  Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. In het opdrachtprompt in de **scheduleJobService** map, voer de volgende opdracht voor het installeren van de **azure-iothub** Device SDK-pakket en **azure-iot-device-mqtt** pakket:
   
    ```
    npm install azure-iothub uuid --save
    ```
3. Maak met een teksteditor een nieuw **scheduleJobService.js** -bestand in de **scheduleJobService** map.
4. Voeg de volgende 'require' instructies toe aan het begin van de **dmpatterns_gscheduleJobServiceetstarted_service.js** bestand:
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```
5. Voeg de volgende variabelendeclaraties toe en vervang de tijdelijke aanduiding voor waarden:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  300;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```
6. Voeg de volgende functie die wordt gebruikt voor het bewaken van de uitvoering van de taak:
   
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
7. Voeg de volgende code om te plannen van de taak die de apparaatmethode aanroept:
   
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
8. Voeg de volgende code om te plannen van de taak voor het bijwerken van het dubbele apparaat:
   
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
9. Opslaan en sluiten de **scheduleJobService.js** bestand.

## <a name="run-the-applications"></a>De toepassingen uitvoeren
U kunt nu de toepassingen gaan uitvoeren.

1. Bij de opdrachtprompt in de **simDevice** map, de volgende opdracht uit om te luisteren naar de directe methode die opnieuw worden opgestart.
   
    ```
    node simDevice.js
    ```
2. Bij de opdrachtprompt in de **scheduleJobService** map, voer de volgende opdracht voor het activeren van de taken voor het vergrendelen van de klep en het dubbele bijwerken
   
    ```
    node scheduleJobService.js
    ```
3. U ziet dat de reactie van het apparaat naar de directe methode die in de console.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie gebruikt u een taak voor het plannen van een rechtstreekse methode aan een apparaat en het bijwerken van eigenschappen van het dubbele apparaat.

Zie het volgende om door te gaan aan de slag met IoT Hub en patronen voor Apparaatbeheer zoals het op afstand via de lucht firmware-update:

[Zelfstudie: Hoe u doet een firmware-update][lnk-fwupdate]

Om door te gaan aan de slag met IoT Hub [aan de slag met Azure IoT Edge][lnk-iot-edge].

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: tutorial-device-twins.md
[lnk-c2d-methods]: quickstart-control-device-node.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: tutorial-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
