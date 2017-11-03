> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-how-to-configure.md)
> * [C#/node.js](../articles/iot-hub/iot-hub-csharp-node-twin-how-to-configure.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-how-to-configure.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-how-to-configure.md)
> 
> 

## <a name="introduction"></a>Inleiding

In [aan de slag met IoT Hub apparaat horende][lnk-twin-tutorial], hebt u geleerd hoe u de metagegevens van apparaten instellen van uw oplossing voor back-end met *labels*, voorwaarden apparaat vanuit een app apparaat rapporteren met behulp van *gerapporteerd eigenschappen*, en deze gegevens door middel van een SQL-achtige taal opvragen.

In deze zelfstudie maakt u informatie over het gebruik van de apparaat-twin *gewenst eigenschappen* samen met *gerapporteerd eigenschappen*, apparaat-apps op afstand te configureren. Meer specifiek, deze zelfstudie laat zien hoe een apparaat-twin gerapporteerd en gewenste eigenschappen een configuratie met meerdere stappen van de apparaattoepassing van een inschakelen en de zichtbaarheid van de oplossing voor back-end van de status van deze bewerking op alle apparaten opgeven. U vindt meer informatie over de rol van apparaatconfiguraties in [overzicht van Apparaatbeheer met IoT Hub][lnk-dm-overview].

Op een hoog niveau kunt horende apparaten het back-end oplossing om op te geven van de gewenste configuratie voor de beheerde apparaten, in plaats van door specifieke opdrachten te sturen. Hiermee wordt het apparaat die verantwoordelijk is voor de beste manier om het bijwerken van de configuratie ervan (belangrijk in IoT-scenario's waarin specifieke apparaat voorwaarden invloed hebben op het onmiddellijk uit te voeren door specifieke opdrachten) in te stellen geplaatst tijdens voortdurend rapporteren aan de back-end oplossing de huidige status en potentiële fouten van het updateproces. Dit patroon is noodzakelijk hulpmiddel voor het beheer van grote sets van apparaten, zoals kunnen de back-end oplossing volledig inzicht in de status van het configuratieproces hebben op alle apparaten.

> [!NOTE]
> In scenario's waarin apparaten worden beheerd op een nieuwe interactieve wijze (inschakelen voor een ventilator van een gebruiker beheerde app), kunt u overwegen [methoden directe][lnk-methods].
> 
> 

In deze zelfstudie, verandert de back-end oplossing de telemetrie-configuratie van een doelapparaat en dat, als gevolg hiervan volgt de apparaattoepassing een proces met meerdere stappen voor het toepassen van een configuratie-update (bijvoorbeeld vereisen van een software-module opnieuw wordt opgestart, waarvoor deze zelfstudie simuleert met een eenvoudige vertraging).

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

> [!NOTE]
> Omdat configuraties kunnen complexe objecten, unieke id's zijn toegewezen (hashes of [GUID's][lnk-guid]) voor het vereenvoudigen van hun vergelijkingen.
> 
> 

De app apparaat rapporteert de huidige configuratie voor het spiegelen van de gewenste eigenschap **telemetryConfig** in de gerapporteerde eigenschappen:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Success",
                    }
                }
                ...
            }
        }

Opmerking hoe de gerapporteerde **telemetryConfig** heeft een extra eigenschap **status**die worden gebruikt voor het rapporteren van de status van het configuratieproces van de update.

Wanneer een nieuwe gewenste configuratie wordt ontvangen, rapporteert de apparaattoepassing een in behandeling configuratie door de gegevens te wijzigen:

        {
            "properties": {
                ...
                "reported": {
                    "telemetryConfig": {
                        "changeId": "{id of the current configuration}",
                        "sendFrequency": "{current configuration}",
                        "status": "Pending",
                        "pendingConfig": {
                            "changeId": "{id of the pending configuration}",
                            "sendFrequency": "{pending configuration}"
                        }
                    }
                }
                ...
            }
        }

Klik vervolgens op een later tijdstip rapporteert de apparaattoepassing het slagen of mislukken van deze bewerking door het bijwerken van de bovenstaande eigenschap.
Houd er rekening mee hoe de back-end oplossing kunnen op elk gewenst moment de status van de configuratie niet opvragen op alle apparaten.

In deze handleiding ontdekt u hoe u:

* Maakt een gesimuleerd apparaat-app die configuratie-updates ontvangt van de back-end van de oplossing en rapporten van meerdere updates als *eigenschappen gerapporteerd* proces van de configuratie niet bijwerken.
* Maak een back-end-app die werkt u de gewenste configuratie van een apparaat en het configuratieproces van de update vervolgens een query.

<!-- links -->

[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-dm-overview]: ../articles/iot-hub/iot-hub-device-management-overview.md
[lnk-twin-tutorial]: ../articles/iot-hub/iot-hub-node-node-twin-getstarted.md
[lnk-guid]: https://en.wikipedia.org/wiki/Globally_unique_identifier
