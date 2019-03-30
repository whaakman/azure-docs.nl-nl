---
title: Herbetreedbaarheid in Azure Service Fabric-actoren | Microsoft Docs
description: Inleiding tot herbetreedbaarheid voor Service Fabric Reliable Actors.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: amanbha
ms.assetid: be23464a-0eea-4eca-ae5a-2e1b650d365e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: 46682787bac2d60d188384a4078ca2fa1f46ae7a
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58669024"
---
# <a name="reliable-actors-reentrancy"></a>Herbetreedbaarheid van betrouwbare actoren
De runtime Reliable Actors kan standaard logische aanroep op basis van een context herbetreedbaarheid van. Hierdoor via actoren op inspringende als ze zich in dezelfde aanroep context keten. Bijvoorbeeld, verzendt een Actor een bericht naar Actor B, die een bericht naar Actor C. verzendt Als onderdeel van de berichtverwerking als Actor C Actor A aanroept, het bericht is inspringende, zodat deze kunnen worden. Andere berichten die deel van een andere context uitmaken wordt op een van de Actor wordt geblokkeerd totdat het verwerken is voltooid.

Er zijn twee opties beschikbaar voor de actor herbetreedbaarheid gedefinieerd in de `ActorReentrancyMode` enum:

* `LogicalCallContext` (standaardinstelling)
* `Disallowed` -schakelt herbetreedbaarheid

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
Herbetreedbaarheid van kan worden geconfigureerd een `ActorService`van instellingen tijdens de registratie. De instelling is van toepassing op alle actorexemplaren in de actor-service hebt gemaakt.

Het volgende voorbeeld ziet u een actorservice die de herbetreedbaarheid modus instellen op `ActorReentrancyMode.Disallowed`. In dit geval, als een actor verzendt een inspringende bericht naar een andere actor, een uitzondering van het type `FabricException` gegenereerd.

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
* Meer informatie over herbetreedbaarheid in de [Actor-API-referentiedocumentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
