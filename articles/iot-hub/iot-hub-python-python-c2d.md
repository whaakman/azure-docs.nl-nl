---
title: Cloud-naar-apparaat-berichten met Azure IoT Hub (Python) | Microsoft Docs
description: Het cloud-naar-apparaat om berichten te verzenden naar een apparaat van een Azure IoT hub met behulp van de Azure IoT SDK's voor Python. U kunt een gesimuleerde apparaattoepassing cloud-naar-apparaat-berichten ontvangen en wijzigen van een back-end-app voor het verzenden van de cloud-naar-apparaat-berichten wijzigen.
services: iot-hub
documentationcenter: python
author: msebolt
manager: timlt
editor: 
ms.assetid: 3ca8a78f-ade2-46e8-8a49-d5d599cdf1f1
ms.service: iot-hub
ms.devlang: javascript
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2018
ms.author: v-masebo
ms.openlocfilehash: 592e0cd858d16715f95955194eca4217d9914b05
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="send-cloud-to-device-messages-with-iot-hub-python"></a>Cloud-naar-apparaat-berichten verzenden met IoT Hub (Python)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]


## <a name="introduction"></a>Inleiding
Azure IoT Hub is een volledig beheerde service waarmee stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten inschakelen en een back-end oplossing. De [aan de slag met IoT Hub] zelfstudie laat zien hoe u een iothub maken, een apparaat-id in het inrichten en code van een gesimuleerde apparaattoepassing dat apparaat-naar-cloud-berichten verzendt.

Deze zelfstudie bouwt voort op [aan de slag met IoT Hub]. Hier ziet u hoe aan:

* Van uw back-end oplossing, cloud-naar-apparaat-berichten naar één enkel apparaat via IoT Hub te verzenden.
* Cloud-naar-apparaat-berichten op een apparaat ontvangen.
* Aanvragen van uw back-end oplossing, levering bevestiging (*feedback*) voor berichten die worden verzonden naar een apparaat uit IoT Hub.

U vindt meer informatie over cloud-naar-apparaat-berichten in de [Ontwikkelaarshandleiding voor IoT Hub][IoT Hub developer guide - C2D].

Aan het einde van deze zelfstudie, moet u twee Python console apps uitvoeren:

* **SimulatedDevice.py**, een aangepaste versie van de app gemaakt in [aan de slag met IoT Hub], die verbinding maakt met uw IoT-hub en cloud-naar-apparaat-berichten worden ontvangen.
* **SendCloudToDeviceMessage.py**, die verzendt een bericht cloud-naar-apparaat naar de gesimuleerde apparaattoepassing via IoT Hub en vervolgens ontvangt de bevestiging levering.

> [!NOTE]
> IoT-Hub heeft SDK-ondersteuning voor veel apparaatplatforms en talen (inclusief C, Java en Javascript) via Azure IoT-apparaat-SDK's. Zie voor stapsgewijze instructies voor het koppelen van uw apparaat in deze zelfstudie code en in het algemeen naar Azure IoT Hub de [Azure IoT Developer Center].
> 

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x][lnk-python-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk *pip* [installeren of upgraden, het Python-pakketbeheersysteem][lnk-install-pip].
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++][lnk-visual-c-redist] om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

> [!NOTE]
> De *pip*-pakketten voor `azure-iothub-service-client` en `azure-iothub-device-client` zijn momenteel alleen beschikbaar voor het Windows-besturingssysteem. Raadpleeg de Linux- en Mac OS-specifieke secties in het bericht [voorbereiden uw ontwikkelomgeving voor Python]-[lnk-python-devbox] voor Linux/Mac OS.
> 


## <a name="receive-messages-in-the-simulated-device-app"></a>Berichten ontvangen in de gesimuleerde apparaattoepassing
In deze sectie maakt u een Python-console-app om te simuleren van het apparaat en cloud-naar-apparaat-berichten ontvangen van de IoT-hub.

1. Maak met een teksteditor, een **SimulatedDevice.py** bestand.

1. Voeg de volgende `import` -instructies en variabelen aan het begin van de **SimulatedDevice.py** bestand:
   
    ```python
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError

    RECEIVE_CONTEXT = 0
    WAIT_COUNT = 10
    RECEIVED_COUNT = 0
    RECEIVE_CALLBACKS = 0
    ```

1. Voeg de volgende code naar **SimulatedDevice.py** bestand. Vervang de waarde van de tijdelijke aanduiding '{deviceConnectionString}' met de apparaat-verbindingsreeks voor het apparaat dat u hebt gemaakt in de [aan de slag met IoT Hub] zelfstudie:
   
    ```python
    # choose AMQP or AMQP_WS as transport protocol
    PROTOCOL = IoTHubTransportProvider.AMQP
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

1. Voeg de volgende functie als u wilt ontvangen berichten naar de console afdrukken:
   
    ```python
    def receive_message_callback(message, counter):
        global RECEIVE_CALLBACKS
        message_buffer = message.get_bytearray()
        size = len(message_buffer)
        print ( "Received Message [%d]:" % counter )
        print ( "    Data: <<<%s>>> & Size=%d" % (message_buffer[:size].decode('utf-8'), size) )
        map_properties = message.properties()
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        counter += 1
        RECEIVE_CALLBACKS += 1
        print ( "    Total calls received: %d" % RECEIVE_CALLBACKS )
        return IoTHubMessageDispositionResult.ACCEPTED

    def iothub_client_init():
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

        client.set_message_callback(receive_message_callback, RECEIVE_CONTEXT)

        return client

    def print_last_message_time(client):
        try:
            last_message = client.get_last_message_receive_time()
            print ( "Last Message: %s" % time.asctime(time.localtime(last_message)) )
            print ( "Actual time : %s" % time.asctime() )
        except IoTHubClientError as iothub_client_error:
            if iothub_client_error.args[0].result == IoTHubClientResult.INDEFINITE_TIME:
                print ( "No message received" )
            else:
                print ( iothub_client_error )
    ```

1. Voeg de volgende code om te initialiseren van de client en wachten tot de cloud-naar-apparaat wordt weergegeven:
   
    ```python
    def iothub_client_init():
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

        client.set_message_callback(receive_message_callback, RECEIVE_CONTEXT)

        return client

    def iothub_client_sample_run():
        try:
            client = iothub_client_init()

            while True:
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    status = client.get_send_status()
                    print ( "Send status: %s" % status )
                    time.sleep(10)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

        print_last_message_time(client)
    ```

1. De volgende belangrijke functie toevoegen:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_sample_run()
    ```

1. Opslaan en sluiten **SimulatedDevice.py** bestand.


## <a name="send-a-cloud-to-device-message"></a>Een cloud naar apparaat verzenden
In deze sectie maakt maken u een Python-consoletoepassing dat cloud-naar-apparaat-berichten naar de gesimuleerde apparaattoepassing verzendt. U moet de apparaat-ID van het apparaat dat u hebt toegevoegd in de [aan de slag met IoT Hub] zelfstudie. U moet ook de IoT Hub-verbindingsreeks voor uw hub die u kunt vinden in de [Azure-portal].

1. Maak met een teksteditor, een **SendCloudToDeviceMessage.py** bestand.

1. Voeg de volgende `import` -instructies en variabelen aan het begin van de **SendCloudToDeviceMessage.py** bestand:
   
    ```python
    import random
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubMessaging, IoTHubMessage, IoTHubError

    OPEN_CONTEXT = 0
    FEEDBACK_CONTEXT = 1
    MESSAGE_COUNT = 1
    AVG_WIND_SPEED = 10.0
    MSG_TXT = "{\"service client sent a message\": %.2f}"
    ```

1. Voeg de volgende code naar **SendCloudToDeviceMessage.py** bestand. Vervang de waarde van de tijdelijke aanduiding '{IoTHubConnectionString}' door de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de [aan de slag met IoT Hub] zelfstudie. De '{deviceId}'-tijdelijke aanduiding vervangen door de apparaat-ID van het apparaat dat u hebt toegevoegd in de [aan de slag met IoT Hub] zelfstudie:
   
    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"
    ```

1. Voeg de volgende functie om de Bewerkingsresultaten aan de console afdrukken:
   
    ```python
    function printResultFor(op) {
      return function printResult(err, res) {
        if (err) console.log(op + ' error: ' + err.toString());
        if (res) console.log(op + ' status: ' + res.constructor.name);
      };
    }
    ```

1. Voeg de volgende functie zodat deze Feedbackberichten naar de console afdrukken:
   
    ```python
    def open_complete_callback(context):
        print ( 'open_complete_callback called with context: {0}'.format(context) )

    def send_complete_callback(context, messaging_result):
        context = 0
        print ( 'send_complete_callback called with context : {0}'.format(context) )
        print ( 'messagingResult : {0}'.format(messaging_result) )
    ```

1. Voeg de volgende code voor het verzenden van een bericht naar uw apparaat en verwerken van het Feedbackbericht wanneer het apparaat het cloud-naar-apparaat-bericht erkent:
   
    ```python
    def iothub_messaging_sample_run():
        try:
            iothub_messaging = IoTHubMessaging(CONNECTION_STRING)

            iothub_messaging.open(open_complete_callback, OPEN_CONTEXT)

            for i in range(0, MESSAGE_COUNT):
                print ( 'Sending message: {0}'.format(i) )
                msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))
                message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))

                # optional: assign ids
                message.message_id = "message_%d" % i
                message.correlation_id = "correlation_%d" % i
                # optional: assign properties
                prop_map = message.properties()
                prop_text = "PropMsg_%d" % i
                prop_map.add("Property", prop_text)

                iothub_messaging.send_async(DEVICE_ID, message, send_complete_callback, i)

            try:
                # Try Python 2.xx first
                raw_input("Press Enter to continue...\n")
            except:
                pass
                # Use Python 3.xx in the case of exception
                input("Press Enter to continue...\n")

            iothub_messaging.close()

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubMessaging sample stopped" )
    ```

1. De volgende belangrijke functie toevoegen:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client Messaging Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_messaging_sample_run()
    ```

1. Opslaan en sluiten **SendCloudToDeviceMessage.py** bestand.


## <a name="run-the-applications"></a>De toepassingen uitvoeren
U kunt nu de toepassingen gaan uitvoeren.

1. Open een opdrachtprompt en installeer de **Azure IoT Hub apparaat-SDK voor Python**.

    ```
    pip install azure-iothub-device-client
    ```

1. Voer de volgende opdracht om te luisteren naar de cloud-naar-apparaat-berichten bij de opdrachtprompt:
   
    ```shell
    python SimulatedDevice.py 
    ```
   
    ![De gesimuleerde apparaattoepassing uitvoeren][img-simulated-device]

1. Open een nieuw opdrachtpromptvenster en installeer de **Azure IoT Hub Service SDK voor Python**.

    ```
    pip install azure-iothub-service-client
    ```

1. Voer de volgende opdracht om een cloud naar apparaat verzenden en wacht tot de feedback bericht bij een opdrachtprompt:
   
    ```shell
    python SendCloudToDeviceMessage.py 
    ```
   
    ![Voer de app voor het verzenden van de opdracht cloud-naar-apparaat][img-send-command]
   
1. Let op het bericht dat is ontvangen door het apparaat.

    ![Bericht ontvangen][img-message-recieved]


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe cloud-naar-apparaat-berichten verzenden en ontvangen. 

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT Hub [Azure IoT Suite].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub].

<!-- Images -->
[img-simulated-device]: media/iot-hub-python-python-c2d/simulated-device.png
[img-send-command]:  media/iot-hub-python-python-c2d/send-command.png
[img-message-recieved]: media/iot-hub-python-python-c2d/message-recieved.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[aan de slag met IoT Hub]: iot-hub-node-node-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[Ontwikkelaarshandleiding voor IoT Hub]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portal]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
