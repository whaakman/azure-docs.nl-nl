---
title: Cloud-naar-apparaat-berichten met Azure IoT Hub (Python) | Microsoft Docs
description: Klik hier voor meer informatie over het cloud-naar-apparaat-berichten verzenden naar een apparaat van een Azure IoT-hub met behulp van de Azure IoT SDK's voor Python. Een gesimuleerde apparaat-app voor het ontvangen van berichten van cloud-naar-apparaat en het wijzigen van een back-end-app om de cloud-naar-apparaat-berichten te verzenden kunt u wijzigen.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 02/22/2019
ms.author: kgremban
ms.openlocfilehash: 0feff40aff4db65104cb2531881119086dc813a7
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57541907"
---
# <a name="send-cloud-to-device-messages-with-iot-hub-python"></a>Cloud-naar-apparaat-berichten verzenden met IoT Hub (Python)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]


## <a name="introduction"></a>Inleiding
Azure IoT Hub is een volledig beheerde service die stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten inschakelen en een back-end oplossing. De [aan de slag met IoT Hub] zelfstudie laat zien hoe u een IoT-hub maken, een apparaat-id in het inrichten en code van een gesimuleerde apparaat-app dat apparaat-naar-cloud-berichten verzendt.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze zelfstudie bouwt voort op [aan de slag met IoT Hub]. Hier ziet u hoe aan:

* Vanuit de back-end, cloud-naar-apparaat-berichten naar een enkel apparaat via IoT Hub te verzenden.
* Cloud-naar-apparaat-berichten op een apparaat ontvangen.
* Aanvragen van de back-end, levering bevestiging (*feedback*) voor berichten die worden verzonden naar een apparaat vanuit IoT Hub.

U vindt meer informatie over cloud-naar-apparaat-berichten in de [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide-messaging.md).

Aan het einde van deze zelfstudie, moet u twee Python-console-apps uitvoeren:

* **SimulatedDevice.py**, een aangepaste versie van de app gemaakt [aan de slag met IoT Hub], die verbinding maakt met uw IoT-hub en cloud-naar-apparaat-berichten worden ontvangen.
* **SendCloudToDeviceMessage.py**, die een cloud-naar-apparaat-bericht naar de gesimuleerde apparaattoepassing via IoT Hub verzendt en ontvangt u vervolgens de bevestiging levering.

> [!NOTE]
> IoT-Hub heeft SDK-ondersteuning voor vele platformen voor apparaten en talen (waaronder C, Java en Javascript) via Azure IoT device-SDK's. Zie voor stapsgewijze instructies voor het verbinding maken tussen uw apparaat in de code van deze zelfstudie, en in het algemeen voor Azure IoT Hub, de [Azure IoT-ontwikkelaarscentrum](https://www.azure.com/develop/iot).
> 

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x](https://www.python.org/downloads/). Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk [pip *installeren of upgraden*, het Python-pakketbeheersysteem](https://pip.pypa.io/en/stable/installing/).
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++](https://www.microsoft.com/download/confirmation.aspx?id=48145) om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

> [!NOTE]
> De *pip*-pakketten voor `azure-iothub-service-client` en `azure-iothub-device-client` zijn momenteel alleen beschikbaar voor het Windows-besturingssysteem. Voor Linux/Mac OS, raadpleegt u de Linux- en Mac OS-specifieke secties in het [uw ontwikkelomgeving voorbereiden voor Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) plaatsen.
> 


## <a name="receive-messages-in-the-simulated-device-app"></a>Berichten ontvangen in het gesimuleerde apparaat-app
In deze sectie maakt u een Python-console-app voor het simuleren van het apparaat en cloud-naar-apparaat-berichten ontvangen van de IoT-hub.

1. Maak met een teksteditor een **SimulatedDevice.py** bestand.

1. Voeg de volgende `import` instructies en -variabelen aan het begin van de **SimulatedDevice.py** bestand:
   
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

1. Voeg de volgende code aan **SimulatedDevice.py** bestand. Vervang de waarde van de tijdelijke aanduiding '{deviceConnectionString}' met de verbindingsreeks voor het apparaat dat u hebt gemaakt in de [aan de slag met IoT Hub] zelfstudie:
   
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

1. Voeg de volgende code om te initialiseren van de client en wachten om de cloud-naar-apparaat-bericht te ontvangen:
   
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

1. Voeg de volgende belangrijkste functie:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_sample_run()
    ```

1. Opslaan en sluiten **SimulatedDevice.py** bestand.


## <a name="send-a-cloud-to-device-message"></a>Een cloud-naar-apparaat-bericht verzenden
In deze sectie maakt maken u een Python-consoletoepassing die cloud-naar-apparaat-berichten naar de gesimuleerde apparaattoepassing verzendt. U moet de apparaat-ID van het apparaat dat u hebt toegevoegd in de [aan de slag met IoT Hub] zelfstudie. U moet ook de IoT Hub-verbindingsreeks voor uw hub die u kunt vinden in de [Azure-portal](https://portal.azure.com).

1. Maak met een teksteditor een **SendCloudToDeviceMessage.py** bestand.

1. Voeg de volgende `import` instructies en -variabelen aan het begin van de **SendCloudToDeviceMessage.py** bestand:
   
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

1. Voeg de volgende code aan **SendCloudToDeviceMessage.py** bestand. Vervang de waarde '{IoTHubConnectionString}'-tijdelijke aanduiding door de IoT Hub-verbindingsreeks voor de hub die u hebt gemaakt in de [aan de slag met IoT Hub] zelfstudie. Vervang de tijdelijke aanduiding voor '{deviceId}' met de apparaat-ID van het apparaat dat u hebt toegevoegd in de [aan de slag met IoT Hub] zelfstudie:
   
    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"
    ```

1. Voeg de volgende functie als u wilt afdrukken van van Feedbackberichten naar de console:
   
    ```python
    def open_complete_callback(context):
        print ( 'open_complete_callback called with context: {0}'.format(context) )

    def send_complete_callback(context, messaging_result):
        context = 0
        print ( 'send_complete_callback called with context : {0}'.format(context) )
        print ( 'messagingResult : {0}'.format(messaging_result) )
    ```

1. Voeg de volgende code om een bericht verzenden naar uw apparaat en de verwerking van het Feedbackbericht wanneer het apparaat het cloud-naar-apparaat bericht erkent:
   
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

1. Voeg de volgende belangrijkste functie:
   
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

1. Voer de volgende opdracht om te luisteren naar berichten van cloud-naar-apparaat bij de opdrachtprompt:
   
    ```shell
    python SimulatedDevice.py 
    ```
   
    ![De gesimuleerde apparaat-app uitvoeren][img-simulated-device]

1. Open een nieuwe opdrachtprompt en installeer de **Azure IoT Hub-Service-SDK voor Python**.

    ```
    pip install azure-iothub-service-client
    ```

1. Voer de volgende opdracht om een cloud-naar-apparaat-bericht verzenden en wacht tot de feedback bericht bij een opdrachtprompt:
   
    ```shell
    python SendCloudToDeviceMessage.py 
    ```
   
    ![De app voor het verzenden van de cloud-naar-apparaat-opdracht uitvoeren][img-send-command]
   
1. Houd er rekening mee het bericht wordt ontvangen door het apparaat.

    ![Bericht ontvangen][img-message-received]


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u cloud-naar-apparaat-berichten verzenden en ontvangen. 

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT-Hub [Azure IoT Remote Monitoring solution accelerator](https://azure.microsoft.com/documentation/suites/iot-suite/).

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md).

<!-- Images -->
[img-simulated-device]: media/iot-hub-python-python-c2d/simulated-device.png
[img-send-command]:  media/iot-hub-python-python-c2d/send-command.png
[img-message-received]: media/iot-hub-python-python-c2d/message-received.png

<!-- Links -->
[Aan de slag met IoT Hub]: quickstart-send-telemetry-python.md
