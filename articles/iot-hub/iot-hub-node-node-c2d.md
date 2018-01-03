---
title: Cloud-naar-apparaat-berichten met Azure IoT Hub (knooppunt) | Microsoft Docs
description: Het cloud-naar-apparaat om berichten te verzenden naar een apparaat van een Azure IoT hub met de Azure IoT SDK's voor Node.js. U kunt een gesimuleerde apparaattoepassing cloud-naar-apparaat-berichten ontvangen en wijzigen van een back-end-app voor het verzenden van de cloud-naar-apparaat-berichten wijzigen.
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 3ca8a78f-ade2-46e8-8a49-d5d599cdf1f1
ms.service: iot-hub
ms.devlang: javascript
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 85a68910c6510be870362b51d5b8fa50e4d42328
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a>Cloud-naar-apparaat-berichten verzenden met IoT Hub (knooppunt)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Inleiding
Azure IoT Hub is een volledig beheerde service waarmee stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten inschakelen en een back-end oplossing. De [aan de slag met IoT Hub] zelfstudie laat zien hoe u een iothub maken, een apparaat-id in het inrichten en code van een gesimuleerde apparaattoepassing dat apparaat-naar-cloud-berichten verzendt.

Deze zelfstudie bouwt voort op [aan de slag met IoT Hub]. Hier ziet u hoe aan:

* Van uw back-end oplossing, cloud-naar-apparaat-berichten naar één enkel apparaat via IoT Hub te verzenden.
* Cloud-naar-apparaat-berichten op een apparaat ontvangen.
* Aanvragen van uw back-end oplossing, levering bevestiging (*feedback*) voor berichten die worden verzonden naar een apparaat uit IoT Hub.

U vindt meer informatie over cloud-naar-apparaat-berichten in de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].

Aan het einde van deze zelfstudie, moet u twee console Node.js-apps uitvoeren:

* **SimulatedDevice**, een aangepaste versie van de app gemaakt in [aan de slag met IoT Hub], die verbinding maakt met uw IoT-hub en cloud-naar-apparaat-berichten worden ontvangen.
* **SendCloudToDeviceMessage**, die verzendt een bericht cloud-naar-apparaat naar de gesimuleerde apparaattoepassing via IoT Hub en vervolgens ontvangt de bevestiging levering.

> [!NOTE]
> IoT-Hub heeft SDK-ondersteuning voor veel apparaatplatforms en talen (inclusief C, Java en Javascript) via Azure IoT-apparaat-SDK's. Zie voor stapsgewijze instructies voor het koppelen van uw apparaat in deze zelfstudie code en in het algemeen naar Azure IoT Hub de [Azure IoT Developer Center].
> 
> 

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Node.js versie 4.0.x of hoger.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

## <a name="receive-messages-in-the-simulated-device-app"></a>Berichten ontvangen in de gesimuleerde apparaattoepassing
In deze sectie, wijzigt u de gesimuleerde apparaattoepassing die u hebt gemaakt in [aan de slag met IoT Hub] cloud-naar-apparaat om berichten te ontvangen van de IoT-hub.

1. Met een teksteditor, open het bestand SimulatedDevice.js.
2. Wijzig de **connectCallback** functie voor het afhandelen van berichten uit IoT Hub. In dit voorbeeld wordt het apparaat altijd roept de **voltooid** functie IoT Hub melden dat deze het bericht is verwerkt. De nieuwe versie van de **connectCallback** functie lijkt op het volgende fragment:
   
    ```javascript
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
        client.on('message', function (msg) {
          console.log('Id: ' + msg.messageId + ' Body: ' + msg.data);
          client.complete(msg, printResultFor('completed'));
        });
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var temperature = 20 + (Math.random() * 15);
            var humidity = 60 + (Math.random() * 20);            
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', temperature: temperature, humidity: humidity });
            var message = new Message(data);
            message.properties.add('temperatureAlert', (temperature > 30) ? 'true' : 'false');
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
   
   > [!NOTE]
   > Als u HTTPS in plaats van de protocollen MQTT of AMQP zijn als het transport de **DeviceClient** exemplaar wordt gecontroleerd op berichten uit IoT Hub zelden (minder dan elke 25 minuten). Zie voor meer informatie over de verschillen tussen MQTT, AMQP- en HTTPS-ondersteuning en Iothub beperking de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].
   > 
   > 

## <a name="send-a-cloud-to-device-message"></a>Een cloud naar apparaat verzenden
In deze sectie maakt maken u een Node.js-consoletoepassing dat cloud-naar-apparaat-berichten naar de gesimuleerde apparaattoepassing verzendt. U moet de apparaat-ID van het apparaat dat u hebt toegevoegd in de [aan de slag met IoT Hub] zelfstudie. U moet ook de IoT Hub-verbindingsreeks voor uw hub die u kunt vinden in de [Azure-portal].

1. Maak een lege map genaamd **sendcloudtodevicemessage**. In de **sendcloudtodevicemessage** map, maakt u een package.json-bestand met de volgende opdracht achter de opdrachtprompt. Accepteer alle standaardwaarden:
   
    ```shell
    npm init
    ```
2. Bij de opdrachtprompt in de **sendcloudtodevicemessage** map, voer de volgende opdracht voor het installeren van de **azure-iothub** pakket:
   
    ```shell
    npm install azure-iothub --save
    ```
3. Maak met een teksteditor, een **SendCloudToDeviceMessage.js** bestand de **sendcloudtodevicemessage** map.
4. Voeg de volgende `require` instructies aan het begin van de **SendCloudToDeviceMessage.js** bestand:
   
    ```javascript
    'use strict';
   
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```
5. Voeg de volgende code naar **SendCloudToDeviceMessage.js** bestand. Vervang de waarde van de tijdelijke aanduiding '{iot hub verbindingsreeks}' door de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de [aan de slag met IoT Hub] zelfstudie. Vervang de tijdelijke aanduiding voor de '{apparaat-id}' met de apparaat-ID van het apparaat dat u hebt toegevoegd in de [aan de slag met IoT Hub] zelfstudie:
   
    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';
   
    var serviceClient = Client.fromConnectionString(connectionString);
    ```
6. Voeg de volgende functie om de Bewerkingsresultaten aan de console afdrukken:
   
    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Voeg de volgende functie levering Feedbackberichten naar de console afdrukken:
   
    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```
8. Voeg de volgende code voor het verzenden van een bericht naar uw apparaat en verwerken van het Feedbackbericht wanneer het apparaat het cloud-naar-apparaat-bericht erkent:
   
    ```javascript
    serviceClient.open(function (err) {
      if (err) {
        console.error('Could not connect: ' + err.message);
      } else {
        console.log('Service client connected');
        serviceClient.getFeedbackReceiver(receiveFeedback);
        var message = new Message('Cloud to device message.');
        message.ack = 'full';
        message.messageId = "My Message ID";
        console.log('Sending message: ' + message.getData());
        serviceClient.send(targetDevice, message, printResultFor('send'));
      }
    });
    ```
9. Opslaan en sluiten **SendCloudToDeviceMessage.js** bestand.

## <a name="run-the-applications"></a>De toepassingen uitvoeren
U kunt nu de toepassingen gaan uitvoeren.

1. Bij de opdrachtprompt in de **simulateddevice** map, voer de volgende opdracht voor het verzenden van telemetrie naar IoT Hub en om te luisteren naar de cloud-naar-apparaat-berichten:
   
    ```shell
    node SimulatedDevice.js 
    ```
   
    ![De gesimuleerde apparaattoepassing uitvoeren][img-simulated-device]
2. Bij een opdrachtprompt in de **sendcloudtodevicemessage** map de volgende opdracht om een cloud naar apparaat verzenden en wacht tot de bevestiging van feedback:
   
    ```shell
    node SendCloudToDeviceMessage.js 
    ```
   
    ![Voer de app voor het verzenden van de opdracht cloud-naar-apparaat][img-send-command]
   
   > [!NOTE]
   > Deze zelfstudie implementeert voor de eenvoud mogelijk te houden, niet een beleid voor opnieuw proberen. In productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout].
   > 
   > 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe cloud-naar-apparaat-berichten verzenden en ontvangen. 

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT Hub [Azure IoT Suite].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[aan de slag met IoT Hub]: iot-hub-node-node-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[Ontwikkelaarshandleiding voor IoT Hub]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://azure.microsoft.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[afhandeling van tijdelijke fout]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
