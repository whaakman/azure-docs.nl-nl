---
title: Azure Service Fabric-status beheren | Microsoft Docs
description: Leer hoe u toegang tot, het opslaan en het verwijderen van Service Fabric Reliable Actors-status.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 37cf466a-5293-44c0-a4e0-037e5d292214
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 7c10d00916ef65767c98616c7337bfa444c339a9
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664723"
---
# <a name="access-save-and-remove-reliable-actors-state"></a>Toegang tot, opslaan en Reliable Actors-status verwijderen
[Reliable Actors](service-fabric-reliable-actors-introduction.md) zijn single-threaded objecten die kunnen inkapselen van zowel de logica en de status en status op betrouwbare wijze beheren. Elke actor-exemplaar heeft een eigen [status manager](service-fabric-reliable-actors-state-management.md): een woordenlijst-achtige gegevensstructuur waarmee op betrouwbare wijze worden opgeslagen sleutel/waarde-paren. De status manager is een wrapper rond een state-provider. U kunt deze gebruiken voor het opslaan van gegevens, ongeacht welke [persistentie instelling](service-fabric-reliable-actors-state-management.md#state-persistence-and-replication) wordt gebruikt.

Status manager sleutels moeten tekenreeksen zijn. Waarden zijn algemeen en kan elk type zijn, met inbegrip van aangepaste typen. Waarden die zijn opgeslagen in de status manager moet serializable-gegevenscontract omdat ze tijdens de replicatie via het netwerk naar andere knooppunten kunnen worden verzonden en kunnen worden geschreven naar de schijf, afhankelijk van de instelling van persistentie van een actor.

De status manager bevat algemene woordenlijst methoden voor het beheren van de status, vergelijkbaar met die in een betrouwbare Dictionary gevonden.

Zie voor meer informatie, [aanbevolen procedures bij het beheren van de actorstatus](service-fabric-reliable-actors-state-management.md#best-practices).

## <a name="access-state"></a>Toegangsstatus
Status toegankelijk is via het statusbeheerherstel dat door de sleutel. Status manager methoden zijn alle asynchrone omdat ze mogelijk schijf-i/o wanneer actoren hebt opgeslagen status. Na de eerste keer opent, worden de status van objecten in het cachegeheugen opgeslagen. Herhaal de toegang tot operations toegang-objecten rechtstreeks uit het geheugen en synchroon retourneren waarbij extra kosten voor schijf-i/o- of asynchrone context wisselt overhead. Een statusobject is verwijderd uit de cache in de volgende gevallen:

* Nadat het ophalen van een object van de status manager, genereert de actormethode van een een onverwerkte uitzondering.
* Een actor opnieuw wordt geactiveerd, na wordt gedeactiveerd of na een storing.
* De state-provider-pagina's staat op schijf. Dit gedrag is afhankelijk van de implementatie van de provider staat. De standaard state-provider voor de `Persisted` instelling, heeft dit probleem.

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

U kunt ook de status ophalen met behulp van een *TryGet* methode die wordt als een vermelding niet voor een sleutel bestaat genereren:

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

## <a name="save-state"></a>Status opslaan
De methoden voor het ophalen van status manager retourneert een verwijzing naar een object in het lokale geheugen. Wijzigen van dit object in het lokale geheugen alleen leidt niet tot deze blijvend worden opgeslagen. Wanneer een object is opgehaald uit de status manager en gewijzigd, moet u deze opnieuw in de status manager blijvend worden opgeslagen.

U kunt de status invoegen met behulp van een onvoorwaardelijk *ingesteld*, het equivalent van de `dictionary["key"] = value` syntaxis:

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

U kunt de status toevoegen met behulp van een *toevoegen* methode. Deze methode genereert `InvalidOperationException`(C#) of `IllegalStateException`(Java) als er wordt geprobeerd om toe te voegen een sleutel die al bestaat.

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

U kunt ook de status toevoegen met behulp van een *TryAdd* methode. Deze methode wordt niet genereren als er wordt geprobeerd om toe te voegen een sleutel die al bestaat.

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

Aan het einde van een actormethode slaat de status manager automatisch alle waarden die zijn toegevoegd of gewijzigd door een bewerking voor invoegen of bijwerken. Een 'opslaan' kunt opnemen met het persistent maken op schijf en replicatie, afhankelijk van de instellingen die worden gebruikt. Waarden die niet zijn gewijzigd, zijn niet opgeslagen of gerepliceerd. Als er geen waarden zijn gewijzigd, het opslaan bewerking doet niets. Als mislukt opslaat, wordt de gewijzigde status verwijderd en wordt de oorspronkelijke status opnieuw is geladen.

U kunt de status ook handmatig opslaan door het aanroepen van de `SaveStateAsync` methode voor de actor-base:

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
U kunt de status permanent verwijderen uit van een actor status manager door het aanroepen van de *verwijderen* methode. Deze methode genereert `KeyNotFoundException`(C#) of `NoSuchElementException`(Java) als er wordt geprobeerd om te verwijderen van een sleutel die niet bestaat.

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

U kunt ook de status permanent verwijderen met behulp van de *TryRemove* methode. Deze methode wordt niet genereren als er wordt geprobeerd om te verwijderen van een sleutel die niet bestaat.

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

Status die opgeslagen in Reliable Actors moet achtereenvolgens worden uitgevoerd voordat de naar schijf worden geschreven en gerepliceerd voor hoge beschikbaarheid. Meer informatie over [Actor typeserialisatie](service-fabric-reliable-actors-notes-on-actor-type-serialization.md).

Vervolgens meer informatie over [Actor diagnostische gegevens en bewaking van toepassingsprestaties](service-fabric-reliable-actors-diagnostics.md).
