---
title: Firmware-update van het apparaat met Azure IoT Hub (Python) | Microsoft Docs
description: Het beheer van apparaten op Azure IoT Hub gebruiken om een firmware-update van het apparaat. U de Azure IoT SDK's voor Python gebruiken om een gesimuleerde apparaat-app en een service-app die wordt geactiveerd de firmware-update te implementeren.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: kgremban
ms.openlocfilehash: d2ebdf54e595c2f02464c0c2446a6e5f5feefb9c
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38482017"
---
# <a name="use-device-management-to-initiate-a-device-firmware-update-pythonpython"></a>Apparaatbeheer gebruiken om te starten van een apparaatfirmware bijwerken (Python/Python)
[!INCLUDE [iot-hub-selector-firmware-update](../../includes/iot-hub-selector-firmware-update.md)]

In de [aan de slag met Apparaatbeheer] [ lnk-dm-getstarted] zelfstudie, hebt u gezien hoe u de [apparaatdubbel] [ lnk-devtwin] en [directe methoden ] [ lnk-c2dmethod] primitieven aan een apparaat op afstand opnieuw opstarten. In deze zelfstudie maakt gebruik van de dezelfde IoT Hub-primitieven en bevat richtlijnen en ziet u hoe u een end-to-end gesimuleerde firmware-update doet.  Dit patroon wordt gebruikt in de firmware-update-implementatie voor de Intel Edison-apparaat maakt.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze handleiding ontdekt u hoe u:

* Een Python-console-app die de directe methode die firmwareUpdate in de gesimuleerde apparaattoepassing via uw IoT-hub aanroept maken.
* Maakt een gesimuleerde apparaat-app die u implementeert een **firmwareUpdate** directe methode. Deze methode initieert een proces met meerdere fasen dat wacht om te downloaden van de firmware-installatiekopie, wordt de firmware-installatiekopie gedownload en ten slotte past de firmware-installatiekopie. Tijdens elke fase van de update wordt het apparaat gebruikt van de gerapporteerde eigenschappen om te rapporteren over hun voortgang.

Aan het einde van deze zelfstudie, beschikt u over twee Python-consoletoepassingen:

**dmpatterns_fwupdate_service.PY**, die een rechtstreekse methode aanroepen in het gesimuleerde apparaat-app, wordt het antwoord weergegeven en periodiek (elke 500ms) geeft de bijgewerkte gerapporteerde eigenschappen.

**dmpatterns_fwupdate_device.PY**, deze toepassing koppelt uw IoT-hub aan de apparaat-id die eerder hebt gemaakt, ontvangt u een rechtstreekse methode firmwareUpdate, wordt uitgevoerd via een proces met meerdere staat voor het simuleren van een firmware-update inclusief: wachten op de installatiekopie Download de nieuwe installatiekopie te downloaden en ten slotte de installatiekopie toe te passen.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x][lnk-python-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk *pip* [installeren of upgraden, het Python-pakketbeheersysteem][lnk-install-pip].
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++][lnk-visual-c-redist] om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

## <a name="trigger-a-remote-firmware-update-on-the-device-using-a-direct-method"></a>Activeren van een externe firmware-update op het apparaat met een rechtstreekse methode
In deze sectie maakt u een Python-consoletoepassing die een externe firmware-update op een apparaat initieert. De app maakt gebruik van een rechtstreekse methode om de update te initiëren en apparaatdubbel-query's gebruikt om periodiek de status van de actieve firmware-update.

1. Bij de opdrachtprompt, voer de volgende opdracht voor het installeren van de **azure-iothub-service-client** pakket:
   
    ```cmd/sh
    pip install azure-iothub-service-client
    ```

1. Maak met een teksteditor in uw werkmap een **dmpatterns_getstarted_service.py** bestand.

1. De volgende importinstructie toe en variabelen toevoegen aan het begin van de **dmpatterns_getstarted_service.py** bestand. Vervang `IoTHubConnectionString` en `deviceId` met de waarden die u eerder hebt genoteerd:
   
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

1. Voeg de volgende functie aanroepen van de directe methode en de waarde van de firmwareUpdate weergeven gerapporteerde eigenschap. Voeg ook de `main` routine:
   
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

1. Opslaan en sluiten de **dmpatterns_fwupdate_service.py** bestand.


## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie doet u het volgende:

* Maakt een Python-console-app die op een rechtstreekse methode aangeroepen door de cloud reageert
* U activeert een gesimuleerde firmware-update.
* U gebruikt de gerapporteerde eigenschappen om apparaatdubbelquery's in te schakelen die de apparaten identificeren en vaststellen wanneer er voor het laatst een firmware-update op deze apparaten is uitgevoerd.

1. Bij de opdrachtprompt, voer de volgende opdracht voor het installeren van de **azure-iothub-apparaat-client** pakket:
   
    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Maak met een teksteditor een **dmpatterns_fwupdate_device.py** bestand.

1. De volgende importinstructie toe en variabelen toevoegen aan het begin van de **dmpatterns_fwupdate_device.py** bestand. Vervang `deviceConnectionString` met de verbindingsreeks van uw IoT-hub:
   
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

1. Voeg de volgende functies die worden gebruikt voor een gerapporteerde eigenschappen van updates en implementatie van de directe methode toe:
   
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

8. Voeg de volgende functie toe de apparaatdubbel initialiseert-gerapporteerde eigenschappen en wacht tot de directe methode die moet worden aangeroepen. Voeg ook de `main` routine:
   
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
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel implementeren [afhandeling van tijdelijke fouten](https://msdn.microsoft.com/library/hh675232.aspx).
> 


## <a name="run-the-apps"></a>De apps uitvoeren
U kunt nu de apps uitvoeren.

1. Bij de opdrachtprompt de volgende opdracht uit om te luisteren naar de directe methode die opnieuw worden opgestart.
   
    ```cmd/sh
    python dmpatterns_fwupdate_device.py
    ```

1. Bij een andere opdrachtprompt, voer de volgende opdracht de extern opnieuw opstarten en de query voor het dubbele apparaat vinden van de laatste keer opnieuw activeren.
   
    ```cmd/sh
    python dmpatterns_fwupdate_service.py
    ```

1. U ziet dat de reactie van het apparaat naar de directe methode die in de console. Noteer de wijziging in de gerapporteerde eigenschappen in de firmware-update.

    ![programma-uitvoer][1]


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie maakt u een rechtstreekse methode gebruikt voor het activeren van een externe firmware-update op een apparaat en gebruikt de gerapporteerde eigenschappen om te volgen van de voortgang van de firmware-update.

Zie voor informatie over het uitbreiden van uw IoT-oplossing en schema-methode op meerdere apparaten roept, de [taken plannen en uitzenden] [ lnk-tutorial-jobs] zelfstudie.

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
