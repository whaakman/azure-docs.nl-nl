
# <a name="azure-and-the-internet-of-things"></a>Azure en Internet of Things

Welkom bij Microsoft Azure en het internet der dingen (IoT). In dit artikel worden de algemene eigenschappen van een IoT-oplossing in de cloud beschreven. IoT-oplossingen vereisen een beveiligde communicatie tussen mogelijk miljoenen apparaten in twee richtingen, evenals een back-end van de oplossing. Een oplossing kan bijvoorbeeld gebruikmaken van geautomatiseerde predictive analytics om inzichten bloot te leggen over de gebeurtenisstroom van het apparaat naar de cloud.

## <a name="iot-solution-architecture"></a>Architectuur voor een IoT-oplossing

Het volgende diagram toont de belangrijkste elementen van een typische architectuur voor een IoT-oplossing. Het diagram staat los van de specifieke implementatiegegevens, zoals de gebruikte Azure-services en de besturingssystemen van apparaten. In deze architectuur verzamelen IoT-apparaten gegevens die ze naar een cloudgateway verzenden. De cloudgateway maakt de gegevens beschikbaar voor verwerking door andere back-endservices. Deze back-end-services kunnen gegevens leveren aan:

* Andere line-of-business-toepassingen.
* Menselijke operators via een dashboard of een andere presentatiemethode.

![Architectuur voor een IoT-oplossing][img-solution-architecture]

> [!NOTE]
> Zie [Microsoft Azure IoT Reference Architecture][lnk-refarch] (Referentiearchitectuur voor Microsoft Azure IoT) voor een uitgebreide beschrijving van de IoT-architectuur.

### <a name="device-connectivity"></a>Connectiviteit van apparaten

In de architectuur van een IoT-oplossing verzenden apparaten doorgaans telemetriegegevens naar de cloud voor opslag en verwerking. In een scenario voor voorspeld onderhoud kan de back-end van de oplossing de stream van sensorgegevens bijvoorbeeld gebruiken om vast te stellen of er aan een pomp onderhoud moet worden uitgevoerd. Apparaten kunnen ook berichten ontvangen die van de cloud naar het apparaat zijn verstuurd en erop reageren door de berichten van een cloudeindpunt te lezen. Zo kunnen er hetzelfde voorbeeld door de back-end van de oplossing berichten worden verzonden naar andere pompen in een gemaal, zodat de waterstromen net voordat de onderhoudswerkzaamheden van start gaan, kunnen worden omgeleid. Deze procedure zorgt ervoor dat de onderhoudstechnicus meteen aan de slag kan wanneer hij aankomt.

De grootste uitdaging voor IoT-oplossingen is vaak om apparaten op een veilige en betrouwbare manier verbinding te laten maken. Vergeleken met andere clients zoals browsers en mobiele apps hebben IoT-apparaten namelijk andere kenmerken. IoT-apparaten:

* Zijn vaak ingesloten systemen waarbij geen menselijke operator komt kijken (in tegenstelling tot een telefoon).
* Kunnen worden geïmplementeerd op verafgelegen locaties, waar fysieke toegang kostbaar is.
* Kunnen mogelijk alleen worden bereikt via de back-end van de oplossing. Er is geen andere manier om te communiceren met het apparaat.
* Hebben mogelijk een beperkt vermogen en een beperkt aantal verwerkingsbronnen.
* Hebben mogelijk een onstabiele, trage of dure netwerkverbinding.
* Vereisen mogelijk het gebruik van bedrijfseigen, aangepaste of branchespecifieke toepassingsprotocollen.
* Kunnen worden gemaakt met behulp van een groot aantal populaire hardware- en softwareplatforms.

Een IoT-oplossing heeft niet alleen bovenstaande beperkingen, maar moet ook schaalbaar, veilig en betrouwbaar zijn.

Afhankelijk van de beschikbaarheid van het communicatieprotocol en het netwerk, kan een apparaat rechtstreeks of via een tussenliggende gateway communiceren met de cloud. IoT-architecturen gebruiken vaak een combinatie van deze twee communicatiepatronen.

### <a name="data-processing-and-analytics"></a>Gegevensverwerking en -analyse

In moderne IoT-oplossingen vindt de gegevensverwerking in de cloud of in het apparaat plaats. Verwerking in het apparaat wordt ook wel *Edge computing* genoemd. De keuze van de locatie voor gegevensverwerking is afhankelijk van factoren zoals:

* Netwerkbeperkingen. Als de bandbreedte tussen de apparaten en de cloud beperkt is, is er een stimulans om meer randbewerkingen uit te voeren.
* Reactietijd. Als het een vereiste is dat een apparaat bijna in realtime moet reageren, kan het beter zijn om de reactie in het apparaat zelf te verwerken. Bijvoorbeeld een robotarm die moet worden gestopt in geval van nood.
* Regelgeving. Sommige gegevens kunnen niet worden verzonden naar de cloud.

De randverwerking en gegevensverwerking in de cloud zijn in het algemeen een combinatie van de volgende mogelijkheden:

* Ontvangen van telemetriegegevens van uw apparaten en bepalen hoe die gegevens moeten worden verwerkt en opgeslagen.
* Analyse van de telemetriegegevens om inzichten te krijgen, ongeacht of deze in realtime of achteraf worden opgedaan.
* Verzenden van opdrachten vanuit de cloud of een gateway-apparaat met een bepaald apparaat.

Een IoT-back-end in de cloud moet daarnaast de volgende zaken bieden:

* Registratiemogelijkheden voor apparaten waarmee u de volgende zaken kunt doen:
    * Apparaten inrichten.
    * Bepalen welke apparaten verbinding mogen maken met uw infrastructuur.
* Apparaatbeheer om de status van uw apparaten bij te houden en hun activiteiten te volgen.

In een scenario voor voorspeld onderhoud slaat de back-end van de cloud bijvoorbeeld historische telemetriegegevens op. De oplossing gebruikt deze gegevens om mogelijk afwijkend gedrag van specifieke pompen te identificeren voordat er een echt probleem ontstaat. Met behulp van gegevensanalyses kan worden bepaald dat de preventieve oplossing een opdracht moet verzenden naar het apparaat om een corrigerende actie te ondernemen. Dit proces genereert een automatische feedbacklus tussen het apparaat en de cloud die de efficiëntie van de oplossing aanzienlijk verhoogt.

### <a name="presentation-and-business-connectivity"></a>Presentatie en bedrijfsconnectiviteit

De laag voor presentatie en bedrijfsconnectiviteit stelt eindgebruikers in staat te communiceren met de IoT-oplossing en de apparaten. Gebruikers kunnen met behulp hiervan de gegevens bekijken en analyseren die van hun apparaten zijn verzameld. Deze weergaven kunnen de vorm hebben van dashboards of BI-rapporten die zowel historische gegevens als gegevens in bijna realtime kunnen weergeven. Een operator kan bijvoorbeeld de status van een bepaald gemaal controleren en zien of het systeem eventueel waarschuwingen heeft gegeven. Deze laag maakt ook integratie mogelijk van de back-end van de IoT-oplossing met bestaande line-of-business-toepassingen zodat deze in bedrijfsprocessen of werkstromen kunnen worden ingevoegd. Een oplossing voor voorspeld onderhoud kan bijvoorbeeld worden geïntegreerd met een werkroostersysteem dat op het moment dat de oplossing een pomp identificeert waaraan onderhoud moet worden uitgevoerd, een technicus boekt die naar het gemaal met de desbetreffende pomp moet gaan.

[img-solution-architecture]: ./media/iot-azure-and-iot/iot-reference-architecture.png
[img-dashboard]: ./media/iot-azure-and-iot/iot-suite.png

[lnk-iot-hub]: ../articles/iot-hub/iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: ../articles/iot-suite/iot-suite-overview.md
[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[Azure IoT Suite]: http://azure.microsoft.com/solutions/iot
[lnk-protocol-gateway]:  ../articles/iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
