---
title: Taken plannen met Azure IoT Hub (knoop punt) | Microsoft Docs
description: Een Azure IoT Hub-taak plannen voor het aanroepen van een directe methode op meerdere apparaten. U gebruikt de Azure IoT Sdk's voor node. js om de gesimuleerde apparaat-apps en een service-app te implementeren om de taak uit te voeren.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 10/06/2017
ms.openlocfilehash: 243f4e63cc04bca018c2bf69492dccf163e92b73
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780829"
---
# <a name="schedule-and-broadcast-jobs-nodejs"></a>Taken plannen en uitzenden (node. js)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub is een volledig beheerde service waarmee een back-end-app taken kan maken en bijhouden waarmee miljoenen apparaten kunnen worden gepland en bijgewerkt.  Taken kunnen worden gebruikt voor de volgende acties:

* Gewenste eigenschappen bijwerken
* Tags bijwerken
* Directe methoden aanroepen

Met een taak wordt een van deze acties gewikkeld en wordt de voortgang van de uitvoering van een set apparaten getraceerd, die wordt gedefinieerd door een dubbele query voor een apparaat.  Een back-end-app kan bijvoorbeeld een taak gebruiken om een methode voor opnieuw opstarten op te roepen op 10.000-apparaten, opgegeven door een dubbele query van een apparaat en in een later tijdstip te worden gepland. Deze toepassing kan vervolgens de voortgang volgen wanneer deze apparaten de methode voor opnieuw opstarten ontvangen en uitvoeren.

Meer informatie over elk van deze mogelijkheden vindt u in de volgende artikelen:

* Dubbele en eigenschappen van apparaat: [Aan de slag met apparaatdubbels](iot-hub-node-node-twin-getstarted.md) en [zelf studie: De dubbele eigenschappen van een apparaat gebruiken](tutorial-device-twins.md)

* Directe methoden: [IOT hub ontwikkelaars handleiding-directe methoden](iot-hub-devguide-direct-methods.md) en [zelf studie: directe methoden](quickstart-control-device-node.md)

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze zelfstudie ontdekt u hoe u:

* Maak een gesimuleerde node. js-app met een directe methode waarmee **lockDoor**wordt ingeschakeld. deze kan worden aangeroepen door de back-end van de oplossing.

* Maak een node. JS-Console-app die de **lockDoor** direct-methode aanroept in de gesimuleerde apparaat-app met behulp van een taak en de gewenste eigenschappen bijwerkt met behulp van een apparaat taak.

Aan het einde van deze zelf studie hebt u twee node. js-apps:

* **simDevice. js**, dat verbinding maakt met uw IOT-hub met de apparaat-id en een **lockDoor** directe methode ontvangt.

* **scheduleJobService. js**, waarmee een directe methode wordt aangeroepen in de gesimuleerde apparaat-app en de gewenste eigenschappen van het apparaat worden bijgewerkt met behulp van een taak.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Node. js versie 10.0. x of hoger [uw ontwikkel omgeving voorbereiden](https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md) hierin wordt beschreven hoe u node. js voor deze zelf studie installeert op Windows of Linux.

* Een actief Azure-account. (Als u geen account hebt, kunt u in slechts een paar minuten een [gratis account](https://azure.microsoft.com/pricing/free-trial/) maken.)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Een nieuw apparaat registreren in de IoT-hub

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken

In deze sectie maakt u een node. JS-Console-app die reageert op een directe methode die wordt aangeroepen door de Cloud, waardoor een gesimuleerde **lockDoor** -methode wordt geactiveerd.

1. Maak een nieuwe lege map met de naam **simDevice**.  Maak in de map **simDevice** een package. JSON-bestand met de volgende opdracht achter de opdracht prompt.  Accepteer alle standaardwaarden:

   ```
   npm init
   ```

2. Voer bij de opdracht prompt in de map **simDevice** de volgende opdracht uit om het **Azure-IOT-Device-SDK-** pakket te installeren en **Azure-IOT-Device-mqtt** -pakket:
   
   ```
   npm install azure-iot-device azure-iot-device-mqtt --save
   ```

3. Maak een nieuw **simDevice. js** -bestand in de map **simDevice** met behulp van een tekst editor.

4. Voeg de volgende ' vereist '-instructies toe aan het begin van het bestand **simDevice. js** :
   
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

6. Voeg de volgende functie toe om de methode **lockDoor** te verwerken.
   
    ```
    var onLockDoor = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```

7. Voeg de volgende code toe om de handler voor de methode **lockDoor** te registreren.

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

8. Sla het bestand **simDevice. js** op en sluit het.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. In productie code moet u beleid voor opnieuw proberen implementeren (zoals een exponentiële uitstel), zoals wordt voorgesteld in het artikel, [tijdelijke fout afhandeling](/azure/architecture/best-practices/transient-faults).
>

## <a name="get-the-iot-hub-connection-string"></a>De IoT hub-connection string ophalen

[!INCLUDE [iot-hub-howto-schedule-jobs-shared-access-policy-text](../../includes/iot-hub-howto-schedule-jobs-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-registryrw-connection-string](../../includes/iot-hub-include-find-registryrw-connection-string.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Taken plannen voor het aanroepen van een directe methode en het bijwerken van de eigenschappen van een apparaat met dubbele gegevens

In deze sectie maakt u een node. JS-Console-app die een externe **lockDoor** op een apparaat initieert met behulp van een directe methode en de eigenschappen van het apparaat twee bijwerkt.

1. Maak een nieuwe lege map met de naam **scheduleJobService**.  Maak in de map **scheduleJobService** een package. JSON-bestand met de volgende opdracht achter de opdracht prompt.  Accepteer alle standaardwaarden:

    ```
    npm init
    ```

2. Voer bij de opdracht prompt in de map **scheduleJobService** de volgende opdracht uit om het **Azure-iothub** apparaat SDK-pakket en het **Azure-IOT-Device-mqtt** -pakket te installeren:
   
    ```
    npm install azure-iothub uuid --save
    ```

3. Maak een nieuw **scheduleJobService. js** -bestand in de map **scheduleJobService** met behulp van een tekst editor.

4. Voeg de volgende ' vereist '-instructies toe aan het begin van het bestand **dmpatterns_gscheduleJobServiceetstarted_service. js** :
   
    ```
    'use strict';
   
    var uuid = require('uuid');
    var JobClient = require('azure-iothub').JobClient;
    ```

5. Voeg de volgende variabelen declaraties toe en vervang de waarden van de tijdelijke aanduiding:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var queryCondition = "deviceId IN ['myDeviceId']";
    var startTime = new Date();
    var maxExecutionTimeInSeconds =  300;
    var jobClient = JobClient.fromConnectionString(connectionString);
    ```

6. Voeg de volgende functie toe die wordt gebruikt om de uitvoering van de taak te bewaken:
   
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

7. Voeg de volgende code toe om de taak te plannen die de methode van het apparaat aanroept:
   
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

8. Voeg de volgende code toe om de taak te plannen om het apparaat bij te werken:
   
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

9. Sla het bestand **scheduleJobService. js** op en sluit het.

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U kunt nu de toepassingen gaan uitvoeren.

1. Voer bij de opdracht prompt in de map **simDevice** de volgende opdracht uit om te beginnen met Luis teren naar de methode voor direct opnieuw opstarten.
   
    ```
    node simDevice.js
    ```

2. Voer bij de opdracht prompt in de map **scheduleJobService** de volgende opdracht uit om de taken te activeren om de deur te vergren delen en de dubbele
   
    ```
    node scheduleJobService.js
    ```

3. U ziet de reactie van het apparaat op de directe methode in de-console.

## <a name="next-steps"></a>Volgende stappen

In deze zelf studie hebt u een taak gebruikt voor het plannen van een directe methode op een apparaat en het bijwerken van de eigenschappen van het apparaat.

Zie [zelf studie als u aan de slag wilt gaan met IOT hub-en Apparaatbeheer patronen zoals extern via de firmware-update. Een firmware-update](tutorial-firmware-update.md)uitvoeren.

Zie aan de slag [met Azure IOT Edge](../iot-edge/tutorial-simulate-device-linux.md)om aan de slag te gaan met IOT hub.
