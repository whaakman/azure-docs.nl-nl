> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md)

Dit overzicht van de [gesimuleerde apparaat Cloud uploaden voorbeeld] leest u hoe u [Azure IoT rand] [ lnk-sdk] IoT Hub apparaat-naar-cloud telemetrie verzenden van de gesimuleerde apparaten .

Dit overzicht omvat:

* **Architectuur**: architecturale informatie over de [gesimuleerde apparaat Cloud uploaden voorbeeld].
* **Ontwikkeling en uitvoering**: de stappen die nodig zijn om het voorbeeld te bouwen en uit te voeren.

## <a name="architecture"></a>Architectuur

De [gesimuleerde apparaat Cloud uploaden voorbeeld] laat zien hoe u een gateway die telemetrie van de gesimuleerde apparaten naar een iothub verzendt maken. Een apparaat mogelijk niet rechtstreeks verbinding maken met IoT Hub omdat het apparaat:

* Gebruikt geen een communicatieprotocol begrepen door de IoT Hub.
* Is geen smartcard te onthouden van de identiteit die is toegewezen door de IoT Hub.

Een IoT-Edge-gateway kan deze problemen op de volgende manieren oplossen:

* De gateway begrijpt het protocol dat wordt gebruikt door het apparaat ontvangt telemetrie voor apparaat-naar-cloud van het apparaat en stuurt deze berichten naar IoT Hub met behulp van een protocol begrepen door de IoT-hub.

* De gateway IoT Hub identiteiten wordt toegewezen aan apparaten en fungeert als een proxy wanneer een apparaat berichten naar IoT Hub verzendt.

Het volgende diagram toont de belangrijkste onderdelen van het voorbeeld, met inbegrip van de rand van de IoT-modules:

![Diagram - gesimuleerd apparaat bericht doorloopt gateway naar IoT Hub][1]

Dit voorbeeld bevat drie modules waaruit de gateway:
1. Module voor het protocolopname
1. Module MAC &lt;-&gt; IoT Hub-id
1. Module voor IoT Hub-communicatie

De modules geven geen berichten rechtstreeks aan elkaar door. De modules publiceren berichten naar een interne broker, die zorgt voor de berichten op de andere modules met behulp van een mechanisme voor het abonnement. Zie voor meer informatie [aan de slag met Azure IoT rand][lnk-gw-getstarted].

![Diagram - modules van de gateway communiceren met broker][2]

### <a name="protocol-ingestion-module"></a>Module voor het protocolopname

De module voor de opname protocol is het startpunt voor het proces van het maken van gegevens van apparaten, via de gateway en naar de cloud. 

In het voorbeeld in deze module:

1. Gesimuleerde temperatuur gegevens maakt. Als u fysieke apparaten, leest de module gegevens van deze fysieke apparaten.
1. Hiermee maakt u een bericht.
1. De gesimuleerde temperatuur-gegevens in de inhoud van het bericht geplaatst.
1. Hiermee voegt u een eigenschap met een valse MAC-adres toe aan het bericht.
1. Maakt het bericht beschikbaar voor de volgende module in de keten.

De module protocol opname is **simulated_device.c** in de broncode.

### <a name="mac-lt-gt-iot-hub-id-module"></a>Module MAC &lt;-&gt; IoT Hub-id

De MAC &lt; - &gt; IoT Hub ID module werkt als een conversieprogramma. In dit voorbeeld wordt een MAC-adres gebruikt als een unieke apparaat-id en wordt deze gekoppeld aan de id van een IoT Hub-apparaat. U kunt echter uw eigen module schrijven die een andere unieke id gebruikt. Bijvoorbeeld, uw apparaten mogelijk zijn unieke serienummers of de telemetriegegevens kan de naam van een unieke embedded-apparaat bevatten.

In het voorbeeld in deze module:

1. Scant voor berichten met een MAC-adres-eigenschap.
1. Als er een MAC-adres, wordt een andere eigenschap met de sleutel van een IoT Hub apparaat toegevoegd aan het bericht. 
1. Maakt het bericht beschikbaar voor de volgende module in de keten.

De ontwikkelaar stelt een toewijzing tussen MAC-adressen en IoT Hub identiteiten in de gesimuleerde apparaten koppelen met IoT Hub apparaat-id's. De ontwikkelaar wordt de toewijzing handmatig toegevoegd als onderdeel van de moduleconfiguratie.

De MAC &lt; - &gt; IoT Hub-ID-module is **identitymap.c** in de broncode. 

### <a name="iot-hub-communication-module"></a>Module voor IoT Hub-communicatie

De IoT Hub communicatie-module geopend één HTTPS-verbinding van de gateway met de IoT Hub. HTTPS is een van de drie protocollen die IoT Hub kent. Deze module voorkomt u dat een verbinding te openen voor elk apparaat door multiplex verbindingen van alle apparaten via een verbinding. Deze aanpak kunt één gateway om veel apparaten te verbinden. 

In het voorbeeld in deze module:

1. Berichten duurt met een IoT Hub apparaat-sleuteleigenschap die is toegewezen door de vorige module. 
1. Inhoud van het bericht verzendt naar IoT Hub met behulp van het HTTPS-protocol. 

De module IoT Hub-communicatie is **iothub.c** in de broncode.

## <a name="before-you-get-started"></a>Voordat u aan de slag gaat

Voordat u begint, moet u het volgende doen:

* [Een iothub maken] [ lnk-create-hub] in uw Azure-abonnement. Voor dit scenario voorbeeld moet u de naam van uw hub. Als u geen account hebt, kunt u binnen een paar minuten een [gratis account][lnk-free-trial] maken.
* Twee apparaten toevoegen aan uw IoT-hub en noteer de id's en apparaten sleutels. U kunt de [apparaat explorer] [ lnk-device-explorer] of [iothub explorer] [ lnk-iothub-explorer] hulpprogramma's voor apparaten toevoegen aan de IoT-hub en ophalen van de sleutels.


<!-- Images -->
[1]: media/iot-hub-iot-edge-simulated-selector/image1.png
[2]: media/iot-hub-iot-edge-simulated-selector/image2.png

<!-- Links -->
[gesimuleerde apparaat Cloud uploaden voorbeeld]: https://github.com/Azure/iot-edge/blob/master/samples/simulated_device_cloud_upload/README.md
[lnk-sdk]: https://github.com/Azure/iot-edge
[lnk-gw-getstarted]: ../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-device-explorer]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer
[lnk-iothub-explorer]: https://github.com/Azure/iothub-explorer/blob/master/readme.md
[lnk-create-hub]: ../articles/iot-hub/iot-hub-create-through-portal.md