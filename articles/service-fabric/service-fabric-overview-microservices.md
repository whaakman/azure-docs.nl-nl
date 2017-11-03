---
title: Inleiding tot microservices in Azure | Microsoft Docs
description: Een overzicht van waarom cloudtoepassingen maken met een benadering microservices is belangrijk voor toepassingsontwikkeling moderne en hoe Azure Service Fabric biedt een platform om dit te bereiken.
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: fae2be85-0ab4-4cd3-9d1f-e0d95fe1959b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 07/02/2017
ms.author: msfussell
ms.openlocfilehash: f69f594d058ba061cec116f87435c96280e19f93
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2017
---
# <a name="why-a-microservices-approach-to-building-applications"></a>Waarom een microservices benadering voor het ontwikkelen van toepassingen?
Software-ontwikkelaars zijn er geen nieuwe functies in onze verwachting over waarbij een toepassing in onderdelen. Het is het centrale paradigma stand object software abstracties en componentization. Vandaag de dag doorgaans dit factoriseren de vorm van klassen en interfaces tussen gedeelde bibliotheken en lagen van de technologie. Normaal gesproken wordt een gelaagde benadering gemaakt met een back-end-winkel, bedrijfslogica middelste laag en een front-gebruikersinterface (UI). Wat *heeft* gewijzigd in de afgelopen jaren is dat we, ontwikkelaars, bouwt gedistribueerde toepassingen die voor de cloud en aangedreven door het bedrijf.

De veranderende bedrijfsbehoeften zijn:

* Een service die gebouwd en werkt op grote schaal om klanten te bereiken in de nieuwe geografische regio's (bijvoorbeeld).
* Snellere levering van functies en mogelijkheden om te kunnen reageren op verzoeken van klanten in een flexibele manier.
* Verbeterde Resourcegebruik om kosten te verlagen.

Deze bedrijfsbehoeften van invloed zijn op *hoe* we bij het ontwikkelen van toepassingen.

Lees voor meer informatie over de aanpak van Azure naar microservices [Microservices: een toepassing revolution aangedreven door de cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservice-design-approach"></a>Monolithische versus microservice ontwerpbenadering
Alle toepassingen ontwikkelen gedurende een bepaalde periode. Succesvolle toepassingen ontwikkelen door handig voor gebruikers. Mislukte toepassingen niet doen evolueren en uiteindelijk zijn afgeschaft. De volgende vraag: hoeveel u weet over de vereisten van uw vandaag en wat ze worden in de toekomst? Stel dat dat u een reporting-toepassing voor een afdeling wilt maken. U bent ervoor dat de toepassing binnen het bereik van uw bedrijf blijft en dat de rapporten tijdelijke. Uw keuze van benadering wijkt af van, spreken, video-inhoud tientallen miljoenen klanten bouwen van een service die biedt. 

Soms iets uit de deur krijgen als bewijs van het concept is de aangedreven factor terwijl u weet dat de toepassing later kan worden aangepast. Er is weinig punt in iets te veel engineering die nooit wordt gebruikt. Het is de gebruikelijke engineering verhouding. Wanneer bedrijven over bouwen voor de cloud praten, is de verwachting aan de andere kant groei en informatie over het gebruik. Het probleem is dat groei en schaal onvoorspelbaar zijn. We willen mogelijk prototype snel achterhoofd ook dat er zich op een pad bevinden naar het omgaan met succes in de toekomst. Dit is het starten van de lean benadering: bouwen, te meten, informatie over en herhalen.

Tijdens de era client / server-doorgaans we ligt de nadruk op gelaagde toepassingen bouwen met behulp van specifieke technologieën in elke laag. De term *monolithische* toepassing geworden dat voor deze methoden. De interfaces doorgaans tussen de lagen en het ontwerp van een meer nauw gekoppeld tussen onderdelen binnen elke laag is gebruikt. Ontwikkelaars ontworpen en meeberekend klassen die zijn gecompileerd in bibliotheken en aan elkaar gekoppeld in een paar uitvoerbare programma's en dll-bestanden. 

Er zijn voordelen voor dergelijke monolithische ontwerpplan. Het is vaak eenvoudiger te ontwerpen en biedt sneller aanroepen tussen onderdelen, omdat deze aanroepen vaak via IPC (interprocess communication zijn). Bovendien iedereen één product dat meer mensen resources efficiënt worden doorgaans wordt getest. Het nadeel is dat er een nauwe koppeling tussen gelaagde lagen, en u kunt geen afzonderlijke onderdelen schalen. Als u uitvoeren voor oplossingen of upgrades wilt, hebt u wachten tot andere klaar bent met testen. Het is moeilijker te flexibel zijn.

Microservices adres deze nadelen en meer nauw uitgelijnd met de voorgaande zakelijke vereisten, maar ze hebben ook zowel voordelen en verplichtingen. De voordelen van microservices zijn dat elk criterium doorgaans eenvoudiger business functionaliteit, zodat u omhoog of omlaag test schalen, implementeren en beheren onafhankelijk ingekapseld. Een belangrijk voordeel van een microservice-benadering is dat teams meer bedrijfsscenario's dan wordt aangestuurd door technologie, waardoor de gelaagde benadering. In de praktijk kleinere teams een microservice op basis van een klant-scenario ontwikkelen en eventuele technologieën die ze gebruiken. 

Met andere woorden, hoeft de organisatie niet tech om bij te houden microservice-toepassingen te standaardiseren. Afzonderlijke teams dat eigen services doen kunnen wat zinvol voor toe op basis van het team expertise of wat is het meest geschikt is voor het oplossen van het probleem. In de praktijk kan een reeks aanbevolen technologieën, zoals een bepaald NoSQL opslaan of web-toepassingsframework, verdient de voorkeur.

Het nadeel van microservices wordt geleverd in het beheren van het toegenomen aantal afzonderlijke entiteiten en omgaan met complexere implementaties en versiebeheer. Netwerkverkeer tussen de microservices wordt verhoogd en de bijbehorende netwerkvertragingen. Veel chatty, gedetailleerde services zijn een recept voor een nachtmerrie prestaties. Zonder hulpmiddelen voor het weergeven deze afhankelijkheden, is het moeilijk te 'Zie"van het gehele systeem. 

Standaarden maken de aanpak microservice werk door afspreken communiceren en fouttolerante alleen de bewerkingen die u nodig van een service hebt wordt in plaats van rigid contracten. Het is belangrijk deze contracten vooraf definiëren in het ontwerp voor services bijwerken onafhankelijk van elkaar. Een andere beschrijving lang geleden bedacht voor het ontwerpen van een benadering microservices is ' fijnmazig service oriented architecture (SOA).'

***De eenvoudigste is de ontwerpbenadering microservices over een ontkoppelde federation Services met onafhankelijke wijzigingen aan elk en overeengekomen standaarden voor communicatie.***

Als u meer cloud-apps worden geproduceerd, detecteren mensen dat deze afbreken van de algehele app in onafhankelijke, scenario gerichte services een beter op lange termijn benadering is.

## <a name="comparison-between-application-development-approaches"></a>Vergelijking tussen toepassingsontwikkeling nadert
![Toepassingsontwikkeling service Fabric-platform][Image1]

1) Een monolithisch app domeinspecifieke functionaliteit bevat en normaal wordt gedeeld door functionele lagen, zoals web en business gegevens.

2) U een monolithisch app schalen door het klonen op meerdere servers/virtuele machines/containers.

3) Een toepassing microservice scheidt functionaliteit in afzonderlijke kleinere services.

4) De microservices benadering schalen uit door het implementeren van elke service afzonderlijk, exemplaren van deze services te maken tussen servers/virtuele machines/containers.

Met een microservice ontwerpen benadering is niet een wondermiddel voor alle projecten, maar deze zijn afgestemd nauwkeuriger de zakelijke doelstellingen die eerder zijn beschreven. Beginnen met een monolithisch benadering wellicht acceptabel als u weet dat u hebt de mogelijkheid hoger herzien van de code in het ontwerp van een microservices. Vaker voorkomt, begint met een monolithisch toepassing en langzaam splits deze in fasen verlopen, beginnen met de modules die moeten worden meer schaalbare of flexibele.

Om samen te vatten, is de microservice-benadering voor het opstellen van uw toepassing van veel kleine services. De services worden uitgevoerd in de containers die zijn geïmplementeerd op een cluster van machines. Kleinere teams ontwikkelen van een service die is gericht op een scenario onafhankelijk, versie testen, implementeren en schalen van elke service, zodat de gehele toepassing kunt ontwikkelen.

## <a name="what-is-a-microservice"></a>Wat is een microservice?
Er zijn verschillende definities van microservices. Als u Internet zoekt, vindt u veel nuttige informatiebronnen die hun eigen standpunten en definities bieden. Echter, de meeste van de volgende kenmerken van microservices zijn algemeen overeengekomen:

* Een klant of zakelijke scenario inkapselen. Wat is het probleem dat u het oplossen van?
* Ontwikkeld door een klein engineering team.
* Geschreven in elke programmeertaal en elk framework gebruiken.
* Bestaan uit de code en (optioneel) state, die beide zijn onafhankelijk samengestelde geïmplementeerd en geschaald.
* Communiceren met andere microservices via goed gedefinieerde interfaces en protocollen.
* Unieke namen (URL's) gebruikt voor het omzetten van de locatie hebben.
* Blijven consistent en beschikbaar is in geval van problemen.

U kunt deze kenmerken in samenvatten:

***Microservice-toepassingen bestaan uit kleine, onafhankelijk van elkaar samengestelde en schaalbare klantgerichte services die via standaardprotocollen met goed gedefinieerde interfaces met elkaar communiceren.***

We de eerste twee punten in de vorige sectie besproken en nu we uitvouwt op en de andere verduidelijken.

### <a name="written-in-any-programming-language-and-use-any-framework"></a>Geschreven in elke programmeertaal en elk framework gebruiken
Ontwikkelaars moeten we kiezen een taal of elk framework dat we, afhankelijk van onze vaardigheden of de behoeften van de service willen. In sommige services mogelijk u prestatievoordelen van C++ boven alle andere waarde. In andere services, kan het gemak van beheerde-ontwikkeling in C# of Java zeer belangrijk zijn. In sommige gevallen moet u wellicht een specifieke partner-bibliotheek, technologie voor gegevensopslag of wijze van het blootstellen van de service voor clients gebruiken.

Nadat u hebt ervoor gekozen een technologie u keert u naar de operationele of lifecycle management en schalen van de service.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Kan code en status als onafhankelijk samengestelde geïmplementeerd en geschaald
U wilt echter schrijven van uw microservices, de code en eventueel de status moeten onafhankelijk implementeren, bijwerken en schalen. Dit is daadwerkelijk een van de problemen moeilijker om op te lossen, omdat het afkomstig niet actief voor uw keuze van technologieën is. Voor vergroten/verkleinen, begrijpen hoe partitie (of shard) de code en de status is lastig. Wanneer de code en de status van afzonderlijke technologieën die algemene vandaag, moeten de implementatiescripts voor uw microservice kunnen omgaan met beide schalen. Dit is ook over wendbaarheid en flexibiliteit, zodat u enkele van de microservices bijwerken kunt zonder dat ze allemaal in één keer te upgraden.

Wordt teruggezonden naar de monolithische versus microservice benadering even, toont het volgende diagram de verschillen in de aanpak voor het opslaan van status.

#### <a name="state-storage-between-application-styles"></a>Status opslag tussen toepassing stijlen
![Service Fabric-platform status opslag][Image2]

***De monolithische aanpak aan de linkerkant heeft een individuele database en lagen van specifieke technologieën.***

***De methode microservices aan de rechterkant biedt een grafiek van onderling verbonden microservices waarin staat doorgaans is afgestemd op de microservice en verschillende technologieën worden gebruikt.***

In een monolithisch benadering, doorgaans de toepassing gebruikt een individuele database. Het voordeel is dat er één locatie, waardoor u eenvoudig te implementeren. Elk onderdeel kan één tabel voor het opslaan van de status hebben. Teams moeten strikt staat die moeilijk te scheiden. Er zijn onvermijdelijk temptations een nieuwe kolom toevoegen aan een bestaande klantentabel, een koppeling tussen tabellen en afhankelijkheden in de opslaglaag maken. Nadat dit gebeurt, kunt u afzonderlijke onderdelen kan niet schalen. 

In de methode microservices, elke service beheert en slaat de eigen staat. Elke service is verantwoordelijk voor het schalen van zowel de code en de status samen om te voldoen aan de vereisten van de service. Een nadeel is wanneer een hoeft te maken van weergaven of query's van de gegevens van uw toepassing, moet u een query in uiteenlopende status winkels. Dit is meestal opgelost door een afzonderlijke microservice die een weergave in een verzameling microservices voortbouwt. Als u meerdere ad hoc query's uitvoeren op de gegevens wilt, moet elke microservice rekening houden met de gegevens schrijven naar een gegevensservice magazijnbeheer voor offline analyses.

Versiebeheer is specifiek voor de geïmplementeerde versie van een microservice zodanig dat meerdere verschillende versies implementeren en naast elkaar uitvoeren. Versiebeheer voor onderdelen heeft betrekking op de scenario's waarbij een nieuwere versie van een microservice mislukt tijdens de upgrade en moet terugkeren naar een eerdere versie. Het andere scenario voor versiebeheer is bezig met testen van A/B-stijl, waarbij verschillende gebruikers verschillende versies van de service ondervindt. Bijvoorbeeld, wordt het meestal een microservice voor een specifieke set klanten nieuwe functionaliteit te testen voordat u veel meer rolling upgrade. Na het levenscyclusbeheer van microservices biedt deze nu ons voor de communicatie ertussen.

### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>Interactie met andere microservices via goed gedefinieerde interfaces en protocollen
In dit onderwerp aandacht weinig hier, omdat uitgebreide documentatie over service oriented architecture die is gepubliceerd in de afgelopen 10 jaar communicatiepatronen beschrijft. Servicecommunicatie gebruikt in het algemeen een REST-benadering met HTTP- en TCP-protocollen en XML- of JSON als de serialisatie-indeling. Vanuit het perspectief van een interface wordt de ontwerpbenadering web gaan hanteren. Maar niets u voorkomt met binaire protocollen of uw eigen gegevensindelingen. Worden voorbereid voor gebruikers lastiger tegelijk met uw microservices als deze protocollen en indelingen niet openbaar beschikbaar zijn.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Heeft een unieke naam (URL) gebruikt voor het omzetten van de locatie
Houd er rekening mee hoe we houden zeggen dat de aanpak microservice lijkt op het web? Zoals het web moet uw microservice adresseerbare waar deze wordt uitgevoerd. Als u denkt u over virtuele machines en welke een bepaalde microservice wordt uitgevoerd, gaan dingen onjuiste snel. 

Op dezelfde manier dat DNS worden omgezet in een bepaalde URL een bepaalde machine, moet uw microservice een unieke naam hebben, zodat de huidige locatie kan worden gedetecteerd. Microservices moet adresseerbare namen zodat ze onafhankelijk van de infrastructuur die ze worden uitgevoerd. Dit betekent dat er een interactie tussen hoe uw service wordt geïmplementeerd en hoe deze wordt gedetecteerd, is omdat er moet een register service zijn. Evenredig, wanneer een machine is mislukt, de registry-service moet laat u weten waar de service wordt nu uitgevoerd. 

Hiermee beschikt u over ons naar het volgende onderwerp: herstelmogelijkheden en consistentie.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Blijft consistent en beschikbaar is in geval van problemen
Omgaan met onverwachte fouten is een van de moeilijkst problemen kunt oplossen, met name in een gedistribueerde systemen. Veel van de code die we als ontwikkelaars schrijven is afhandeling van uitzonderingen en dit is ook waar de meeste tijd is besteed aan het testen. Het probleem is meer dan het schrijven van code voor het afhandelen van fouten die zijn betrokken. Wat gebeurt er wanneer de computer waarop de microservice wordt uitgevoerd mislukt? Niet alleen moet u deze microservice-fout (een vaste probleem zelf) te detecteren, maar u moet ook iets opnieuw opstarten van uw microservice. 

Een microservice moet netwerkfouten fouten en opnieuw opstarten vaak op een andere computer omwille van de beschikbaarheid. Dit wordt ook geleverd omlaag naar de status die is opgeslagen namens de microservice wanneer deze status uit door de microservice kunt herstellen, en of de microservice kunnen starten met succes is. Met andere woorden, moet er herstelmogelijkheden in de berekening (het proces opnieuw opgestart), evenals de herstelmogelijkheden in de status of de gegevens (zonder verlies van gegevens en de gegevens consistent blijven).

De problemen van de tolerantie zijn samengesteld tijdens andere scenario's, zoals wanneer fouten tijdens een upgrade van de toepassing optreden. De microservice, werken met het implementatiesysteem hoeft niet te herstellen. Ook moet deze vervolgens beslissen of u kunt doorgaan verder gaan naar de nieuwere versie of in plaats daarvan terugdraaien naar een eerdere versie te onderhouden van een consistente status. Vragen zoals of voldoende machines zijn beschikbaar voor het doorsturen zwevend houden en het herstellen van eerdere versies van de microservice moeten worden overwogen. Hiervoor moet de microservice statusgegevens om deze beslissingen te kunnen verzenden.

### <a name="reports-health-and-diagnostics"></a>Rapporten status en diagnose
Het lijkt misschien duidelijk, en wordt vaak wordt overgeslagen, maar een microservice de status en diagnose moet rapporteren. Anders is er weinig inzicht vanuit het perspectief van een bewerkingen. Correleren van diagnostische gebeurtenissen in een set onafhankelijke services en omgaan met machine klok laat gevoel de volgorde van de gebeurtenis is lastig. Op dezelfde manier die u met een microservice via overeengekomen protocollen en gegevensopmaak werken, ontstaat er naar standaardisering in hoe u zich aanmeldt gezondheid en diagnostische gebeurtenissen die uiteindelijk in een archief van de gebeurtenis terechtkomen voor het uitvoeren van query's en weergeven. In een benadering microservices is deze sleutel verschillende teams overeenstemming worden bereikt over een één-indeling. Er moet een consistente benadering van diagnostische gebeurtenissen weergeven in de toepassing als geheel.

Health wijkt af van diagnostische gegevens. De status is over de microservice rapportage van de huidige status juiste acties te ondernemen. Een goed voorbeeld werkt samen met upgrade- en implementatie mechanismen om beschikbaarheid te houden. Hoewel een service is mogelijk momenteel beschadigd vanwege het vastlopen van een proces of opnieuw opstarten van de computer, de service nog steeds mogelijk operationeel. Het laatste wat dat u nodig is om dit te slechter door het uitvoeren van een upgrade. De aanbevolen aanpak is het onderzoek doen eerst of er voldoende tijd uittrekken om de microservice te herstellen. Gebeurtenissen van de status van een microservice helpt ons gefundeerde beslissingen te nemen en van kracht te zelfherstellende services maken.

## <a name="service-fabric-as-a-microservices-platform"></a>Service Fabric als microservices platform
Azure Service Fabric ontstaan uit een overgang door Microsoft in het vak-producten, doorgaans monolithische in stijl zijn, bij het leveren van services af te leveren. De ervaring van gebouw en grote services, zoals Azure SQL Database en Azure Cosmos DB besturingssysteem in de vorm van Service Fabric. Het platform ontwikkeld na verloop van tijd meer services wordt aangenomen. Houd voor ogen dat hadden Service Fabric uitgevoerd moeten niet alleen in Azure, maar ook in implementaties met zelfstandige Windows Server.

***Het doel van de Service Fabric is het oplossen van problemen met vaste bouwen en uitvoeren van een service en infrastructuur bronnen efficiënt gebruiken zodat teams met behulp van een benadering microservices bedrijfsproblemen kunnen oplossen.***

Service Fabric bevat drie brede gebieden te maken van toepassingen die gebruikmaken van een benadering microservices:

* Een platform waarmee systeemservices te implementeren en bijwerken, detecteren, en mislukte services opnieuw starten, services detecteren, routeren van berichten, status beheren en controleren status. Deze systeemservices kunnen van kracht veel van de kenmerken van microservices die eerder is beschreven.
* De mogelijkheid om toepassingen te implementeren ofwel in containers of als processen die wordt uitgevoerd. Service Fabric is een container en een proces orchestrator.
* Productief programming API's voor hulp bij het ontwikkelen van toepassingen als microservices: [ASP.NET Core, Reliable Actors en Reliable Services](service-fabric-choose-framework.md). U kunt de code voor het bouwen van uw microservice. Maar maak deze API's van de taak eenvoudiger en ze met het platform op een dieper niveau integreren. Op deze manier bijvoorbeeld, status en diagnose informatie kunt u vinden, of kunt u profiteren van de ingebouwde hoge beschikbaarheid.

***Service Fabric is agnostisch op hoe het bouwen van uw service en kunt u elke technologie. Dit biedt echter ingebouwde programming API's waarmee het eenvoudiger om microservices samen te stellen.***

### <a name="migrating-existing-applications-to-service-fabric"></a>Migreren van bestaande toepassingen naar Service Fabric
Een sleutel benadering van Service Fabric is het hergebruiken van bestaande code, die vervolgens kan worden modernized met nieuwe microservices. Er zijn vijf fasen toepassing modernisering en u kunt starten en stoppen op een van de fasen. Dit zijn;

1) Een traditionele monolithische toepassing  
2) Lift- en Shift - containers of Gast uitvoerbare bestanden gebruiken voor het hosten van bestaande code in Service Fabric.  
3) Modernisering - nieuwe microservices toegevoegd naast de bestaande beperkte code.  
4) Innoveren - de monolithische opsplitsen microservices alleen is gebaseerd op nodig.  
5) Omgezet in microservices - de transformatie van bestaande monolithische toepassingen of het ontwikkelen van nieuwe greenfield toepassingen.

![Migratie naar Microservices][Image3]

Het is belangrijk om te benadrukken opnieuw kunt u **starten en stoppen op een van deze fasen**. U bent niet verplicht tot de volgende fase. We gaan nu kijken voorbeelden voor elk van deze fasen.

**Lift- en verschuiven** -grote aantallen van bedrijven zijn op te heffen en bestaande monolithische toepassingen in containers verschuiving van twee redenen;

- Kosten vermindering ofwel vanwege consolidatie en verwijderen van bestaande hardware of met toepassingen met hogere dichtheid. 
- Consistente implementatie contract tussen ontwikkel- en bewerkingen.

Kosten kortingen zijn te begrijpen, en binnen Microsoft grote aantallen van bestaande toepassingen zijn wordt beperkte gewoon om op te slaan miljoenen bedragen. Consistente distributie is moeilijker te evalueren, maar als belangrijk. Wordt aangegeven dat ontwikkelaars nog steeds gratis worden kunnen kiezen van de technologie die suites ze, maar de bewerkingen slechts één methode accepteert te implementeren en beheren van deze toepassingen. Dit vermindert de bewerkingen van hoeven te maken met de complexiteit van veel verschillende technologieën of ontwikkelaars kunnen alleen bepaalde objecten kiezen om te dwingen. In wezen elke toepassing is beperkte in zelfstandige implementatie-installatiekopieën.

Veel organisaties stoppen hier. Al heeft, de voordelen van containers en Service Fabric bevat de volledige beheerervaring van implementatie, upgrades, versiebeheer, Rollback, health monitoring enzovoort.

**Modernisering** -is de toevoeging van nieuwe services samen met de bestaande beperkte code. Als u nieuwe code te schrijven, is het raadzaam te besluiten te nemen kleine stappen omlaag het pad microservices. Dit kan een nieuw REST-API-eindpunt, of een nieuwe bedrijfsregels worden toevoegt. Op deze manier die u start op het traject van nieuwe microservices gebouw en praktijken ontwikkelen en implementeren.

**Innoveren** -Vergeet niet de oorspronkelijke veranderende bedrijfsbehoeften aan het begin van dit artikel, die de aanpak microservices genereren toeneemt? Dit gebeurt op mijn huidige toepassing in deze fase de beslissing wordt, en zo ja, ik wil de monoliet splitsen of vernieuwing starten. Hier een voorbeeld is wanneer een database een knelpunt verwerken wordt omdat deze wordt gebruikt als een werkstroom wachtrij. Als het aantal werkstroom moeten aanvragen verhogen van het werk voor schaal worden gedistribueerd. Voor die bepaald onderdeel van de toepassing die niet schalen, of u moet dus vaker bijgewerkt, wordt dit uit te splitsen in een microservice en innoveren. 

**Omgezet in microservices** -waar uw toepassing is het volledig samengesteld uit (of ontleed in) microservices. Hier is bereikt, kunt u het traject microservices hebt aangebracht. Begin hier, maar u kunt dit doen zonder een microservices platform om u te helpen een aanzienlijke investering is. 

### <a name="are-microservices-right-for-my-application"></a>Microservices zijn geschikt voor mijn toepassing?
Misschien. We heeft ondergaan is veel van deze voordelen van een benadering microservice-achtige duurt mogelijk als er steeds meer teams in Microsoft begon te bouwen voor de cloud voor zakelijke redenen, gerealiseerde. Bing, bijvoorbeeld heeft is de ontwikkeling van microservices in zoeken naar jaar. De microservices aanpak is voor andere teams nieuwe. Teams vastgesteld dat er vaste problemen oplossen buiten hun belangrijkste gebieden van kracht zijn. Dit is de reden waarom de Service Fabric tractie ervaring als de technologie van keuze voor het bouwen van services.

Het doel van Service Fabric is het verminderen van de complexiteit van het bouwen van toepassingen met een benadering microservice, zodat er geen te doorlopen als veel dure redesigns. Begin klein, indien nodig worden geschaald, afschaffen services, nieuwe toevoegen en ontwikkelen met klant gebruik van de aanpak is. We ook weten dat er veel problemen nog moet worden opgelost zijn zodat microservices meer toegankelijk voor de meeste ontwikkelaars. Containers en het programmeermodel actor zijn voorbeelden van kleine stappen in deze richting en er zeker van te zijn dat er meer innovaties ontstaan te vergemakkelijken zijn.
 
<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van service Fabric-terminologie](service-fabric-technical-overview.md)
* [Microservices: Een toepassing revolution aangedreven door de cloud](https://azure.microsoft.com/en-us/blog/microservices-an-application-revolution-powered-by-the-cloud/)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
[Image3]: media/service-fabric-overview-microservices/microservices-migration.png
