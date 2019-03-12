---
title: Aan de slag met apparaatdubbels Azure IoT Hub (Python) | Microsoft Docs
description: Het gebruik van Azure IoT Hub-apparaatdubbels tags toevoegen en vervolgens een IoT Hub-query. U kunt de Azure IoT SDK's voor Python gebruiken voor het implementeren van het gesimuleerde apparaat-app en een service-app die wordt toegevoegd de labels en de IoT Hub-query wordt uitgevoerd.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: kgremban
ms.openlocfilehash: edf6fa98224613ba31eeed871cbb0eaf4e614600
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57535294"
---
# <a name="get-started-with-device-twins-python"></a>Aan de slag met apparaatdubbels (Python)
[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Aan het einde van deze zelfstudie hebt u twee Python-consoletoepassingen:

* **AddTagsAndQuery.py**, een Python back-end-app, die wordt toegevoegd tags en apparaatdubbels-query's.
* **ReportConnectivity.py**, een Python-app, die een apparaat simuleert dat verbinding met uw IoT-hub aan de apparaat-id die eerder hebt gemaakt maakt, en rapporten van de voorwaarde van de verbinding.

> [!NOTE]
> Het artikel [Azure IoT SDK's](iot-hub-devguide-sdks.md) bevat informatie over de Azure IoT SDK's die u gebruiken kunt om apparaat- en back-end-apps te bouwen.

Voor deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x](https://www.python.org/downloads/). Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk [pip *installeren of upgraden*, het Python-pakketbeheersysteem](https://pip.pypa.io/en/stable/installing/).
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++](https://www.microsoft.com/download/confirmation.aspx?id=48145) om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* Een actief Azure-account. (Als u geen account hebt, kunt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) binnen een paar minuten.)

> [!NOTE]
> De *pip*-pakketten voor `azure-iothub-service-client` en `azure-iothub-device-client` zijn momenteel alleen beschikbaar voor het Windows-besturingssysteem. Voor Linux/Mac OS, raadpleegt u de Linux- en Mac OS-specifieke secties in het [uw ontwikkelomgeving voorbereiden voor Python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) plaatsen.
> 

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Verbindingsreeks voor IoT-hub ophalen

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Een nieuw apparaat registreren in de IoT-hub

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="create-the-service-app"></a>De service-app maken
In deze sectie maakt u een Python-consoletoepassing die metagegevens van de locatie toegevoegd aan het dubbele apparaat dat is gekoppeld aan uw **{apparaat-ID}**. Deze vervolgens de dubbele apparaten die zijn opgeslagen in de IoT-hub selecteren van de apparaten die zich in Redmond, en vervolgens de virtuele machines die zijn melden van een mobiele verbinding een query.

1. Open een opdrachtprompt en installeer de **SDK voor de Azure IoT Hub-service voor Python**. Sluit de opdrachtprompt na de installatie van de SDK.

    ```
    pip install azure-iothub-service-client
    ```

1. Maak met een teksteditor een nieuw **AddTagsAndQuery.py** bestand.

3. Voeg de volgende code toe om de vereiste modules uit de service-SDK te importeren:

    ```python
    import sys
    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceTwin, IoTHubError
    ```
2. Voeg de volgende code, vervang de tijdelijke aanduiding voor `[IoTHub Connection String]` en `[Device Id]` door de verbindingsreeks voor de IoT-hub en de apparaat-ID die u hebt gemaakt in de vorige secties.
   
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "[Device Id]"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"location\":\"Redmond\"}}}"

    UPDATE_JSON_SEARCH = "\"location\":\"Redmond\""
    UPDATE_JSON_CLIENT_SEARCH = "\"connectivity\":\"cellular\""
    ```

1. Voeg de volgende code aan de **AddTagsAndQuery.py** bestand:
   
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
   
    De **register** object bevat de methoden die zijn vereist om te communiceren met de dubbele apparaten uit de service. De code voor het eerst wordt geïnitialiseerd de **register** object en vervolgens het apparaat twin voor updates **deviceId**, en ten slotte voert twee query's. De eerste selecteert alleen de apparaatdubbels van apparaten die zich in de **Redmond43** plant en het tweede verfijning van de query voor het selecteren van alleen de apparaten die ook zijn verbonden via mobiel netwerk.
   
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
   
    U ziet één apparaat in de resultaten voor het stellen van de query voor alle apparaten in **Redmond43** en niets in de query die de resultaten beperkt tot apparaten die gebruikmaken van een mobiel netwerk.
   
    ![eerste query][1]

In de volgende sectie maakt u een apparaat-app die de gegevens van hostconnectiviteit rapporten en het resultaat van de query in de vorige sectie wordt gewijzigd.

## <a name="create-the-device-app"></a>De apparaat-app maken
In deze sectie maakt u een Python-console-app die is verbonden met uw hub als uw **{apparaat-ID}**, en vervolgens de updates die de apparaatdubbel de gerapporteerde eigenschappen bevat de informatie die is verbonden met een mobiel netwerk.

1. Open een opdrachtprompt en installeer de **SDK voor de Azure IoT Hub-service voor Python**. Sluit de opdrachtprompt na de installatie van de SDK.

    ```
    pip install azure-iothub-device-client
    ```

1. Maak met een teksteditor een nieuw **ReportConnectivity.py** bestand.

3. Voeg de volgende code toe om de vereiste modules uit de service-SDK te importeren:

    ```python
    import time
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError
    ```

2. Voeg de volgende code, vervang de tijdelijke aanduiding voor `[IoTHub Device Connection String]` door de verbindingsreeks voor de IoT hub-apparaat die u hebt gemaakt in de vorige secties.
   
    ```python
    CONNECTION_STRING = "[IoTHub Device Connection String]"

    # choose HTTP, AMQP, AMQP_WS or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT

    TIMER_COUNT = 5
    TWIN_CONTEXT = 0
    SEND_REPORTED_STATE_CONTEXT = 0
    ```

1. Voeg de volgende code aan de **ReportConnectivity.py** bestand voor het implementeren van het apparaat dubbels functionaliteit:

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

    De **Client** object bevat de methoden die u nodig hebt om te communiceren met de dubbele apparaten van het apparaat. De vorige code, nadat het initialiseren van de **Client** object, haalt de apparaatdubbel voor uw apparaat en de gerapporteerde eigenschap updates met de gegevens van hostconnectiviteit.

1. Voeg de volgende code toe aan het einde van **ReportConnectivity.py** voor het implementeren van de **iothub_client_sample_run** functie:
   
    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python client sample..." )

        iothub_client_sample_run()
    ```

1. De apparaat-app uitvoeren
   
    ```cmd/sh
    python ReportConnectivity.py
    ```
   
    U ziet de bevestiging van die de dubbele apparaten zijn bijgewerkt.

    ![dubbele bijwerken][2]

6. Nu dat het apparaat heeft gemeld dat de gegevens over de connectiviteit, wordt deze weergegeven in beide query's. Ga terug en voer de query opnieuw uit:
   
    ```cmd/sh
    python AddTagsAndQuery.py
    ```
   
    Deze keer uw **{apparaat-ID}** moet worden weergegeven in de resultaten van beide query.
   
    ![tweede query][3]

## <a name="next-steps"></a>Volgende stappen
In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U metagegevens van apparaten als labels toegevoegd vanuit een back-end-app en een gesimuleerde apparaat-app geschreven naar apparaatgegevens met connectiviteit van rapport in de apparaatdubbel. U hebt ook geleerd hoe u query's van deze informatie met behulp van het register.

Gebruik de volgende bronnen voor meer informatie over het:

* Verzenden van telemetrie van apparaten met de [aan de slag met IoT Hub](quickstart-send-telemetry-python.md) zelfstudie
* apparaten configureren met de gewenste eigenschappen van het dubbele apparaat met de [gebruik gewenste eigenschappen om apparaten te configureren](tutorial-device-twins.md) zelfstudie
* Beheren van apparaten interactief (zoals het inschakelen van een fan, van een gebruiker beheerde app), met de [directe methoden gebruiken](quickstart-control-device-python.md) zelfstudie.

<!-- images -->
[1]: media/iot-hub-python-twin-getstarted/1.png
[2]: media/iot-hub-python-twin-getstarted/2.png
[3]: media/iot-hub-python-twin-getstarted/3.png
