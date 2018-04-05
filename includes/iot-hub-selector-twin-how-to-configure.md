> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [C#/Node.js](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-how-to-configure.md)
> * [Python](../articles/iot-hub/iot-hub-python-python-twin-how-to-configure.md)
> 
> 

## <a name="introduction"></a>Inleiding

In [aan de slag met IoT Hub apparaat horende][lnk-twin-tutorial], hebt u geleerd hoe in te stellen apparaten metagegevens met *labels*. U voorwaarden apparaat ontvangen van een apparaat-app met *gerapporteerd eigenschappen*, en vervolgens deze gegevens door middel van een SQL-achtige taal opgevraagd.

Deze zelfstudie wordt beschreven hoe u de apparaat-twin *gewenst eigenschappen* en *eigenschappen gerapporteerd* apparaat-apps op afstand te configureren. Gemeld en gewenste eigenschappen in een twin apparaat een configuratie met meerdere stappen van de apparaattoepassing van een inschakelen en geef de zichtbaarheid van de status van deze bewerking op alle apparaten. U vindt meer informatie over de rol van apparaatconfiguraties in [overzicht van Apparaatbeheer met IoT Hub][lnk-dm-overview].

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

Op een hoog niveau kunt horende apparaten het back-end oplossing om op te geven van de gewenste configuratie voor de beheerde apparaten, in plaats van door specifieke opdrachten te sturen. Het apparaat is verantwoordelijk voor het instellen van de beste manier om de configuratie van (belangrijk in IoT-scenario's waarin specifieke apparaat voorwaarden invloed hebben op het onmiddellijk uit te voeren door specifieke opdrachten) bijwerken tijdens het rapportage voortdurend de huidige status en potentiële fouten van het updateproces. Dit patroon is noodzakelijk hulpmiddel voor het beheer van grote sets van apparaten, zoals dit de volledige oplossing back-end-zichtbaarheid van de status van het configuratieproces op alle apparaten biedt.

> [!TIP]
> In scenario's waarin apparaten worden beheerd op een nieuwe interactieve wijze (bijvoorbeeld een ventilator van een gebruiker beheerde app inschakelen), kunt u overwegen [methoden directe][lnk-methods].

De back-end oplossing wijzigingen in deze zelfstudie de telemetrie-configuratie van een apparaat zodat de apparaattoepassing van toepassing een configuratie-update is. Een configuratie-update zou bijvoorbeeld een software-module opnieuw wordt opgestart, die in deze zelfstudie met een eenvoudige vertraging simuleert vereisen.

De back-end oplossing slaat de configuratie in de apparaat-twin gewenste eigenschappen in de volgende manier:

        {
            ...
            "properties": {
                ...
                "desired": {
                    "telemetryConfig": {
                        "configId": "{id of the configuration}",
                        "sendFrequency": "{config}"
                    }
                }
                ...
            }
            ...
        }

Omdat configuraties kunnen complexe objecten, unieke id's zijn toegewezen (hashes of [GUID's][lnk-guid]).


De app apparaat rapporteert de huidige configuratie voor het spiegelen van de gewenste eigenschap **telemetryConfig** in de gerapporteerde eigenschappen:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "configId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Opmerking hoe de gerapporteerde **telemetryConfig** heeft een extra eigenschap **status**die worden gebruikt voor het rapporteren van de status van het configuratieproces van de update.

Wanneer een nieuwe gewenste configuratie wordt ontvangen, rapporteert de apparaattoepassing een in behandeling configuratie door de status te wijzigen:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "configId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "configId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Klik vervolgens op een later tijdstip rapporteert de apparaattoepassing het slagen of mislukken van deze bewerking door het bijwerken van de eigenschap. De back-end oplossing kan de status van het configuratieproces opvragen op alle apparaten op elk gewenst moment.

In deze handleiding ontdekt u hoe u:

* Maakt een gesimuleerd apparaat-app die configuratie-updates ontvangt van de back-end van de oplossing en rapporten van meerdere updates als *eigenschappen gerapporteerd* proces van de configuratie niet bijwerken.
* Maak een back-end-app die werkt u de gewenste configuratie van een apparaat en het configuratieproces van de update vervolgens een query.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
