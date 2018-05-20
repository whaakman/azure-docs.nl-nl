---
title: Gebeurtenissen in Azure microservices actor gebaseerde | Microsoft Docs
description: Inleiding tot de gebeurtenissen voor Service Fabric Reliable Actors.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: aa01b0f7-8f88-403a-bfe1-5aba00312c24
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/06/2017
ms.author: amanbha
ms.openlocfilehash: ed920c8d4ff7254b19c6eef8f5961593bb56bacf
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="actor-events"></a>Actor-gebeurtenissen
Acteur gebeurtenissen bieden een manier om het best-effort meldingen verzenden vanuit de actor aan de clients. Acteur gebeurtenissen zijn ontworpen voor communicatie actor-naar-client en mag niet worden gebruikt voor actor-actor-communicatie.

De volgende codefragmenten laten zien hoe actor-gebeurtenissen in uw toepassing gebruiken.

Definieer een interface die worden de gebeurtenissen die zijn gepubliceerd door de actor beschreven. Deze interface moet worden afgeleid van de `IActorEvents` interface. De argumenten van de methoden moeten [gegevenscontract serialiseerbaar](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). De methoden moeten als retourtype void, als gebeurtenis meldingen zijn een manier en zo goed mogelijke poging.

```csharp
public interface IGameEvents : IActorEvents
{
    void GameScoreUpdated(Guid gameId, string currentScore);
}
```
```Java
public interface GameEvents implements ActorEvents
{
    void gameScoreUpdated(UUID gameId, String currentScore);
}
```
De gebeurtenissen die zijn gepubliceerd door de actor in de interface actor declareren.

```csharp
public interface IGameActor : IActor, IActorEventPublisher<IGameEvents>
{
    Task UpdateGameStatus(GameStatus status);

    Task<string> GetGameScore();
}
```
```Java
public interface GameActor extends Actor, ActorEventPublisherE<GameEvents>
{
    CompletableFuture<?> updateGameStatus(GameStatus status);

    CompletableFuture<String> getGameScore();
}
```
Implementeert de gebeurtenis-handler aan de clientzijde.

```csharp
class GameEventsHandler : IGameEvents
{
    public void GameScoreUpdated(Guid gameId, string currentScore)
    {
        Console.WriteLine(@"Updates: Game: {0}, Score: {1}", gameId, currentScore);
    }
}
```

```Java
class GameEventsHandler implements GameEvents {
    public void gameScoreUpdated(UUID gameId, String currentScore)
    {
        System.out.println("Updates: Game: "+gameId+" ,Score: "+currentScore);
    }
}
```

Maken van een proxy voor de actor die de gebeurtenis publiceert en zich abonneren op de gebeurtenissen op de client.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

In het geval van failovers, kan de actor failover naar een ander proces of het knooppunt. De proxy actor beheert de actieve abonnementen en automatisch opnieuw op is geabonneerd ze. U kunt het abonnementinterval opnieuw via bepalen de `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API. Als u wilt opzeggen, gebruiken de `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.

Op de actor publiceert u de gebeurtenissen als ze gebeuren. Als er abonnees aan de gebeurtenis zijn, verzendt de runtime actoren deze de melding.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a>Volgende stappen
* [Acteur herintreding](service-fabric-reliable-actors-reentrancy.md)
* [Acteur diagnostische gegevens en prestatiebewaking](service-fabric-reliable-actors-diagnostics.md)
* [Acteur API-naslagdocumentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [C# voorbeeldcode](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [C# .NET Core voorbeeldcode](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [Voorbeeldcode voor Java](http://github.com/Azure-Samples/service-fabric-java-getting-started)
