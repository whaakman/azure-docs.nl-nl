---
title: Functies implementeren in Azure Service Fabric-actoren | Microsoft Docs
description: Beschrijft hoe u uw eigen actorservice die functies op serviceniveau op dezelfde manier als implementeert wanneer u StatefulService overnemen schrijven.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 57894770ad9d27430d5803c9a93ce6973355878a
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58662972"
---
# <a name="implement-service-level-features-in-your-actor-service"></a>Functies op serviceniveau implementeren in uw actor-service

Zoals beschreven in [service-lagen](service-fabric-reliable-actors-platform.md#service-layering), de actorservice zelf is een betrouwbare service. U kunt uw eigen service die is afgeleid van schrijven `ActorService`. Ook kunt u implementeren functies op serviceniveau in dezelfde manier als wanneer u een stateful service, zoals overnemen:

- Service maken en herstellen.
- Gedeelde functionaliteit voor alle actoren, bijvoorbeeld een Circuitonderbreker.
- Externe procedureaanroepen op de actorservice zelf en op elke afzonderlijke actor.

## <a name="use-the-actor-service"></a>Gebruik de actor-service

Actor-exemplaren hebben toegang tot de actor-service waarin ze worden uitgevoerd. Actor-exemplaren kunnen programmatisch de servicecontext verkrijgen via de actorservice. De servicecontext heeft de partitie-ID, servicenaam, toepassingsnaam van de en andere Azure Service Fabric-platform-specifieke informatie.

```csharp
Task MyActorMethod()
{
    Guid partitionId = this.ActorService.Context.PartitionId;
    string serviceTypeName = this.ActorService.Context.ServiceTypeName;
    Uri serviceInstanceName = this.ActorService.Context.ServiceName;
    string applicationInstanceName = this.ActorService.Context.CodePackageActivationContext.ApplicationName;
}
```
```Java
CompletableFuture<?> MyActorMethod()
{
    UUID partitionId = this.getActorService().getServiceContext().getPartitionId();
    String serviceTypeName = this.getActorService().getServiceContext().getServiceTypeName();
    URI serviceInstanceName = this.getActorService().getServiceContext().getServiceName();
    String applicationInstanceName = this.getActorService().getServiceContext().getCodePackageActivationContext().getApplicationName();
}
```

Net als alle betrouwbare Services, moet de actor-service zijn geregistreerd met een servicetype in de Service Fabric-runtime. Actor-service om uit te voeren van uw actor-exemplaren, worden uw actortype ook geregistreerd bij de actor-service. De `ActorRuntime`-registratiemethode doet dit voor actors. In het meest eenvoudige geval is, kunt u uw actortype registreren en de actorservice gebruikt vervolgens de standaardinstellingen.

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>().GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```

U kunt ook een lambda geleverd door de registratiemethode gebruiken om samen te stellen van de actor-service zelf. Vervolgens kunt u configureren de actor-service en uw actorexemplaren expliciet te maken. U kunt afhankelijkheden aan uw actor via de constructor invoeren.

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new ActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
static class Program
{
    private static void Main()
    {
      ActorRuntime.registerActorAsync(
              MyActor.class,
              (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
              timeout);

        Thread.sleep(Long.MAX_VALUE);
    }
}
```

## <a name="actor-service-methods"></a>Actor-service-methoden

De actor-service wordt geïmplementeerd `IActorService` (C#) of `ActorService` (Java), die op zijn beurt implementeert `IService` (C#) of `Service` (Java). Deze interface wordt gebruikt door externe communicatie Reliable Services, waarmee externe procedureaanroepen op service-methoden. Het bevat serviceniveau-methoden die extern kunnen worden aangeroepen via externe communicatie met service. U kunt deze [opsommen](service-fabric-reliable-actors-enumerate.md) en [verwijderen](service-fabric-reliable-actors-delete-actors.md) actoren.


## <a name="custom-actor-service"></a>Aangepaste actor-service

U kunt uw eigen aangepaste actor-service die is afgeleid van registreren met behulp van de actor-registratie lambda, `ActorService` (C#) en `FabricActorService` (Java). U kunt de functionaliteit van uw eigen serviceniveau vervolgens implementeren met het schrijven van een serviceklasse die eigenschappen overneemt `ActorService` (C#) of `FabricActorService` (Java). Een aangepaste actor-service alle de actor-runtime-functionaliteit neemt over `ActorService` (C#) of `FabricActorService` (Java). Het kan worden gebruikt om uw eigen servicemethoden te implementeren.

```csharp
class MyActorService : ActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }
}
```
```Java
class MyActorService extends FabricActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, BiFunction<FabricActorService, ActorId, ActorBase> newActor)
    {
         super(context, typeInfo, newActor);
    }
}
```

```csharp
static class Program
{
    private static void Main()
    {
        ActorRuntime.RegisterActorAsync<MyActor>(
            (context, actorType) => new MyActorService(context, actorType, () => new MyActor()))
            .GetAwaiter().GetResult();

        Thread.Sleep(Timeout.Infinite);
    }
}
```
```Java
public class Program
{
    public static void main(String[] args)
    {
        ActorRuntime.registerActorAsync(
                MyActor.class,
                (context, actorTypeInfo) -> new FabricActorService(context, actorTypeInfo),
                timeout);
        Thread.sleep(Long.MAX_VALUE);
    }
}
```

## <a name="implement-actor-backup-and-restore"></a>Implementeer actor back-up en herstel

Een aangepaste actor-service beschikbaar maakt een methode voor het back-up actor-gegevens door te profiteren van de listener voor externe toegang die al aanwezig in `ActorService`. Zie voor een voorbeeld [back-up en herstel actoren](service-fabric-reliable-actors-backup-and-restore.md).

## <a name="actor-that-uses-a-remoting-v2-interface-compatible-stack"></a>Actor die gebruikmaakt van een remoting V2-stack (interface-compatibel)

De remoting V2 (interface compatibel zijn, ook wel V2_1) stack heeft alle functies van de externe toegang-stack V2. De interface is compatibel met de externe communicatie van V1-stack, maar het is niet compatibel met V2 en V1. Als u wilt upgraden van V1 naar V2_1 met geen gevolgen voor de beschikbaarheid van de service, de stappen in de volgende sectie uit te voeren.

De volgende wijzigingen zijn vereist voor het gebruik van de externe toegang V2_1 stapel:

1. De volgende assembly-kenmerk toevoegen op de actor-interfaces.
  
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
   ```

2. Bouw en upgrade actor-service en client actorprojecten om te beginnen met behulp van de V2-stack.

### <a name="actor-service-upgrade-to-remoting-v2-interface-compatible-stack-without-affecting-service-availability"></a>Actor-service een upgrade uit naar remoting V2 (compatibel interface)-stack zonder die betrekking hebben op beschikbaarheid van de service

Deze wijziging is een upgrade van verificatie in twee stappen. Volg de stappen in deze reeks.

1. De volgende assembly-kenmerk toevoegen op de actor-interfaces. Dit kenmerk wordt gestart voor twee listeners voor de actorservice, V1 (bestaand) en de listener V2_1. Upgrade van de actorservice met deze wijziging.

   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
   ```

2. De actor-clients upgraden na het voltooien van de vorige upgrade.
   Deze stap zorgt ervoor dat de stack V2_1 voor externe toegang maakt gebruik van de actor-proxy.

3. Deze stap is optioneel. Wijzig het vorige kenmerk als u wilt verwijderen van de V1-listener.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
    ```

## <a name="actor-that-uses-the-remoting-v2-stack"></a>Actor die gebruikmaakt van de stack remoting V2

Gebruikers kunnen nu de remoting V2-stack, waardoor beter presteert en waarmee u functies zoals aangepaste serialisatie gebruiken met de versie 2.8 NuGet-pakket. Remoting V2 is niet compatibel met de bestaande externe toegang-stack (nu de V1 voor externe toegang-stack genoemd).

De volgende wijzigingen zijn vereist voor de remoting V2-stack gebruiken.

1. De volgende assembly-kenmerk toevoegen op de actor-interfaces.

   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
   ```

2. Bouw en werk de actor-service en client actorprojecten om te beginnen met behulp van de V2-stack.

### <a name="upgrade-the-actor-service-to-the-remoting-v2-stack-without-affecting-service-availability"></a>De actorservice upgraden naar de remoting V2-stack zonder die betrekking hebben op beschikbaarheid van de service

Deze wijziging is een upgrade van verificatie in twee stappen. Volg de stappen in deze reeks.

1. De volgende assembly-kenmerk toevoegen op de actor-interfaces. Dit kenmerk wordt gestart voor twee listeners voor de actorservice, V1 (bestaand) en de V2-listener. Upgrade van de actorservice met deze wijziging.

   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
   ```

2. De actor-clients upgraden na het voltooien van de vorige upgrade.
   Deze stap zorgt ervoor dat de actor-proxy wordt gebruikt voor de remoting V2-stack.

3. Deze stap is optioneel. Wijzig het vorige kenmerk als u wilt verwijderen van de V1-listener.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
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
