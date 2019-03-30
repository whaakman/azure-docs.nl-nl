---
title: Polymorfisme in Reliable Actors framework | Microsoft Docs
description: Bouw hiërarchieën van .NET-interfaces en typen in het kader van Reliable Actors-functionaliteit en API-definities opnieuw kunt gebruiken.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: vturecek
ms.assetid: ef0eeff6-32b7-410d-ac69-87cba8b8fd46
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: c14b3006184f7bd6dcd1eb67be11bd0214957d72
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662955"
---
# <a name="polymorphism-in-the-reliable-actors-framework"></a>Polymorfisme in Reliable Actors-framework
Het framework Reliable Actors kunt u bouwen met veel van dezelfde technieken die u in objectgeoriënteerde ontwerp gebruiken wilt actoren. Een van deze methoden is polymorfisme zodat typen en -interfaces voor het overnemen van meer ouders gegeneraliseerd. Overname in Reliable Actors framework wordt algemeen volgt het .NET-model met een paar extra beperkingen. In het geval van Java/Linux betekent dit het Java-model.

## <a name="interfaces"></a>Interfaces
Het Reliable Actors-framework, moet u ten minste één interface om te worden geïmplementeerd door uw actortype definiëren. Deze interface wordt gebruikt voor het genereren van een proxyklasse die door clients kan worden gebruikt om te communiceren met uw actoren. Interfaces kunnen overnemen van andere interfaces, zolang elke interface die door een actortype is geïmplementeerd en alle bijbehorende bovenliggende klassen uiteindelijk zijn afgeleid van IActor (C#) of Actor(Java). IActor (C#) en Actor(Java) zijn respectievelijk het platform gedefinieerde basis interfaces voor actoren in de frameworks .NET en Java. Het klassieke polymorfisme-voorbeeld met behulp van shapes ziet dus er ongeveer als volgt uit:

![Hiërarchie van de interface voor vorm actoren][shapes-interface-hierarchy]

## <a name="types"></a>Typen
U kunt ook een hiërarchie van de actor-typen die zijn afgeleid van de basisklasse van Actor die wordt geleverd door het platform maken. In het geval van vormen, mogelijk hebt u een base `Shape`(C#) of `ShapeImpl`(Java)-type:

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

Subtypes van `Shape`(C#) of `ShapeImpl`(Java) methoden van de basis kunt overschrijven.

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

Houd er rekening mee de `ActorService` -kenmerk op de actortype. Dit kenmerk wordt de Reliable Actor-framework dat een service voor het hosten van actoren van dit type automatisch moeten worden gemaakt. In sommige gevallen wilt u mogelijk maken van een basistype dat is uitsluitend bedoeld voor het delen van functies met subtypen en nooit worden gebruikt voor het starten van concrete actoren. In deze gevallen moet u de `abstract` trefwoord om aan te geven dat u nooit een actor op basis van dat type maakt.

## <a name="next-steps"></a>Volgende stappen
* Zie [hoe het framework Reliable Actors maakt gebruik van de Service Fabric-platform](service-fabric-reliable-actors-platform.md) voor betrouwbaarheid, schaalbaarheid en een consistente status.
* Meer informatie over de [actor lifecycle](service-fabric-reliable-actors-lifecycle.md).

<!-- Image references -->

[shapes-interface-hierarchy]: ./media/service-fabric-reliable-actors-polymorphism/Shapes-Interface-Hierarchy.png
