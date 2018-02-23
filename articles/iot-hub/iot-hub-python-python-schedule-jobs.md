---
title: Plannen van taken met Azure IoT Hub (Python) | Microsoft Docs
description: Klik hier voor meer informatie over het plannen van een taak Azure IoT Hub een directe methode op meerdere apparaten aan te roepen. U kunt Azure IoT SDK's voor Python gebruiken voor het implementeren van het gesimuleerde apparaat-apps en een app service de taak uit te voeren.
services: iot-hub
documentationcenter: .net
author: msebolt
manager: timlt
editor: 
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/16/2018
ms.author: v-masebo
ms.openlocfilehash: 3b6daf1ce1116ea3787550f9dac8d90b3751f9af
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="schedule-and-broadcast-jobs-python"></a>Planning en broadcast-taken (Python)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Azure IoT Hub is een volledig beheerde service waarmee u een back-end-app maken en bijhouden van taken die gepland en miljoenen apparaten werken.  Taken kunnen worden gebruikt voor de volgende acties:

* Gewenste eigenschappen bijwerken
* Labels bijwerken
* Directe methoden aanroepen

Conceptueel gezien een taak een van deze acties worden verpakt en de voortgang van de uitvoering op basis van een verzameling apparaten, dit wordt gedefinieerd door een apparaat twin query.  Een back-endserver voor apps kunt bijvoorbeeld een taak een methode voor opnieuw opstarten op 10.000 apparaten, is opgegeven door een apparaat twin query en gepland op een later tijdstip aan te roepen.  Deze toepassing kunt vervolgens voortgang bijhouden als elk van deze apparaten ontvangen en de methode voor opnieuw opstarten worden uitgevoerd.

Meer informatie over elk van deze mogelijkheden in deze artikelen:

* Apparaat-twin en eigenschappen: [aan de slag met apparaat horende] [ lnk-get-started-twin] en [zelfstudie: het gebruik van twin apparaateigenschappen][lnk-twin-props]
* Directe methoden: [IoT Hub developer guide - rechtstreekse methoden] [ lnk-dev-methods] en [zelfstudie: directe methoden][lnk-c2d-methods]

In deze handleiding ontdekt u hoe u:

* Maken van een gesimuleerde apparaattoepassing van Python met een directe methode waarmee **lockDoor**, die kan worden aangeroepen door de back-end oplossing.
* Een Python consoletoepassing maken die roept de **lockDoor** directe methode in de gesimuleerde apparaattoepassing een taak en updates met de gewenste eigenschappen met behulp van een taak van het apparaat.

Aan het einde van deze zelfstudie hebt u twee Python-apps:

**simDevice.py**, die is verbonden met uw IoT-hub aan de apparaat-id en ontvangt een **lockDoor** directe methode.

**scheduleJobService.py**, die een directe methode wordt aangeroepen in de gesimuleerde apparaattoepassing en updates van de apparaat-twin gewenst eigenschappen met behulp van een taak.

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* [Python 2.x of 3.x][lnk-python-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk *pip* [installeren of upgraden, het Python-pakketbeheersysteem][lnk-install-pip].
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++][lnk-visual-c-redist] om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

> [!NOTE]
> De **Azure IoT SDK voor Python** ondersteunt niet direct **taken** functionaliteit. In plaats daarvan biedt deze zelfstudie een alternatieve oplossing met behulp van asynchrone threads en timers. Zie voor verdere updates, de **Service Client SDK** functielijst op de [Azure IoT SDK voor Python](https://github.com/Azure/azure-iot-sdk-python) pagina. 
> 
> 

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]


## <a name="create-a-simulated-device-app"></a>Een gesimuleerde apparaattoepassing maken
In deze sectie maakt u een Python consoletoepassing maken die reageert op een directe methode is aangeroepen door de cloud, die een gesimuleerde activeert **lockDoor** methode.

1. Voer bij de opdrachtprompt de volgende opdracht voor het installeren van de **azure-iot-apparaat-client** pakket:
   
    ```cmd/sh
    pip install azure-iothub-device-client
    ```

1. Start een teksteditor en maak een nieuwe **simDevice.py** bestand in uw werkmap.

1. Voeg de volgende `import` -instructies en variabelen aan het begin van de **simDevice.py** bestand. Vervang `deviceConnectionString` met de verbindingsreeks van het apparaat dat u hiervoor hebt gemaakt:
   
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

1. Voeg de volgende functie-retouraanroep voor het afhandelen van de **lockDoor** methode:
   
    ```python
    def device_method_callback(method_name, payload, user_context):
        if method_name == "lockDoor":
            print ( "Locking Door!" )

            device_method_return_value = DeviceMethodReturnValue()
            device_method_return_value.response = "{ \"Response\": \"lockDoor called successfully\" }"
            device_method_return_value.status = 200
            return device_method_return_value
    ```

1. Een andere functie retouraanroep voor het afhandelen van horende apparaatupdates toevoegen:

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

1. Sla op en sluit de **simDevice.py** bestand.

> [!NOTE]
> Om de zaken niet nodeloos ingewikkeld te maken, is in deze handleiding geen beleid voor opnieuw proberen geïmplementeerd. Bij de productiecode moet u een beleid voor opnieuw proberen implementeren (zoals exponentieel uitstel), zoals aangegeven in het MSDN-artikel [Transient Fault Handling][lnk-transient-faults] (Afhandeling van tijdelijke fouten).
> 
> 


## <a name="schedule-jobs-for-calling-a-direct-method-and-updating-a-device-twins-properties"></a>Plannen van taken voor het aanroepen van een directe methode en de eigenschappen voor een apparaat-twin bijwerken
In deze sectie maakt u een Python consoletoepassing maken die een externe initieert **lockDoor** op een apparaat met een directe methode en de eigenschappen van de apparaat-twin bijwerken.

1. Voer bij de opdrachtprompt de volgende opdracht voor het installeren van de **azure-iot-service-client** pakket:
   
    ```cmd/sh
    pip install azure-iothub-service-client
    ```

1. Start een teksteditor en maak een nieuwe **scheduleJobService.py** bestand in uw werkmap.

1. Voeg de volgende `import` -instructies en variabelen aan het begin van de **scheduleJobService.py** bestand:
   
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

1. Voeg de volgende functie die wordt gebruikt voor de query voor apparaten:
   
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

1. Voeg de volgende methoden om uit te voeren van de taken die de directe methode en apparaat-twin aanroepen:
   
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

1. Voeg de volgende code voor het plannen van de taken en taakstatus bijwerken. Ook de `main` routine:
   
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

1. Sla op en sluit de **scheduleJobService.py** bestand.


## <a name="run-the-applications"></a>De toepassingen uitvoeren
U kunt nu de toepassingen gaan uitvoeren.

1. Bij de opdrachtprompt in uw werkmap, voer de volgende opdracht om te beginnen met luisteren naar de directe methode voor opnieuw opstarten:
   
    ```cmd/sh
    python simDevice.py
    ```

1. Bij een andere opdrachtprompt in uw werkmap, voer de volgende opdracht voor het activeren van de taken voor het vergrendelen van de deur en de twin bijwerken:
   
    ```cmd/sh
    python scheduleJobService.py
    ```

1. U ziet de antwoorden van het apparaat aan de directe methode en apparaat horende bijwerken in de console.

    ![apparaat-uitvoer][1]

    ![service-uitvoer][2]


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie gebruikt u een taak om een directe methode voor een apparaat en het bijwerken van de eigenschappen van de apparaat-twin plannen.

Zie het volgende om door te gaan aan de slag met IoT Hub en device management patronen, zoals extern via de lucht firmware-update:

[Zelfstudie: Hoe u een firmware-update][lnk-fwupdate]

Zie het volgende om door te gaan aan de slag met IoT Hub [aan de slag met Azure IoT rand][lnk-iot-edge].

[lnk-get-started-twin]: iot-hub-python-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-python-python-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/tree/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-transient-faults]: https://docs.microsoft.com/azure/architecture/best-practices/transient-faults

[1]: ./media/iot-hub-python-python-schedule-jobs/1.png
[2]: ./media/iot-hub-python-python-schedule-jobs/2.png
