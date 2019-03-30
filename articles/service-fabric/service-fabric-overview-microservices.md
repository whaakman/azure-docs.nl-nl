---
title: Inleiding tot microservices in Azure | Microsoft Docs
description: Een overzicht van waarom het bouwen van cloudtoepassingen met een microservices-benadering is belangrijk voor het ontwikkelen van moderne toepassing en hoe Azure Service Fabric biedt een platform om dit te bereiken.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: fae2be85-0ab4-4cd3-9d1f-e0d95fe1959b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: atsenthi
ms.openlocfilehash: 1e02e4fdf0dbe04d8756fc6355c6a9e414b27d2b
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58665591"
---
# <a name="why-a-microservices-approach-to-building-applications"></a>Waarom een microservices-benadering voor het bouwen van toepassingen?

Software-ontwikkelaars is er niets nieuws in de manier waarop we denken over het kiezen van een toepassing in onderdelen. Een gelaagde benadering wordt meestal gebruikt met een back-end-store, bedrijfslogica middelste laag en een front-end-gebruikersinterface (UI). Wat *heeft* gewijzigd in de afgelopen jaren is dat we ontwikkelaars, gedistribueerde toepassingen bouwen voor de cloud.

De wisselende bedrijfsbehoeften van uw zijn:

* Een service die ingebouwd en werkt op grote schaal om klanten in nieuwe geografische regio's te bereiken.
* Snellere levering van functies en mogelijkheden om te kunnen reageren op verzoeken van klanten op flexibele wijze inzetten.
* Verbeterde Resourcegebruik om kosten te verlagen.

Deze behoeften van uw bedrijf van invloed zijn op *hoe* we bij het bouwen van toepassingen.

Lees voor meer informatie over de aanpak van Azure om microservices [Microservices: Een toepassing revolution mogelijk gemaakt door de cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservice-design-approach"></a>Monolithische versus microservice-benadering voor ontwerp

Toepassingen loop der tijd veranderen. Succesvolle toepassingen veranderen door handig voor gebruikers. Mislukte toepassingen niet doen ontwikkelen en uiteindelijk zijn afgeschaft. De vraag is: Hoeveel wist u dat u over de vereisten voor vandaag en wat ze worden in de toekomst? Stel dat het bouwen van een reporting-toepassing voor een afdeling. U bent of de toepassing is alleen van toepassing binnen het bereik van uw bedrijf en of de rapporten tijdelijke zijn. Uw keuze van benadering wijkt af van, zeg, video-inhoud bouwen van een service die voorziet in tientallen miljoenen klanten.

Soms iets uit de deur krijgen als bewijs van concept is de drijvende factor, terwijl u weet dat de toepassing later kan worden ontworpen. Er is weinig punt in iets te veel engineering die nooit wordt gebruikt. Het is de gebruikelijke engineering verhouding. Wanneer bedrijven over het bouwen van voor de cloud praten, is de verwachting dat aan de andere kant groei en het gebruik. Het probleem is dat groei en de schaal onvoorspelbaar zijn. Willen we snel prototypen kunnen worden terwijl u ook weten dat er zich op een pad bevinden naar het omgaan met succes in de toekomst. Dit is de methode lean opstarten: bouwen, meten, leren en herhalen.

Tijdens het tijdperk client / server-uitbreiden we richten op het gelaagde toepassingen bouwen met behulp van specifieke technologieën in elke laag. De term *monolithische* toepassing geworden voor deze methoden. De interfaces uitbreiden moet tussen de lagen en ontwerp voor een meer nauw gekoppelde tussen onderdelen binnen elke laag is gebruikt. Ontwikkelaars die zijn ontworpen en gewogen klassen die zijn gecompileerd in bibliotheken en aan elkaar gekoppeld in een paar uitvoerbare bestanden en .dll-bestanden. 

Er zijn voordelen van deze benadering van een monolithisch ontwerp. Het is vaak eenvoudiger te ontwerpen en er sneller aanroepen tussen onderdelen omdat deze aanroepen vaak over interprocess communication (IPC zijn). Bovendien iedereen één enkel product, doorgaans meer mensen-resource-efficiënt worden getest. Het nadeel is dat er een hechte koppeling tussen de lagen van gelaagde, en u afzonderlijke componenten niet schalen. Als u nodig hebt om uit te voeren voor problemen of upgrades worden uitgevoerd, moet u wachten op anderen tests voltooid. Het is moeilijker flexibel zijn.

Microservices adres deze aantal nadelen en meer nauw zijn afgestemd op de voorgaande zakelijke vereisten, maar ze hebben ook zowel de voordelen en de verplichtingen. De voordelen van microservices zijn dat elke doorgaans ingekapseld eenvoudiger bedrijfsfunctionaliteit, die u kunt omhoog of omlaag schalen, testen, implementeren en afzonderlijk beheren. Een belangrijk voordeel van een microservice-benadering is dat teams meer bedrijfsscenario's dan worden aangedreven door technologie, waardoor de gelaagde benadering. In de praktijk kleinere teams ontwikkelen van een microservice op basis van een klant scenario en ze ervoor kiezen eventuele-technologieën gebruiken.

Met andere woorden, hoeft de organisatie te standaardiseren tech om bij te houden van microservice-toepassingen. Afzonderlijke teams die eigen services doen kunnen wat zinvol voor hen op basis van team kennis of wat is het meest geschikt is om het probleem te verhelpen. In de praktijk moet een set van aanbevolen technologieën, zoals een bepaalde NoSQL opslaan of framework voor webtoepassingen, is het beter.

Het nadeel van microservices wordt geleverd bij het beheren van het grotere aantal afzonderlijke entiteiten en omgaan met complexere implementaties en versiebeheer. Netwerkverkeer tussen de microservices wordt verhoogd en de bijbehorende netwerklatenties. Veel van intensieve, gespecialiseerde services zijn een recept voor een nachtmerrie prestaties. Zonder hulpprogramma's weergeven waarmee deze afhankelijkheden, het is moeilijk te 'Zie"het hele systeem.

Standaarden maken de microservice-benadering werk door als u akkoord gaat over het communiceren en gevoelig voor alleen de dingen die u een service moet wordt in plaats van Star contracten. Het is belangrijk dat deze opdrachten een definiëren in het ontwerp, omdat services onafhankelijk van elkaar werken. Een andere beschrijving lang geleden bedacht voor het ontwerpen met een microservices-benadering is "fijnmazig service oriented architecture (SOA)."

***De eenvoudigste, wordt de microservices ontwerpen-aanpak is over een losgekoppelde federation Services met onafhankelijke wijzigingen aan elk en overeengekomen standaarden voor communicatie.***

Omdat er meer cloud-apps zijn gemaakt, hebben mensen gedetecteerd dat deze ontleding van de algehele app in onafhankelijke, zeer sterk gericht op scenario services een beter op de lange termijn benadering is.

## <a name="comparison-between-application-development-approaches"></a>Vergelijking van de ontwikkeling van toepassingen nadert

![Toepassingsontwikkeling voor service Fabric-platform][Image1]

1) Een monolithische app domeinspecifieke functionaliteit bevat en normaal gesproken wordt gedeeld door functionele lagen, zoals web-, bedrijfs- en gegevens.

2) U een monolithische app schalen door deze te klonen op meerdere servers/virtuele machines/containers.

3) Een microservice-toepassingen gescheiden functionaliteit in afzonderlijke, kleinere services.

4) De microservices-benadering schalen uit door het implementeren van elke service onafhankelijk van elkaar, exemplaren van deze services te maken over servers/virtuele machines/containers.

Het ontwerpen van een microservice benadering is niet een wondermiddel voor alle projecten, maar deze beter aansluiten uitgelijnd met de zakelijke doelstellingen die eerder zijn beschreven. Beginnen met een monolithische aanpak mogelijk acceptabel als u weet dat u de mogelijkheid hebt om de code later bijwerken naar een microservice-ontwerp. Meer over het algemeen u beginnen met een monolithische toepassing en langzaam splits deze in fasen, beginnen met het functionele aspecten die moeten worden schaalbare of meer flexibele.

De microservice-benadering betekent dat u uw toepassing van veel kleine services samenstellen. De services in containers die zijn geïmplementeerd in een computercluster worden uitgevoerd. Kleinere teams ontwikkelen van een service die is gericht op een scenario en onafhankelijk van elkaar, versie testen, implementeren en schalen van elke service, zodat de gehele toepassing kunt ontwikkelen.

## <a name="what-is-a-microservice"></a>Wat is een microservice?

Er zijn verschillende definities van microservices. Echter, de meeste van de volgende kenmerken van microservices zijn veel overeengekomen:

* Een klant of scenario bevatten. Wat is het probleem dat u het oplossen van?
* Ontwikkeld door een kleine IT-team.
* Geschreven in elke programmeertaal en elk gewenst framework gebruiken.
* Bestaan uit code en (optioneel) staat, die beide zijn onafhankelijke versies, geïmplementeerd en geschaald.
* Communiceren met andere microservices via goed gedefinieerde interfaces en protocollen.
* Een unieke naam (URL's) gebruikt voor het omzetten van hun locatie hebben.
* Consistente en geval van problemen beschikbaar blijven.

Samenvatting:

***Microservice-toepassingen worden samengesteld uit kleine, onafhankelijke versies en schaalbare klantgerichte services die met elkaar via standaardprotocollen met goed gedefinieerde interfaces communiceren.***

### <a name="written-in-any-programming-language-and-use-any-framework"></a>Geschreven in elke programmeertaal en elk gewenst framework gebruiken

Ontwikkelaars willen we kiezen een andere taal of framework dat we, afhankelijk van onze vaardigheden of de behoeften van de service willen. In sommige services, kunt u de prestatievoordelen van C++ boven alle andere waarde. In andere services, kan het gemak van beheerde ontwikkeling in C# of Java belangrijkste zijn. In sommige gevallen moet u mogelijk gebruikmaken van een specifieke partner-bibliotheek, technologie voor gegevensopslag of betekent dat de service aan clients.

Nadat u hebt ervoor gekozen een technologie, keert u naar de operationele of lifecycle management en schalen van de service.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Kan code en de status onafhankelijke versies, geïmplementeerd en geschaald

U wilt echter uw microservices, schrijft de code en (optioneel) de status moeten onafhankelijk van elkaar implementeren, bijwerken en schalen. Dit is een van de problemen moeilijker op te lossen omdat het gaat naar beneden om uw eigen keuze aan technologieën. Voor vergroten/verkleinen, inzicht in hoe u partitie (of shard) de code en de status uitdagingen met zich mee. Wanneer de code en de status van afzonderlijke technologieën, dit komt veel voor vandaag, moeten de implementatiescripts voor uw microservices worden ze allebei schalen. Dit is ook over de wendbaarheid en flexibiliteit, zodat u enkele van de microservices bijwerken kunt zonder dat ze allemaal in één keer te upgraden.

Terugkeren naar de monolithische ten opzichte van microservice-benadering voor even de tijd, toont het volgende diagram de verschillen in de aanpak voor het opslaan van status.

#### <a name="state-storage-between-application-styles"></a>Status opslag tussen toepassing stijlen

![Service Fabric-platform status opslag][Image2]

***De monolithische aanpak aan de linkerkant heeft een individuele database en lagen van specifieke technologieën.***

***De microservices-benadering aan de rechterkant heeft een grafiek met elkaar verbonden microservices waarin staat doorgaans is afgestemd op de microservices en verschillende technologieën worden gebruikt.***

De toepassing in een monolithische aanpak gebruikt doorgaans een individuele database. Het voordeel is dat het een enkele locatie, waardoor het eenvoudig te implementeren. Elk onderdeel kan één tabel voor het opslaan van de status hebben. Teams moeten strikt scheiden staat, die een hele uitdaging. Er zijn onvermijdelijk temptations een nieuwe kolom toevoegen aan een bestaande klantentabel, een koppeling tussen tabellen en afhankelijkheden maken in de storage-laag. Als dit gebeurt, kunt u afzonderlijke onderdelen kan niet schalen.

In de microservices-benadering, elke service beheert en slaat de eigen staat. Elke service is verantwoordelijk voor het schalen van zowel de code en de status samen om te voldoen aan de eisen van de service. Een nadeel is dat wanneer een hoeft te maken van weergaven of query's van gegevens van uw toepassing, moet u query's uitvoeren voor meerdere archieven van de status. Dit is meestal opgelost door een afzonderlijke microservice dat een weergave in een verzameling van microservices. Als u meerdere ad hoc-query's uitvoeren voor de gegevens wilt, elke microservice moet rekening houden met de gegevens schrijven naar een service voor gegevensopslag voor offline analyse.

Microservices zijn samengesteld en is het mogelijk dat verschillende versies van een microservice side-by-side mogelijk uitgevoerd. Een nieuwere versie van een microservice kan mislukken tijdens de upgrade en moet worden teruggezet naar een eerdere versie. Versiebeheer is ook nuttig voor het testen van A/B-stijl, waarbij verschillende gebruikers verschillende versies van de service ondervindt. Het is bijvoorbeeld gebruikelijk om te upgraden van een microservice voor een bepaalde set klanten naar de nieuwe functionaliteit testen voordat u veel meer rolling.

### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>Communiceert met andere microservices via goed gedefinieerde interfaces en protocollen

Uitgebreide documentatie over service oriented architecture is gepubliceerd in de afgelopen 10 jaar met een beschrijving van communicatiepatronen. Over het algemeen servicecommunicatie een REST-benadering met HTTP- en TCP-protocollen en XML of JSON gebruikt als de serialisatie-indeling. Vanuit het perspectief van een interface draait de aanpak voor het ontwerp van web overstappen. Maar niets stopt u binaire protocollen of de opmaak van uw eigen gegevens te gebruiken. Worden voorbereid voor personen een moeilijker tijdstip met behulp van uw microservices als deze protocollen en indelingen niet openlijk beschikbaar zijn.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Heeft een unieke naam (URL) gebruikt voor het omzetten van de locatie

Uw microservice moet worden gebruikt wanneer deze wordt uitgevoerd. Als u denkt u over virtuele machines en welke een bepaalde microservice wordt uitgevoerd, gaat ongeldige snel. 

Uw microservice moet een unieke naam op dezelfde manier dat er een bepaalde URL op DNS wordt omgezet naar een bepaalde machine, zodat de huidige locatie kan worden gedetecteerd. Microservices moet adresseerbare namen die onafhankelijk zijn van de infrastructuur waarop ze worden uitgevoerd. Dit betekent dat er een interactie tussen hoe de service is geïmplementeerd en hoe deze wordt gedetecteerd, is omdat er moet een service-register. Wanneer een virtuele machine is mislukt, de registry-service moet laat u weten waar de service is verplaatst naar.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Consistente en geval van problemen beschikbaar blijft

Omgaan met onverwachte fouten is een van de moeilijkst problemen op te lossen, met name in een gedistribueerd systeem. Veel van de code die we als ontwikkelaars schrijven afhandeling van uitzonderingen en dit is ook waar de meeste tijd is besteed aan het testen. Het probleem is meer betrokken dan het schrijven van code voor het afhandelen van fouten. Wat gebeurt er wanneer de computer waarop de microservice is uitgevoerd mislukt? Niet alleen hebt u nodig voor het detecteren van de fout (een vaste probleem op een eigen), maar u moet ook opnieuw opstarten van uw microservice.

Omwille van de beschikbaarheid van moet een microservice tegen uitvallen en opnieuw te starten op een andere computer. Naast de programmacode wordt flexibel, al dan niet mogen worden er verlies van gegevens en de gegevens nodig om consistent te blijven.

Flexibiliteit is moeilijk te bereiken als er fouten optreden tijdens de upgrade van een toepassing. De microservice, werken met het implementatiesysteem hoeft niet te herstellen. Nodig om te bepalen of u kunt doorgaan te gaan naar de nieuwste versie in plaats daarvan terugdraaien naar een eerdere versie om te kunnen handhaven een consistente status. Vragen zoals of voldoende machines zijn beschikbaar zijn voor houden naar voren verplaatsen en het herstellen van eerdere versies van de microservice moeten worden overwogen. Hiervoor moet de microservice om te verzenden van gegevens over de servicestatus om deze beslissingen te maken.

### <a name="reports-health-and-diagnostics"></a>Rapporten status en diagnose

Er lijken misschien overduidelijk, en het is vaak over het hoofd gezien, maar een microservice moet de status en diagnostische gegevens worden gerapporteerd. Anders wordt er weinig inzicht in de status van een operations-perspectief. Correleren van diagnostische gebeurtenissen in een set onafhankelijke services en omgaan met machine tijdsverschillen te zinvol in de volgorde van de gebeurtenissen, is het lastig om. Op dezelfde manier dat u met een microservice via overeengekomen protocollen en opmaak van gegevens communiceren, moet u standaardiseren hoe u zich aanmeldt status en diagnostische gebeurtenissen die uiteindelijk wordt terechtkomen in een gebeurtenissenarchief voor het uitvoeren van query's en weergeven. In een microservices-benadering is het belangrijkste die verschillende teams eens zijn over een één-indeling. Er moet een consistent benadering voor het bekijken van diagnostische gebeurtenissen in de toepassing als geheel.

Status wijkt af van diagnostische gegevens. De status is over de microservices rapportage van de huidige status juiste acties te ondernemen. Een goed voorbeeld werkt samen met de upgrade en implementatie mechanismen om beschikbaarheid te houden. Hoewel een service kan momenteel niet in orde vanwege het vastlopen van een proces of opnieuw opstarten van de computer, de service mogelijk nog steeds operationeel zijn. Het laatste wat dat u nodig is om te slechter maken door een upgrade uit te voeren. De aanbevolen aanpak is het eerst een onderzoek doen of wacht totdat de microservice om te herstellen. Statusgebeurtenissen van een microservice help ons gefundeerde beslissingen te nemen en van kracht te maken van de zelf-herstellende services.

## <a name="service-fabric-as-a-microservices-platform"></a>Service Fabric als een microservices-platform

Azure Service Fabric naar voren gekomen als Microsoft is overgegaan van dergelijke producten die doorgaans monolithische in stijl zijn, leveren aan het leveren van services. De ervaring van het bouwen en gebruiken van grote services, zoals Azure SQL Database en Azure Cosmos DB, vorm van Service Fabric. Het platform zich na verloop van tijd ontwikkeld als meer services genomen. Service Fabric had om uit te voeren in Azure maar in standalone Windows Server-implementaties.

***Het doel van Service Fabric is op te lossen de moeilijke problemen van het bouwen en uitvoeren van een service en gebruikmaken van beschikken over infrastructurele resources efficiënt, zodat teams met behulp van een microservices-benadering bedrijfsproblemen kunnen oplossen.***

Service Fabric kunt u toepassingen die gebruikmaken van een microservices-benadering door te geven:

* Een platform waarmee systeemservices te implementeren, upgraden, detecteren, en start de mislukte services, services detecteren, routeren van berichten, status beheren en status controleren.
* De mogelijkheid om toepassingen te implementeren een van beide wordt uitgevoerd in containers of processen. Service Fabric is een container en het proces orchestrator.
* Productieve programming API's, voor informatie over het bouwen van toepassingen, zoals microservices: [ASP.NET Core en Reliable Actors, Reliable Services](service-fabric-choose-framework.md). Bijvoorbeeld, krijgt u gegevens over status en diagnostische gegevens, of kunt u profiteren van de ingebouwde hoge beschikbaarheid.

***Service Fabric is agnostisch op hoe u uw service ontwikkelen en kunt u elke technologie. Het biedt echter ingebouwde programmeertaal API's die het eenvoudiger om te bouwen van microservices.***

### <a name="migrating-existing-applications-to-service-fabric"></a>Migreren van bestaande toepassingen naar Service Fabric

Service Fabric kunt u bestaande code, die vervolgens kan worden gemoderniseerd met nieuwe microservices. Er zijn vijf fasen modernisering van toepassingen, en u kunt starten en stoppen op een van de fasen. Dit zijn:

1) Beginnen met een traditionele monolithische toepassing.  
2) Lift en Shift - containers of uitvoerbare gastbestanden gebruiken voor het hosten van bestaande code in Service Fabric.  
3) Modernisering - nieuwe microservices toegevoegd naast de bestaande beperkte code.  
4) Innovatie - in microservices uitsluitend op basis van de monolithische opsplitsen.  
5) Omgezet in microservices - de transformatie van bestaande monolithische toepassingen of het bouwen van nieuwe greenfield-toepassingen.

![Migratie naar Microservices][Image3]

Het is belangrijk om te benadrukken opnieuw kunt u **starten en stoppen bij elk van deze fasen**. U bent niet verplicht tot de volgende fase. Laten we nu kijken voorbeelden voor elk van deze fasen.

**Lift- and -Shift**  
Groot aantal bedrijven zijn via lift- and verschuiven van bestaande monolithische toepassingen in containers om twee redenen:

- Kosten vermindering vanwege consolidatie en verwijderen van bestaande hardware of met toepassingen met hogere dichtheid.
- Consistente implementatie contract tussen ontwikkel- en bewerkingen.

Kosten kortingen zijn opgebouwd uit overzichtelijke en binnen Microsoft, groot aantal bestaande toepassingen zijn die wordt opgenomen in een container gewoon om miljoenen dollars te besparen. Consistente implementatie is het moeilijker om te evalueren, maar als belangrijk. Dit betekent dat ontwikkelaars nog steeds kiezen van de technologie die bij u past worden kunnen, maar de bewerkingen accepteert alleen een één manier om te implementeren en beheren van deze toepassingen. Dit vermindert de bewerkingen van dat u hoeft te bekommeren om de complexiteit van veel verschillende technologieën of ontwikkelaars kunnen alleen bepaalde waarden kiezen forceren. In wezen elke toepassing wordt opgenomen in een container in onafhankelijke implementatie-installatiekopieën.

Veel organisaties stoppen hier. Ze al de voordelen van containers en Service Fabric biedt de volledige beheerervaring van implementatie, upgrades, versiebeheer, terugdraaiacties, health monitoring enzovoort.

**Modernisering**  
Het toevoegen van nieuwe services naast de bestaande App code in een container. Als u nieuwe code te schrijven, is het raadzaam te besluiten te nemen van kleine stappen in het pad van microservices. Dit kan een nieuwe REST API-eindpunt of een nieuwe bedrijfsregels worden toegevoegd. Op deze manier die u op de reis van nieuwe microservices bouwen en praktijk ontwikkelen en implementeren van deze starten.

**Innoveer**  
Een microservices-benadering geschikt is voor de wisselende bedrijfsbehoeften van uw. In dit stadium is de beslissing of u wilt beginnen met het splitsen van de monolithische app in de services of innoveren. Hier een voorbeeld is wanneer een database die wordt gebruikt als een werkstroom-wachtrij een bottleneck in de verwerking wordt. Als het aantal werkstroom toeneemt aanvragen, moet het werk voor schaal worden gedistribueerd. Dit scenario te splitsen in een microservice en Innoveer voor die bepaald onderdeel van de toepassing die niet wordt geschaald en moet die vaker worden bijgewerkt.

**Omgezet in microservices**  
Dit is waar uw toepassing is volledig bestaat uit (of uit) microservices. Als u wilt bereiken hier, kunt u het traject microservices hebt gemaakt. U kunt hier beginnen, maar om dit te doen zonder een microservices-platform om u te helpen is een aanzienlijke investering. 

### <a name="are-microservices-right-for-my-application"></a>Microservices zijn geschikt voor mijn toepassing?

Misschien. Wat we ervaren is veel van deze de voordelen van een microservice-achtige benadering mogelijk als er steeds meer teams in Microsoft begon te bouwen voor de cloud voor zakelijke redenen, gerealiseerd. Bing, bijvoorbeeld, heeft zijn ontwikkelen met behulp van microservices jaar. De microservices-benadering is voor andere teams, nieuwe. Teams gevonden die moeilijke problemen op te lossen buiten hun kerngebieden van kracht zijn. Dit is de reden waarom de Service Fabric opgedaan steun die krijgt als de technologie van uw voorkeur voor het bouwen van services.

Het doel van Service Fabric is om te beperken van de complexiteit van het bouwen van microservice-toepassingen die u geen hebt zo veel dure redesigns doorlopen. Begin klein, schalen wanneer dat nodig is, geen meer services, toevoegen van nieuwe oplossingen en ontwikkelen met gebruik van de klant. Ook weten dat er tal van andere problemen nog moet worden opgelost zijn om microservices meer toegankelijk voor de meeste ontwikkelaars. Containers en de actor-programmeermodel zijn voorbeelden van kleine stappen in deze richting, en we ervoor dat er meer innovaties ontstaan als u wilt dit eenvoudiger maken.
 
<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van service Fabric-terminologie](service-fabric-technical-overview.md)
* [Microservices: Een toepassing revolution mogelijk gemaakt door de cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
[Image3]: media/service-fabric-overview-microservices/microservices-migration.png
