---
title: Implementatie van functies in Azure Service Fabric-actoren | Microsoft Docs
description: Beschrijft hoe u uw eigen actor-service waarmee functies op serviceniveau dezelfde manier als wanneer wordt overgenomen van StatefulService schrijven.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/19/2018
ms.author: vturecek
ms.openlocfilehash: 6aff9e9599d31942f994f3cb4e5e9219f33dc7e1
ms.sourcegitcommit: 30221e77dd199ffe0f2e86f6e762df5a32cdbe5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39205517"
---
# <a name="implementing-service-level-features-in-your-actor-service"></a>Functies op serviceniveau implementeren in uw actor-service
Zoals beschreven in [service-lagen](service-fabric-reliable-actors-platform.md#service-layering), de actorservice zelf is een betrouwbare service.  U kunt uw eigen service die is afgeleid van schrijven `ActorService` en functies op serviceniveau implementeren de dezelfde manier als wanneer wordt overgenomen van StatefulService, zoals:

- Service maken en herstellen.
- Gedeelde functionaliteit voor alle actoren, bijvoorbeeld een Circuitonderbreker.
- Externe procedureaanroepen op de actorservice zelf en op elke afzonderlijke actor.

## <a name="using-the-actor-service"></a>Met behulp van de actor-service
Actor-exemplaren hebben toegang tot de actor-service waarin ze worden uitgevoerd. Actor-exemplaren kunnen programmatisch de servicecontext verkrijgen via de actorservice. De servicecontext heeft de partitie-ID, servicenaam, toepassingsnaam van de en andere Service Fabric-platform-specifieke informatie:

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

Net als alle betrouwbare Services, moet de actor-service zijn geregistreerd met een servicetype in de Service Fabric-runtime. Voor de actor-service kan uw actorexemplaren uitvoeren, moet ook uw actortype worden geregistreerd bij de actorservice. De `ActorRuntime`-registratiemethode doet dit voor actors. In het meest eenvoudige geval is, kunt u alleen uw actortype registreren en de actorservice met standaardinstellingen wordt impliciet worden gebruikt:

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

U kunt ook een lambda geleverd door de registratiemethode gebruiken om samen te stellen van de actor-service zelf. U kunt vervolgens de actorservice configureren en expliciet maken uw actorexemplaren, waar u afhankelijkheden aan uw actor via de constructor kunt invoeren:

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
De Actor-service wordt geïmplementeerd `IActorService` (C#) of `ActorService` (Java), die op zijn beurt implementeert `IService` (C#) of `Service` (Java). Dit is de interface die worden gebruikt door externe communicatie Reliable Services, waarmee externe procedureaanroepen op service-methoden. Het serviceniveau-methoden die op afstand via de service voor externe toegang kunnen worden aangeroepen en kunnen u bevat [opsommen](service-fabric-reliable-actors-enumerate.md) en [verwijderen](service-fabric-reliable-actors-delete-actors.md) actoren.


## <a name="custom-actor-service"></a>Aangepaste actor-service
U kunt uw eigen aangepaste actor-service die is afgeleid van registreren met behulp van de actor-registratie lambda, `ActorService` (C#) en `FabricActorService` (Java). In deze aangepaste actor-service, kunt u de functionaliteit van uw eigen serviceniveau implementeren door het schrijven van een serviceklasse die eigenschappen overneemt `ActorService` (C#) of `FabricActorService` (Java). Een aangepaste actor-service alle de actor-runtime-functionaliteit neemt over `ActorService` (C#) of `FabricActorService` (Java) en kan worden gebruikt om uw eigen servicemethoden te implementeren.

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

## <a name="implementing-actor-backup-and-restore"></a>Actor-up en herstel implementeren
Een aangepaste actor-service beschikbaar maakt een methode voor het back-up actor-gegevens door te profiteren van de listener voor externe toegang die al aanwezig in `ActorService`.  Zie voor een voorbeeld [back-up en herstel actoren](service-fabric-reliable-actors-backup-and-restore.md).

## <a name="actor-using-remoting-v2interfacecompatible-stack"></a>Met behulp van externe toegang V2(InterfaceCompatible) Stack actor
Remoting V2 (ook wel V2_1 InterfaceCompatible)-stack heeft alle functies van Remoting V2-stack van het feit dat het compatibel stack interface naar externe communicatie van V1-stack is maar niet compatibel met V2 en V1 is. Als u wilt u de upgrade van V1 naar V2_1 zonder enige impact op de beschikbaarheid van de service, volg de onderstaande [artikel](#actor-service-upgrade-to-remoting-v2interfacecompatible-stack-without-impacting-service-availability).

Volgende wijzigingen zijn vereist voor de externe toegang V2_1-Stack gebruiken.
 1. De volgende assembly-kenmerk toevoegen op de Actor-Interfaces.
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
   ```

 2. Build en Upgrade ActorService Actor-Client en projecten met V2-Stack aan de slag.

#### <a name="actor-service-upgrade-to-remoting-v2interfacecompatible-stack-without-impacting-service-availability"></a>Actor-Service upgraden naar externe communicatie V2(InterfaceCompatible) Stack zonder enige impact op de beschikbaarheid van de Service.
Deze wijziging is een upgrade van een 2-stap. Volg de stappen in dezelfde volgorde als die worden vermeld.

1.  De volgende assembly-kenmerk toevoegen op de Actor-Interfaces. Dit kenmerk wordt gestart twee listeners voor de ActorService, V1 (bestaand) en V2_1 Listener. ActorService bijwerken met deze wijziging.

  ```csharp
  [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
  ```

2. ActorClients upgraden na het voltooien van de bovenstaande upgrade.
Deze stap zorgt ervoor dat Actor-Proxy wordt gebruikt voor externe toegang V2_1 Stack.

3. Deze stap is optioneel. Wijzig het bovenstaande kenmerk als u wilt verwijderen van V1-Listener.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2_1,RemotingClientVersion = RemotingClientVersion.V2_1)]
    ```

## <a name="actor-using-remoting-v2-stack"></a>Actor Remoting V2-Stack gebruiken
Gebruikers kunnen nu Remoting V2-stack, waardoor beter is en waarmee u functies zoals aangepaste serialisatie gebruiken 2,8 nuget-pakket. Remoting V2 is niet compatibel met bestaande voor externe toegang-stack (we bellen nu deze als externe communicatie van V1-stack).

Volgende wijzigingen zijn vereist voor de Remoting V2-Stack gebruiken.
 1. De volgende assembly-kenmerk toevoegen op de Actor-Interfaces.
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
   ```

 2. Build en Upgrade ActorService Actor-Client en projecten met V2-Stack aan de slag.

#### <a name="actor-service-upgrade-to-remoting-v2-stack-without-impacting-service-availability"></a>Actor-Service upgraden naar Remoting V2 Stack zonder enige impact op de beschikbaarheid van de Service.
Deze wijziging is een upgrade van een 2-stap. Volg de stappen in dezelfde volgorde als die worden vermeld.

1.  De volgende assembly-kenmerk toevoegen op de Actor-Interfaces. Dit kenmerk wordt gestart twee listeners voor de ActorService, V1 (bestaand) en V2-Listener. ActorService bijwerken met deze wijziging.

  ```csharp
  [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V1|RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
  ```

2. ActorClients upgraden na het voltooien van de bovenstaande upgrade.
Deze stap zorgt ervoor dat Actor Proxy Remoting V2 Stack wordt gebruikt.

3. Deze stap is optioneel. Wijzig het bovenstaande kenmerk als u wilt verwijderen van V1-Listener.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListenerVersion = RemotingListenerVersion.V2,RemotingClientVersion = RemotingClientVersion.V2)]
    ```

## <a name="next-steps"></a>Volgende stappen
* [Statusbeheer actor](service-fabric-reliable-actors-state-management.md)
* [Actor-levenscyclus en garbagecollection verzameling](service-fabric-reliable-actors-lifecycle.md)
* [Actoren API-referentiedocumentatie](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [.NET-voorbeeldcode](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Java-voorbeeldcode](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
