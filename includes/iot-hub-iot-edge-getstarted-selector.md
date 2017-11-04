> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-iot-edge-get-started.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-iot-edge-get-started.md)
> 
> 

Dit artikel bevat een gedetailleerd overzicht van de [Hallo wereld-voorbeeldcode][lnk-helloworld-sample] om de fundamentele onderdelen van de architectuur van [Azure IoT Edge][lnk-iot-edge] te illustreren. In het voorbeeld wordt Azure IoT Edge gebruikt om een eenvoudige gateway te maken die om de vijf seconden een 'Hallo wereld'-bericht vastlegt in een bestand.

Dit overzicht omvat:

* **Hallo wereld voorbeeldarchitectuur**: hierin wordt beschreven hoe [Azure IoT rand architectuur concepten] [ lnk-edge-concepts] van toepassing op het Hallo wereld-voorbeeld en hoe de onderdelen in elkaar passen.
* **Het voorbeeld maken**: de stappen die nodig zijn om het voorbeeld te maken.
* **Het voorbeeld uitvoeren**: de stappen die nodig zijn om het voorbeeld uit te voeren. 
* **Typische uitvoer**: een voorbeeld van de uitvoer die u kunt verwachten wanneer u het voorbeeld uitvoert.
* **Codefragmenten**: een verzameling van codefragmenten om weer te geven hoe het Hallo wereld-voorbeeld belangrijke onderdelen van de rand van de IoT gateway implementeert.


## <a name="hello-world-sample-architecture"></a>Architectuur van het 'Hallo wereld'-voorbeeld
Het 'Hallo wereld'-voorbeeld illustreert de concepten die in de vorige sectie zijn beschreven. Het Hallo wereld-voorbeeld implementeert een IoT-Edge-gateway met een pijplijn die bestaat uit twee IoT Edge-modules:

* De *Hallo wereld*-module maakt om de vijf seconden een bericht en geeft dit door aan de logboekregistratiemodule.
* De *logboekregistratie*module schrijft de ontvangen berichten naar een bestand.

![Architectuur van 'Hallo wereld'-voorbeeld gebouwd met Azure IoT Edge][4]

Zoals in de vorige sectie is beschreven, geeft de module 'Hallo wereld' niet elke vijf seconden berichten rechtstreeks aan de logboekregistratiemodule door. In plaats daarvan wordt het bericht elke vijf seconden naar de broker gepubliceerd.

De loggermodule ontvangt het bericht van de broker en reageert erop door de inhoud van het bericht naar een bestand te schrijven.

De loggermodule verwerkt alleen berichten van de broker en publiceert nooit nieuwe berichten naar de broker.

![Hoe de broker berichten routeert tussen modules in Azure IoT Edge][5]

De voorgaande afbeelding toont de architectuur van het Hallo wereld-voorbeeld en de relatieve paden naar de bronbestanden die implementeren van verschillende delen van het voorbeeld in de [opslagplaats][lnk-iot-edge]. Verken de code in uw eigen of gebruik de codefragmenten in dit artikel als richtlijn.

<!-- Images -->
[4]: media/iot-hub-iot-edge-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-iot-edge-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/iot-edge/tree/master/samples/hello_world
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-edge-concepts]: ../articles/iot-hub/iot-hub-iot-edge-overview.md