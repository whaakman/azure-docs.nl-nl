> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/iot-hub-node-node-direct-methods.md)
> * [C#/Node.js](../articles/iot-hub/iot-hub-csharp-node-direct-methods.md)
> * [C#](../articles/iot-hub/iot-hub-csharp-csharp-direct-methods.md)
> * [Java](../articles/iot-hub/iot-hub-java-java-direct-methods.md)
> * [Python](../articles/iot-hub/iot-hub-python-python-direct-methods.md)

Azure IoT Hub is een volledig beheerde service waarmee betrouwbare en veilige tweerichtingscommunicatie tussen miljoenen apparaten en een oplossing voor back-end. Vorige zelfstudies ([aan de slag met IoT Hub] en [Cloud naar apparaat verzenden met IoT Hub]) illustratie van de apparaat-naar-cloud- en cloud-naar-apparaat messaging basisfunctionaliteit van IoT-Hub. IoT Hub biedt u ook de mogelijkheid om aan te roepen niet duurzame methoden op apparaten vanuit de cloud. Rechtstreekse methoden vertegenwoordigen een request-reply interactie met een apparaat dat vergelijkbaar is met een HTTPS-aanroep in dat ze slagen of onmiddellijk (na een door de gebruiker opgegeven time), mislukken zodat de gebruiker die de status van de aanroep kennen. [Een directe methode aangeroepen voor een apparaat] [ lnk-devguide-methods] worden direct methoden in meer detail beschreven en biedt hulp bij het gebruik van directe methoden in plaats van cloud naar apparaat-berichten of gewenste eigenschappen.

In deze handleiding ontdekt u hoe u:

* De Azure portal gebruiken voor het maken van een IoT-hub en een apparaat-id maakt in uw IoT-hub.
* Maak een gesimuleerde apparaattoepassing met een directe methode die kan worden aangeroepen door de cloud.
* Een consoletoepassing maken die een directe methode wordt aangeroepen in de gesimuleerde apparaattoepassing via uw IoT-hub.


[lnk-devguide-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md

[Cloud naar apparaat verzenden met IoT Hub]: ../articles/iot-hub/iot-hub-csharp-csharp-c2d.md
[aan de slag met IoT Hub]: ../articles/iot-hub/iot-hub-node-node-getstarted.md