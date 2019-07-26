---
title: Aan de slag met Azure IoT Hub Device apparaatdubbels (python) | Microsoft Docs
description: Azure IoT Hub Device apparaatdubbels gebruiken om labels toe te voegen en vervolgens een IoT Hub query te gebruiken. U gebruikt de Azure IoT Sdk's voor python voor het implementeren van de gesimuleerde apparaat-app en een service-app waarmee de tags worden toegevoegd en de IoT Hub query wordt uitgevoerd.
author: kgremban
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 02/21/2019
ms.author: kgremban
ms.openlocfilehash: 53e3d32497c7aae6c584d23b9baddbaeaf1bd822
ms.sourcegitcommit: 9dc7517db9c5817a3acd52d789547f2e3efff848
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68405879"
---
# <a name="get-started-with-device-twins-python"></a>Aan de slag met Device apparaatdubbels (python)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

Aan het einde van deze zelf studie hebt u twee python-console-apps:

* **AddTagsAndQuery.py**, een python back-end-app, waarmee labels en query's apparaat apparaatdubbels worden toegevoegd.

* **ReportConnectivity.py**, een python-app, die een apparaat simuleert dat verbinding maakt met uw IOT-hub met de apparaat-id die u eerder hebt gemaakt, en rapporteert de connectiviteits voorwaarde.

> [!NOTE]
> Het artikel [Azure IOT sdk's](iot-hub-devguide-sdks.md) bevat informatie over de Azure IOT-sdk's die u kunt gebruiken om zowel apparaat-als back-end-apps te bouwen.

U hebt het volgende nodig om deze zelf studie te volt ooien:

* [Python 2. x of 3. x](https://www.python.org/downloads/). Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk [pip *installeren of upgraden*, het Python-pakketbeheersysteem](https://pip.pypa.io/en/stable/installing/).

* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++](https://www.microsoft.com/download/confirmation.aspx?id=48145) om het gebruik van systeemeigen DLL's van Python mogelijk te maken.

* Een actief Azure-account. (Als u geen account hebt, kunt u in slechts een paar minuten een [gratis account](https://azure.microsoft.com/pricing/free-trial/) maken.)

> [!NOTE]
> De *pip*-pakketten voor `azure-iothub-service-client` en `azure-iothub-device-client` zijn momenteel alleen beschikbaar voor het Windows-besturingssysteem. Voor Linux/Mac OS raadpleegt u de sectie met Linux-en Mac OS-specifieke secties in de [ontwikkel omgeving voorbereiden voor python](https://github.com/Azure/azure-iot-sdk-python/blob/master/doc/python-devbox-setup.md) post.
>

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="register-a-new-device-in-the-iot-hub"></a>Een nieuw apparaat registreren in de IoT-hub

[!INCLUDE [iot-hub-include-create-device](../../includes/iot-hub-include-create-device.md)]

## <a name="get-the-iot-hub-connection-string"></a>De IoT hub-connection string ophalen

[!INCLUDE [iot-hub-howto-twin-shared-access-policy-text](../../includes/iot-hub-howto-twin-shared-access-policy-text.md)]

[!INCLUDE [iot-hub-include-find-custom-connection-string](../../includes/iot-hub-include-find-custom-connection-string.md)]

## <a name="create-the-service-app"></a>De service-app maken

In deze sectie maakt u een python-console-app waarmee de meta gegevens van de locatie worden toegevoegd aan het apparaat dat is gekoppeld aan uw **{apparaat-id}** . Vervolgens wordt een query uitgevoerd op het apparaat apparaatdubbels dat is opgeslagen in de IoT-hub en vervolgens de apparaten selecteert die zich in Redmond bevinden. vervolgens wordt een mobiele verbinding gerapporteerd.

1. Open een opdrachtprompt en installeer de **SDK voor de Azure IoT Hub-service voor Python**. Sluit de opdrachtprompt na de installatie van de SDK.

   ```
   pip install azure-iothub-service-client
   ```

2. Maak een nieuw **AddTagsAndQuery.py** -bestand met behulp van een tekst editor.

3. Voeg de volgende code toe om de vereiste modules uit de service-SDK te importeren:

   ```python
   import sys
   import iothub_service_client
   from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
   from iothub_service_client import IoTHubDeviceTwin, IoTHubError
   ```

4. Voeg de volgende code toe, waarbij u de `[IoTHub Connection String]` tijdelijke `[Device Id]` aanduiding vervangt door en met de Connection String voor de IOT-hub en de apparaat-id die u hebt gemaakt in de vorige secties.
  
    ```python
    CONNECTION_STRING = "[IoTHub Connection String]"
    DEVICE_ID = "[Device Id]"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"location\":\"Redmond\"}}}"

    UPDATE_JSON_SEARCH = "\"location\":\"Redmond\""
    UPDATE_JSON_CLIENT_SEARCH = "\"connectivity\":\"cellular\""
    ```

5. Voeg de volgende code toe aan het **AddTagsAndQuery.py** -bestand:

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

    Het **register** object bevat alle methoden die nodig zijn om te communiceren met apparaatdubbels van de service. De code initialiseert eerst het **register** object, werkt het apparaat vervolgens bij voor **deviceId**en voert ten slotte twee query's uit. De eerste selecteert alleen het apparaat apparaatdubbels van apparaten die zich in de **Redmond43** -installatie bevinden en de tweede verfijnt de query om alleen de apparaten te selecteren die ook zijn verbonden via een mobiel netwerk.

6. Voeg de volgende code toe aan het einde van **AddTagsAndQuery.py** om de functie **iothub_service_sample_run** te implementeren:

    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python service sample..." )

        iothub_service_sample_run()
    ```

7. Voer de toepassing uit met:

    ```cmd/sh
    python AddTagsAndQuery.py
    ```

    U ziet één apparaat in de resultaten voor de query die vraagt naar alle apparaten in **Redmond43** en geen voor de query waarmee de resultaten worden beperkt tot apparaten die gebruikmaken van een mobiel netwerk.

    ![eerste query waarin alle apparaten in Redmond worden weer gegeven](./media/iot-hub-python-twin-getstarted/1-device-twins-python-service-sample.png)

In de volgende sectie maakt u een apparaat-app die de connectiviteits gegevens rapporteert en het resultaat van de query in de vorige sectie wijzigt.

## <a name="create-the-device-app"></a>De apparaat-app maken

In deze sectie maakt u een python-console-app die verbinding maakt met uw hub als uw **{apparaat-id}** en vervolgens de gerapporteerde eigenschappen van het apparaat bijwerkt om de gegevens te bevatten die zijn verbonden met behulp van een mobiel netwerk.

1. Open een opdrachtprompt en installeer de **SDK voor de Azure IoT Hub-service voor Python**. Sluit de opdrachtprompt na de installatie van de SDK.

    ```
    pip install azure-iothub-device-client
    ```

2. Maak een nieuw **ReportConnectivity.py** -bestand met behulp van een tekst editor.

3. Voeg de volgende code toe om de vereiste modules uit de service-SDK te importeren:

    ```python
    import time
    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError
    ```

4. Voeg de volgende code toe, waarbij u de `[IoTHub Device Connection String]` tijdelijke aanduiding vervangt door de Connection String voor het IOT hub-apparaat dat u in de vorige secties hebt gemaakt.

    ```python
    CONNECTION_STRING = "[IoTHub Device Connection String]"

    # choose HTTP, AMQP, AMQP_WS or MQTT as transport protocol
    PROTOCOL = IoTHubTransportProvider.MQTT

    TIMER_COUNT = 5
    TWIN_CONTEXT = 0
    SEND_REPORTED_STATE_CONTEXT = 0
    ```

5. Voeg de volgende code toe aan het **ReportConnectivity.py** -bestand om de apparaatdubbels-functionaliteit van het apparaat te implementeren:

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

    Het **client** object bevat alle methoden die u nodig hebt om te communiceren met apparaatdubbels van het apparaat. De vorige code, na het initialiseren van het **client** object, haalt het apparaat op voor uw apparaat en werkt de gerapporteerde eigenschap bij met de verbindings gegevens.

6. Voeg de volgende code toe aan het einde van **ReportConnectivity.py** om de functie **iothub_client_sample_run** te implementeren:

    ```python
    if __name__ == '__main__':
        print ( "Starting the IoT Hub Device Twins Python client sample..." )

        iothub_client_sample_run()
    ```

7. De apparaat-app uitvoeren:

    ```cmd/sh
    python ReportConnectivity.py
    ```

    U ziet een bevestiging dat de apparaatdubbels van het apparaat zijn bijgewerkt.

    ![apparaatdubbels bijwerken](./media/iot-hub-python-twin-getstarted/2-python-client-sample.png)

8. Nu het apparaat de verbindings gegevens heeft gerapporteerd, zou het in beide query's moeten worden weer gegeven. Ga terug en voer de query's opnieuw uit:

    ```cmd/sh
    python AddTagsAndQuery.py
    ```

    Deze keer dat uw **{device id}** in beide query resultaten moet worden weer gegeven.

    ![tweede query](./media/iot-hub-python-twin-getstarted/3-device-twins-python-service-sample.png)

## <a name="next-steps"></a>Volgende stappen

In deze handleiding hebt u een nieuwe IoT-hub geconfigureerd in Azure Portal en vervolgens een apparaat-id gemaakt in het id-register van de IoT-hub. U hebt meta gegevens van apparaten toegevoegd als tags van een back-end-app en een gesimuleerde apparaat-app geschreven om connectiviteits gegevens van apparaten te rapporteren in het dubbele apparaat. U hebt ook geleerd hoe u deze gegevens in het REGI ster kunt opvragen.

Gebruik de volgende bronnen voor meer informatie over:

* Verzend telemetrie van apparaten met de zelf studie [aan de slag met IOT hub](quickstart-send-telemetry-python.md) .

* Configureer apparaten met behulp van de gewenste eigenschappen van het apparaat met de zelf studie [gewenste eigenschappen gebruiken om apparaten te configureren](tutorial-device-twins.md) .

* Apparaten interactief beheren (bijvoorbeeld door een ventilator in te scha kelen vanuit een door de gebruiker beheerde app), met de zelf studie [directe methoden gebruiken](quickstart-control-device-python.md) .