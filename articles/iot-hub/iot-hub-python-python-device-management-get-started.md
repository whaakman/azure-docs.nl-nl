---
title: Aan de slag met Apparaatbeheer via Azure IoT Hub (Python) | Microsoft Docs
description: Het gebruik van Apparaatbeheer via IoT Hub om te beginnen met een extern apparaat opnieuw opstarten. U de Azure IoT-SDK voor Python gebruiken voor het implementeren van een gesimuleerde apparaat-app met een rechtstreekse methode en een service-app die de directe methode aanroept.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 01/02/2018
ms.author: kgremban
ms.openlocfilehash: dbe2ba6ce4e001f6e49fbbee9189fa5b4d99ec33
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2018
ms.locfileid: "51514380"
---
# <a name="get-started-with-device-management-python"></a>Aan de slag met Apparaatbeheer (Python)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

In deze zelfstudie ontdekt u hoe u:

* De Azure portal gebruiken voor het maken van een IoT-Hub en een apparaat-id maken in uw IoT-hub.
* Maak een gesimuleerde apparaat-app met een rechtstreekse methode die het apparaat opnieuw wordt opgestart. Directe methoden zijn aangeroepen vanuit de cloud.
* Maak een Python-consoletoepassing die de directe methode voor opnieuw opstarten in de gesimuleerde apparaattoepassing via uw IoT-hub roept.

Aan het einde van deze zelfstudie, beschikt u over twee Python-consoletoepassingen:

**dmpatterns_getstarted_device.PY**, deze toepassing koppelt uw IoT-hub aan de apparaat-id die eerder hebt gemaakt, ontvangt u een rechtstreekse methode voor opnieuw opstarten, simuleert fysieke opnieuw worden opgestart en rapporten van de tijd voor de laatste keer opnieuw opstarten.

**dmpatterns_getstarted_service.PY**, die een rechtstreekse methode aanroepen in het gesimuleerde apparaat-app, wordt het antwoord weergegeven en wordt de bijgewerkte gerapporteerde eigenschappen.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x][lnk-python-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk *pip* [installeren of upgraden, het Python-pakketbeheersysteem][lnk-install-pip].
    * Installeer de [azure-iothub-apparaat-client](https://pypi.org/project/azure-iothub-device-client/) pakket, met de opdracht   `pip install azure-iothub-device-client`
    * Installeer de [azure-iothub-service-client](https://pypi.org/project/azure-iothub-service-client/) pakket, met de opdracht   `pip install azure-iothub-service-client`
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++][lnk-visual-c-redist] om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

### <a name="retrieve-connection-string-for-iot-hub"></a>Voor IoT hub-verbindingsreeks ophalen

[!INCLUDE [iot-hub-include-find-connection-string](../../includes/iot-hub-include-find-connection-string.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie maakt u het volgende doen:

* Maakt een Python-console-app die op een rechtstreekse methode aangeroepen door de cloud reageert
* Simuleren van een apparaat opnieuw opstarten
* Gebruik van de gerapporteerde eigenschappen om te schakelen apparaatdubbel-query's om apparaten te identificeren en wanneer ze het laatst opnieuw opgestart

1. Maak met een teksteditor een **dmpatterns_getstarted_device.py** bestand.

1. Voeg de volgende `import` instructies toe aan het begin van de **dmpatterns_getstarted_device.py** bestand.
   
    ```python
    import random
    import time, datetime
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError, DeviceMethodReturnValue
    ```

1. Variabelen met inbegrip van een **CONNECTION_STRING** variabele en de initialisatie van de client.  Vervang de verbindingsreeks door de verbindingsreeks van uw apparaat.  
   
    ```python
    CONNECTION_STRING = "{deviceConnectionString}"
    PROTOCOL = IoTHubTransportProvider.MQTT

    CLIENT = IoTHubClient(CONNECTION_STRING, PROTOCOL)

    WAIT_COUNT = 5

    SEND_REPORTED_STATE_CONTEXT = 0
    METHOD_CONTEXT = 0

    SEND_REPORTED_STATE_CALLBACKS = 0
    METHOD_CALLBACKS = 0
    ```

1. Voeg de volgende functie callbacks voor het implementeren van de directe methode die op het apparaat.
   
    ```python
    def send_reported_state_callback(status_code, user_context):
        global SEND_REPORTED_STATE_CALLBACKS
    
        print ( "Device twins updated." )

    def device_method_callback(method_name, payload, user_context):
        global METHOD_CALLBACKS
    
        if method_name == "rebootDevice":
            print ( "Rebooting device..." )
        
            time.sleep(20)
        
            print ( "Device rebooted." )
        
            current_time = str(datetime.datetime.now())
            reported_state = "{\"rebootTime\":\"" + current_time + "\"}"
            CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
        
            print ( "Updating device twins: rebootTime" )
            
        device_method_return_value = DeviceMethodReturnValue()
        device_method_return_value.response = "{ \"Response\": \"This is the response from the device\" }"
        device_method_return_value.status = 200
    
        return device_method_return_value
    ```

1. Start de directe methode-listener en wachten.
   
    ```python
    def iothub_client_init():
        if CLIENT.protocol == IoTHubTransportProvider.MQTT or client.protocol == IoTHubTransportProvider.MQTT_WS:
            CLIENT.set_device_method_callback(device_method_callback, METHOD_CONTEXT)
        
    def iothub_client_sample_run():
        try:
            iothub_client_init()

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

1. Opslaan en sluiten de **dmpatterns_getstarted_device.py** bestand.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals aangegeven in het artikel implementeren [afhandeling van tijdelijke fouten](/azure/architecture/best-practices/transient-faults).


## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Een extern opnieuw opstarten op het apparaat met een rechtstreekse methode activeren
In deze sectie maakt u een Python-consoletoepassing die een extern opnieuw opstarten op een apparaat met een rechtstreekse methode initieert. De app maakt gebruik van apparaatdubbel-query's voor het detecteren van de laatste keer opnieuw opstarten voor dat apparaat.

1. Maak met een teksteditor een **dmpatterns_getstarted_service.py** bestand.

1. Voeg de volgende `import` instructies toe aan het begin van de **dmpatterns_getstarted_service.py** bestand.
   
    ```python
    import sys, time
    import iothub_service_client

    from iothub_service_client import IoTHubDeviceMethod, IoTHubError, IoTHubDeviceTwin
    ```

1. Voeg de volgende variabelendeclaraties. Alleen vervangen door waarden van de tijdelijke aanduiding voor _IoTHubConnectionString_ en _deviceId_.
   
    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "rebootDevice"
    METHOD_PAYLOAD = "{\"method_number\":\"42\"}"
    TIMEOUT = 60
    WAIT_COUNT = 10
    ```

1. Voeg de volgende functie voor het aanroepen van de apparaatmethode voor het opnieuw opstarten van het doelapparaat en klik vervolgens op te vragen voor de apparaatdubbels en ontvang de laatste keer dat opnieuw opstarten.
   
    ```python
    def iothub_devicemethod_sample_run():
        try:
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)
        
            print ( "" )
            print ( "Invoking device to reboot..." )

            response = iothub_device_method.invoke(DEVICE_ID, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)

            print ( "" )
            print ( "Successfully invoked the device to reboot." )

            print ( "" )
            print ( response.payload )
        
            while True:
                print ( "" )
                print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
                    twin_info = iothub_twin_method.get_twin(DEVICE_ID)
                
                    if twin_info.find("rebootTime") != -1:
                        print ( "Last reboot time: " + twin_info[twin_info.find("rebootTime")+11:twin_info.find("rebootTime")+37])
                    else:
                        print ("Waiting for device to report last reboot time...")

                    time.sleep(5)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}".format(iothub_error) )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubDeviceMethod sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Service Client DeviceManagement Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_devicemethod_sample_run()
    ```

1. Opslaan en sluiten de **dmpatterns_getstarted_service.py** bestand.


## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.

1. Bij de opdrachtprompt de volgende opdracht uit om te luisteren naar de directe methode die opnieuw worden opgestart.
   
    ```
    python dmpatterns_getstarted_device.py
    ```

1. Bij een andere opdrachtprompt, voer de volgende opdracht de extern opnieuw opstarten en de query voor het dubbele apparaat vinden van de laatste keer opnieuw activeren.
   
    ```
    python dmpatterns_getstarted_service.py
    ```

1. U ziet dat de reactie van het apparaat naar de directe methode die in de console.

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/

[img-output]: media/iot-hub-get-started-with-dm/image6.png
[img-dm-ui]: media/iot-hub-get-started-with-dm/dmui.png

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure portal]: https://portal.azure.com/
[Using resource groups to manage your Azure resources]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
