---
title: Azure Storage-wachtrijen en Service Bus-wachtrijen - vergeleken en tegenstelling tot | Microsoft Docs
description: Analyseert verschillen en overeenkomsten tussen de twee typen wachtrijen die door Azure worden aangeboden.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: tysonn
ms.assetid: f07301dc-ca9b-465c-bd5b-a0f99bab606b
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: 555759073507219188b59af76a82be74b112c57c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="storage-queues-and-service-bus-queues---compared-and-contrasted"></a>Opslagwachtrijen en Service Bus-wachtrijen - vergeleken en tegenstelling tot
In dit artikel analyseert de verschillen en overeenkomsten tussen de twee typen wachtrijen die tegenwoordig worden aangeboden door Microsoft Azure: opslagwachtrijen en Service Bus-wachtrijen. U kunt deze informatie gebruiken om de verschillende technologieën te vergelijken en tegen elkaar af te zetten zodat u een weloverwogen beslissing kunt nemen en de oplossing kiest die beste voldoet aan uw behoeften.

## <a name="introduction"></a>Inleiding
Azure ondersteunt twee soorten wachtrij mechanismen: **opslagwachtrijen** en **Service Bus-wachtrijen**.

**Opslagwachtrijen**, die deel uitmaken van de [Azure storage](https://azure.microsoft.com/services/storage/) infrastructuur, functie een eenvoudige interface van de REST gebaseerde GET/PUT/PEEK biedt betrouwbare, permanente messaging binnen en tussen services.

**Service Bus-wachtrijen** deel uitmaken van een breder [Azure messaging](https://azure.microsoft.com/services/service-bus/) infrastructuur die ondersteuning biedt voor queuing evenals publiceren/abonneren en meer geavanceerde integratie patronen. Zie voor meer informatie over Service Bus-onderwerpen-wachtrijen/abonnementen de [overzicht van Service Bus](service-bus-messaging-overview.md).

Hoewel beide queuing technologieën gelijktijdig bestaan, kennisgemaakt opslagwachtrijen eerst als een toegewezen wachtrij opslagmechanisme gebouwd op Azure Storage-services. Service Bus-wachtrijen zijn gebouwd op de breder ' ' berichteninfrastructuur ontworpen voor integratie van toepassingen of de onderdelen van de toepassing die meerdere communicatieprotocollen, gegevenscontracten, vertrouwen domeinen en/of netwerkomgevingen kunnen omvatten.

## <a name="technology-selection-considerations"></a>Overwegingen voor selectie van technologie
Zijn de implementaties van de message Queuing-service dat momenteel wordt aangeboden op Microsoft Azure Storage-wachtrijen en Service Bus-wachtrijen. Elk heeft een iets andere functieset, wat betekent dat u kunt kiezen een of beide, afhankelijk van de behoeften van uw bepaalde oplossing of -business-technische probleem, u het oplossen van.

Bij het bepalen van welke queuing technologie geschikt is voor het doel voor een bepaalde oplossing oplossingsarchitecten en -ontwikkelaars moeten rekening houden met de onderstaande aanbevelingen. Zie de volgende sectie voor meer informatie.

Als een oplossing systeemarchitect of ontwikkelaar, **Overweeg het gebruik van opslagwachtrijen** wanneer:

* Uw toepassing moet meer dan 80 GB van berichten opslaan in een wachtrij, waarbij de berichten een korter zijn dan 7 dagen levensduur hebben.
* Uw toepassing wil bijhouden uitgevoerd voor het verwerken van een bericht in de wachtrij. Dit is handig als het verwerken van een bericht van het werkproces is vastgelopen. Een daaropvolgende worker kunt die gegevens vervolgens gebruiken om door te gaan van waar de voorafgaande worker gebleven was.
* Gewenste server side-logboeken van alle transacties die op uw wachtrijen worden uitgevoerd.

Als een oplossing systeemarchitect of ontwikkelaar, **Overweeg het gebruik van Service Bus-wachtrijen** wanneer:

* Uw oplossing moet berichten kunnen ontvangen zonder op te vragen van de wachtrij. Met Service Bus, kan dit worden bereikt door het gebruik van de lange-polling opnieuw met de TCP-protocollen die ondersteuning biedt voor Service Bus ontvangen.
* Uw oplossing is vereist voor de wachtrij voor het bieden van een gegarandeerde first-in-first-out (FIFO) besteld levering.
* Wilt u een symmetrische ervaring in Azure en Windows Server (privécloud). Zie voor meer informatie [Service Bus voor Windows Server](https://msdn.microsoft.com/library/dn282144.aspx).
* Uw oplossing moet mogelijk zijn ter ondersteuning van automatische detectie van duplicaten.
* U wilt dat uw toepassing om berichten te verwerken als parallelle langlopende gegevensstromen (berichten zijn gekoppeld aan een stream met de [SessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid?view=azureservicebus-4.0.0#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) eigenschap voor het bericht). In dit model wordt op elk knooppunt in de betreffende toepassing concurreert om gegevensstromen, in plaats van berichten. Wanneer een stream is gegeven aan een knooppunt in beslag nemen, kan het knooppunt de status van de toepassingsstatus van de stroom met behulp van transacties bekijkt.
* Uw oplossing is vereist voor transactionele gedrag en atomisch bij het verzenden of ontvangen van meerdere berichten uit een wachtrij.
* De time-to-live (TTL) kenmerk van de werkbelasting toepassingsspecifieke kan groter zijn dan de 7 dagen.
* Uw toepassing berichten verwerkt die kunnen groter zijn dan 64 KB, maar is niet waarschijnlijk benadering 256 KB wordt beperkt.
* U werkt met een vereiste voor een model toegangsgroepen op basis van de wachtrijen en andere rechten machtigingen gelden voor afzenders en ontvangers.
* De wachtrijgrootte van uw groeit niet groter zijn dan 80 GB.
* Wilt u het AMQP 1.0 op standaarden gebaseerde messaging-protocol gebruiken. Zie voor meer informatie over AMQP [overzicht van Service Bus-AMQP](service-bus-amqp-overview.md).
* U kunt een uiteindelijke op basis van een wachtrij point-to-point communicatie te migreren naar een bericht exchange-patroon waarmee naadloze integratie van extra ontvangers (abonnees), die elk onafhankelijke kopieën van sommige of alle berichten die worden verzonden naar de wachtrij ontvangt voor ogen hebt. De laatste verwijst naar de mogelijkheid publiceren/abonneren systeemeigen geleverd door de Service Bus.
* Uw oplossing voor berichten moeten kunnen ondersteunen de garantie 'In de meeste-eens' levering zonder dat u de onderdelen van de aanvullende infrastructuur te bouwen.
* U wilt publiceren en gebruiken van batches van berichten.

## <a name="comparing-storage-queues-and-service-bus-queues"></a>Vergelijking van opslagwachtrijen en Service Bus-wachtrijen
De tabellen in de volgende secties bieden een logische groepering van functies van de wachtrij en kunnen u in een oogopslag de mogelijkheden die beschikbaar zijn in wachtrijen voor opslag en Service Bus-wachtrijen vergelijken.

## <a name="foundational-capabilities"></a>Fundamentele voorzieningen
Deze sectie worden enkele van de fundamentele queuing mogelijkheden van opslagwachtrijen en Service Bus-wachtrijen vergeleken.

| Vergelijkingscriteria | Opslagwachtrijen | Service Bus-wachtrijen |
| --- | --- | --- |
| Ordening garantie |**Nee** <br/><br>Zie de eerste opmerking in de sectie "Als u meer informatie" voor meer informatie.</br> |**Ja - First-In-First-Out (FIFO)**<br/><br>(met behulp van messaging-sessies) |
| Levering garantie |**Op de minste eenmaal** |**Op de minste eenmaal**<br/><br/>**In de meeste eens** |
| Ondersteuning voor atomic-bewerking |**Nee** |**Ja**<br/><br/> |
| Gedrag ontvangen |**Niet-blokkerende**<br/><br/>(uitgevoerd onmiddellijk als er geen nieuw bericht is gevonden) |**Blokkeren met of zonder time-out**<br/><br/>(biedt lange polling, of de ['Komeet techniek'](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Niet-blokkerende**<br/><br/>(door het gebruik van .NET beheerd API alleen) |
| Push-stijl-API |**Nee** |**Ja**<br/><br/>[OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage#Microsoft_ServiceBus_Messaging_QueueClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__) en **OnMessage** sessies .NET API. |
| Modus ontvangen |**Inspecteren & Lease** |**Inspecteren & vergrendelen**<br/><br/>**Ontvangen & verwijderen** |
| Modus van exclusieve toegang |**Op basis van de lease** |**Op basis van een vergrendeling** |
| Leasevergrendeling duur |**30 seconden (standaard)**<br/><br/>**7 dagen (maximum)** (u kunt vernieuwen of vrijgeven van een bericht lease met de [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API.) |**60 seconden (standaard)**<br/><br/>U kunt ook een bericht vergrendelen met verlengen de [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API. |
| Leasevergrendeling precisie |**Berichtniveau**<br/><br/>(elk bericht kan hebben een verschillende time-outwaarde, die u vervolgens indien nodig bijwerken kunt tijdens het verwerken van het bericht met behulp van de [UpdateMessage](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage.aspx) API) |**Wachtrijniveau**<br/><br/>(elke wachtrij heeft een vergrendeling precisie toegepast op alle van de berichten, maar kunt u ook het gebruik van de vergrendeling vernieuwen de [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API.) |
| Batch verwerkt ontvangen |**Ja**<br/><br/>(expliciet op te geven aantal berichten bij het ophalen van berichten, maximaal 32 berichten) |**Ja**<br/><br/>(inschakelen van de eigenschap van een vooraf vastgestelde impliciet of expliciet door het gebruik van transacties) |
| Batch verzenden |**Nee** |**Ja**<br/><br/>(door het gebruik van transacties of clientzijde batchverwerking) |

### <a name="additional-information"></a>Aanvullende informatie
* Berichten in de opslagwachtrij worden doorgaans first-in-first-out kunnen, maar soms volgorde; bijvoorbeeld, wanneer een bericht zichtbaarheid time-outduur verloopt (bijvoorbeeld als gevolg van een clienttoepassing gecrasht tijdens de verwerking). Wanneer de zichtbaarheid time-out is verstreken, wordt het bericht zichtbaar opnieuw in de wachtrij voor een andere werknemer naar deze wachtrij. Het nieuwe zichtbaar bericht mogelijk op dat moment in de wachtrij (om te worden uit wachtrij opnieuw geplaatst) worden geplaatst nadat een bericht dat in de wachtrij oorspronkelijk nadat deze.
* Het gegarandeerde FIFO-patroon in Service Bus-wachtrijen vereist het gebruik van de messaging-sessies. In het geval dat de toepassing vastloopt tijdens het verwerken van een bericht ontvangen de **inspecteren & vergrendelen** modus, de volgende keer dat de ontvanger van een wachtrij een messaging-sessie accepteert wordt gestart met het bericht is mislukt nadat de time-to-live (TTL) is verlopen.
* Opslagwachtrijen zijn ontworpen ter ondersteuning van standaard queuing scenario's, zoals toepassingsonderdelen ontkoppeling om meer schaalbaarheid en tolerantie voor fouten, load leveling en samenstellen van proceswerkstromen.
* Service Bus-wachtrijen ondersteunen de *op in de minste eenmaal* levering van garantie. Bovendien de *in de meeste eens* semantische met behulp van de sessiestatus voor het opslaan van de toepassingsstatus en met behulp van transacties moment berichten ontvangen en bijwerken van de sessiestatus kan worden ondersteund.
* Opslagwachtrijen bieden een uniforme en consistente programmeermodel via wachtrijen, tabellen en BLOBs – voor ontwikkelaars en operations-teams.
* Service Bus-wachtrijen bieden ondersteuning voor lokale transacties in de context van een enkele wachtrij.
* De **ontvangt en verwijdert** modus wordt ondersteund door Service Bus biedt de mogelijkheid te reduceren messaging bewerking count (en de bijbehorende kosten) voor de levering van verlaagde zekerheid.
* Leases bieden opslagwachtrijen de mogelijkheid om uit te breiden de leases voor berichten. Hierdoor kan de werknemers korte-leases op berichten te onderhouden. Dus als een werknemer vastloopt, kan het bericht snel opnieuw worden verwerkt door een andere werknemer. Bovendien een werknemer die de lease op een bericht kunt uitbreiden, als moet worden verwerkt het langer dan de leasetijd voor het huidige.
* Opslagwachtrijen bieden een zichtbaarheid time-out die u kunt instellen op de doelwachtrij plaatsen of waarbij van een bericht. U kunt bovendien bijwerken van een bericht met verschillende lease waarden tijdens runtime, en verschillende waarden voor berichten in de wachtrij met dezelfde werk. Service Bus vergrendeling time-outs zijn gedefinieerd in de metagegevens van de wachtrij. u kunt echter de vergrendeling verlengen door het aanroepen van de [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) methode.
* De maximale time-out is voor een blokkering ontvangstbewerking in Service Bus-wachtrijen 24 dagen. Time-outs REST gebaseerde hebben echter een maximumwaarde van 55 seconden.
* Client-side batchverwerking geleverd door de Service Bus, kunnen clients wachtrij om meerdere berichten in een enkel verzendbewerking batch te verwerken. Batchverwerking is alleen beschikbaar voor verzenden van asynchrone bewerkingen.
* Functies, zoals het maximum 200 TB opslagwachtrijen (meer wanneer u accounts virtualiseren) en onbeperkte wachtrijen maken het een ideaal platform voor SaaS-providers.
* Opslagwachtrijen bieden een flexibele en zodat overgedragen mechanisme voor toegangsbeheer.

## <a name="advanced-capabilities"></a>Geavanceerde mogelijkheden
Deze sectie worden vergeleken geavanceerde mogelijkheden van opslagwachtrijen en Service Bus-wachtrijen.

| Vergelijkingscriteria | Opslagwachtrijen | Service Bus-wachtrijen |
| --- | --- | --- |
| Geplande levering |**Ja** |**Ja** |
| Automatische dode letters |**Nee** |**Ja** |
| Toenemende wachtrij time-to-live-waarde |**Ja**<br/><br/>(via InPlace-update van zichtbaarheid time-out) |**Ja**<br/><br/>(aangeboden via een speciale API-functie) |
| Ondersteuning voor message poison |**Ja** |**Ja** |
| InPlace-update |**Ja** |**Ja** |
| Transactielogboek serverzijde |**Ja** |**Nee** |
| Metrische gegevens Storage |**Ja**<br/><br/>**Minuut van metrische gegevens**: biedt realtime metrische gegevens voor beschikbaarheid, TPS, API aanroepen, tellingen, fout tellingen en meer in realtime (geaggregeerd per minuut en gerapporteerd binnen een paar minuten uit wat net is er gebeurd in productie. Zie voor meer informatie [Storage Analytics Metrics](/rest/api/storageservices/fileservices/About-Storage-Analytics-Metrics). |**Ja**<br/><br/>(query's door aan te roepen bulksgewijs [GetQueues](/dotnet/api/microsoft.servicebus.namespacemanager.getqueues#Microsoft_ServiceBus_NamespaceManager_GetQueues)) |
| Statusbeheer |**Nee** |**Ja**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](/dotnet/api/microsoft.servicebus.messaging.entitystatus.active), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.disabled), [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.senddisabled), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.receivedisabled) |
| Automatisch doorsturen van berichten |**Nee** |**Ja** |
| Wachtrij-functie opschonen |**Ja** |**Nee** |
| Bericht-groepen |**Nee** |**Ja**<br/><br/>(met behulp van messaging-sessies) |
| De status van de toepassing per groep bericht |**Nee** |**Ja** |
| Detectie van duplicaten |**Nee** |**Ja**<br/><br/>(aan de kant van de afzender te configureren) |
| Bladeren door groepen berichten |**Nee** |**Ja** |
| Bericht-sessies door-ID ophalen |**Nee** |**Ja** |

### <a name="additional-information"></a>Aanvullende informatie
* Beide technologieën queuing inschakelen op een later tijdstip worden gepland voor ontvangst van een bericht.
* Automatisch doorsturen van wachtrij kunt duizenden wachtrijen automatisch doorsturen hun berichten naar een enkele wachtrij, van waaruit de ontvangende toepassing het bericht verbruikt. U kunt dit mechanisme gebruiken om te bereiken, beveiliging, transportbesturing en opslag tussen elke uitgever bericht isoleren.
* Opslagwachtrijen bieden ondersteuning voor het bijwerken van de inhoud van het bericht. U kunt deze functionaliteit voor persistent maken informatie over de status en van de incrementele voortgangsupdates gebruiken in het bericht zodat deze kan worden verwerkt vanaf het laatste controlepunt, in plaats van vanaf het begin starten. U kunt hetzelfde scenario door het gebruik van bericht sessies inschakelen met Service Bus-wachtrijen. Sessies, zodat u kunt opslaan en ophalen van de verwerkingsstatus van de toepassing (met behulp van [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) en [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState)).
* [Dode lettering](service-bus-dead-letter-queues.md), die wordt alleen ondersteund door Service Bus-wachtrijen kan nuttig zijn voor het isoleren van berichten die met succes kan niet worden verwerkt door de ontvangende toepassing of wanneer berichten hun bestemming als gevolg van een eigenschap verlopen time to live (TTL) kunnen niet bereiken. De TTL-waarde geeft aan hoe lang een bericht blijft in de wachtrij. Met Service Bus wordt het bericht verplaatst naar een speciale wachtrij $DeadLetterQueue wordt aangeroepen wanneer de TTL is verlopen.
* 'Verontreinigd' om berichten te zoeken in opslagwachtrijen, wanneer een bericht waarbij de toepassing onderzoekt de  **[DequeueCount](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueuemessage.dequeuecount.aspx)**  eigenschap van het bericht. Als **DequeueCount** groter is dan een opgegeven drempelwaarde, de toepassing het bericht verplaatst naar een toepassingsspecifieke ' ' wachtrij met onbestelde.
* Opslagwachtrij kunnen u een gedetailleerd logboek van alle transacties die worden uitgevoerd met de wachtrij als goed als samengevoegde metrische gegevens te verkrijgen. Beide opties zijn handig voor foutopsporing en inzicht in hoe uw toepassing gebruikmaakt van opslagwachtrijen. Ze zijn ook nuttig voor uw toepassing prestaties afstemmen en de kosten van het gebruik van wachtrijen.
* Het concept van '-berichten uitwisselen' wordt ondersteund door Service Bus kunt berichten die deel uitmaken van een bepaalde logische groep moet worden gekoppeld aan een bepaalde ontvanger die op zijn beurt een sessie-achtige affiniteit tussen berichten en hun respectieve ontvangers maakt. U kunt deze geavanceerde functionaliteit van Service Bus door in te stellen de [SessionID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) eigenschap voor een bericht. Ontvangers kunnen vervolgens luisteren op een bepaalde sessie-ID en ontvangen van berichten die de opgegeven sessie-id delen.
* De functie voor de detectie duplicatie automatisch wordt ondersteund door Service Bus-wachtrijen verwijdert dubbele berichten verzonden naar een wachtrij of onderwerp, op basis van de waarde van de [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) eigenschap.

## <a name="capacity-and-quotas"></a>Capaciteit en quota 's
Deze sectie vergelijkt opslagwachtrijen en Service Bus-wachtrijen vanuit het perspectief van [capaciteit en quota](service-bus-quotas.md) die mogelijk van toepassing is.

| Vergelijkingscriteria | Opslagwachtrijen | Service Bus-wachtrijen |
| --- | --- | --- |
| Maximale wachtrijgrootte |**500 TB**<br/><br/>(beperkt tot een [opslagaccountcapaciteit eenmalige](../storage/common/storage-introduction.md#queue-storage)) |**1 GB tot 80 GB**<br/><br/>(gedefinieerd bij het maken van een wachtrij en [inschakelen partitioneren](service-bus-partitioning.md) : Zie de sectie 'Aanvullende informatie') |
| Maximale berichtgrootte |**64 KB**<br/><br/>(48 KB bij gebruik van **Base64** codering)<br/><br/>Azure biedt ondersteuning voor grote berichten door een combinatie van wachtrijen en blobs – in dat geval u in de wachtrij plaatsen kunt van 200GB voor één item. |**256 KB** of **1 MB**<br/><br/>(met inbegrip van de kop- en hoofdtekst maximale header-grootte: 64 KB).<br/><br/>Afhankelijk van de [servicelaag](service-bus-premium-messaging.md). |
| Maximale bericht TTL |**7 dagen** |**`TimeSpan.Max`** |
| Maximum aantal wachtrijen |**Onbeperkt** |**10,000**<br/><br/>(per naamruimte van de service kan worden verhoogd) |
| Maximum aantal gelijktijdige clients |**Onbeperkt** |**Onbeperkt**<br/><br/>(de limiet van 100 gelijktijdige verbindingen alleen van toepassing op TCP-protocol-communicatie) |

### <a name="additional-information"></a>Aanvullende informatie
* De maximale grootte wachtrij zorgt ervoor dat Service Bus. De maximale wachtrijgrootte is opgegeven bij het maken van de wachtrij en een waarde tussen 1 en 80 GB kan hebben. Als de waarde voor de wachtrij ingesteld op het maken van de wachtrij is bereikt, worden extra binnenkomende berichten geweigerd en wordt een uitzondering wordt ontvangen door de aanroepende code. Zie voor meer informatie over quota in Service Bus [Service Bus quota](service-bus-quotas.md).
* In de [standaardcategorie](service-bus-premium-messaging.md), kunt u Service Bus-wachtrijen in 1, 2, 3, 4 of 5 GB-grootten (de standaardwaarde is 1 GB). U kunt wachtrijen maken in de laag Premium maximaal 80 GB groot. In de standaard-laag, met partitionering ingeschakeld (dit is de standaardwaarde), Service Bus maakt 16 partities voor elke GB die u opgeeft. Als zodanig als u een wachtrij die 5 GB groot is maken, met 16 partities de maximale wachtrijgrootte wordt (5 * 16) = 80 GB. U kunt de maximale grootte van uw gepartitioneerde wachtrij of onderwerp zien door te kijken op de vermelding ervan op de [Azure-portal][Azure portal]. In de laag Premium alleen 2 partities per wachtrij gemaakt.
* Met opslagwachtrijen, als de inhoud van het bericht geen XML-kluis is, moet deze zijn **Base64** gecodeerd. Als u **Base64**-coderen van het bericht, de nettolading van de gebruiker kan maximaal 48 KB, in plaats van 64 KB zijn.
* Met Service Bus-wachtrijen wordt elk bericht dat is opgeslagen in een wachtrij bestaat uit twee delen: een header en een hoofdtekst. De totale grootte van het bericht kan niet groter zijn dan de maximale berichtgrootte ondersteund door de servicetier.
* Wanneer clients met Service Bus-wachtrijen via de TCP-protocol communiceren, is het maximum aantal gelijktijdige verbindingen met een enkele Service Bus-wachtrij beperkt tot 100. Dit nummer wordt gedeeld tussen afzenders en ontvangers. Als dit quotum is bereikt, worden volgende aanvragen voor aanvullende verbindingen wordt geweigerd en wordt een uitzondering ontvangen door de aanroepende code. Deze limiet is niet ingesteld op clients die verbinding maken in de wachtrijen op basis van REST-API gebruiken.
* Als u meer dan 10.000 wachtrijen in een enkele Service Bus-naamruimte vereist, kunt u contact op met het ondersteuningsteam van Azure en een verhoging aanvragen. Als u wilt schalen voorbij 10.000 met Service Bus-wachtrijen, kunt u ook maken met behulp van aanvullende naamruimten de [Azure-portal][Azure portal].

## <a name="management-and-operations"></a>Beheer van en bewerkingen
Deze sectie vergelijkt de beheerfuncties die is geleverd door opslagwachtrijen en Service Bus-wachtrijen.

| Vergelijkingscriteria | Opslagwachtrijen | Service Bus-wachtrijen |
| --- | --- | --- |
| Management-protocol |**REST-via HTTP/HTTPS** |**REST-via HTTPS** |
| Runtime-protocol |**REST-via HTTP/HTTPS** |**REST-via HTTPS**<br/><br/>**AMQP 1.0 standaard (TCP-protocol met TLS)** |
| .NET API |**Ja**<br/><br/>(.NET storage Client-API) |**Ja**<br/><br/>(.NET Servicebus API) |
| Native C++ |**Ja** |**Ja** |
| Java-API |**Ja** |**Ja** |
| PHP-API |**Ja** |**Ja** |
| Node.js-API |**Ja** |**Ja** |
| Ondersteuning voor willekeurige metagegevens |**Ja** |**Nee** |
| Wachtrij-naamgevingsregels |**Maximaal 63 tekens bevatten**<br/><br/>(Letters in de naam van een wachtrij moeten kleine letters.) |**Maximaal 260 tekens bevatten**<br/><br/>(Wachtrij paden en namen zijn niet hoofdlettergevoelig). |
| De functie lengte wachtrij ophalen |**Ja**<br/><br/>(Geschatte waarde als berichten buiten de TTL verlopen zonder te worden verwijderd.) |**Ja**<br/><br/>(Exacte, punt in tijd waarde.) |
| Functie inspecteren |**Ja** |**Ja** |

### <a name="additional-information"></a>Aanvullende informatie
* Opslagwachtrijen bieden ondersteuning voor willekeurige kenmerken die kunnen worden toegepast op de omschrijving van de wachtrij in de vorm van naam/waarde-paren.
* Beide technologieën wachtrij bieden de mogelijkheid om te kijken in een bericht zonder te vergrendelen, die nuttig kunnen zijn bij het implementeren van een wachtrij explorer-browser-hulpprogramma.
* De .NET van Service Bus brokered messaging API's gebruiken voor de volledige-duplex TCP-verbindingen voor verbeterde prestaties in vergelijking met REST via HTTP en het standaard AMQP 1.0-protocol ondersteunen.
* Namen van opslagwachtrijen kan 3-63 tekens lang zijn, kan bestaan uit kleine letters, cijfers en afbreekstreepjes bevatten. Zie voor meer informatie [naamgeving van wachtrijen en metagegevens](/rest/api/storageservices/fileservices/Naming-Queues-and-Metadata).
* Namen van de service Bus-wachtrijen kunnen 260 tekens lang zijn en minder beperkend naamgevingsregels hebben. Namen van de service Bus-wachtrijen kunnen letters, cijfers, punten, afbreekstreepjes en onderstrepingstekens bevatten.

## <a name="authentication-and-authorization"></a>Verificatie en autorisatie
Deze sectie wordt de verificatie en autorisatie functies ondersteund door opslagwachtrijen en Service Bus-wachtrijen beschreven.

| Vergelijkingscriteria | Opslagwachtrijen | Service Bus-wachtrijen |
| --- | --- | --- |
| Authentication |**Symmetrische sleutel** |**Symmetrische sleutel** |
| Beveiligingsmodel |Gedelegeerde toegang via SAS-tokens. |SAS |
| Provider van identiteitsfederatie |**Nee** |**Ja** |

### <a name="additional-information"></a>Aanvullende informatie
* Elke aanvraag naar elk van de wachtrij technologieën moet worden geverifieerd. Openbare wachtrijen met anonieme toegang worden niet ondersteund. Met behulp van [SAS](service-bus-sas.md), u kunt dit scenario oplossen door het publiceren van een alleen-schrijven SAS, alleen-lezen SAS of zelfs een SAS volledige toegang.
* Het verificatieschema opgegeven door opslag wachtrijen omvat het gebruik van een symmetrische sleutel, die een HMAC hash-based Message Authentication Code () is, berekend met het algoritme SHA-256 en gecodeerd als een **Base64** tekenreeks. Zie voor meer informatie over het betreffende protocol [verificatie voor de Azure Storage-Services](/rest/api/storageservices/fileservices/Authentication-for-the-Azure-Storage-Services). Service Bus-wachtrijen ondersteunen een vergelijkbaar model met symmetrische sleutels. Zie voor meer informatie [Shared Access Signature-verificatie met Service Bus](service-bus-sas.md).

## <a name="conclusion"></a>Conclusie
Door een beter begrip van de twee technologieën heeft gekregen, kunt u zich kunt maken van een meer gefundeerde beslissing over welke technologie wachtrij moet worden gebruikt, en wanneer. De beslissing over het gebruik van Storage of Service Bus-wachtrijen is duidelijk afhankelijk van een aantal factoren. Deze factoren kunnen afhankelijk van geheugenlatentie de specifieke behoeften van uw toepassing en de bijbehorende architectuur. Als uw toepassing al gebruikmaakt van de belangrijkste mogelijkheden van Microsoft Azure, u mogelijk liever kiezen opslagwachtrijen, vooral als u nodig hebt basic communicatie en uitwisseling van berichten tussen services of wachtrijen nodig die groter zijn dan 80 GB groot.

Omdat Service Bus-wachtrijen een aantal geavanceerde functies, zoals sessies, transacties bieden, detectie, automatische dubbele mogelijkheden voor verwerking van onbestelbare berichten en duurzame publiceren/abonneren ze kunnen een uitstekende keuze is als u een hybride-toepassing bouwt of als uw toepassing anders deze functies vereist.

## <a name="next-steps"></a>Volgende stappen
De volgende artikelen bevatten meer richtlijnen en informatie over het gebruik van Storage of Service Bus-wachtrijen.

* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Het gebruik van de Queue Storage-Service](../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Aanbevolen procedures voor verbeterde prestaties met Service Bus brokered messaging](service-bus-performance-improvements.md)
* [Inleiding tot wachtrijen en onderwerpen in de Azure Service Bus (blogbericht)](http://www.code-magazine.com/article.aspx?quickid=1112041)
* [De ontwikkelaarshandleiding voor Servicebus](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
* [Gebruik de wachtrijservices in Azure](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)

[Azure portal]: https://portal.azure.com

