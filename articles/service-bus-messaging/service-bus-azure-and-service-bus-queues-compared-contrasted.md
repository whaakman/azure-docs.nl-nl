---
title: Azure Storage-wachtrijen en Service Bus-wachtrijen overeenkomsten en verschillen | Microsoft Docs
description: Analyseert verschillen en overeenkomsten tussen de twee typen wachtrijen die worden aangeboden door Azure.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: f07301dc-ca9b-465c-bd5b-a0f99bab606b
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 09/05/2018
ms.author: spelluru
ms.openlocfilehash: f48e9a5600dca1e13d6ee94a675d5bc824915118
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393958"
---
# <a name="storage-queues-and-service-bus-queues---compared-and-contrasted"></a>Storage-wachtrijen en Service Bus-wachtrijen: overeenkomsten en verschillen
In dit artikel analyseert de verschillen en overeenkomsten tussen de twee typen wachtrijen die momenteel worden aangeboden door Microsoft Azure: Storage-wachtrijen en Service Bus-wachtrijen. U kunt deze informatie gebruiken om de verschillende technologieën te vergelijken en tegen elkaar af te zetten zodat u een weloverwogen beslissing kunt nemen en de oplossing kiest die beste voldoet aan uw behoeften.

## <a name="introduction"></a>Inleiding
Azure ondersteunt twee typen mechanismen voor wachtrij: **opslagwachtrijen** en **Service Bus-wachtrijen**.

**Storage-wachtrijen**, die deel uitmaken van de [Azure storage](https://azure.microsoft.com/services/storage/) infrastructuur, functie een eenvoudige op REST-gebaseerde GET/PUT/PEEK-interface biedt betrouwbare, vaste berichtenuitwisseling binnen en tussen services.

**Service Bus-wachtrijen** deel uitmaken van een bredere [Azure-messaging](https://azure.microsoft.com/services/service-bus/) infrastructuur die ondersteuning biedt voor queuing en publiceren/abonneren en meer geavanceerde integratiepatronen. Zie voor meer informatie over Service Bus-wachtrijen/onderwerpen/abonnementen, de [overzicht van Service Bus](service-bus-messaging-overview.md).

Hoewel beide queuing technologieën gelijktijdig bestaan, kennisgemaakt Storage-wachtrijen eerst als een toegewezen wachtrij opslagmechanisme gebouwd op Azure Storage-services. Service Bus-wachtrijen zijn gebaseerd op de bredere messaging-infrastructuur die is ontworpen voor integratie van toepassingen of onderdelen van de toepassing die verschillende communicatieprotocollen, gegevenscontracten, vertrouwde domeinen en/of netwerkomgevingen kunnen omvatten.

## <a name="technology-selection-considerations"></a>Overwegingen bij het selecteren van technologie
Zowel Storage-wachtrijen en Service Bus-wachtrijen zijn implementaties van de message Queuing-service die momenteel worden aangeboden door Microsoft Azure. Elk heeft een enigszins functieset, wat betekent dat u kunt een of andere of beide, afhankelijk van de behoeften van uw specifieke oplossing of u het oplossen van zakelijke en technische-probleem gebruiken.

Bij het bepalen van welke queuing technologie geschikt is voor het doel voor een bepaalde oplossing, oplossingsarchitecten en ontwikkelaars moeten rekening houden met deze aanbevelingen. Zie de volgende sectie voor meer informatie.

Als ontwikkelaar/solution architect, **Overweeg het gebruik van Storage-wachtrijen** wanneer:

* Uw toepassing moet meer dan 80 GB aan berichten opslaan in een wachtrij.
* Uw toepassing wil bijhouden uitgevoerd voor het verwerken van een bericht in de wachtrij. Dit is handig als het verwerken van een bericht werkrol uitvalt. Een volgende worker kunt die informatie vervolgens gebruiken om door te gaan van waar de voorafgaande werknemer afgebroken.
* U naast de logboeken van alle transacties uitgevoerd op uw wachtrijen nodig hebt.

Als ontwikkelaar/solution architect, **Overweeg het gebruik van Service Bus-wachtrijen** wanneer:

* Uw oplossing moet kunnen ontvangen van berichten zonder op te vragen voor de wachtrij. Met Service Bus, dit kan worden bereikt door het gebruik van de long-polling ontvangen opnieuw met de TCP-protocollen die ondersteuning biedt voor Service Bus.
* Uw oplossing vereist dat de wachtrij voor een gegarandeerde first-in-first-out (FIFO) besteld levering.
* Uw oplossing moet mogelijk zijn ter ondersteuning van automatische detectie van duplicaten.
* U wilt dat uw toepassing om berichten te verwerken als een stroom met parallelle langlopende (berichten zijn gekoppeld aan een stroom met de [SessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid) eigenschap voor het bericht). In dit model wordt elk knooppunt in de betreffende toepassing concurreert voor gegevensstromen, in plaats van berichten. Wanneer een stroom is opgegeven met een knooppunt in beslag nemen, kunt de status van de status van de toepassing-stream met behulp van transacties bekijken in het knooppunt.
* Uw oplossing is vereist voor transactionele gedrag en atomisch bij het verzenden of ontvangen van meerdere berichten van een wachtrij.
* Uw toepassing verwerkt berichten die kunnen groter zijn dan 64 KB, maar niet waarschijnlijk benadering van 256 KB wordt beperkt.
* U werkt met een vereiste voor een model op basis van de rol aan de wachtrijen en andere rights/machtigingen voor afzenders en ontvangers.
* De wachtrijgrootte van uw wordt niet groter zijn dan 80 GB.
* Wilt u de AMQP 1.0-standaarden gebaseerde messaging-protocol gebruiken. Zie voor meer informatie over AMQP [overzicht van Service Bus AMQP](service-bus-amqp-overview.md).
* Zodat u ze een uiteindelijke migratie van op basis van wachtrij point-to-point communicatie naar een bericht exchange-patroon waarmee de naadloze integratie van extra ontvangers (abonnees), die elk onafhankelijke exemplaren van sommige of alle ontvangt berichten die worden verzonden naar de wachtrij. De laatste verwijst naar de mogelijkheid voor publiceren/abonneren systeemeigen geleverd door Service Bus.
* Uw oplossing voor berichten moeten kunnen ondersteunen de garantie van de levering van 'In de meeste-eens' zonder de noodzaak voor u de onderdelen van de aanvullende infrastructuur te bouwen.
* U wilt mogelijk om te publiceren en gebruiken van batches van berichten.

## <a name="comparing-storage-queues-and-service-bus-queues"></a>Storage-wachtrijen en Service Bus-wachtrijen vergelijken
De tabellen in de volgende secties bieden een logische groepering van de wachtrijfuncties en kunnen u in één oogopslag de mogelijkheden die beschikbaar zijn in Azure Storage-wachtrijen en Service Bus-wachtrijen vergelijken.

## <a name="foundational-capabilities"></a>Fundamentele mogelijkheden
In deze sectie worden enkele van de fundamentele queuing mogelijkheden geboden door Storage-wachtrijen en Service Bus-wachtrijen vergeleken.

| Vergelijkingscriteria | Opslagwachtrijen | Service Bus-wachtrijen |
| --- | --- | --- |
| Volgorde gegarandeerd |**Nee** <br/><br>Zie voor meer informatie, de eerste opmerking in de sectie 'Extra gegevens'.</br> |**Ja - First In First Out (FIFO)**<br/><br>(door het gebruik van messaging sessies) |
| Aflevering gegarandeerd |**Op-één keer** |**Op-één keer**<br/><br/>**In de meeste eens** |
| Atomische bewerking ondersteuning |**Nee** |**Ja**<br/><br/> |
| Gedrag ontvangen |**Niet-blokkerende**<br/><br/>(uitgevoerd onmiddellijk als er geen nieuw bericht is gevonden.) |**Met/zonder time-out blokkeren**<br/><br/>(biedt long polling, of de ["Comet techniek"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Niet-blokkerende**<br/><br/>(door het gebruik van .NET beheerde API alleen) |
| Push-stijl-API |**Nee** |**Ja**<br/><br/>[OnMessage](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage#Microsoft_ServiceBus_Messaging_QueueClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__) en **OnMessage** sessies .NET API. |
| Modus ontvangen |**Een & Lease** |**Een & vergrendelen**<br/><br/>**Ontvangen en verwijderen** |
| Exclusieve toegang-modus |**Op basis van een lease** |**Op basis van een vergrendeling** |
| Leasevergrendeling duur |**30 seconden (standaard)**<br/><br/>**7 dagen (maximum)** (u kunt verlengen of vrijgeven van een bericht lease met de [UpdateMessage](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage) API.) |**60 seconden (standaard)**<br/><br/>U kunt vanaf een bericht vergrendelen met de [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API. |
| Leasevergrendeling precisie |**Het berichtniveau van**<br/><br/>(elk bericht kan hebben een andere time-outwaarde, die u vervolgens indien nodig bijwerken kunt tijdens het verwerken van het bericht met behulp van de [UpdateMessage](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage) API) |**Het wachtrijniveau van de**<br/><br/>(elke wachtrij is een vergrendeling precisie toegepast op alle van de berichten, maar u kunt vanaf de vergrendeling met behulp van de [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API.) |
| Batchgewijs ontvangen |**Ja**<br/><br/>(expliciet op te geven aantal berichten bij het ophalen van berichten, tot een maximum van 32 berichten) |**Ja**<br/><br/>(inschakelen van de eigenschap van een vooraf vastgestelde impliciet of expliciet door het gebruik van transacties) |
| Batch verzenden |**Nee** |**Ja**<br/><br/>(door het gebruik van transacties of batchverwerking van client-side) |

### <a name="additional-information"></a>Aanvullende informatie
* Berichten in de Storage-wachtrijen worden meestal first-in-first-out kunnen, maar soms ze niet de juiste volgorde; bijvoorbeeld, wanneer van een bericht zichtbaarheid time-out duur is verlopen (bijvoorbeeld, als gevolg van een clienttoepassing vastloopt tijdens het verwerken). Wanneer de time-out voor zichtbaarheid is verlopen, wordt het bericht zichtbaar op de wachtrij voor een andere werknemer uit de wachtrij verwijderen het opnieuw. Op dat moment kan het bericht pas zichtbaar in de wachtrij (om te worden uit de wachtrij genomen opnieuw) worden geplaatst nadat een bericht dat in de wachtrij geplaatste oorspronkelijk nadat deze.
* De gegarandeerde FIFO-patroon in Service Bus-wachtrijen vereist het gebruik van de messaging-sessies. In het geval dat de toepassing vastloopt tijdens het verwerken van een bericht wordt ontvangen in de **bekijken & vergrendelen** modus, de volgende keer dat de ontvanger van een wachtrij een berichtensessie accepteert deze begint met het bericht is mislukt nadat de time-to-live (TTL) periode is verlopen.
* Storage-wachtrijen zijn ontworpen ter ondersteuning van standard queuing's, zoals toepassingsonderdelen ontkoppeling zodat de schaalbaarheid en tolerantie voor fouten, load leveling en het samenstellen van proceswerkstromen.
* Service Bus-wachtrijen ondersteunen de *op-één keer* bezorging gegarandeerd. Bovendien de *op in de meeste eenmaal* semantische kan worden ondersteund met behulp van de sessiestatus voor het opslaan van de status van de toepassing en met behulp van transacties atomisch ontvangen van berichten en bijwerken van de sessiestatus.
* Storage-wachtrijen bieden een uniforme en consistente programmeermodel voor wachtrijen, tabellen en BLOBs – voor ontwikkelaars en bewerkingsteams.
* Service Bus-wachtrijen bieden ondersteuning voor lokale transacties in de context van één wachtrij.
* De **ontvangt en verwijdert** modus wordt ondersteund door Service Bus biedt de mogelijkheid om te beperken van de berichten item (en de bijbehorende kosten) ruil verlaagde levering assurance.
* Storage-wachtrijen bieden de mogelijkheid om uit te breiden de leases voor berichten leases. Hiermee wordt de werknemers te handhaven korte leases op berichten. Dus als een werkrol uitvalt, kan het bericht snel opnieuw worden verwerkt door een andere werknemer. Bovendien een werknemer die de lease op een bericht kunt uitbreiden, als moet worden verwerkt het langer dan de huidige leasetijd.
* Storage-wachtrijen bieden een time-out voor zichtbaarheid die u kunt instellen op de enqueuing of eruit worden verwijderd van een bericht. U kunt bovendien bijwerken van een bericht met verschillende lease waarden tijdens runtime en bijwerken van verschillende waarden voor berichten in de dezelfde wachtrij. Service Bus-lock-outs zijn gedefinieerd in de metagegevens van de wachtrij. echter, kunt u de vergrendeling vernieuwen door het aanroepen van de [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) methode.
* De maximale time-out is voor bewerking een blokkering ontvangen in Service Bus-wachtrijen 24 dagen. Op basis van REST-outs hebben echter een maximumwaarde van 55 seconden.
* Client-side batchverwerking geleverd door Service Bus, kunt een client wachtrij om meerdere berichten in een bewerking voor het verzenden van één batch te verwerken. Batchverwerking is alleen beschikbaar voor het verzenden van asynchrone bewerkingen.
* Functies zoals het maximum 200 TB van opslagwachtrijen (meer wanneer u accounts virtualiseren) en een onbeperkt aantal wachtrijen kunnen u een ideaal platform voor SaaS-providers.
* Storage-wachtrijen bieden een flexibele en goed presterende overgedragen mechanisme voor toegangsbeheer.

## <a name="advanced-capabilities"></a>Geavanceerde mogelijkheden
In deze sectie worden vergeleken geavanceerde mogelijkheden van Storage-wachtrijen en Service Bus-wachtrijen.

| Vergelijkingscriteria | Opslagwachtrijen | Service Bus-wachtrijen |
| --- | --- | --- |
| Geplande bezorging |**Ja** |**Ja** |
| Automatische onbestelbare |**Nee** |**Ja** |
| Toenemende wachtrij time-to-live-waarde |**Ja**<br/><br/>(via InPlace-update van time-out voor zichtbaarheid) |**Ja**<br/><br/>(geleverd via een toegewezen API-functie) |
| Onverwerkbare berichten ondersteuning |**Ja** |**Ja** |
| InPlace-update |**Ja** |**Ja** |
| Server-side-transactielogboek |**Ja** |**Nee** |
| Metrische opslaggegevens |**Ja**<br/><br/>**Minuut metrische gegevens**: biedt realtime metrische gegevens voor beschikbaarheid, TPS, API-aanroepen telt het aantal, telt het aantal fout, en meer, allemaal in realtime (geaggregeerd per minuut en die is gerapporteerd binnen een paar minuten uit wat er zojuist is gebeurd in de productieomgeving. Zie voor meer informatie, [over Storage Analytics Metrics](/rest/api/storageservices/fileservices/About-Storage-Analytics-Metrics). |**Ja**<br/><br/>(bulk-query's door het aanroepen van [GetQueues](/dotnet/api/microsoft.servicebus.namespacemanager.getqueues#Microsoft_ServiceBus_NamespaceManager_GetQueues)) |
| Statusbeheer |**Nee** |**Ja**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](/dotnet/api/microsoft.servicebus.messaging.entitystatus), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus), [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus) |
| Automatisch doorsturen van bericht |**Nee** |**Ja** |
| Functie van de wachtrij verwijderen |**Ja** |**Nee** |
| Bericht-groepen |**Nee** |**Ja**<br/><br/>(door het gebruik van messaging sessies) |
| De status van de toepassing per groep bericht |**Nee** |**Ja** |
| Detectie van duplicaten |**Nee** |**Ja**<br/><br/>(configureerbaar in de kant van de afzender) |
| Bericht groepen bladeren |**Nee** |**Ja** |
| Berichtsessies ophalen op ID |**Nee** |**Ja** |

### <a name="additional-information"></a>Aanvullende informatie
* Beide queuing technologieën kunnen een bericht moet worden gepland voor de levering op een later tijdstip.
* Wachtrij automatisch doorsturen kunt duizenden van wachtrijen naar hun berichten met één wachtrij, van waaruit de ontvangende toepassing het bericht verbruikt automatisch doorsturen. U kunt dit mechanisme gebruiken om te bereiken, beveiliging, Controlestroom en opslag tussen elke uitgever bericht isoleren.
* Storage-wachtrijen bieden ondersteuning voor het bijwerken van inhoud van het bericht. U kunt deze functionaliteit voor informatie over de permanente status en van incrementele voortgangsupdates gebruiken in het bericht zodat deze kan worden verwerkt vanaf het laatste controlepunt, in plaats van helemaal vanaf. Met Service Bus-wachtrijen, kunt u hetzelfde scenario door het gebruik van berichtsessies inschakelen. Sessies kunt u opslaan en ophalen van de verwerkingsstatus van de toepassing (met behulp van [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) en [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState)).
* [Dode lettering](service-bus-dead-letter-queues.md), die wordt alleen ondersteund door Service Bus-wachtrijen, kan nuttig zijn voor het isoleren van berichten die met succes kan niet worden verwerkt door de ontvangende toepassing of als berichten hun bestemming vanwege een verlopen time-to-live (niet bereiken De eigenschap TTL). De TTL-waarde geeft aan hoe lang een bericht in de wachtrij blijft. Met Service Bus, wordt het bericht wordt verplaatst naar een speciale wachtrij $DeadLetterQueue wordt aangeroepen wanneer de TTL is verlopen.
* "Beheer van Onverwerkbare" berichten in wachtrijen van opslag, wanneer een bericht eruit worden verwijderd de gewenste toepassing vinden onderzoekt de [DequeueCount](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueuemessage.dequeuecount.aspx) eigenschap van het bericht. Als **DequeueCount** groter is dan een bepaalde drempelwaarde, de toepassing wordt het bericht verplaatst naar een toepassingsspecifieke "" wachtrij voor onbestelbare.
* Storage-wachtrijen kunnen u een gedetailleerd logboek van alle transacties uitgevoerd voor de wachtrij, als ook als samengevoegde metrische gegevens te verkrijgen. Beide van deze opties zijn handig voor foutopsporing en inzicht in hoe uw toepassing gebruikmaakt van Storage-wachtrijen. Ze zijn ook nuttig voor prestatieverbetering uw toepassing en de kosten van het gebruik van wachtrijen.
* Het concept van "berichtsessies' wordt ondersteund door Service Bus kan berichten die deel uitmaken van een bepaalde logische groep moet worden gekoppeld aan een bepaalde ontvanger die op zijn beurt zorgt voor een sessie-achtige affiniteit tussen berichten en hun respectieve ontvangers. U kunt deze geavanceerde functionaliteit van Service Bus door in te stellen de [SessionID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) eigenschap voor een bericht. Ontvangers kunnen vervolgens luisteren op een specifieke sessie-ID en ontvangen van berichten die de opgegeven sessie-id delen.
* De functie voor de detectie duplicatie automatisch wordt ondersteund door Service Bus-wachtrijen verwijdert dubbele berichten verzonden naar een wachtrij of onderwerp, op basis van de waarde van de [MessageId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) eigenschap.

## <a name="capacity-and-quotas"></a>Capaciteit en quota
In deze sectie worden vergeleken Storage-wachtrijen en Service Bus-wachtrijen vanuit het perspectief van [capaciteit en quota's](service-bus-quotas.md) die mogelijk van toepassing is.

| Vergelijkingscriteria | Opslagwachtrijen | Service Bus-wachtrijen |
| --- | --- | --- |
| Maximale wachtrijgrootte |**500 TB**<br/><br/>(beperkt tot een [enkel opslagaccountcapaciteit](../storage/common/storage-introduction.md#queue-storage)) |**1 GB tot 80 GB**<br/><br/>(gedefinieerd bij het maken van een wachtrij en [inschakelen partitioneren](service-bus-partitioning.md) : Zie de sectie "Extra informatie") |
| Maximale berichtgrootte |**64 KB**<br/><br/>(48 KB bij het gebruik van **Base64** codering)<br/><br/>Azure biedt ondersteuning voor grote berichten door een combinatie van wachtrijen en blobs – op dat moment u in de wachtrij zetten kunt tot 200 GB voor één item. |**256 KB** of **1 MB**<br/><br/>(met inbegrip van de koptekst en hoofdtekst, maximale header-grootte: 64 KB).<br/><br/>Afhankelijk van de [servicelaag](service-bus-premium-messaging.md). |
| Maximumgrootte van een bericht TTL |**Oneindige** (vanaf 27-07-2017-api-versie) |**TimeSpan.Max** |
| Maximum aantal wachtrijen |**Onbeperkt** |**10,000**<br/><br/>(per service-naamruimte) |
| Maximum aantal gelijktijdige clients |**Onbeperkt** |**Onbeperkt**<br/><br/>(de limiet van 100 gelijktijdige verbindingen geldt alleen voor op basis van het protocol TCP-communicatie) |

### <a name="additional-information"></a>Aanvullende informatie
* Service Bus wordt de maximale grootte wachtrij afgedwongen. De maximale wachtrijgrootte is opgegeven bij het maken van de wachtrij en een waarde tussen 1 en 80 GB kan hebben. Als de waarde voor de wachtrij ingesteld op het maken van de wachtrij is bereikt, wordt extra binnenkomende berichten geweigerd en wordt een uitzondering worden ontvangen door de aanroepende code. Zie voor meer informatie over quota voor Service Bus, [Service Bus-quota](service-bus-quotas.md).
* Partitioneren wordt niet ondersteund in de [Premium-laag](service-bus-premium-messaging.md). U kunt Service Bus-wachtrijen in 1, 2, 3, 4 of 5 GB-grootten (de standaardwaarde is 1 GB) kunt maken in de laag standaard. In de standaard-laag, met partitionering ingeschakeld (dit is de standaardwaarde), Service Bus 16 partities maakt voor elke GB die u opgeeft. Als zodanig, als u een wachtrij die is 5 GB groot maakt, met 16 partities de maximale wachtrijgrootte wordt (5 * 16) = 80 GB. U kunt de maximale grootte van de gepartitioneerde wachtrij of onderwerp zien door te kijken op de vermelding ervan op de [Azure-portal][Azure portal].
* Met Storage-wachtrijen, als de inhoud van het bericht geen XML-kluis is, moet deze zijn **Base64** gecodeerd. Als u **Base64**-coderen van het bericht, de nettolading van de gebruiker mag maximaal 48 KB, in plaats van 64 KB.
* Met Service Bus-wachtrijen wordt elk bericht dat wordt opgeslagen in een wachtrij bestaat uit twee delen: een koptekst en een hoofdtekst. De totale grootte van het bericht kan niet groter zijn dan de maximale berichtgrootte die wordt ondersteund door de service tier.
* Wanneer clients met Service Bus-wachtrijen via het TCP-protocol communiceren, is het maximum aantal gelijktijdige verbindingen met één Service Bus-wachtrij beperkt tot 100. Dit getal wordt gedeeld tussen afzenders en ontvangers. Als dit quotum is bereikt, wordt elke volgende keer dat u meer verbindingen geweigerd en wordt een uitzondering worden ontvangen door de aanroepende code. Deze limiet is niet ingesteld op clients verbinding maken met de wachtrijen op basis van REST-API gebruiken.
* Als u meer dan 10.000 wachtrijen in een enkele Service Bus-naamruimte nodig hebt, kunt u contact op met de ondersteuning van Azure-team en een verhoging. Als u wilt schalen meer dan 10.000 met Service Bus-wachtrijen, kunt u ook maken met behulp van aanvullende naamruimten de [Azure-portal][Azure portal].

## <a name="management-and-operations"></a>Beheer en bewerkingen
In deze sectie worden de beheerfuncties die is geleverd door de Storage-wachtrijen en Service Bus-wachtrijen met elkaar vergeleken.

| Vergelijkingscriteria | Opslagwachtrijen | Service Bus-wachtrijen |
| --- | --- | --- |
| Management-protocol |**REST via HTTP/HTTPS** |**REST-via HTTPS** |
| Runtime-protocol |**REST via HTTP/HTTPS** |**REST-via HTTPS**<br/><br/>**AMQP 1.0 standaard (TCP met TLS)** |
| .NET API |**Ja**<br/><br/>(.NET storage Client-API) |**Ja**<br/><br/>(.NET Servicebus API) |
| Native C++ |**Ja** |**Ja** |
| Java-API |**Ja** |**Ja** |
| PHP-API |**Ja** |**Ja** |
| Node.js-API |**Ja** |**Ja** |
| Ondersteuning voor willekeurige metagegevens |**Ja** |**Nee** |
| Naamgevingsregels voor wachtrij |**Maximaal 63 tekens bevatten**<br/><br/>(Letters in de naam van een wachtrij moeten kleine letters.) |**Maximaal 260 tekens bevatten**<br/><br/>(Paden van de wachtrij en de namen zijn niet hoofdlettergevoelig). |
| Ophalen van de functie lengte wachtrij |**Ja**<br/><br/>(Bij benadering waarde als berichten buiten de TTL-waarde verlopen zonder te worden verwijderd.) |**Ja**<br/><br/>(Exacte, point-in-time-waarde.) |
| Functie bekijken |**Ja** |**Ja** |

### <a name="additional-information"></a>Aanvullende informatie
* Storage-wachtrijen bieden ondersteuning voor willekeurige kenmerken die kunnen worden toegepast op de beschrijving van de wachtrij in de vorm van de naam/waarde-paren.
* Beide technologieën wachtrij bieden de mogelijkheid om te kijken in een bericht zonder te vergrendelen, die kan nuttig zijn bij het implementeren van een wachtrij explorer-browser-hulpprogramma.
* .NET Service Bus brokered messaging API's gebruikmaken van full-duplex TCP-verbindingen voor betere prestaties in vergelijking met REST via HTTP en het standaard AMQP 1.0-protocol ondersteunen.
* Namen van de Storage-wachtrijen tussen 3 en 63 tekens lang is, kunnen kleine letters, cijfers en afbreekstreepjes bevatten. Zie voor meer informatie, [naamgeving van wachtrijen en metagegevens](/rest/api/storageservices/fileservices/Naming-Queues-and-Metadata).
* Namen van de service Bus-wachtrijen kunnen maximaal 260 tekens lang zijn en minder beperkend naamgevingsregels hebben. Namen van de service Bus-wachtrijen kunnen letters, cijfers, punten, afbreekstreepjes en onderstrepingstekens bevatten.

## <a name="authentication-and-authorization"></a>Verificatie en autorisatie
Deze sectie wordt de verificatie en autorisatie functies die worden ondersteund door de Storage-wachtrijen en Service Bus-wachtrijen beschreven.

| Vergelijkingscriteria | Opslagwachtrijen | Service Bus-wachtrijen |
| --- | --- | --- |
| Verificatie |**Symmetrische sleutel** |**Symmetrische sleutel** |
| Beveiligingsmodel |Gedelegeerde toegang via SAS-tokens. |SAS |
| Id-provider-Federatie |**Nee** |**Ja** |

### <a name="additional-information"></a>Aanvullende informatie
* Elke aanvraag aan een van de wachtrij technologieën moet worden geverifieerd. Openbare wachtrijen met anonieme toegang worden niet ondersteund. Met behulp van [SAS](service-bus-sas.md), kunt u dit scenario oplossen door het publiceren van een alleen-schrijven SAS, alleen-lezen SAS of zelfs een SAS volledige toegang.
* Het verificatieschema opgegeven door Storage-wachtrijen omvat het gebruik van een symmetrische sleutel, die een HMAC hash-based Message Authentication Code (), berekend met het algoritme SHA-256 en gecodeerd als een **Base64** tekenreeks. Zie voor meer informatie over het betreffende protocol [-verificatie voor de Azure Storage-Services](/rest/api/storageservices/fileservices/Authentication-for-the-Azure-Storage-Services). Service Bus-wachtrijen ondersteunen een vergelijkbaar model met behulp van symmetrische sleutels. Zie voor meer informatie, [Shared Access Signature-verificatie met Service Bus](service-bus-sas.md).

## <a name="conclusion"></a>Conclusie
Door het verkrijgen van een beter begrip van de twee technologieën, kunt u zich op een weloverwogen beslissing nemen over welke technologie wachtrij moet worden gebruikt, en wanneer. De beslissing over wanneer u Storage-wachtrijen of Service Bus-wachtrijen is duidelijk afhankelijk is van een aantal factoren. Deze factoren kunnen sterk afhankelijk zijn van de specifieke behoeften van uw toepassing en de bijbehorende architectuur. Als uw toepassing al gebruikmaakt van de belangrijkste mogelijkheden van Microsoft Azure, kunt u wellicht beter om te kiezen Storage-wachtrijen, met name als u nodig hebt basic communicatie en uitwisseling van berichten tussen services of wachtrijen nodig die groter is dan 80 GB groot.

Omdat Service Bus-wachtrijen een aantal geavanceerde functies, zoals sessies, transacties bieden, detectie, automatische dubbele onbestelbare en duurzaam publiceren/abonneren mogelijkheden, zij je misschien een uitstekende keuze als u een hybride bouwt de toepassing of als uw toepassing anders deze functies vereist.

## <a name="next-steps"></a>Volgende stappen
De volgende artikelen bieden meer richtlijnen en informatie over het gebruik van Storage-wachtrijen of Service Bus-wachtrijen.

* [Aan de slag met Service Bus-wachtrijen](service-bus-dotnet-get-started-with-queues.md)
* [Het gebruik van de Queue Storage-Service](../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Aanbevolen procedures voor prestatieverbeteringen met Service Bus brokered messaging](service-bus-performance-improvements.md)
* [Inleiding tot wachtrijen en onderwerpen in Azure Service Bus (blogbericht)](http://www.code-magazine.com/article.aspx?quickid=1112041)
* [De Developer's Guide voor Servicebus](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
* [Met behulp van de Queuing-Service in Azure](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)

[Azure portal]: https://portal.azure.com

