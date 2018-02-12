---
title: Aan de slag met Azure IoT Hub Apparaatbeheer (Python) | Microsoft Docs
description: "Het externe apparaat opnieuw opstarten initiëren met Apparaatbeheer via IoT Hub. U kunt de Azure IoT SDK voor Python gebruiken voor het implementeren van een gesimuleerde apparaattoepassing die een directe methode bevat en een service-app die de directe methode aanroept."
services: iot-hub
documentationcenter: .net
author: msebolt
manager: timlt
editor: 
ms.assetid: e044006d-ffd6-469b-bc63-c182ad066e31
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/02/2018
ms.author: v-masebo
ms.openlocfilehash: b9dfbd775de3bb54e405e4054bca4e2b878305a3
ms.sourcegitcommit: 4723859f545bccc38a515192cf86dcf7ba0c0a67
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2018
---
# <a name="get-started-with-device-management-python"></a>Aan de slag met Apparaatbeheer (Python)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

In deze handleiding ontdekt u hoe u:

* De Azure portal gebruiken voor het maken van een IoT-Hub en een apparaat-id maakt in uw IoT-hub.
* Maak een gesimuleerde apparaattoepassing met een directe methode die het apparaat opnieuw wordt opgestart. Rechtstreekse methoden worden aangeroepen vanuit de cloud.
* Een Python consoletoepassing maken die de gesimuleerde apparaattoepassing via uw IoT-hub de directe methode voor opnieuw opstarten roept.

Aan het einde van deze zelfstudie hebt u twee Python console apps:

**dmpatterns_getstarted_device.PY**, die verbinding maakt met uw IoT-hub aan de apparaat-id eerder hebt gemaakt, ontvangt u een directe methode voor opnieuw opstarten, simuleert fysieke opnieuw worden opgestart en rapporten van de tijd voor de laatste keer opnieuw opstarten.

**dmpatterns_getstarted_service.PY**, die een directe methode wordt aangeroepen in de gesimuleerde apparaattoepassing geeft het antwoord weer en geeft de bijgewerkte eigenschappen gerapporteerd.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x][lnk-python-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk *pip* [installeren of upgraden, het Python-pakketbeheersysteem][lnk-install-pip].
    * Installeer de [azure-iothub-apparaat-client](https://pypi.org/project/azure-iothub-device-client/) pakket, met de opdracht`pip install azure-iothub-device-client`
    * Installeer de [azure-iothub-service-client](https://pypi.org/project/azure-iothub-service-client/) pakket, met de opdracht`pip install azure-iothub-service-client`
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++][lnk-visual-c-redist] om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
U wordt in deze sectie:

* Een Python-consoletoepassing dat met een directe methode is aangeroepen door de cloud overeenkomt maken
* Simuleren van een apparaat opnieuw starten
* Gebruik de gemelde eigenschappen om het apparaat twin query's om apparaten te identificeren en wanneer ze het laatst opnieuw opgestart

1. Maak met een teksteditor, een **dmpatterns_getstarted_device.py** bestand.

1. Voeg de volgende `import` instructies aan het begin van de **dmpatterns_getstarted_device.py** bestand.
   
    ```python
    import random
    import time, datetime
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult, IoTHubError, DeviceMethodReturnValue
    ```

1. Toevoegen van variabelen met inbegrip van een **koppeling-reeks** variabele en de initialisatie van de client.  De verbindingsreeks vervangen door de verbindingsreeks van uw apparaat.  
   
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

1. Voeg de volgende functie callbacks voor het implementeren van de directe methode op het apparaat.
   
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

1. Start de directe methode-listener en wacht.
   
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

1. Sla op en sluit de **dmpatterns_getstarted_device.py** bestand.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u een beleid voor opnieuw proberen implementeren (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel [Transient Fault Handling][lnk-transient-faults] (Afhandeling van tijdelijke fouten).


## <a name="trigger-a-remote-reboot-on-the-device-using-a-direct-method"></a>Activeren van een extern opnieuw opstarten op het apparaat met een directe methode
In deze sectie maakt u een Python consoletoepassing maken die een externe opnieuw opstarten op een apparaat met een directe methode initieert. De app gebruikmaakt van apparaat twin query's voor het detecteren van de laatste keer opnieuw opstarten voor dat apparaat.

1. Maak met een teksteditor, een **dmpatterns_getstarted_service.py** bestand.

1. Voeg de volgende `import` instructies aan het begin van de **dmpatterns_getstarted_service.py** bestand.
   
    ```python
    import sys, time
    import iothub_service_client

    from iothub_service_client import IoTHubDeviceMethod, IoTHubError, IoTHubDeviceTwin
    ```

1. Voeg de volgende variabele declaraties. Alleen Vervang de waarden van de tijdelijke aanduiding voor _IoTHubConnectionString_ en _deviceId_.
   
    ```python
    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "rebootDevice"
    METHOD_PAYLOAD = "{\"method_number\":\"42\"}"
    TIMEOUT = 60
    WAIT_COUNT = 10
    ```

1. Voeg de volgende functie om aan te roepen de apparaat-methode voor opnieuw opstarten van het doelapparaat en vervolgens de horende apparaten zoeken en ophalen van de laatste keer dat opnieuw opstarten.
   
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

1. Sla op en sluit de **dmpatterns_getstarted_service.py** bestand.


## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.

1. Bij de opdrachtprompt de volgende opdracht uitvoeren om te beginnen met luisteren naar de directe methode voor opnieuw opstarten.
   
    ```
    python dmpatterns_getstarted_device.py
    ```

1. Voer bij een andere opdrachtprompt de volgende opdracht de extern opnieuw opstarten en de query voor het apparaat vinden van de laatste keer opnieuw activeren.
   
    ```
    python dmpatterns_getstarted_service.py
    ```

1. U ziet de reactie van het apparaat aan de directe methode in de console.

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
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
