---
title: Gebeurtenissen in Azure Service Fabric-actoren op basis van een actor | Microsoft Docs
description: Inleiding op gebeurtenissen voor Service Fabric Reliable Actors.
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
ms.openlocfilehash: fead1cf639a379fc288122e39629909786aa3f14
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57844797"
---
# <a name="actor-events"></a>Actor-gebeurtenissen
Actor-gebeurtenissen bieden een manier om de best-effort meldingen verzenden vanuit de actor aan de clients. Actor-gebeurtenissen zijn ontworpen voor de communicatie van de actor-naar-client en mag niet worden gebruikt voor communicatie van de actor-naar-actor.

De volgende codefragmenten laten zien hoe actor gebeurtenissen in uw toepassing gebruiken.

Definieer een interface waarmee de gebeurtenissen die zijn gepubliceerd door de actor wordt beschreven. Deze interface moet zijn afgeleid van de `IActorEvents` interface. De argumenten van de methoden moeten [data contract serialiseerbare](service-fabric-reliable-actors-notes-on-actor-type-serialization.md). De methoden als resultaat void moeten, als gebeurtenis meldingen zijn een manier en best-effort.

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
De gebeurtenissen die zijn gepubliceerd door de actor in de actor-interface declareren.

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
Op de client, implementeert u de gebeurtenis-handler.

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

Maken van een proxy op de actor die de gebeurtenis publiceert en zich abonneren op gebeurtenissen op de client.

```csharp
var proxy = ActorProxy.Create<IGameActor>(
                    new ActorId(Guid.Parse(arg)), ApplicationName);

await proxy.SubscribeAsync<IGameEvents>(new GameEventsHandler());
```

```Java
GameActor actorProxy = ActorProxyBase.create<GameActor>(GameActor.class, new ActorId(UUID.fromString(args)));

return ActorProxyEventUtility.subscribeAsync(actorProxy, new GameEventsHandler());
```

In het geval van failover, kan de actor failover naar een ander proces of het knooppunt. De actor-proxy de actieve abonnementen beheert en automatisch opnieuw Hiermee abonneert u zich deze. U kunt het abonnementinterval opnieuw via bepalen de `ActorProxyEventExtensions.SubscribeAsync<TEvent>` API. Als u wilt opzeggen, gebruikt u de `ActorProxyEventExtensions.UnsubscribeAsync<TEvent>` API.

Op de actor aangeroepen, moet u de gebeurtenissen publiceren wanneer deze zich voordoen. Als er abonnees aan de gebeurtenis zijn, verzendt de runtime actoren deze de melding.

```csharp
var ev = GetEvent<IGameEvents>();
ev.GameScoreUpdated(Id.GetGuidId(), score);
```
```Java
GameEvents event = getEvent<GameEvents>(GameEvents.class);
event.gameScoreUpdated(Id.getUUIDId(), score);
```


## <a name="next-steps"></a>Volgende stappen
* [Herbetreedbaarheid actor](service-fabric-reliable-actors-reentrancy.md)
* [Actor-diagnose en bewaking van toepassingsprestaties](service-fabric-reliable-actors-diagnostics.md)
* [Actor-API-referentiedocumentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Voorbeeld van C#-code](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [C# .NET Core-voorbeeldcode](https://github.com/Azure-Samples/service-fabric-dotnet-core-getting-started)
* [Java-voorbeeldcode](https://github.com/Azure-Samples/service-fabric-java-getting-started)
