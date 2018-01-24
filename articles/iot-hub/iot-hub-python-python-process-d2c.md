---
title: Routeren van berichten met Azure IoT Hub (Python) | Microsoft Docs
description: Hoe Azure IoT Hub apparaat-naar-cloud-berichten verwerken met behulp van regels voor het doorsturen en aangepaste eindpunten verzending van berichten naar andere back-end-services.
services: iot-hub
documentationcenter: python
author: msebolt
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: python
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2018
ms.author: v-masebo
ms.openlocfilehash: 24c556c8ea585a522c890dab16ae9fb96dc51c22
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="routing-messages-with-iot-hub-python"></a>Routeren van berichten met IoT Hub (Python)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Deze zelfstudie bouwt voort op de [aan de slag met IoT Hub] zelfstudie.  De zelfstudie:

* Laat zien hoe u regels voor doorsturen verzending van apparaat-naar-cloud-berichten in een eenvoudig en gebaseerd op configuratie-hulpprogramma te gebruiken.
* Laat zien hoe isoleren interactieve berichten die directe actie van de back-end oplossing voor verdere verwerking is vereist.  Een apparaat kan bijvoorbeeld een waarschuwing weergegeven dat activeert een ticket in een CRM-systeem invoegen verzenden.  Daarentegen feed gegevenspunt berichten, zoals temperatuur telemetrie, in een analyse-engine.

Aan het einde van deze zelfstudie, moet u drie Python console apps uitvoeren:

* **SimulatedDevice.py**, een aangepaste versie van de app gemaakt in de [aan de slag met IoT Hub] zelfstudie gegevenspunt apparaat-naar-cloud-berichten verzendt elke tweede en interactieve apparaat-naar-cloud-berichten per willekeurige interval. Deze app gebruikt de protocollen MQTT-protocol om te communiceren met IoT Hub.
* **ReadCriticalQueue.py** ongedaan de kritieke berichten uit de Service Bus-wachtrij gekoppeld aan de IoT-hub in de wachtrij geplaatst.

> [!NOTE]
> IoT Hub heeft ondersteuning voor veel apparaatplatforms en talen, waaronder C, Java en JavaScript SDK. Zie voor instructies over hoe het apparaat in deze zelfstudie vervangen door een fysiek apparaat en hoe apparaten verbinden met een IoT-Hub, de [Azure IoT Developer Center].

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

* Een volledige werkende versie van de [aan de slag met IoT Hub] zelfstudie.
* [Python 2.x of 3.x][lnk-python-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Zorg ervoor dat u Python toevoegt aan uw platformspecifieke omgevingsvariabele als u hierom wordt gevraagd tijdens de installatie. Als u Python 2.x gebruikt, moet u mogelijk *pip* [installeren of upgraden, het Python-pakketbeheersysteem][lnk-install-pip].
* Als u een Windows-besturingssysteem hebt, gebruikt u vervolgens het [herdistribueerbare pakket van Visual C++][lnk-visual-c-redist] om het gebruik van systeemeigen DLL's van Python mogelijk te maken.
* [Node.js 4.0 of hoger][lnk-node-download]. Zorg ervoor dat u de 32-bits of 64-bits installatie gebruikt, zoals vereist door uw configuratie. Dit is nodig voor het installeren van het [hulpprogramma IoT Hub-verkenner][lnk-iot-hub-explorer].
* Een actief Azure-account. (Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.)

Wordt ook aangeraden lezen over [Azure Storage] en [Azure Service Bus].


## <a name="send-interactive-messages-from-a-device-app"></a>Interactieve berichten verzenden vanuit een apparaat-app
In deze sectie die u wijzigt de apparaat-app die u hebt gemaakt in de [aan de slag met IoT Hub] zelfstudie af en toe om berichten te verzenden waarvoor onmiddellijke verwerking.

1. Gebruik een teksteditor openen de **SimulatedDevice.py** bestand. Dit bestand bevat de code voor de **SimulatedDevice** app die u hebt gemaakt in de [aan de slag met IoT Hub] zelfstudie.

2. Vervang de **iothub_client_telemetry_sample_run** functie met de volgende code:

    ```python
    def iothub_client_telemetry_sample_run():

    try:
        client = iothub_client_init()
        print ( "IoT Hub device sending periodic messages, press Ctrl-C to exit" )
        message_counter = 0

        while True:
            random_seed = random.random()
            msg_txt_formatted = MSG_TXT % (AVG_WIND_SPEED + (random_seed * 4 + 2))
            # messages can be encoded as string or bytearray
            if (message_counter & 1) == 1:
                message = IoTHubMessage(bytearray(msg_txt_formatted, 'utf8'))
            else:
                message = IoTHubMessage(msg_txt_formatted)
            # optional: assign ids
            message.message_id = "message_%d" % message_counter
            message.correlation_id = "correlation_%d" % message_counter
            # optional: assign properties
            prop_map = message.properties()
            if random_seed > .5:
                if random_seed > .8:
                    prop_map.add("level", 'critical')
                else:
                    prop_map.add("level", 'storage')

            client.send_event_async(message, send_confirmation_callback, message_counter)
            print ( "IoTHubClient.send_event_async accepted message [%d] for transmission to IoT Hub." % message_counter )

            status = client.get_send_status()
            print ( "Send status: %s" % status )
            time.sleep(30)

            status = client.get_send_status()
            print ( "Send status: %s" % status )

            message_counter += 1

    except IoTHubError as iothub_error:
        print ( "Unexpected error %s from IoTHub" % iothub_error )
        return
    except KeyboardInterrupt:
        print ( "IoTHubClient sample stopped" )
    ```
   
    Deze methode wordt willekeurig toegevoegd voor de eigenschap `"level": "critical"` en `"level": "storage"` op berichten die door het apparaat verzonden, die een bericht waarvoor onmiddellijke actie door de back-end van de toepassing of een die moet worden permanent opgeslagen simuleert. De toepassing ondersteunt routeren van berichten op basis van de berichttekst.
   
   > [!NOTE]
   > U kunt de berichteigenschappen om berichten te routeren voor verschillende scenario's, inclusief de verwerking van koude pad, naast het hot pad voorbeeld dat hier wordt weergegeven.

1. Sla het bestand **SimulatedDevice.py** op en sluit het.

    > [!NOTE]
    > Omwille van de eenvoud in deze zelfstudie niet geïmplementeerd voor een beleid voor opnieuw proberen. In productiecode moet u een beleid voor opnieuw proberen zoals exponentieel uitstel, zoals voorgesteld in het MSDN-artikel implementeren [afhandeling van tijdelijke fout].


## <a name="add-queues-to-your-iot-hub-and-route-messages-to-them"></a>Wachtrijen toevoegen aan uw IoT hub en route-berichten toe

In deze sectie zowel een Service Bus-wachtrij en een opslagaccount maken, verbind ze met uw IoT-hub en configureren van uw IoT-hub om berichten te verzenden naar de wachtrij op basis van de aanwezigheid van een eigenschap van het bericht en alle berichten naar het opslagaccount. Zie voor meer informatie over het verwerken van berichten van Service Bus-wachtrijen [aan de slag met wachtrijen] [ lnk-sb-queues-node] en hoe u opslag te beheren, Zie [aan de slag met Azure Storage] [Azure Storage].

1. Een Service Bus-wachtrij maakt, zoals beschreven in [aan de slag met wachtrijen][lnk-sb-queues-node]. Noteer de naam van de naamruimte en de wachtrij.

1. Open uw IoT-hub in de Azure-portal en klikt u op **eindpunten**.

    ![Eindpunten van IoT-hub][30]

1. In de **eindpunten** blade, klikt u op **toevoegen** boven uw wachtrij toevoegen aan uw IoT-hub. Naam van het eindpunt **CriticalQueue** en selecteer met de vervolgkeuzelijsten **Service Bus-wachtrij**, de Service Bus-naamruimte waarin de wachtrij zich bevindt en de naam van de wachtrij. Wanneer u klaar bent, klikt u op **OK** onderaan.  

    ![Een eindpunt toevoegen][31]

1. Klik nu op **Routes** in uw IoT-Hub. Klik op **toevoegen** boven aan de blade voor het maken van een regel voor doorsturen waarmee berichten worden doorgestuurd naar de wachtrij die u zojuist hebt toegevoegd. 

    ![Toevoegen van een route][34]

    Voer een naam en selecteer **apparaat-berichten** als de bron van gegevens. Kies **CriticalQueue** als een aangepaste eindpunt als de routering regel endpoint en voer `level="critical"` als de queryreeks. Klik op **opslaan** onderaan.

    ![Routedetails][32]

    Zorg ervoor dat de terugval route is ingesteld op **ON**. Deze instelling is de standaardconfiguratie van een IoT-hub.

    ![Route voor terugval][33]

## <a name="optional-read-from-the-queue-endpoint"></a>(Optioneel) Lezen van het eindpunt van de wachtrij

In deze sectie maakt u een Python-consoletoepassing die kritieke berichten uit IoT Service Bus lezen. U vindt meer informatie in [aan de slag met wachtrijen][lnk-sb-queues-node]. 

1. Open een nieuwe opdrachtprompt en installeer als volgt de apparaat-SDK voor Azure IoT Hub voor Python. Sluit de opdrachtprompt na de installatie.

    ```cmd/sh
    pip install azure-servicebus
    ```

    > [!NOTE]
    > Voor problemen met het installeren van de **azure servicebus** pakket of Zie voor meer opties voor de installatie de [Python-pakket van azure servicebus][lnk-python-service-bus].

1. Maak een bestand met de naam **ReadCriticalQueue.py**. Open het bestand in een Python-editor/IDE naar keuze, bijvoorbeeld IDLE.

1. Voeg de volgende code voor het importeren van de vereiste modules van het apparaat SDK:

    ```python
    from azure.servicebus import ServiceBusService, Message, Queue
    ```

1. Voeg de volgende code en vervang de tijdelijke aanduidingen door verbindingsgegevens voor uw servicebus:

    ```python
    SERVICE_BUS_NAME = {serviceBusName}
    SHARED_ACCESS_POLICY_NAME = {sharedAccessPolicyName}
    SHARED_ACCESS_POLICY_KEY_VALUE = {sharedAccessPolicyKeyValue}
    QUEUE_NAME = {queueName}    
    ```

5. Voeg de volgende code om te verbinden met en het lezen van de servicebus: 

    ```python
    def setup_client():
        bus_service = ServiceBusService(
        service_namespace=SERVICE_BUS_NAME,
        shared_access_key_name=SHARED_ACCESS_POLICY_NAME,
        shared_access_key_value=SHARED_ACCESS_POLICY_KEY_VALUE)

        while True:
            msg = bus_service.receive_queue_message(QUEUE_NAME, peek_lock=False)
            print(msg.body)
    ```

1. Voeg ten slotte de hoofdfunctie toe. 

    ```python
    if __name__ == '__main__':
        setup_client()
    ```

1. Sla op en sluit de **ReadCriticalQueue.py** bestand. U kunt nu de toepassingen gaan uitvoeren.


## <a name="run-the-applications"></a>De toepassingen uitvoeren

U kunt nu de toepassingen gaan uitvoeren.

1. Open een opdrachtprompt en installeer de IoT Hub-verkenner. 

    ```cmd/sh
    npm install -g iothub-explorer
    ```

1. Voer de volgende opdracht uit in de opdrachtprompt om de apparaat-naar-cloud-berichten van uw apparaat bij te houden. Gebruik de verbindingsreeks van uw IoT Hub in de tijdelijke aanduiding na `--login`.

    ```cmd/sh
    iothub-explorer monitor-events [deviceId] --login "[IoTHub connection string]"
    ```

1. Open een nieuwe opdrachtprompt en navigeer naar de map met het bestand **SimulatedDevice.py**.

1. Voer het bestand **SimulatedDevice.py** uit. Dit bestand stuurt periodiek telemetriegegevens naar uw IoT Hub. 
   
    ```cmd/sh
    python SimulatedDevice.py
    ```

1. Om uit te voeren de **ReadCriticalQueue** toepassing in een opdrachtprompt of een shell gaat u naar **ReadCriticalQueue.py** -bestand en voer de volgende opdracht:

   ```cmd/sh
   python ReadCriticalQueue.py
   ```

1. Bekijk de apparaatberichten in de opdrachtprompt die in het vorige gedeelte werd uitgevoerd in de IoT Hub-verkenner. Houd rekening met de `critical` berichten in de **ReadCriticalQueue** toepassing.

    ![Apparaat-naar-cloud-berichten met Python][2]


## <a name="optional-add-storage-container-to-your-iot-hub-and-route-messages-to-it"></a>(Optioneel) Storage-Container toevoegen aan uw IoT hub en route-berichten naar het

In deze sectie een opslagaccount maken, verbinden met uw IoT-hub en configureren van uw iothub berichten verzenden naar de account op basis van de aanwezigheid van een eigenschap van het bericht. Zie voor meer informatie over het beheren van opslag [aan de slag met Azure Storage][Azure Storage].

 > [!NOTE]
   > IoT Hub-accounts die zijn gemaakt onder het _F1 gratis_ laag zijn beperkt tot één **eindpunt**. Als u niet beperkt tot één **eindpunt**, u kunt setup de **StorageContainer** naast de **CriticalQueue** en beide simulatneously uitvoeren.

1. Een opslagaccount maken, zoals beschreven in [documentatie bij Azure Storage][lnk-storage]. Noteer de accountnaam.

2. Open uw IoT-hub in de Azure-portal en klikt u op **eindpunten**.

3. In de **eindpunten** blade, selecteer de **CriticalQueue** eindpunt en klik op **verwijderen**. Klik op **Ja**, en klik vervolgens op **toevoegen**. Naam van het eindpunt **StorageContainer** en selecteer met de vervolgkeuzelijsten **Azure Storage-Container**, en maak een **opslagaccount** en een **opslag container**.  Noteer de namen.  Wanneer u klaar bent, klikt u op **OK** onderaan. 

 > [!NOTE]
   > IoT Hub-accounts die zijn gemaakt onder het _F1 gratis_ laag zijn beperkt tot één **eindpunt**. Als u niet beperkt tot één **eindpunt**, u hoeft niet te verwijderen de **CriticalQueue**.

4. Klik op **Routes** in uw IoT-Hub. Klik op **toevoegen** boven aan de blade voor het maken van een regel voor doorsturen waarmee berichten worden doorgestuurd naar de wachtrij die u zojuist hebt toegevoegd. Selecteer **apparaat-berichten** als de bron van gegevens. Voer `level="storage"` als de voorwaarde, en kies **StorageContainer** als een aangepaste eindpunt als de routering eindpunt van de regel. Klik op **opslaan** onderaan.  

    Zorg ervoor dat de terugval route is ingesteld op **ON**. Deze instelling is de standaardconfiguratie van een IoT-hub.

1. Zorg ervoor dat uw vorige toepassing **SimulatedDevice.py** nog steeds actief is. 

1. In de Azure-Portal, gaat u naar uw opslagaccount onder **Blob-Service**, klikt u op **blobs bladeren...** .  Selecteer de container, gaat u naar en klikt u op het JSON-bestand en klikt u op **downloaden** om de gegevens weer te geven.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe betrouwbaar apparaat-naar-cloud-berichten verzenden met behulp van de berichtroutering van IoT Hub.

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT Hub [Azure IoT Suite][lnk-suite].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub].

Zie voor meer informatie over het routeren van berichten van IoT-Hub, [berichten verzenden en ontvangen met IoT Hub][lnk-devguide-messaging].

<!-- Images. -->
[2]: ./media/iot-hub-python-python-process-d2c/output.png

[30]: ./media/iot-hub-python-python-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-python-python-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-python-python-process-d2c/route-creation.png
[33]: ./media/iot-hub-python-python-process-d2c/fallback-route.png
[34]: ./media/iot-hub-python-python-process-d2c/click-routes.png

<!-- Links -->
[lnk-python-download]: https://www.python.org/downloads/
[lnk-visual-c-redist]: http://www.microsoft.com/download/confirmation.aspx?id=48145
[lnk-node-download]: https://nodejs.org/en/download/
[lnk-install-pip]: https://pip.pypa.io/en/stable/installing/
[lnk-iot-hub-explorer]: https://github.com/Azure/iothub-explorer
[lnk-sb-queues-node]: ../service-bus-messaging/service-bus-python-how-to-use-queues.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[Ontwikkelaarshandleiding voor IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[aan de slag met IoT Hub]: iot-hub-python-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot

[afhandeling van tijdelijke fout]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-free-trial]: https://azure.microsoft.com/free/
[lnk-storage]: https://docs.microsoft.com/en-us/azure/storage/
[lnk-python-service-bus]: https://pypi.python.org/pypi/azure-servicebus