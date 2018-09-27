---
title: Plannen van taken met Azure IoT Hub (Python) | Microsoft Docs
description: Klik hier voor meer informatie over het plannen van een taak is Azure IoT Hub een rechtstreekse methode aanroepen op meerdere apparaten. U de Azure IoT SDK's voor Python gebruiken voor het implementeren van de gesimuleerde apparaat-apps en een app service de taak uit te voeren.
author: kgremban
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.devlang: python
ms.topic: conceptual
ms.date: 02/16/2018
ms.author: kgremban
ms.openlocfilehash: 831b1c12514e57d378e22d007c75d8a3715f7276
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47219920"
---
# <a name="schedule-and-broadcast-jobs-python"></a>Taken plannen en uitzenden (Python)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub is een volledig beheerde service waarmee u een back-end-app voor het maken en bijhouden van taken die plannen en bijwerken van miljoenen apparaten.  Taken kunnen worden gebruikt voor de volgende acties:

* Gewenste eigenschappen bijwerken
* Bijwerken van tags
* Directe methoden aanroepen

Conceptueel gezien, een taak een van deze acties wordt verpakt en wordt de voortgang van de uitvoering op basis van een set apparaten dat is gedefinieerd door een dubbele apparaatquery bijgehouden.  Een back-end-app kan bijvoorbeeld een taak gebruiken om aan te roepen een methode voor opnieuw opstarten op 10.000 apparaten, opgegeven door een dubbele apparaatquery en gepland op een later tijdstip.  Deze toepassing kunt vervolgens voortgang bijhouden als elk van deze apparaten ontvangen en de methode voor opnieuw opstarten worden uitgevoerd.

Meer informatie over elk van deze mogelijkheden in deze artikelen:

* Apparaatdubbel en eigenschappen: [aan de slag met apparaatdubbels] [ lnk-get-started-twin] en [zelfstudie: apparaatdubbeleigenschappen gebruiken][lnk-twin-props]
* Directe methoden: [het Ontwikkelaarshandleiding voor IoT Hub - directe methoden] [ lnk-dev-methods] en [zelfstudie: directe methoden][lnk-c2d-methods]

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

In deze zelfstudie ontdekt u hoe u:

* Maken van een Python gesimuleerde apparaat-app met een rechtstreekse methode waarmee **lockDoor**, die kan worden aangeroepen in de back-end van de oplossing.
* Maakt een Python-console-app die roept de **lockDoor** directe methode in het gesimuleerde apparaat-app met behulp van een taak en updates de gewenste eigenschappen met behulp van een apparaattaak.

Aan het einde van deze zelfstudie hebt u twee Python-apps:

**simDevice.py**, die verbinding maakt met uw IoT-hub aan de apparaat-id en ontvangt een **lockDoor** directe methode.

**scheduleJobService.py**, die een rechtstreekse methode aanroepen in het gesimuleerde apparaat-app en werkt het dubbele apparaat gewenste eigenschappen met behulp van een taak.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x][lnk-python-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk *pip* [installeren of upgraden, het Python-pakketbeheersysteem][lnk-install-pip].
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++][lnk-visual-c-redist] om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

> [!NOTE]
> De **Azure IoT SDK voor Python** ondersteunt niet rechtstreeks **taken** functionaliteit. In plaats daarvan biedt deze zelfstudie een alternatieve oplossing met behulp van asynchrone threads en timers. Zie voor verdere updates, de **Service Client SDK** functielijst op de [Azure IoT SDK voor Python](https://github.com/Azure/azure-iot-sdk-python) pagina. 
> 
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]


## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie maakt u een Python-console-app die reageert op een rechtstreekse methode aangeroepen door de cloud, waartoe een gesimuleerde activeert **lockDoor** methode.

1. Bij de opdrachtprompt, voer de volgende opdracht voor het installeren van de **azure-iot-device-client** pakket:
   
    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Maak met een teksteditor een nieuw **simDevice.py** bestand in uw werkmap.

1. Voeg de volgende `import` instructies en -variabelen aan het begin van de **simDevice.py** bestand. Vervang `deviceConnectionString` door de verbindingsreeks van het apparaat dat u hierboven hebt gemaakt:
   
    ```python
    import time
    import sys

    import iothub_client
    from iothub_client import IoTHubClient, IoTHubClientError, IoTHubTransportProvider, IoTHubClientResult
    from iothub_client import IoTHubError, DeviceMethodReturnValue

    METHOD_CONTEXT = 0
    TWIN_CONTEXT = 0
    WAIT_COUNT = 10

    PROTOCOL = IoTHubTransportProvider.MQTT
    CONNECTION_STRING = "{deviceConnectionString}"
    ```

1. Voeg de volgende functie-callback voor het afhandelen van de **lockDoor** methode:
   
    ```python
    def device_method_callback(method_name, payload, user_context):
        if method_name == "lockDoor":
            print ( "Locking Door!" )

            device_method_return_value = DeviceMethodReturnValue()
            device_method_return_value.response = "{ \"Response\": \"lockDoor called successfully\" }"
            device_method_return_value.status = 200
            return device_method_return_value
    ```

1. Voeg een andere functie callback voor het afhandelen van device twins updates:

    ```python
    def device_twin_callback(update_state, payload, user_context):
        print ( "")
        print ( "Twin callback called with:")
        print ( "payload: %s" % payload )
    ```

1. Voeg de volgende code voor het registreren van de handler voor de **lockDoor** methode. Ook de `main` routine:
   
    ```python
    def iothub_jobs_sample_run():
        try:
            client = IoTHubClient(CONNECTION_STRING, PROTOCOL)
            client.set_device_method_callback(device_method_callback, METHOD_CONTEXT)
            client.set_device_twin_callback(device_twin_callback, TWIN_CONTEXT)

            print ( "Direct method initialized." )
            print ( "Device twin callback initialized." )
            print ( "IoTHubClient waiting for commands, press Ctrl-C to exit" )
        
            while True:
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
        print ( "Starting the IoT Hub Python jobs sample..." )
        print ( "    Protocol %s" % PROTOCOL )
        print ( "    Connection string=%s" % CONNECTION_STRING )

        iothub_jobs_sample_run()
    ```

1. Opslaan en sluiten de **simDevice.py** bestand.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u beleid voor opnieuw proberen (zoals exponentieel uitstel), zoals aangegeven in het artikel implementeren [afhandeling van tijdelijke fouten](/azure/architecture/best-practices/transient-faults).
> 
> 


## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Taken plannen voor een rechtstreekse methode aanroepen en het bijwerken van eigenschappen van een apparaatdubbel
In deze sectie maakt u een Python-consoletoepassing die een externe initieert **lockDoor** op een apparaat met een rechtstreekse methode en eigenschappen van het dubbele apparaat bijwerken.

1. Bij de opdrachtprompt, voer de volgende opdracht voor het installeren van de **azure-iot-service-client** pakket:
   
    ```cmd/sh
    pip install azure-iothub-service-client
    ```

1. Maak met een teksteditor een nieuw **scheduleJobService.py** bestand in uw werkmap.

1. Voeg de volgende `import` instructies en -variabelen aan het begin van de **scheduleJobService.py** bestand:
   
    ```python
    import sys
    import time
    import threading
    import uuid

    import iothub_service_client
    from iothub_service_client import IoTHubRegistryManager, IoTHubRegistryManagerAuthMethod
    from iothub_service_client import IoTHubDeviceTwin, IoTHubDeviceMethod, IoTHubError

    CONNECTION_STRING = "{IoTHubConnectionString}"
    DEVICE_ID = "{deviceId}"

    METHOD_NAME = "lockDoor"
    METHOD_PAYLOAD = "{\"lockTime\":\"10m\"}"
    UPDATE_JSON = "{\"properties\":{\"desired\":{\"building\":43,\"floor\":3}}}"
    TIMEOUT = 60
    WAIT_COUNT = 5
    ```

1. Voeg de volgende functie die wordt gebruikt om op te vragen apparaten:
   
    ```python
    def query_condition(device_id):
        iothub_registry_manager = IoTHubRegistryManager(CONNECTION_STRING)
    
        number_of_devices = 10
        dev_list = iothub_registry_manager.get_device_list(number_of_devices)
    
        for device in range(0, number_of_devices):
            if dev_list[device].deviceId == device_id:
                return 1

        print ( "Device not found" )
        return 0
    ```

1. Voeg de volgende methoden voor het uitvoeren van de taken die de directe methode en apparaat dubbele aanroepen:
   
    ```python
    def device_method_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job: " + str(job_id) )
        time.sleep(wait_time)
    
        if query_condition(device_id):
            iothub_device_method = IoTHubDeviceMethod(CONNECTION_STRING)
    
            response = iothub_device_method.invoke(device_id, METHOD_NAME, METHOD_PAYLOAD, TIMEOUT)
        
            print ( "" )
            print ( "Direct method " + METHOD_NAME + " called." )
        
    def device_twin_job(job_id, device_id, wait_time, execution_time):
        print ( "" )
        print ( "Scheduling job " + str(job_id) )
        time.sleep(wait_time)
    
        if query_condition(device_id):
            iothub_twin_method = IoTHubDeviceTwin(CONNECTION_STRING)
    
            twin_info = iothub_twin_method.update_twin(DEVICE_ID, UPDATE_JSON)
        
            print ( "" )
            print ( "Device twin updated." )
    ```

1. Voeg de volgende code om de taken plannen en werk de taakstatus. Ook de `main` routine:
   
    ```python
    def iothub_jobs_sample_run():
        try:
            method_thr_id = uuid.uuid4()
            method_thr = threading.Thread(target=device_method_job, args=(method_thr_id, DEVICE_ID, 20, TIMEOUT), kwargs={})
            method_thr.start()
        
            print ( "" )
            print ( "Direct method called with Job Id: " + str(method_thr_id) )
        
            twin_thr_id = uuid.uuid4()
            twin_thr = threading.Thread(target=device_twin_job, args=(twin_thr_id, DEVICE_ID, 10, TIMEOUT), kwargs={})
            twin_thr.start()
        
            print ( "" )
            print ( "Device twin called with Job Id: " + str(twin_thr_id) )
        
            while True:
                print ( "" )
            
                if method_thr.is_alive():
                    print ( "...job " + str(method_thr_id) + " still running." )
                else:
                    print ( "...job " + str(method_thr_id) + " complete." )
            
                if twin_thr.is_alive():
                    print ( "...job " + str(twin_thr_id) + " still running." )
                else:
                    print ( "...job " + str(twin_thr_id) + " complete." )
                
                print ( "Job status posted, press Ctrl-C to exit" )

                status_counter = 0
                while status_counter <= WAIT_COUNT:
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
        print ( "Starting the IoT Hub jobs Python sample..." )
        print ( "    Connection string = {0}".format(CONNECTION_STRING) )
        print ( "    Device ID         = {0}".format(DEVICE_ID) )

        iothub_jobs_sample_run()
    ```

1. Opslaan en sluiten de **scheduleJobService.py** bestand.


## <a name="run-the-applications"></a>De toepassingen uitvoeren
U kunt nu de toepassingen gaan uitvoeren.

1. Bij de opdrachtprompt in uw werkmap, de volgende opdracht uit om te luisteren naar de directe methode voor opnieuw opstarten:
   
    ```cmd/sh
    python simDevice.py
    ```

1. Voer de volgende opdracht voor het activeren van de taken voor het vergrendelen van de klep en bijwerken van het dubbele vanaf een andere opdrachtregel het volgende in uw werkmap:
   
    ```cmd/sh
    python scheduleJobService.py
    ```

1. U ziet de apparaat-antwoorden op de directe methode en apparaatdubbels bijwerken in de console.

    ![apparaat-uitvoer][1]

    ![service-uitvoer][2]


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie gebruikt u een taak voor het plannen van een rechtstreekse methode aan een apparaat en het bijwerken van eigenschappen van het dubbele apparaat.

Zie het volgende om door te gaan aan de slag met IoT Hub en patronen voor Apparaatbeheer zoals het op afstand via de lucht firmware-update:

[Zelfstudie: Hoe u doet een firmware-update][lnk-fwupdate]

Om door te gaan aan de slag met IoT Hub [aan de slag met Azure IoT Edge][lnk-iot-edge].

[lnk-get-started-twin]: iot-hub-python-twin-getstarted.md
[lnk-twin-props]: tutorial-device-twins.md
[lnk-c2d-methods]: quickstart-control-device-python.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: tutorial-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/

[1]: ./media/iot-hub-python-python-schedule-jobs/1.png
[2]: ./media/iot-hub-python-python-schedule-jobs/2.png
