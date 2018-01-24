---
title: Routeren van berichten met Azure IoT Hub (knooppunt) | Microsoft Docs
description: Hoe Azure IoT Hub apparaat-naar-cloud-berichten verwerken met behulp van regels voor het doorsturen en aangepaste eindpunten verzending van berichten naar andere back-end-services.
services: iot-hub
documentationcenter: node
author: msebolt
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: node
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/17/2017
ms.author: v-masebo
ms.openlocfilehash: e5d57e087e5f4dc1e0abf112001218aa7390a4f7
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="routing-messages-with-iot-hub-node"></a>Routeren van berichten met IoT Hub (knooppunt)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Deze zelfstudie bouwt voort op de [aan de slag met IoT Hub] zelfstudie.  De zelfstudie:

* Laat zien hoe u regels voor doorsturen verzending van apparaat-naar-cloud-berichten in een eenvoudig en gebaseerd op configuratie-hulpprogramma te gebruiken.
* Laat zien hoe isoleren interactieve berichten die directe actie van de back-end oplossing voor verdere verwerking is vereist.  Een apparaat kan bijvoorbeeld een waarschuwing weergegeven dat activeert een ticket in een CRM-systeem invoegen verzenden.  Daarentegen feed gegevenspunt berichten, zoals temperatuur telemetrie, in een analyse-engine.

Aan het einde van deze zelfstudie, moet u drie Node.js console apps uitvoeren:

* **SimulatedDevice.js**, een aangepaste versie van de app gemaakt in de [aan de slag met IoT Hub] zelfstudie gegevenspunt apparaat-naar-cloud-berichten verzendt elke tweede en interactieve apparaat-naar-cloud-berichten per willekeurige interval. Deze app gebruikt de protocollen MQTT-protocol om te communiceren met IoT Hub.
* **ReadDeviceToCloudMessages.js** geeft de telemetrie die is verzonden door de app op uw apparaat weer.
* **ReadCriticalQueue.js** ongedaan de kritieke berichten uit de Service Bus-wachtrij gekoppeld aan de IoT-hub in de wachtrij geplaatst.

> [!NOTE]
> IoT Hub heeft ondersteuning voor veel apparaatplatforms en talen, waaronder C, Java en JavaScript SDK. Zie voor instructies over hoe het apparaat in deze zelfstudie vervangen door een fysiek apparaat en hoe apparaten verbinden met een IoT-Hub, de [Azure IoT Developer Center].

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een volledige werkende versie van de [aan de slag met IoT Hub] zelfstudie.
* Node.js versie 4.0.x of hoger.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

Wordt ook aangeraden lezen over [Azure Storage] en [Azure Service Bus].

## <a name="send-interactive-messages-from-a-device-app"></a>Interactieve berichten verzenden vanuit een apparaat-app
In deze sectie die u wijzigt de apparaat-app die u hebt gemaakt in de [aan de slag met IoT Hub] zelfstudie af en toe om berichten te verzenden waarvoor onmiddellijke verwerking.

1. Gebruik een teksteditor openen de **simulateddevice\SimulatedDevice.js** bestand. Dit bestand bevat de code voor de **SimulatedDevice** app die u hebt gemaakt in de [aan de slag met IoT Hub] zelfstudie.

2. Vervang de **connectCallback** functie met de volgende code:

    ```nodejs
    var connectCallback = function (err) {
      if (err) {
        console.log('Could not connect: ' + err);
      } else {
        console.log('Client connected');
    
        // Create a message and send it to the IoT Hub every second
        setInterval(function(){
        var data, message;
        if (Math.random() > 0.7) {
            if (Math.random() > 0.5) {
                data = "This is a critical message.";
                message = new Message(data);
                message.properties.add("level", "critical");
            } else {
                data = "This is a storage message.";
                message = new Message(data);
                message.properties.add("level", "storage");
            }
            console.log("Sending message: " + message.getData());
        }
        else {
                var temperature = 20 + (Math.random() * 15);
                var humidity = 60 + (Math.random() * 20);            
                data = JSON.stringify({ deviceId: 'myFirstNodeDevice', temperature: temperature, humidity: humidity });
                message = new Message(data);
                message.properties.add('temperatureAlert', (temperature > 30) ? 'true' : 'false');
                console.log("Sending message: " + message.getData());
            }
        client.sendEvent(message, printResultFor('send'));
        }, 1000);
      }
    };
    ```
   
    Deze methode wordt willekeurig toegevoegd voor de eigenschap `"level": "critical"` en `"level": "storage"` op berichten die door het apparaat verzonden, die een bericht waarvoor onmiddellijke actie door de back-end van de toepassing of een die moet worden permanent opgeslagen simuleert. De toepassing ondersteunt routeren van berichten op basis van de berichttekst.
   
   > [!NOTE]
   > U kunt de berichteigenschappen om berichten te routeren voor verschillende scenario's, inclusief de verwerking van koude pad, naast het hot pad voorbeeld dat hier wordt weergegeven.

2. Sla op en sluit de **simulateddevice\SimulatedDevice.js** bestand.

    > [!NOTE]
    > Het wordt aangeraden dat u een beleid voor opnieuw proberen zoals exponentieel uitstel, zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout].

## <a name="add-service-bus-queue-to-your-iot-hub-and-route-messages-to-it"></a>Service Bus-wachtrij toevoegen aan uw IoT hub en route-berichten naar het

In deze sectie maakt een Service Bus-wachtrij, verbinden met uw IoT-hub en configureren van uw IoT-hub om berichten te verzenden naar de wachtrij op basis van de aanwezigheid van een eigenschap van het bericht. Zie voor meer informatie over het verwerken van berichten van Service Bus-wachtrijen [aan de slag met wachtrijen][lnk-sb-queues-node].

1. Een Service Bus-wachtrij maakt, zoals beschreven in [aan de slag met wachtrijen][lnk-sb-queues-node]. Noteer de naam van de naamruimte en de wachtrij.

2. Open uw IoT-hub in de Azure-portal en klikt u op **eindpunten**.

    ![Eindpunten van IoT-hub][30]

3. In de **eindpunten** blade, klikt u op **toevoegen** boven uw wachtrij toevoegen aan uw IoT-hub. Naam van het eindpunt **CriticalQueue** en selecteer met de vervolgkeuzelijsten **Service Bus-wachtrij**, de Service Bus-naamruimte waarin de wachtrij zich bevindt en de naam van de wachtrij. Wanneer u klaar bent, klikt u op **OK** onderaan.  

    ![Een eindpunt toevoegen][31]

4. Klik nu op **Routes** in uw IoT-Hub. Klik op **toevoegen** boven aan de blade voor het maken van een regel voor doorsturen waarmee berichten worden doorgestuurd naar de wachtrij die u zojuist hebt toegevoegd. Selecteer **apparaat-berichten** als de bron van gegevens. Voer `level="critical"` als de voorwaarde, en kies **CriticalQueue** als een aangepaste eindpunt als de routering eindpunt van de regel. Klik op **opslaan** onderaan.  

    ![Toevoegen van een route][32]

    Zorg ervoor dat de terugval route is ingesteld op **ON**. Deze instelling is de standaardconfiguratie van een IoT-hub.

    ![Route voor terugval][33]

## <a name="optional-read-from-the-queue-endpoint"></a>(Optioneel) Lezen van het eindpunt van de wachtrij

In deze sectie maakt u een Node.js-consoletoepassing die kritieke berichten uit IoT Service Bus lezen. U vindt meer informatie in [aan de slag met wachtrijen][lnk-sb-queues-node]. 

1. Maak een lege map met de naam `readcriticalqueue`. Maak in de map `readcriticalqueue` een package.json-bestand door achter de opdrachtprompt de volgende opdracht op te geven. Accepteer alle standaardwaarden:

    ```cmd/sh
    npm init
    ```

2. Bij de opdrachtprompt in de `readcriticalqueue` map, voer de volgende opdracht voor het installeren van de **azure** pakket:

    ```cmd/sh
    npm install azure --save
    ```

3. Maak met een teksteditor, een **ReadCriticalQueue.js** bestand de `readcriticalqueue` map.

4. Voeg de volgende `require` instructies aan het begin van de **ReadCriticalQueue.js** bestand:

    ```nodejs
    'use strict';

    var azure = require('azure');
    ```

5. Voeg de volgende variabelen declareren en vervang de tijdelijke aanduiding voor waarden door IoT Service Bus tekenreeks en wachtrij naam van de verbinding:

    ```nodejs
    var connectionString = '{iotservicebus connection string}';
    var queueName = '{iotservice bus queue name}';
    ```

6. Voeg de volgende twee functies toe die de uitvoer naar de console afdrukken:

    ```nodejs
    var serviceBusService = azure.createServiceBusService(connectionString);

    setInterval(function() {serviceBusService.receiveQueueMessage(queueName, function(error, receivedMessage) {
        if(!error){
            // Message received and deleted
            console.log(receivedMessage);
        } else { console.log(error); }
        });
    }, 3000);
    ```

7. Sla op en sluit de **ReadCriticalQueue.js** bestand.

## <a name="run-the-applications"></a>De toepassingen uitvoeren

U bent nu klaar om uit te voeren van de drie toepassingen.

1. Om uit te voeren de **ReadDeviceToCloudMessages.js** toepassing in een opdrachtprompt of een shell Navigeer naar de map readdevicetocloudmessages en voer de volgende opdracht:

   ```cmd/sh
   node ReadDeviceToCloudMessages.js
   ```

   ![Read-d2c-messages uitvoeren][readd2c]

2. Om uit te voeren de **ReadCriticalQueue.js** toepassing in een opdrachtprompt of een shell Navigeer naar de map readcriticalqueue en voer de volgende opdracht:

   ```cmd/sh
   node ReadCriticalQueue.js
   ```
   
   ![Alleen kritieke berichten uitgevoerd][readqueue]

3. Om uit te voeren de **SimulatedDevice.js** app in een opdrachtprompt of een shell Navigeer naar de map simulateddevice en voer de volgende opdracht:

   ```cmd/sh
   node SimulatedDevice.js
   ```
   
   ![Simulated-device uitvoeren][simulateddevice]

## <a name="optional-add-storage-container-to-your-iot-hub-and-route-messages-to-it"></a>(Optioneel) Storage-Container toevoegen aan uw IoT hub en route-berichten naar het

In deze sectie een opslagaccount maken, verbinden met uw IoT-hub en configureren van uw iothub berichten verzenden naar de account op basis van de aanwezigheid van een eigenschap van het bericht. Zie voor meer informatie over het beheren van opslag [aan de slag met Azure Storage][Azure Storage].

 > [!NOTE]
   > Als u niet beperkt tot één **eindpunt**, u kunt setup de **StorageContainer** naast de **CriticalQueue** en beide simulatneously uitvoeren.

1. Een opslagaccount maken, zoals beschreven in [documentatie bij Azure Storage][lnk-storage]. Noteer de accountnaam.

2. Open uw IoT-hub in de Azure-portal en klikt u op **eindpunten**.

3. In de **eindpunten** blade, selecteer de **CriticalQueue** eindpunt en klik op **verwijderen**. Klik op **Ja**, en klik vervolgens op **toevoegen**. Naam van het eindpunt **StorageContainer** en selecteer met de vervolgkeuzelijsten **Azure Storage-Container**, en maak een **opslagaccount** en een **opslag container**.  Noteer de namen.  Wanneer u klaar bent, klikt u op **OK** onderaan. 

 > [!NOTE]
   > Als u niet beperkt tot één **eindpunt**, u hoeft niet te verwijderen de **CriticalQueue**.

4. Klik op **Routes** in uw IoT-Hub. Klik op **toevoegen** boven aan de blade voor het maken van een regel voor doorsturen waarmee berichten worden doorgestuurd naar de wachtrij die u zojuist hebt toegevoegd. Selecteer **apparaat-berichten** als de bron van gegevens. Voer `level="storage"` als de voorwaarde, en kies **StorageContainer** als een aangepaste eindpunt als de routering eindpunt van de regel. Klik op **opslaan** onderaan.  

    Zorg ervoor dat de terugval route is ingesteld op **ON**. Deze instelling is de standaardconfiguratie van een IoT-hub.

1. Zorg ervoor dat uw vorige toepassing **SimulatedDevice.js** nog steeds actief is. 

1. In de Azure-Portal, gaat u naar uw opslagaccount onder **Blob-Service**, klikt u op **blobs bladeren...** .  Selecteer de container, gaat u naar en klikt u op het JSON-bestand en klikt u op **downloaden** om de gegevens weer te geven.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe betrouwbaar apparaat-naar-cloud-berichten verzenden met behulp van de berichtroutering van IoT Hub.

De [het verzenden van berichten van de cloud-naar-apparaat met IoT Hub] [ lnk-c2d] ziet u hoe u berichten verzenden naar uw apparaten vanuit de back-end van uw oplossing.

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT Hub [Azure IoT Suite][lnk-suite].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub].

Zie voor meer informatie over het routeren van berichten van IoT-Hub, [berichten verzenden en ontvangen met IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-node-node-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-node-node-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-node-node-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-node-node-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-node-node-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-node-node-process-d2c/route-creation.png
[33]: ./media/iot-hub-node-node-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-node]: ../service-bus-messaging/service-bus-nodejs-how-to-use-queues.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[Ontwikkelaarshandleiding voor IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[aan de slag met IoT Hub]: iot-hub-node-node-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[afhandeling van tijdelijke fout]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Afhandeling van tijdelijke fout]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-c2d]: iot-hub-node-node-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-free-trial]: https://azure.microsoft.com/free/
[lnk-storage]: https://docs.microsoft.com/azure/storage/