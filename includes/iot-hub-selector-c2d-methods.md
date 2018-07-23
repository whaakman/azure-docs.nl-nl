> [!div class="op_single_selector"]
> * [Node.js](../articles/iot-hub/quickstart-control-device-node.md)
> * [C#](../articles/iot-hub/quickstart-control-device-dotnet.md)
> * [Java](../articles/iot-hub/quickstart-control-device-java.md)
> * [Python](../articles/iot-hub/quickstart-control-device-python.md)

Azure IoT Hub is een volledig beheerde service waarmee betrouwbare en veilige tweerichtingscommunicatie tussen miljoenen apparaten en een oplossing voor back-end. Vorige zelfstudies ([Aan de slag met IoT Hub] en [Cloud-naar-apparaat-berichten verzenden met IoT Hub]) illustratie van de apparaat-naar-cloud- en cloud-naar-apparaat messaging basisfunctionaliteit van IoT-Hub. IoT Hub biedt u ook de mogelijkheid om niet-duurzame methoden aanroepen op apparaten vanuit de cloud. Directe methoden vertegenwoordigen een aanvraag / antwoord-interactie met een apparaat die vergelijkbaar is met een HTTPS-aanroep in dat ze slagen of onmiddellijk (na een door de gebruiker opgegeven time) mislukken zodat de gebruiker de status van de aanroep kennen. Het artikel [een rechtstreekse methode aanroepen op een apparaat] [ lnk-devguide-methods] worden directe methoden in meer detail beschreven en biedt richtlijnen over het gebruik van de directe methoden in plaats van de cloud-naar-apparaat-berichten of de gewenste eigenschappen.

[!INCLUDE [iot-hub-basic](iot-hub-basic-whole.md)]

In deze zelfstudie ontdekt u hoe u:

* De Azure portal gebruiken voor het maken van een IoT-hub en een apparaat-id maken in uw IoT-hub.
* Maak een gesimuleerde apparaat-app met een rechtstreekse methode die kan worden aangeroepen in de cloud.
* Maak een consoletoepassing die een rechtstreekse methode in de gesimuleerde apparaattoepassing via uw IoT-hub aanroepen.


[lnk-devguide-methods]: ../articles/iot-hub/iot-hub-devguide-direct-methods.md
[lnk-devguide-mqtt]: ../articles/iot-hub/iot-hub-mqtt-support.md

[Cloud-naar-apparaat-berichten verzenden met IoT Hub]: ../articles/iot-hub/iot-hub-csharp-csharp-c2d.md
[Aan de slag met IoT Hub]: ../articles/iot-hub/quickstart-send-telemetry-node.md