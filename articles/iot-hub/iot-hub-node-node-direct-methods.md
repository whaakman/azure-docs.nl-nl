---
title: Azure IoT Hub directe methoden (knooppunt) | Microsoft Docs
description: Het gebruik van Azure IoT Hub rechtstreekse methoden. U kunt Azure IoT SDK's voor Node.js gebruiken voor het implementeren van een gesimuleerde apparaattoepassing die een directe methode bevat en een service-app die de directe methode aanroept.
services: iot-hub
documentationcenter: 
author: nberdy
manager: timlt
editor: 
ms.assetid: ea9c73ca-7778-4e38-a8f1-0bee9d142f04
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: nberdy
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9a73c25724a239e56c3ea62a8452bb7c3a2b51be
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-direct-methods-on-your-iot-device-with-nodejs"></a>Directe methoden te gebruiken op uw IoT-apparaat met behulp van Node.js
[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

Aan het einde van deze zelfstudie hebt u twee console Node.js-apps:

* **CallMethodOnDevice.js**, die een methode wordt aangeroepen in de gesimuleerde apparaattoepassing en het antwoord weergegeven.
* **SimulatedDevice.js**, die verbinding maakt met uw IoT-hub aan de apparaat-id eerder hebt gemaakt en reageert op de methode aangeroepen door de cloud.

> [!NOTE]
> Het artikel [Azure IoT-SDK's][lnk-hub-sdks] bevat informatie over de verschillende Azure IoT-SDK's die u kunt gebruiken om beide toepassingen zo te maken dat ze zowel op het apparaat als op de back-end van uw oplossing kunnen worden uitgevoerd.
> 
> 

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Node.js versie 4.0.x of hoger.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie maakt u een Node.js-consoletoepassing dat met een methode aangeroepen door de cloud overeenkomt.

1. Maak een nieuwe lege map met de naam **simulateddevice**. In de map **simulateddevice** maakt u een package.json-bestand door in het opdrachtprompt de volgende opdracht op te geven. Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. In de opdrachtprompt in de map **simulateddevice** voert u de volgende opdracht uit om het **azure-iot-device-amqp** Device SDK-pakket en het **azure-iot-device-mqtt**-pakket te installeren:
   
    ```
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```
3. Maak met een tekstverwerker een nieuw bestand **SimulatedDevice.js** in de map **simulateddevice**.
4. Voeg de volgende `require` instructies toe aan het begin van het bestand **SimulatedDevice.js**:
   
    ```
    'use strict';
   
    var Mqtt = require('azure-iot-device-mqtt').Mqtt;
    var DeviceClient = require('azure-iot-device').Client;
    ```
5. Voeg een **connectionString** variabele en deze gebruiken voor het maken een **DeviceClient** exemplaar. Vervang **{verbindingsreeks apparaat}** tekenreeks met de apparaatverbinding u hebt gegenereerd tijdens de *maken van een apparaat-id* sectie:
   
    ```
    var connectionString = '{device connection string}';
    var client = DeviceClient.fromConnectionString(connectionString, Mqtt);
    ```
6. Voeg de volgende functie voor het implementeren van de methode op het apparaat toe:
   
    ```
    function onWriteLine(request, response) {
        console.log(request.payload);
   
        response.send(200, 'Input was written to log.', function(err) {
            if(err) {
                console.error('An error ocurred when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.' );
            }
        });
    }
    ```
7. Open de verbinding met uw IoT-hub en begin initialiseren de listener voor methode:
   
    ```
    client.open(function(err) {
        if (err) {
            console.error('could not open IotHub client');
        }  else {
            console.log('client opened');
            client.onDeviceMethod('writeLine', onWriteLine);
        }
    });
    ```
8. Sla het bestand **SimulatedDevice.js** op en sluit het.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. In productiecode moet u beleid voor opnieuw proberen (zoals verbinding opnieuw), zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout][lnk-transient-faults].
> 
> 

## <a name="call-a-method-on-a-device"></a>Een methode is aangeroepen voor een apparaat
In deze sectie maakt u een Node.js-consoletoepassing die een methode wordt aangeroepen in de gesimuleerde apparaattoepassing en wordt vervolgens het antwoord.

1. Maak een nieuwe lege map genaamd **callmethodondevice**. In de **callmethodondevice** map, maakt u een package.json-bestand met de volgende opdracht achter de opdrachtprompt. Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de **callmethodondevice** map, voer de volgende opdracht voor het installeren van de **azure-iothub** pakket:
   
    ```
    npm install azure-iothub --save
    ```
3. Maak met een teksteditor, een **CallMethodOnDevice.js** bestand de **callmethodondevice** map.
4. Voeg de volgende `require` instructies aan het begin van de **CallMethodOnDevice.js** bestand:
   
    ```
    'use strict';
   
    var Client = require('azure-iothub').Client;
    ```
5. Voeg de volgende variabeledeclaratie toe en vervang de waarde van de tijdelijke aanduiding door de IoT Hub-verbindingsreeks van uw hub:
   
    ```
    var connectionString = '{iothub connection string}';
    var methodName = 'writeLine';
    var deviceId = 'myDeviceId';
    ```
6. De client voor het openen van de verbinding met uw iothub maken.
   
    ```
    var client = Client.fromConnectionString(connectionString);
    ```
7. Voeg de volgende functie zodat deze de apparaat-methode worden aangeroepen en de reactie van het apparaat naar de console afdrukken:
   
    ```
    var methodParams = {
        methodName: methodName,
        payload: 'hello world',
        timeoutInSeconds: 30
    };
   
    client.invokeDeviceMethod(deviceId, methodParams, function (err, result) {
        if (err) {
            console.error('Failed to invoke method \'' + methodName + '\': ' + err.message);
        } else {
            console.log(methodName + ' on ' + deviceId + ':');
            console.log(JSON.stringify(result, null, 2));
        }
    });
    ```
8. Sla op en sluit de **CallMethodOnDevice.js** bestand.

## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.

1. Bij een opdrachtprompt in de **simulateddevice** map, voer de volgende opdracht om te beginnen met luisteren voor methodeaanroepen vanuit uw IoT-Hub:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![][7]
2. Bij een opdrachtprompt in de **callmethodondevice** map, voer de volgende opdracht om te beginnen met bewaken van uw IoT-hub:
   
    ```
    node CallMethodOnDevice.js 
    ```
   
    ![][8]
3. Hier ziet u het apparaat aan de methode reageren door het afdrukken van het bericht en de toepassing die het antwoord van de weergave van de methode van het apparaat aangeroepen:
   
    ![][9]

## <a name="next-steps"></a>Volgende stappen
In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U hebt deze apparaat-id gebruikt om in te schakelen van de gesimuleerde apparaattoepassing om te reageren op de methoden die worden aangeroepen door de cloud. Hebt u ook een app roept methoden op het apparaat en de reactie van het apparaat wordt gemaakt. 

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Aan de slag met IoT Hub]
* [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Zie voor meer informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten aanroepen, de [planning en broadcast taken] [ lnk-tutorial-jobs] zelfstudie.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-direct-methods/run-simulated-device.png
[8]: ./media/iot-hub-node-node-direct-methods/run-callmethodondevice.png
[9]: ./media/iot-hub-node-node-direct-methods/methods-output.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-devguide-methods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[Send Cloud-to-Device messages with IoT Hub]: iot-hub-csharp-csharp-c2d.md
[Process Device-to-Cloud messages]: iot-hub-csharp-csharp-process-d2c.md
[Aan de slag met IoT Hub]: iot-hub-node-node-getstarted.md
