---
title: Aan de slag met Azure IoT Hub apparaat horende (Python) | Microsoft Docs
description: Het gebruik van Azure IoT Hub apparaat horende labels toevoegen en vervolgens met de query voor een IoT Hub. U kunt Azure IoT SDK's voor Python gebruiken voor het implementeren van de gesimuleerde apparaattoepassing en een service-app die de labels worden toegevoegd en de IoT Hub-query wordt uitgevoerd.
services: iot-hub
documentationcenter: python
author: msebolt
manager: timlt
editor: 
ms.assetid: 314c88e4-cce1-441c-b75a-d2e08e39ae7d
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/04/2017
ms.author: v-masebo
ms.openlocfilehash: 20c1eeee6ca690ddcf0b9489b88689213b79488e
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="get-started-with-device-twins-python"></a>Aan de slag met apparaat horende (Python)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Aan het einde van deze zelfstudie hebt u twee Python console apps:

* **AddTagsAndQuery.py**, een back-end-app van Python, die labels toegevoegd en wordt opgevraagd horende apparaten.
* **ReportConnectivity.py**, een Python-app, die een apparaat simuleert dat verbinding met uw IoT-hub aan de apparaat-id eerder hebt gemaakt maakt en de voorwaarde connectiviteit rapporten.

> [!NOTE]
> Het artikel [Azure IoT SDK's] [ lnk-hub-sdks] bevat informatie over de Azure IoT SDK's dat u gebruiken kunt om zowel apparaatgegevens als back-end-apps te bouwen.
> 
> 

Voor deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x][lnk-python-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk *pip* [installeren of upgraden, het Python-pakketbeheersysteem][lnk-install-pip].
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++][lnk-visual-c-redist] om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

> [!NOTE]
> De *pip*-pakketten voor `azure-iothub-service-client` en `azure-iothub-device-client` zijn momenteel alleen beschikbaar voor het Windows-besturingssysteem. Voor Linux/Mac OS raadpleegt u de specifieke secties voor die besturingssystemen in het Engelstalige artikel [Prepare your development environment for Python][lnk-python-devbox].
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-service-app"></a>De service-app maken
In deze sectie maakt u een Python consoletoepassing maken die locatie metagegevens toegevoegd aan de apparaat-twin die zijn gekoppeld aan uw **{apparaat-Id}**. Deze bevraagt vervolgens het apparaat horende opgeslagen in de IoT-hub te selecteren van de apparaten die zich in Redmond en degene die een mobiele verbinding rapporteren.

1. Open een opdrachtprompt en installeer de **SDK voor de Azure IoT Hub-service voor Python**. Sluit de opdrachtprompt na de installatie van de SDK.

    ```
    pip install azure-iothub-service-client
    ```

1. Start een teksteditor en maak een nieuwe **AddTagsAndQuery.py** bestand.

3. Voeg de volgende code toe om de vereiste modules uit de service-SDK te importeren:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceTwin, IoTHubError
    ```
2. Voeg de volgende code, vervang de tijdelijke aanduiding voor `[IoTHub Connection String]` en `[Device Id]` met de verbindingsreeks voor de IoT-hub en de apparaat-id die u hebt gemaakt in de vorige secties.
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "[Device Id]"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"location\":\"Redmond\"}}}"

    UPDATE_JSON_SEARCH = "\"location\":\"Redmond\""
    UPDATE_JSON_CLIENT_SEARCH = "\"connectivity\":\"cellular\""
    ```

1. Voeg de volgende code naar de **AddTagsAndQuery.py** bestand:
   
     ```python
    def iothub_service_sample_run():
        try:
            iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
        
            iothub_registry_statistics = iothub_registry_manager.get_statistics()
            print ( "Total device count                       : {0}".format(iothub_registry_statistics.totalDeviceCount) )
            print ( "Enabled device count                     : {0}".format(iothub_registry_statistics.enabledDeviceCount) )
            print ( "Disabled device count                    : {0}".format(iothub_registry_statistics.disabledDeviceCount) )
            print ( "" )

            number_of_devices = iothub_registry_statistics.totalDeviceCount
            dev_list = iothub_registry_manager.get_device_list(number_of_devices)
        
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)

            for device in range(0, number_of_devices):
                if dev_list[device].deviceId == DEVICE_ID:
                    twin_info = iothub_twin_method.update_twin(dev_list[device].deviceId, UPDATE_JSON)
        
            print ( "Devices in Redmond: " )        
            for device in range(0, number_of_devices):
                twin_info = iothub_twin_method.get_twin(dev_list[device].deviceId)
         
                if twin_info.find(UPDATE_JSON_SEARCH) > -1:
                    print ( dev_list[device].deviceId )
        
            print ( "" )
        
            print ( "Devices in Redmond using cellular network: " )
            for device in range(0, number_of_devices):
                twin_info = iothub_twin_method.get_twin(dev_list[device].deviceId)
                
                if twin_info.find(UPDATE_JSON_SEARCH) > -1:
                    if twin_info.find(UPDATE_JSON_CLIENT_SEARCH) > -1:
                        print ( dev_list[device].deviceId )

        except IoTHubError as iothub_error:
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "IoTHub sample stopped" )
    ```
   
    De **register** object bevat de methoden die zijn vereist voor interactie met horende apparaten van de service. De code eerst initialiseert de **register** object en vervolgens het apparaat twin voor updates **deviceId**, en ten slotte twee query's uitvoert. De eerste selecteert alleen de apparaat-horende apparaten zich in de **Redmond43** plant en de tweede verfijning de query voor het selecteren van alleen de apparaten die ook via het mobiele netwerk zijn verbonden.
   
1. Voeg de volgende code toe aan het einde van **AddTagsAndQuery.py** voor het implementeren van de **iothub_service_sample_run** functie:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python service sample..." )

        iothub_service_sample_run()
    ```

1. Voer de toepassing met:
   
    ```cmd/sh
    python AddTagsAndQuery.py
    ```
   
    U ziet één apparaat in de resultaten van de query wordt gevraagd voor alle apparaten vinden in **Redmond43** en er is geen voor de query die de resultaten beperkt tot apparaten die gebruikmaken van een mobiel netwerk.
   
    ![eerste query][1]

In de volgende sectie maakt u een apparaat-app die rapporten de informatie over de connectiviteit en het resultaat van de query in de vorige sectie wordt gewijzigd.

## <a name="create-the-device-app"></a>De apparaat-app maken
In deze sectie maakt u een Python consoletoepassing maken die verbinding met uw hub als maakt uw **{apparaat-Id}**, en vervolgens de updates die de apparaat-twin de eigenschappen van de informatie dat is verbonden met een mobiel netwerk bevatten gerapporteerd.

1. Open een opdrachtprompt en installeer de **SDK voor de Azure IoT Hub-service voor Python**. Sluit de opdrachtprompt na de installatie van de SDK.

    ```
    pip install azure-iothub-device-client
    ```

1. Start een teksteditor en maak een nieuwe **ReportConnectivity.py** bestand.

3. Voeg de volgende code toe om de vereiste modules uit de service-SDK te importeren:

    ```python
    import time
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError
    ```

2. Voeg de volgende code, vervang de tijdelijke aanduiding voor `[IoTHub Device Connection String]` met de verbindingsreeks voor de IoT hub-apparaat die u hebt gemaakt in de vorige secties.
   
    ```python
    CONNECTION_STRING = "[IoTHub Device Connection String]"

    # choose HTTP, AMQP, AMQP_WS or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT

    TIMER_COUNT = 5
    TWIN_CONTEXT = 0
    SEND_REPORTED_STATE_CONTEXT = 0
    ```

1. Voeg de volgende code naar de **ReportConnectivity.py** bestand voor het implementeren van het apparaat horende functionaliteit:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        print ( "" )
        print ( "Twin callback called with:" )
        print ( "    updateStatus: %s" % update_state )
        print ( "    payload: %s" % payload )

    def send_reported_state_callback(status_code, user_context):
        print ( "" )
        print ( "Confirmation for reported state called with:" )
        print ( "    status_code: %d" % status_code )

    def iothub_client_init():
        client = IoTHubClient(CONNECTION_STRING, PROTOCOL)

        if client.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            client.set_device_twin_callback(
                device_twin_callback, TWIN_CONTEXT)

        return client

    def iothub_client_sample_run():
        try:
            client = iothub_client_init()

            if client.protocol == IoTHubTransportProvider.MQTT:
                print ( "Sending data as reported property..." )

                reported_state = "{\"connectivity\":\"cellular\"}"

                client.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)

            while True:
                print ( "Press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= TIMER_COUNT:
                    status = client.get_send_status()
                    time.sleep(10)
                    status_counter += 1 
        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )
    ```   

    De **Client** object bevat de methoden die u nodig hebt om te communiceren met horende apparaten van het apparaat. De vorige code nadat het initialiseren van de **Client** object, haalt de apparaat-twin voor uw apparaat en de bijbehorende eigenschap gerapporteerde verbindingsgegevens bijgewerkt door de.

1. Voeg de volgende code toe aan het einde van **ReportConnectivity.py** voor het implementeren van de **iothub_client_sample_run** functie:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python client sample..." )

        iothub_client_sample_run()
    ```

1. De apparaat-app uitvoeren
   
    ```cmd/sh
    python ReportConnectivity.js
    ```
   
    U ziet de bevestiging van die het apparaat horende zijn bijgewerkt.

    ![horende bijwerken][2]

6. Nu dat het apparaat heeft gemeld dat de gegevens over de connectiviteit, moet deze worden weergegeven in beide query's. Ga terug en voer de query opnieuw uit:
   
    ```cmd/sh
    python AddTagsAndQuery.js
    ```
   
    Nu uw **{apparaat-Id}** moet worden weergegeven in beide queryresultaten.
   
    ![tweede query][3]

## <a name="next-steps"></a>Volgende stappen
In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U metagegevens van apparaten als labels toegevoegd vanuit een back-end-app en een gesimuleerde apparaattoepassing geschreven naar apparaten connectiviteit rapportgegevens in de apparaat-twin. U hebt ook geleerd hoe query uitvoeren op deze informatie met het register.

Gebruik de volgende bronnen voor meer informatie over hoe:

* verzenden van telemetrie vanaf apparaten met de [aan de slag met IoT Hub] [ lnk-iothub-getstarted] zelfstudie
* apparaten configureren met de gewenste eigenschappen van apparaat twin met de [gebruik gewenst eigenschappen voor het configureren van apparaten] [ lnk-twin-how-to-configure] zelfstudie
* beheren van apparaten interactief (zoals het inschakelen van een ventilator van een gebruiker beheerde app), met de [direct methoden gebruiken] [ lnk-methods-tutorial] zelfstudie.

<!-- images -->
[1]: media/iot-hub-python-twin-getstarted/1.png
[2]: media/iot-hub-python-twin-getstarted/2.png
[3]: media/iot-hub-python-twin-getstarted/3.png

<!-- links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-python-devbox]: https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md

[lnk-d2c]: iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md

[lnk-iothub-getstarted]: iot-hub-python-getstarted.md
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/

[lnk-twin-how-to-configure]: iot-hub-node-node-twin-how-to-configure.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-methods-tutorial]: iot-hub-node-node-direct-methods.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
