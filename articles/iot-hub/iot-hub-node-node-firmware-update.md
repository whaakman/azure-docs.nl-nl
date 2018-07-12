---
title: Firmware-update van het apparaat met Azure IoT Hub (Node) | Microsoft Docs
description: Het beheer van apparaten op Azure IoT Hub gebruiken om een firmware-update van het apparaat. U de Azure IoT SDK's voor Node.js gebruiken om een gesimuleerde apparaat-app en een service-app die wordt geactiveerd de firmware-update te implementeren.
author: juanjperez
manager: cberlin
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/07/2017
ms.author: juanpere
ms.openlocfilehash: 0cd8c019cf9a65e0e72227ba99c1995a45ed4067
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38452427"
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-nodenode"></a>Apparaatbeheer gebruiken om een apparaat firmware-update (knooppunt/Node)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

In de [aan de slag met Apparaatbeheer] [ lnk-dm-getstarted] zelfstudie, hebt u gezien hoe u de [apparaatdubbel] [ lnk-devtwin] en [directe methoden ] [ lnk-c2dmethod] primitieven aan een apparaat op afstand opnieuw opstarten. In deze zelfstudie maakt gebruik van de dezelfde IoT Hub-primitieven en bevat richtlijnen en ziet u hoe u een end-to-end gesimuleerde firmware-update doet.  Dit patroon wordt gebruikt in de firmware-update-implementatie voor de Intel Edison-apparaat maakt.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze handleiding ontdekt u hoe u:

* Een Node.js-consoletoepassing die de directe methode die firmwareUpdate in de gesimuleerde apparaattoepassing via uw IoT-hub aanroept maken.
* Maakt een gesimuleerde apparaat-app die u implementeert een **firmwareUpdate** directe methode. Deze methode initieert een proces met meerdere fasen dat wacht om te downloaden van de firmware-installatiekopie, wordt de firmware-installatiekopie gedownload en ten slotte past de firmware-installatiekopie. Tijdens elke fase van de update wordt het apparaat gebruikt van de gerapporteerde eigenschappen om te rapporteren over hun voortgang.

Aan het einde van deze zelfstudie, beschikt u over twee Node.js-consoletoepassingen:

**dmpatterns_fwupdate_service.js**, die een rechtstreekse methode aanroepen in het gesimuleerde apparaat-app, wordt het antwoord weergegeven en periodiek (elke 500ms) geeft de bijgewerkte gerapporteerde eigenschappen.

**dmpatterns_fwupdate_device.js**, deze toepassing koppelt uw IoT-hub aan de apparaat-id die eerder hebt gemaakt, ontvangt u een rechtstreekse methode firmwareUpdate, wordt uitgevoerd via een proces met meerdere staat voor het simuleren van een firmware-update inclusief: wachten op de installatiekopie Download de nieuwe installatiekopie te downloaden en ten slotte de installatiekopie toe te passen.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Node.js versie 4.0.x of hoger <br/>  [Uw ontwikkelomgeving voorbereiden] [ lnk-dev-setup] wordt beschreven hoe u Node.js voor deze zelfstudie installeren op Windows of Linux.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

Ga als volgt de [aan de slag met Apparaatbeheer](iot-hub-node-node-device-management-get-started.md) artikel om te maken van uw IoT-hub en uw IoT Hub-verbindingsreeks ophalen.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Activeren van een externe firmware-update op het apparaat met een rechtstreekse methode
In deze sectie maakt u een Node.js-consoletoepassing die een externe firmware-update op een apparaat initieert. De app maakt gebruik van een rechtstreekse methode om de update te initiëren en apparaatdubbel-query's gebruikt om periodiek de status van de actieve firmware-update.

1. Maak een lege map genaamd **triggerfwupdateondevice**.  In de **triggerfwupdateondevice** map, maakt u een package.json-bestand met de volgende opdracht in uw opdrachtvenster.  Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. In het opdrachtprompt in de **triggerfwupdateondevice** map, voer de volgende opdracht voor het installeren van de **azure-iot-hub** pakket:
   
    ```
    npm install azure-iothub --save
    ```
3. Maak met een teksteditor een **dmpatterns_getstarted_service.js** -bestand in de **triggerfwupdateondevice** map.
4. Voeg de volgende 'require' instructies toe aan het begin van de **dmpatterns_getstarted_service.js** bestand:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Voeg de volgende variabelendeclaraties toe en vervang de tijdelijke aanduiding voor waarden:
   
    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
6. Voeg de volgende functie om te zoeken en de waarde van de firmwareUpdate weer te geven gerapporteerde eigenschap.
   
    ```
    var queryTwinFWUpdateReported = function() {
        registry.getTwin(deviceToUpdate, function(err, twin){
            if (err) {
              console.error('Could not query twins: ' + err.constructor.name + ': ' + err.message);
            } else {
              console.log((JSON.stringify(twin.properties.reported.iothubDM.firmwareUpdate)) + "\n");
            }
        });
    };
    ```
7. Voeg de volgende functie voor het aanroepen van de methode firmwareUpdate opnieuw opstarten van het doelapparaat:
   
    ```
    var startFirmwareUpdateDevice = function() {
      var params = {
          fwPackageUri: 'https://secureurl'
      };
   
      var methodName = "firmwareUpdate";
      var payloadData =  JSON.stringify(params);
   
      var methodParams = {
        methodName: methodName,
        payload: payloadData,
        timeoutInSeconds: 30
      };
   
      client.invokeDeviceMethod(deviceToUpdate, methodParams, function(err, result) {
        if (err) {
          console.error('Could not start the firmware update on the device: ' + err.message)
        } 
      });
    };
    ```
8. Voeg ten slotte de volgende functie toe aan code voor het starten van de firmware-update-reeks en periodiek wordt weergegeven de gerapporteerde eigenschappen:
   
    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
9. Opslaan en sluiten de **dmpatterns_fwupdate_service.js** bestand.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.

1. Bij de opdrachtprompt in de **simulateddevice** map, de volgende opdracht uit om te luisteren naar de directe methode die opnieuw worden opgestart.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. Bij de opdrachtprompt in de **triggerfwupdateondevice** map, voer de volgende opdracht de extern opnieuw opstarten en de query voor het dubbele apparaat vinden van de laatste keer opnieuw activeren.
   
    ```
    node dmpatterns_fwupdate_service.js
    ```
3. U ziet dat de reactie van het apparaat naar de directe methode die in de console.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een rechtstreekse methode gebruikt voor het activeren van een externe firmware-update op een apparaat en gebruikt de gerapporteerde eigenschappen om te volgen van de voortgang van de firmware-update.

Zie voor informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten roept, de [taken plannen en uitzenden] [ lnk-tutorial-jobs] zelfstudie.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
