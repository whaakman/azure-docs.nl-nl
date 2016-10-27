
# Azure en internet der dingen (IoT)

Welkom bij Microsoft Azure en het internet der dingen (IoT). In dit artikel maakt u kennis met een architectuur voor een IoT-oplossing waarin de algemene eigenschappen van een IoT-oplossing worden beschreven die u zou kunnen implementeren met Azure-services. IoT-oplossingen vereisen een beveiligde communicatie tussen mogelijk miljoenen apparaten in twee richtingen, evenals een back-end van de oplossing. Een back-end van de oplossing maakt bijvoorbeeld gebruik van geautomatiseerde predictive analytics om inzichten bloot te leggen over de gebeurtenisstroom van het apparaat naar de cloud.

Azure IoT Hub is een essentiële bouwsteen bij de implementatie van de architectuur voor deze IoT-oplossing met behulp van Azure-services. IoT Suite biedt volledige end-to-end-implementaties van deze architectuur voor specifieke IoT-scenario's. Bijvoorbeeld: 

- Met de *externe bewakingsoplossing* kunt u de status van apparaten, zoals verkoopautomaten, controleren. 
- De oplossing voor *voorspeld onderhoud* helpt u om te anticiperen op onderhoudsbehoeften van apparaten, zoals pompen in verafgelegen gemalen, en ongeplande uitvaltijd te voorkomen.

## Architectuur voor een IoT-oplossing

Het volgende diagram toont een typische architectuur voor een IoT-oplossing. Het diagram bevat de namen van de specifieke Azure-services niet, maar hierin worden wel de belangrijkste elementen in een algemene architectuur voor een IoT-oplossing beschreven. In deze architectuur verzamelen IoT-apparaten gegevens die ze naar een cloudgateway verzenden. De cloudgateway maakt de gegevens beschikbaar voor verwerking door andere back-endservices waarvan de gegevens bij andere Line-Of-Business-toepassingen of menselijke operators via een dashboard of een ander apparaat voor presentatie worden afgeleverd.

![Architectuur voor een IoT-oplossing][img-solution-architecture]

> [AZURE.NOTE] Zie [Microsoft Azure IoT Reference Architecture][lnk refarch] (Referentiearchitectuur voor Microsoft Azure IoT Ink-refarch) voor een uitgebreide beschrijving van de IoT-architectuur.

### Connectiviteit van apparaten

In deze architectuur voor een IoT-oplossing verzenden apparaten telemetriegegevens, zoals sensormetingen van een gemaal, naar een cloudeindpunt om daar te worden opgeslagen en verwerkt. In een scenario voor voorspeld onderhoud gebruikt de back-end de stroom van sensorgegevens mogelijk om vast te stellen of er aan een pomp onderhoud moet worden uitgevoerd. Apparaten kunnen ook opdrachten die van de cloud naar het apparaat zijn verstuurd, ontvangen en erop reageren door de berichten afkomstig van een cloudeindpunt te lezen. Zo kunnen er in een scenario voor voorspeld onderhoud door de back-end van de oplossing opdrachten worden verzonden naar andere pompen in een gemaal, zodat de waterstromen net voordat de onderhoudswerkzaamheden van start gaan, kunnen worden omgeleid en de onderhoudstechnicus meteen aan de slag kan wanneer hij aankomt.

Een van de grootste uitdagingen van IoT-projecten is het op stabiele en veilige wijze verbinden van apparaten met de back-end van een oplossing. Vergeleken met andere clients zoals browsers en mobiele apps hebben IoT-apparaten andere kenmerken. IoT-apparaten:

- Zijn vaak ingesloten systemen waarbij geen menselijke operator komt kijken.
- Kunnen worden geïmplementeerd op verafgelegen locaties, waar fysieke toegang kostbaar is.
- Kunnen mogelijk alleen worden bereikt via de back-end van de oplossing. Er is geen andere manier om te communiceren met het apparaat.
- Hebben mogelijk een beperkt vermogen en een beperkt aantal verwerkingsbronnen.
- Hebben mogelijk een onstabiele, trage of dure netwerkverbinding.
- Vereisen mogelijk het gebruik van bedrijfseigen, aangepaste of branchespecifieke toepassingsprotocollen.
- Kunnen worden gemaakt met behulp van een groot aantal populaire hardware- en softwareplatforms.

Naast de bovenstaande vereisten moet een IoT-oplossing schaalbaar, veilig en betrouwbaar zijn. De resulterende reeks connectiviteitsvereisten zijn moeilijk te implementeren en kost veel tijd als er gebruik wordt gemaakt van conventionele technologieën zoals webcontainers en berichtenbrokers. De SDK's van Azure IoT-Hub en IoT Device maken het gemakkelijker om oplossingen te implementeren die aan deze vereisten voldoen.

Een apparaat kan rechtstreeks communiceren met een eindpunt van een cloudgateway. Als het apparaat geen van de communicatieprotocollen kan gebruiken die door de cloudgateway worden ondersteund, kan het ook verbinding maken via een tussenliggende gateway. De [IoT Hub-protocolgateway][lnk-protocolgateway] kan protocollen vertalen, wanneer de apparaten geen van de protocollen kunnen gebruiken die door de IoT Hub worden ondersteund.

### Gegevensverwerking en -analyse

In de cloud vindt het grootste gedeelte van de gegevensverwerking plaats in de back-end van de IoT-oplossing, zoals het filteren en samenvoegen van telemetriegegevens, en deze naar andere services doorsturen. De back-end van de IoT-oplossing:

- Ontvangt van uw apparaten op schaal de telemetriegegevens en bepaalt hoe die gegevens moeten worden verwerkt en opgeslagen. 
- Kan het voor u mogelijk maken om opdrachten te verzenden van de cloud naar een specifiek apparaat.
- Biedt mogelijkheden voor het registreren van uw apparaten waardoor u apparaten kunt inrichten en kunt beheren welke apparaten er verbinding mogen maken met uw infrastructuur.
- Maakt het voor u mogelijk om de status van uw apparaten bij te houden en hun activiteiten te volgen.

In het scenario voor voorspeld onderhoud slaat de back-end van de oplossing historische telemetriegegevens op. De back-end kan deze gegevens gebruiken om patronen te identificeren die aangeven dat onderhoud van een specifieke pomp noodzakelijk is.

IoT-oplossingen kunnen automatische feedbacklussen omvatten. Zo kan een analysemodule in de back-end op grond van de telemetriegegevens bijvoorbeeld zien dat de temperatuur van een specifiek apparaat hoger is dan de normale bedrijfstemperatuurniveaus. De oplossing kan vervolgens een opdracht naar het apparaat verzenden met de instructie om corrigerende actie te ondernemen.

### Presentatie en bedrijfsconnectiviteit

De laag voor presentatie en bedrijfsconnectiviteit stelt eindgebruikers in staat te communiceren met de IoT-oplossing en de apparaten. Gebruikers kunnen met behulp hiervan de gegevens bekijken en analyseren die van hun apparaten zijn verzameld. Deze weergaven kunnen de vorm hebben van dashboards of BI-rapporten die zowel historische gegevens als gegevens in bijna realtime kunnen weergeven. Een operator kan bijvoorbeeld de status van een bepaald gemaal controleren en zien of het systeem eventueel waarschuwingen heeft gegeven. Deze laag maakt ook integratie mogelijk van de back-end van de IoT-oplossing met bestaande line-of-business-toepassingen zodat deze in bedrijfsprocessen of werkstromen kunnen worden ingevoegd. De oplossing voor voorspeld onderhoud kan bijvoorbeeld worden geïntegreerd met een werkroostersysteem dat op het moment dat de oplossing een pomp identificeert waaraan onderhoud moet worden uitgevoerd, een technicus boekt die naar het gemaal met de desbetreffende pomp moet gaan.

![Dashboard van de IoT-oplossing][img-dashboard]

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocolgateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf


<!--HONumber=Oct16_HO3-->


