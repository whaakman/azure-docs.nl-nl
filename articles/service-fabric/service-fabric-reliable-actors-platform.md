---
title: Betrouwbare actoren op Service Fabric | Microsoft Docs
description: Hierin wordt beschreven hoe Reliable Actors zijn gelaagd op Reliable Services en de functies van de Service Fabric-platform.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: vturecek
ms.openlocfilehash: bd3a77e1486d4af61539e55f67811221dd971b37
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52582330"
---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a>Hoe Reliable Actors het Service Fabric-platform gebruiken
In dit artikel wordt uitgelegd hoe Reliable Actors werkt op het Azure Service Fabric-platform. Reliable Actors uitvoeren in een framework dat wordt gehost in een implementatie van een stateful betrouwbare service met de naam de *actorservice*. De actorservice bevat alle onderdelen die nodig zijn voor het beheren van de levenscyclus en bericht verzenden voor uw actoren:

* De Actor-Runtime levenscyclus, garbagecollection, beheert en één thread toegang wordt afgedwongen.
* Een listener van een actor-service voor externe toegang accepteert de aanroepen van externe toegang naar actoren en stuurt deze naar een functie voor berichtverzending om te leiden naar de juiste actor-instantie.
* De Actor State-Provider wordt verpakt statusproviders (zoals de betrouwbare verzamelingen state-provider) en biedt een adapter voor statusbeheer actor.

Deze onderdelen vormen samen het Reliable Actor-framework.

## <a name="service-layering"></a>Service-lagen
Omdat de actorservice zelf een betrouwbare service is, alle de [toepassingsmodel](service-fabric-application-model.md), levenscyclus, [verpakking](service-fabric-package-apps.md), [implementatie](service-fabric-deploy-remove-applications.md), upgraden en concepten van betrouwbare schalen Services dezelfde manier actorservices van toepassing.

![Actor-service-lagen][1]

Het vorige diagram toont de relatie tussen de frameworks van Service Fabric-toepassing en de gebruikerscode. Blauwe elementen vertegenwoordigen de Reliable Services-toepassingsframework, oranje vertegenwoordigt de Reliable Actor-framework en groen gebruikerscode.

In Reliable Services uw service neemt de `StatefulService` klasse. Deze klasse is afgeleid van `StatefulServiceBase` (of `StatelessService` voor stateless services). In Reliable Actors gebruikt u de actor-service. De actorservice is een andere implementatie van de `StatefulServiceBase` klasse die de actor-patroon waar uw actoren worden uitgevoerd. Omdat de actorservice zelf alleen een implementatie van is `StatefulServiceBase`, kunt u uw eigen service die is afgeleid van `ActorService` en functies op serviceniveau implementeren de dezelfde manier als wanneer wordt overgenomen `StatefulService`, zoals:

* Service maken en herstellen.
* Gedeelde functionaliteit voor alle actoren, bijvoorbeeld een Circuitonderbreker.
* Externe procedureaanroepen op de actorservice zelf en op elke afzonderlijke actor.

Zie voor meer informatie, [functies op serviceniveau implementeren in uw actor-service](service-fabric-reliable-actors-using.md).

## <a name="application-model"></a>Toepassingsmodel
Actorservices zijn betrouwbare Services, zodat het toepassingsmodel hetzelfde is. De actor framework opbouwhulpprogramma's genereren echter enkele van de toepassingsbestanden model voor u.

### <a name="service-manifest"></a>Servicemanifest
De actor framework opbouwhulpprogramma's automatisch genereren van de inhoud van uw actor-service ServiceManifest.xml-bestand. Dit bestand bevat:

* Actortype-service. Naam van het type wordt gegenereerd op basis van de naam van uw actor-project. Op basis van het kenmerk persistentie van uw actor, wordt de vlag HasPersistedState ook ingesteld dienovereenkomstig.
* Codepakket.
* Configuratiepakket.
* Resources en eindpunten.

### <a name="application-manifest"></a>Manifest van de toepassing
De definitie van een standaard-service voor uw actorservice wordt automatisch gemaakt door de actor framework build-hulpprogramma's. De build-hulpprogramma's vullen van de standaard-service-eigenschappen:

* Replica set count wordt bepaald door het kenmerk persistentie van uw actor. Telkens wanneer die het kenmerk persistentie van uw actor wordt gewijzigd, het aantal replica instellen in de definitie van de standaard-service wordt opnieuw ingesteld dienovereenkomstig.
* Partitieschema en bereik zijn ingesteld op uniforme Int64 met het volledige bereik van de Int64-sleutel.

## <a name="service-fabric-partition-concepts-for-actors"></a>Concepten van service Fabric-partitie voor actors
Actorservices zijn gepartitioneerde stateful services. Elke partitie van een actor-service bevat een set van actoren. Service-partities worden automatisch verdeeld over meerdere knooppunten in Service Fabric. Actor-exemplaren worden als gevolg hiervan gedistribueerd.

![Actor partitioneren en distributie][5]

Reliable Services kunnen worden gemaakt met verschillende partitieschema en partitie sleutelbereiken. De actor-service maakt gebruik van het partitieschema Int64 met de volledige Int64 sleutelbereik actoren toewijzen aan partities.

### <a name="actor-id"></a>Actor-id
Elke acteur dat gemaakt in de service heeft een unieke ID die is gekoppeld, vertegenwoordigd door de `ActorId` klasse. `ActorId` is een onduidelijke ID-waarde die voor gelijkmatige verdeling van actors kan worden gebruikt voor de servicepartities door het genereren van willekeurige id's:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Elke `ActorId` wordt opgedeeld naar een gegevenstype Int64. Dit is de reden waarom de actor-service moet een gegevenstype Int64-partitieschema met het volledige bereik van de Int64-sleutel gebruiken. Aangepaste id-waarden kunnen echter worden gebruikt voor een `ActorID`, met inbegrip van GUID/UUID's, tekenreeksen en Int64s.

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

Wanneer u GUID/UUID's en tekenreeksen gebruikt, worden de waarden worden opgedeeld in een gegevenstype Int64. Echter, wanneer u expliciet bieden een gegevenstype Int64 naar een `ActorId`, de Int64 zonder verdere hashing rechtstreeks naar een partitie wordt toegewezen. U kunt deze techniek om te bepalen welke partitie in de actoren worden geplaatst.


## <a name="next-steps"></a>Volgende stappen
* [Statusbeheer actor](service-fabric-reliable-actors-state-management.md)
* [Actor-levenscyclus en garbagecollection verzameling](service-fabric-reliable-actors-lifecycle.md)
* [Actoren API-referentiedocumentatie](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.actors?redirectedfrom=MSDN&view=azure-dotnet#microsoft_servicefabric_actors)
* [.NET-voorbeeldcode](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java-voorbeeldcode](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
