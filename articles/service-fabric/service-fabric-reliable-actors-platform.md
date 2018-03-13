---
title: Reliable Actors op Service Fabric | Microsoft Docs
description: Hierin wordt beschreven hoe Reliable Actors zijn gelaagd op Reliable Services en de functies van het Service Fabric-platform gebruiken.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: amanbha
ms.assetid: 45839a7f-0536-46f1-ae2b-8ba3556407fb
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: vturecek
ms.openlocfilehash: ee248cb656eeb54e259ff1adf45080a207b5a866
ms.sourcegitcommit: a0be2dc237d30b7f79914e8adfb85299571374ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2018
---
# <a name="how-reliable-actors-use-the-service-fabric-platform"></a>Hoe Reliable Actors het Service Fabric-platform gebruiken
In dit artikel wordt uitgelegd hoe Reliable Actors op de Azure Service Fabric-platform. Reliable Actors uitvoeren in een framework dat wordt gehost in een implementatie van een betrouwbare stateful service aangeroepen de *actor service*. De service actor bevat alle componenten die vereist zijn voor het beheren van de levenscyclus van en het bericht tijdens het verzenden van uw gebruiken:

* De Runtime Actor levensduur, garbagecollection, beheert en één thread toegang wordt afgedwongen.
* Een actor-service voor externe toegang-listener accepteert de aanroepen van externe toegang actoren en stuurt deze naar een dispatcher doorsturen naar het juiste actor-exemplaar.
* De Actor State-Provider verpakt statusproviders (zoals de betrouwbare verzamelingen state-provider) en biedt een adapter voor actor statusbeheer.

Deze onderdelen vormen samen het betrouwbare Actor-framework.

## <a name="service-layering"></a>Service gelaagdheid
Omdat de actor-service zelf een betrouwbare service alle de [toepassingsmodel](service-fabric-application-model.md), levensduur, [verpakking](service-fabric-package-apps.md), [implementatie](service-fabric-deploy-remove-applications.md), upgrade en concepten van betrouwbare schalen Services dezelfde manier actorservices van toepassing.

![Acteur service gelaagdheid][1]

Het voorgaande diagram toont de relatie tussen de Service Fabric-toepassingsframeworks en gebruikerscode. Blauw elementen het toepassingsframework Reliable Services vertegenwoordigen, oranje staat voor het framework betrouwbare Actor en groen voor gebruikerscode.

In Reliable Services uw service neemt over de `StatefulService` klasse. Deze klasse is afgeleid van `StatefulServiceBase` (of `StatelessService` voor stateless services). In Reliable Actors gebruikt u de actor-service. De actor-service is een andere implementatie van de `StatefulServiceBase` klasse die de actor-patroon waar uw actoren uitgevoerd implementeert. Omdat de actor-service zelf slechts een implementatie van is `StatefulServiceBase`, kunt u uw eigen service die is afgeleid van `ActorService` en implementeren van serviceniveau-functies dezelfde manier als wanneer wordt overgenomen `StatefulService`, zoals:

* Service back-up en herstel.
* Gedeelde functionaliteit voor alle actoren, bijvoorbeeld een Circuitonderbreker.
* Externe procedureaanroepen weer dat op de actor-service zelf en op elke afzonderlijke actor.

### <a name="using-the-actor-service"></a>Met behulp van de service actor
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

### <a name="actor-service-methods"></a>De methoden van actor
De service Actor implements `IActorService` (C#) of `ActorService` (Java) die op zijn beurt implementeert `IService` (C#) of `Service` (Java). Dit is de interface die wordt gebruikt door externe toegang Reliable Services, waarmee externe procedureaanroepen weer dat op de service-methoden. Het bevat serviceniveau-methoden die op afstand via de service voor externe toegang kunnen worden aangeroepen.

#### <a name="enumerating-actors"></a>Actoren opsommen
De actor-service kan een client opsommen metagegevens over de actoren waarmee de service wordt gehost. Omdat de actor-service een gepartitioneerde stateful service is, wordt de opsomming per partitie uitgevoerd. Omdat elke partitie veel actoren bevat mogelijk, wordt de opsomming geretourneerd als een set pagina's met zoekresultaten. De pagina's worden via herhaald totdat alle pagina's worden gelezen. Het volgende voorbeeld ziet u het maken van een lijst met alle actieve actoren in een partitie van een actor-service:

```csharp
IActorService actorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new List<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = await actorServiceProxy.GetActorsAsync(continuationToken, cancellationToken);

    activeActors.AddRange(page.Items.Where(x => x.IsActive));

    continuationToken = page.ContinuationToken;
}
while (continuationToken != null);
```

```Java
ActorService actorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), partitionKey);

ContinuationToken continuationToken = null;
List<ActorInformation> activeActors = new ArrayList<ActorInformation>();

do
{
    PagedResult<ActorInformation> page = actorServiceProxy.getActorsAsync(continuationToken);

    while(ActorInformation x: page.getItems())
    {
         if(x.isActive()){
              activeActors.add(x);
         }
    }

    continuationToken = page.getContinuationToken();
}
while (continuationToken != null);
```

#### <a name="deleting-actors"></a>Verwijderd actoren
De actor-service biedt ook een functie voor het verwijderen van actoren:

```csharp
ActorId actorToDelete = new ActorId(id);

IActorService myActorServiceProxy = ActorServiceProxy.Create(
    new Uri("fabric:/MyApp/MyService"), actorToDelete);

await myActorServiceProxy.DeleteActorAsync(actorToDelete, cancellationToken)
```
```Java
ActorId actorToDelete = new ActorId(id);

ActorService myActorServiceProxy = ActorServiceProxy.create(
    new URI("fabric:/MyApp/MyService"), actorToDelete);

myActorServiceProxy.deleteActorAsync(actorToDelete);
```

Zie voor meer informatie over het verwijderen van actors en hun status, de [actor lifecycle documentatie](service-fabric-reliable-actors-lifecycle.md).

### <a name="custom-actor-service"></a>Aangepaste actor-service
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

#### <a name="implementing-actor-backup-and-restore"></a>Implementatie van actor-back-up en herstel
 In het volgende voorbeeld wordt de aangepaste actor-service beschrijft een methode voor het back-ups actor door gebruik te maken van de listener voor externe toegang die al aanwezig in `ActorService`:

```csharp
public interface IMyActorService : IService
{
    Task BackupActorsAsync();
}

class MyActorService : ActorService, IMyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<ActorBase> newActor)
        : base(context, typeInfo, newActor)
    { }

    public Task BackupActorsAsync()
    {
        return this.BackupAsync(new BackupDescription(PerformBackupAsync));
    }

    private async Task<bool> PerformBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store the contents of backupInfo.Directory
           return true;
        }
        finally
        {
           Directory.Delete(backupInfo.Directory, recursive: true);
        }
    }
}
```
```Java
public interface MyActorService extends Service
{
    CompletableFuture<?> backupActorsAsync();
}

class MyActorServiceImpl extends ActorService implements MyActorService
{
    public MyActorService(StatefulServiceContext context, ActorTypeInformation typeInfo, Func<FabricActorService, ActorId, ActorBase> newActor)
    {
       super(context, typeInfo, newActor);
    }

    public CompletableFuture backupActorsAsync()
    {
        return this.backupAsync(new BackupDescription((backupInfo, cancellationToken) -> performBackupAsync(backupInfo, cancellationToken)));
    }

    private CompletableFuture<Boolean> performBackupAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
    {
        try
        {
           // store the contents of backupInfo.Directory
           return true;
        }
        finally
        {
           deleteDirectory(backupInfo.Directory)
        }
    }

    void deleteDirectory(File file) {
        File[] contents = file.listFiles();
        if (contents != null) {
            for (File f : contents) {
               deleteDirectory(f);
             }
        }
        file.delete();
    }
}
```


In dit voorbeeld `IMyActorService` is een contract voor externe toegang die implementeert `IService` (C#) en `Service` (Java) en vervolgens wordt geïmplementeerd door `MyActorService`. Door dit contract remoting, methoden toe te voegen op `IMyActorService` zijn nu ook beschikbaar voor een client door het maken van een proxy voor externe toegang via `ActorServiceProxy`:

```csharp
IMyActorService myActorServiceProxy = ActorServiceProxy.Create<IMyActorService>(
    new Uri("fabric:/MyApp/MyService"), ActorId.CreateRandom());

await myActorServiceProxy.BackupActorsAsync();
```
```Java
MyActorService myActorServiceProxy = ActorServiceProxy.create(MyActorService.class,
    new URI("fabric:/MyApp/MyService"), actorId);

myActorServiceProxy.backupActorsAsync();
```

## <a name="application-model"></a>Toepassingsmodel
Actorservices zijn Reliable Services, zodat de toepassingsmodel hetzelfde is. De actor framework build tools genereren echter enkele van de toepassingsbestanden model voor u.

### <a name="service-manifest"></a>Servicemanifest
De actor framework build-hulpprogramma's worden automatisch de inhoud van uw actor-service ServiceManifest.xml bestand genereren. Dit bestand bevat:

* Actortype-service. Naam van het type wordt gegenereerd op basis van uw actor projectnaam. Op basis van het kenmerk persistentie bij uw acteur, wordt de vlag HasPersistedState ook ingesteld dienovereenkomstig.
* Codepakket.
* Configuratiepakket.
* Bronnen en eindpunten.

### <a name="application-manifest"></a>Het toepassingsmanifest
Een standaard-servicedefinitie voor uw actor-service wordt automatisch gemaakt door de actor framework build-hulpprogramma's. De build-hulpprogramma's vullen de standaardeigenschappen voor de service:

* Replica set count wordt bepaald door het kenmerk persistentie bij uw actor. Telkens wanneer die de persistentie-kenmerk op uw actor is gewijzigd, de replica set in de servicedefinitie standaard beginwaarde dienovereenkomstig.
* Partitieschema en bereik zijn ingesteld op uniforme Int64 met het volledige bereik van de Int64-sleutel.

## <a name="service-fabric-partition-concepts-for-actors"></a>Concepten van service Fabric-partitie gebruiken
Actorservices zijn gepartitioneerde stateful services. Elke partitie van een actor-service bevat een set van actoren. Service-partities worden automatisch verdeeld over meerdere knooppunten in het Service Fabric. Actor-exemplaren worden als gevolg hiervan gedistribueerd.

![Acteur partitioneren en distribueren][5]

Reliable Services kunnen worden gemaakt met verschillende partitieschema's en partitie sleutelbereiken. De actor-service gebruikt het partitieschema Int64 alle belangrijke functies Int64 actoren toewijzen aan partities.

### <a name="actor-id"></a>Actor-ID
Elke actor dat gemaakt in de service heeft een unieke ID die is gekoppeld, vertegenwoordigd door de `ActorId` klasse. `ActorId` is een ondoorzichtige ID-waarde die voor gelijkmatige verdeling van actoren kan worden gebruikt tussen de servicepartities door het genereren van willekeurige id's:

```csharp
ActorProxy.Create<IMyActor>(ActorId.CreateRandom());
```
```Java
ActorProxyBase.create<MyActor>(MyActor.class, ActorId.newId());
```


Elke `ActorId` wordt opgedeeld naar een gegevenstype Int64. Dit is de reden waarom de actor-service een partitieschema Int64 met het volledige bereik van de Int64-sleutel gebruiken moet. Aangepaste ID-waarden kunnen echter worden gebruikt voor een `ActorID`, met inbegrip van GUID's / UUID's, tekenreeksen en Int64s.

```csharp
ActorProxy.Create<IMyActor>(new ActorId(Guid.NewGuid()));
ActorProxy.Create<IMyActor>(new ActorId("myActorId"));
ActorProxy.Create<IMyActor>(new ActorId(1234));
```
```Java
ActorProxyBase.create(MyActor.class, new ActorId(UUID.randomUUID()));
ActorProxyBase.create(MyActor.class, new ActorId("myActorId"));
ActorProxyBase.create(MyActor.class, new ActorId(1234));
```

Wanneer u van GUID's / UUID's en tekenreeksen gebruikmaakt, worden de waarden worden opgedeeld in een Int64. Echter, wanneer u expliciet bieden een gegevenstype Int64 naar een `ActorId`, de Int64 wordt rechtstreeks verwijzen naar een partitie zonder verdere hashing. U kunt deze techniek om te bepalen welke partitie in de actoren worden geplaatst.

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
