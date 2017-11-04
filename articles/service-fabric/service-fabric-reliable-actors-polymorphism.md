---
title: Polymorfisme in het kader Reliable Actors | Microsoft Docs
description: "Maak hiërarchieën van .NET-interfaces en de typen in het kader Reliable Actors functionaliteit en API-definities."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: vturecek
ms.assetid: ef0eeff6-32b7-410d-ac69-87cba8b8fd46
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 38a86b25b30420c6f0b3027258fa094529c90278
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="polymorphism-in-the-reliable-actors-framework"></a>Polymorfisme in het kader Reliable Actors
Het framework Reliable Actors kunt u met behulp van veel van dezelfde technieken die u in objectgeoriënteerde ontwerp gebruiken wilt actoren bouwen. Een van deze methoden polymorfisme die kunt typen en interfaces voor het overnemen van meer is gegeneraliseerd ouders. Overname in het kader Reliable Actors volgt doorgaans het .NET-model met een paar extra beperkingen. In geval van een Java-/ Linux volgt het Java-model.

## <a name="interfaces"></a>Interfaces
Het framework Reliable Actors moet u ten minste één interface worden geïmplementeerd door uw actortype definiëren. Deze interface wordt gebruikt voor het genereren van een proxyklasse die door clients kan worden gebruikt om te communiceren met uw actoren. Interfaces kunnen overnemen van andere interfaces, zolang elke interface die wordt geïmplementeerd met een actortype en alle bijbehorende bovenliggende klassen uiteindelijk worden afgeleid van IActor(C#) of Actor(Java). IActor(C#) en Actor(Java) zijn respectievelijk de platform gedefinieerde basisinterfaces voor actoren in de frameworks .NET en Java. Dus in het klassieke polymorfisme voorbeeld met behulp van vormen mogelijk als volgt uitzien:

![Hiërarchie van de interface voor vorm actors][shapes-interface-hierarchy]

## <a name="types"></a>Typen
U kunt ook een hiërarchie van actor-typen die zijn afgeleid van de basisklasse van Actor die wordt geleverd door het platform maken. In het geval van vormen, moet u wellicht een base `Shape`(C#) of `ShapeImpl`(Java)-type:

```csharp
public abstract class Shape : Actor, IShape
{
    public abstract Task<int> GetVerticeCount();

    public abstract Task<double> GetAreaAsync();
}
```
```Java
public abstract class ShapeImpl extends FabricActor implements Shape
{
    public abstract CompletableFuture<int> getVerticeCount();

    public abstract CompletableFuture<double> getAreaAsync();
}
```

Subtypes van `Shape`(C#) of `ShapeImpl`(Java) kunt u methoden van de base overschrijven.

```csharp
[ActorService(Name = "Circle")]
[StatePersistence(StatePersistence.Persisted)]
public class Circle : Shape, ICircle
{
    public override Task<int> GetVerticeCount()
    {
        return Task.FromResult(0);
    }

    public override async Task<double> GetAreaAsync()
    {
        CircleState state = await this.StateManager.GetStateAsync<CircleState>("circle");

        return Math.PI *
            state.Radius *
            state.Radius;
    }
}
```
```Java
@ActorServiceAttribute(name = "Circle")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class Circle extends ShapeImpl implements Circle
{
    @Override
    public CompletableFuture<Integer> getVerticeCount()
    {
        return CompletableFuture.completedFuture(0);
    }

    @Override
    public CompletableFuture<Double> getAreaAsync()
    {
        return (this.stateManager().getStateAsync<CircleState>("circle").thenApply(state->{
          return Math.PI * state.radius * state.radius;
        }));
    }
}
```

Opmerking de `ActorService` kenmerk van het actortype. Dit kenmerk instrueert het betrouwbare Actor-framework dat een service voor het hosten van actoren van dit type automatisch moeten worden gemaakt. In sommige gevallen wilt u mogelijk maken van een basistype dat is uitsluitend bedoeld voor delen functionaliteit met subtypen en nooit instantiëren concrete actoren worden gebruikt. In deze gevallen moet u de `abstract` trefwoord om aan te geven dat u nooit een actor op basis van dat type maakt.

## <a name="next-steps"></a>Volgende stappen
* Zie [hoe het framework Reliable Actors maakt gebruik van de Service Fabric-platform](service-fabric-reliable-actors-platform.md) voor betrouwbaarheid, schaalbaarheid en consistente status.
* Meer informatie over de [actor lifecycle](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
