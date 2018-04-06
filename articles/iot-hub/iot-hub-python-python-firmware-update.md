---
title: Firmware-update van het apparaat met Azure IoT Hub (Python) | Microsoft Docs
description: Klik hier voor meer informatie over het beheer van apparaten op Azure IoT Hub gebruiken om te zetten van een apparaat firmware-update. U kunt Azure IoT SDK's voor Python gebruiken voor het implementeren van een gesimuleerde apparaattoepassing en een service-app waarmee de firmware-update wordt geactiveerd.
services: iot-hub
documentationcenter: .net
author: kgremban
manager: timlt
editor: ''
ms.assetid: 70b84258-bc9f-43b1-b7cf-de1bb715f2cf
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2018
ms.author: v-masebo;kgremban
ms.openlocfilehash: 4a042723e6bebebb891640950e579e031e27e61d
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-pythonpython"></a>Gebruik Apparaatbeheer initiëren van een apparaatfirmware bijwerken (Python/Python)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

In de [aan de slag met Apparaatbeheer] [ lnk-dm-getstarted] zelfstudie, hebt u gezien hoe u de [apparaat twin] [ lnk-devtwin] en [methoden directe] [ lnk-c2dmethod] primitieven op afstand opnieuw opstarten van een apparaat. Deze zelfstudie maakt gebruik van de dezelfde IoT Hub primitieven en biedt richtlijnen en ziet u hoe u een end-to-end gesimuleerde firmware-update.  Dit patroon wordt gebruikt in de firmware-update-implementatie voor de Intel Edison apparaat.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze handleiding ontdekt u hoe u:

* Een Python consoletoepassing maken die de directe methode firmwareUpdate in de gesimuleerde apparaattoepassing via uw IoT-hub aanroepen.
* Maakt een gesimuleerd apparaat-app die u implementeert een **firmwareUpdate** directe methode. Deze methode initieert een meerdere fasen die wacht de firmware-installatiekopie te downloaden, de installatiekopie van het firmware downloaden en ten slotte is de installatiekopie van de firmware van toepassing. In elke fase van de update wordt het apparaat de gerapporteerde eigenschappen gebruikt om te rapporteren over de voortgang.

Aan het einde van deze zelfstudie hebt u twee Python console apps:

**dmpatterns_fwupdate_service.PY**, wordt de reactie die een directe methode wordt aangeroepen in de gesimuleerde apparaattoepassing weergegeven, en regelmatig (elke 500ms) geeft de bijgewerkte gerapporteerde eigenschappen.

**dmpatterns_fwupdate_device.PY**, die verbinding maakt met uw IoT-hub aan de apparaat-id eerder hebt gemaakt, ontvangen een directe methode firmwareUpdate, wordt uitgevoerd via een proces met meerdere status om te simuleren een firmware-update inclusief: wacht tot de installatiekopie download downloaden van de nieuwe installatiekopie en ten slotte de installatiekopie toe te passen.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x][lnk-python-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk *pip* [installeren of upgraden, het Python-pakketbeheersysteem][lnk-install-pip].
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++][lnk-visual-c-redist] om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Activeren van een externe firmware-update op het apparaat met een directe methode
In deze sectie maakt u een Python consoletoepassing maken die een externe firmware-update op een apparaat initieert. De app gebruikmaakt van een directe methode om de update te initiëren en apparaat twin query's gebruikt om het periodiek de status van de actieve firmware-update niet ophalen.

1. Voer bij de opdrachtprompt de volgende opdracht voor het installeren van de **azure-iothub-service-client** pakket:
   
    ```cmd/sh
    pip install azure-iothub-service-client
    ```

1. Maak met een teksteditor, in uw werkmap, een **dmpatterns_getstarted_service.py** bestand.

1. De volgende importinstructies en variabelen toevoegen aan het begin van de **dmpatterns_getstarted_service.py** bestand. Vervang `IoTHubConnectionString` en `deviceId` door de waarden die eerder is aangegeven:
   
    ```python
    import sys
    import time

    import iothub_service_client
    from iothub_service_client import IoTHubDeviceTwin, IoTHubDeviceMethod, IoTHubError

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "firmwareUpdate"
    METHOD_PAYLOAD = "{\"fwPackageUri\":\"test.com\"}"
    TIMEOUT = 60
    MESSAGE_COUNT = 5
    ```

1. Voeg de volgende functie aanroepen van de directe methode en de waarde van de firmwareUpdate weergeven eigenschap gerapporteerd. Voeg ook de `main` routine:
   
    ```python
    def iothub_firmware_sample_run():
        try:
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)

            print ( "" )
            print ( "Direct Method called." )
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)
        
            response = iothub_device_method.invoke(DEVICE_ID, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)
            print ( response.payload )
        
            print ( "" )
            print ( "Device Twin queried, press Ctrl-C to exit" )
            while True:
                twin_info = iothub_twin_method.get_twin(DEVICE_ID)

                if "\"firmwareStatus\":\"standBy\"" in twin_info:
                    print ( "Waiting on device..." )
                elif "\"firmwareStatus\":\"downloading\"" in twin_info:
                    print ( "Downloading firmware..." )
                elif "\"firmwareStatus\":\"applying\"" in twin_info:
                    print ( "Download complete, applying firmware..." )
                elif "\"firmwareStatus\":\"completed\"" in twin_info:
                    print ( "Firmware applied" )
                    print ( "" )
                    print ( "Get reported properties from device twin:" )
                    print ( twin_info )
                    break
                else:
                    print ( "Unknown status" )

                status_counter = 0
                while status_counter <= MESSAGE_COUNT:
                    time.sleep(1)
                    status_counter += 1

        except IoTHubError as iothub_error:
            print ( "" )
            print ( "Unexpected error {0}" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "" )
            print ( "IoTHubService sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub firmware update Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_firmware_sample_run()
    ```

1. Sla op en sluit de **dmpatterns_fwupdate_service.py** bestand.


## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie doet u het volgende:

* Een Python-consoletoepassing dat met een directe methode is aangeroepen door de cloud overeenkomt maken
* U activeert een gesimuleerde firmware-update.
* U gebruikt de gerapporteerde eigenschappen om apparaatdubbelquery's in te schakelen die de apparaten identificeren en vaststellen wanneer er voor het laatst een firmware-update op deze apparaten is uitgevoerd.

1. Voer bij de opdrachtprompt de volgende opdracht voor het installeren van de **azure-iothub-apparaat-client** pakket:
   
    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Maak met een teksteditor, een **dmpatterns_fwupdate_device.py** bestand.

1. De volgende importinstructies en variabelen toevoegen aan het begin van de **dmpatterns_fwupdate_device.py** bestand. Vervang `deviceConnectionString` met de apparaat-verbindingsreeks uit uw IoT-hub:
   
    ```python
    import time, datetime
    import sys
    import threading

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubError, DeviceMethodReturnValue

    SEND_REPORTED_STATE_CONTEXT = 0
    METHOD_CONTEXT = 0

    MESSAGE_COUNT = 10

    PROTOCOL = IoTHubTransportProvider.MQTT
    CONNECTION_STRING = "{deviceConnectionString}"
    CLIENT = IoTHubClient(CONNECTION_STRING, PROTOCOL)
    ```

1. Voeg de volgende functies die worden gebruikt voor het leveren gerapporteerde eigenschappen updates en implementeren van de directe methode toe:
   
    ```python
    def send_reported_state_callback(status_code, user_context):
        print ( "Reported state updated." )

    def device_method_callback(method_name, payload, user_context):
        if method_name == "firmwareUpdate":
            print ( "Starting firmware update." )
            image_url = payload
            thr = threading.Thread(target=simulate_download_image, args=([payload]), kwargs={})
            thr.start()
    
            device_method_return_value = DeviceMethodReturnValue()
            device_method_return_value.response = "{ \"Response\": \"Firmware update started\" }"
            device_method_return_value.status = 200
            return device_method_return_value
    ```

1. Voeg de volgende functies toe. Deze simuleren het downloaden en toepassen van de firmware-installatiekopie:
   
    ```python
    def simulate_download_image(image_url):
        time.sleep(15)
        print ( "Downloading image from: " + image_url )

        reported_state = "{\"firmwareStatus\":\"downloading\", \"downloadComplete\":\"" + str(datetime.datetime.now()) + "\"}"
        CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
        time.sleep(15)
    
        simulate_apply_image(image_url)
    
    def simulate_apply_image(image_url):
        print ( "Applying image from: " + image_url )

        reported_state = "{\"firmwareStatus\":\"applying\", \"startedApplyingImage\":\"" + str(datetime.datetime.now()) + "\"}"
        CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
        time.sleep(15)
    
        simulate_complete_image()

    def simulate_complete_image():
        print ( "Image applied." )

        reported_state = "{\"firmwareStatus\":\"completed\", \"lastFirmwareUpdate\":\"" + str(datetime.datetime.now()) + "\"}"
        CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
    ```

8. Voeg de volgende functie die de apparaat-twin initialiseert gerapporteerd eigenschappen en wacht tot de directe methode moet worden aangeroepen. Voeg ook de `main` routine:
   
    ```python
    def iothub_firmware_sample_run():
        try:
            CLIENT.set_device_method_callback(device_method_callback, METHOD_CONTEXT)

            reported_state = "{\"firmwareStatus\":\"standBy\", \"logTime\":\"" + str(datetime.datetime.now()) + "\"}"
            CLIENT.send_reported_state(reported_state, len(reported_state), send_reported_state_callback, SEND_REPORTED_STATE_CONTEXT)
            print ( "Device twins initialized." )
            print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )
        
            while True:
                status_counter = 0
                while status_counter <= MESSAGE_COUNT:
                    time.sleep(10)
                    status_counter += 1
            
        except IoTHubError as iothub_error:
            print ( "Unexpected error %s from IoTHub" % iothub_error )
            return
        except KeyboardInterrupt:
            print ( "IoTHubClient sample stopped" )

    if __name__ == '__main__':
        print ( "Starting the IoT Hub Python firmware update sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_firmware_sample_run()
    ```

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. In productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout](https://msdn.microsoft.com/library/hh675232.aspx).
> 


## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.

1. Bij de opdrachtprompt de volgende opdracht uitvoeren om te beginnen met luisteren naar de directe methode voor opnieuw opstarten.
   
    ```cmd/sh
    python dmpatterns_fwupdate_device.py
    ```

1. Voer bij een andere opdrachtprompt de volgende opdracht de extern opnieuw opstarten en de query voor het apparaat vinden van de laatste keer opnieuw activeren.
   
    ```cmd/sh
    python dmpatterns_fwupdate_service.py
    ```

1. U ziet de reactie van het apparaat aan de directe methode in de console. Noteer de wijziging in de eigenschappen van gemeld in de firmware-update.

    ![Programma-uitvoer][1]


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een directe methode gebruikt voor het activeren van een externe firmware-update op een apparaat en de gerapporteerde eigenschappen gebruikt voor het bijhouden van de voortgang van de firmware-update.

Zie voor meer informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten aanroepen, de [planning en broadcast taken] [ lnk-tutorial-jobs] zelfstudie.

[lnk-devtwin]: iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: iot-hub-devguide-direct-methods.md
[lnk-dm-getstarted]: iot-hub-node-node-device-management-get-started.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[1]: ./media/iot-hub-python-python-firmware-update/1.png
