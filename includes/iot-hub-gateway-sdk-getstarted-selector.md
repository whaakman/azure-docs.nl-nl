> [!div class="op_single_selector"]
> * [Linux](../articles/iot-hub/iot-hub-linux-gateway-sdk-get-started.md)
> * [Windows](../articles/iot-hub/iot-hub-windows-gateway-sdk-get-started.md)
> 
> 

Dit artikel bevat een gedetailleerd overzicht van de [Hello World-voorbeeldcode][lnk-helloworld-sample] om de fundamentele onderdelen van de architectuur van de [Azure IoT Gateway-SDK][lnk-gateway-sdk] te illustreren. In het voorbeeld wordt de Azure IoT Gateway SDK gebruikt om een eenvoudige gateway te maken die om de vijf seconden een 'Hello World'-bericht vastlegt in een bestand.

Dit overzicht omvat:

* **Concepten**: een conceptueel overzicht van de onderdelen waaruit een gateway bestaat die u met de IoT Gateway SDK maakt.  
* **Architectuur van het 'Hallo wereld’- voorbeeld**: hierin wordt beschreven hoe de concepten van toepassing zijn op het 'Hallo wereld'-voorbeeld en hoe de onderdelen in elkaar passen.
* **Het voorbeeld maken**: de stappen die nodig zijn om het voorbeeld te maken.
* **Het voorbeeld uitvoeren**: de stappen die nodig zijn om het voorbeeld uit te voeren. 
* **Typische uitvoer**: een voorbeeld van de uitvoer die u kunt verwachten wanneer u het voorbeeld uitvoert.
* **Codefragmenten**: een verzameling van codefragmenten om weer te geven hoe het 'Hallo wereld'-voorbeeld de belangrijkste gateway-onderdelen implementeert.

## <a name="azure-iot-gateway-sdk-concepts"></a>Azure IoT Gateway SDK-concepten
Voordat u de voorbeeldcode gaat bekijken of uw eigen gateway met de IoT Gateway SDK gaat maken, moet u de belangrijkste concepten kennen waarop de architectuur van de SDK is gebaseerd.

### <a name="modules"></a>Modules
U maakt een gateway met de Azure IoT gateway-SDK door *modules* te maken en aan elkaar te koppelen. Modules gebruiken *berichten* om gegevens met elkaar uit te wisselen. Een module ontvangt een bericht, voert daarmee een bepaalde actie uit, zet het eventueel om in een nieuw bericht en publiceert dit bericht vervolgens zodat het door andere modules kan worden verwerkt. Sommige modules kunnen alleen nieuwe berichten produceren en verwerken nooit binnenkomende berichten. Een keten van modules vormt een gegevensverwerkingspijplijn waarbij elke module een transformatie uitvoert van de gegevens op één punt in die pijplijn.

![Een keten van modules in gateway gebouwd met de Azure IoT Gateway SDK][1]

De SDK bevat het volgende:

* Vooraf geschreven modules die algemene gatewayfuncties uitvoeren.
* De interfaces die een ontwikkelaar kan gebruiken om aangepaste modules te schrijven.
* De infrastructuur die nodig is om een reeks modules te implementeren en uit te voeren.

De SDK biedt een laag van abstractie zodat u gateways kunt maken die uitvoerbaar zijn op verschillende besturingssystemen en platforms.

![Azure IoT Gateway SDK-abstractielaag][2]

### <a name="messages"></a>Berichten
Hoewel de idee van modules die berichten aan elkaar doorgeven een handige manier is om te visualiseren hoe een gateway werkt, wordt hiermee niet nauwkeurig weergegeven wat er precies gebeurt. Modules gebruiken een broker om met elkaar te communiceren, ze publiceren berichten naar de broker (bus, pubsub of een ander berichtpatroon) en laten de broker het bericht vervolgens verzenden naar de verbonden modules.

Een module gebruikt de functie **Broker_Publish** om een bericht naar de broker te publiceren. De broker levert berichten aan een module door een callbackfunctie te activeren. Een bericht bestaat uit een reeks sleutel/waarde-eigenschappen en inhoud die als een blok geheugen worden doorgegeven.

![De rol van de broker in de Azure IoT Gateway SDK][3]

### <a name="message-routing-and-filtering"></a>Berichtroutering en filteren
Er zijn twee manieren om berichten naar de juiste modules te sturen. Een set koppelingen kan worden doorgegeven aan de broker zodat de broker de bron en sink voor elke module kent, of de module kan filteren op de eigenschappen van het bericht. Een module mag alleen reageren op een bericht als het bericht is bedoeld voor die module. De koppelingen en het filteren van berichten resulteren in een berichtenpijplijn.

## <a name="hello-world-sample-architecture"></a>Architectuur van het 'Hallo wereld'-voorbeeld
Het 'Hallo wereld'-voorbeeld illustreert de concepten die in de vorige sectie zijn beschreven. In het 'Hallo wereld'-voorbeeld wordt een gateway geïmplementeerd die een pijplijn van twee modules heeft:

* De *Hallo wereld*-module maakt om de vijf seconden een bericht en geeft dit door aan de logboekregistratiemodule.
* De *logboekregistratie*module schrijft de ontvangen berichten naar een bestand.

![Architectuur van 'Hallo wereld'-voorbeeld gebouwd met de Azure IoT Gateway SDK][4]

Zoals in de vorige sectie is beschreven, geeft de module 'Hallo wereld' niet elke vijf seconden berichten rechtstreeks aan de logboekregistratiemodule door. In plaats daarvan wordt het bericht elke vijf seconden naar de broker gepubliceerd.

De loggermodule ontvangt het bericht van de broker en reageert erop door de inhoud van het bericht naar een bestand te schrijven.

De loggermodule verwerkt alleen berichten van de broker en publiceert nooit nieuwe berichten naar de broker.

![Hoe de broker berichten routeert tussen modules in de Azure IoT Gateway SDK][5]

In de bovenstaande afbeelding ziet u de architectuur van het 'Hello World'-voorbeeld en de relatieve paden naar de bronbestanden die verschillende delen van het voorbeeld implementeren in de [opslagplaats][lnk-gateway-sdk]. Verken zelf de code of gebruik de codefragmenten hieronder als richtlijn.

<!-- Images -->
[1]: media/iot-hub-gateway-sdk-getstarted-selector/modules.png
[2]: media/iot-hub-gateway-sdk-getstarted-selector/modules_2.png
[3]: media/iot-hub-gateway-sdk-getstarted-selector/messages_1.png
[4]: media/iot-hub-gateway-sdk-getstarted-selector/high_level_architecture.png
[5]: media/iot-hub-gateway-sdk-getstarted-selector/detailed_architecture.png

<!-- Links -->
[lnk-helloworld-sample]: https://github.com/Azure/azure-iot-gateway-sdk/tree/master/samples/hello_world
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

<!--HONumber=Dec16_HO1-->


