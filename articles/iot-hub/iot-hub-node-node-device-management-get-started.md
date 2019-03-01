---
title: Aan de slag met Azure IoT Hub device management (Node) | Microsoft Docs
description: Het gebruik van Apparaatbeheer via IoT Hub om te beginnen met een extern apparaat opnieuw opstarten. U de Azure IoT-SDK voor Node.js gebruiken voor het implementeren van een gesimuleerde apparaat-app met een rechtstreekse methode en een service-app die de directe methode aanroept.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/25/2017
ms.openlocfilehash: 3f861259c8c760b921f1c33f4e449853a3686a40
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57010800"
---
# <a name="get-started-with-device-management-node"></a>Aan de slag met Apparaatbeheer (Node)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

In deze zelfstudie ontdekt u hoe u:

* De Azure portal gebruiken voor het maken van een IoT-Hub en een apparaat-id maken in uw IoT-hub.
* Maak een gesimuleerde apparaat-app met een rechtstreekse methode die het apparaat opnieuw wordt opgestart. Directe methoden zijn aangeroepen vanuit de cloud.
* Maak een Node.js-consoletoepassing die de directe methode voor opnieuw opstarten in de gesimuleerde apparaattoepassing via uw IoT-hub roept.

Aan het einde van deze zelfstudie, beschikt u over twee Node.js-consoletoepassingen:

**dmpatterns_getstarted_device.js**, deze toepassing koppelt uw IoT-hub aan de apparaat-id die eerder hebt gemaakt, ontvangt u een rechtstreekse methode voor opnieuw opstarten, simuleert fysieke opnieuw worden opgestart en rapporten van de tijd voor de laatste keer opnieuw opstarten.

**dmpatterns_getstarted_service.js**, die een rechtstreekse methode aanroepen in het gesimuleerde apparaat-app, wordt het antwoord weergegeven en wordt de bijgewerkte gerapporteerde eigenschappen.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Node.js versie 4.0.x of hoger <br/>  [Uw ontwikkelomgeving voorbereiden] [ lnk-dev-setup] wordt beschreven hoe u Node.js voor deze zelfstudie installeren op Windows of Linux.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Verbindingsreeks voor IoT-hub ophalen

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie wordt u

* U maakt een Node.js-console-app die reageert op een directe methode die door de cloud wordt aangeroepen.
* Activeren van een gesimuleerd apparaat opnieuw opstarten
* Gebruik van de gerapporteerde eigenschappen om te schakelen apparaatdubbel-query's om apparaten te identificeren en wanneer ze het laatst opnieuw opgestart

1. U maakt een lege map met de naam **simulateddevice**.  Maak in de map **simulateddevice** een bestand met de naam package.json door achter de opdrachtprompt de volgende opdracht op te geven.  Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. In het opdrachtprompt in de **simulateddevice** map, voer de volgende opdracht voor het installeren van de **azure-iot-device** Device SDK-pakket en **azure-iot-device-mqtt** pakket:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Maak met een teksteditor een **dmpatterns_getstarted_device.js** -bestand in de **simulateddevice** map.
4. Voeg de volgende 'require' instructies toe aan het begin van de **dmpatterns_getstarted_device.js** bestand:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Voeg een **connectionString**-variabele toe en gebruik deze om een **client**exemplaar te maken.  Vervang de verbindingsreeks door de verbindingsreeks van uw apparaat.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. Voeg de volgende functie voor het implementeren van de directe methode die op het apparaat
   
    ```
    var onReboot = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (err) {
                console.error('An error occurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        // Report the reboot before the physical restart
        var date = new Date();
        var patch = {
            iothubDM : {
                reboot : {
                    lastReboot : date.toISOString(),
                }
            }
        };
   
        // Get device Twin
        client.getTwin(function(err, twin) {
            if (err) {
                console.error('could not get twin');
            } else {
                console.log('twin acquired');
                twin.properties.reported.update(patch, function(err) {
                    if (err) throw err;
                    console.log('Device reboot twin state reported')
                });  
            }
        });
   
        // Add your device's reboot API for physical restart.
        console.log('Rebooting!');
    };
    ```
7. Maak verbinding met uw IoT-hub en de directe methode listener starten:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('Could not open IotHub client');
        }  else {
            console.log('Client opened.  Waiting for reboot method.');
            client.onDeviceMethod('reboot', onReboot);
        }
    });
    ```
8. Opslaan en sluiten de **dmpatterns_getstarted_device.js** bestand.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals aangegeven in het artikel implementeren [afhandeling van tijdelijke fouten](/azure/architecture/best-practices/transient-faults).

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Een extern opnieuw opstarten op het apparaat met een rechtstreekse methode activeren
In deze sectie maakt u een Node.js-consoletoepassing die een extern opnieuw opstarten op een apparaat met een rechtstreekse methode initieert. De app maakt gebruik van apparaatdubbel-query's voor het detecteren van de laatste keer opnieuw opstarten voor dat apparaat.

1. Maak een lege map genaamd **triggerrebootondevice**.  In de **triggerrebootondevice** map, maakt u een package.json-bestand met de volgende opdracht in uw opdrachtvenster.  Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. In het opdrachtprompt in de **triggerrebootondevice** map, voer de volgende opdracht voor het installeren van de **azure-iothub** Device SDK-pakket en **azure-iot-device-mqtt** pakket:
   
    ```
    npm install azure-iothub --save
    ```
3. Maak met een teksteditor een **dmpatterns_getstarted_service.js** -bestand in de **triggerrebootondevice** map.
4. Voeg de volgende 'require' instructies toe aan het begin van de **dmpatterns_getstarted_service.js** bestand:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Voeg de volgende variabelendeclaraties toe en vervang de tijdelijke aanduiding voor waarden:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
6. Voeg de volgende functie voor het aanroepen van de apparaatmethode opnieuw opstarten van het doelapparaat:
   
    ```
    var startRebootDevice = function(twin) {
   
        var methodName = "reboot";
   
        var methodParams = {
            methodName: methodName,
            payload: null,
            timeoutInSeconds: 30
        };
   
        client.invokeDeviceMethod(deviceToReboot, methodParams, function(err, result) {
            if (err) { 
                console.error("Direct method error: "+err.message);
            } else {
                console.log("Successfully invoked the device to reboot.");  
            }
        });
    };
    ```
7. Voeg de volgende functie om te zoeken naar het apparaat en de laatste keer dat opnieuw opstarten:
   
    ```
    var queryTwinLastReboot = function() {
   
        registry.getTwin(deviceToReboot, function(err, twin){
   
            if (twin.properties.reported.iothubDM != null)
            {
                if (err) {
                    console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
                } else {
                    var lastRebootTime = twin.properties.reported.iothubDM.reboot.lastReboot;
                    console.log('Last reboot time: ' + JSON.stringify(lastRebootTime, null, 2));
                }
            } else 
                console.log('Waiting for device to report last reboot time.');
        });
    };
    ```
8. Voeg de volgende code voor het aanroepen van de functies die de directe methode voor opnieuw opstarten en de query is geactiveerd voor de laatste keer dat opnieuw opstarten:
   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
9. Opslaan en sluiten de **dmpatterns_getstarted_service.js** bestand.

## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.

1. Bij de opdrachtprompt in de **simulateddevice** map, de volgende opdracht uit om te luisteren naar de directe methode die opnieuw worden opgestart.
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. Bij de opdrachtprompt in de **triggerrebootondevice** map, voer de volgende opdracht de extern opnieuw opstarten en de query voor het dubbele apparaat vinden van de laatste keer opnieuw activeren.
   
    ```
    node dmpatterns_getstarted_service.js
    ```
3. U ziet dat de reactie van het apparaat naar de directe methode die in de console.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
