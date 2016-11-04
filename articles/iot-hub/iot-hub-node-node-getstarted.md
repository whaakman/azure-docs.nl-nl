---
title: Aan de slag met Azure IoT Hub voor Node.js | Microsoft Docs
description: Instructiehandleiding voor Azure IoT Hub met Node.js. Gebruik Azure IoT Hub en Node.js in combinatie met de Microsoft Azure IoT SDK's voor het implementeren van een Internet of Things (IoT)-oplossing.
services: iot-hub
documentationcenter: nodejs
author: dominicbetts
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: javascript
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2016
ms.author: dobett

---
# Aan de slag met Azure IoT Hub voor Node.js
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Nadat u deze handleiding volledig hebt doorlopen, beschikt u over drie Node.js-consoletoepassingen:

* **CreateDeviceIdentity.js**: deze toepassing maakt een apparaat-id en de bijbehorende beveiligingssleutel waarmee uw gesimuleerde apparaat verbonden kan worden.
* **ReadDeviceToCloudMessages.js**: deze toepassing geeft de telemetrie weer die is verzonden door uw gesimuleerde apparaat.
* **SimulatedDevice.js**: deze toepassing koppelt uw IoT-hub aan de apparaat-id die u eerder hebt gemaakt, en verzendt iedere seconde een telemetriebericht via het AMQPS-protocol.

> [!NOTE]
> Het artikel [IoT-Hub SDK's][lnk-hub-sdks] bevat informatie over de verschillende SDK's die u kunt gebruiken om beide toepassingen zo te maken dat ze zowel op het apparaat als op de back-end van uw oplossing kunnen worden uitgevoerd.
> 
> 

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Node.js versie 0.10.x of hoger.
* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een gratis proefaccount aanmaken. Zie [Gratis proefversie van Azure][lnk-free-trial] voor meer informatie.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

U hebt nu uw IoT-hub gemaakt. U hebt de IoT-Hub-hostnaam en -verbindingsreeks die u nodig hebt voor de rest van deze zelfstudie.

## Een apparaat-id maken
In dit gedeelte gaat u een Node.js-consoletoepassing maken die een apparaat-id maakt in het identiteitenregister van uw IoT-hub. Een apparaat kan geen verbinding maken met de IoT-hub, tenzij het vermeld staat in het registers met apparaat-id’s. Raadpleeg het gedeelte **Register met apparaat-id’s** in de [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide-identiteit] voor meer informatie. Wanneer u deze consoletoepassing uitvoert, worden er een unieke apparaat-id en sleutel gegenereerd waarmee uw apparaat zichzelf kan identificeren tijdens het verzenden van apparaat-naar-cloud-berichten naar IoT Hub.

1. Maak een nieuwe lege map met de naam **createdeviceidentity**. In de map **createdeviceidentity** maakt u een package.json-bestand door bij de opdrachtprompt de volgende opdracht te geven. Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de map **createdeviceidentity** voert u de volgende opdracht uit om het **azure-iothub** Service SDK-pakket te installeren:
   
    ```
    npm install azure-iothub --save
    ```
3. Maak met een tekstverwerker een **CreateDeviceIdentity.js**-bestand in de map **createdeviceidentity**.
4. Voeg de volgende `require` instructie toe aan het begin van het bestand **CreateDeviceIdentity.js**-bestand:
   
    ```
    'use strict';
   
    var iothub = require('azure-iothub');
    ```
5. Voeg de volgende code toe aan het bestand **CreateDeviceIdentity.js** en vervang de waarde van de tijdelijke aanduiding door de verbindingsreeks voor de IoT-hub die u in het vorige gedeelte hebt gemaakt: 
   
    ```
    var connectionString = '{iothub connection string}';
   
    var registry = iothub.Registry.fromConnectionString(connectionString);
    ```
6. Voeg de volgende code toe om een apparaatdefinitie te maken in het register met apparaat-id’s van uw IoT-hub. Met deze code wordt een apparaat gemaakt als de apparaat-id nog niet voorkomt in het register. Anders wordt de sleutel van het bestaande apparaat geretourneerd:
   
    ```
    var device = new iothub.Device(null);
    device.deviceId = 'myFirstNodeDevice';
    registry.create(device, function(err, deviceInfo, res) {
      if (err) {
        registry.get(device.deviceId, printDeviceInfo);
      }
      if (deviceInfo) {
        printDeviceInfo(err, deviceInfo, res)
      }
    });
   
    function printDeviceInfo(err, deviceInfo, res) {
      if (deviceInfo) {
        console.log('Device id: ' + deviceInfo.deviceId);
        console.log('Device key: ' + deviceInfo.authentication.SymmetricKey.primaryKey);
      }
    }
    ```
7. Sla het bestand **CreateDeviceIdentity.js** op en sluit het.
8. Als u de toepassing **createdeviceidentity** wilt uitvoeren, geef dan de volgende opdracht bij de opdrachtprompt in de map createdeviceidentity:
   
    ```
    node CreateDeviceIdentity.js 
    ```
9. Noteer de **apparaat-id** en de **apparaatsleutel**. U hebt deze waarden later nodig wanneer u een toepassing maakt die verbinding maakt met IoT Hub als apparaat.

> [!NOTE]
> In het id-register van IoT Hub worden alleen apparaat-id’s opgeslagen waarmee veilig toegang tot de hub kan worden verkregen. De apparaat-id’s en sleutels worden opgeslagen en gebruikt als beveiligingsreferenties. Met de vlag voor ingeschakeld/uitgeschakeld kunt u toegang tot een afzonderlijk apparaat uitschakelen. Als uw toepassing andere apparaatspecifieke metagegevens moet opslaan, moet deze een toepassingsspecifieke opslagmethode gebruiken. Raadpleeg de [IoT Hub Developer Guide ][lnk-devguide-identiteit] (Ontwikkelaarshandleiding voor IoT Hub) voor meer informatie.
> 
> 

## Apparaat-naar-cloud-berichten ontvangen
In dit gedeelte maakt u een Node.js-consoletoepassing die apparaat-naar-cloud-berichten uit IoT Hub leest. Een IoT-hub toont een [Event Hubs][lnk-event-hubs-overview]-compatibel eindpunt waarmee u apparaat-naar-cloud-berichten kunt lezen. Om de zaken niet nodeloos ingewikkeld te maken, maakt u met deze handleiding een basislezer die niet geschikt is voor hoge doorvoersnelheden. In de handleiding [Apparaat-naar-cloud-berichten verwerken][lnk-process-d2c-tutorial] leert u hoe u op grote schaal apparaat-naar-cloud-berichten kunt verwerken. In de zelfstudie [Aan de slag met Event Hubs][lnk-eventhubs-tutorial] leest u meer over het verwerken van berichten van Event Hubs. Deze zelfstudie is van toepassing op de Event Hubs-compatibele eindpunten van IoT Hub.

> [!NOTE]
> Het met Event Hubs compatibele eindpunt voor het lezen van apparaat-naar-cloud-bericht maakt altijd gebruik van het AMQPS-protocol.
> 
> 

1. Maak een nieuwe lege map met de naam **readdevicetocloudmessages**. In de map **readdevicetocloudmessages** maakt u een package.json-bestand door bij de opdrachtprompt de volgende opdracht te geven. Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Voer bij de opdrachtprompt in de map **readdevicetocloudmessages** de volgende opdracht uit om het pakket **azure-event-hubs** te installeren:
   
    ```
    npm install azure-event-hubs --save
    ```
3. Maak met een tekstverwerker een bestand **ReadDeviceToCloudMessages.js** in de map **readdevicetocloudmessages**.
4. Voeg de volgende `require` instructies toe aan het begin van het bestand **ReadDeviceToCloudMessages.js**:
   
    ```
    'use strict';
   
    var EventHubClient = require('azure-event-hubs').Client;
    ```
5. Voeg de volgende variabeledeclaratie toe en vervang de waarde van de tijdelijke aanduiding door de verbindingsreeks voor uw IoT-hub:
   
    ```
    var connectionString = '{iothub connection string}';
    ```
6. Voeg de volgende twee functies toe die de uitvoer naar de console afdrukken:
   
    ```
    var printError = function (err) {
      console.log(err.message);
    };
   
    var printMessage = function (message) {
      console.log('Message received: ');
      console.log(JSON.stringify(message.body));
      console.log('');
    };
    ```
7. Voeg de volgende code toe om de **EventHubClient** te maken, open de verbinding met uw IoT-Hub en maak voor elke partitie een ontvanger. Deze toepassing gebruikt een filter bij het maken van een ontvanger, zodat de ontvanger alleen berichten leest die naar IoT Hub worden verzonden wanneer de ontvanger is geactiveerd. Dit filter is handig in een testomgeving, omdat u zo alleen de huidige reeks berichten ziet. In een productieomgeving moet uw code ervoor zorgen dat alle berichten worden verwerkt. Zie de zelfstudie [How to process IoT Hub device-to-cloud messages (IoT Hub apparaat-naar-cloud-berichten verwerken)][lnk-process-d2c-tutorial] voor meer informatie:
   
    ```
    var client = EventHubClient.fromConnectionString(connectionString);
    client.open()
        .then(client.getPartitionIds.bind(client))
        .then(function (partitionIds) {
            return partitionIds.map(function (partitionId) {
                return client.createReceiver('$Default', partitionId, { 'startAfterTime' : Date.now()}).then(function(receiver) {
                    console.log('Created partition receiver: ' + partitionId)
                    receiver.on('errorReceived', printError);
                    receiver.on('message', printMessage);
                });
            });
        })
        .catch(printError);
    ```
8. Sla het bestand **ReadDeviceToCloudMessages.js** op en sluit het.

## Een gesimuleerde apparaattoepassing maken
In dit gedeelte maakt u een Node.js-consoletoepassing die een apparaat simuleert dat apparaat-naar-cloud-berichten naar een IoT-hub verzendt.

1. Maak een nieuwe lege map met de naam **simulateddevice**. In de map **simulateddevice** maakt u een package.json-bestand door bij de opdrachtprompt de volgende opdracht te geven. Accepteer alle standaardwaarden:
   
    ```
    npm init
    ```
2. Bij de opdrachtprompt in de map **simulateddevice** voert u de volgende opdracht uit om het **azure-iot-device-amqp** Device SDK-pakket en het **azure-iot-device-amqp**-pakket te installeren:
   
    ```
    npm install azure-iot-device azure-iot-device-amqp --save
    ```
3. Maak met een tekstverwerker een nieuw bestand **SimulatedDevice.js** in de map **simulateddevice**.
4. Voeg de volgende `require` instructies toe aan het begin van het bestand **SimulatedDevice.js**:
   
    ```
    'use strict';
   
    var clientFromConnectionString = require('azure-iot-device-amqp').clientFromConnectionString;
    var Message = require('azure-iot-device').Message;
    ```
5. Voeg een **connectionString**-variabele toe en gebruik deze om een apparaat-client te maken. Vervang **{youriothostname}** door de naam van de IoT-hub die u hebt gemaakt in de sectie *Een IoT-hub maken*. Vervang **{yourdevicekey}** door de sleutelwaarde die u hebt gegenereerd in de sectie *Een apparaat-id maken*:
   
    ```
    var connectionString = 'HostName={youriothostname};DeviceId=myFirstNodeDevice;SharedAccessKey={yourdevicekey}';
   
    var client = clientFromConnectionString(connectionString);
    ```
6. Voeg de volgende functie toe om uitvoer van de toepassing weer te geven:
   
    ```
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```
7. Maak een callback en verzend iedere seconde met de functie **setInterval** een nieuw bericht naar uw IoT-hub:
   
    ```
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
   
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
            var windSpeed = 10 + (Math.random() * 4);
            var data = JSON.stringify({ deviceId: 'myFirstNodeDevice', windSpeed: windSpeed });
            var message = new Message(data);
            console.log("Sending message: " + message.getData());
            client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
8. Maak verbinding met uw IoT Hub en begin met het verzenden van berichten:
   
    ```
    client.open(connectCallback);
    ```
9. Sla het bestand **SimulatedDevice.js** op en sluit het.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u een beleid voor opnieuw proberen implementeren (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel [Transient Fault Handling ][lnk-transient-faults] (Afhandeling van tijdelijke fouten).
> 
> 

## De toepassingen uitvoeren
U kunt nu de toepassingen gaan uitvoeren.

1. Voer bij een opdrachtprompt in de map **readdevicetocloudmessages** de volgende opdracht uit om uw IoT-hub te kunnen volgen:
   
    ```
    node ReadDeviceToCloudMessages.js 
    ```
   
    ![De Node.js-clienttoepassing voor IoT Hub-services voor het bewaken van berichten die van het apparaat naar de cloud gaan][7]
2. Voer bij een opdrachtprompt in de map **simulateddevice** de volgende opdracht uit om telemetriegegevens naar uw IoT-hub te verzenden:
   
    ```
    node SimulatedDevice.js
    ```
   
    ![De Node.js-clienttoepassing voor IoT Hub-apparaten voor het verzenden van berichten van het apparaat naar de cloud][8]
3. De tegel **Usage** in de [Azure Portal][lnk-portal] toont het aantal berichten dat is verzonden naar de hub:
   
    ![De tegel Usage in de Azure-portal met het aantal berichten dat is verzonden naar IoT Hub][43]

## Volgende stappen
In deze handleiding, hebt u een nieuwe IoT-hub geconfigureerd in de portal en vervolgens hebt u een apparaat-id gemaakt in het id-register van de hub. U hebt deze apparaat-id gebruikt om de gesimuleerde apparaattoepassing in staat te stellen om apparaat-naar-cloud-berichten te verzenden naar de hub. Ook hebt een app gemaakt die de berichten weergeeft die worden ontvangen door de hub. 

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Connecting your device (Uw apparaat verbinden)][lnk-connect-device]
* [Getting started with device management (Aan de slag met apparaatbeheer)][lnk-device-management]
* [Getting started with the Gateway SDK (Aan de slag met de Gateway-SDK)][lnk-gateway-SDK]

Raadpleeg de zelfstudie [Process device-to-cloud messages (Apparaat-naar-cloud-berichten verwerken)][lnk-process-d2c-tutorial] voor meer informatie over het uitbreiden van uw IoT-oplossing en het op schaal verwerken van apparaat-naar-cloud-berichten.

<!-- Images. -->
[7]: ./media/iot-hub-node-node-getstarted/runapp1.png
[8]: ./media/iot-hub-node-node-getstarted/runapp2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identiteit]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/get_started/node-devbox-setup.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/



<!--HONumber=Oct16_HO3-->


