---
title: Service Fabric Reliable Actors overzicht | Microsoft Docs
description: Inleiding tot het model voor Reliable Actors van Service Fabric.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 7fdad07f-f2d6-4c74-804d-e0d56131f060
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: 640e051a909b1b9457b20cbd507b418342297c6e
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="introduction-to-service-fabric-reliable-actors"></a>Inleiding tot Service Fabric Reliable Actors
Reliable Actors is een Service Fabric-toepassingsframework op basis van de [virtuele Actor](http://research.microsoft.com/en-us/projects/orleans/) patroon. De betrouwbare actoren API biedt een single thread-programmeermodel die gebaseerd op de schaalbaarheid en betrouwbaarheid garanties geleverd door de Service Fabric.

## <a name="what-are-actors"></a>Wat zijn Actors?
Een actor is een eenheid geïsoleerd, onafhankelijk van de berekenings- en staat met één thread worden uitgevoerd. De [actor patroon](https://en.wikipedia.org/wiki/Actor_model) is een rekenkundige model gelijktijdige of gedistribueerde systemen in die een groot aantal deze actoren kunt tegelijkertijd uitvoeren en onafhankelijk van elkaar. Actoren kunnen communiceren met elkaar en meer actoren te maken.

### <a name="when-to-use-reliable-actors"></a>Wanneer gebruikt u Reliable Actors
Service Fabric Reliable Actors is een implementatie van het ontwerppatroon actor. Net als bij een ontwerppatroon software overeenkomt de beslissing of u wilt gebruiken van een specifiek patroon is gemaakt op basis van het al dan niet de probleem voor het ontwerpen van een software met het patroon.

Hoewel de actor ontwerpen patroon een goede passen op een aantal problemen gedistribueerde systemen en scenario's zorgvuldig rekening wordt gehouden met de beperkingen van het patroon en de implementatie van het framework moet worden gemaakt. Houd rekening met het patroon actor als model voor uw probleem of scenario als algemene richtlijnen:

* De ruimte van uw probleem betrekking heeft op een groot aantal (duizendtallen of meer) van kleine, onafhankelijke en geïsoleerde eenheden van de status en logica.
* U wilt werken met één thread-objecten die geen aanzienlijke interactie met externe onderdelen, inclusief de status opvragen van een set van actoren vereisen.
* Uw actor-exemplaren niet aanroepfuncties met onvoorspelbare vertragingen worden geblokkeerd door uitgifte van i/o-bewerkingen.

## <a name="actors-in-service-fabric"></a>Actoren in Service Fabric
In Service Fabric actoren zijn geïmplementeerd in het kader Reliable Actors: een toepassing op basis van actor-patroon framework gebouwd boven [Service Fabric Reliable Services](service-fabric-reliable-services-introduction.md). Elke betrouwbare Actor-service die u schrijft is daadwerkelijk een gepartitioneerde, stateful betrouwbare Service.

Elke actor wordt gedefinieerd als een exemplaar van een actortype identiek is aan de manier waarop die.NET-object een exemplaar van een .NET-type is. Bijvoorbeeld, kan er een actortype dat de functionaliteit van een rekenmachine implementeert en er zijn veel actoren van dat type die worden gedistribueerd op verschillende knooppunten in een cluster. Elke dergelijke actor wordt uniek geïdentificeerd door een actor-ID.

### <a name="actor-lifetime"></a>Actor-levensduur
Service Fabric actoren zijn virtueel, wat betekent dat hun levensduur niet is gekoppeld aan hun representatie in het geheugen. Hierdoor hoeft ze niet expliciet worden gemaakt of vernietigd. De runtime Reliable Actors wordt automatisch geactiveerd voor een actor de eerste keer die een aanvraag voor die actor-ID ontvangt. Als een actor niet voor een bepaalde periode gebruikt wordt, de runtime Reliable Actors garbagecollection-verzamelt het object in het geheugen. Ook blijven kennis van de actor bestaan hoeft het later opnieuw worden geactiveerd. Zie voor meer informatie [Actor lifecycle en garbage collection](service-fabric-reliable-actors-lifecycle.md).

Deze virtuele actor levensduur abstractie voert enkele waarschuwingen als gevolg van het virtuele actor-model en in feite de Reliable Actors uitvoering op tijdstippen afwijkt van dit model.

* Een actor wordt automatisch geactiveerd (waardoor een actor-object om te worden samengesteld) de eerste keer dat een bericht verzonden naar de actor-ID. Na een bepaalde tijd is het object actor garbage collector zijn verzameld. In de toekomst actor-ID opnieuw gebruikt, zorgt ervoor dat een nieuwe actor-object om te worden samengesteld. Status van een actor outlives levensduur van het object als opgeslagen in de status manager.
* Een actormethode aanroepen voor een actor-ID activeert die actor. Om deze reden hebben acteur types hun constructor die impliciet wordt aangeroepen door de runtime. Daarom kan niet clientcode parameters doorgeven aan de actor Typeconstructor, hoewel parameters kunnen worden doorgegeven aan de actor-constructor door de service zelf. Het resultaat is dat actoren kunnen worden geconstrueerd in een status gedeeltelijk geïnitialiseerd door de andere methoden worden genoemd, tijd als de actor van de initialisatieparameters van de client vereist. Er is geen één toegangspunt voor de activering van een acteur van de client.
* Hoewel Reliable Actors impliciet actor-objecten maken. u hebt de mogelijkheid een actor- en de status expliciet verwijderen.

### <a name="distribution-and-failover"></a>Distributie en failover
Voor schaalbaarheid en betrouwbaarheid, Service Fabric distribueert actoren in het cluster en automatisch van knooppunten overzet naar orde protocollen zoals vereist. Dit is een abstractie via een [betrouwbare gepartitioneerde stateful Service](service-fabric-concepts-partitioning.md). Distributie, schaalbaarheid, betrouwbaarheid en automatische failover zijn alle opgegeven grond van het feit dat actoren worden uitgevoerd binnen een stateful betrouwbare Service aangeroepen de *Actor Service*.

Actors zijn verdeeld over de partities van de Actor-Service en deze partities zijn verdeeld over de knooppunten in een Service Fabric-cluster. Elke partitie service bevat een set van actoren. Service Fabric beheert distributie en failover van de servicepartities.

Bijvoorbeeld, zou een actor-service met negen partities die worden geïmplementeerd op drie knooppunten met behulp van de plaatsing standaard actor partitie thusly worden gedistribueerd:

![Betrouwbare actoren distributie][2]

Het Framework Actor beheert partitie schema en de sleutel bereik-instellingen voor u. Dit vereenvoudigt enkele mogelijkheden, maar ook een nadelige invloed sommige overweging:

* Reliable Services kunt u kiest een partitieschema, de belangrijkste bereik (als u een bereik partitieschema) en aantal partitie. Reliable Actors is beperkt tot het bereik partitieschema (uniform Int64 schema) en moet u het volledige bereik van de Int64-sleutel gebruiken.
* Standaard worden actoren willekeurig in partities, wat resulteert in een gelijkmatige verdeling geplaatst.
* Omdat actoren willekeurig worden geplaatst, moet worden verwacht dat actor-bewerkingen altijd netwerkcommunicatie moet, met inbegrip van serialisatie en deserialisatie van methode-aanroepgegevens, latentie en overhead aangaan.
* In geavanceerde scenario's is het mogelijk op de plaatsing van de partitie actor beheer met behulp van Int64 actor-id's die worden toegewezen aan specifieke partities. Echter, doen dus kan resulteren in een distributie onbalans van actoren meerdere partities.

Raadpleeg voor meer informatie over hoe actorservices worden gepartitioneerd, [partitioneren concepten voor actoren](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

### <a name="actor-communication"></a>Actor-communicatie
Actor-interacties worden gedefinieerd in een interface die wordt gedeeld door de actor die de interface implementeert en de client die een proxy wordt een actor via dezelfde interface. Omdat deze interface aan te roepen actor methoden asynchroon wordt gebruikt, moet u elke methode op de interface moet geretourneerd.

Methode-aanroepen en hun antwoorden uiteindelijk op netwerkaanvragen in het cluster, zodat de argumenten en de resultaattypen van de taken die ze retourneren moeten serialiseerbaar zijn door het platform. In het bijzonder, moeten ze [gegevenscontract serialiseerbaar](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

#### <a name="the-actor-proxy"></a>De proxy actor
De client Reliable Actors API biedt communicatie tussen een actor-exemplaar en een actor-client. Om te communiceren met een actor, maakt een client een actor proxy-object dat de actor-interface implementeert. De client communiceert met de actor door het aanroepen van methoden voor de proxyobject. De proxy acteur kan worden gebruikt voor client-naar-actor en actor-actor-communicatie.

```csharp
// Create a randomly distributed actor ID
ActorId actorId = ActorId.CreateRandom();

// This only creates a proxy object, it does not activate an actor or invoke any methods yet.
IMyActor myActor = ActorProxy.Create<IMyActor>(actorId, new Uri("fabric:/MyApp/MyActorService"));

// This will invoke a method on the actor. If an actor with the given ID does not exist, it will be activated by this method call.
await myActor.DoWorkAsync();
```

```java
// Create actor ID with some name
ActorId actorId = new ActorId("Actor1");

// This only creates a proxy object, it does not activate an actor or invoke any methods yet.
MyActor myActor = ActorProxyBase.create(actorId, new URI("fabric:/MyApp/MyActorService"), MyActor.class);

// This will invoke a method on the actor. If an actor with the given ID does not exist, it will be activated by this method call.
myActor.DoWorkAsync().get();
```


Houd er rekening mee dat de twee stukjes informatie die wordt gebruikt voor het maken van het object van de proxy actor de actor-ID en naam van de toepassing zijn. De actor-ID is uniek voor de actor terwijl de toepassingsnaam identificeert de [Service Fabric-toepassing](service-fabric-reliable-actors-platform.md#application-model) waarop de actor wordt geïmplementeerd.

De `ActorProxy`(C#) / `ActorProxyBase`klasse (Java) op de client voert de benodigde resolutie de actor door-ID vinden en openen van een communicatiekanaal met het. Het is ook opnieuw probeert om de actor niet vinden in het geval van communicatiefouten en failovers. Als gevolg hiervan heeft levering van berichten de volgende kenmerken:

* Levering van berichten is zo goed mogelijke poging.
* Actoren mogelijk dezelfde client dubbele berichten ontvangen.

### <a name="concurrency"></a>Gelijktijdigheid
De runtime Reliable Actors biedt een eenvoudige inschakelen op basis van een toegangsmodel voor toegang tot actor-methoden. Dit betekent dat niet meer dan één thread actief binnen een actor-object-code op elk gewenst moment zijn kan. Schakel gebaseerde toegang vereenvoudigt gelijktijdige systemen als er geen synchronisatiemechanismen voor toegang tot gegevens nodig is. Het betekent ook systemen moeten worden ontworpen met speciale overwegingen voor de toegang tot één thread aard van elk actor-exemplaar.

* Een enkele actor-exemplaar kan niet meer dan één verzoek tegelijkertijd wordt verwerkt. Een actor-exemplaar kan een knelpunt doorvoer veroorzaken als er wordt verwacht om gelijktijdige aanvragen te verwerken.
* Actoren kunnen impasse op elkaar, als er een circulaire verzoek tussen twee actors tijdens een externe aanvraag is verzonden naar een van de actoren tegelijkertijd. De runtime actor wordt automatisch een time-out op actoraanroepen en Veroorzaak een exception naar de aanroeper impasse situaties onderbreken.

![Betrouwbare actoren communicatie][3]

#### <a name="turn-based-access"></a>Schakel gebaseerde toegang
Een Schakel bestaat uit het uitvoeren van een actormethode in reactie op een aanvraag van andere actoren of clients of het uitvoeren van een [timer/herinnering](service-fabric-reliable-actors-timers-reminders.md) retouraanroep. Hoewel deze methoden en retouraanroepen asynchrone, kan de runtime actoren ze komt niet interleave. Een Schakel moet volledig is voltooid voordat een nieuwe Schakel is toegestaan. Met andere woorden, een actor-methode of timer/herinnering callback die momenteel wordt uitgevoerd moet volledig is voltooid voordat u een nieuwe aanroep van een methode of retouraanroep is toegestaan. Een methode of de retouraanroep wordt beschouwd als hebt opgegeven als de uitvoering van de methode heeft geretourneerd of retouraanroep en wordt de taak die is geretourneerd door de methode of een retouraanroep is voltooid. Wordt het bewerken van benadrukken dat die op basis van Schakel gelijktijdigheid zelfs in verschillende methoden en timers retouraanroepen is voldaan.

De runtime actoren gehandhaafd gelijktijdigheid op basis van inschakelen door een afzonderlijke actorvergrendeling die aan het begin van een beurt en aan het einde van de beurt de vergrendeling verkrijgen. Schakel gebaseerde gelijktijdigheid van taken wordt dus afgedwongen op basis van per actor en niet via actoren. Actor-methoden en timer/herinnering retouraanroepen kunnen tegelijkertijd uitvoeren namens actoren.

Het volgende voorbeeld ziet u de bovenstaande concepten. Houd rekening met een actortype waarmee twee asynchrone methoden (bijvoorbeeld *Method1* en *Method2*), een timer en een herinnering. Het volgende diagram toont een voorbeeld van een tijdlijn voor de uitvoering van deze methoden en retouraanroepen namens twee actoren (*ActorId1* en *ActorId2*) die deel uitmaken van dit actortype.

![Betrouwbare actoren runtime op basis van Schakel gelijktijdigheid van taken en -toegang][1]

Dit diagram volgt deze conventies:

* Elke verticale lijn ziet u de logische stroom van de uitvoering van een methode of een retouraanroep namens een bepaalde actor.
* De gebeurtenissen die zijn gemarkeerd op elke verticale lijn worden uitgevoerd met nieuwere gebeurtenissen hieronder oudere in chronologische volgorde.
* Verschillende kleuren worden gebruikt voor tijdlijnen overeenkomt met verschillende actoren.
* Markering wordt om aan te geven van de duur waarvoor per actorvergrendeling wordt vastgehouden namens een methode of de retouraanroep gebruikt.

Aantal belangrijke zaken te overwegen:

* Terwijl *Method1* wordt uitgevoerd namens *ActorId2* in reactie op aanvraag van de client *xyz789*, een andere clientaanvraag (*abc123*) binnenkomt die vereist ook *Method1* moet worden uitgevoerd door *ActorId2*. Echter, de tweede uitvoering van *Method1* begint niet totdat de vorige uitvoering is voltooid. Op deze manier is geregistreerd door een herinnering *ActorId2* wordt geactiveerd tijdens *Method1* wordt uitgevoerd in reactie op aanvraag van de client *xyz789*. De callback herinnering wordt uitgevoerd nadat beide uitvoeringen van *Method1* zijn voltooid. Dit alles is vanwege Schakel gebaseerde gelijktijdigheid van taken wordt afgedwongen voor *ActorId2*.
* Op deze manier ook inschakelen op basis van een gelijktijdigheid van taken wordt afgedwongen voor *ActorId1*, zoals wordt weergegeven bij de uitvoering van *Method1*, *Method2*, en de timercallback namens *ActorId1* gebeurt er in een seriële wijze.
* De uitvoering van *Method1* namens *ActorId1* overlapt met de uitvoering ervan namens *ActorId2*. Dit is omdat op basis van Schakel gelijktijdigheid alleen binnen een actor en niet tussen actoren wordt afgedwongen.
* In sommige van de methode/callback-uitvoeringen de `Task`(C#) / `CompletableFuture`(Java) geretourneerd door de methode/retouraanroep is voltooid nadat de methode retourneert. In sommige andere is asynchrone bewerking al voltooid op het moment dat de methode/callback retourneert. In beide gevallen worden per actorvergrendeling wordt vrijgegeven nadat de methode/callback retourneert zowel de asynchrone bewerking is voltooid.

#### <a name="reentrancy"></a>Herintreding
De runtime actoren kunt herintreding standaard. Dit betekent dat wanneer een actormethode van *Actor A* een methode wordt aangeroepen op *Actor B*, die op zijn beurt een andere methode aanroepen op *Actor A*, methode kan worden uitgevoerd. Dit is omdat deze deel uitmaakt van dezelfde logische aanroep-keten context. Alle timer en herinnering aanroepen beginnen met de nieuwe logische aanroepcontext. Zie de [Reliable Actors herintreding](service-fabric-reliable-actors-reentrancy.md) voor meer informatie.

#### <a name="scope-of-concurrency-guarantees"></a>Bereik van garanties gelijktijdigheid van taken
De runtime actoren biedt deze garanties gelijktijdigheid van taken in situaties waar Hiermee kunt u het aanroepen van deze methoden. Bijvoorbeeld, biedt deze garanties voor de methode-aanroepen die worden uitgevoerd in reactie op aanvragen van clients en voor de timer- en herinnering retouraanroepen. Als de code actor rechtstreeks deze methoden buiten de mechanismen die is opgegeven door de runtime actoren aanroept, kan niet klikt u vervolgens de runtime evenwel garanties gelijktijdigheid van taken. Bijvoorbeeld, als de methode wordt aangeroepen in de context van een taak die is niet gekoppeld aan de taak die is geretourneerd door de actor-methoden, klikt u vervolgens de runtime kan niet gelijktijdigheid garanties bieden. Als de methode wordt aangeroepen vanuit een thread die de actor op een eigen maakt, kan niet klikt u vervolgens de runtime bieden ook garanties gelijktijdigheid van taken. Daarom actoren moeten gebruiken om uit te voeren bewerkingen op de achtergrond, [actor timers en actor herinneringen](service-fabric-reliable-actors-timers-reminders.md) die op basis van Schakel gelijktijdigheid respecteren.

## <a name="next-steps"></a>Volgende stappen
Aan de slag met het bouwen van uw eerste Reliable Actors-service:
   * [Aan de slag met Reliable Actors op .NET](service-fabric-reliable-actors-get-started.md)
   * [Aan de slag met Reliable Actors op Java](service-fabric-reliable-actors-get-started-java.md)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-introduction/concurrency.png
[2]: ./media/service-fabric-reliable-actors-introduction/distribution.png
[3]: ./media/service-fabric-reliable-actors-introduction/actor-communication.png
