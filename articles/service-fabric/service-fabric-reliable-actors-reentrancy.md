---
title: Herbetreedbaarheid in Azure Service Fabric-actoren | Microsoft Docs
description: Inleiding tot herbetreedbaarheid voor Service Fabric Reliable Actors.
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
ms.openlocfilehash: c7a4066a949ad6e66c45dff67f1e80801f2fa4cd
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44055257"
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
