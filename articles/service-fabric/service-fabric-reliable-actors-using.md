---
title: Implementatie van functies in Azure Service Fabric actoren | Microsoft Docs
description: Beschrijft hoe u uw eigen actor-service die serviceniveau-functies dezelfde manier als implementeert wanneer wordt overgenomen van StatefulService schrijven.
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
ms.openlocfilehash: 41548c3395fa0c8f56e62cfcfb7338a2d53f040f
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="implementing-service-level-features-in-your-actor-service"></a>Serviceniveau-functies implementeren in uw actor-service
Zoals beschreven in [service gelaagdheid](service-fabric-reliable-actors-platform.md#service-layering), de actor-service zelf is een betrouwbare service.  U kunt uw eigen service die is afgeleid van schrijven `ActorService` en implementeren van serviceniveau-functies dezelfde manier als wanneer wordt overgenomen van StatefulService, zoals:

- Service back-up en herstel.
- Gedeelde functionaliteit voor alle actoren, bijvoorbeeld een Circuitonderbreker.
- Externe procedureaanroepen weer dat op de actor-service zelf en op elke afzonderlijke actor.

## <a name="using-the-actor-service"></a>Met behulp van de service actor
Actor-exemplaren hebben toegang tot de actor-service waarin ze worden uitgevoerd. Actor-exemplaren kunnen via de service actor programmatisch verkrijgen de servicecontext. De servicecontext heeft de partitie-ID, de servicenaam, de toepassingsnaam en andere Service Fabric-platform-specifieke informatie:

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

Net als alle Reliable Services, moet de actor-service zijn geregistreerd met een servicetype in de Service Fabric-runtime. Voor de service actor uw actor-exemplaren uitvoeren, moet uw actortype ook zijn geregistreerd met de actor-service. De `ActorRuntime`-registratiemethode doet dit voor actors. In het meest eenvoudige geval kunt u alleen uw actortype registreren en de service actor met standaardinstellingen wordt impliciet worden gebruikt:

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

U kunt ook kunt u een lambda geleverd door de registratiemethode om samen te stellen de actor-service zelf. U kunt vervolgens de actor-service te configureren en expliciet opstelt actor-exemplaren, waar u de afhankelijkheden aan uw actor via de constructor kunt invoeren:

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

## <a name="actor-service-methods"></a>De methoden van actor
De service Actor implements `IActorService` (C#) of `ActorService` (Java) die op zijn beurt implementeert `IService` (C#) of `Service` (Java). Dit is de interface die wordt gebruikt door externe toegang Reliable Services, waarmee externe procedureaanroepen weer dat op de service-methoden. Het serviceniveau-methoden die op afstand via de service voor externe toegang kunnen worden aangeroepen en kunnen u bevat [opsommen](service-fabric-reliable-actors-enumerate.md) en [verwijderen](service-fabric-reliable-actors-delete-actors.md) actoren.


## <a name="custom-actor-service"></a>Aangepaste actor-service
U kunt uw eigen aangepaste actor-service die is afgeleid van registreren met behulp van de registratie actor lambda `ActorService` (C#) en `FabricActorService` (Java). In deze service aangepaste actor u uw eigen serviceniveau-functionaliteit kunt implementeren met het schrijven van een serviceklasse die overneemt `ActorService` (C#) of `FabricActorService` (Java). Een aangepaste actor-service neemt over alle actor runtime functionaliteit van `ActorService` (C#) of `FabricActorService` (Java) en kan worden gebruikt voor het implementeren van uw eigen service-methoden.

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

## <a name="implementing-actor-backup-and-restore"></a>Implementatie van actor-back-up en herstel
Een aangepaste actor-service kan een methode voor het back-ups actor door gebruik te maken van de listener voor externe toegang die al aanwezig in blootgesteld `ActorService`.  Zie voor een voorbeeld [back-up en herstel actoren](service-fabric-reliable-actors-backup-and-restore.md).

## <a name="actor-using-remoting-v2-stack"></a>Met behulp van externe toegang V2 Stack actor
Gebruikers kunnen nu Remoting V2-stack, waardoor meer zodat is en waarmee u functies zoals aangepaste serialisatie gebruiken met 2,8 nuget-pakket. Externe communicatie V2 is niet compatibel met bestaande Remoting stack (we bellen nu het als Remoting V1-stack).

Volgende wijzigingen zijn vereist voor het gebruik van de Remoting V2-Stack.
 1. De volgende assembly-kenmerk toevoegen op Actor-Interfaces.
   ```csharp
   [assembly:FabricTransportActorRemotingProvider(RemotingListener = RemotingListener.V2Listener,RemotingClient = RemotingClient.V2Client)]
   ```

 2. Build en Upgrade ActorService Actor-Client en projecten aan de slag met V2-Stack.

### <a name="actor-service-upgrade-to-remoting-v2-stack-without-impacting-service-availability"></a>Actor-Service bijwerken naar Remoting V2 Stack zonder enige impact op de beschikbaarheid van de Service.
Deze wijziging is een upgrade van een 2-stap. Volg de stappen in dezelfde volgorde zoals vermeld.

1.  De volgende assembly-kenmerk toevoegen op Actor-Interfaces. Dit kenmerk twee listeners voor ActorService, V1 wordt gestart (bestaand) en V2-Listener. ActorService bijwerken met deze wijziging.

  ```csharp
  [assembly:FabricTransportActorRemotingProvider(RemotingListener = RemotingListener.CompatListener,RemotingClient = RemotingClient.V2Client)]
  ```

2. Upgrade ActorClients na voltooiing van de bovenstaande upgrade.
Deze stap zorgt ervoor dat actor-Proxy wordt gebruikt voor externe toegang V2-Stack.

3. Deze stap is optioneel. Wijzigen van het bovenstaande kenmerk V1-Listener te verwijderen.

    ```csharp
    [assembly:FabricTransportActorRemotingProvider(RemotingListener = RemotingListener.V2Listener,RemotingClient = RemotingClient.V2Client)]
    ```

## <a name="next-steps"></a>Volgende stappen
* [Statusbeheer actor](service-fabric-reliable-actors-state-management.md)
* [Acteur lifecycle en garbage collection](service-fabric-reliable-actors-lifecycle.md)
* [API-naslagdocumentatie actors](https://msdn.microsoft.com/library/azure/dn971626.aspx)
* [Voorbeeldcode voor .NET](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [Voorbeeldcode voor Java](http://github.com/Azure-Samples/service-fabric-java-getting-started)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-platform/actor-service.png
[2]: ./media/service-fabric-reliable-actors-platform/app-deployment-scripts.png
[3]: ./media/service-fabric-reliable-actors-platform/actor-partition-info.png
[4]: ./media/service-fabric-reliable-actors-platform/actor-replica-role.png
[5]: ./media/service-fabric-reliable-actors-introduction/distribution.png
