
# <a name="azure-and-internet-of-things"></a>Azure en internet der dingen (IoT)

Welkom bij Microsoft Azure en het internet der dingen (IoT). In dit artikel maakt u kennis met de algemene kenmerken van een IoT-oplossing die u kunt implementeren met Azure-services. IoT-oplossingen vereisen een beveiligde communicatie tussen vele apparaten in twee richtingen, evenals een back-end voor de oplossing. De back-end voor de oplossing kan gebruikmaken van geautomatiseerde predictive analytics om inzichten bloot te leggen over de gebeurtenisstroom van het apparaat naar de cloud.

[Azure IoT Hub][lnk-iot-hub] is een essentiële bouwsteen van elke IoT-oplossing die gebruikmaakt van Azure-services. IoT Hub is een volledig beheerde service die stabiele en veilige tweerichtingscommunicatie tussen miljoenen IoT-apparaten en de back-end van een oplossing mogelijk maakt. 

[Azure IoT Suite][lnk-iot-suite] biedt volledige end-to-end-implementaties van deze architectuur voor specifieke IoT-scenario's. Bijvoorbeeld:

* Met de *externe bewakingsoplossing* kunt u de status van apparaten, zoals verkoopautomaten, controleren.
* De oplossing voor *predictief onderhoud* helpt u om te anticiperen op onderhoudsbehoeften van apparaten, zoals pompen in verafgelegen gemalen, en om ongeplande uitvaltijd te voorkomen.
* De *verbonden vooraf gedefinieerde* oplossing helpt u om verbinding te maken met uw industriële apparaten en om deze te bewaken.

## <a name="iot-solution-architecture"></a>Architectuur voor een IoT-oplossing

Het volgende diagram toont een typische architectuur voor een IoT-oplossing. Het diagram bevat de namen van de specifieke Azure-services niet, maar hierin worden wel de belangrijkste elementen in een algemene architectuur voor een IoT-oplossing beschreven. In deze architectuur verzamelen IoT-apparaten gegevens die ze naar een cloudgateway verzenden. De cloudgateway maakt de gegevens beschikbaar voor verwerking door andere back-endservices. De back-end van de oplossing levert gegevens via een dashboard of rapport aan line-of-business-toepassingen of menselijke operators.

![Architectuur voor een IoT-oplossing][img-solution-architecture]

> [!NOTE]
> Zie [Microsoft Azure IoT Reference Architecture][lnk-refarch] (Referentiearchitectuur voor Microsoft Azure IoT) voor een uitgebreide beschrijving van de IoT-architectuur.

### <a name="device-connectivity"></a>Connectiviteit van apparaten

In deze IoT-oplossing verzenden apparaten telemetriegegevens, zoals sensormetingen van een gemaal, naar een cloudeindpunt om daar te worden opgeslagen en verwerkt. In een scenario voor voorspeld onderhoud kan de back-end van de oplossing de stream van sensorgegevens gebruiken om vast te stellen of er aan een pomp onderhoud moet worden uitgevoerd. Apparaten kunnen ook berichten ontvangen die van de cloud naar het apparaat zijn verstuurd en erop reageren door de berichten van een cloudeindpunt te lezen. Zo kunnen er in een scenario voor voorspeld onderhoud door de back-end van de oplossing berichten worden verzonden naar andere pompen in een gemaal, zodat de waterstromen net voordat de onderhoudswerkzaamheden van start gaan, kunnen worden omgeleid. Deze procedure moet ervoor zorgen dat de onderhoudstechnicus meteen aan de slag kan om het probleem op te lossen.

Een van de grootste uitdagingen van IoT-projecten is het op stabiele en veilige wijze verbinden van apparaten met de back-end van een oplossing. Vergeleken met andere clients zoals browsers en mobiele apps hebben IoT-apparaten andere kenmerken. IoT-apparaten:

* Zijn vaak ingesloten systemen waarbij geen menselijke operator komt kijken.
* Kunnen worden geïmplementeerd op verafgelegen locaties, waar fysieke toegang kostbaar is.
* Kunnen mogelijk alleen worden bereikt via de back-end van de oplossing. Er is geen andere manier om te communiceren met het apparaat.
* Hebben mogelijk een beperkt vermogen en een beperkt aantal verwerkingsbronnen.
* Hebben mogelijk een onstabiele, trage of dure netwerkverbinding.
* Vereisen mogelijk het gebruik van bedrijfseigen, aangepaste of branchespecifieke toepassingsprotocollen.
* Kunnen worden gemaakt met behulp van een groot aantal populaire hardware- en softwareplatforms.

Naast de bovenstaande vereisten moet een IoT-oplossing schaalbaar, veilig en betrouwbaar zijn. De resulterende reeks connectiviteitsvereisten zijn moeilijk te implementeren en kost veel tijd als er gebruik wordt gemaakt van conventionele technologieën zoals webcontainers en berichtenbrokers. Met de SDK's van Azure IoT-Hub en Azure IoT Device kunnen oplossingen die aan deze vereisten voldoen, gemakkelijker worden geïmplementeerd.

Een apparaat kan rechtstreeks met het eindpunt van een cloudgateway communiceren. Als het apparaat geen van de communicatieprotocollen kan gebruiken die door de cloudgateway worden ondersteund, kan het ook verbinding maken via een tussenliggende gateway. De [Azure IoT Hub-protocolgateway][lnk-protocol-gateway] kan protocollen vertalen wanneer de protocollen die in de IoT Hub worden ondersteund, niet kunnen worden gebruikt voor de apparaten.

### <a name="data-processing-and-analytics"></a>Gegevensverwerking en -analyse

In de cloud is het de back-end van de IoT-oplossing waarin de meeste gegevensverwerking plaatsvindt. De back-end van de IoT-oplossing:

* Ontvangt van uw apparaten op schaal de telemetriegegevens en bepaalt hoe die gegevens moeten worden verwerkt en opgeslagen. 
* Kan het voor u mogelijk maken om opdrachten te verzenden van de cloud naar specifieke apparaten.
* Biedt mogelijkheden voor het registreren van uw apparaten, zodat u apparaten kunt inrichten en kunt beheren welke apparaten er verbinding mogen maken met uw infrastructuur.
* Maakt het voor u mogelijk om de status van uw apparaten bij te houden en hun activiteiten te volgen.

In het scenario voor voorspeld onderhoud slaat de back-end van de oplossing historische telemetriegegevens op. De back-end van de oplossing kan deze gegevens gebruiken om patronen te identificeren die aangeven dat onderhoud van een specifieke pomp noodzakelijk is.

IoT-oplossingen kunnen automatische feedbacklussen omvatten. Zo kan een analysemodule in de back-end van de oplossing op grond van de telemetriegegevens bijvoorbeeld zien dat de temperatuur van een specifiek apparaat hoger is dan de normale bedrijfstemperatuurniveaus. De oplossing kan vervolgens een opdracht naar het apparaat verzenden met de instructie om corrigerende actie te ondernemen.

### <a name="presentation-and-business-connectivity"></a>Presentatie en bedrijfsconnectiviteit

De laag voor presentatie en bedrijfsconnectiviteit stelt eindgebruikers in staat te communiceren met de IoT-oplossing en de apparaten. Gebruikers kunnen met behulp hiervan de gegevens bekijken en analyseren die van hun apparaten zijn verzameld. Deze weergaven kunnen de vorm hebben van dashboards of rapporten die zowel historische gegevens als gegevens in bijna realtime kunnen weergeven. Een operator kan bijvoorbeeld de status van een bepaald gemaal controleren en zien of het systeem eventueel waarschuwingen heeft gegeven. Deze laag maakt ook integratie mogelijk van de back-end van de IoT-oplossing met bestaande line-of-business-toepassingen zodat deze in bedrijfsprocessen of werkstromen kunnen worden ingevoegd. De oplossing voor predictief onderhoud kan bijvoorbeeld worden geïntegreerd in een planningssysteem. Als door de oplossing een pomp wordt geïdentificeerd waaraan onderhoud moet worden gepleegd, wordt door het planningssysteem een technicus ingeboekt die het gemaal moet bezoeken.

![Dashboard van de IoT-oplossing][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-iot-hub]: ../articles/iot-hub/iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: ../articles/iot-suite/iot-suite-overview.md
[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
