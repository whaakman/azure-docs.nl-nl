> [!div class="op_single_selector"]
> * [Apparaat: Node.js-Service: Node.js](../articles/iot-hub/iot-hub-node-node-device-management-get-started.md)
> * [Apparaat: Node.js-Service: C#](../articles/iot-hub/iot-hub-csharp-node-device-management-get-started.md)
> * [Apparaat: C#-Service: C#](../articles/iot-hub/iot-hub-csharp-csharp-device-management-get-started.md)
> * [Apparaat: Java-Service: Java](../articles/iot-hub/iot-hub-java-java-device-management-getstarted.md)
> * [Apparaat: Python-Service: Python](../articles/iot-hub/iot-hub-python-python-device-management-get-started.md)

Back-end apps kunnen gebruiken Azure IoT Hub primitieven, zoals [apparaat twin] [ lnk-devtwin] en [methoden directe][lnk-c2dmethod], op afstand starten en bewaken van apparaat acties op apparaten. Deze zelfstudie laat zien hoe een back-end-app en een apparaat-app samenwerken kunnen om te starten en te bewaken van externe apparaten opnieuw worden opgestart met IoT Hub.

Een directe methode gebruiken om acties voor apparaat (zoals het opnieuw opstarten, Fabrieksinstellingen terugzetten en firmware-update) vanuit een back-end-app in de cloud. Het apparaat is verantwoordelijk voor:

* Verwerking van de methode-aanvraag die uit IoT Hub zijn verzonden.
* Bezig met starten van de overeenkomstige apparaatspecifieke actie op het apparaat.
* Van statusupdates via bieden *eigenschappen gerapporteerd* IoT-hub.

U kunt een back-endserver voor apps in de cloud apparaat twin query's om te rapporteren over de voortgang van uw apparaat beheeracties uitvoeren.

[lnk-devtwin]: ../articles/iot-hub/iot-hub-devguide-device-twins.md
[lnk-c2dmethod]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
