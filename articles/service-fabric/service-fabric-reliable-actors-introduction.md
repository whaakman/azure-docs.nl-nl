---
title: Service Fabric Reliable Actors overzicht | Microsoft Docs
description: Inleiding tot de Service Fabric Reliable Actors-programmeermodel.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: 7fdad07f-f2d6-4c74-804d-e0d56131f060
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: 363cba145ed4d5bcf138cf3f7130763891c51e8b
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258058"
---
# <a name="introduction-to-service-fabric-reliable-actors"></a>Inleiding tot Service Fabric Reliable Actors
Reliable Actors is een Service Fabric-toepassingsframework op basis van de [virtuele Actor](https://research.microsoft.com/en-us/projects/orleans/) patroon. De betrouwbare actoren-API biedt een single-threaded programming model gebouwd op de schaalbaarheid en betrouwbaarheid garanties geboden door de Service Fabric.

## <a name="what-are-actors"></a>Wat zijn Actors?
Een actor is een eenheid geïsoleerd, onafhankelijk van reken- en status met één thread worden uitgevoerd. De [actor patroon](https://en.wikipedia.org/wiki/Actor_model) is een rekenkundige model voor gelijktijdige of gedistribueerde systemen in die een groot aantal van deze actoren kunt tegelijkertijd uitvoeren en onafhankelijk van elkaar. Actoren met elkaar kunnen communiceren en ze meer actors kunnen maken.

### <a name="when-to-use-reliable-actors"></a>Wanneer u betrouwbare actoren
Service Fabric Reliable Actors is een implementatie van het ontwerppatroon actor. Net als bij een ontwerppatroon software overeenkomt de beslissing of u wilt gebruiken van een specifiek patroon is gemaakt op basis van het al dan niet de probleem voor het ontwerpen van een software met het patroon.

Hoewel de actor ontwerpen patroon kan worden een goede aanpassen aan een aantal problemen met gedistribueerde systemen en scenario's, zorgvuldige afweging van de beperkingen van het patroon en de implementatie van het framework moet worden gemaakt. Houd rekening met het patroon actor om uw probleem of scenario te modelleren als als algemene richtlijn geldt:

* De adresruimte van uw probleem betrekking heeft op een groot aantal (duizenden of meerdere) van klein, onafhankelijk en gescheiden eenheden van de status en logica.
* U wilt werken met één thread objecten waarvoor geen aanzienlijke interactie met externe componenten, inclusief de status opvragen van een set van actoren.
* Uw actorexemplaren niet aanroepfuncties met onvoorspelbare vertragingen worden geblokkeerd door uitgifte van i/o-bewerkingen.

## <a name="actors-in-service-fabric"></a>Actoren in Service Fabric
In Service Fabric, actors zijn geïmplementeerd in het kader van Reliable Actors: een actor-patroon gebaseerde toepassingsframework gebouwd boven [Service Fabric Reliable Services](service-fabric-reliable-services-introduction.md). Elke Reliable Actor-service die u schrijft is eigenlijk een gepartitioneerde, stateful betrouwbare Service.

Elke actor wordt gedefinieerd als een exemplaar van een actortype, identiek zijn aan de manier waarop die een .NET-object een exemplaar van een .NET-type is. Bijvoorbeeld, kan er een actortype waarmee de functionaliteit van een Rekenmachine en kunnen er veel actoren van dat type die op verschillende knooppunten in een cluster worden gedistribueerd. Elke dergelijke actor wordt uniek geïdentificeerd door een actor-ID.

## <a name="actor-lifetime"></a>Actor-levensduur
Service Fabric-actoren zijn virtueel, wat betekent dat hun levensduur niet is gebonden aan de weergave in het geheugen. Als gevolg hiervan hoeft ze niet expliciet worden gemaakt of vernietigd. De Reliable Actors-runtime wordt automatisch geactiveerd voor een actor de eerste keer die wordt een aanvraag voor die actor-ID ontvangen. Als een actor niet voor een bepaalde periode gebruikt wordt, de Reliable Actors-runtime garbagecollection-verzamelt het object in het geheugen. Dit wordt ook kennis van de aanwezigheid van de actor onderhouden hoeft het later opnieuw worden geactiveerd. Zie voor meer informatie, [Actor levenscyclus en garbagecollection verzameling](service-fabric-reliable-actors-lifecycle.md).

Deze abstractie van de levensduur van virtuele actor voert enkele waarschuwingen als gevolg van het actormodel virtuele en in feite de Reliable Actors-toepassing op tijdstippen afwijkt van dit model.

* Een actor wordt automatisch geactiveerd (waardoor een actorobject om te worden samengesteld) de eerste keer dat een bericht is verzonden naar de actor-ID. Na een bepaalde tijd is de actorobject verwijderd. In de toekomst met behulp van de actor-ID opnieuw, zorgt ervoor dat een nieuwe actorobject om te worden samengesteld. Status van een actor outlives levensduur van het object wanneer die zijn opgeslagen in de status manager.
* Aanroepen van een actormethode om een actor-ID activeert die actor. Om deze reden hebben actor typen hun constructor die impliciet wordt aangeroepen door de runtime. Daarom niet kan clientcode parameters doorgeven aan de constructor van het actortype, hoewel parameters kunnen worden doorgegeven aan van de actor-constructor door de service zelf. Het resultaat is dat actors, in een status gedeeltelijk geïnitialiseerd op het moment dat andere methoden worden genoemd, kunnen worden gebouwd als de actor initialisatieparameters van de client vereist. Er is geen enkel ingangspunt van het activeren van een actor van de client.
* Hoewel Reliable Actors impliciet actor-objecten maken. u hebt de mogelijkheid expliciet verwijderen van een actor en de status.

## <a name="distribution-and-failover"></a>Distributie en failover
Voor schaalbaarheid en betrouwbaarheid, Service Fabric distribueert actoren in het cluster en automatisch van mislukte knooppunten overzet naar in orde zijn, zoals vereist. Dit is een abstractie via een [gepartitioneerde, stateful betrouwbare Service](service-fabric-concepts-partitioning.md). Distributie, schaalbaarheid, betrouwbaarheid en automatische failover zijn alle opgegeven omdat het feit dat de actoren worden uitgevoerd binnen een stateful betrouwbare Service met de naam de *Actorservice*.

Actoren worden verdeeld over de partities van de Actor-Service en deze partities worden verdeeld over de knooppunten in een Service Fabric-cluster. Elke servicepartitie bevat een set van actoren. Service Fabric beheert en failover van de servicepartities.

Bijvoorbeeld, zou een actor-service met negen partities die worden geïmplementeerd met drie knooppunten met behulp van de plaatsing van de standaard actor partitie thusly worden gedistribueerd:

![Distributie van betrouwbare actoren][2]

Het Actorframework beheert partitie schema en de sleutel bereik-instellingen voor u. Dit vereenvoudigt het bepaalde opties, maar bevatten ook overweging:

* Reliable Services kunt u kiezen van een partitieschema, een sleutelbereik (als u een bereik partitieschema) en het aantal partities. Reliable Actors is beperkt tot het bereik partitieschema (het uniform Int64 schema) en moet u het volledige bereik van de Int64-sleutel gebruiken.
* Standaard worden willekeurig actoren geplaatst in partities leidt tot gelijkmatige verdeling.
* Omdat actoren willekeurig worden geplaatst, moet worden verwacht actor-bewerkingen moet altijd de netwerkcommunicatie, met inbegrip van serialisatie en deserialisatie van methode aanroep van gegevens, waarbij extra kosten voor latentie en overhead.
* In geavanceerde scenario's is het mogelijk besturingselement actor partitie plaats met behulp van Int64 actor-id's die worden toegewezen aan specifieke partities. Echter, als dit dus kan resulteren in een onevenwichtige verdeling van actoren over meerdere partities.

Raadpleeg voor meer informatie over de manier waarop de actorservices zijn gepartitioneerd, [voor actors het partitioneren van concepten](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors).

## <a name="actor-communication"></a>Actor-communicatie
Actor-interacties worden gedefinieerd in een interface die wordt gedeeld door de actor waarmee de interface wordt geïmplementeerd als de client die een proxy naar een actor via dezelfde interface opgehaald. Omdat deze interface wordt gebruikt om aan te roepen actor methoden asynchroon, moet elke methode op de interface taak retourneren.

Methodeaanroepen en de antwoorden daarop uiteindelijk op netwerkaanvragen in het cluster, zodat de argumenten en de resultaattypen van de taken die ze terug moeten serialiseerbare door het platform. In het bijzonder, moeten ze [data contract serialiseerbare](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

### <a name="the-actor-proxy"></a>De actor-proxy
De client Reliable Actors-API biedt communicatie tussen een actor-exemplaar en een actor-client. Om te communiceren met een actor, maakt u een client een actor proxy-object dat de actorinterface implementeert. De client communiceert met de actor door het aanroepen van methoden voor de proxyobject. De actor-proxy kan worden gebruikt voor communicatie van client-naar-acteur en actor-naar-actor.

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


Houd er rekening mee dat de twee soorten informatie gebruikt voor het maken van de actor-proxy-object de actor-ID en naam van de toepassing zijn. De actor-ID is uniek voor de actor, terwijl de naam van de toepassing identificeert de [Service Fabric-toepassing](service-fabric-reliable-actors-platform.md#application-model) waar de actor wordt geïmplementeerd.

De `ActorProxy`(C#) / `ActorProxyBase`klasse (Java) op de client voert de benodigde oplossing door aan de actor op ID zoeken en openen van een communicatiekanaal met het. Het is ook opnieuw probeert om te zoeken van de actor in het geval van communicatiefouten en failovers. Als gevolg hiervan heeft levering van berichten de volgende kenmerken:

* Levering van berichten is best-effort.
* Actoren mogelijk dubbele berichten ontvangen van dezelfde client.

## <a name="concurrency"></a>Gelijktijdigheid
De Reliable Actors-runtime biedt een eenvoudige toegang op basis van een inschakelen voor toegang tot de actor-methoden. Dit betekent dat niet meer dan één thread op elk gewenst moment actief zijn binnen een actorobject-code kan zijn. Schakel gebaseerde toegang vereenvoudigt gelijktijdige systemen omdat er geen behoefte synchronisatiemechanismen voor toegang tot gegevens. Het betekent ook systemen moeten zijn ontworpen met speciale overwegingen voor de toegang tot één thread aard van elk exemplaar van de actor.

* Een enkele actor-exemplaar kan niet meer dan één aanvraag tegelijk verwerken. Een exemplaar van de actor kan een knelpunt doorvoer veroorzaken als het is waarschijnlijk om gelijktijdige aanvragen te verwerken.
* Actoren kunnen impasse op elkaar worden verbonden als er een circulaire verzoek tussen twee actoren tijdens een externe aanvraag wordt gedaan bij een van de actoren gelijktijdig. De actor-runtime wordt automatisch een time-out op de actoraanroepen en genereert een uitzondering voor de oproepende functie om te onderbreken impasse situaties.

![Reliable Actors-communicatie][3]

### <a name="turn-based-access"></a>Op basis van de inschakelen
Beurt bestaat uit het uitvoeren van een actormethode in reactie op een aanvraag van andere actors of clients of de volledige uitvoering van een [timer/herinnering](service-fabric-reliable-actors-timers-reminders.md) retouraanroep. Hoewel deze methoden en callbacks asynchroon zijn, kan de runtime actoren deze komt niet interleave. Een inschakelen moet volledig is voltooid voordat u een nieuwe inschakelen is toegestaan. Met andere woorden, een actor methode of timer/herinnering callbackfunctie die momenteel wordt uitgevoerd moet volledig worden voltooid voordat u een nieuwe aanroep naar een methode of retouraanroep is toegestaan. Een methode of de retouraanroep wordt beschouwd als klaar bent met het als de uitvoering van de methode heeft geretourneerd of callback en de taak die wordt geretourneerd door de methode of een retouraanroep is voltooid. Het verdient benadrukken dat die schakelt op basis van gelijktijdigheid zelfs in verschillende methoden en timers callbacks in acht wordt genomen.

De runtime Actors afgedwongen gelijktijdigheid inschakelen op basis van aanschaf van een afzonderlijke actorvergrendeling die aan het begin van een inschakelen en de vergrendeling aan het einde van de inschakelen. Inschakelen op basis van gelijktijdigheid wordt dus op basis van per actor en niet tussen actors afgedwongen. Actor-methoden en timer/herinnering callbacks kunnen tegelijkertijd uitvoeren namens andere actors.

Het volgende voorbeeld wordt de bovenstaande concepten. Houd rekening met een actortype waarmee twee asynchrone methoden (bijvoorbeeld *Method1* en *Method2*), een timer, en een herinnering. Het volgende diagram toont een voorbeeld van een tijdlijn voor het uitvoeren van deze methoden en callbacks namens twee actoren (*ActorId1* en *ActorId2*) die deel uitmaken van dit actortype.

![Betrouwbare actoren runtime inschakelen op basis van gelijktijdigheid- en toegangsbeheer][1]

In dit diagram met de volgende deze conventies:

* Elke verticale lijn ziet u de logische stroom van de uitvoering van een methode of een retouraanroep namens een specifieke actor.
* De gebeurtenissen die zijn gemarkeerd op de verticale lijn die optreden in chronologische volgorde met nieuwere gebeurtenissen hieronder oudere zijn.
* Verschillende kleuren worden gebruikt voor tijdlijnen overeenkomt met de verschillende actoren.
* Markering wordt om aan te geven van de duur waarvoor per actorvergrendeling wordt vastgehouden namens een methode of terugbellen gebruikt.

Enkele belangrijke punten om te overwegen:

* Terwijl *Method1* wordt uitgevoerd namens *ActorId2* in reactie op clientaanvraag *xyz789*, een andere clientaanvraag (*abc123*) die binnenkomt vereist ook *Method1* moet worden uitgevoerd door *ActorId2*. Echter, de tweede uitvoering van *Method1* begint niet totdat de vorige uitvoering is voltooid. Op deze manier een herinnering geregistreerd door *ActorId2* wordt geactiveerd tijdens het *Method1* wordt uitgevoerd in reactie op clientaanvraag *xyz789*. De callback herinnering wordt uitgevoerd nadat de beide uitvoeringen van *Method1* zijn voltooid. Dit wordt veroorzaakt door inschakelen op basis van gelijktijdigheid wordt afgedwongen voor *ActorId2*.
* Op deze manier ook inschakelen op basis van gelijktijdigheid wordt afgedwongen voor *ActorId1*, zoals wordt geïllustreerd door het uitvoeren van *Method1*, *Method2*, en de timercallback namens  *ActorId1* in een seriële manier gebeurt.
* Uitvoering van *Method1* namens *ActorId1* overlapt met de uitvoering ervan namens *ActorId2*. Dit is omdat inschakelen op basis van gelijktijdigheid wordt alleen binnen een actor en niet tussen actors afgedwongen.
* In sommige van de uitvoeringen methode/callback de `Task`(C#) / `CompletableFuture`(Java) geretourneerd door de methode/de retouraanroep is voltooid na de methode retourneert. In andere, is de asynchrone bewerking al voltooid op het moment dat de methode/de retouraanroep wordt geretourneerd. In beide gevallen worden per actorvergrendeling wordt vrijgegeven nadat de methode/de retouraanroep retourneert zowel de asynchrone bewerking is voltooid.

### <a name="reentrancy"></a>Herintreding
Herbetreedbaarheid van kan de runtime Actors standaard. Dit betekent dat als een actormethode om *Actor A* een methode aanroepen op *Actor B*, die op zijn beurt een andere methode aanroepen op *Actor A*, dat de methode is toegestaan om uit te voeren. Dit is omdat het deel van dezelfde logische-aanroepketen context uitmaakt. Alle aanroepen voor timer en een herinnering beginnen met de nieuwe logische call-context. Zie de [herbetreedbaarheid van betrouwbare actoren](service-fabric-reliable-actors-reentrancy.md) voor meer informatie.

### <a name="scope-of-concurrency-guarantees"></a>Bereik van gelijktijdigheid garanties
De runtime actoren biedt deze garanties gelijktijdigheid in situaties waar deze Hiermee bepaalt u het aanroepen van deze methoden. Bijvoorbeeld, biedt deze garanties voor de methodes die zijn uitgevoerd in reactie op aanvragen van clients, evenals voor de timer en een herinnering callbacks. Als de actor-code roept rechtstreeks aan deze methoden buiten de door de runtime actoren geleverde rapportagemethoden, kan niet klikt u vervolgens de runtime bieden echter geen garanties gelijktijdigheid. Bijvoorbeeld, als de methode wordt aangeroepen in de context van een taak die niet is gekoppeld aan de taak die wordt geretourneerd door de actor-methoden, kan geen klikt u vervolgens de runtime garanties bieden voor de gelijktijdigheid van taken. Als de methode wordt aangeroepen vanuit een thread die de actor wordt gemaakt op een eigen, kan geen klikt u vervolgens de runtime ook garanties bieden voor de gelijktijdigheid van taken. Daarom actoren moeten gebruiken om uit te voeren bewerkingen op de achtergrond, [actor timers en herinneringen actor](service-fabric-reliable-actors-timers-reminders.md) die aansluiten bij inschakelen op basis van gelijktijdigheid.

## <a name="next-steps"></a>Volgende stappen
Aan de slag met het bouwen van uw eerste Reliable Actors-service:
   * [Aan de slag met Reliable Actors op .NET](service-fabric-reliable-actors-get-started.md)
   * [Aan de slag met Reliable Actors in Java](service-fabric-reliable-actors-get-started-java.md)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-introduction/concurrency.png
[2]: ./media/service-fabric-reliable-actors-introduction/distribution.png
[3]: ./media/service-fabric-reliable-actors-introduction/actor-communication.png
