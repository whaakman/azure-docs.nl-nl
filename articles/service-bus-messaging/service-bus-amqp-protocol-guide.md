---
title: AMQP 1.0 in Azure Service Bus en Event Hubs-protocolhandleiding | Microsoft Docs
description: Expressies en beschrijving van AMQP 1.0 in Azure Service Bus en Event Hubs-protocolhandleiding
services: service-bus-messaging,event-hubs
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: d2d3d540-8760-426a-ad10-d5128ce0ae24
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: cf06be778fb1bd251b55adcc503db63a2adf3f8b
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55197922"
---
# <a name="amqp-10-in-azure-service-bus-and-event-hubs-protocol-guide"></a>AMQP 1.0 in Azure Service Bus en Event Hubs-protocolhandleiding

Het Advanced Message Queueing Protocol 1.0 is een gestandaardiseerde framing en transfer protocol voor het asynchroon, veilig en betrouwbaar berichten overdragen tussen twee partijen. Dit is het primaire protocol van de Azure Service Bus-berichten en Azure Event Hubs. Beide services bieden ook ondersteuning voor HTTPS. Het eigen SBMP-protocol dat wordt ook ondersteund is geleidelijk vervangen door AMQP.

AMQP 1.0 is het resultaat van samenwerking met brede branche die middleware leveranciers, zoals Microsoft en Red Hat, met veel messaging middleware gebruikers zoals JP Morgan Chase voor de financiële dienstverleningssector bij elkaar gebracht. Het technische normalisatie-forum voor de AMQP-protocol en de extensie specificaties OASIS is en deze formele goedkeuring als een internationale standaard als ISO/IEC 19494 heeft bereikt.

## <a name="goals"></a>Doelstellingen

In dit artikel bevat een overzicht van de belangrijkste concepten van de AMQP 1.0-specificatie samen met een kleine set conceptspecificaties-extensie die momenteel zijn worden voltooid in de technische Comité OASIS AMQP berichten kort en wordt uitgelegd hoe Azure Servicebus implementeert en bouwt voort op deze specificaties.

Het doel is voor iedere ontwikkelaar met behulp van een bestaande AMQP 1.0-client-stack op elk platform te kunnen werken met Azure Service Bus via AMQP 1.0.

Algemene voor algemeen gebruik AMQP 1.0-stacks, zoals Apache Proton of Lite AMQP.NET implementeren al alle kernprotocollen AMQP 1.0. Deze fundamentele gebaren zijn soms verpakt met een hoger niveau API; Apache Proton biedt ook twee, de imperatieve Messenger-API en de reactieve Reactor-API.

In het volgende onderwerp gaan we ervan uit dat het beheer van AMQP-verbindingen, sessies, en koppelingen en de verwerking van kader worden overgedragen en datatransportbesturing worden verwerkt door de respectieve stack (zoals Apache Proton-C) en hoeven niet te veel eventuele specifieke aandacht van ontwikkelaars van toepassingen. Aannemende abstract sprake is van een aantal API-primitieven, zoals de mogelijkheid om verbinding te maken en te maken van een vorm van *afzender* en *ontvanger* abstractie objecten, die vervolgens hebben enkele vorm van `send()` en `receive()` bewerkingen, respectievelijk.

Wanneer u geavanceerde mogelijkheden van Azure Service Bus, zoals berichten doorzoeken of het beheer van sessies, worden deze functies worden beschreven in AMQP voorwaarden, maar ook als een gelaagde pseudo-implementatie op deze aangenomen API-abstractie.

## <a name="what-is-amqp"></a>Wat is AMQP?

AMQP is een protocol framing en de overdracht. Framing betekent dat deze structuur biedt voor binaire gegevensstromen die in beide richtingen van een netwerkverbinding. De structuur biedt begrenzing voor verschillende blokken met gegevens, met de naam *frames*, moeten tussen de verbonden partijen worden uitgewisseld. De bestandsoverdracht mogelijk Zorg ervoor dat beide partijen communicatie tot stand brengen een gedeelde begrip over wanneer frames worden overgedragen en wanneer overdrachten beschouwd als voltooid.

In tegenstelling tot eerdere verlopen conceptversies die worden geproduceerd door de AMQP-werkgroep die nog steeds gebruikt door enkele berichtenbrokers worden, bepalen de werkgroep definitief en gestandaardiseerde AMQP 1.0-protocol niet de aanwezigheid van een berichtenbroker of een bepaalde topologie voor entiteiten in een berichtenbroker.

Het protocol kan worden gebruikt voor symmetrische peer-to-peer-communicatie voor interactie met de berichtenbrokers die ondersteuning bieden voor wachtrijen en publiceren/abonneren op entiteiten, zoals Azure Service Bus. Het kan ook worden gebruikt voor interactie met infrastructuur voor berichten waarbij de patronen interactie verschillen van reguliere wachtrijen, zoals het geval is met Azure Event Hubs. Een Event Hub fungeert als een wachtrij wanneer gebeurtenissen worden verzonden, maar meer fungeert als een seriële storage-service wanneer gebeurtenissen worden gelezen uit. het is enigszins vergelijkbaar met een tapestation. De client een offset kiest in de gegevensstroom beschikbaar en wordt vervolgens alle gebeurtenissen uit deze offset aan de meest recente beschikbare geleverd.

De AMQP 1.0-protocol is ontworpen om te worden uitgebreid, waardoor verdere specificaties voor het verbeteren van de mogelijkheden ervan. De drie extensie-specificaties in dit document besproken illustreren dit. Voor communicatie via bestaande HTTPS/WebSockets-infrastructuur, kan de systeemeigen AMQP TCP-poorten configureren lastig zijn. Een binding-specificatie definieert hoe aan laag AMQP via WebSockets. De AMQP-management-specificatie definieert voor interactie met de infrastructuur voor berichten in een aanvraag/antwoord manier voor beheerdoeleinden of om geavanceerde functionaliteit te bieden, de vereiste basic interactie primitieven. Voor federatieve autorisatie model-integratie definieert de specificatie van de claims-beveiliging op basis van AMQP hoe koppelen en autorisatietokens die zijn gekoppeld aan de koppelingen te vernieuwen.

## <a name="basic-amqp-scenarios"></a>Algemene scenario's voor AMQP

Deze sectie wordt uitgelegd dat het basisgebruik van AMQP 1.0 met Azure Service Bus, waaronder verbindingen, sessies en koppelingen maken, en het overdragen van berichten naar en van Service Bus-entiteiten zoals wachtrijen, onderwerpen en abonnementen.

De gezaghebbende bron voor meer informatie over de werking van AMQP de AMQP 1.0-specificatie is, maar de specificatie is geschreven voor het nauwkeurig implementatie en niet om te leren van het protocol. Deze sectie richt zich op de introductie van zoveel terminologie zo nodig voor het beschrijven hoe Service Bus maakt gebruik van AMQP 1.0. Voor een uitgebreidere Inleiding tot AMQP, evenals een uitgebreidere bespreking van AMQP 1.0, kunt u bekijken [deze videocursus][this video course].

### <a name="connections-and-sessions"></a>Verbindingen en sessies

AMQP roept de communicerende programma's *containers*; deze bevatten *knooppunten*, die de communicerende entiteiten binnen die containers zijn. Een wachtrij kan een dergelijke knooppunt zijn. AMQP kan multiplexing, zodat één verbinding kan worden gebruikt voor veel communicatiepaden tussen knooppunten. de toepassingsclient van een kan bijvoorbeeld gelijktijdig ontvangen van een wachtrij en verzenden via de dezelfde netwerkverbinding naar een andere wachtrij.

![][1]

De netwerkverbinding is dus verankerd op de container. Deze is gestart door de container in de rol van de client een uitgaande TCP-socketverbinding met een container maken in de rol van de ontvanger, die luistert naar binnenkomende TCP-verbindingen accepteert. De handshake van verbinding is, bevat de protocolversie, verklaren of onderhandelen over het gebruik van Transport Level Security (TLS/SSL) en een verificatie/autorisatie-handshake voor de verbinding scope die is gebaseerd op SASL onderhandelen.

Azure Service Bus vereist het gebruik van TLS te allen tijde. Het ondersteunt verbindingen via TCP-poort 5671, waarbij de TCP-verbinding eerst met TLS overlapt is voordat het in de handshake AMQP-protocol en biedt ook ondersteuning voor verbindingen via TCP-poort 5672 waarbij de server direct de upgrade van een verplichte van verbinding biedt met behulp van het model AMQP voorgeschreven TLS. De binding AMQP WebSockets maakt een tunnel via TCP-poort 443, die vervolgens gelijk is aan 5671 AMQP-verbindingen.

Na het instellen van de verbinding en TLS, biedt Service Bus twee SASL mechanisme opties:

* SASL zonder opmaak wordt doorgaans gebruikt voor het doorgeven van referenties voor gebruikersnaam en wachtwoord op een server. Service Bus heeft geen accounts, maar met de naam [gedeelde toegang beveiligingsregels](service-bus-sas.md), die het verlenen van rechten en zijn gekoppeld aan een sleutel. De naam van een regel wordt gebruikt als de gebruikersnaam en de sleutel (zoals base64-tekst gecodeerde) wordt gebruikt als het wachtwoord. De rechten die zijn gekoppeld aan de geselecteerde regel van toepassing op de bewerkingen op de verbinding toegestaan.
* SASL anonieme wordt gebruikt voor het overslaan van SASL autorisatie wanneer de client wil gebruiken de claims-beveiligingsmodel op basis van-(CBS) die later wordt beschreven. Met deze optie kan een clientverbinding anoniem worden gemaakt voor een korte periode gedurende welke de client kan alleen communiceren met het eindpunt CBS en de handshake CBS moet voltooien.

Nadat de transportverbinding tot stand is gebracht, wordt de containers Declareer de framegrootte van de maximale die ze bereid bent te verwerken, en na een time-out voor inactiviteit ze zult eenzijdig verbreken als er geen activiteit op de verbinding.

Ze geven ook hoeveel gelijktijdige kanalen worden ondersteund. Een kanaal is een overdracht in één richting, uitgaand, virtueel pad boven op de verbinding. Een sessie duurt een kanaal uit elk van de gekoppelde containers om te vormen een bidirectionele communicatie-pad.

Sessies hebben een model voor het toegangsbeheer op basis van het venster; Wanneer een sessie wordt gemaakt, beide partijen geeft aan hoeveel frames wilt accepteren in het venster. Als de partijen exchange frames, overgebracht frames opvulling venster en overdrachten stoppen wanneer het venster vol is en totdat het venster opnieuw instellen of met behulp van uitgebreide haalt de *stroom performative* (*performative* is de term AMQP voor op protocolniveau gebaren die worden uitgewisseld tussen de twee partijen).

Dit model op basis van het venster is grofweg overeen met het TCP-concept van datatransportbesturing op basis van een venster, maar op het sessieniveau van de in de socket. Concept van het protocol van het toestaan van voor meerdere gelijktijdige sessies bestaat, zodat verkeer met hoge prioriteit kan worden meest voorbij beperkte normale verkeer, zoals op een snelle certificering weg.

Azure Service Bus gebruikt momenteel één sessie voor elke verbinding. De Service Bus-frame-maximumgrootte is 262.144 bytes (256 kB) voor Service Bus Standard- en Event Hubs. Het is 1.048.576 (1 MB) voor Service Bus Premium. Service Bus legt een bepaalde sessie-niveau bandbreedtebeperking windows, maar stelt u het venster regelmatig opnieuw als onderdeel van de link-level datatransportbesturing (Zie [de volgende sectie](#links)).

Verbindingen, kanalen en sessies zijn kortstondige. Als de onderliggende verbinding is samengevouwen, verbindingen, TLS-tunnel, SASL autorisatiecontext en sessies moeten opnieuw worden gemaakt.

### <a name="links"></a>Koppelingen

AMQP overdrachten van berichten via koppelingen. Een koppeling is een communicatiepad dat is gemaakt via een sessie waarmee overgebracht berichten in één richting; de status van overdracht onderhandeling is via de koppeling en twee richtingen tussen de verbonden partijen.

![][2]

Koppelingen kunnen worden gemaakt door een container op elk gewenst moment en op een bestaande sessie, waardoor AMQP af van veel andere protocollen, zoals HTTP- en MQTT, waarbij de inleiding van gegevensoverdracht en het pad van de overdracht is een exclusieve bevoegdheden van de partij die het maken van de socket-verbinding.

De container voor het initiëren van de koppeling wordt u gevraagd de tegengestelde container om te accepteren van een koppeling en een rol van de afzender of ontvanger worden gekozen. Dus een container kunt starten met het maken van Unidirectioneel of bi-directionele communicatiepaden, met de laatste gemodelleerd als paren van koppelingen.

Koppelingen zijn met de naam en die zijn gekoppeld aan knooppunten. Zoals vermeld in het begin, zijn knooppunten de communicerende entiteiten binnen een container.

Een knooppunt is in Service Bus, rechtstreeks gelijk is aan een wachtrij, een onderwerp, een abonnement of een subwachtrij voor onbestelbare berichten van een wachtrij of abonnement. De naam van het knooppunt gebruikt AMQP is daarom de relatieve naam van de entiteit binnen de Service Bus-naamruimte. Als de naam van een wachtrij `myqueue`, dat is ook de naam van de AMQP-knooppunt. Een onderwerpabonnement volgt de HTTP-API-conventie door wordt gesorteerd in een verzameling van de resource 'abonnementen' en dus een abonnement **sub** op een onderwerp **mytopic** heeft de naam van het AMQP-knooppunt  **mytopic/abonnementen/sub**.

De client die verbinding maakt is ook gebruik van een naam in het lokale knooppunt voor het maken van koppelingen; vereist Service Bus is niet over de knooppuntnamen prescriptieve en worden niet geïnterpreteerd. AMQP 1.0-client-stacks algemeen gebruik een schema om te verzekeren dat die deze tijdelijke knooppuntnamen uniek binnen het bereik van de client zijn.

### <a name="transfers"></a>Overdrachten

Nadat is een koppeling tot stand is gebracht, kunnen berichten worden overgedragen via deze koppeling. In het AMQP-, een overdracht wordt uitgevoerd met een gebaar van de expliciete protocol (de *overdracht* performative) die een bericht verplaatst van afzender naar ontvanger via een koppeling. Een overdracht is voltooid als het 'betaald', wat betekent dat beide partijen een gedeelde begrip van het resultaat van deze overdracht hebt vastgesteld.

![][3]

In het meest eenvoudige geval, kunt de afzender kiezen voor het verzenden van berichten 'vooraf verrekend', wat betekent dat de client niet geïnteresseerd in het resultaat is en de ontvanger geen feedback over de uitkomst van de bewerking biedt. In deze modus wordt ondersteund door Service Bus op het niveau van het AMQP-protocol, maar niet weergegeven in een van de client-API's.

De reguliere case is dat berichten worden verzonden niet-vereffende en de ontvanger geeft vervolgens aan acceptatie- of weigerings-met behulp van de *toestand* performative. Afwijzing treedt op wanneer de ontvanger het bericht voor een bepaalde reden niet kan accepteren en het bericht afwijzing bevat informatie over de reden, dit een fout-structuur die is gedefinieerd door AMQP is. Als berichten worden geweigerd vanwege een interne fouten binnen de Service Bus, retourneert de service extra informatie in die structuur die kan worden gebruikt voor het ontwikkelen van hints diagnostische gegevens naar het ondersteuningspersoneel als u bij het indienen van ondersteuningsaanvragen. Leert u later meer informatie over fouten.

Een speciale vorm van weigering is de *die zijn uitgebracht* staat, waarmee wordt aangegeven dat de ontvanger heeft geen technische bezwaar tegen de overdracht, maar ook niet van belang vereffenen van de overdracht. Deze aanvraag bestaat, bijvoorbeeld wanneer een bericht aan een Service Bus-client wordt geleverd, en de client kiest voor het bericht 'afbreken' omdat het resultaat zijn van het verwerken van het bericht; werk kan niet worden uitgevoerd de bezorging van berichten zelf is niet op fouten. Een variant van deze status is de *gewijzigd* status heeft waarmee wijzigingen in het bericht zoals dit wordt uitgebracht. Deze status wordt niet gebruikt door Service Bus op dit moment.

De AMQP 1.0-specificatie definieert een verdere toestand staat met de naam *ontvangen*, die specifiek helpt om af te handelen koppeling herstel. Koppeling herstel kunt endinfrastructuur van de status van een koppeling en alle in behandeling leveringen boven op een nieuwe verbinding en de sessie, wanneer de eerdere verbinding en de sessie verloren gegaan zijn.

Service Bus biedt geen ondersteuning voor herstel van de koppeling; Als de client de verbinding met Service Bus met een niet-vereffende bericht verbroken overdracht is in behandeling, de overdracht van dat bericht is verloren gaan en de client moet opnieuw verbinding maken, herstellen van de koppeling en probeer de overdracht.

Als zodanig, Service Bus en Event Hubs ondersteunt 'ten minste eenmaal' overdracht waarbij de afzender kunt er zeker van zijn voor het bericht zijn opgeslagen en geaccepteerd, maar kan geen ondersteuning voor 'eenmalige' overdrachten op het AMQP-niveau, waarbij het systeem probeert te herstellen van de koppeling en Doorgaan om te onderhandelen over de status van de levering om te voorkomen dat duplicatie van het bericht is verzonden.

Om te compenseren voor eventuele dubbele verzendt, Service Bus biedt ondersteuning voor detectie van duplicaten als een optionele functie van wachtrijen en onderwerpen. Detectie van duplicaten registreert de bericht-id's van alle binnenkomende berichten tijdens een periode die door de gebruiker gedefinieerde en vervolgens op de achtergrond komt alle berichten die tijdens dat dezelfde venster met de dezelfde bericht-id's worden verzonden.

### <a name="flow-control"></a>Datatransportbesturing

Naast het niveau van de sessie-flow-model voor toegangsbeheer die eerder werd besproken, heeft elke koppeling een eigen stroom-model voor toegangsbeheer. Sessie-level datatransportbesturing voorkomt dat de container die voor het afhandelen van te veel frames op zodra het link-level datatransportbesturing de toepassing die verantwoordelijk is voor het aantal berichten in het bedrijf wil verwerken via een koppeling wordt geplaatst en wanneer.

![][4]

Op een koppeling overdrachten kunnen alleen gebeuren als de afzender voldoende heeft *koppelen tegoed*. Koppeling tegoed is een teller die is ingesteld door de ontvanger die met behulp van de *stroom* performative, die is afgestemd op een koppeling. Wanneer de afzender koppeling tegoed toegewezen wordt, probeert deze te gebruiken die tegoed door het leveren van berichten. Elke message delivery verlaagt de resterende koppeling tegoed met 1. Als het link-tegoed is verbruikt, niet meer leveringen.

Wanneer Service Bus in de rol van de ontvanger is, biedt het direct de afzender met voldoende koppeling tegoed, zodat berichten kunnen onmiddellijk worden verzonden. Als koppeling tegoed wordt gebruikt, Service Bus van tijd tot tijd verzendt een *stroom* performative aan de afzender om bij te werken het saldo van de koppeling.

In de rol van de afzender verzendt Service Bus berichten voor alle openstaande koppeling-tegoed.

Een aanroep 'ontvangen' op het niveau van de API wordt omgezet in een *stroom* performative naar Service Bus door de client en Service Bus wordt verzonden dat tegoed verbruikt door de eerste beschikbare, niet-vergrendelde bericht uit de wachtrij, deze vergrendelen en u kunt deze overbrengt. Als er geen bericht direct beschikbaar zijn voor de levering, alle openstaande tegoed door een koppeling tot stand gebracht met dat bepaalde entiteit opgenomen in de volgorde van de aankomst blijft en berichten worden vergrendeld en overgedragen zodra deze beschikbaar is, om het openstaande tegoed gebruiken.

De vergrendeling op een bericht wordt weer vrijgegeven wanneer de overdracht wordt verrekend in een van de terminal statussen *geaccepteerd*, *geweigerd*, of *die zijn uitgebracht*. Het bericht is verwijderd uit de Service Bus wanneer de definitieve status *geaccepteerd*. Het blijft in Service Bus en aan de volgende ontvanger wordt geleverd wanneer de overdracht een van de andere Staten bereikt. Het bericht verplaatst Service Bus automatisch naar de wachtrij voor onbestelbare transactieberichten van de entiteit wanneer het bereikt het maximumaantal leveringen toegestaan voor de entiteit vanwege herhaalde weigeringen of versies.

Zelfs als de Service Bus-API's die een optie vandaag nog niet rechtstreeks blootstellen, kunt een lager niveau AMQP-protocol-client het link-tegoed-model gebruiken om in te schakelen van de interactie 'pull-stijl' van het uitgeven van één eenheid van tegoed voor elke aanvraag ontvangen in een model "push-stijl" door uitgifte van een groot aantal credits koppelen en vervolgens berichten ontvangen zodra deze beschikbaar zonder verdere tussenkomst. Push is alleen beschikbaar via de [MessagingFactory.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagingfactory) of [MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount) eigenschapsinstellingen. Wanneer ze dan nul zijn, de AMQP-client wordt gebruikt als de koppeling-tegoed.

In deze context is het belangrijk om te weten dat de klok voor de vervaldatum van de vergrendeling van het bericht in de entiteit wordt gestart wanneer het bericht is afkomstig uit de entiteit niet wanneer het bericht wordt geplaatst op de kabel. Wanneer de client aangeeft dat de gereedheid voor het ontvangen van berichten met behulp van de koppeling tegoed, is het daarom waarschijnlijk worden actief binnenhalen van berichten via het netwerk en klaar om deze te verwerken. Anders kan de vergrendeling van het bericht verlopen voordat het bericht zelfs wordt geleverd. Het gebruik van koppeling tegoed datatransportbesturing moet rechtstreeks vergelijkbaar zijn met direct gereed is om op te lossen met beschikbare berichten verzonden naar de ontvanger.

Kortom vindt de volgende secties u een schematische overzicht van de performative stroom tijdens de interacties met andere API. Elke sectie beschrijft een andere logische bewerking. Sommige van deze interacties mogelijk 'vertraagde,"wat betekent dat ze kunnen alleen worden uitgevoerd indien nodig. Het maken van de afzender van een bericht mogelijk niet de interactie van een netwerk totdat het eerste bericht is verzonden of wordt aangevraagd.

De pijlen in de volgende tabel stroomrichting de performative.

#### <a name="create-message-receiver"></a>Bericht ontvanger maken

| Client | Service Bus |
| --- | --- |
| --> (koppelen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke ingang},<br/>rol =**ontvanger**,<br/>bron = {entiteitsnaam van de},<br/>doel = {client koppeling-ID}<br/>) |Client koppelt aan de entiteit als ontvanger |
| Service Bus-antwoorden die het einde van de koppeling toevoegen |<--(koppelen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke ingang},<br/>rol =**afzender**,<br/>bron = {entiteitsnaam van de},<br/>doel = {client koppeling-ID}<br/>) |

#### <a name="create-message-sender"></a>Maken van de afzender van bericht

| Client | Service Bus |
| --- | --- |
| --> (koppelen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke ingang},<br/>rol =**afzender**,<br/>bron = {client koppeling ID}<br/>doel = {naam van de entiteit}<br/>) |Geen actie |
| Geen actie |<--(koppelen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke ingang},<br/>rol =**ontvanger**,<br/>bron = {client koppeling ID}<br/>doel = {naam van de entiteit}<br/>) |

#### <a name="create-message-sender-error"></a>Maken van de afzender van bericht (fout)

| Client | Service Bus |
| --- | --- |
| --> (koppelen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke ingang},<br/>rol =**afzender**,<br/>bron = {client koppeling ID}<br/>doel = {naam van de entiteit}<br/>) |Geen actie |
| Geen actie |<--(koppelen<br/>naam = {naam van de koppeling}<br/>verwerken = {numerieke ingang},<br/>rol =**ontvanger**,<br/>source=null,<br/>target=null<br/>)<br/><br/><--(loskoppelen<br/>verwerken = {numerieke ingang},<br/>gesloten =**waar**,<br/>Fout = {foutgegevens}<br/>) |

#### <a name="close-message-receiversender"></a>Bericht sluiten ontvanger/afzender

| Client | Service Bus |
| --- | --- |
| --> (loskoppelen<br/>verwerken = {numerieke ingang},<br/>gesloten =**true**<br/>) |Geen actie |
| Geen actie |<--(loskoppelen<br/>verwerken = {numerieke ingang},<br/>gesloten =**true**<br/>) |

#### <a name="send-success"></a>Verzenden (geslaagd)

| Client | Service Bus |
| --- | --- |
| overdracht (--><br/>Delivery-id = {numerieke ingang},<br/>Delivery-tag = {binaire ingang},<br/>verrekend =**false**,, meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>) |Geen actie |
| Geen actie |<--toestand (<br/>role=receiver,<br/>eerst = {delivery-ID}<br/>laatste = {delivery-ID}<br/>verrekend =**waar**,<br/>status =**geaccepteerd**<br/>) |

#### <a name="send-error"></a>Verzenden (fout)

| Client | Service Bus |
| --- | --- |
| overdracht (--><br/>Delivery-id = {numerieke ingang},<br/>Delivery-tag = {binaire ingang},<br/>verrekend =**false**,, meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>) |Geen actie |
| Geen actie |<--toestand (<br/>role=receiver,<br/>eerst = {delivery-ID}<br/>laatste = {delivery-ID}<br/>verrekend =**waar**,<br/>status =**geweigerd**()<br/>Fout = {foutgegevens}<br/>)<br/>) |

#### <a name="receive"></a>Ontvangen

| Client | Service Bus |
| --- | --- |
| stroom (--><br/>link-credit=1<br/>) |Geen actie |
| Geen actie |< (overdragen<br/>Delivery-id = {numerieke ingang},<br/>Delivery-tag = {binaire ingang},<br/>verrekend =**false**,<br/>meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>) |
| toestand (--><br/>rol =**ontvanger**,<br/>eerst = {delivery-ID}<br/>laatste = {delivery-ID}<br/>verrekend =**waar**,<br/>status =**geaccepteerd**<br/>) |Geen actie |

#### <a name="multi-message-receive"></a>Meerdere bericht ontvangen

| Client | Service Bus |
| --- | --- |
| stroom (--><br/>link-credit=3<br/>) |Geen actie |
| Geen actie |< (overdragen<br/>Delivery-id = {numerieke ingang},<br/>Delivery-tag = {binaire ingang},<br/>verrekend =**false**,<br/>meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>) |
| Geen actie |< (overdragen<br/>Delivery-id = {numerieke ingang + 1},<br/>Delivery-tag = {binaire ingang},<br/>verrekend =**false**,<br/>meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>) |
| Geen actie |< (overdragen<br/>Delivery-id = {numerieke ingang + 2},<br/>Delivery-tag = {binaire ingang},<br/>verrekend =**false**,<br/>meer =**false**,<br/>status =**null**,<br/>hervatten =**false**<br/>) |
| toestand (--><br/>role=receiver,<br/>eerst = {delivery-ID}<br/>laatste = {delivery-ID + 2},<br/>verrekend =**waar**,<br/>status =**geaccepteerd**<br/>) |Geen actie |

### <a name="messages"></a>Berichten

De volgende secties wordt uitgelegd welke eigenschappen uit de standard AMQP bericht secties worden gebruikt door Service Bus en hoe deze worden toegewezen aan de Service Bus-API-set.

Elke eigenschap die nodig is om definieert toepassing moet worden toegewezen aan de AMQP `application-properties` kaart.

#### <a name="header"></a>koptekst

| Veldnaam | Gebruik | API-naam |
| --- | --- | --- |
| duurzame |- |- |
| priority |- |- |
| ttl |Time to live voor dit bericht |[TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive) |
| first-acquirer |- |- |
| aantal bezorgingspogingen |- |[DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |

#### <a name="properties"></a>properties

| Veldnaam | Gebruik | API-naam |
| --- | --- | --- |
| bericht-id |Toepassingsspecifieke, vrije-id voor dit bericht. Gebruikt voor detectie van duplicaten. |[MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) |
| gebruikers-id |Id van gebruiker toepassingsspecifieke, niet geïnterpreteerd door Service Bus. |Niet toegankelijk zijn via de Service Bus-API. |
| tot |Toepassingsspecifieke doel-id niet geïnterpreteerd door Service Bus. |[Aan](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_To) |
| onderwerp |Bericht toepassingsspecifieke doel-id, niet geïnterpreteerd door Service Bus. |[Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) |
| Antwoordadres |Antwoord-path toepassingsspecifieke indicator niet geïnterpreteerd door Service Bus. |[ReplyTo](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ReplyTo) |
| correlatie-id |Toepassingsspecifieke correlatie-id niet geïnterpreteerd door Service Bus. |[CorrelationId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |
| inhoudstype |Toepassingsspecifieke inhoudstype indicator voor de hoofdtekst van het niet geïnterpreteerd door Service Bus. |[ContentType](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |
| content-encoding |Toepassingsspecifieke indicator codering van inhoud voor de hoofdtekst van het niet geïnterpreteerd door Service Bus. |Niet toegankelijk zijn via de Service Bus-API. |
| absolute-expiry-time |Geeft aan op welke absolute instant het bericht is verlopen. Op de invoer (koptekst TTL wordt waargenomen), genegeerd gezaghebbende op uitvoer. |[ExpiresAtUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ExpiresAtUtc) |
| creation-time |Verklaart op dat moment het bericht is gemaakt. Niet gebruikt door Service Bus |Niet toegankelijk zijn via de Service Bus-API. |
| groep-id |Toepassingsspecifieke id voor een gerelateerde set berichten. Gebruikt voor Service Bus-sessies. |[SessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) |
| volgorde van de groep |Teller voor het identificeren van de relatieve volgnummer van het bericht in een sessie. Genegeerd door Servicebus. |Niet toegankelijk zijn via de Service Bus-API. |
| antwoord-naar-groep-id |- |[ReplyToSessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage) |

#### <a name="message-annotations"></a>Bericht-aantekeningen

Er zijn enkele andere service bus-bericht-eigenschappen, maken geen deel uit van de eigenschappen van berichten van AMQP en worden doorgegeven als `MessageAnnotations` voor het bericht.

| Aantekening kaart sleutel | Gebruik | API-naam |
| --- | --- | --- |
| x-opt-scheduled-enqueue-time | Verklaart op dat moment het bericht op de entiteit moet worden weergegeven |[ScheduledEnqueueTime](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.scheduledenqueuetimeutc?view=azure-dotnet) |
| x-opt-partition-key | Toepassingsspecifieke sleutel die welke partitie bepaalt het bericht moet worden neergezet in. | [partitionKey](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.partitionkey?view=azure-dotnet) |
| x-opt-via-partition-key | Toepassing gedefinieerde partitiesleutel waarde als een transactie moet worden gebruikt voor het verzenden van berichten uit via een wachtrij voor overdracht. | [ViaPartitionKey](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.viapartitionkey?view=azure-dotnet) |
| x-opt-enqueued-time | Service gedefinieerde UTC-tijd dat de werkelijke tijd van het bericht enqueuing vertegenwoordigt. Op de invoer wordt genegeerd. | [EnqueuedTimeUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc?view=azure-dotnet) |
| x-opt-sequence-number | Service gedefinieerde unieke nummer toegewezen aan een bericht. | [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber?view=azure-dotnet) |
| x-opt-offset | Service is gedefinieerd in de wachtrij geplaatste volgnummer van het bericht. | [EnqueuedSequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedsequencenumber?view=azure-dotnet) |
| x-opt-vergrendeld-pas | Service-gedefinieerd. De datum en tijd waarop het bericht vergrendeld in de wachtrij /-abonnement. | [LockedUntilUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.lockeduntilutc?view=azure-dotnet) |
| x-opt-deadletter-source | Service-gedefinieerd. Als het bericht wordt ontvangen van de wachtrij voor onbestelbare berichten, de bron van het oorspronkelijke bericht. | [DeadLetterSource](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deadlettersource?view=azure-dotnet) |

### <a name="transaction-capability"></a>Transactie-mogelijkheid

Een transactie worden twee of meer bewerkingen samen in een bereik worden uitgevoerd. Een dergelijke transactie moet ervoor zorgen dat alle bewerkingen die behoren tot een bepaalde groep bewerkingen slagen of gezamenlijk mislukken door aard.
De bewerkingen worden gegroepeerd met een id `txn-id`.

Voor interactie met transactionele, de client fungeert als een `transaction controller` , welke besturingselementen van de bewerkingen die moeten worden gegroepeerd. Service Bus-Service fungeert als een `transactional resource` en werk uitvoert, zoals aangevraagd door de `transaction controller`.

De client en de service communiceren via een `control link` , die tot stand is gebracht door de client. De `declare` en `discharge` berichten worden verzonden door de netwerkcontroller via de koppeling besturingselement toegewezen en het uitvoeren van transacties respectievelijk (ze geen vertegenwoordigen de begrenzing van transactioneel werk). De werkelijke dat verzenden/ontvangen wordt op deze koppeling niet uitgevoerd. Elke transactionele de aangevraagde bewerking is expliciet aangeduid met de gewenste `txn-id` en daarom kunnen zich voordoen op een koppeling op de verbinding. Als de controle-koppeling is gesloten terwijl er niet-ontslagen transacties die deze gemaakt, klikt u vervolgens al deze transacties onmiddellijk teruggedraaid en pogingen tot op deze verder transactioneel werk verrichten zal leiden tot een fout. Berichten op het besturingselement koppeling mag geen pre verrekend.

Elke verbinding heeft een eigen besturingselement koppeling om te beginnen en eindigen transacties kunnen initiëren. De service definieert een speciale bestemming die als fungeert een `coordinator`. De client/controller stelt een besturingselement koppeling naar dit doel. Besturingselement koppeling is buiten de grens van een entiteit, dat wil zeggen, dezelfde besturingselement koppeling kan worden gebruikt om te initiëren en uitvoering van transacties voor meerdere entiteiten.

#### <a name="starting-a-transaction"></a>Starten van een transactie

Om te beginnen transactioneel werk. het verkrijgen van de domeincontroller moet een `txn-id` uit de coördinator. Dit gebeurt door het verzenden van een `declare` type bericht. Als de verklaring geslaagd is, wordt de coördinator reageert met een resultaat toestand, dat het toegewezen `txn-id`.

| Client (Controller) | | Servicebus (coördinator) |
| --- | --- | --- |
| attach(<br/>naam = {naam van de koppeling}<br/>... ,<br/>rol =**afzender**,<br/>doel =**Coordinator**<br/>) | ------> |  |
|  | <------ | attach(<br/>naam = {naam van de koppeling}<br/>... ,<br/>target=Coordinator()<br/>) |
| overdracht (<br/>Delivery-id = 0,...)<br/>{ AmqpValue (**Declare()**)}| ------> |  |
|  | <------ | toestand ( <br/> first=0, last=0, <br/>status =**Declared**()<br/>**transacties-id**= {transactie-ID}<br/>))|

#### <a name="discharging-a-transaction"></a>Gebruik van een transactie

De controller is de transactioneel werk door te sturen een `discharge` bericht aan de coördinator. De controller wordt aangegeven dat deze wil doorvoeren of terugdraaien van de transactioneel werk door in te stellen de `fail` vlag aan de hoofdtekst van het ontslag. Als de coördinator niet kan voltooien van het ontslag, het bericht is geweigerd met de uitvoering van dit resultaat de `transaction-error`.

> Opmerking: mislukt = true verwijst naar ongedaan maken van een transactie en mislukt = false verwijst naar doorvoeren.

| Client (Controller) | | Servicebus (coördinator) |
| --- | --- | --- |
| overdracht (<br/>Delivery-id = 0,...)<br/>{ AmqpValue (Declare())}| ------> |  |
|  | <------ | toestand ( <br/> first=0, last=0, <br/>state=Declared(<br/>transacties-id = {transactie-ID}<br/>))|
| | . . . <br/>Transactioneel werk<br/>op andere koppelingen<br/> . . . |
| overdracht (<br/>delivery-id=57, ...)<br/>{ AmqpValue (<br/>**Managers (transacties-id = 0,<br/>mislukt = false)**)}| ------> |  |
| | <------ | toestand ( <br/> first=57, last=57, <br/>state=**Accepted()**)|

#### <a name="sending-a-message-in-a-transaction"></a>Verzenden van een bericht in een transactie

Alle transactioneel werk gedaan met de status van de transactionele levering `transactional-state` die de transacties-id bevat. In het geval van het verzenden van berichten, wordt de transactiestatus uitgevoerd door van het bericht overdracht frame. 

| Client (Controller) | | Servicebus (coördinator) |
| --- | --- | --- |
| overdracht (<br/>Delivery-id = 0,...)<br/>{ AmqpValue (Declare())}| ------> |  |
|  | <------ | toestand ( <br/> first=0, last=0, <br/>state=Declared(<br/>transacties-id = {transactie-ID}<br/>))|
| overdracht (<br/>verwerken = 1,<br/>Delivery-id = 1, <br/>**state=<br/>TransactionalState(<br/>txn-id=0)**)<br/>{de nettolading van}| ------> |  |
| | <------ | toestand ( <br/> eerst = 1, laatste = 1, <br/>state=**TransactionalState(<br/>txn-id=0,<br/>outcome=Accepted()**))|

#### <a name="disposing-a-message-in-a-transaction"></a>Een bericht in een transactie wordt verwijderd

Bericht toestand omvat bewerkingen zoals `Complete`  /  `Abandon`  /  `DeadLetter`  /  `Defer`. Als u wilt uitvoeren van deze bewerkingen binnen een transactie, geven de `transactional-state` met de toestand.

| Client (Controller) | | Servicebus (coördinator) |
| --- | --- | --- |
| overdracht (<br/>Delivery-id = 0,...)<br/>{ AmqpValue (Declare())}| ------> |  |
|  | <------ | toestand ( <br/> first=0, last=0, <br/>state=Declared(<br/>transacties-id = {transactie-ID}<br/>))|
| | <------ |overdracht (<br/>handle=2,<br/>Delivery-id = 11 <br/>status = null)<br/>{de nettolading van}|  
| toestand ( <br/> eerst = 11, laatste = 11 <br/>state=**TransactionalState(<br/>txn-id=0,<br/>outcome=Accepted()**))| ------> |


## <a name="advanced-service-bus-capabilities"></a>Geavanceerde mogelijkheden voor Service Bus

In deze sectie bevat informatie over geavanceerde mogelijkheden van Azure Service Bus die zijn gebaseerd op concept uitbreidingen van AMQP, momenteel ontwikkeld in de technische Comité OASIS voor AMQP. Service Bus implementeert de nieuwste versies van deze concepten en neemt de wijzigingen die zijn geïntroduceerd omdat deze concepten worden standaard status bereikt.

> [!NOTE]
> Service Bus Messaging geavanceerde bewerkingen worden ondersteund via een patroon voor aanvraag/antwoord. De details van deze bewerkingen worden beschreven in het artikel [AMQP 1.0 in Service Bus: aanvraag-reactie-bewerkingen](service-bus-amqp-request-response.md).
> 
> 

### <a name="amqp-management"></a>AMQP-management

De AMQP-management-specificatie is de eerste dag van de draft-extensies die in dit artikel worden besproken. Deze specificatie definieert een set protocollen gelaagd boven op het AMQP-protocol waarmee beheerinteracties met de infrastructuur voor berichten via AMQP. De specificatie definieert de algemene bewerkingen zoals *maken*, *lezen*, *bijwerken*, en *verwijderen* voor het beheren van entiteiten binnen een infrastructuur voor berichten en een set querybewerkingen.

Alle deze gebaren vereisen een verzoek/reactie-interactie tussen de client en de messaging-infrastructuur, en daarom de specificatie definieert voor het model dat interactiepatroon boven op AMQP: de client verbinding maakt met de messaging-infrastructuur initieert een sessie en maakt vervolgens een paar van koppelingen. Op een koppeling, de client fungeert als de afzender en op de andere het fungeert als ontvanger, waardoor het maken van een paar van de koppelingen die als een bi-directioneel-kanaal fungeren.

| Logische-bewerking | Client | Service Bus |
| --- | --- | --- |
| Antwoord Aanvraagpad maken |--> (koppelen<br/>naam = {*koppelingnaam*},<br/>verwerken = {*numerieke ingang*},<br/>rol =**afzender**,<br/>bron =**null**,<br/>target=”myentity/$management”<br/>) |Geen actie |
| Antwoord Aanvraagpad maken |Geen actie |\<--(koppelen<br/>naam = {*koppelingnaam*},<br/>verwerken = {*numerieke ingang*},<br/>rol =**ontvanger**,<br/>source=null,<br/>target=”myentity”<br/>) |
| Antwoord Aanvraagpad maken |--> (koppelen<br/>naam = {*koppelingnaam*},<br/>verwerken = {*numerieke ingang*},<br/>rol =**ontvanger**,<br/>source=”myentity/$management”,<br/>target=”myclient$id”<br/>) | |
| Antwoord Aanvraagpad maken |Geen actie |\<--(koppelen<br/>naam = {*koppelingnaam*},<br/>verwerken = {*numerieke ingang*},<br/>rol =**afzender**,<br/>bron = "myentity"<br/>target=”myclient$id”<br/>) |

Met deze combinatie van koppelingen in plaats, de aanvraag/antwoord-uitvoering is vrij eenvoudig: er wordt een aanvraag een bericht verzonden naar een entiteit in de infrastructuur voor berichten die werkt met dit patroon. In deze aanvraagbericht de *antwoordadres* veld in de *eigenschappen* sectie is ingesteld op de *doel* -id voor de koppeling waarop u het antwoord leveren. De entiteit verwerking verwerkt de aanvraag, en levert u vervolgens het antwoord op de koppeling waarvan *doel* -id overeenkomt met de opgegeven *antwoordadres* id.

Het patroon moet duidelijk dat de client-container en de gegenereerde client-id voor het doel van het antwoord uniek zijn voor alle clients en, uit veiligheidsoverwegingen, ook moeilijk te voorspellen.

De berichten worden uitgewisseld gebruikt voor het management-protocol en voor alle andere protocollen die gebruikmaken van hetzelfde patroon gebeuren op toepassingsniveau; deze definiëren nieuwe AMQP-protocol op serverniveau gebaren. Dat is bedoeld, zodat toepassingen kunnen onmiddellijk profiteren van deze extensies met voldoet AMQP 1.0-stacks.

Service Bus momenteel implementeert niet een van de belangrijkste functies van de management-specificatie, maar het verzoek/reactie-patroon dat is gedefinieerd door de management-specificatie is fundamenteel voor de claims-beveiliging op basis van functie en bijna alle van de geavanceerde mogelijkheden die in de volgende secties worden besproken:

### <a name="claims-based-authorization"></a>Autorisatie op basis van claims

Het concept van de specificatie AMQP Claims-autorisatie op basis van (CBS) is gebaseerd op de management-specificatie verzoek/reactie-patroon en een beschrijving van een gegeneraliseerde model voor het gebruik van federatieve beveiligingstokens met AMQP.

Het standaard-beveiligingsmodel van AMQP besproken in de inleiding is gebaseerd op SASL en kan worden geïntegreerd met de handshake AMQP-verbinding. SASL heeft het voordeel dat een uitbreidbare model waarvoor een set mechanismen zijn gedefinieerd biedt in elk protocol dat voorheen op SASL leans kan profiteren. Deze mechanismen zijn "PLATTE" voor de overdracht van gebruikersnamen en wachtwoorden, 'Extern' verbinding maken met TLS-level security, 'Anoniem' om de afwezigheid van expliciete verificatie/autorisatie en tal van andere mechanismen waarmee doorgeven verificatie en/of autorisatie referenties of -tokens.

De AMQP SASL integratie heeft twee nadelen:

* Alle referenties en tokens zijn gericht op de verbinding. Een infrastructuur voor berichten willen gedifferentieerde toegangsbeheer op basis van per entiteit; bijvoorbeeld, zodat de houder van een token te verzenden naar een wachtrij, maar niet naar de wachtrij B. Met de autorisatiecontext verankerd op de verbinding, is het niet mogelijk slechts één verbinding en nog andere toegangstokens voor wachtrij A en B. wachtrij gebruiken
* Toegangstokens zijn doorgaans alleen geldig voor een beperkte periode. Deze geldigheid moet de gebruiker periodiek opnieuw ophalen van tokens en biedt de mogelijkheid voor de uitgever van het token te weigeren uitgeven van een nieuwe token als van de gebruiker toegang tot machtigingen zijn gewijzigd. AMQP-verbindingen kunnen de laatste gedurende lange perioden. De SASL-model biedt alleen een kans om in te stellen van een token tijdens de verbinding, wat dat de infrastructuur voor berichten is betekent verbreken van de client wanneer het token is verlopen of nodig is om te accepteren van het risico van waardoor verdere communicatie met een client die de toegangsrechten is mogelijk in de tussentijd ingetrokken.

De specificatie AMQP CBS geïmplementeerd door Service Bus, kunt een elegante tijdelijke oplossing voor zowel van die problemen: Hierdoor kan een client access tokens koppelen aan elk knooppunt, en om bij te werken die tokens voordat ze zijn verlopen, zonder dat de stroom wordt onderbroken.

CBS definieert een virtuele management-knooppunt, met de naam *$cbs*, te worden verstrekt door de messaging-infrastructuur. Het beheerknooppunt accepteert tokens namens een andere knooppunten in de infrastructuur voor berichten.

Het gebaar protocol is een aanvraag/antwoord-uitwisseling zoals gedefinieerd door de management-specificatie. Dat betekent dat de client tot stand brengt een combinatie van koppelingen in met de *$cbs* knooppunt en geeft vervolgens een aanvraag door op de uitgaande koppeling en vervolgens het antwoord wacht op de inkomende koppeling.

Het aanvraagbericht bevat de volgende toepassingseigenschappen:

| Sleutel | Optioneel | Waardetype | De inhoud van waarde |
| --- | --- | --- | --- |
| bewerking |Nee |string |**Put-token** |
| type |Nee |string |Het type van het token wordt geplaatst. |
| naam |Nee |string |De 'doelgroep', die de token van toepassing is. |
| verlooptijd |Ja |tijdstempel |De verlooptijd van het token. |

De *naam* eigenschap identificeert de entiteit waarmee het token gekoppeld worden moet. Dit is het pad naar de wachtrij of onderwerp/abonnement in Service Bus. De *type* eigenschap identificeert het type token:

| Tokentype | Beschrijving van token | Type instantie | Opmerkingen |
| --- | --- | --- | --- |
| amqp:jwt |JSON Webtoken (JWT) |AMQP-waarde (tekenreeks) |Nog niet beschikbaar. |
| amqp:SWT |Eenvoudige Webtoken (SWT) |AMQP-waarde (tekenreeks) |Alleen ondersteund voor SWT-tokens die zijn uitgegeven door de AAD/ACS |
| servicebus.windows.net:sastoken |Service Bus SAS-Token |AMQP-waarde (tekenreeks) |- |

Tokens verleent rechten. Service Bus op de hoogte van drie fundamentele rechten: 'Verzenden' kunnen worden verzonden, "Listen" kunnen ontvangen, en manipuleren entiteiten 'Beheren' kunnen. Deze rechten bevatten SWT-tokens die zijn uitgegeven door de AAD/ACS expliciet als claims. Service Bus SAS-tokens Raadpleeg regels die zijn geconfigureerd op de naamruimte of de entiteit en deze regels zijn geconfigureerd met rechten. Het token ondertekenen met de sleutel die is gekoppeld aan deze regel dus kunt u de token snelle de respectieve rechten. Het token dat is gekoppeld aan een entiteit met *put-token* toestaat de verbonden client om te communiceren met de entiteit per token rechten. Een koppeling waar de client wordt op de *afzender* rol vereist de "verzenden" rechts; op de *ontvanger* rol vereist het "Listen" rechts.

Het antwoordbericht bevat de volgende *toepassingseigenschappen* waarden

| Sleutel | Optioneel | Waardetype | De inhoud van waarde |
| --- | --- | --- | --- |
| status-code |Nee |int |HTTP-responscode **[RFC2616]**. |
| Beschrijving van status |Ja |string |Beschrijving van de status. |

De client kunt aanroepen *put-token* herhaaldelijk en voor elke entiteit in de infrastructuur voor berichten. De tokens zijn binnen het bereik van de huidige client en verankerd op de huidige verbinding, wat betekent dat de server komt geen tokens behouden wanneer de verbinding uitvalt.

De huidige implementatie van Service Bus staat alleen CBS in combinatie met de SASL-methode 'Anoniem'. Een SSL/TLS-verbinding moet altijd aanwezig zijn voordat de SASL-handshake.

Het mechanisme voor anonieme moet daarom worden ondersteund door de gekozen AMQP 1.0-client. Anonieme toegang betekent dat de eerste verbinding-handshake, waaronder het maken van de eerste sessie gebeurt zonder dat Service Bus weten wie de verbinding wordt gemaakt.

Zodra de verbinding en de sessie tot stand is gebracht, Bezig met koppelen van de koppelingen naar de *$cbs* knooppunt en het verzenden van de *put-token* aanvragen zijn de enige toegestane bewerkingen. Een geldig token moet worden ingesteld via een *put-token* aanvraag voor een bepaald knooppunt entiteit binnen 20 seconden nadat de verbinding is tot stand is gebracht, anders wordt de verbinding wordt eenzijdig verbroken door Service Bus.

De client is voor het bijhouden van de geldigheidsduur van het token. Wanneer een token is verlopen, komt Service Bus onmiddellijk alle koppelingen voor de verbinding met de respectieve entiteit. Als u wilt voorkomen dat het probleem optreedt, de client het token voor het knooppunt kunt vervangen door een nieuw exemplaar op elk gewenst moment via de virtuele *$cbs* beheerknooppunt met dezelfde *put-token* gebaar en zonder ophalen in de de nettolading van de manier om verkeer dat stromen op verschillende koppelingen.

### <a name="send-via-functionality"></a>Verzenden via functionaliteit

[Verzenden via / Transfer afzender](service-bus-transactions.md#transfers-and-send-via) is een functie waarmee service bus een bepaald bericht doorsturen naar een bestemming entiteit via een andere entiteit. Deze functie wordt gebruikt voor het uitvoeren van bewerkingen voor entiteiten in één transactie.

Met deze functionaliteit, die u maakt een afzender en tot stand brengen van de koppeling naar de `via-entity`. Aanvullende informatie wordt doorgegeven tijdens het maken van de koppeling voor het maken van de waarde true bestemming van de berichten/overdrachten op deze koppeling. Zodra het koppelen voltooid is, alle berichten die op deze koppeling wordt verzonden, automatisch worden doorgestuurd naar de *doelentiteit* via *via entiteit*. 

> Opmerking: Verificatie moet worden uitgevoerd voor beide *via entiteit* en *doelentiteit* voordat u deze koppeling tot stand brengen.

| Client | | Service Bus |
| --- | --- | --- |
| attach(<br/>naam = {naam van de koppeling}<br/>rol = afzender<br/>bron = {client koppeling ID}<br/>doel =**{via entity}**,<br/>**Eigenschappen van kaart = [(<br/>com.microsoft:transfer doeladres =<br/>{doelentiteit})]** ) | ------> | |
| | <------ | attach(<br/>naam = {naam van de koppeling}<br/>role=receiver,<br/>bron = {client koppeling ID}<br/>doel = {via-entiteit},<br/>properties=map [(<br/>com.microsoft:transfer-destination-address=<br/>{Doelentiteit})] ) |

## <a name="next-steps"></a>Volgende stappen

Voor meer informatie over AMQP, gaat u naar de volgende koppelingen:

* [Service Bus AMQP-overzicht]
* [AMQP 1.0-ondersteuning voor Service Bus gepartitioneerde wachtrijen en onderwerpen]
* [AMQP in WindowsServer-Servicebus]

[this video course]: https://www.youtube.com/playlist?list=PLmE4bZU0qx-wAP02i0I7PJWvDWoCytEjD
[1]: ./media/service-bus-amqp-protocol-guide/amqp1.png
[2]: ./media/service-bus-amqp-protocol-guide/amqp2.png
[3]: ./media/service-bus-amqp-protocol-guide/amqp3.png
[4]: ./media/service-bus-amqp-protocol-guide/amqp4.png

[Service Bus AMQP-overzicht]: service-bus-amqp-overview.md
[AMQP 1.0-ondersteuning voor Service Bus gepartitioneerde wachtrijen en onderwerpen]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP in WindowsServer-Servicebus]: https://msdn.microsoft.com/library/dn574799.aspx
