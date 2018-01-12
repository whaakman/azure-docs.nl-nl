---
title: Aan de slag met Azure IoT Hub Apparaatbeheer (knooppunt) | Microsoft Docs
description: "Het externe apparaat opnieuw opstarten initiëren met Apparaatbeheer via IoT Hub. U kunt de Azure IoT SDK voor Node.js gebruiken voor het implementeren van een gesimuleerde apparaattoepassing die een directe methode bevat en een service-app die de directe methode aanroept."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: juanpere
ms.openlocfilehash: 0aeba8560de46ae2032304f133b66ad0a70f1354
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="get-started-with-device-management-node"></a>Aan de slag met Apparaatbeheer (knooppunt)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

In deze handleiding ontdekt u hoe u:

* De Azure portal gebruiken voor het maken van een IoT-Hub en een apparaat-id maakt in uw IoT-hub.
* Maak een gesimuleerde apparaattoepassing met een directe methode die het apparaat opnieuw wordt opgestart. Rechtstreekse methoden worden aangeroepen vanuit de cloud.
* Een Node.js-consoletoepassing die de directe methode voor opnieuw opstarten in de gesimuleerde apparaattoepassing via uw IoT-hub maken.

Aan het einde van deze zelfstudie hebt u twee console Node.js-apps:

**dmpatterns_getstarted_device.js**, die verbinding maakt met uw IoT-hub aan de apparaat-id eerder hebt gemaakt, ontvangt u een directe methode voor opnieuw opstarten, simuleert fysieke opnieuw worden opgestart en rapporten van de tijd voor de laatste keer opnieuw opstarten.

**dmpatterns_getstarted_service.js**, die een directe methode wordt aangeroepen in de gesimuleerde apparaattoepassing geeft het antwoord weer en geeft de bijgewerkte eigenschappen gerapporteerd.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Node.js-versie 4.0.x of hoger <br/>  [Uw ontwikkelingsomgeving voorbereiden] [ lnk-dev-setup] wordt beschreven hoe u Node.js voor deze zelfstudie installeren op Windows of Linux.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie wordt u

* U maakt een Node.js-console-app die reageert op een directe methode die door de cloud wordt aangeroepen.
* Een gesimuleerd apparaat opnieuw activeren
* Gebruik de gemelde eigenschappen om het apparaat twin query's om apparaten te identificeren en wanneer ze het laatst opnieuw opgestart

1. U maakt een lege map met de naam **simulateddevice**.  Maak in de map **simulateddevice** een bestand met de naam package.json door achter de opdrachtprompt de volgende opdracht op te geven.  Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de **manageddevice** map, voer de volgende opdracht voor het installeren van de **azure-iot-device** apparaat-SDK-pakket en **azure-iot-device-mqtt** pakket:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Maak met een teksteditor, een **dmpatterns_getstarted_device.js** bestand de **manageddevice** map.
4. Voeg de volgende 'vereist' instructies aan het begin van de **dmpatterns_getstarted_device.js** bestand:
   
    ```
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```
5. Voeg een **connectionString**-variabele toe en gebruik deze om een **client**exemplaar te maken.  De verbindingsreeks vervangen door de verbindingsreeks van uw apparaat.  
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myDeviceId;SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```
6. De volgende functie voor het implementeren van de directe methode op het apparaat toevoegen
   
    ```
    var onReboot = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, 'Reboot started', function(err) {
            if (err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
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
7. Maak verbinding met uw IoT-hub en start de listener directe methode:
   
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
8. Sla op en sluit de **dmpatterns_getstarted_device.js** bestand.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u een beleid voor opnieuw proberen implementeren (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel [Transient Fault Handling][lnk-transient-faults] (Afhandeling van tijdelijke fouten).

## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Activeren van een extern opnieuw opstarten op het apparaat met een directe methode
In deze sectie maakt maken u een Node.js-consoletoepassing die start een extern opnieuw opstarten op een apparaat met een directe methode. De app gebruikmaakt van apparaat twin query's voor het detecteren van de laatste keer opnieuw opstarten voor dat apparaat.

1. Maak een lege map genaamd **triggerrebootondevice**.  In de **triggerrebootondevice** map, maakt u een package.json-bestand met de volgende opdracht achter de opdrachtprompt.  Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de **triggerrebootondevice** map, voer de volgende opdracht voor het installeren van de **azure-iothub** apparaat-SDK-pakket en **azure-iot-device-mqtt** pakket:
   
    ```
    npm install azure-iothub --save
    ```
3. Maak met een teksteditor, een **dmpatterns_getstarted_service.js** bestand de **triggerrebootondevice** map.
4. Voeg de volgende 'vereist' instructies aan het begin van de **dmpatterns_getstarted_service.js** bestand:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Voeg de volgende variabele declaraties en vervang de tijdelijke aanduiding voor waarden:
   
    ```
    var connectionString = '{iothubconnectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToReboot = 'myDeviceId';
    ```
6. De volgende functie voor het aanroepen van de methode van het apparaat opnieuw opstarten van het doelapparaat toevoegen:
   
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
7. Voeg de volgende functie om een query voor het apparaat en krijgt u de laatste keer dat opnieuw opstarten:
   
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
8. Voeg de volgende code voor het aanroepen van de functies die de directe methode voor opnieuw opstarten en de query voor het laatst opnieuw activeren:
   
    ```
    startRebootDevice();
    setInterval(queryTwinLastReboot, 2000);
    ```
9. Sla op en sluit de **dmpatterns_getstarted_service.js** bestand.

## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.

1. Bij de opdrachtprompt in de **manageddevice** map, voer de volgende opdracht om te beginnen met luisteren naar de directe methode voor opnieuw opstarten.
   
    ```
    node dmpatterns_getstarted_device.js
    ```
2. Bij de opdrachtprompt in de **triggerrebootondevice** map, voer de volgende opdracht de extern opnieuw opstarten en de query voor het apparaat vinden van de laatste keer opnieuw activeren.
   
    ```
    node dmpatterns_getstarted_service.js
    ```
3. U ziet de reactie van het apparaat aan de directe methode in de console.

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
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
