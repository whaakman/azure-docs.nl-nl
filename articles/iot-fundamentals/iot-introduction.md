---
title: Inleiding tot Azure Internet of Things (IoT)
description: Overzicht van Azure IoT en verwante services en technologieën.
author: BryanLa
manager: timlt
ms.service: iot-fundamentals
services: iot-fundamentals
ms.topic: overview
ms.date: 05/18/2018
ms.author: bryanla
ms.openlocfilehash: 685de6a3cfe0e3f5a2ea82da831247d7b4144fea
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34636202"
---
# <a name="introduction-to-azure-and-the-internet-of-things"></a>Inleiding tot Azure en Internet of Things

Azure IoT bestaat uit drie gebieden technologieën en oplossingen: oplossingen, platformservices en edge. Deze zijn ontworpen om de end-to-end-ontwikkeling van uw IoT-toepassing mogelijk te maken. Dit artikel begint met een beschrijving van de algemene eigenschappen van een IoT-oplossing in de cloud, gevolgd door een overzicht van hoe Azure IoT uitdagingen bij IoT-projecten aanpakt en waarom u zou moeten overwegen Azure IoT te gebruiken.

## <a name="iot-solution-architecture"></a>Architectuur voor een IoT-oplossing

IoT-oplossingen vereisen een beveiligde communicatie tussen mogelijk miljoenen apparaten in twee richtingen, evenals een back-end van de oplossing. Een oplossing kan bijvoorbeeld gebruikmaken van geautomatiseerde predictive analytics om inzichten bloot te leggen over de gebeurtenisstroom van het apparaat naar de cloud. 

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

## <a name="why-azure-iot"></a>Waarom Azure IoT?

Azure IoT vereenvoudigt de complexiteit van IoT-projecten en pakt de uitdagingen aan, zoals beveiliging, incompatibiliteit met de infrastructuur en het schalen van uw IoT-oplossing. Dit gaat als volgt:

**Flexibel** <br>
Uw IoT-traject versnellen
* Schaal: begin klein, groei tot elke omvang, overal en altijd - miljoenen apparaten, terabytes aan gegevens, in de meeste regio's wereldwijd.

* Open: gebruik wat u hebt, of moderniseer voor de toekomst, door verbinding te maken met een apparaat, software of service.

* Hybride: bouw op basis van uw behoeften door uw IoT-oplossing te implementeren in de rand, in de cloud of ergens daartussen.

* Tempo: implementeer sneller, versnel de introductietijd en blijf uw concurrentie een stap voor met de leider op het gebied van oplossingsverbeteringen en vernieuwingstempo in IoT.

**Uitgebreid** <br>
Impact voor uw bedrijf bereiken

* Volledig: Microsoft is de enige provider van IoT-oplossingen met een volledig platform dat apparaat naar cloud overspant, over big data, geavanceerde analyses en met beheerde services.

* Partner voor succes: maak gebruik van de kracht van 's werelds grootste ecosysteem voor partners en wek line-of-business en technologie tot leven, binnen de branche en over de hele wereld.

* Gegevensgestuurd: IoT gaat over gegevens en de beste IoT-oplossingen brengen alle tools samen die u nodig hebt om gegevens te bewaren, interpreteren, transformeren, analyseren en presenteren aan de juiste gebruiker, op de juiste plaats, op het juiste moment.

* Apparaatgericht: met Microsoft IoT kunt u alles met elkaar verbinden, van verouderde apparatuur tot een enorm ecosysteem van gecertificeerde hardware, en hebt u de mogelijkheid om uw eigen apparaten te bouwen op Edge en op mobiele en ingesloten systemen.

**Beveiligen** <br>
Het lastigste onderdeel van IoT oplossen: beveiliging

* Meer mogelijkheden: met Microsoft IoT kunt u uw visie samenbrengen met de technologie, aanbevolen procedures en de mogelijkheden om het lastigste onderdeel van IoT op te lossen, namelijk beveiliging.

* Actie ondernemen: beveilig uw IoT-gegevens en beheer risico's met identiteits-en toegangsbeheer, bedreigings- en informatiebescherming en beveiligingsbeheer.

* Gemoedsrust: garandeer de veiligheid van gevoelige informatie op alle apparaten, software, toepassingen en cloudservices, evenals in lokale omgevingen.

* Naleving: Microsoft leidt de branche bij het vaststellen van de beveiligingsvereisten die voldoen aan een uitgebreide reeks internationale en branchespecifieke standaarden voor IoT-apparaten, gegevens en services.

## <a name="next-steps"></a>Volgende stappen

Bestudeer de volgende gebieden van technologieën en oplossingen of raadpleeg de inhoudsopgave links voor een overzicht van Azure IoT-services.

<ul class="panelContent cardsF">  
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
                        <h3>Oplossingen</h3>
                        <a href="/azure/iot-suite">IoT-oplossingsversnellers</a><br/>
                        <a href="/azure/iot-central">IoT Central</a>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
                        <h3>Platformservices</h3>
                        <a href="/azure/iot-hub">IoT Hub</a><br/>
                        <a href="/azure/iot-dps">IoT Hub Device Provisioning Service</a><br/>
                        <a href="/azure/azure-maps">Kaarten</a><br/>
                        <a href="/azure/time-series-insights">Time Series Insights</a>
                    </div>
                </div>
            </div>
        </div>
    </li>  
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
                        <h3>Edge</h3>
                        <a href="/azure/iot-edge">IoT Edge</a><br/>
                        <a href="/azure/iot-edge/how-iot-edge-works">Wat is IoT Edge?</a>
                    </div>
                </div>
            </div>
        </div>
    </li>      
</ul>

[img-paas-saas-technologies-solutions]: media/index/paas-saas-technologies-solutions.png
[img-solution-architecture]: ./media/iot-introduction/iot-reference-architecture.png
[img-dashboard]: ./media/iot-introduction/iot-suite.png

[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-iot-central-land]: https://docs.microsoft.com/microsoft-iot-central/
[lnk-iot-dps-land]: /azure/iot-dps/index.yml
[lnk-iot-edge-land]: /azure/iot-edge/index.yml
[lnk-iot-hub-land]: /azure/iot-hub/index.md
[lnk-iot-maps-land]: /azure/maps/index.yml
[lnk-iot-sa-land]: ../iot-accelerators/index.md
[lnk-iot-tsi-land]: /azure/time-series-insights/index.yml

[lnk-iot-hub]: ../iot-hub/about-iot-hub.md
[lnk-iot-sa]: ../iot-accelerators/iot-accelerators-what-are-solution-accelerators.md
[lnk-machinelearning]: http://azure.microsoft.com/documentation/services/machine-learning/
[lnk-protocol-gateway]:  ../iot-hub/iot-hub-protocol-gateway.md
[lnk-refarch]: https://aka.ms/iotrefarchitecture


