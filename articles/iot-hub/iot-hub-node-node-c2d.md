---
title: Cloud-naar-apparaat-berichten met Azure IoT Hub (Node) | Microsoft Docs
description: Klik hier voor meer informatie over het cloud-naar-apparaat-berichten verzenden naar een apparaat van een Azure IoT-hub met behulp van de Azure IoT SDK's voor Node.js. Een gesimuleerde apparaat-app voor het ontvangen van berichten van cloud-naar-apparaat en het wijzigen van een back-end-app om de cloud-naar-apparaat-berichten te verzenden kunt u wijzigen.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.devlang: javascript
ms.topic: conceptual
ms.date: 06/16/2017
ms.openlocfilehash: 0cc60ef0cddc2867ce10324334782fa6ed418bdd
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57009304"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-node"></a>Cloud-naar-apparaat-berichten verzenden met IoT Hub (Node)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>Inleiding
Azure IoT Hub is een volledig beheerde service die stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten inschakelen en een back-end oplossing. De [aan de slag met IoT Hub] zelfstudie laat zien hoe u een IoT-hub maken, een apparaat-id in het inrichten en code van een gesimuleerde apparaat-app dat apparaat-naar-cloud-berichten verzendt.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze zelfstudie bouwt voort op [aan de slag met IoT Hub]. Hier ziet u hoe aan:

* Vanuit de back-end, cloud-naar-apparaat-berichten naar een enkel apparaat via IoT Hub te verzenden.
* Cloud-naar-apparaat-berichten op een apparaat ontvangen.
* Aanvragen van de back-end, levering bevestiging (*feedback*) voor berichten die worden verzonden naar een apparaat vanuit IoT Hub.

U vindt meer informatie over cloud-naar-apparaat-berichten in de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].

Aan het einde van deze zelfstudie, moet u twee Node.js-consoletoepassingen uitvoeren:

* **SimulatedDevice**, een aangepaste versie van de app gemaakt [aan de slag met IoT Hub], die verbinding maakt met uw IoT-hub en cloud-naar-apparaat-berichten worden ontvangen.
* **SendCloudToDeviceMessage**, die een cloud-naar-apparaat-bericht naar de gesimuleerde apparaattoepassing via IoT Hub verzendt en ontvangt u vervolgens de bevestiging levering.

> [!NOTE]
> IoT-Hub heeft SDK-ondersteuning voor vele platformen voor apparaten en talen (waaronder C, Java en Javascript) via Azure IoT device-SDK's. Zie voor stapsgewijze instructies voor het verbinding maken tussen uw apparaat in de code van deze zelfstudie, en in het algemeen voor Azure IoT Hub, de [Azure IoT-ontwikkelaarscentrum].
> 
> 

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Node.js versie 4.0.x of hoger.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

## <a name="receive-messages-in-the-simulated-device-app"></a>Berichten ontvangen in het gesimuleerde apparaat-app
In deze sectie maakt u de gesimuleerde apparaat-app die u hebt gemaakt in [aan de slag met IoT Hub] cloud-naar-apparaat-berichten ontvangen van de IoT-hub.

1. Open met een teksteditor het bestand SimulatedDevice.js.
2. Wijzig de **connectCallback** functie voor het afhandelen van berichten uit IoT Hub. In dit voorbeeld wordt het apparaat altijd roept de **voltooid** functie voor het melden van IoT Hub dat deze het bericht is verwerkt. De nieuwe versie van de **connectCallback** functie ziet eruit als in het volgende codefragment:
   
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
   > Als u HTTPS in plaats van MQTT- of AMQP als het transport, de **DeviceClient** exemplaar wordt gecontroleerd op berichten uit IoT Hub weinig (minder dan elke 25 minuten). Zie voor meer informatie over de verschillen tussen MQTT-, AMQP- en HTTPS-ondersteuning en het beperken van IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].
   > 
   > 

## <a name="send-a-cloud-to-device-message"></a>Een cloud-naar-apparaat-bericht verzenden
In deze sectie maakt maken u een Node.js-consoletoepassing die cloud-naar-apparaat-berichten naar de gesimuleerde apparaattoepassing verzendt. U moet de apparaat-ID van het apparaat dat u hebt toegevoegd in de [aan de slag met IoT Hub] zelfstudie. U moet ook de IoT Hub-verbindingsreeks voor uw hub die u kunt vinden in de [Azure Portal].

1. Maak een lege map genaamd **sendcloudtodevicemessage**. In de **sendcloudtodevicemessage** map, maakt u een package.json-bestand met de volgende opdracht in uw opdrachtvenster. Accepteer alle standaardwaarden:
   
    ```shell
    npm init
    ```
2. In het opdrachtprompt in de **sendcloudtodevicemessage** map, voer de volgende opdracht voor het installeren van de **azure-iothub** pakket:
   
    ```shell
    npm install azure-iothub --save
    ```
3. Maak met een teksteditor een **SendCloudToDeviceMessage.js** -bestand in de **sendcloudtodevicemessage** map.
4. Voeg de volgende `require` instructies toe aan het begin van de **SendCloudToDeviceMessage.js** bestand:
   
    ```javascript
    'use strict';
   
    var Client = require('azure-iothub').Client;
    var Message = require('azure-iot-common').Message;
    ```
5. Voeg de volgende code aan **SendCloudToDeviceMessage.js** bestand. Vervang de waarde '{iot hub connection string}'-tijdelijke aanduiding door de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de [aan de slag met IoT Hub] zelfstudie. Vervang de tijdelijke aanduiding voor '{apparaat-id}' met de apparaat-ID van het apparaat dat u hebt toegevoegd in de [aan de slag met IoT Hub] zelfstudie:
   
    ```javascript
    var connectionString = '{iot hub connection string}';
    var targetDevice = '{device id}';
   
    var serviceClient = Client.fromConnectionString(connectionString);
    ```
6. Voeg de volgende functie als u wilt afdrukken van resultaten van de bewerking naar de console:
   
    ```javascript
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Voeg de volgende functie als u wilt afdrukken van levering Feedbackberichten naar de console:
   
    ```javascript
    function receiveFeedback(err, receiver){
      receiver.on('message', function (msg) {
        console.log('Feedback message:')
        console.log(msg.getData().toString('utf-8'));
      });
    }
    ```
8. Voeg de volgende code om een bericht verzenden naar uw apparaat en de verwerking van het Feedbackbericht wanneer het apparaat het cloud-naar-apparaat bericht erkent:
   
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

1. Bij de opdrachtprompt in de **simulateddevice** map, voer de volgende opdracht voor het verzenden van telemetrie naar IoT Hub en om te luisteren naar berichten van cloud-naar-apparaat:
   
    ```shell
    node SimulatedDevice.js 
    ```
   
    ![De gesimuleerde apparaat-app uitvoeren][img-simulated-device]
2. Bij een opdrachtprompt in de **sendcloudtodevicemessage** map, voer de volgende opdracht voor het verzenden van een cloud-naar-apparaat-bericht en wacht tot de feedback bevestiging:
   
    ```shell
    node SendCloudToDeviceMessage.js 
    ```
   
    ![De app voor het verzenden van de cloud-naar-apparaat-opdracht uitvoeren][img-send-command]
   
   > [!NOTE]
   > In deze zelfstudie implementeert voor geeft een beeld van de eenvoud, niet een beleid voor opnieuw proberen. Bij de productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals aangegeven in het artikel implementeren [afhandeling van tijdelijke fouten](/azure/architecture/best-practices/transient-faults).
   > 
   > 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u cloud-naar-apparaat-berichten verzenden en ontvangen. 

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT-Hub [Azure IoT-Remote Monitoring solution accelerator].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub].

<!-- Images -->
[img-simulated-device]: media/iot-hub-node-node-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-node-node-c2d/sendc2d.png

<!-- Links -->

[Aan de slag met IoT Hub]: quickstart-send-telemetry-node.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[Ontwikkelaarshandleiding voor IoT Hub]: iot-hub-devguide.md
[Azure IoT-ontwikkelaarscentrum]: http://azure.microsoft.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[Azure Portal]: https://portal.azure.com
[Azure IoT-Remote Monitoring solution accelerator]: https://azure.microsoft.com/documentation/suites/iot-suite/
