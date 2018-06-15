---
title: Herintreding in Azure microservices actor gebaseerde | Microsoft Docs
description: Inleiding tot herintreding voor Service Fabric Reliable Actors
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: be23464a-0eea-4eca-ae5a-2e1b650d365e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 40f52cb399f2d7391657ce4356a0c30921d46e5f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
ms.locfileid: "34207091"
---
# <a name="reliable-actors-reentrancy"></a>Betrouwbare actoren herintreding
De runtime Reliable Actors kunt standaard u logische aanroep context gebaseerde herintreding. Hierdoor actoren worden inspringende als ze in dezelfde aanroep context keten. Bijvoorbeeld, verzendt Actor A een bericht naar Actor-B, die een bericht naar Actor C. verzendt Als onderdeel van de berichtverwerking als Actor C Actor A, roept is het bericht inspringende, dus wordt toegestaan. Alle berichten die deel van de aanroepcontext van een andere uitmaken wordt geblokkeerd op Actor A totdat het verwerken is voltooid.

Er zijn twee opties beschikbaar voor actor herintreding gedefinieerd in de `ActorReentrancyMode` enum:

* `LogicalCallContext` (standaardinstelling)
* `Disallowed` -herintreding uitgeschakeld

```csharp
public enum ActorReentrancyMode
{
    LogicalCallContext = 1,
    Disallowed = 2
}
```
```Java
public enum ActorReentrancyMode
{
    LogicalCallContext(1),
    Disallowed(2)
}
```
Herintreding kan worden geconfigureerd in een `ActorService`van instellingen tijdens de registratie. De instelling is van toepassing op alle actor exemplaren gemaakt in de actor-service.

Het volgende voorbeeld ziet u een actor-service die de herintreding modus instellen op `ActorReentrancyMode.Disallowed`. In dit geval, als een actor verzendt een inspringende bericht naar een andere acteur, een uitzondering van het type `FabricException` gegenereerd.

```csharp
static class Program
{
    static void Main()
    {
        try
        {
            ActorRuntime.RegisterActorAsync<Actor1>(
                (context, actorType) => new ActorService(
                    context,
                    actorType, () => new Actor1(),
                    settings: new ActorServiceSettings()
                    {
                        ActorConcurrencySettings = new ActorConcurrencySettings()
                        {
                            ReentrancyMode = ActorReentrancyMode.Disallowed
                        }
                    }))
                .GetAwaiter().GetResult();

            Thread.Sleep(Timeout.Infinite);
        }
        catch (Exception e)
        {
            ActorEventSource.Current.ActorHostInitializationFailed(e.ToString());
            throw;
        }
    }
}
```
```Java
static class Program
{
    static void Main()
    {
        try
        {
            ActorConcurrencySettings actorConcurrencySettings = new ActorConcurrencySettings();
            actorConcurrencySettings.setReentrancyMode(ActorReentrancyMode.Disallowed);

            ActorServiceSettings actorServiceSettings = new ActorServiceSettings();
            actorServiceSettings.setActorConcurrencySettings(actorConcurrencySettings);

            ActorRuntime.registerActorAsync(
                Actor1.getClass(),
                (context, actorType) -> new FabricActorService(
                    context,
                    actorType, () -> new Actor1(),
                    null,
                    stateProvider,
                    actorServiceSettings, timeout);

            Thread.sleep(Long.MAX_VALUE);
        }
        catch (Exception e)
        {
            throw e;
        }
    }
}
```


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over herintreding in de [Actor-API-naslagdocumentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
