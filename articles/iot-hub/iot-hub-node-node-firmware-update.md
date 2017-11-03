---
title: Firmware-update van het apparaat met Azure IoT Hub (knooppunt) | Microsoft Docs
description: Klik hier voor meer informatie over het beheer van apparaten op Azure IoT Hub gebruiken om te zetten van een apparaat firmware-update. U kunt Azure IoT SDK's voor Node.js gebruiken voor het implementeren van een gesimuleerde apparaattoepassing en een service-app waarmee de firmware-update wordt geactiveerd.
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 70b84258-bc9f-43b1-b7cf-de1bb715f2cf
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/07/2017
ms.author: juanpere
ms.openlocfilehash: e169367592b25ea45c3d1017937316a3b3b538b8
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-nodenode"></a>Apparaatbeheer gebruiken om te zetten van een apparaat firmware-update (knooppunt/knooppunt)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

In de [aan de slag met Apparaatbeheer] [ lnk-dm-getstarted] zelfstudie, hebt u gezien hoe u de [apparaat twin] [ lnk-devtwin] en [methoden directe] [ lnk-c2dmethod] primitieven op afstand opnieuw opstarten van een apparaat. Deze zelfstudie maakt gebruik van de dezelfde IoT Hub primitieven en biedt richtlijnen en ziet u hoe u een end-to-end gesimuleerde firmware-update.  Dit patroon wordt gebruikt in de firmware-update-implementatie voor de Intel Edison apparaat.

In deze handleiding ontdekt u hoe u:

* Een Node.js-consoletoepassing die de directe firmwareUpdate-methode in de gesimuleerde apparaattoepassing via uw IoT-hub aanroept maken.
* Maakt een gesimuleerd apparaat-app die u implementeert een **firmwareUpdate** directe methode. Deze methode initieert een meerdere fasen die wacht de firmware-installatiekopie te downloaden, de installatiekopie van het firmware downloaden en ten slotte is de installatiekopie van de firmware van toepassing. In elke fase van de update wordt het apparaat de gerapporteerde eigenschappen gebruikt om te rapporteren over de voortgang.

Aan het einde van deze zelfstudie hebt u twee console Node.js-apps:

**dmpatterns_fwupdate_service.js**, wordt de reactie die een directe methode wordt aangeroepen in de gesimuleerde apparaattoepassing weergegeven, en regelmatig (elke 500ms) geeft de bijgewerkte gerapporteerde eigenschappen.

**dmpatterns_fwupdate_device.js**, die verbinding maakt met uw IoT-hub aan de apparaat-id eerder hebt gemaakt, ontvangen een directe methode firmwareUpdate, wordt uitgevoerd via een proces met meerdere status om te simuleren een firmware-update inclusief: wachten op voor het downloaden van de installatiekopie, downloaden van de nieuwe installatiekopie en ten slotte de installatiekopie toe te passen.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Node.js-versie 4.0.x of hoger <br/>  [Uw ontwikkelingsomgeving voorbereiden] [ lnk-dev-setup] wordt beschreven hoe u Node.js voor deze zelfstudie installeren op Windows of Linux.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

Ga als volgt de [aan de slag met Apparaatbeheer](iot-hub-node-node-device-management-get-started.md) artikel voor het maken van uw IoT-hub en uw IoT Hub-verbindingsreeks ophalen.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Activeren van een externe firmware-update op het apparaat met een directe methode
In deze sectie maakt u een Node.js-consoletoepassing die een externe firmware-update op een apparaat start. De app gebruikmaakt van een directe methode om de update te initiëren en apparaat twin query's gebruikt om het periodiek de status van de actieve firmware-update niet ophalen.

1. Maak een lege map genaamd **triggerfwupdateondevice**.  In de **triggerfwupdateondevice** map, maakt u een package.json-bestand met de volgende opdracht achter de opdrachtprompt.  Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de **triggerfwupdateondevice** map, voer de volgende opdracht voor het installeren van de **azure-iot-hub** pakket:
   
    ```
    npm install azure-iothub --save
    ```
3. Maak met een teksteditor, een **dmpatterns_getstarted_service.js** bestand de **triggerfwupdateondevice** map.
4. Voeg de volgende 'vereist' instructies aan het begin van de **dmpatterns_getstarted_service.js** bestand:
   
    ```
    'use strict';
   
    var Registry = require('azure-iothub').Registry;
    var Client = require('azure-iothub').Client;
    ```
5. Voeg de volgende variabele declaraties en vervang de tijdelijke aanduiding voor waarden:
   
    ```
    var connectionString = '{device_connectionstring}';
    var registry = Registry.fromConnectionString(connectionString);
    var client = Client.fromConnectionString(connectionString);
    var deviceToUpdate = 'myDeviceId';
    ```
6. Voeg de volgende functie vinden en de waarde van de firmwareUpdate weer te geven eigenschap gerapporteerd.
   
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
7. De volgende functie voor het aanroepen van de methode firmwareUpdate opnieuw opstarten van het doelapparaat toevoegen:
   
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
8. Ten slotte de volgende functie toevoegen aan code voor het starten van de firmware-update wordt uitgevoerd en start periodiek waarin de gerapporteerde eigenschappen:
   
    ```
    startFirmwareUpdateDevice();
    setInterval(queryTwinFWUpdateReported, 500);
    ```
9. Sla op en sluit de **dmpatterns_fwupdate_service.js** bestand.

[!INCLUDE [iot-hub-device-firmware-update](../../includes/iot-hub-device-firmware-update.md)]

## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.

1. Bij de opdrachtprompt in de **manageddevice** map, voer de volgende opdracht om te beginnen met luisteren naar de directe methode voor opnieuw opstarten.
   
    ```
    node dmpatterns_fwupdate_device.js
    ```
2. Bij de opdrachtprompt in de **triggerfwupdateondevice** map, voer de volgende opdracht de extern opnieuw opstarten en de query voor het apparaat vinden van de laatste keer opnieuw activeren.
   
    ```
    node dmpatterns_fwupdate_service.js
    ```
3. U ziet de reactie van het apparaat aan de directe methode in de console.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een directe methode gebruikt voor het activeren van een externe firmware-update op een apparaat en de gerapporteerde eigenschappen gebruikt voor het bijhouden van de voortgang van de firmware-update.

Zie voor meer informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten aanroepen, de [planning en broadcast taken] [ lnk-tutorial-jobs] zelfstudie.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
