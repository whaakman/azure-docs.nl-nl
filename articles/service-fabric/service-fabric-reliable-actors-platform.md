---
title: Reliable Actors op Service Fabric | Microsoft Docs
description: Hierin wordt beschreven hoe Reliable Actors zijn gelaagd op Reliable Services en de functies van het Service Fabric-platform gebruiken.
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
ms.openlocfilehash: b2369f9468c54f10d01203841b6d7ba44b7ba2de
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a>Hoe Reliable Actors het Service Fabric-platform gebruiken
In dit artikel wordt uitgelegd hoe Reliable Actors op de Azure Service Fabric-platform. Reliable Actors uitvoeren in een framework dat wordt gehost in een implementatie van een betrouwbare stateful service aangeroepen de *actor service*. De service actor bevat alle componenten die vereist zijn voor het beheren van de levenscyclus van en het bericht tijdens het verzenden van uw gebruiken:

* De Runtime Actor levensduur, garbagecollection, beheert en één thread toegang wordt afgedwongen.
* Een actor-service voor externe toegang-listener accepteert de aanroepen van externe toegang actoren en stuurt deze naar een dispatcher doorsturen naar het juiste actor-exemplaar.
* De Actor State-Provider verpakt statusproviders (zoals de betrouwbare verzamelingen state-provider) en biedt een adapter voor actor statusbeheer.

Deze onderdelen vormen samen het betrouwbare Actor-framework.

## <a name="service-layering"></a>Service gelaagdheid
Omdat de actor-service zelf een betrouwbare service alle de [toepassingsmodel](service-fabric-application-model.md), levensduur, [verpakking](service-fabric-package-apps.md), [implementatie](service-fabric-deploy-remove-applications.md), upgrade en concepten van betrouwbare schalen Services dezelfde manier actorservices van toepassing.

![Acteur service gelaagdheid][1]

Het voorgaande diagram toont de relatie tussen de Service Fabric-toepassingsframeworks en gebruikerscode. Blauw elementen het toepassingsframework Reliable Services vertegenwoordigen, oranje staat voor het framework betrouwbare Actor en groen voor gebruikerscode.

In Reliable Services uw service neemt over de `StatefulService` klasse. Deze klasse is afgeleid van `StatefulServiceBase` (of `StatelessService` voor stateless services). In Reliable Actors gebruikt u de actor-service. De actor-service is een andere implementatie van de `StatefulServiceBase` klasse die de actor-patroon waar uw actoren uitgevoerd implementeert. Omdat de actor-service zelf slechts een implementatie van is `StatefulServiceBase`, kunt u uw eigen service die is afgeleid van `ActorService` en implementeren van serviceniveau-functies dezelfde manier als wanneer wordt overgenomen `StatefulService`, zoals:

* Service back-up en herstel.
* Gedeelde functionaliteit voor alle actoren, bijvoorbeeld een Circuitonderbreker.
* Externe procedureaanroepen weer dat op de actor-service zelf en op elke afzonderlijke actor.

> [!NOTE]
> Stateful services worden momenteel niet ondersteund in Java-/ Linux.

Zie voor meer informatie [serviceniveau-functies implementeren in uw service actor](service-fabric-reliable-actors-using.md).

## <a name="application-model"></a>Toepassingsmodel
Actorservices zijn Reliable Services, zodat de toepassingsmodel hetzelfde is. De actor framework build tools genereren echter enkele van de toepassingsbestanden model voor u.

### <a name="service-manifest"></a>Servicemanifest
De actor framework build-hulpprogramma's worden automatisch de inhoud van uw actor-service ServiceManifest.xml bestand genereren. Dit bestand bevat:

* Actortype-service. Naam van het type wordt gegenereerd op basis van uw actor projectnaam. Op basis van het kenmerk persistentie bij uw acteur, wordt de vlag HasPersistedState ook ingesteld dienovereenkomstig.
* Codepakket.
* Configuratiepakket.
* Bronnen en eindpunten.

### <a name="application-manifest"></a>Het toepassingsmanifest
Een standaard-servicedefinitie voor uw actor-service wordt automatisch gemaakt door de actor framework build-hulpprogramma's. De build-hulpprogramma's vullen de standaardeigenschappen voor de service:

* Replica set count wordt bepaald door het kenmerk persistentie bij uw actor. Telkens wanneer die de persistentie-kenmerk op uw actor is gewijzigd, de replica set in de servicedefinitie standaard beginwaarde dienovereenkomstig.
* Partitieschema en bereik zijn ingesteld op uniforme Int64 met het volledige bereik van de Int64-sleutel.

## <a name="service-fabric-partition-concepts-for-actors"></a>Concepten van service Fabric-partitie gebruiken
Actorservices zijn gepartitioneerde stateful services. Elke partitie van een actor-service bevat een set van actoren. Service-partities worden automatisch verdeeld over meerdere knooppunten in het Service Fabric. Actor-exemplaren worden als gevolg hiervan gedistribueerd.

![Acteur partitioneren en distribueren][5]

Reliable Services kunnen worden gemaakt met verschillende partitieschema's en partitie sleutelbereiken. De actor-service gebruikt het partitieschema Int64 alle belangrijke functies Int64 actoren toewijzen aan partities.

### <a name="actor-id"></a>Actor-ID
Elke actor dat gemaakt in de service heeft een unieke ID die is gekoppeld, vertegenwoordigd door de `ActorId` klasse. `ActorId` is een ondoorzichtige ID-waarde die voor gelijkmatige verdeling van actoren kan worden gebruikt tussen de servicepartities door het genereren van willekeurige id's:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Elke `ActorId` wordt opgedeeld naar een gegevenstype Int64. Dit is de reden waarom de actor-service een partitieschema Int64 met het volledige bereik van de Int64-sleutel gebruiken moet. Aangepaste ID-waarden kunnen echter worden gebruikt voor een `ActorID`, met inbegrip van GUID's / UUID's, tekenreeksen en Int64s.

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

Wanneer u van GUID's / UUID's en tekenreeksen gebruikmaakt, worden de waarden worden opgedeeld in een Int64. Echter, wanneer u expliciet bieden een gegevenstype Int64 naar een `ActorId`, de Int64 wordt rechtstreeks verwijzen naar een partitie zonder verdere hashing. U kunt deze techniek om te bepalen welke partitie in de actoren worden geplaatst.


## <a name="next-steps"></a>Volgende stappen
* [Statusbeheer actor](service-fabric-reliable-actors-state-management.md)
* [Acteur lifecycle en garbage collection](service-fabric-reliable-actors-lifecycle.md)
* [API-naslagdocumentatie actors](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Voorbeeldcode voor .NET](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Voorbeeldcode voor Java](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
