---
title: Aan de slag met Azure IoT Hub (Python) | Microsoft Docs
description: Informatie over het verzenden van apparaat-naar-cloud-berichten naar Azure IoT Hub met behulp van IoT SDK's voor Python. U maakt gesimuleerde apparaat- en service-apps om uw apparaat te registreren, berichten te verzenden en berichten uit IoT Hub te lezen.
services: iot-hub
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: python
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: dkshir
ms.custom: na
ms.openlocfilehash: 7ebbac4464d793717f68a4cb7905c53d1f5c051a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connect-your-simulated-device-to-your-iot-hub-using-python"></a>Uw gesimuleerde apparaat verbinding laten maken met uw IoT Hub met Python
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Aan het einde van deze zelfstudie hebt u twee Python-apps:

* **CreateDeviceIdentity.py**: deze app maakt een apparaat-id en de bijbehorende beveiligingssleutel waarmee uw gesimuleerde apparaat-app kan worden verbonden.
* **SimulatedDevice.py**: deze app koppelt uw IoT Hub aan de apparaat-id die u eerder hebt gemaakt en verzendt regelmatig een telemetriebericht via het MQTT-protocol.

> [!NOTE]
> Het artikel [Azure IoT-SDK's][lnk-hub-sdks] bevat informatie over de verschillende Azure IoT-SDK's die u kunt gebruiken om beide toepassingen zo te maken dat ze zowel op het apparaat als op de back-end van uw oplossing kunnen worden uitgevoerd.
> 
> 

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x][lnk-python-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk *pip* [installeren of upgraden, het Python-pakketbeheersysteem][lnk-install-pip].
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++][lnk-visual-c-redist] om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* [Node.js 4.0 of hoger][lnk-node-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Dit is nodig voor het installeren van het [hulpprogramma IoT Hub-verkenner][lnk-iot-hub-explorer].
* Een actief Azure-account. Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.

> [!NOTE]
> De *pip*-pakketten voor `azure-iothub-service-client` en `azure-iothub-device-client` zijn momenteel alleen beschikbaar voor het Windows-besturingssysteem. Voor Linux/Mac OS raadpleegt u de specifieke secties voor die besturingssystemen in het Engelstalige artikel [Prepare your development environment for Python][lnk-python-devbox].
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

U hebt nu uw IoT-hub gemaakt. Gebruik de IoT Hub-hostnaam en de IoT Hub-verbindingsreeks in de rest van deze zelfstudie.

> [!NOTE]
> U kunt uw IoT Hub ook gemakkelijk via een opdrachtregel maken met behulp van de op Python of Node.js gebaseerde Azure CLI. Het artikel [Create an IoT hub using the Azure CLI 2.0][lnk-azure-cli-hub] (Een IoT Hub maken met behulp van de Azure CLI 2.0) bevat de snelle stappen om dit te doen. 
> 

## <a name="create-a-device-identity"></a>Een apparaat-id maken
Dit gedeelte bevat de stappen om een Python-console-app te maken die een apparaat-id kan maken in het id-register van uw IoT Hub. Een apparaat kan alleen verbinding maken met de IoT Hub als het vermeld staat in het id-register. Zie het gedeelte **Id-register** in de [ontwikkelaarshandleiding voor IoT Hub][lnk-devguide-identity] voor meer informatie. Wanneer u deze consoletoepassing uitvoert, worden er een unieke apparaat-id en sleutel gegenereerd waarmee uw apparaat zichzelf kan identificeren tijdens het verzenden van apparaat-naar-cloud-berichten naar IoT Hub.

1. Open een opdrachtprompt en installeer de **SDK voor de Azure IoT Hub-service voor Python**. Sluit de opdrachtprompt na de installatie van de SDK.

    ```
    pip install azure-iothub-service-client
    ```

2. Maak een Python-bestand genaamd **CreateDeviceIdentity.py**. Open dit in [een Python-editor/IDE naar keuze][lnk-python-ide-list], bijvoorbeeld de standaard [IDLE][lnk-idle].

3. Voeg de volgende code toe om de vereiste modules uit de service-SDK te importeren:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceStatus, IoTHubError
    ```
2. Voeg de volgende code toe en vervang de tijdelijke aanduiding door `[IoTHub Connection String]` met de verbindingsreeks voor de IoT Hub die u in het vorige gedeelte hebt gemaakt. U kunt een willekeurige naam gebruiken voor de `DEVICE_ID`.
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "MyFirstPythonDevice"
    ```
   [!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

3. Voeg de volgende functie toe om bepaalde apparaatgegevens af te drukken.

    ```python
    def print_device_info(title, iothub_device):
        print ( title + ":" )
        print ( "iothubDevice.deviceId                    = {0}".format(iothub_device.deviceId) )
        print ( "iothubDevice.primaryKey                  = {0}".format(iothub_device.primaryKey) )
        print ( "iothubDevice.secondaryKey                = {0}".format(iothub_device.secondaryKey) )
        print ( "iothubDevice.connectionState             = {0}".format(iothub_device.connectionState) )
        print ( "iothubDevice.status                      = {0}".format(iothub_device.status) )
        print ( "iothubDevice.lastActivityTime            = {0}".format(iothub_device.lastActivityTime) )
        print ( "iothubDevice.cloudToDeviceMessageCount   = {0}".format(iothub_device.cloudToDeviceMessageCount) )
        print ( "iothubDevice.isManaged                   = {0}".format(iothub_device.isManaged) )
        print ( "iothubDevice.authMethod                  = {0}".format(iothub_device.authMethod) )
        print ( "" )
    ```
3. Voeg de volgende functie toe om de apparaat-id te maken met behulp van het registerbeheer. 

    ```python
    def iothub_createdevice():
        try:
            iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
            auth_method = IoTHubRegistryManagerAuthMethod.SHARED_PRIVATE_KEY
            new_device = iothub_registry_manager.create_device(DEVICE_ID, "", "", auth_method)
            print_device_info("CreateDevice", new_device)

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "iothub_createdevice stopped" )
    ```
4. Voeg ten slotte als volgt de hoofdfunctie toe en sla het bestand op.

    ```python
    if __name__ == '__main__':
        print ( "" )
        print ( "Python {0}".format(sys.version) )
        print ( "Creating device using the Azure IoT Hub Service SDK for Python" )
        print ( "" )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_createdevice()
    ```
5. Voer in de opdrachtprompt als volgt de **CreateDeviceIdentity.py** uit:

    ```python
    python CreateDeviceIdentity.py
    ```
6. Als het goed is, ziet u nu dat het gesimuleerde apparaat wordt gemaakt. Noteer de **deviceId** en **primaryKey** van het apparaat. U hebt deze waarden later nodig wanneer u een toepassing maakt die verbinding maakt met IoT Hub als apparaat.

    ![Apparaat maken voltooid][1]

> [!NOTE]
> In het id-register van IoT Hub worden alleen apparaat-id's opgeslagen waarmee veilig toegang tot de IoT-hub kan worden verkregen. De apparaat-id’s en sleutels worden opgeslagen en gebruikt als beveiligingsreferenties. Met de vlag voor ingeschakeld/uitgeschakeld kunt u toegang tot een afzonderlijk apparaat uitschakelen. Als uw toepassing andere apparaatspecifieke metagegevens moet opslaan, moet deze een toepassingsspecifieke opslagmethode gebruiken. Zie de [ontwikkelaarshandleiding voor IoT Hub][lnk-devguide-identity] voor meer informatie.
> 
> 


## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
Dit gedeelte bevat de stappen voor het maken van een Python-console-app die een apparaat simuleert en apparaat-naar-cloud-berichten naar uw IoT Hub verzendt.

1. Open een nieuwe opdrachtprompt en installeer als volgt de apparaat-SDK voor Azure IoT Hub voor Python. Sluit de opdrachtprompt na de installatie.

    ```
    pip install azure-iothub-device-client
    ```
2. Maak een bestand genaamd **SimulatedDevice.py**. Open het bestand in een Python-editor/IDE naar keuze, bijvoorbeeld IDLE.

3. Voeg de volgende code toe om de vereiste modules uit de apparaat-SDK te importeren.

    ```python
    import random
    import time
    import sys
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubMessage, IoTHubMessageDispositionResult, IoTHubError, DeviceMethodReturnValue
    ```
4. Voeg de volgende code toe en vervang de tijdelijke aanduiding voor `[IoTHub Device Connection String]` door de verbindingsreeks voor uw apparaat. De verbindingsreeks van het apparaat heeft meestal de indeling `HostName=<hostName>;DeviceId=<deviceId>;SharedAccessKey=<primaryKey>`. Gebruik de **deviceId** en **primaryKey** van het apparaat dat u in het vorige gedeelte hebt gemaakt om respectievelijk de `<deviceId>` en `<primaryKey>` te vervangen. Vervang `<hostName>` door de hostnaam van uw IoT Hub, meestal ingedeeld als `<IoT hub name>.azure-devices.net`.

    ```python
    # String containing Hostname, Device Id & Device Key in the format
    CONNECTION_STRING = "[IoTHub Device Connection String]"
    # choose HTTP, AMQP or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT
    MESSAGE_TIMEOUT = 10000
    AVG_WIND_SPEED = 10.0
    SEND_CALLBACKS = 0
    MSG_TXT = "{\"deviceId\": \"MyFirstPythonDevice\",\"windSpeed\": %.2f}"    
    ```
5. Voeg de volgende code toe om een terugbeloproep voor het verzenden van bevestiging te definiëren. 

    ```python
    def send_confirmation_callback(message, result, user_context):
        global SEND_CALLBACKS
        print ( "Confirmation[%d] received for message with result = %s" % (user_context, result) )
        map_properties = message.properties()
        print ( "    message_id: %s" % message.message_id )
        print ( "    correlation_id: %s" % message.correlation_id )
        key_value_pair = map_properties.get_internals()
        print ( "    Properties: %s" % key_value_pair )
        SEND_CALLBACKS += 1
        print ( "    Total calls confirmed: %d" % SEND_CALLBACKS )
    ```
6. Voeg de volgende code toe om de apparaat-client te initialiseren.

    ```python
    def iothub_client_init():
        # prepare iothub client
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
        # set the time until a message times out
        client.set_option("messageTimeout", MESSAGE_TIMEOUT)
        client.set_option("logtrace", 0)
        return client
    ```
7. Voeg de volgende functie toe om een bericht te formatteren en van het gesimuleerde apparaat naar uw IoT Hub te verzenden.

    ```python
    def iothub_client_telemetry_sample_run():

        try:
            client = iothub_client_init()
            print ( "IoT Hub device sending periodic messages, press Ctrl-C to exit" )
            message_counter = 0

            while True:
                msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random.random() * 4 + 2))
                # messages can be encoded as string or bytearray
                if (message_counter & 1) == 1:
                    message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))
                else:
                    message = IoTHubMessage(msg_txt_formatted)
                # optional: assign ids
                message.message_id = "message_%d" % message_counter
                message.correlation_id = "correlation_%d" % message_counter
                # optional: assign properties
                prop_map = message.properties()
                prop_text = "PropMsg_%d" % message_counter
                prop_map.add("Property", prop_text)

                client.send_event_async(message, send_confirmation_callback, message_counter)
                print ( "IoTHubClient.send_event_async accepted message [%d] for transmission to IoT Hub." % message_counter )

                status = client.get_send_status()
                print ( "Send status: %s" % status )
                time.sleep(30)

                status = client.get_send_status()
                print ( "Send status: %s" % status )

                message_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
    ```
8. Voeg ten slotte de hoofdfunctie toe. 

    ```python
    if __name__ == '__main__':
        print ( "Simulating a device using the Azure IoT Hub Device SDK for Python" )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_telemetry_sample_run()
    ```
9. Sla het bestand **SimulatedDevice.py** op en sluit het. U kunt nu de app uitvoeren.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u een beleid voor opnieuw proberen implementeren (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel [Transient Fault Handling][lnk-transient-faults] (Afhandeling van tijdelijke fouten).
> 
> 

## <a name="receive-messages-from-your-simulated-device"></a>Berichten ontvangen van uw gesimuleerde apparaat
Als u telemetrieberichten van uw apparaat wilt ontvangen, moet u een met [Event Hubs][lnk-event-hubs-overview] compatibel eindpunt gebruiken dat wordt weergegeven door de IoT Hub die de apparaat-naar-cloud-berichten leest. Raadpleeg de zelfstudie [Aan de slag met Event Hubs][lnk-eventhubs-tutorial] voor informatie over het verwerken van berichten van Event Hubs voor de Event Hub-compatibele eindpunten van uw IoT Hub. Event Hubs biedt nog geen ondersteuning voor telemetrie in Python. U kunt een op Event Hubs gebaseerde console-app met [Node.js](iot-hub-node-node-getstarted.md#D2C_node) of [.NET](iot-hub-csharp-csharp-getstarted.md#D2C_csharp) maken voor het lezen van de apparaat-naar-cloud-berichten van de IoT Hub. In deze zelfstudie wordt uitgelegd hoe u het [hulpprogramma IoT Hub-verkenner][lnk-iot-hub-explorer] kunt gebruiken om deze apparaatberichten te lezen.

1. Open een opdrachtprompt en installeer de IoT Hub-verkenner. 

    ```
    npm install -g iothub-explorer
    ```

2. Voer de volgende opdracht uit in de opdrachtprompt om de apparaat-naar-cloud-berichten van uw apparaat bij te houden. Gebruik de verbindingsreeks van uw IoT Hub in de tijdelijke aanduiding na `--login`.

    ```
    iothub-explorer monitor-events MyFirstPythonDevice --login "[IoTHub connection string]"
    ```

3. Open een nieuwe opdrachtprompt en navigeer naar de map met het bestand **SimulatedDevice.py**.

4. Voer het bestand **SimulatedDevice.py** uit. Dit bestand stuurt periodiek telemetriegegevens naar uw IoT Hub. 
   
    ```
    python SimulatedDevice.py
    ```
5. Bekijk de apparaatberichten in de opdrachtprompt die in het vorige gedeelte werd uitgevoerd in de IoT Hub-verkenner. 

    ![Apparaat-naar-cloud-berichten met Python][2]

## <a name="next-steps"></a>Volgende stappen
In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U hebt deze apparaat-id gebruikt om de gesimuleerde apparaattoepassing in staat te stellen apparaat-naar-cloud-berichten te verzenden naar de IoT-hub. U hebt de berichten bekeken die door de IoT Hub werden ontvangen met behulp van het hulpprogramma IoT Hub-verkenner. 

Ga naar [deze Git Hub-opslagplaats][lnk-python-github] om de Python-SDK voor gebruik met Azure IoT Hub verder te verkennen. Als u de berichtmogelijkheden van de service-SDK van Azure IoT Hub voor Python wilt controleren, kunt u [iothub_messaging_sample.py][lnk-messaging-sample] downloaden en uitvoeren. Voor simulatie aan de apparaatzijde met behulp van de apparaat-SDK van Azure IoT Hub voor Python, kunt u [iothub_client_sample.py][lnk-client-sample] downloaden en uitvoeren.

Als u aan de slag wilt gaan met IoT Hub en andere IoT-scenario's wilt verkennen, leest u deze artikelen:

* [Verbinding maken met uw apparaat][lnk-connect-device]
* [Aan de slag met apparaatbeheer][lnk-device-management]
* [Aan de slag met Azure IoT Edge][lnk-iot-edge]

Raadpleeg de zelfstudie [Apparaat-naar-cloud-berichten verwerken][lnk-process-d2c-tutorial] voor meer informatie over hoe u uw IoT-oplossing uitbreidt en apparaat-naar-cloud-berichten op schaal verwerkt.
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[1]: ./media/iot-hub-python-getstarted/createdevice.png
[2]: ./media/iot-hub-python-getstarted/sendd2cmessage.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-azure-cli-hub]: https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-using-cli
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-idle]: https://docs.python.org/3/library/idle.html
[lnk-python-ide-list]: https://wiki.python.org/moin/IntegratedDevelopmentEnvironments
[lnk-iot-hub-explorer]: https://github.com/Azure/iothub-explorer
[lnk-python-github]: https://github.com/Azure/azure-iot-sdk-python
[lnk-messaging-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/service/samples/iothub_messaging_sample.py
[lnk-client-sample]: https://github.com/Azure/azure-iot-sdk-python/blob/master/device/samples/iothub_client_sample.py

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-python-devbox]: https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md

[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
