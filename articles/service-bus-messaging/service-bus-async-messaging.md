---
title: Asynchrone Service Bus-berichtenservice | Microsoft Docs
description: Beschrijving van Azure Service Bus asynchrone berichten.
services: service-bus-messaging
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: f1435549-e1f2-40cb-a280-64ea07b39fc7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2018
ms.author: spelluru
ms.openlocfilehash: 9bacce96e65a7aef611bec3ddae8b1872d5f9fae
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47391460"
---
# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asynchrone berichtpatronen en hoge beschikbaarheid

Asynchrone berichten kan worden geïmplementeerd in een aantal verschillende manieren. Azure Service Bus ondersteunt met wachtrijen, onderwerpen en abonnementen, asynchronism via een winkel en een mechanisme voor doorsturen. Verzenden van berichten met wachtrijen en onderwerpen in de normale werking (synchrone) en ontvangen van berichten van wachtrijen en abonnementen. Toepassingen die u schrijft, is afhankelijk van deze entiteiten die altijd beschikbaar worden gesteld. Wanneer de entiteitsstatus wordt gewijzigd, vanwege allerlei omstandigheden, moet u een manier om aan te bieden van een entiteit met verminderde functionaliteit dat is voor de meeste behoeften geschikt.

Asynchrone berichtpatronen toepassingen meestal gebruiken om in te schakelen van een aantal scenario's voor communicatie. U kunt toepassingen waarin clients berichten naar services, verzenden kunnen zelfs wanneer de service wordt niet uitgevoerd op te bouwen. Voor toepassingen die ervaring pieken in communicatie, een wachtrij kunt niveau van de belasting door te geven van een interne buffer-communicatie. Ten slotte, krijgt u een eenvoudige maar effectieve load balancer om berichten te verdelen over meerdere machines.

U kunt een aantal verschillende manieren waarop deze entiteiten kunnen worden weergegeven is niet beschikbaar voor een duurzaam berichtensysteem gehandhaafd beschikbaarheid van elk van deze entiteiten. In het algemeen zien we de entiteit niet meer beschikbaar zijn voor toepassingen die we in de volgende manieren schrijven:

* Kan het verzenden van berichten.
* Kan geen berichten ontvangen.
* Kan geen entiteiten beheren (maken, ophalen, bijwerken of verwijderen van entiteiten).
* Kan geen contact op met de service.

Voor elk van deze fouten bestaan verschillende foutmodi waarmee een toepassing om door te gaan om uit te voeren werk op basis van bepaalde mate van verminderde functionaliteit. Bijvoorbeeld, een systeem dat u kunt berichten verzenden, maar deze niet meer ontvangen kunt nog steeds orders van klanten ontvangen maar deze orders kan niet worden verwerkt. In dit onderwerp worden de mogelijke problemen die kunnen optreden, en hoe die problemen zijn verholpen. Service Bus heeft een aantal oplossingen die u moet kiezen voor geïntroduceerd en in dit onderwerp worden ook de regels voor het gebruik van deze oplossingen aanmelden.

## <a name="reliability-in-service-bus"></a>Betrouwbaarheid in Servicebus
Er zijn verschillende manieren voor het afhandelen van problemen met het bericht en de entiteit en er zijn richtlijnen voor het juiste gebruik van deze oplossingen. Voor meer informatie over de richtlijnen, moet u eerst te begrijpen wat in Service Bus kan mislukken. Vanwege het ontwerp van de Azure-systemen vaak al deze problemen worden tijdelijke. Op hoog niveau uitzien de andere oorzaken van niet beschikbaar zijn als volgt:

* Beperking van een extern systeem waarop Service Bus hangt ervan af. Beperking optreedt van de interacties met de opslag en rekenresources.
* Probleem voor een systeem waarop Service Bus hangt ervan af. Een onderdeel van de opslag kan bijvoorbeeld problemen optreden.
* Fout van Service Bus op één subsysteem. In dit geval een rekenknooppunt kunt krijgen in een inconsistente toestand en moet opnieuw worden opgestart, waardoor alle entiteiten die het resultaat wordt gebruikt om taken te verdelen naar andere knooppunten. Dit kan een korte periode van trage berichtverwerking op zijn beurt veroorzaken.
* Fout van Service Bus in een Azure-datacenter. Dit is een 'Onherstelbare fout"gedurende welke het systeem niet bereikbaar voor het aantal minuten of een paar uur is.

> [!NOTE]
> De term **opslag** kan betekenen zowel Azure Storage en SQL Azure.
> 
> 

Service Bus bevat een aantal oplossingen voor deze problemen. De volgende secties worden besproken elk probleem en hun respectieve oplossingen.

### <a name="throttling"></a>Beperking
Met Service Bus kunt beperking gezamenlijke bericht tarief management. Elke afzonderlijke Service Bus-knooppunt ook veel entiteiten nieuwste. Elk van deze entiteiten kunt eisen op het systeem in termen van CPU, geheugen, opslag en andere facetten. Wanneer een van deze facetten gebruik detecteert die groter is dan gedefinieerde drempelwaarden, Service Bus een bepaalde aanvraag kunt weigeren. De oproepende functie ontvangt een [ServerBusyException] [ ServerBusyException] en nieuwe pogingen na tien seconden.

Als een beperking, moet de code de fout bij het lezen en stoppen van een nieuwe pogingen van het bericht ten minste 10 seconden. Omdat de fout op verschillende onderdelen van de Klanttoepassing plaatsvinden kan, is het waarschijnlijk dat elk onafhankelijk wordt uitgevoerd de logica voor opnieuw proberen. De code kunt verminderen de kans wordt beperkt door in te schakelen op een wachtrij of onderwerp partitioneren.

### <a name="issue-for-an-azure-dependency"></a>Probleem met een Azure-afhankelijkheid
Andere onderdelen in Azure kunnen van tijd tot tijd hebben problemen met de service. Bijvoorbeeld, wanneer een systeem dat gebruikmaakt van Service Bus wordt bijgewerkt, kan dat systeem tijdelijk ervaren beperkte mogelijkheden. Als tijdelijke oplossing voor dit soort problemen, Service Bus regelmatig onderzoekt het probleem en oplossingen implementeert. Neveneffecten van deze oplossingen worden ook weergegeven. Voor het afhandelen van tijdelijke problemen met opslag, bijvoorbeeld: implementeert Service Bus een systeem dat Hiermee kunt u bewerkingen van bericht verzenden naar het consistent worden gewerkt. Vanwege de aard van de oplossing, kan een verzonden bericht tot 15 minuten in de betreffende wachtrij of abonnement wordt weergegeven en gereed voor een ontvangstbewerking duren. De meeste entiteiten wordt dit probleem in het algemeen niet optreden. Echter, gezien het aantal entiteiten in Service Bus in Azure, deze beperking is soms nodig voor een kleine subset van Service Bus-klanten.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Service Bus-fout op een enkele subsysteem
Met elke toepassing omstandigheden kunnen leiden tot een interne onderdeel van Service Bus inconsistent worden. Wanneer Service Bus gedetecteerd, worden gegevens verzameld van de toepassing om te helpen bij het vaststellen van wat is er gebeurd. Zodra de gegevens zijn verzameld, kan de toepassing wordt opnieuw gestart in een poging om een consistente status weer. Dit proces vrij snel wordt uitgevoerd, en resultaten in een entiteit zijn niet beschikbaar voor maximaal een paar minuten, maar typische uitvaltijden lijkt veel korter.

In dergelijke gevallen de clienttoepassing genereert een [System.TimeoutException] [ System.TimeoutException] of [MessagingException] [ MessagingException] uitzondering. Service Bus bevat een beperking voor dit probleem in de vorm van logica voor opnieuw proberen van geautomatiseerde client. Zodra de tijd tussen elke poging is verbruikt en het bericht is niet afgeleverd, u kunt verkennen met behulp van andere functies zoals [gekoppelde naamruimten][paired namespaces]. Gekoppelde naamruimten hebben andere beperkingen die in dit artikel worden besproken.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Fout van Service Bus in een Azure-datacenter
De meest waarschijnlijke oorzaak van een probleem in een Azure-datacenter is een mislukte upgrade-implementatie van Service Bus of een afhankelijke systeem. Als het platform is ontwikkeld, is de kans op dit soort fouten verminderd. Een datacenter-fout kan ook gebeuren om redenen die onder andere het volgende:

* Elektrische onderbreking (voeding en genereren power verdwijnen).
* Connectiviteit (internet pauze tussen uw clients en Azure).

In beide gevallen wordt oorzaak een noodgeval natuurlijke of kunstmatige van het probleem. Dit probleem omzeilen en zorg ervoor dat u kunt nog steeds berichten verzenden, kunt u [gekoppelde naamruimten] [ paired namespaces] waarmee berichten worden verzonden naar een tweede locatie als de primaire locatie weer in orde is gemaakt. Zie voor meer informatie, [aanbevolen procedures voor de isolatie van toepassingen op basis van Service Bus-uitval en noodgevallen][Best practices for insulating applications against Service Bus outages and disasters].

## <a name="paired-namespaces"></a>Gekoppelde naamruimten
De [gekoppelde naamruimten] [ paired namespaces] functie biedt ondersteuning voor scenario's waarin een Service Bus-entiteit of een implementatie binnen een datacenter niet meer beschikbaar is. Terwijl deze gebeurtenis wordt niet vaak worden uitgevoerd, moeten gedistribueerde systemen nog worden voorbereid voor het afhandelen van slechtste use-casescenario's. Deze gebeurtenis gebeurt gewoonlijk, omdat sommige element waarop Service Bus is afhankelijk van een op korte termijn probleem ondervindt. Als u wilt behouden beschikbaarheid van toepassingen tijdens een storing, kunnen Service Bus gebruikers twee afzonderlijke naamruimten, bij voorkeur in verschillende datacenters, gebruiken voor het hosten van hun berichtentiteiten. De rest van deze sectie maakt gebruik van de volgende terminologie:

* Primaire naamruimte: de naamruimte waarmee uw toepassing communiceert voor verzenden en ontvangen van bewerkingen.
* Secundaire naamruimte: de naamruimte die als een back-up naar de primaire naamruimte fungeert. Toepassingslogica communiceert niet met deze naamruimte.
* Failover-interval: de hoeveelheid tijd voor het accepteren van normale fouten voordat de toepassing uit de primaire naamruimte wordt overgeschakeld naar de secundaire naamruimte.

Gekoppelde naamruimten ondersteuning *verzenden beschikbaarheid*. Verzenden beschikbaarheid blijft behouden de mogelijkheid om berichten te verzenden. Voor het gebruik van de beschikbaarheid verzenden, kan uw toepassing moet voldoen aan de volgende vereisten:

1. Berichten worden alleen ontvangen van de primaire naamruimte.
2. Berichten verzonden naar een bepaalde wachtrij of onderwerp mogelijk niet de juiste volgorde binnenkomen.
3. Berichten in een sessie kunnen niet de juiste volgorde binnenkomen. Dit is een onderbreking van de normale functionaliteit van sessies. Dit betekent dat uw toepassing gebruikmaakt van sessies berichten logisch groeperen.
4. Status van de sessie wordt alleen onderhouden op de primaire naamruimte.
5. De primaire wachtrij kunt online is gekomen en start berichten geaccepteerd voordat de secundaire wachtrij alle berichten in de primaire wachtrij wordt geplaatst biedt.

De volgende secties worden de API's, hoe de API's worden geïmplementeerd en ziet u voorbeelden van code die wordt gebruikgemaakt van de functie. Houd er rekening mee dat er met zich meebrengt die zijn gekoppeld aan deze functie zijn.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>De MessagingFactory.PairNamespaceAsync API
De functie gekoppelde naamruimten bevat de [PairNamespaceAsync] [ PairNamespaceAsync] methode voor het [Microsoft.ServiceBus.Messaging.MessagingFactory] [ Microsoft.ServiceBus.Messaging.MessagingFactory] klasse:

```csharp
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Wanneer de taak is voltooid, het koppelen van de naamruimte ook is voltooid en gereed om te reageren op een [MessageReceiver][MessageReceiver], [QueueClient] [ QueueClient] , of [TopicClient] [ TopicClient] die zijn gemaakt met de [MessagingFactory] [ MessagingFactory] exemplaar. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] [ Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] is de basisklasse voor de verschillende typen koppelen die beschikbaar met zijn een [MessagingFactory] [ MessagingFactory] object. Op dit moment de enige afgeleide klasse is een met de naam [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions], die de vereisten voor verzenden beschikbaarheid implementeert. [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] bevat een set constructors die boven op elkaar gebouwd. De constructor met de meeste parameters bekijkt, kunt u het gedrag van de andere constructors begrijpen.

```csharp
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Deze parameters hebben de volgende betekenis:

* *secondaryNamespaceManager*: een geïnitialiseerd [NamespaceManager] [ NamespaceManager] -exemplaar voor de secundaire naamruimte die de [PairNamespaceAsync] [ PairNamespaceAsync] methode kunt gebruiken voor het instellen van de secundaire naamruimte. De manager van de naamruimte wordt gebruikt om op te halen van de lijst met wachtrijen in de naamruimte en om ervoor te zorgen dat de vereiste achterstand wachtrijen bestaan. Als de wachtrijen niet bestaan, worden gemaakt. [NamespaceManager] [ NamespaceManager] vereist de mogelijkheid om te maken van een token met de **beheren** claim.
* *messagingFactory*: de [MessagingFactory] [ MessagingFactory] -exemplaar voor de secundaire naamruimte. De [MessagingFactory] [ MessagingFactory] object wordt gebruikt om te verzenden en, indien de [EnableSyphon] [ EnableSyphon] eigenschap is ingesteld op **waar**, berichten ontvangen van de achterstand wachtrijen.
* *backlogQueueCount*: het aantal achterstand wachtrijen te maken. Deze waarde moet ten minste 1 zijn. Wanneer u berichten verzendt naar de achterstand, wordt een van deze wachtrijen willekeurig gekozen. Als u de waarde ingesteld op 1, kunt wordt slechts één wachtrij ooit gebruikt. Wanneer dit gebeurt en de wachtrij een achterstand fouten genereert, wordt de client is niet mogelijk om te proberen een wachtrij verschillende achterstand en mislukken om uw bericht te verzenden. Het is raadzaam om de waarde instelt op een grotere waarde en standaard de waarde 10. U kunt dit wijzigen naar een hogere of lagere waarde, afhankelijk van hoeveel gegevens uw toepassing per dag verzendt. Elke wachtrij achterstand kan tot 5 GB aan berichten bevatten.
* *failoverInterval*: de hoeveelheid tijd gedurende welke u fouten op de primaire naamruimte accepteren wilt voordat u een enkele entiteit via overschakelt naar de secundaire naamruimte. Failovers optreden op basis van de entiteit met entiteit. Entiteiten in één enkele naamruimte bevinden zich vaak in verschillende knooppunten in Service Bus. Een fout in de ene entiteit betekent niet dat een fout in een andere. U kunt deze waarde instellen op [System.TimeSpan.Zero] [ System.TimeSpan.Zero] voor failover naar de secundaire onmiddellijk na de eerste, niet-tijdelijke fout. Fouten die de failover-timer trigger zijn [MessagingException] [ MessagingException] waarin de [IsTransient] [ IsTransient] eigenschap is false, of een [ System.TimeoutException][System.TimeoutException]. Andere uitzonderingen, zoals [UnauthorizedAccessException] [ UnauthorizedAccessException] veroorzaken geen failover, omdat ze geven aan dat de client onjuist is geconfigureerd. Een [ServerBusyException] [ ServerBusyException] niet oorzaak failover omdat de juiste patroon is 10 seconden wachten vervolgens verzendt het bericht opnieuw.
* *enableSyphon*: geeft aan dat deze bepaalde koppelen moet ook syphon berichten van de secundaire naamruimte terug naar de primaire naamruimte. Toepassingen die berichten verzenden moeten in het algemeen is deze waarde ingesteld op **false**; toepassingen die berichten ontvangen, moeten deze waarde instelt op **waar**. De reden hiervoor is dat vaak het geval is, er minder bericht ontvangers dan afzenders van berichten. Afhankelijk van het aantal ontvangers, kunt u beschikt over een enkele toepassing-exemplaar de rechten syphon verwerken. Met behulp van de vele ontvangers heeft voor elke wachtrij achterstand met zich meebrengt.

Voor het gebruik van de code, maakt u een primaire [MessagingFactory] [ MessagingFactory] exemplaar, een secundaire [MessagingFactory] [ MessagingFactory] exemplaar, een secundaire [ NamespaceManager] [ NamespaceManager] -exemplaar en een [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] exemplaar. De aanroep is net zo eenvoudig als het volgende:

```csharp
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Wanneer de taak die wordt geretourneerd door de [PairNamespaceAsync] [ PairNamespaceAsync] methode is voltooid, alles is ingesteld en klaar voor gebruik. Voordat de taak wordt geretourneerd, u mogelijk niet voltooid al het achtergrondwerk die nodig zijn voor de koppeling naar rechts werken. Als gevolg hiervan moet u niet starten verzenden van berichten, totdat de taak retourneert. Als er fouten opgetreden, zoals onjuiste referenties, of dat de wachtrijen achterstand maken wordt deze uitzonderingen worden gegenereerd nadat de taak is voltooid. Zodra de taak wordt geretourneerd, Controleer of de wachtrijen zijn gevonden of is gemaakt door te controleren de [BacklogQueueCount] [ BacklogQueueCount] eigenschap op uw [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] exemplaar. Voor de bovenstaande code weergegeven die voor deze bewerking als volgt:

```csharp
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt de basisbeginselen van asynchrone berichten in Service Bus, Raadpleeg voor meer informatie over [gekoppelde naamruimten][paired namespaces].

[ServerBusyException]: /dotnet/api/microsoft.servicebus.messaging.serverbusyexception
[System.TimeoutException]: https://msdn.microsoft.com/library/system.timeoutexception.aspx
[MessagingException]: /dotnet/api/microsoft.servicebus.messaging.messagingexception
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Microsoft.ServiceBus.Messaging.MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[MessageReceiver]: /dotnet/api/microsoft.servicebus.messaging.messagereceiver
[QueueClient]: /dotnet/api/microsoft.servicebus.messaging.queueclient
[TopicClient]: /dotnet/api/microsoft.servicebus.messaging.topicclient
[Microsoft.ServiceBus.Messaging.PairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.pairednamespaceoptions
[MessagingFactory]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory
[SendAvailabilityPairedNamespaceOptions]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions
[NamespaceManager]: /dotnet/api/microsoft.servicebus.namespacemanager
[PairNamespaceAsync]: /dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PairNamespaceAsync_Microsoft_ServiceBus_Messaging_PairedNamespaceOptions_
[EnableSyphon]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_EnableSyphon
[System.TimeSpan.Zero]: https://msdn.microsoft.com/library/system.timespan.zero.aspx
[IsTransient]: /dotnet/api/microsoft.servicebus.messaging.messagingexception#Microsoft_ServiceBus_Messaging_MessagingException_IsTransient
[UnauthorizedAccessException]: https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx
[BacklogQueueCount]: /dotnet/api/microsoft.servicebus.messaging.sendavailabilitypairednamespaceoptions?redirectedfrom=MSDN#Microsoft_ServiceBus_Messaging_SendAvailabilityPairedNamespaceOptions_BacklogQueueCount
[paired namespaces]: service-bus-paired-namespaces.md
