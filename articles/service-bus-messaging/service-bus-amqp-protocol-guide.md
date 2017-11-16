---
title: AMQP 1.0 in Azure Service Bus en Event Hubs protocol handleiding | Microsoft Docs
description: Protocol-handleiding voor expressies en beschrijving van AMQP 1.0 in Azure Service Bus en Event Hubs
services: service-bus-messaging,event-hubs
documentationcenter: .net
author: clemensv
manager: timlt
editor: 
ms.assetid: d2d3d540-8760-426a-ad10-d5128ce0ae24
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/08/2017
ms.author: clemensv;hillaryc;sethm
ms.openlocfilehash: 4e1fa9db3b4801103069163c55a9b342a27d00ac
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="amqp-10-in-azure-service-bus-and-event-hubs-protocol-guide"></a>AMQP 1.0 in Azure Service Bus en Event Hubs-protocol-handleiding

Geavanceerde Message Queueing Protocol 1.0 is een gestandaardiseerde framing en transfer protocol voor het asynchroon, veilig en betrouwbaar berichten over te brengen tussen twee partijen. Dit is het primaire protocol van Azure Service Bus-berichtenservice en Azure Event Hubs. Beide services bieden ook ondersteuning voor HTTPS. De eigen SBMP protocol waarmee wordt ook ondersteund wordt door de AMQP geleidelijk stopgezet.

AMQP 1.0 is het resultaat van de samenwerking met brede bedrijfstak die bij elkaar gebracht middleware leveranciers, zoals Microsoft en Red Hat met veel messaging middleware gebruikers zoals JP Morgan Chase voor de branche financiële diensten. Het technische normalisatie-forum voor de AMQP-protocol en de extensie specificaties OASIS is en deze formele goedkeuring als een internationale standaard als ISO/IEC 19494 heeft bereikt.

## <a name="goals"></a>Doelstellingen

Dit artikel bevat een overzicht van de belangrijkste concepten van de AMQP 1.0-specificatie samen met een kleine set ontwerpspecificaties voor uitbreiding die momenteel nog in het technisch comité OASIS AMQP gewerkt wordt messaging kort en wordt uitgelegd hoe Azure Service Bus wordt geïmplementeerd en is gebaseerd op deze specificaties.

Het doel is voor ontwikkelaars met behulp van een bestaande AMQP 1.0-client-stack op elk platform te kunnen werken met Azure Service Bus via AMQP 1.0.

Algemene algemene AMQP 1.0-stacks, zoals Apache Proton of Lite AMQP.NET implementeren al alle kernprotocollen AMQP 1.0. Deze fundamentele gebaren zijn soms verpakt met een hoger niveau API; Apache Proton biedt zelfs twee, de imperatieve Messenger-API en de reactieve Reactor-API.

In de volgende bespreking nemen we aan dat het beheer van AMQP-verbindingen, sessies, en koppelingen en de verwerking van frame overdrachten en datatransportbesturing worden verwerkt door de respectieve stack (zoals Apache Proton-C) en hoeven niet veel eventuele specifieke aandacht van ontwikkelaars van toepassingen. We abstract wordt ervan uitgegaan dat het bestaan van een paar API primitieven zoals de mogelijkheid verbinding maken en te maken van een vorm van *afzender* en *ontvanger* abstractie objecten, die vervolgens zijn de enige vorm van `send()` en `receive()` bewerkingen, respectievelijk.

Wanneer u geavanceerde mogelijkheden van Azure Service Bus, zoals bericht bladeren of beheer van sessies, worden deze functies worden beschreven in AMQP voorwaarden, maar ook als de implementatie van een gelaagd pseudo boven deze aangenomen API abstractie op.

## <a name="what-is-amqp"></a>Wat is AMQP?

AMQP is een protocol framing en de overdracht. Framing betekent dat deze structuur biedt voor binaire gegevensstromen die in beide richtingen van een netwerkverbinding stromen. De structuur biedt begrenzing voor afzonderlijke blokken met gegevens, aangeroepen *frames*, om te worden uitgewisseld tussen de verbonden partijen. De capaciteit voor gegevensoverdracht Zorg ervoor dat beide communicerende partijen tot stand brengen een gedeelde kennis over wanneer frames worden overgedragen, en wanneer overdrachten worden beschouwd voltooid.

In tegenstelling tot eerdere verlopen conceptversies geproduceerd door de werkgroep AMQP die nog in gebruik door een enkele bericht beleggingsmakelaars, schrijven de werkgroep final en gestandaardiseerde AMQP 1.0-protocol niet voor de aanwezigheid van een bericht broker of een bepaalde topologie voor entiteiten in een bericht broker.

Het protocol kan worden gebruikt voor symmetrische peer-to-peer-communicatie voor interactie met bericht beleggingsmakelaars die ondersteuning bieden voor wachtrijen en entiteiten publiceren/abonneren, zoals Azure Service Bus biedt. Het kan ook worden gebruikt voor interactie met berichteninfrastructuur waar de interactie zijn andere reguliere wachtrijen, zoals het geval met Azure Event Hubs. Een Event Hub fungeert als een wachtrij wanneer gebeurtenissen worden verzonden, maar meer fungeert als een seriële opslagservice wanneer gebeurtenissen worden gelezen uit het lijkt erg op een tapestation. De client haalt een offset in de gegevensstroom beschikbaar en wordt vervolgens alle gebeurtenissen vanaf die verschuiving naar de meest recente beschikbare behandeld.

Het AMQP 1.0-protocol is ontworpen om te worden uitgebreid, waardoor verdere specificaties voor het verbeteren van de mogelijkheden ervan. De drie extensie-specificaties die in dit document besproken illustratie. Een binding-specificatie definieert voor communicatie via bestaande HTTPS/WebSockets infrastructuur waar de systeemeigen AMQP TCP-poorten configureren kan lastig zijn, hoe AMQP via WebSockets laag. De AMQP management-specificatie definieert voor interactie met de messaging-infrastructuur op aanvraag/antwoord wijze voor beheerdoeleinden of om geavanceerde functionaliteit te bieden, de vereiste basic interactie primitieven. Voor federatieve autorisatie model-integratie definieert de AMQP claims-beveiliging op basis van-specificatie hoe om te koppelen en autorisatie-tokens die zijn gekoppeld aan de koppelingen te vernieuwen.

## <a name="basic-amqp-scenarios"></a>Basic AMQP-scenario 's

Deze sectie wordt uitgelegd dat het basisgebruik van AMQP 1.0 met Azure Service Bus, waaronder verbindingen, sessies en koppelingen maken en berichten naar en van Service Bus-entiteiten zoals wachtrijen, onderwerpen en abonnementen over te brengen.

De gezaghebbende bron voor meer informatie over de werking van AMQP is de AMQP 1.0-specificatie, maar de specificatie als richtlijn voor implementatie nauwkeurig en niet om te leren van het protocol is geschreven. Deze sectie richt zich op de introductie van zoveel terminologie zo nodig voor het beschrijven hoe Service Bus maakt gebruik van AMQP 1.0. Raadpleeg voor een uitgebreidere Inleiding tot AMQP, evenals een uitgebreidere bespreking van AMQP 1.0, [deze video maatregel][this video course].

### <a name="connections-and-sessions"></a>Verbindingen en sessies

AMQP roept de communicerende programma's *containers*; deze bevatten *knooppunten*, die de communicerende entiteiten binnen deze containers zijn. Een wachtrij kan deze een knooppunt zijn. AMQP kunt voor multiplex, zodat één verbinding kan worden gebruikt voor veel communicatiepaden tussen knooppunten. de toepassingsclient van een kan bijvoorbeeld gelijktijdig ontvangen uit een wachtrij en naar een andere wachtrij verzenden via dezelfde netwerkverbinding.

![][1]

De netwerkverbinding is dus verankerd voor de container. Deze is gestart door de container in de rol van de client een uitgaande TCP-socket-verbinding naar een container maken in de rol van de ontvanger, die luistert naar en binnenkomende TCP-verbindingen accepteert. De handshake van verbinding bevat de protocolversie, declareren of onderhandelen over het gebruik van Transport Level Security (TLS/SSL) en een handshake-verificatie/autorisatie voor de verbinding scope die is gebaseerd op SASL onderhandelen.

Azure Service Bus vereist het gebruik van TLS te allen tijde. Het ondersteunt verbindingen via TCP-poort 5671, waarbij de TCP-verbinding heen eerst met TLS weergegeven wordt voordat het in de handshake AMQP-protocol en ondersteunt ook verbindingen via TCP-poort 5672 waarbij de server direct biedt een upgrade van een verplichte van verbinding met TLS met het model AMQP voorgeschreven. De binding AMQP WebSockets maakt een tunnel via TCP-poort 443, die vervolgens gelijk is aan AMQP 5671 verbindingen.

Na het instellen van de verbinding en TLS, biedt Service Bus twee SASL mechanisme opties:

* SASL zonder opmaak wordt meestal gebruikt voor het doorgeven van referenties van gebruikersnaam en wachtwoord op een server. Service Bus heeft geen accounts, maar met de naam [gedeelde toegang beveiligingsregels](service-bus-sas.md), die rechten verlenen en zijn gekoppeld aan een sleutel. De naam van een regel wordt gebruikt als de gebruikersnaam en de sleutel (zoals base64-tekst gecodeerde) als het wachtwoord wordt gebruikt. De rechten die zijn gekoppeld aan de gekozen regel van toepassing de bewerkingen op de verbinding toegestaan.
* ANONIEME SASL wordt gebruikt voor het omzeilen van SASL autorisatie wanneer de client wil gebruiken de claims-beveiliging op basis van-(CBS)-model, die verderop wordt beschreven. Met deze optie wordt worden een clientverbinding anoniem ingesteld voor een korte periode gedurende welke de client kan alleen communiceren met het eindpunt CBS en de handshake CBS moet voltooien.

Nadat de transportverbinding tot stand is gebracht, de containers declareert u de maximale framegrootte die ze bereid bent te verwerken en na een inactiviteit ze hebt eenzijdig verbreken als er geen activiteit op de verbinding.

Tevens declareren hoeveel gelijktijdige kanalen worden ondersteund. Een kanaal is een overdracht unidirectioneel, uitgaande, virtuele pad boven op de verbinding. Een sessie heeft een kanaal van elk van de onderling verbonden containers naar een bidirectionele communicatie-pad.

Sessies hebben een model voor het toegangsbeheer op basis van het venster stroom; Wanneer een sessie wordt gemaakt, elke partij wordt aangegeven hoeveel frames is wilt accepteren in het venster ontvangen. Als de overgebrachte frames opvulling venster en overdrachten stoppen wanneer het venster vol is en totdat het venster opgehaald opnieuw worden ingesteld of met behulp van uitgebreide-frames exchange partijen de *stroom performative* (*performative* is de term AMQP voor protocolniveau gebaren uitgewisseld tussen de twee partijen).

Dit model op basis van een venster is grofweg overeen met het TCP-concept van datatransportbesturing op basis van het venster, maar op het niveau van de sessie in de socket. Het protocol concept van meerdere gelijktijdige sessies waardoor ervoor te zorgen dat hoge prioriteit verkeer kan worden meest voorbij beperkte normale verkeer, zoals op een snelle lane weg.

Azure Service Bus gebruikt momenteel precies één sessie voor elke verbinding. De Service Bus-frame-maximumgrootte is 262.144 bytes (256 kB) voor Service Bus Standard en Event Hubs. Het is 1.048.576 (1 MB) voor Service Bus Premium. Service Bus een bepaalde sessie-niveau bandbreedteregeling windows niet afdwingen, maar Hiermee stelt u het venster regelmatig als onderdeel van de link-level datatransportbesturing (Zie [in de volgende sectie](#links)).

Verbindingen, kanalen en sessies zijn kortstondige. Als de onderliggende verbinding worden samengevouwen, verbindingen, TLS-tunnel, SASL verificatiecontext en sessies moeten opnieuw worden gemaakt.

### <a name="links"></a>Koppelingen

AMQP draagt berichten via koppelingen. Een koppeling is een communicatiepad gemaakt via een sessie waarmee overgebracht berichten in één richting; de status van-onderhandeling overdracht is via de koppeling en twee richtingen tussen de verbonden partijen.

![][2]

Koppelingen kunnen worden gemaakt op de container op elk gewenst moment en via een bestaande sessie, waardoor AMQP verschilt van de vele andere protocollen, met inbegrip van HTTP- en MQTT, waarbij de initialisatie van overdrachten en pad van de overdracht een exclusieve bevoegdheid van de partij die het maken van de socketverbinding is.

De container voor het initiëren van de koppeling wordt gevraagd de tegengestelde container een koppeling te accepteren en een rol van de afzender of ontvanger worden gekozen. Daarom een container maken Unidirectioneel kunt starten of bidirectionele communicatiepaden, met de laatste gemodelleerd als paren van koppelingen.

Koppelingen zijn met de naam en die zijn gekoppeld aan knooppunten. Zoals vermeld in het begin, zijn knooppunten de communicerende entiteiten in een container.

In de Service Bus is een knooppunt rechtstreeks gelijk is aan een wachtrij, een onderwerp, een abonnement of een wachtrij voor onbestelbare subwachtrij van een wachtrij of abonnement. De naam van het knooppunt gebruikt in AMQP is daarom de relatieve naam van de entiteit binnen de Service Bus-naamruimte. Als de naam van een wachtrij **rapportberichten**, is ook de naam van de AMQP-knooppunt. Een onderwerpabonnement volgt conventie voor de HTTP-API door wordt gesorteerd in een verzameling van de resource 'abonnementen' en dus een abonnement **sub** of een onderwerp **mytopic** heeft de naam van het AMQP-knooppunt **abonnementen mytopic subitems**.

De client ook vereist voor het gebruik van een lokale knooppuntnaam voor het maken van koppelingen; Service Bus is geen richtlijnen over de knooppuntnamen en komt niet interpreteren. AMQP 1.0-client stacks in het algemeen gebruikt u een schema om te verzekeren dat dat deze tijdelijke knooppuntnamen uniek binnen het bereik van de client zijn.

### <a name="transfers"></a>Overdrachten

Zodra een koppeling tot stand is gebracht, kunnen berichten worden verzonden via deze koppeling. In AMQP, een overdracht wordt uitgevoerd met een gebaar van de expliciete protocol (de *overdracht* performative) die een bericht verplaatst van afzender naar ontvanger via een koppeling. Een overdracht is voltooid als het 'betaald', wat betekent dat beide partijen een gedeelde kennis van het resultaat van deze overdracht hebt gemaakt.

![][3]

In de meest eenvoudige geval heeft kiezen de afzender voor het verzenden van berichten 'vooraf afgewikkeld', wat betekent dat de client niet geïnteresseerd in de resultaten is en de ontvanger geen opmerkingen of vragen hebt over de uitkomst van de bewerking biedt. Deze modus wordt ondersteund door Service Bus op het niveau van de AMQP-protocol, maar niet worden weergegeven in een van de client-API.

Het normale geval is dat berichten worden verzonden niet-vereffende en de ontvanger vervolgens acceptatie- of afwijzing met behulp van betekent de *toestand* performative. Afwijzing treedt op wanneer de ontvanger het bericht om een bepaalde reden niet accepteren en het bericht afwijzing bevat informatie over de reden een fout structuur gedefinieerd door AMQP is. Als berichten zijn afgewezen als gevolg van interne fouten binnen de Service Bus, stuurt de service extra informatie in die structuur die kan worden gebruikt voor het ontwikkelen van hints diagnostische gegevens naar het ondersteuningspersoneel als u ondersteuningsaanvragen indient. U leert later meer informatie over fouten.

Een speciale vorm van afwijzing is de *uitgebracht* staat, waarmee wordt aangegeven dat de ontvanger heeft geen technisch bezwaar voor de overdracht, maar ook geen belang bij de overdracht vereffenen. Deze aanvraag bestaat, bijvoorbeeld wanneer een bericht bij een Service Bus-client wordt afgeleverd en de client kiest voor het bericht 'verlaten' omdat de hoeveelheid werk die voortvloeien uit het verwerken van het bericht; kan niet worden uitgevoerd de aflevering van het bericht is niet op fouten. Een variant van die status is de *gewijzigd* status heeft waarmee wijzigingen in het bericht bij de release. Die staat, wordt op dit moment niet door Service Bus gebruikt.

De AMQP 1.0-specificatie definieert een verdere disposition status aangeroepen *ontvangen*, die specifiek helpt voor het afhandelen koppeling herstel. Herstel van de koppeling kunt endinfrastructuur van de status van een koppeling en in behandeling zijnde leveringen boven op een nieuwe verbinding en -sessie wanneer de eerdere verbinding en sessie verloren gegaan zijn.

Service Bus biedt geen ondersteuning voor herstel van de koppeling; Als de client de verbinding met Service Bus met een niet-vereffende bericht verbroken transfer in behandeling, deze overdracht bericht is verloren gegaan en de client moet verbinding opnieuw tot stand brengen met de koppeling en probeer de overdracht.

Als zodanig Service Bus en Event Hubs ondersteunen 'ten minste eenmaal' overdrachten waarbij de afzender zeker van kunt zijn voor het bericht is opgeslagen en geaccepteerd, maar bieden geen ondersteuning voor 'eenmalige' overdrachten op het AMQP-niveau, waarbij het systeem probeert te herstellen van de koppeling en doorgaan om te onderhandelen over de levering van status om te voorkomen, duplicatie van het bericht is verzonden.

Om te compenseren voor eventuele dubbele verzendt, Service Bus biedt ondersteuning voor detectie van duplicaten als een optionele functie van wachtrijen en onderwerpen. Detectie van duplicaten registreert de bericht-id's van alle inkomende berichten tijdens een periode die door de gebruiker gedefinieerde en vervolgens de achtergrond verwijdert alle berichten die met dezelfde bericht-id's die dezelfde databaseprestaties.

### <a name="flow-control"></a>Datatransportbesturing

Naast het model van de sessie-niveau stroom besturingselement dat eerder is besproken, heeft elke koppeling een eigen stroom-model voor toegangsbeheer. Sessie-level datatransportbesturing voorkomt dat de container met te veel frames aan verwerken zodra link-level datatransportbesturing plaatst de toepassing die verantwoordelijk is voor het aantal berichten wil van een koppeling verwerken en wanneer.

![][4]

Op een koppeling overdrachten kunnen alleen gebeuren wanneer de afzender voldoende heeft *koppelen tegoed*. Link-tegoed is een teller die is ingesteld door de ontvanger met behulp van de *stroom* performative, die is afgestemd op een koppeling. Wanneer de afzender link-tegoed toegewezen is, probeert deze die credit gebruikt door het leveren van berichten. Elke message delivery verlaagt de koppeling van de resterende tegoed van 1. Wanneer de link-tegoed is gebruikt, niet meer leveringen.

Wanneer Service Bus in de rol van de ontvanger, biedt het onmiddellijk de afzender ruime link-tegoed, zodat berichten kunnen direct worden verzonden. Link-tegoed is gebruikt, Service Bus af en toe stuurt een *stroom* performative naar de afzender het saldo koppeling bijwerken.

In de rol van de afzender verzendt Service Bus berichten gebruiken om eventuele uitstaande link-tegoed.

Een aanroep 'ontvangen' op de API-niveau wordt omgezet in een *stroom* performative wordt verzonden naar Service Bus door de client en Service Bus die credit verbruikt door duurt het eerste beschikbare, niet-vergrendelde bericht uit de wachtrij, vergrendelt en overdragen. Als er geen bericht direct beschikbaar is voor de levering van openstaande tegoed door een koppeling tot stand gebracht met dat bepaalde entiteit opgenomen in de volgorde van de aankomst blijft en berichten worden vergrendeld en zodra deze beschikbaar voor gebruik door openstaande tegoed overgedragen.

De vergrendeling voor een bericht wordt uitgebracht als de overdracht naar een van de terminal statussen betaald *geaccepteerd*, *afgewezen*, of *uitgebracht*. Het bericht is verwijderd uit de Service Bus wanneer de definitieve status *geaccepteerd*. Het blijft in Service Bus en is geleverd aan de volgende ontvanger wanneer de overdracht een van de andere statussen bereikt. Het bericht verplaatst Service Bus automatisch naar de entiteit wachtrij voor onbestelbare berichten bij de levering van het maximum aantal toegestaan voor de entiteit vanwege herhaalde weigeringen of releases.

Ondanks dat de Service Bus-API's die een optie vandaag de dag niet rechtstreeks blootstellen, een lager niveau AMQP protocol client gebruikt, het model link-tegoed voor de interactie 'pull-style' van het uitgeven van één eenheid van het krediet voor elke aanvraag ontvangen in een model 'push-style' door uitgifte van een groot aantal link-tegoed inschakelen en vervolgens ontvangen berichten zodra deze beschikbaar zijn zonder verdere tussenkomst. Push wordt ondersteund door de [MessagingFactory.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PrefetchCount) of [MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount) eigenschapsinstellingen. Wanneer ze niet nul zijn, wordt het in de AMQP-client gebruikt als het link-tegoed.

In deze context is het belangrijk te weten dat de klok voor de vervaldatum van de vergrendeling van het bericht in de entiteit wordt gestart wanneer het bericht is genomen van de entiteit niet wanneer het bericht op de kabel wordt geplaatst. Wanneer de client aangeeft dat de gereedheid voor het ontvangen van berichten door uitgifte van link-tegoed, daarom wordt verwacht worden actief binnenhalen van berichten via het netwerk en klaar om deze te verwerken. Anders kan de vergrendeling bericht verlopen voordat het bericht wordt ook geleverd. Het gebruik van link-tegoed datatransportbesturing moet rechtstreeks onmiddellijke gereed om te gaan met de beschikbare berichten verzonden naar de ontvanger overeenkomen.

Kortom bieden de volgende secties een schematische overzicht van de performative stroom tijdens verschillende API interacties. Elke sectie beschrijft een andere logische bewerking. Sommige van deze interacties zijn 'vertraagde,' wat betekent dat ze kunnen alleen worden uitgevoerd wanneer dat nodig. Maken de afzender van een bericht kan niet leiden tot een netwerk interactie totdat het eerste bericht is verzonden of aangevraagd.

De pijlen in de volgende tabel stroomrichting de performative.

#### <a name="create-message-receiver"></a>Bericht ontvanger maken

| Client | Service Bus |
| --- | --- |
| --> () koppelen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke ingang}<br/>rol =**ontvanger**,<br/>bron = {entiteitnaam}<br/>doel = {client koppeling id}<br/>) |Client die is gekoppeld aan een entiteit als ontvanger |
| Service Bus-antwoorden die het einde van de koppeling koppelen |<--(koppelen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke ingang}<br/>rol =**afzender**,<br/>bron = {entiteitnaam}<br/>doel = {client koppeling id}<br/>) |

#### <a name="create-message-sender"></a>Maken van de afzender

| Client | Service Bus |
| --- | --- |
| --> () koppelen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke ingang}<br/>rol =**afzender**,<br/>bron = {client koppeling id}<br/>doel = {naam van de entiteit}<br/>) |Er is geen actie |
| Er is geen actie |<--(koppelen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke ingang}<br/>rol =**ontvanger**,<br/>bron = {client koppeling id}<br/>doel = {naam van de entiteit}<br/>) |

#### <a name="create-message-sender-error"></a>Maken van de afzender van bericht (fout)

| Client | Service Bus |
| --- | --- |
| --> () koppelen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke ingang}<br/>rol =**afzender**,<br/>bron = {client koppeling id}<br/>doel = {naam van de entiteit}<br/>) |Er is geen actie |
| Er is geen actie |<--(koppelen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke ingang}<br/>rol =**ontvanger**,<br/>bron = null,<br/>doel = null<br/>)<br/><br/><--loskoppelen ()<br/>verwerken = {numerieke ingang}<br/>gesloten =**true**,<br/>Fout = {foutgegevens}<br/>) |

#### <a name="close-message-receiversender"></a>Afsluitbericht ontvanger/afzender

| Client | Service Bus |
| --- | --- |
| --> loskoppelen ()<br/>verwerken = {numerieke ingang}<br/>gesloten =**true**<br/>) |Er is geen actie |
| Er is geen actie |<--loskoppelen ()<br/>verwerken = {numerieke ingang}<br/>gesloten =**true**<br/>) |

#### <a name="send-success"></a>Verzenden (geslaagd)

| Client | Service Bus |
| --- | --- |
| overdracht (--><br/>levering-id = {numerieke ingang}<br/>levering-tag = {binaire ingang}<br/>afgewikkeld =**false**,, meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>) |Er is geen actie |
| Er is geen actie |<--toestand (<br/>rol ontvanger =<br/>eerst = {levering van id}<br/>laatste = {levering van id}<br/>afgewikkeld =**true**,<br/>status =**geaccepteerd**<br/>) |

#### <a name="send-error"></a>Verzonden (fout)

| Client | Service Bus |
| --- | --- |
| overdracht (--><br/>levering-id = {numerieke ingang}<br/>levering-tag = {binaire ingang}<br/>afgewikkeld =**false**,, meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>) |Er is geen actie |
| Er is geen actie |<--toestand (<br/>rol ontvanger =<br/>eerst = {levering van id}<br/>laatste = {levering van id}<br/>afgewikkeld =**true**,<br/>status =**afgewezen**()<br/>Fout = {foutgegevens}<br/>)<br/>) |

#### <a name="receive"></a>Ontvangen

| Client | Service Bus |
| --- | --- |
| stroom (--><br/>link-tegoed = 1<br/>) |Er is geen actie |
| Er is geen actie |< transfer ()<br/>levering-id = {numerieke ingang}<br/>levering-tag = {binaire ingang}<br/>afgewikkeld =**false**,<br/>meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>) |
| toestand (--><br/>rol =**ontvanger**,<br/>eerst = {levering van id}<br/>laatste = {levering van id}<br/>afgewikkeld =**true**,<br/>status =**geaccepteerd**<br/>) |Er is geen actie |

#### <a name="multi-message-receive"></a>Met meerdere berichten ontvangen

| Client | Service Bus |
| --- | --- |
| stroom (--><br/>link-tegoed = 3<br/>) |Er is geen actie |
| Er is geen actie |< transfer ()<br/>levering-id = {numerieke ingang}<br/>levering-tag = {binaire ingang}<br/>afgewikkeld =**false**,<br/>meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>) |
| Er is geen actie |< transfer ()<br/>levering-id = {numerieke ingang + 1},<br/>levering-tag = {binaire ingang}<br/>afgewikkeld =**false**,<br/>meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>) |
| Er is geen actie |< transfer ()<br/>levering-id = {numerieke ingang + 2},<br/>levering-tag = {binaire ingang}<br/>afgewikkeld =**false**,<br/>meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>) |
| toestand (--><br/>rol ontvanger =<br/>eerst = {levering van id}<br/>laatste = {-id voor levering + 2}<br/>afgewikkeld =**true**,<br/>status =**geaccepteerd**<br/>) |Er is geen actie |

### <a name="messages"></a>Berichten

De volgende secties wordt uitgelegd welke eigenschappen van de standaard AMQP bericht secties worden gebruikt door de Service Bus en hoe deze worden toegewezen aan de Service Bus-API-set.

#### <a name="header"></a>koptekst

| Veldnaam | Gebruik | API-naam |
| --- | --- | --- |
| duurzame |- |- |
| Prioriteit |- |- |
| TTL |Levensduur voor dit bericht |[TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive) |
| eerste verwerver |- |- |
| levering tellen |- |[DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) |

#### <a name="properties"></a>properties

| Veldnaam | Gebruik | API-naam |
| --- | --- | --- |
| bericht-id |ID voor dit bericht toepassingsspecifieke, vrije vorm. Gebruikt voor detectie van duplicaten. |[MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) |
| gebruikers-id |Toepassingen gedefinieerde gebruiker-id, niet worden geïnterpreteerd door Service Bus. |Niet toegankelijk is via Service Bus-API. |
| tot |Bestemming toepassingen gedefinieerde id, niet worden geïnterpreteerd door Service Bus. |[Aan](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_To) |
| Onderwerp |Bericht toepassingsspecifieke doel-id, niet worden geïnterpreteerd door Service Bus. |[Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) |
| antwoord aan |Antwoord-path toepassingsspecifieke indicator, niet worden geïnterpreteerd door Service Bus. |[ReplyTo](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ReplyTo) |
| correlatie-id |Toepassingsspecifieke correlatie-id, niet worden geïnterpreteerd door Service Bus. |[CorrelationId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_CorrelationId) |
| type inhoud |Toepassingsspecifieke inhoudstype indicator voor de hoofdtekst, niet worden geïnterpreteerd door Service Bus. |[ContentType](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ContentType) |
| codering van inhoud |Toepassingsspecifieke indicator codering van inhoud voor de hoofdtekst, niet worden geïnterpreteerd door Service Bus. |Niet toegankelijk is via Service Bus-API. |
| absolute verlooptijdstip |Verklaart op welke absolute instant het bericht verloopt. Genegeerd op invoer (koptekst TTL is waargenomen), de gezaghebbende op uitvoer. |[ExpiresAtUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ExpiresAtUtc) |
| Aanmaaktijd |Verklaart op dat moment het bericht is gemaakt. Niet gebruikt door de Service Bus |Niet toegankelijk is via Service Bus-API. |
| groep-id |Toepassingen gedefinieerde id voor een bijbehorende set berichten. Gebruikt voor Service Bus-sessies. |[Sessie-id](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) |
| volgorde van de groep |De teller het relatieve volgnummer van het bericht in een sessie te identificeren. Door de Servicebus genegeerd. |Niet toegankelijk is via Service Bus-API. |
| antwoord-naar-groep-id |- |[ReplyToSessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ReplyToSessionId) |

## <a name="advanced-service-bus-capabilities"></a>Geavanceerde mogelijkheden voor Service Bus

Deze sectie bevat informatie over geavanceerde mogelijkheden van Azure Service Bus die zijn gebaseerd op concept extensies AMQP, ontwikkeling in het technisch comité OASIS voor AMQP. Service Bus implementeert de nieuwste versies van deze concepten en wijzigingen die zijn geïntroduceerd als deze concepten worden standaard status bereikt aanneemt.

> [!NOTE]
> Service Bus-berichtenservice geavanceerde bewerkingen worden ondersteund door middel van een aanvraag/antwoord-patroon. De details van deze bewerkingen worden beschreven in het artikel [AMQP 1.0 in Service Bus: aanvraag-antwoord-bewerkingen](service-bus-amqp-request-response.md).
> 
> 

### <a name="amqp-management"></a>AMQP-management

De specificatie AMQP-management is de eerste dag van het concept-uitbreidingen die worden beschreven in dit artikel. Deze specificatie definieert een reeks protocollagen boven op het AMQP-protocol die management interactie met de messaging-infrastructuur via AMQP toestaan. De specificatie definieert algemene bewerkingen zoals *maken*, *lezen*, *bijwerken*, en *verwijderen* voor het beheren van entiteiten in een berichteninfrastructuur en een reeks querybewerkingen.

Alle deze gebaren vereisen een aanvraag/antwoord-interactie tussen de client en de messaging-infrastructuur en daarom de specificatie definieert hoe interactie patroon boven op AMQP model: de client verbinding maakt met de messaging-infrastructuur, initieert een sessie en maakt vervolgens een combinatie van koppelingen. De client fungeert als de afzender en op de andere het fungeert als ontvanger, waardoor een paar koppelingen die als een bidirectioneel kanaal optreden kan op één koppeling.

| Logische-bewerking | Client | Service Bus |
| --- | --- | --- |
| Aanvraag antwoord pad maken |--> () koppelen<br/>naam = {*naam van de koppeling*},<br/>verwerken = {*numerieke ingang*},<br/>rol =**afzender**,<br/>bron =**null**,<br/>target = 'management myentity / $'<br/>) |Er is geen actie |
| Aanvraag antwoord pad maken |Er is geen actie |\<--(koppelen<br/>naam = {*naam van de koppeling*},<br/>verwerken = {*numerieke ingang*},<br/>rol =**ontvanger**,<br/>bron = null,<br/>TARGET = "myentity"<br/>) |
| Aanvraag antwoord pad maken |--> () koppelen<br/>naam = {*naam van de koppeling*},<br/>verwerken = {*numerieke ingang*},<br/>rol =**ontvanger**,<br/>bron = 'myentity / $management',<br/>target = 'id myclient$ '<br/>) | |
| Aanvraag antwoord pad maken |Er is geen actie |\<--(koppelen<br/>naam = {*naam van de koppeling*},<br/>verwerken = {*numerieke ingang*},<br/>rol =**afzender**,<br/>bron = "myentity"<br/>target = 'id myclient$ '<br/>) |

Met deze combinatie van koppelingen in plaats, de implementatie van de aanvraag/antwoord is eenvoudig: een verzoek is een bericht verzonden naar een entiteit binnen de messaging-infrastructuur die werkt met dit patroon. In dat aanvraagbericht de *antwoordadres* veld in de *eigenschappen* sectie is ingesteld op de *doel* id voor de koppeling waarop u het antwoord leveren. De entiteit voor de verwerking van de aanvraag wordt verwerkt en levert u vervolgens het antwoord op de koppeling waarvan *doel* id komt overeen met de aangegeven *antwoordadres* id.

Het patroon is natuurlijk vereist dat de client-container en de gegenereerde client-id voor de doel-antwoord uniek zijn voor alle clients en, uit veiligheidsoverwegingen ook moeilijk te voorspellen.

De berichten worden uitgewisseld gebruikt voor het management-protocol en voor alle andere protocollen die gebruikmaken van hetzelfde patroon gebeuren op toepassingsniveau; ze geen nieuwe AMQP-protocolniveau gebaren gedefinieerd. Dat is opzettelijk, zodat toepassingen kunnen profiteren direct met de volgende extensies met compatibele AMQP 1.0-stacks.

Service Bus momenteel implementeert niet de belangrijkste functies van de management-specificatie, maar de aanvraag/antwoord-patroon gedefinieerd door de management-specificatie is fundamenteel voor de claims-beveiliging op basis van-functie en voor bijna alle van de geavanceerde mogelijkheden die in de volgende secties worden besproken.

### <a name="claims-based-authorization"></a>Claims gebaseerde verificatie

Het concept van de specificatie AMQP Claims-gebaseerde verificatie (CBS) bouwt voort op het patroon management specificatie aanvraag/antwoord en een beschrijving van een algemene model voor het gebruik van federatieve beveiligingstokens met AMQP.

Het beveiligingsmodel van AMQP beschreven in de introductie van standaard is gebaseerd op SASL en kan worden geïntegreerd met de handshake van de AMQP-verbinding. Met behulp van SASL heeft als voordeel dat hiermee een uitbreidbare model waarvoor een reeks mechanismen zijn gedefinieerd uit elk protocol dat voorheen op SASL leans kan profiteren. Onder deze mechanismen zijn 'PLATTE' voor de overdracht van gebruikersnamen en wachtwoorden, 'Extern' verbinding maken met TLS-level security, 'Anoniem' aan het ontbreken van expliciete verificatie/autorisatie en tal van aanvullende mechanismen waarmee het doorgeven van verificatie en/of autorisatiereferenties of tokens express.

De AMQP SASL integratie heeft twee nadelen:

* Alle referenties en tokens zijn afgestemd op de verbinding. Messaging-infrastructuur wilt gedifferentieerde toegangsbeheer op basis van het per entiteit; bijvoorbeeld, zodat de drager van een token te verzenden naar een wachtrij, maar niet naar de wachtrij B. Met de autorisatiecontext verankerd op de verbinding, is het niet mogelijk slechts één verbinding en nog gebruiken verschillende toegangstokens voor wachtrij A en B. wachtrij
* Toegangstokens zijn doorgaans alleen geldig voor een beperkte periode. Deze geldigheid vereist dat gebruikers periodiek opnieuw ophalen tokens en biedt de mogelijkheid voor de uitgever van het beveiligingstoken te weigeren uitgeven van een nieuw token als de gebruiker machtigingen zijn gewijzigd. AMQP verbindingen mogelijk langere tijd duren. Het model SASL biedt alleen een kans om in te stellen van een token tijdens de verbinding, wat dat de messaging-infrastructuur is betekent verbreken van de client wanneer het token is verlopen of deze moeten accepteren van het risico op permanente communicatie met een client die is toegangsrechten kunnen in de tussentijd is ingetrokken.

De AMQP CBS specificatie, geïmplementeerd door de Service Bus kunt een elegante tijdelijke oplossing voor beide die problemen: kunt u een client toegangstokens koppelen aan elk knooppunt, en om bij te werken die tokens voordat ze verlopen, zonder dat de berichtenstroom worden onderbroken.

CBS definieert een virtuele management-knooppunt, met de naam *$cbs*, te worden verstrekt door de messaging-infrastructuur. Het beheerknooppunt accepteert tokens namens een andere knooppunten in de messaging-infrastructuur.

Een gebaar van het protocol is een aanvraag/antwoord-uitwisseling zoals gedefinieerd door de management-specificatie. Dat betekent dat de client tot stand brengt een combinatie van koppelingen met de *$cbs* knooppunt en stuurt vervolgens een aanvraag op de uitgaande koppeling en wordt er gewacht totdat het antwoord op de inkomende koppeling.

Het aanvraagbericht heeft de volgende toepassingseigenschappen:

| Sleutel | Optioneel | Waardetype | De inhoud |
| --- | --- | --- | --- |
| bewerking |Nee |Tekenreeks |**Put-token** |
| type |Nee |Tekenreeks |Het type van het token wordt geplaatst. |
| naam |Nee |Tekenreeks |De 'doelgroep', die het token van toepassing is. |
| vervaldatum |Ja |tijdstempel |De verlooptijd van het token. |

De *naam* eigenschap identificeert de entiteit waaraan het token gekoppeld worden moet. Dit is het pad naar de wachtrij of onderwerp/abonnement in de Service Bus. De *type* eigenschap identificeert het tokentype:

| Type token | Beschrijving van token | Hoofdtekst van Type | Opmerkingen |
| --- | --- | --- | --- |
| amqp:jwt |JSON Web Token (JWT) |AMQP-waarde (tekenreeks) |Nog niet beschikbaar. |
| amqp:SWT |Eenvoudige Web Token (SWT) |AMQP-waarde (tekenreeks) |Alleen ondersteund voor SWT tokens die zijn uitgegeven door AAD/ACS |
| servicebus.Windows.NET:sastoken |Service Bus SAS-Token |AMQP-waarde (tekenreeks) |- |

Tokens verleent rechten. Service Bus op de hoogte van de drie fundamentele rechten: 'Verzenden' kunnen worden verzonden, 'Luisteren' kunt ontvangen en manipuleren entiteiten 'Beheren' maakt. Deze rechten bevatten SWT tokens die zijn uitgegeven door AAD/ACS expliciet als claims. Service Bus SAS-tokens Raadpleeg regels die zijn geconfigureerd op de naamruimte of entiteit en deze regels zijn geconfigureerd met rechten. Ondertekening van het token met de sleutel gekoppeld aan de regel die maakt de token snelle de respectieve rechten dus. Het token dat is gekoppeld aan een entiteit met *put-token* kan de verbonden client om te communiceren met de entiteit per de token rechten. Een koppeling waarop de client neemt de *afzender* rol vereist de 'verzenden' recht; op de *ontvanger* rol de 'luisteren' rechts vereist.

Het antwoord heeft de volgende *eigenschappen van application* waarden

| Sleutel | Optioneel | Waardetype | De inhoud |
| --- | --- | --- | --- |
| statuscode in |Nee |int |HTTP-antwoordcode **[RFC2616]**. |
| Beschrijving van status |Ja |Tekenreeks |Beschrijving van de status. |

De client kan aanroepen *put-token* herhaaldelijk en voor elke entiteit in de messaging-infrastructuur. De tokens zijn binnen het bereik van de huidige client en de huidige verbinding, wat betekent dat de server tot terugkerende tokens zakt wanneer de verbinding uitvalt verankerd.

De huidige Service Bus-implementatie kan alleen CB's in combinatie met de methode SASL 'Anoniem'. SSL/TLS-verbinding moet altijd aanwezig zijn voordat de SASL-handshake.

Het mechanisme voor anonieme moet daarom worden ondersteund door de gekozen AMQP 1.0-client. Anonieme toegang betekent dat de handshake van de eerste verbinding, zoals het maken van de eerste sessie gebeurt zonder Service Bus weten wie de verbinding wordt gemaakt.

Wanneer de verbinding en de sessie is ingesteld, koppelen van de koppelingen naar de *$cbs* knooppunt en het verzenden van de *put-token* aanvragen zijn de enige toegestane bewerkingen. Een geldig token moet worden ingesteld met een *put-token* aanvraag voor een bepaald knooppunt entiteit binnen 20 seconden nadat de verbinding tot stand is gebracht, anders is de verbinding wordt eenzijdig verbroken door Service Bus.

De client vervolgens verantwoordelijk is voor het bijhouden van verlopen van het token. Wanneer een token is verlopen, verwijdert Service Bus zo spoedig mogelijk alle koppelingen voor de verbinding met de betreffende entiteit. Om dit te voorkomen, kunt de client de token voor het knooppunt vervangen door een nieuwe op elk gewenst moment via de virtuele *$cbs* knooppunt met dezelfde *put-token* gebaar en het ophalen van zonder heeft op het gebied de nettolading-verkeer dat op andere koppelingen loopt.

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over AMQP, gaat u naar de volgende koppelingen:

* [Service Bus AMQP-overzicht]
* [AMQP 1.0-ondersteuning voor Service Bus-wachtrijen en onderwerpen gepartitioneerd]
* [AMQP in WindowsServer-Servicebus]

[this video course]: https://www.youtube.com/playlist?list=PLmE4bZU0qx-wAP02i0I7PJWvDWoCytEjD
[1]: ./media/service-bus-amqp-protocol-guide/amqp1.png
[2]: ./media/service-bus-amqp-protocol-guide/amqp2.png
[3]: ./media/service-bus-amqp-protocol-guide/amqp3.png
[4]: ./media/service-bus-amqp-protocol-guide/amqp4.png

[Service Bus AMQP-overzicht]: service-bus-amqp-overview.md
[AMQP 1.0-ondersteuning voor Service Bus-wachtrijen en onderwerpen gepartitioneerd]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP in WindowsServer-Servicebus]: https://msdn.microsoft.com/library/dn574799.aspx
