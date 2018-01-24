> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-twin-getstarted.md)
> * [C#/Node.js](../articles/iot-hub/iot-hub-csharp-node-twin-getstarted.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-twin-getstarted.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-twin-getstarted.md)
> * [Python](../articles/iot-hub/iot-hub-python-twin-getstarted.md)

Apparaatdubbels zijn JSON-documenten waarin statusinformatie van een apparaat (metagegevens, configuraties en voorwaarden) zijn opgeslagen. IoT Hub persistente een apparaat twin voor elk apparaat dat verbinding met het maakt.

Apparaat horende te gebruiken:

* Opslaan van metagegevens van apparaten uit de back-end van uw oplossing.
* Huidige statusinformatie zoals de beschikbare mogelijkheden en voorwaarden (bijvoorbeeld: de connectiviteit-methode gebruikt) van uw app apparaat rapporteren.
* De status van langlopende werkstromen (zoals updates, firmware en configuratie) tussen een apparaat-app en een back-endserver voor apps worden gesynchroniseerd.
* De metagegevens van apparaten, de configuratie of de status opvragen.

> [!NOTE]
> Apparaat horende zijn ontworpen voor synchronisatie en voor het uitvoeren van query's apparaatconfiguraties en voorwaarden. Meer informatie over het gebruik van apparaat horende vindt u in [apparaat horende begrijpen][lnk-twins].

Apparaat horende worden opgeslagen in een IoT-hub en bevatten:

* *labels*, de metagegevens van de apparaten is alleen toegankelijk is voor de back-end oplossing;
* *Eigenschappen gewenst*, JSON-objecten die kunnen worden gewijzigd door de oplossing back-end en opslagtijd met de apparaat-app en
* *Eigenschappen gerapporteerd*, JSON-objecten kunnen worden gewijzigd door de apparaat-app en gelezen door de back-end oplossing. Labels en eigenschappen kunnen geen matrices bevatten, maar objecten kunnen worden genest.

![][img-twin]

De back-end oplossing kan bovendien apparaat horende op basis van de bovenstaande gegevens opvragen.
Raadpleeg [horende apparaten begrijpen] [ lnk-twins] voor meer informatie over het apparaat horende de [querytaal IoT Hub] [ lnk-query] referentiemateriaal uitvoeren van query's.


In deze handleiding ontdekt u hoe u:

* Maakt een back-end-app die wordt toegevoegd *labels* een twin apparaat en een gesimuleerde apparaattoepassing die rapporteert het kanaal verbinding als een *gerapporteerd eigenschap* op het apparaat twin.
* Query uitvoeren op apparaten van uw back-end-app met behulp van filters op de labels en de eigenschappen die eerder hebt gemaakt.

<!-- images -->
[img-twin]: media/iot-hub-selector-twin-get-started/twin.png

<!-- links -->
[lnk-query]: ../articles/iot-hub/iot-hub-devguide-query-language.md
[lnk-twins]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-d2c]: ../articles/iot-hub/iot-hub-devguide-messaging.md#device-to-cloud-messages
[lnk-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md