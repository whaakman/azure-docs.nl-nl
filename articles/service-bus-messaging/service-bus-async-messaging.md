---
title: Asynchrone Service Bus-berichtenservice | Microsoft Docs
description: Beschrijving van de asynchrone Azure Service Bus-berichtenservice.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f1435549-e1f2-40cb-a280-64ea07b39fc7
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/06/2017
ms.author: sethm
ms.openlocfilehash: d36360f3fb46adf96f53976584987590791b07d0
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="asynchronous-messaging-patterns-and-high-availability"></a>Asynchrone berichtpatronen en hoge beschikbaarheid

Asynchrone berichten kan worden geïmplementeerd in een aantal verschillende manieren. Met wachtrijen, onderwerpen en abonnementen ondersteunt Azure Service Bus asynchronism via een winkel en voorwaarts mechanisme. In de normale werking (synchrone), kunt u berichten verzenden met wachtrijen en onderwerpen en berichten ontvangen van wachtrijen en abonnementen. Toepassingen die u schrijft, is afhankelijk van deze entiteiten worden altijd beschikbaar. Wanneer de entiteitsstatus wordt gewijzigd, optreden wegens diverse omstandigheden, moet u een oplossing voor een entiteit met verminderde functionaliteit waarmee de meeste behoeften aan.

Asynchrone berichtpatronen toepassingen meestal gebruiken om in te schakelen van een aantal scenario's voor communicatie. U kunt bouwen toepassingen waarin clients berichten naar services, verzenden kunnen zelfs wanneer de service niet wordt uitgevoerd. Voor toepassingen niveau ervaring bursts van communicatie, een wachtrij kunnen helpen de belasting doordat een plaats buffer communicatie. Ten slotte kunt u een eenvoudige maar effectieve load balancer voor de distributie van berichten over meerdere machines ophalen.

Overweeg om de beschikbaarheid van een van deze entiteiten blijven, een aantal verschillende manieren waarop deze entiteiten niet beschikbaar voor een duurzame berichtensysteem kunnen voorkomen. In het algemeen, ziet u de entiteit die niet meer beschikbaar voor toepassingen die we in de volgende verschillende manieren schrijven:

* Kan geen berichten verzenden.
* Kan geen berichten ontvangen.
* Kan niet voor het beheren van entiteiten (maken, ophalen, bijwerken of verwijderen van entiteiten).
* Er kan geen verbinding maken met de service.

Voor elk van deze oorzaken bestaan fout verschillende modi waarmee een toepassing om te blijven werken op een bepaald niveau verminderde functionaliteit. Bijvoorbeeld, een systeem waarmee u kunt berichten verzenden, maar deze niet ontvangen kunt nog steeds orders van klanten ontvangen maar die orders kan niet worden verwerkt. In dit onderwerp worden de mogelijke problemen die kunnen optreden en hoe deze problemen worden verholpen. Service Bus is een aantal oplossingen die u moet kiezen voor geïntroduceerd en in dit onderwerp worden ook de regels voor het gebruik van deze oplossingen aanmelden.

## <a name="reliability-in-service-bus"></a>Betrouwbaarheid in Servicebus
Er zijn verschillende manieren voor het afhandelen van problemen met het bericht en de entiteit en er zijn richtlijnen voor het juiste gebruik van deze oplossingen. Om de richtlijnen te begrijpen, moet u eerst begrijpen wat in de Service Bus kan mislukken. Als gevolg van het ontwerp van de Azure-systemen, al deze problemen zijn meestal tijdelijke. Op een hoog niveau uitzien de andere oorzaken van niet beschikbaar zijn als volgt:

* Beperking van een extern systeem waarvan Service Bus afhankelijk is. Beperking optreedt van de interacties met opslagruimte en rekencapaciteit resources.
* Probleem voor een systeem waarop de Service Bus afhankelijk is. Een bepaald gedeelte van de opslag kan bijvoorbeeld problemen optreden.
* Fout van Service Bus op één subsysteem. In dit geval een rekenknooppunt kunt krijgen tot een inconsistente status heeft en moet opnieuw worden opgestart, waardoor alle entiteiten die het fungeert om taken te verdelen naar andere knooppunten. Dit kan een korte periode van trage berichtverwerking op zijn beurt veroorzaken.
* Fout van Service Bus in een Azure-datacenter. Dit is een 'Onherstelbare fout' dat het systeem niet bereikbaar aan veel minuten of enkele uren is.

> [!NOTE]
> De term **opslag** kan betekenen zowel Azure Storage en Azure SQL.
> 
> 

Service Bus bevat een aantal oplossingen voor deze problemen. De volgende secties worden besproken elk probleem en hun respectieve oplossingen beschreven.

### <a name="throttling"></a>Beperking
Met Service Bus kunt beperking gezamenlijke bericht snelheid management. Elke afzonderlijke Service Bus-knooppunten ook nieuwste veel entiteiten. Elk van deze entiteiten maakt eisen op het systeem in termen van CPU, geheugen, opslag en andere facetten. Wanneer een van deze facetten gebruik detecteert die gedefinieerde drempelwaarden overschrijdt, Service Bus een bepaalde aanvraag kunt weigeren. De aanroeper ontvangt een [ServerBusyException] [ ServerBusyException] en pogingen na 10 seconden.

Als een risicobeperking moet de code de leesfout en alle nieuwe pogingen van het bericht is gestopt voor ten minste 10 seconden. Omdat de fout tussen onderdelen van de Klanttoepassing optreden kan, verwacht wordt dat elk onafhankelijk wordt uitgevoerd de Pogingslogica. De code kan verminderen de kans dat wordt beperkt door in te schakelen op een wachtrij of onderwerp partitioneren.

### <a name="issue-for-an-azure-dependency"></a>Probleem met een Azure-afhankelijkheid
Andere onderdelen in Azure kunnen van tijd tot tijd hebben problemen met de service. Wanneer een besturingssysteem dat gebruikmaakt van Service Bus wordt bijgewerkt, kan dat systeem tijdelijk zich bijvoorbeeld minder mogelijkheden. U kunt dit soort problemen omzeilen, Service Bus regelmatig onderzoekt en oplossingen implementeert. Neveneffecten van deze oplossingen worden weergegeven. Voor het afhandelen van tijdelijke problemen met opslag, bijvoorbeeld: implementeert Service Bus een systeem waarmee bericht verzenden bewerkingen zijn consistent worden gewerkt. Een verzonden bericht kan worden weergegeven in de betreffende wachtrij of een abonnement en gereed voor een ontvangstbewerking 15 minuten duren voordat vanwege de aard van de oplossing. Normaal gesproken de meeste entiteiten wordt dit probleem zich niet. Echter, gezien het aantal entiteiten in de Service Bus in Azure, deze beperking is soms nodig voor een kleine subset van Service Bus-klanten.

### <a name="service-bus-failure-on-a-single-subsystem"></a>Service Bus-fout op een enkele subsysteem
Met elke toepassing, omstandigheden kunnen leiden tot een interne onderdeel van Service Bus inconsistent worden. Wanneer Service Bus gedetecteerd, worden gegevens verzameld van de toepassing om te helpen bij het oplossen van wat is er gebeurd. Nadat de gegevens worden verzameld, wordt de toepassing opnieuw gestart in een poging tot terugzetten op een consistente status. Dit proces wordt uitgevoerd vrij snel en resulteert in een entiteit lijkt te zijn niet beschikbaar voor maximaal een paar minuten, hoewel typische uitvaltijden veel korter zijn.

In dergelijke gevallen de clienttoepassing genereert een [System.TimeoutException] [ System.TimeoutException] of [MessagingException] [ MessagingException] uitzondering. Service Bus bevat een beperking voor dit probleem in de vorm van geautomatiseerde client Pogingslogica. Zodra de tijd tussen elke poging is verbruikt en het bericht is niet bezorgd, u kunt verkennen met andere functies, zoals [naamruimten gekoppeld][paired namespaces]. Gekoppelde naamruimten hebben andere waarschuwingen die worden beschreven in dit artikel.

### <a name="failure-of-service-bus-within-an-azure-datacenter"></a>Fout van Service Bus in een Azure-datacenter
De meest waarschijnlijke reden voor een storing in een Azure-datacenter is een mislukte upgrade-implementatie van Service Bus- of een afhankelijke systeem. Als het platform is gekomen, is de kans op dit type fout verminderd. Een datacenter-fout kan ook gebeuren vanwege het volgende:

* Elektrische storing (voeding en genereren power verdwijnen).
* De verbinding (internet pauze tussen uw clients en Azure).

In beide gevallen veroorzaakt een noodgeval natuurlijke of kunstmatige het probleem. Dit probleem omzeilen en zorg ervoor dat u kunt nog steeds berichten verzenden, kunt u [naamruimten gekoppeld] [ paired namespaces] zodat berichten worden verzonden naar een tweede locatie terwijl de primaire locatie opnieuw in orde wordt gemaakt. Zie voor meer informatie [aanbevolen procedures voor de isolatie van toepassingen tegen storingen van de Service Bus en noodsituaties][Best practices for insulating applications against Service Bus outages and disasters].

## <a name="paired-namespaces"></a>Gekoppelde naamruimten
De [naamruimten gekoppeld] [ paired namespaces] functie ondersteunt scenario's waarin een Service Bus-entiteit of implementatie binnen een datacenter niet meer beschikbaar is. Terwijl deze gebeurtenis treedt op zelden, moeten gedistribueerde systemen nog worden voorbereid voor het afhandelen van slechtste gebruiksscenario's. Normaal gesproken treedt deze gebeurtenis op omdat sommige element waarvan Service Bus afhankelijk is, is een korte termijn probleem opgetreden. Om te blijven beschikbaarheid van toepassingen tijdens een storing, kunnen gebruikers van de Service Bus twee afzonderlijke naamruimten, bij voorkeur in afzonderlijke datacenters, gebruiken voor het hosten van hun berichtentiteiten. De rest van deze sectie maakt gebruik van de volgende terminologie:

* Primaire naamruimte: de naamruimte waarmee uw toepassing communiceert voor verzend- en ontvangstbewerkingen.
* Secundaire naamruimte: de naamruimte die als een back-up naar de primaire naamruimte fungeert. Toepassingslogica communiceert niet met deze naamruimte.
* Failover-interval: de hoeveelheid tijd normale storingen accepteren voordat de toepassing van de primaire naamruimte in de secundaire naamruimte verandert.

Ondersteuning voor naamruimten gekoppeld *verzenden beschikbaarheid*. Verzenden van beschikbaarheid van bewaart de mogelijkheid om berichten te verzenden. Als u wilt verzenden beschikbaarheid gebruiken, moet uw toepassing voldoen aan de volgende vereisten:

1. Berichten worden alleen ontvangen van de primaire naamruimte.
2. Berichten naar een bepaalde wachtrij of onderwerp mogelijk een verkeerde volgorde binnenkomen.
3. Berichten in een sessie mogelijk een verkeerde volgorde binnenkomen. Dit is een onderbreking van de normale functionaliteit van sessies. Dit betekent dat uw toepassing gebruik maakt van sessies berichten logisch groeperen.
4. Sessiestatus alleen behouden blijven op de primaire naamruimte.
5. De primaire wachtrij kunt online komen en start berichten worden geaccepteerd voordat de secundaire wachtrij alle berichten in de primaire wachtrij biedt.

De volgende secties worden de API's, hoe de API's worden geïmplementeerd en voorbeeldcode ziet u dat gebruikmaakt van de functie. Houd er rekening mee dat er facturering gevolgen die zijn gekoppeld aan deze functie zijn.

### <a name="the-messagingfactorypairnamespaceasync-api"></a>De MessagingFactory.PairNamespaceAsync API
De functie gekoppelde naamruimten bevat de [PairNamespaceAsync] [ PairNamespaceAsync] methode op de [Microsoft.ServiceBus.Messaging.MessagingFactory] [ Microsoft.ServiceBus.Messaging.MessagingFactory] klasse:

```csharp
public Task PairNamespaceAsync(PairedNamespaceOptions options);
```

Wanneer de taak is voltooid, de naamruimte koppeling ook is voltooid en klaar om te reageren op een [MessageReceiver][MessageReceiver], [QueueClient] [ QueueClient] , of [TopicClient] [ TopicClient] gemaakt met de [MessagingFactory] [ MessagingFactory] exemplaar. [Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] [ Microsoft.ServiceBus.Messaging.PairedNamespaceOptions] is de basisklasse voor de verschillende soorten koppelen die beschikbaar met zijn een [MessagingFactory] [ MessagingFactory] object. De enige afgeleide klasse is een met de naam momenteel [SendAvailabilityPairedNamespaceOptions][SendAvailabilityPairedNamespaceOptions], die de vereisten van de beschikbaarheid verzenden implementeert. [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] een constructors die zijn gebaseerd op elkaar heeft. De constructor met de meeste parameters bekijkt, kunt u het gedrag van de andere constructors begrijpen.

```csharp
public SendAvailabilityPairedNamespaceOptions(
    NamespaceManager secondaryNamespaceManager,
    MessagingFactory messagingFactory,
    int backlogQueueCount,
    TimeSpan failoverInterval,
    bool enableSyphon)
```

Deze parameters hebben de volgende betekenis:

* *secondaryNamespaceManager*: een geïnitialiseerd [NamespaceManager] [ NamespaceManager] -exemplaar voor de secundaire naamruimte die de [PairNamespaceAsync] [ PairNamespaceAsync] methode kunt gebruiken voor het instellen van de secundaire naamruimte. De naamruimte manager wordt gebruikt om de lijst van wachtrijen in de naamruimte te verkrijgen en om ervoor te zorgen dat de vereiste achterstand wachtrijen bestaan. Als deze wachtrijen niet bestaan, worden ze gemaakt. [NamespaceManager] [ NamespaceManager] vereist dat de mogelijkheid te maken van een token met de **beheren** claim.
* *messagingFactory*: de [MessagingFactory] [ MessagingFactory] -exemplaar voor de secundaire naamruimte. De [MessagingFactory] [ MessagingFactory] object wordt gebruikt om te verzenden en, indien de [EnableSyphon] [ EnableSyphon] eigenschap is ingesteld op **waar**, berichten ontvangen van de achterstand wachtrijen.
* *backlogQueueCount*: het aantal wachtrijen achterstand maken. Deze waarde moet ten minste 1 zijn. Tijdens het verzenden van berichten naar de achterstand, wordt een van deze wachtrijen willekeurig gekozen. Als u de waarde op 1 instelt, kunt wordt slechts één wachtrij ooit gebruikt. Wanneer dit gebeurt en de wachtrij een achterstand fouten genereert, wordt de client kan geen probeer een andere achterstand wachtrij en mogelijk uw bericht te verzenden. Het is raadzaam om de waarde instelt op een grotere waarde en standaard de waarde 10. U kunt dit wijzigen in een hogere of lagere waarde afhankelijk van hoeveel gegevens door uw toepassing per dag verzendt. Elke wachtrij achterstand kan maximaal 5 GB aan berichten bevatten.
* *failoverInterval*: de hoeveelheid tijd waarin u fouten op de primaire naamruimte accepteren wilt voordat u een enkele entiteit via overschakelt naar de secundaire naamruimte. Failovers optreden op basis van de entiteit met entiteit. Entiteiten in een enkele naamruimte bevinden vaak zich in verschillende knooppunten binnen een Service Bus. Een fout in één entiteit betekent niet dat een storing in een andere. U kunt deze waarde instellen op [System.TimeSpan.Zero] [ System.TimeSpan.Zero] failover van de secundaire onmiddellijk na de eerste, tijdelijke fout. Fouten die de timer van de failover activeren zijn [MessagingException] [ MessagingException] waarin de [IsTransient] [ IsTransient] eigenschap is false of een [ System.TimeoutException][System.TimeoutException]. Andere uitzonderingen, zoals [UnauthorizedAccessException] [ UnauthorizedAccessException] zorgen niet voor failover, omdat ze aangeeft dat de client niet correct is geconfigureerd. Een [ServerBusyException] [ ServerBusyException] komt niet oorzaak failover omdat de juiste patroon is 10 seconden wachten verzend het bericht opnieuw.
* *enableSyphon*: geeft aan dat dit bepaalde combinatie moet ook syphon berichten van de secundaire naamruimte terug naar de primaire naamruimte. Toepassingen die berichten verzenden moeten in het algemeen zijn deze waarde ingesteld op **false**; toepassingen die berichten te ontvangen, moeten deze waarde instelt op **true**. De reden hiervoor is dat vaak het geval is, er minder bericht ontvangers dan afzenders van berichten zijn. Afhankelijk van het aantal ontvangers, kunt u een exemplaar zijn één toepassing de rechten syphon verwerken. Met behulp van veel ontvangers heeft facturering gevolgen voor elke wachtrij achterstand.

Voor het gebruik van de code, maakt u een primaire [MessagingFactory] [ MessagingFactory] exemplaar, een secundaire [MessagingFactory] [ MessagingFactory] exemplaar, een secundaire [ NamespaceManager] [ NamespaceManager] exemplaar, en een [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] exemplaar. De aanroep kan net zo eenvoudig als het volgende zijn:

```csharp
SendAvailabilityPairedNamespaceOptions sendAvailabilityOptions = new SendAvailabilityPairedNamespaceOptions(secondaryNamespaceManager, secondary);
primary.PairNamespaceAsync(sendAvailabilityOptions).Wait();
```

Wanneer de taak die is geretourneerd door de [PairNamespaceAsync] [ PairNamespaceAsync] methode is voltooid, alles is ingesteld en klaar voor gebruik. Voordat de taak wordt geretourneerd, mogelijk niet voltooid al het achtergrondwerk nodig zijn voor de koppeling rechts werken. Als gevolg hiervan moet u niet eerst verzenden van berichten, totdat de taak retourneert. Als er fouten opgetreden, zoals ongeldige referenties of mislukken de achterstand-wachtrijen maken wordt deze uitzonderingen worden gegenereerd zodra de taak is voltooid. Nadat de taak retourneert, controleren of de wachtrijen zijn gevonden of is gemaakt door in de [BacklogQueueCount] [ BacklogQueueCount] -eigenschap op uw [SendAvailabilityPairedNamespaceOptions] [ SendAvailabilityPairedNamespaceOptions] exemplaar. Voor de bovenstaande code weergegeven die voor deze bewerking als volgt:

```csharp
if (sendAvailabilityOptions.BacklogQueueCount < 1)
{
    // Handle case where no queues were created.
}
```

## <a name="next-steps"></a>Volgende stappen
Nu dat u de basisbeginselen van asynchrone in Service Bus-berichtenservice hebt geleerd, leest u meer informatie over [naamruimten gekoppeld][paired namespaces].

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
