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
ms.date: 06/18/2019
ms.author: atsenthi
ms.openlocfilehash: 5bcb52165c7cae18b807eff03c80b51eae8e2717
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67204796"
---
# <a name="why-use-a-microservices-approach-to-building-applications"></a>Waarom een microservices-benadering voor het bouwen van toepassingen gebruiken?

Voor software-ontwikkelaars is waarbij u rekening houdt een toepassing in onderdelen niets nieuwe. Normaal gesproken een gelaagde benadering wordt gebruikt, met een back-end-store, bedrijfslogica middelste laag en een front-end-gebruikersinterface (UI). Wat *heeft* gewijzigd in de afgelopen jaren is dat ontwikkelaars zijn worden gebruikt voor het bouwen van gedistribueerde toepassingen voor de cloud.

Hier volgen enkele wisselende bedrijfsbehoeften van uw:

* Een service die is gebouwd op schaal om klanten in nieuwe geografische regio's te bereiken.
* Snellere levering van functies en mogelijkheden om te reageren op verzoeken van klanten op flexibele wijze inzetten.
* Verbeterde Resourcegebruik om kosten te verlagen.

Deze behoeften van uw bedrijf van invloed zijn op *hoe* we bij het bouwen van toepassingen.

Zie voor meer informatie over de Azure-benadering tot microservices [Microservices: Een toepassing revolution mogelijk gemaakt door de cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/).

## <a name="monolithic-vs-microservices-design-approach"></a>Monolithische versus benadering van microservices ontwerpen

Toepassingen loop der tijd veranderen. Succesvolle toepassingen veranderen door handig voor gebruikers. Mislukte toepassingen niet veranderen en uiteindelijk zijn afgeschaft. De vraag als volgt: hoeveel wist u dat u over de vereisten voor vandaag en wat ze zijn in de toekomst? Stel dat u het bouwen van een reporting-toepassing voor een afdeling in uw bedrijf. U kunt ervoor dat de toepassing is van toepassing alleen binnen het bereik van uw bedrijf en dat de rapporten lang wordt niet bijgehouden. Uw benadering die afwijkt van, zeg, video-inhoud bouwen van een service die voorziet in tientallen miljoenen klanten.

Iets uit de deur aan als een bewijs van concept is soms de sparen factor. U weet dat de aanvraag later opnieuw kan worden ontworpen. Er is weinig punt in iets te veel engineering die nooit wordt gebruikt. Wanneer bedrijven voor de cloud bouwt, is de verwachting dat aan de andere kant groei en het gebruik. Groei en de schaal zijn onvoorspelbaar. We willen prototype snel terwijl u ook weten dat de bewerking op een pad dat toekomstig succes kan verwerken. Dit is de methode lean opstarten: bouwen, meten, leren en herhalen.

Tijdens het tijdperk client/server doorgaans we richten op het gelaagde toepassingen bouwen met behulp van specifieke technologieën in elke laag. De term *monolithische* toepassing geworden om te beschrijven van deze methoden. De interfaces uitbreiden moet tussen de lagen en ontwerp voor een meer nauw gekoppelde tussen onderdelen binnen elke laag is gebruikt. Ontwikkelaars die zijn ontworpen en rekening gehouden met klassen die zijn gecompileerd in bibliotheken en aan elkaar gekoppeld in een paar uitvoerbare bestanden en .dll-bestanden.

Er zijn voordelen van een monolithisch ontwerp-benadering. Monolithische toepassingen zijn vaak eenvoudiger te ontwerpen en aanroepen tussen onderdelen zijn sneller, omdat deze aanroepen vaak via interprocess communication (IPC zijn). Bovendien iedereen één enkel product, doorgaans een efficiënter gebruik van human resources worden gecontroleerd. Het nadeel is dat er een hechte koppeling tussen de lagen van gelaagde, en u afzonderlijke componenten niet schalen. Als u wilt de fixes of upgrades worden uitgevoerd, moet u wachten op anderen tests voltooid. Het is moeilijker flexibel zijn.

Microservices los deze aantal nadelen en meer nauw zijn afgestemd op de voorgaande zakelijke vereisten. Maar ze kunnen zowel de voordelen en de verplichtingen. De voordelen van microservices zijn dat elke doorgaans ingekapseld eenvoudiger bedrijfsfunctionaliteit, die u kunt omhoog of omlaag schalen, testen, implementeren en afzonderlijk beheren. Een belangrijk voordeel van een microservices-benadering is dat teams meer bedrijfsscenario's dan worden aangestuurd door de technologie. Kleinere teams ontwikkelen van een microservice op basis van een klant-scenario en gebruik van een technologie die ze willen gebruiken.

Met andere woorden, hoeft de organisatie te standaardiseren tech om bij te houden van microservice-toepassingen. Afzonderlijke teams die eigen services doen kunnen wat zinvol voor hen op basis van team kennis of wat is het meest geschikt is om het probleem te verhelpen. In de praktijk moet een set van aanbevolen-technologieën, zoals een bepaald NoSQL-archief of framework voor webtoepassingen, is het beter.

Het nadeel van microservices is dat u moet meer afzonderlijke entiteiten beheren en omgaan met complexere implementaties en versiebeheer. Netwerkverkeer tussen de microservices toeneemt, net als de bijbehorende netwerklatenties. Veel van intensieve, gespecialiseerde services kunnen leiden tot een nachtmerrie prestaties. Zonder hulpmiddelen waarmee u kunt de afhankelijkheden bekijken, is het moeilijk te zien van het gehele systeem.

Standaarden maken de microservices-benadering werken door te geven hoe om te communiceren en tolerating alleen de dingen die u een service moet in plaats van Star contracten. Het is belangrijk voor het definiëren van deze opdrachten een in het ontwerp omdat services onafhankelijk van elkaar werken. Een andere beschrijving lang geleden bedacht voor het ontwerpen met een microservices-benadering is "fijnmazig service oriented architecture (SOA)."

***De eenvoudigste, wordt de microservices ontwerpen-aanpak is over een losgekoppelde federation Services met onafhankelijke wijzigingen aan elk en overeengekomen standaarden voor communicatie.***

Als meer cloudtoepassingen worden geproduceerd, hebben mensen gedetecteerd dat deze ontleding van de algemene toepassing in onafhankelijke, zeer sterk gericht op scenario services een beter op de lange termijn benadering is.

## <a name="comparison-between-application-development-approaches"></a>Vergelijking van de ontwikkeling van toepassingen nadert

![Toepassingsontwikkeling voor service Fabric-platform][Image1]

1) Een monolithische toepassing bevat domeinspecifieke functionaliteit en normaal gesproken is onderverdeeld in functionele lagen, zoals web-, bedrijfs- en gegevens.

2) U een monolithische toepassing schalen door deze te klonen op meerdere servers/virtuele machines/containers.

3) Een microservice-toepassingen gescheiden functionaliteit in afzonderlijke, kleinere services.

4) De microservices-benadering schalen uit door het implementeren van elke service onafhankelijk van elkaar, exemplaren van deze services te maken over servers/virtuele machines/containers.

Ontwerpen met een microservices benadering is geschikt voor alle projecten, maar deze beter aansluiten uitgelijnd met de zakelijke doelstellingen die eerder zijn beschreven. Beginnen met een monolithische aanpak kan zinvol zijn als u weet dat u hebt de mogelijkheid om de code later bijwerken naar een microservice-ontwerp. Meer over het algemeen u beginnen met een monolithische toepassing en langzaam splits deze in fasen, beginnen met het functionele aspecten die moeten worden schaalbare of meer flexibele.

Wanneer u een microservices-benadering gebruikt, kunt u uw toepassing van veel kleine services samenstellen. Services in containers die zijn geïmplementeerd in een computercluster worden uitgevoerd. Kleinere teams ontwikkelen van een service die is gericht op een scenario en onafhankelijk van elkaar, versie testen, implementeren en schalen van elke service, zodat de gehele toepassing kunt ontwikkelen.

## <a name="what-is-a-microservice"></a>Wat is een microservice?

Er zijn verschillende definities van microservices. Maar de meeste van deze kenmerken van microservices breed worden geaccepteerd:

* Een klant of scenario bevatten. Wat is er aan de probleemoplossing?
* Ontwikkeld door een kleine IT-team.
* Geschreven in elke programmeertaal, met een willekeurig framework.
* Bestaan uit code, en (optioneel) de status, die beide onafhankelijke versies zijn, geïmplementeerd en geschaald.
* Communiceren met andere microservices via goed gedefinieerde interfaces en protocollen.
* Unieke namen hebben (URL's) die worden gebruikt voor het omzetten van hun locatie.
* Consistente en geval van problemen beschikbaar blijven.

Te totaliseren die:

***Microservice-toepassingen worden samengesteld uit kleine, onafhankelijke versies en schaalbare klantgerichte services die met elkaar via standaardprotocollen met goed gedefinieerde interfaces communiceren.***

### <a name="written-in-any-programming-language-using-any-framework"></a>Die zijn geschreven in elke programmeertaal, met een willekeurig framework

Ontwikkelaars willen we kiezen een andere taal of framework, afhankelijk van de vaardigheden en wat de behoeften van de service die wordt gemaakt. Voor sommige services kunt u de prestatievoordelen van waarde C++ hierboven iets anders. Voor andere, de gebruiksvriendelijke manier van beheerde ontwikkelen die u via C# of Java mogelijk belangrijker. In sommige gevallen moet u mogelijk gebruik van een specifieke partner-bibliotheek, een technologie voor gegevensopslag of een methode voor de weergave van de service aan clients.

Nadat u ervoor een technologie kiest, moet u Houd rekening met de operationele database of de levenscyclus van management en schalen van de service.

### <a name="allows-code-and-state-to-be-independently-versioned-deployed-and-scaled"></a>Kan code en de status onafhankelijke versies, geïmplementeerd en geschaald

Ongeacht hoe u uw microservices, de code en (optioneel) de status schrijft, moet onafhankelijk van elkaar implementeren, bijwerken en schalen. Dit probleem is het moeilijk om op te lossen omdat het gaat naar beneden om uw eigen keuze aan technologieën. Voor vergroten/verkleinen, inzicht in hoe u partitie (of shard) zowel de code en de status van de uitdagingen met zich mee. Wanneer de code en de status van verschillende technologieën, dit komt veel voor vandaag, moeten de implementatiescripts voor uw microservices worden ze allebei schalen. Dankzij deze scheiding is ook over de wendbaarheid en flexibiliteit, zodat u enkele van de microservices bijwerken kunt zonder dat ze allemaal in één keer te upgraden.

We gaan terug naar onze vergelijking van de benaderingen monolithische en microservices voor even de tijd. In dit diagram ziet u de verschillen tussen de methoden voor het opslaan van status:

#### <a name="state-storage-for-the-two-approaches"></a>Status van de opslag voor de twee benaderingen

![Service Fabric-platform status opslag][Image2]

***Aan de linkerkant, heeft een individuele database en lagen van specifieke technologieën de monolithische aanpak.***

***De microservices-benadering en heeft aan de rechterkant, een grafiek met elkaar verbonden microservices waarin staat doorgaans is afgestemd op de microservices en verschillende technologieën worden gebruikt.***

De toepassing in een monolithische aanpak gebruikt doorgaans een individuele database. Het voordeel van het gebruik van één database is dat deze zich op één locatie, waardoor het eenvoudig te implementeren. Elk onderdeel kan één tabel voor het opslaan van de status hebben. Teams moeten strikt scheiden staat, die een hele uitdaging. Iemand zijn onvermijdelijk geneigd een kolom toevoegen aan een bestaande klantentabel, een koppeling tussen tabellen en afhankelijkheden maken in de storage-laag. Als dit gebeurt, kunt u afzonderlijke onderdelen kan niet schalen.

In de microservices-benadering, elke service beheert en slaat de eigen staat. Elke service is verantwoordelijk voor het schalen van zowel de code en de status samen om te voldoen aan de eisen van de service. Een nadeel is dat wanneer u nodig hebt om te maken van weergaven of query's van gegevens van uw toepassing, moet u query's uitvoeren voor meerdere archieven van de status. Dit probleem wordt meestal opgelost door een afzonderlijke microservice dat een weergave in een verzameling van microservices. Als u meerdere ad hoc-query's uitvoeren op de gegevens wilt, moet u rekening houden met gegevens van elke microservice schrijven naar een service voor gegevensopslag voor offline analyse.

Microservices zijn samengesteld. Het is mogelijk voor verschillende versies van een microservice om uit te voeren naast elkaar. Een nieuwere versie van een microservice kan mislukken tijdens een upgrade en moet worden teruggezet naar een eerdere versie. Versiebeheer is ook nuttig voor A / B-tests, waarbij verschillende gebruikers verschillende versies van de service ondervindt. Het is bijvoorbeeld gebruikelijk om te upgraden van een microservice voor een bepaalde set klanten naar de nieuwe functionaliteit testen voordat u veel meer rolling.

### <a name="interacts-with-other-microservices-over-well-defined-interfaces-and-protocols"></a>Communiceert met andere microservices via goed gedefinieerde interfaces en protocollen

In de afgelopen 10 jaar is uitgebreide informatie gepubliceerd met een beschrijving van communicatiepatronen in servicegeoriënteerde architecturen. Over het algemeen servicecommunicatie een REST-benadering met HTTP- en TCP-protocollen en XML of JSON gebruikt als de serialisatie-indeling. Vanuit het perspectief van een interface wordt een web-ontwerp benadering. Maar niets moet voorkomen dat u met behulp van de binaire protocollen of de opmaak van uw eigen gegevens. Alleen er rekening mee dat personen een moeilijker tijdstip met behulp van uw microservices hebben als deze protocollen en indelingen niet openlijk beschikbaar zijn.

### <a name="has-a-unique-name-url-used-to-resolve-its-location"></a>Heeft een unieke naam (URL) gebruikt voor het omzetten van de locatie

Uw microservice moet worden gebruikt wanneer deze wordt uitgevoerd. Als u erover over virtuele machines denkt en welke een bepaalde microservice wordt uitgevoerd, kunnen iets snel ongeldige gaat.

Uw microservice moet een unieke naam op dezelfde manier dat er een bepaalde URL op DNS wordt omgezet naar een bepaalde machine, zodat de huidige locatie kan worden gedetecteerd. Microservices moet adresseerbare namen die onafhankelijk van de infrastructuur die ze zijn op worden uitgevoerd. Dit betekent dat er een interactie tussen hoe de service is geïmplementeerd en hoe deze wordt gedetecteerd, is omdat er moet een service-register. Wanneer een virtuele machine is mislukt, moet de registry-service om aan te geven waarnaar de service is verplaatst.

### <a name="remains-consistent-and-available-in-the-presence-of-failures"></a>Consistente en geval van problemen beschikbaar blijft

Omgaan met onverwachte fouten is een van de moeilijkst problemen op te lossen, met name in een gedistribueerd systeem. Veel van de code die we als ontwikkelaars schrijven is voor het afhandelen van uitzonderingen. Tijdens het testen besteden we ook de meeste tijd aan de afhandeling van uitzonderingen. Het proces is meer betrokken dan het schrijven van code voor het afhandelen van fouten. Wat gebeurt er wanneer de computer waarop de microservice wordt uitgevoerd mislukt? U moet voor het detecteren van de fout, dit een probleem met een vaste op een eigen is. Maar u moet ook opnieuw starten van uw microservice.

Voor de beschikbaarheid van moet een microservice tegen uitvallen en opnieuw te starten op een andere computer. Naast deze vereisten voor flexibiliteit, mogen geen gegevens verloren gaan en gegevens nodig om consistent te blijven.

Flexibiliteit is moeilijk te bereiken als er fouten optreden tijdens de upgrade van een toepassing. De microservice, werken met het implementatiesysteem hoeft niet te herstellen. Nodig om te bepalen of u kunt doorgaan om te gaan naar de nieuwste versie of terugkeren naar een eerdere versie voor het onderhouden van een consistente status. U moet rekening houden met een aantal vragen, zoals of voldoende machines zijn beschikbaar zijn voor houden naar voren verplaatsen en het herstellen van eerdere versies van de microservices. Als u wilt deze beslissingen te nemen, moet u de microservice om te verzenden van gegevens over de servicestatus.

### <a name="reports-health-and-diagnostics"></a>Rapporten status en diagnose

Kan het lijken alsof duidelijk, en dit wordt vaak vergeten, maar een microservice moet de status en diagnostische gegevens rapporteren. Anders hebt u weinig inzicht in de status van een operations-perspectief. Correleren van diagnostische gebeurtenissen in een set onafhankelijke services en omgaan met machine tijdsverschillen te zinvol in de volgorde van de gebeurtenissen, is het lastig om. Op dezelfde manier dat u met een microservice via overeengekomen protocollen en opmaak van gegevens communiceren, moet u standaardiseren hoe u zich aanmeldt status en diagnostische gebeurtenissen die uiteindelijk wordt terechtkomen in een gebeurtenissenarchief voor het uitvoeren van query's en weergeven. Met een microservices-benadering moeten verschillende teams eens zijn over een één-indeling. Er moet een consistent benadering voor het bekijken van diagnostische gebeurtenissen in de toepassing als geheel.

Status wijkt af van diagnostische gegevens. De status is over de microservices rapportage van de huidige status juiste acties te ondernemen. Een goed voorbeeld werkt samen met de upgrade en implementatie mechanismen om beschikbaarheid te houden. Als een service kan worden momenteel niet in orde vanwege het vastlopen van een proces of opnieuw opstarten van de computer, de service mogelijk nog steeds operationeel zijn. Het laatste wat dat u nodig is om de situatie nog erger door een upgrade wordt gestart. De aanbevolen aanpak is het eerste onderzoeken of wacht totdat de microservice om te herstellen. Statusgebeurtenissen van een microservice help ons gefundeerde beslissingen te nemen en van kracht te maken van de zelf-herstellende services.

## <a name="guidance-for-designing-microservices-on-azure"></a>Richtlijnen voor het ontwerpen van microservices op Azure 
Ga naar het Azure architecture center voor richtlijnen over [ontwerpen en bouwen van microservices op Azure](https://docs.microsoft.com/azure/architecture/microservices/).

## <a name="service-fabric-as-a-microservices-platform"></a>Service Fabric als een microservices-platform

Azure Service Fabric naar voren gekomen als Microsoft is overgegaan van exemplaar-producten, die doorgaans monolithische leveren aan het leveren van services. De ervaring van het bouwen en gebruiken van grote services, zoals Azure SQL Database en Azure Cosmos DB, vorm van Service Fabric. Het platform zich na verloop van tijd ontwikkeld als meer services genomen. Service Fabric had om uit te voeren in Azure maar in standalone Windows Server-implementaties.

***Het doel van Service Fabric is het vaste problemen van het bouwen en uitvoeren van een service op te lossen en het gebruik van beschikken over infrastructurele resources efficiënt, zodat teams zakelijke problemen oplossen kunnen met behulp van een microservices-benadering.***

Service Fabric kunt u toepassingen die gebruikmaken van een microservices-benadering door te geven:

* Een platform waarmee systeemservices te implementeren, upgraden, detecteren, en start de mislukte services, services detecteren, routeren van berichten, status beheren en status controleren.
* De mogelijkheid om toepassingen te implementeren een van beide wordt uitgevoerd in containers of processen. Service Fabric is een container en het proces orchestrator.
* Productieve programming-API's waarmee u toepassingen, zoals microservices: [ASP.NET Core en Reliable Actors, Reliable Services](service-fabric-choose-framework.md). Bijvoorbeeld, krijgt u gegevens over status en diagnostische gegevens, of kunt u profiteren van de ingebouwde hoge beschikbaarheid.

***Service Fabric is agnostisch over hoe u uw service ontwikkelen en kunt u elke technologie. Maar biedt ingebouwde programmeertaal API's die het eenvoudiger om te bouwen van microservices.***

### <a name="migrating-existing-applications-to-service-fabric"></a>Migreren van bestaande toepassingen naar Service Fabric

Service Fabric kunt u bestaande code opnieuw te gebruiken voor het moderniseren met nieuwe microservices. Er zijn vijf fasen modernisering van toepassingen, en u kunt starten en stoppen tijdens elke fase. De fasen zijn:

1) Beginnen met een traditionele monolithische toepassing.  
2) Migreren. Containers of uitvoerbare gastbestanden gebruiken voor het hosten van de bestaande code in Service Fabric.  
3) Moderniseren. Voeg nieuwe microservices samen met bestaande beperkte code.  
4) Innoveer. De monolithische toepassing opsplitsen in op basis van microservices.  
5) Toepassingen in microservices transformeren. Bestaande monolithische toepassingen transformeren of nieuwe greenfield-toepassingen te bouwen.

![Migratie naar microservices][Image3]

Denk eraan dat u kunt *starten en stoppen bij elk van deze fasen*. U hoeft niet worden voortgezet op de volgende fase. 

Laten we kijken voorbeelden voor elk van deze fasen.

**Migreren**  
Veel bedrijven migreert bestaande monolithische toepassingen in containers om twee redenen:

* Kosten reduceren vanwege consolidatie en verwijderen van bestaande hardware of vanwege het uitvoeren van toepassingen op de hogere dichtheid.
* Een consistente implementatie overeenkomst tussen ontwikkel- en bewerkingen.

Kosten kortingen zijn simpel. Bij Microsoft, veel bestaande toepassingen zijn die wordt opgenomen in een container, leidt tot miljoenen dollars wordt bespaard. Consistente implementatie is het moeilijker om te evalueren, maar net zo belangrijk. Dit betekent dat ontwikkelaars de technologieën die geschikt is voor ze kunnen kiezen, maar operations slechts één enkele methode accepteert voor het implementeren en beheren van de toepassingen. Deze vermindert bewerkingen niet hoeft te bekommeren om de complexiteit van de ondersteuning van verschillende technologieën zonder ontwikkelaars kiezen alleen bepaalde objecten. In wezen elke toepassing wordt opgenomen in een container in onafhankelijke implementatie-installatiekopieën.

Veel organisaties stoppen hier. Ze al de voordelen van containers en Service Fabric biedt de volledige beheerervaring, inclusief de implementatie, upgrades, versiebeheer, terugdraaiacties en statuscontrole.

**Moderniseren**  
Modernisering is de toevoeging van nieuwe services samen met bestaande beperkte code. Als u nieuwe code te schrijven, is het raadzaam te Neem kleine stappen in het pad van microservices. Dit kan betekenen dat een nieuwe REST API-eindpunt of nieuwe zakelijke logica toe te voegen. Op deze manier kunt u het proces van de nieuwe microservices bouwen en praktijken ontwikkelen en implementeren van deze starten.

**Innoveer**  
Een microservices-benadering geschikt is voor de wisselende bedrijfsbehoeften van uw. In deze fase moet u beslissen of u de monolithische toepassing in services splitsen of innoveren te starten. Een klassieke in dit voorbeeld is wanneer een database die u als een werkstroom-wachtrij een bottleneck in de verwerking wordt. Als het aantal werkstroom toeneemt aanvragen, moet het werk voor schaal worden gedistribueerd. Die bepaald onderdeel van de toepassing die niet wordt geschaald, of die vaker worden bijgewerkt en deze uit als een microservice opsplitsen en innoveren moet nemen.

**Toepassingen in microservices transformeren**  
In deze fase is uw toepassing volledig bestaat uit (of splitsen in) microservices. Als u wilt dit punt bereikt, kunt u het traject microservices hebt aangebracht. Begin hier, maar om dit te doen zonder een microservices-platform om u te helpen vereist een aanzienlijke investering.

### <a name="are-microservices-right-for-my-application"></a>Microservices zijn geschikt voor mijn toepassing?

Misschien. Bij Microsoft, zoals meer teams begonnen op te bouwen voor de cloud voor zakelijke redenen, gerealiseerde veel van deze de voordelen van een microservice-achtige benadering. Bing, bijvoorbeeld, heeft zijn met behulp van microservices jaar. De microservices-benadering is voor andere teams, nieuwe. Teams gevonden die moeilijke problemen op te lossen buiten hun kerngebieden van kracht zijn. Dit is de reden waarom de Service Fabric opgedaan steun die krijgt als de technologie voor het bouwen van services.

Het doel van Service Fabric is het verminderen van de complexiteit van het bouwen van microservice-toepassingen, zodat u niet hoeft te doorlopen zo veel dure redesigns. Begin klein, schalen wanneer dat nodig is, geen meer services, toevoegen van nieuwe oplossingen en ontwikkelen met gebruik van de klant. Ook weten dat er tal van andere problemen nog moet worden opgelost zijn om microservices meer toegankelijk voor de meeste ontwikkelaars. Containers en de actor-programmeermodel zijn voorbeelden van kleine stappen in deze richting. We ervoor dat er meer innovaties ontstaan een microservices-benadering om eenvoudiger te maken.


## <a name="next-steps"></a>Volgende stappen

* [Microservices: Een toepassing revolution mogelijk gemaakt door de cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/)
* [Azure Architecture Center: Microservices bouwen in Azure](https://docs.microsoft.com/azure/architecture/microservices/)
* [Aanbevolen procedures voor Azure Service Fabric-toepassing en cluster](service-fabric-best-practices-overview.md)
* [Overzicht van service Fabric-terminologie](service-fabric-technical-overview.md)

[Image1]: media/service-fabric-overview-microservices/monolithic-vs-micro.png
[Image2]: media/service-fabric-overview-microservices/statemonolithic-vs-micro.png
[Image3]: media/service-fabric-overview-microservices/microservices-migration.png
