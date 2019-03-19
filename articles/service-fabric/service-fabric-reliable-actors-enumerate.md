---
title: Het inventariseren van actoren op Azure Service Fabric | Microsoft Docs
description: Informatie over het inventariseren van Reliable Actors en de bijbehorende metagegevens.
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
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 300ab4f73fdae0224d2de5e1e1dea6cc6cfa7438
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57864368"
---
# <a name="enumerate-service-fabric-reliable-actors"></a>Service Fabric-betrouwbare actoren opsommen
De Reliable Actors-service kan een client voor het inventariseren van metagegevens over de actoren waarop de service wordt gehost. Omdat de actorservice een gepartitioneerde stateful service is, wordt de opsomming per partitie uitgevoerd. Omdat elke partitie veel actoren bevatten kan, worden de opsomming wordt geretourneerd als een set pagina's met zoekresultaten. De pagina's worden via herhaald totdat alle pagina's worden gelezen. Het volgende voorbeeld ziet u hoe u een lijst met alle actieve actoren in een partitie van een actor-service maakt:

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```



## <a name="next-steps"></a>Volgende stappen
* [Statusbeheer actor](service-fabric-reliable-actors-state-management.md)
* [Actor-levenscyclus en garbagecollection verzameling](service-fabric-reliable-actors-lifecycle.md)
* [Actoren API-referentiedocumentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [.NET-voorbeeldcode](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java-voorbeeldcode](https://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
