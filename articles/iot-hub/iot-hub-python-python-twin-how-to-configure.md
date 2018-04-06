---
title: Gebruik Azure IoT Hub apparaat twin eigenschappen (Python) | Microsoft Docs
description: Het gebruik van Azure IoT Hub apparaat horende apparaten configureren. De Azure IoT SDK's voor Python kunt u een gesimuleerde apparaattoepassing en een service-app die gevolgen heeft voor de apparaatconfiguratie van een met behulp van een apparaat-twin geïmplementeerd.
services: iot-hub
documentationcenter: .net
author: kgremban
manager: timlt
editor: ''
ms.assetid: d0bcec50-26e6-40f0-8096-733b2f3071ec
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/12/2018
ms.author: v-masebo;kgremban
ms.openlocfilehash: e6f4aa61e37769dc4851f8284d07a7991ee38c91
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="use-desired-properties-to-configure-devices-python"></a>Gewenste eigenschappen gebruiken voor het configureren van apparaten (Python)
[!INCLUDE [iot-hub-selector-twin-how-to-configure](../../includes/iot-hub-selector-twin-how-to-configure.md)]

Aan het einde van deze zelfstudie hebt u twee Python console apps:

* **SimulateDeviceConfiguration.py**, een gesimuleerde apparaattoepassing die wordt gewacht op een gewenste configuratie-update en rapporteert de status van een gesimuleerde configuratie updateproces.
* **SetDesiredConfigurationAndQuery.py**, een Python back-end-app, die de gewenste configuratie ingesteld op een apparaat en het configuratieproces van de update-query's.

> [!NOTE]
> Het artikel [Azure IoT SDK's] [ lnk-hub-sdks] bevat informatie over de Azure IoT SDK's dat u gebruiken kunt om zowel apparaatgegevens als back-end-apps te bouwen.
> 
> 

Voor deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x][lnk-python-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk *pip* [installeren of upgraden, het Python-pakketbeheersysteem][lnk-install-pip].
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++][lnk-visual-c-redist] om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

Als u hebt gevolgd de [aan de slag met apparaat horende] [ lnk-twin-tutorial] zelfstudie, u hebt al een IoT-hub en een apparaat-id genoemd **myDeviceId**; en u kunt doorgaan met de [de gesimuleerde apparaattoepassing maken] [ lnk-how-to-configure-createapp] sectie.

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-the-simulated-device-app"></a>De gesimuleerde apparaattoepassing maken
In deze sectie maakt u een Python consoletoepassing maken die verbinding met uw hub als maakt **myDeviceId**, wordt gewacht op een gewenste configuratie-update en vervolgens rapporten updates op het updateproces gesimuleerde configuratie.

1. Installeer de **apparaat-SDK van Azure IoT Python** met de volgende opdracht achter de opdrachtprompt:
   
    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Start een teksteditor en maak een nieuwe **SimulateDeviceConfiguration.py** bestand.

1. Voeg de volgende code naar de **SimulateDeviceConfiguration.py** -bestand en vervang de **{verbindingsreeks apparaat}** aanduiding voor items met de apparaat-verbindingsreeks die u tijdens het maken van de gekopieerd**myDeviceId** apparaat-id:

    ```python   
    import time
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError

    CONNECTION_STRING = "{deviceConnectionString}"
    PROTOCOL = IoTHubTransportProvider.MQTT

    CLIENT = IoTHubClient(CONNECTION_STRING, PROTOCOL)

    WAIT_COUNT = 5

    TWIN_CONTEXT = 0
    SEND_REPORTED_STATE_CONTEXT = 0

    TWIN_CALLBACKS = 0
    SEND_REPORTED_STATE_CALLBACKS = 0

    CONFIG_ID = "1"
    TWIN_PAYLOAD = "{\"configId\":" + CONFIG_ID + ",\"sendFrequency\":\"24h\"}"
    ```

    De **CLIENT** object bevat de methoden die zijn vereist voor interactie met horende apparaten van het apparaat. In meer code koppelt u een handler voor de update op de gewenste eigenschappen vervolgens een extra handler toevoegen om te controleren of er een wijzigingsaanvraag voor de huidige configuratie door de configIds, die vervolgens roept een methode die de configuratiewijziging begint vergelijken . Omwille van de eenvoud de vorige code vastgelegde standaardwaarde gebruikt voor de eerste configuratie. Een echte app zou waarschijnlijk dat de configuratie van een lokale opslag laden.
   
   > [!IMPORTANT]
   > Gewenste eigenschap wijzigingsgebeurtenissen altijd één keer worden verzonden op het apparaatverbinding, Controleer of er een werkelijke wijziging in de eigenschappen van de gewenste is voordat u een actie uitvoert.
   > 

1. Voeg de volgende code toe aan het einde van de **SimulateDeviceConfiguration.py** bestand:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        global TWIN_CALLBACKS
        global CONFIG_ID

        desired_configId = payload[payload.find("configId")+11:payload.find("configId")+12]
        if CONFIG_ID != desired_configId:
            print ( "" )
            print ( "Reported pending config change: %s" % payload)
        
            desired_sendFrequency = payload[payload.find("sendFrequency")+17:payload.find("sendFrequency")+19]
            print ( "...desired configId: " + desired_configId)
            print ( "...desired sendFrequency: " + desired_sendFrequency)
            new_payload = "{\"configId\":" + desired_configId + ",\"sendFrequency\":\"" + desired_sendFrequency + "\"}"
            CLIENT.send_reported_state(new_payload, len(new_payload), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
        
        CONFIG_ID = desired_configId
    
    def send_reported_state_callback(status_code, user_context):
        global SEND_REPORTED_STATE_CALLBACKS
    
        print ( "" )
        print ( "Device twins updated." )
    ```
   
    De **device_twin_callback** methode updates gerapporteerd eigenschappen van het lokale apparaat twin object met de configuratie bijwerken aanvraag en stelt de _configId_. Nadat het apparaat twin is bijgewerkt, wordt een updatebericht afgedrukt. Voor het opslaan van bandbreedte gemelde eigenschappen zijn bijgewerkt door te geven alleen de eigenschappen te wijzigen (met de naam **TWIN_PAYLOAD** in de bovenstaande code), in plaats van het hele document vervangen.
   
   > [!NOTE]
   > Deze zelfstudie wordt een gedrag voor updates voor gelijktijdige configuratie niet simuleren. Bepaalde configuratie-update-processen mogelijk wijzigingen van de doelconfiguratie voor terwijl de update wordt uitgevoerd, anderen mogelijk moet u ze in de wachtrij en anderen met een fout opgetreden weigeren kunnen. Zorg ervoor dat rekening houden met het gewenste gedrag voor uw specifieke configuratie-proces en de juiste logica toevoegen voordat u begint de wijziging in de configuratie.
   > 

1. Voeg de volgende code toe aan het einde van de **SimulateDeviceConfiguration.py** bestand: 

    ```python
    def iothub_client_init():
        if CLIENT.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            CLIENT.set_device_twin_callback(device_twin_callback, TWIN_CONTEXT)

    def iothub_client_sample_run():
        try:
            iothub_client_init()
        
            CLIENT.send_reported_state(TWIN_PAYLOAD, len(TWIN_PAYLOAD), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)

            while True:
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(10)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_client_sample_run()
    ```

1. Voer de app voor apparaat:
   
    ```cmd/sh
    node SimulateDeviceConfiguration.js
    ```
   
    U ziet het bericht `Device twins updated.`. De app actief houden.


## <a name="create-the-service-app"></a>De service-app maken
In deze sectie maakt u een console-app voor Python-updates de *gewenst eigenschappen* op het apparaat twin gekoppeld **myDeviceId** met een nieuwe telemetrie configuration-object. Vervolgens zoekt de horende apparaat is opgeslagen in de IoT-hub en ziet u het verschil tussen de gewenste en gerapporteerde configuraties van het apparaat.

1. Installeer de **Python-SDK van Azure IoT** met de volgende opdracht achter de opdrachtprompt:
   
    ```cmd/sh
    pip install azure-iothub-service-client
    ```

1. Start een teksteditor en maak een nieuwe **SetDesiredAndQuery.py** bestand.

1. Voeg de volgende code naar de **SetDesiredAndQuery.py** -bestand en vervang de **{IoTHubConnectionString}** aanduiding voor items met de IoT Hub-verbindingsreeks die u tijdens het maken van uw hub hebt gekopieerd en de **{deviceId}** aanduiding voor items met de naam van uw apparaat:

    ```python
    import sys, time
    import iothub_service_client

    from iothub_service_client import IoTHubError, IoTHubDeviceTwin

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    UPDATE_JSON = "{\"properties\":{\"desired\":{\"configId\":3, \"sendFrequency\":\"" + sys.argv[1:][0] + "\"}}}"
    TIMEOUT = 60
    WAIT_COUNT = 10
    ```

1. Voeg de volgende code toe aan het einde van de **SetDesiredAndQuery.py** bestand:

    ```python
    def iothub_devicetwin_sample_run():
        try:
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)
        
            twin_info = iothub_twin_method.get_twin(DEVICE_ID)
            print ( "" )
            print ( "Device Twin before update    :" )
            print ( "{0}".format(twin_info) )

            twin_info = iothub_twin_method.update_twin(DEVICE_ID, UPDATE_JSON)
            print ( "" )
            print ( "Device Twin after update     :" )
            print ( "{0}".format(twin_info) )
        
            while True:
                time.sleep(10)

                twin_info = iothub_twin_method.get_twin(DEVICE_ID)
                print ( "" )
                print ( "Device Twin after client change initiated    :" )
                print ( "{0}".format(twin_info) )
        
                print ( "" )
                print ( "IoT Hub service sample complete, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    time.sleep(30)
                
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubDeviceMethod sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client DeviceTwins Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_devicetwin_sample_run()
    ```

1. Met **SimulateDeviceConfiguration.py** wordt uitgevoerd, de toepassing uitvoert in een nieuwe opdrachtprompt met:

    ```cmd/sh
    python SetDesiredAndQuery.py 5m
    ```
   
    U ziet de gerapporteerde configuratie-ID die is gewijzigd van **1** naar **2** verzenden met de nieuwe actieve frequentie van vijf minuten in plaats van 24 uur.


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie stelt u een gewenste configuratie als *gewenst eigenschappen* vanuit een back-end-app en een gesimuleerde apparaattoepassing om te detecteren die wijziging en een updateproces melden als de status ervan simuleren geschreven *gerapporteerd eigenschappen* aan de apparaat-twin.

Gebruik de volgende bronnen voor meer informatie over hoe:

* verzenden van telemetrie vanaf apparaten met de [aan de slag met IoT Hub] [ lnk-iothub-getstarted] zelfstudie
* schema of bewerkingen uitvoeren op grote sets van apparaten, Zie de [planning en broadcast taken] [ lnk-schedule-jobs] zelfstudie.
* beheren van apparaten interactief (zoals het inschakelen van een ventilator van een gebruiker beheerde app), met de [direct methoden gebruiken] [ lnk-methods-tutorial] zelfstudie.

<!-- links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-twin-notifications]: iot-hub-devguide-device-twins.md#back-end-operations
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: iot-hub-device-management-overview.md
[lnk-twin-tutorial]: iot-hub-python-twin-getstarted.md
[lnk-schedule-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-iothub-getstarted]: iot-hub-python-getstarted.md
[lnk-methods-tutorial]: iot-hub-python-python-direct-methods.md

[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier

[lnk-how-to-configure-createapp]: iot-hub-node-node-twin-how-to-configure.md#create-the-simulated-device-app
