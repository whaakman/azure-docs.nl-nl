---
title: Azure Service Fabric-status beheren | Microsoft Docs
description: Informatie over het openen, opslaan en Reliable Actors van Service Fabric-status verwijderen.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: ''
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: ac3afe144b9cf9e2fb307087edb175a603ffe4e9
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34206744"
---
# <a name="access-save-and-remove-reliable-actors-state"></a>Toegang tot, opslaan en Reliable Actors status verwijderen
[Reliable Actors](service-fabric-reliable-actors-introduction.md) single thread-objecten kunnen inkapselen zowel de logica en de status en onderhouden van de status betrouwbaar zijn. Elke instantie actor heeft zijn eigen [statusbeheer](service-fabric-reliable-actors-state-management.md): een dictionary-achtige gegevensstructuur slaat veilig sleutel/waarde-paren. De status manager is een wrapper rond een state-provider. U kunt deze gebruiken voor het opslaan van gegevens, ongeacht welke [persistentie instelling](service-fabric-reliable-actors-state-management.md#state-persistence-and-replication) wordt gebruikt.

Status manager sleutels moeten tekenreeksen zijn. Waarden zijn algemeen en kunnen worden ingesteld, inclusief aangepaste typen. Waarden die zijn opgeslagen in de status manager moeten gegevenscontract serialiseerbaar omdat ze tijdens de replicatie via het netwerk naar andere knooppunten worden verzonden en kunnen worden geschreven naar de schijf, afhankelijk van een actor-statusinstelling persistentie.

De status manager beschrijft algemene woordenlijst methoden voor het beheren van status, vergelijkbaar met die in betrouwbare bibliotheek gevonden.

Zie voor informatie [aanbevolen procedures bij het beheren van de actorstatus](service-fabric-reliable-actors-state-management.md#best-practices).

## <a name="access-state"></a>Toegangsstatus
Status is toegankelijk via de status manager door de sleutel. De status manager methoden zijn alle asynchrone omdat ze schijf-i/o vereisen mogelijk wanneer actoren nog status persistent hebt gemaakt. Na de eerste toegang status objecten in cache zijn opgeslagen in het geheugen. Herhaal toegang operations-objecten rechtstreeks uit het geheugen en retourneren synchroon zonder schijf i/o- of asynchrone context-switching overhead. Een object met de status is verwijderd uit de cache in de volgende gevallen:

* Een actormethode genereert een onverwerkte uitzondering nadat het ophalen van een object van de status manager.
* Een actor opnieuw wordt geactiveerd, na wordt gedeactiveerd of na een storing.
* De state-provider pagina staat op schijf. Dit gedrag is afhankelijk van de implementatie van de persistentieprovider van de status. De standaardprovider voor de status voor de `Persisted` instelling, heeft dit gedrag.

U kunt de status ophalen met behulp van een standaard *ophalen* bewerking die genereert `KeyNotFoundException`(C#) of `NoSuchElementException`(Java) als er een vermelding bestaat voor de sleutel:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task<int> GetCountAsync()
    {
        return this.StateManager.GetStateAsync<int>("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().getStateAsync("MyState");
    }
}
```

U kunt ook de status ophalen met behulp van een *TryGet* methode die niet genereren wordt als een vermelding niet voor een sleutel bestaat:

```csharp
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task<int> GetCountAsync()
    {
        ConditionalValue<int> result = await this.StateManager.TryGetStateAsync<int>("MyState");
        if (result.HasValue)
        {
            return result.Value;
        }

        return 0;
    }
}
```
```Java
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture<Integer> getCountAsync()
    {
        return this.stateManager().<Integer>tryGetStateAsync("MyState").thenApply(result -> {
            if (result.hasValue()) {
                return result.getValue();
            } else {
                return 0;
            });
    }
}
```

## <a name="save-state"></a>Sla de toestand
De methoden voor het ophalen van status manager een verwijzing retourneren naar een object in het lokale geheugen. Wijzigen van dit object in het lokale geheugen alleen leidt niet tot deze blijvend worden opgeslagen. Wanneer een object is opgehaald van de status manager en gewijzigd, moet opnieuw in de status manager worden blijvend opgeslagen worden ingevoegd.

U kunt de status invoegen met behulp van een onvoorwaardelijke *ingesteld*, dit is het equivalent van de `dictionary["key"] = value` syntaxis:

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task SetCountAsync(int value)
    {
        return this.StateManager.SetStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture setCountAsync(int value)
    {
        return this.stateManager().setStateAsync("MyState", value);
    }
}
```

U kunt toevoegen met behulp van een *toevoegen* methode. Deze methode genereert `InvalidOperationException`(C#) of `IllegalStateException`(Java) als er wordt geprobeerd een sleutel toevoegen die al bestaat.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task AddCountAsync(int value)
    {
        return this.StateManager.AddStateAsync<int>("MyState", value);
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().addOrUpdateStateAsync("MyState", value, (key, old_value) -> old_value + value);
    }
}
```

U kunt ook de status toevoegen met behulp van een *TryAdd* methode. Deze methode wordt niet genereren tijdens het toevoegen van een sleutel die al bestaat.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task AddCountAsync(int value)
    {
        bool result = await this.StateManager.TryAddStateAsync<int>("MyState", value);

        if (result)
        {
            // Added successfully!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture addCountAsync(int value)
    {
        return this.stateManager().tryAddStateAsync("MyState", value).thenApply((result)->{
            if(result)
            {
                // Added successfully!
            }
        });
    }
}
```

De status manager aan het einde van een actormethode automatisch alle waarden die zijn toegevoegd of gewijzigd door een bewerking insert of update opgeslagen. Een 'opslaan' kunt opnemen persistent maken op schijf en replicatie, afhankelijk van de instellingen die worden gebruikt. Waarden die niet zijn gewijzigd, zijn niet persistent of gerepliceerd. Als u geen waarden zijn gewijzigd, het opslaan bewerking doet niets. Als u opslaat mislukt, de gewijzigde status is verwijderd en de oorspronkelijke status opnieuw wordt geladen.

U kunt ook de status handmatig opslaan aanroepen van de `SaveStateAsync` methode op basis van actor:

```csharp
async Task IMyActor.SetCountAsync(int count)
{
    await this.StateManager.AddOrUpdateStateAsync("count", count, (key, value) => count > value ? count : value);

    await this.SaveStateAsync();
}
```
```Java
interface MyActor {
    CompletableFuture setCountAsync(int count)
    {
        this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value).thenApply();

        this.stateManager().saveStateAsync().thenApply();
    }
}
```

## <a name="remove-state"></a>Status verwijderen
U kunt de status permanent verwijderen uit een actor status manager door het aanroepen van de *verwijderen* methode. Deze methode genereert `KeyNotFoundException`(C#) of `NoSuchElementException`(Java) als er wordt geprobeerd om te verwijderen van een sleutel die niet bestaat.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public Task RemoveCountAsync()
    {
        return this.StateManager.RemoveStateAsync("MyState");
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().removeStateAsync("MyState");
    }
}
```

U kunt ook de status permanent verwijderen met behulp van de *TryRemove* methode. Deze methode wordt niet genereren wanneer deze probeert te verwijderen van een sleutel die niet bestaat.

```csharp
[StatePersistence(StatePersistence.Persisted)]
class MyActor : Actor, IMyActor
{
    public MyActor(ActorService actorService, ActorId actorId)
        : base(actorService, actorId)
    {
    }

    public async Task RemoveCountAsync()
    {
        bool result = await this.StateManager.TryRemoveStateAsync("MyState");

        if (result)
        {
            // State removed!
        }
    }
}
```
```Java
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
class MyActorImpl extends FabricActor implements  MyActor
{
    public MyActorImpl(ActorService actorService, ActorId actorId)
    {
        super(actorService, actorId);
    }

    public CompletableFuture removeCountAsync()
    {
        return this.stateManager().tryRemoveStateAsync("MyState").thenApply((result)->{
            if(result)
            {
                // State removed!
            }
        });
    }
}
```

## <a name="next-steps"></a>Volgende stappen

Status die opgeslagen in Reliable Actors moet worden geserialiseerd voordat de is naar schijf geschreven en de gerepliceerde voor hoge beschikbaarheid. Meer informatie over [Actor type serialisatie](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

Vervolgens meer wilt weten over [Actor diagnostische gegevens en prestatiebewaking](service-fabric-reliable-actors-diagnostics.md).
