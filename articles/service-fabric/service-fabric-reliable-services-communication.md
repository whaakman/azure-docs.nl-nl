---
title: Overzicht van de communicatie betrouwbare Services | Microsoft Docs
description: Overzicht van de communicatiemodel Reliable Services, waaronder openen listeners van services, het omzetten van eindpunten en communicatie tussen services.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: BharatNarasimman
ms.assetid: 36217988-420e-409d-b0a4-e0e875b6eac8
ms.service: service-fabric
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 04/07/2017
ms.author: vturecek
ms.openlocfilehash: b418904f50b772c12bfcdbb95beb9312c8b9fb00
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-the-reliable-services-communication-apis"></a>Het gebruik van de communicatie met Reliable Services API 's
Azure Service Fabric als platform is volledig agnostisch over de communicatie tussen services. Alle protocollen en stacks zijn aanvaardbaar van UDP naar HTTP. Het is aan de ontwikkelaar van de service om te kiezen hoe services moeten communiceren. Het framework Reliable Services biedt ingebouwde communicatie stacks, alsmede de API's die u gebruiken kunt voor het bouwen van uw aangepaste communicatie-onderdelen.

## <a name="set-up-service-communication"></a>Servicecommunicatie instellen
De API voor betrouwbare Services maakt gebruik van een eenvoudige interface voor servicecommunicatie. U opent een eindpunt voor uw service door deze interface eenvoudig te implementeren:

```csharp

public interface ICommunicationListener
{
    Task<string> OpenAsync(CancellationToken cancellationToken);

    Task CloseAsync(CancellationToken cancellationToken);

    void Abort();
}

```

```java
public interface CommunicationListener {
    CompletableFuture<String> openAsync(CancellationToken cancellationToken);

    CompletableFuture<?> closeAsync(CancellationToken cancellationToken);

    void abort();
}
```

Vervolgens kunt u uw communicatie-listener-implementatie toevoegen door deze terug te geven in een onderdrukking van de methode op basis van een service-klasse.

Voor stateless services:

```csharp
class MyStatelessService : StatelessService
{
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        ...
    }
    ...
}
```
```java
public class MyStatelessService extends StatelessService {

    @Override
    protected List<ServiceInstanceListener> createServiceInstanceListeners() {
        ...
    }
    ...
}
```

Voor stateful services:

> [!NOTE]
> Stateful betrouwbare services worden nog niet ondersteund in Java.
>
>

```csharp
class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

In beide gevallen moet u een verzameling van listeners retourneren. Hierdoor kan uw service luisteren op meerdere eindpunten, met mogelijk verschillende protocollen, met behulp van meerdere listeners. Bijvoorbeeld, wellicht u een HTTP-listener en een afzonderlijke WebSocket-listener. Elke listener haalt u een naam en de resulterende verzameling *naam: adres* paren worden weergegeven als een JSON-object als een client de luisterende adressen voor een service-exemplaar of een partitie vraagt.

In een stateless service retourneert de onderdrukking een verzameling ServiceInstanceListeners. Een `ServiceInstanceListener` bevat een functie voor het maken een `ICommunicationListener(C#) / CommunicationListener(Java)` en hieraan een naam. De onderdrukking retourneert voor stateful services, een verzameling van ServiceReplicaListeners. Dit is enigszins afwijken van het bijbehorende equivalent staatloze omdat een `ServiceReplicaListener` heeft een optie om een `ICommunicationListener` op secundaire replica's. Niet alleen kunt u meerdere communicatielisteners gebruiken in een service, maar u kunt ook opgeven welke listeners accepteren van aanvragen op secundaire replica's en welke luistert alleen op primaire replica's.

Bijvoorbeeld, u kunt een ServiceRemotingListener RPC-aanroepen krijgt alleen op primaire replica's hebben en een tweede, aangepaste listener lezen waarmee aanvragen op de secundaire replica's via HTTP:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new[]
    {
        new ServiceReplicaListener(context =>
            new MyCustomHttpListener(context),
            "HTTPReadonlyEndpoint",
            true),

        new ServiceReplicaListener(context =>
            this.CreateServiceRemotingListener(context),
            "rpcPrimaryEndpoint",
            false)
    };
}
```

> [!NOTE]
> Bij het maken van meerdere listeners voor een service wordt elke listener **moet** een unieke naam.
>
>

Ten slotte beschrijven de eindpunten die vereist zijn voor de service in de [servicemanifest](service-fabric-application-model.md) onder de sectie voor eindpunten.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

De listener communicatie toegang tot de eindpunt-resources toegewezen aan in de `CodePackageActivationContext` in de `ServiceContext`. De listener kunt vervolgens luisteren naar aanvragen wanneer deze wordt geopend starten.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> Eindpunt bronnen voor het hele servicepakket gelden en ze worden toegewezen door de Service Fabric wanneer het servicepakket is geactiveerd. Meerdere service replica's die worden gehost in de dezelfde ServiceHost kunnen delen dezelfde poort. Dit betekent dat de communicatie-listener delen van de poort moet ondersteunen. De aanbevolen manier is voor de communicatie-listener voor de partitie-ID en replica-exemplaar-ID gebruiken bij het genereren van het adres luisteren.
>
>

### <a name="service-address-registration"></a>Registratie van de service-adres
Een systeemservice aangeroepen de *Naming Service* wordt uitgevoerd op de Service Fabric-clusters. De Naming Service is een registrar voor services en de adressen die op elk exemplaar of de replica van de service luistert. Wanneer de `OpenAsync(C#) / openAsync(Java)` methode van een `ICommunicationListener(C#) / CommunicationListener(Java)` is voltooid, wordt de return-waarde opgehaald geregistreerd in de Naming Service. Deze waarde die wordt gepubliceerd in de Naming Service is een tekenreeks waarvan de waarde van alles op alle zijn kan. Deze waarde is wat clients zien wanneer ze voor een adres voor de service uit de Naming Service vragen.

```csharp
public Task<string> OpenAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.CodePackageActivationContext.GetEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.Port;

    this.listeningAddress = string.Format(
                CultureInfo.InvariantCulture,
                "http://+:{0}/",
                port);

    this.publishAddress = this.listeningAddress.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);

    this.webApp = WebApp.Start(this.listeningAddress, appBuilder => this.startup.Invoke(appBuilder));

    // the string returned here will be published in the Naming Service.
    return Task.FromResult(this.publishAddress);
}
```
```java
public CompletableFuture<String> openAsync(CancellationToken cancellationToken)
{
    EndpointResourceDescription serviceEndpoint = serviceContext.getCodePackageActivationContext.getEndpoint("ServiceEndpoint");
    int port = serviceEndpoint.getPort();

    this.publishAddress = String.format("http://%s:%d/", FabricRuntime.getNodeContext().getIpAddressOrFQDN(), port);

    this.webApp = new WebApp(port);
    this.webApp.start();

    /* the string returned here will be published in the Naming Service.
     */
    return CompletableFuture.completedFuture(this.publishAddress);
}
```

Service Fabric bevat API's waarmee clients en andere services kunnen vervolgens vraagt u voor dit adres met de servicenaam. Dit is belangrijk omdat het serviceadres niet statisch is. Services worden in het cluster voor netwerktaakverdeling en beschikbaarheid doeleinden van resource verplaatst. Dit is het mechanisme dat clientcomputers kunnen omzetten van het luisterende adres voor een service.

> [!NOTE]
> Zie voor een volledig overzicht van het schrijven van een communicatie-listener [Service Fabric-Web-API-services met OWIN zelf hosting](service-fabric-reliable-services-communication-webapi.md) voor C#, dat u uw eigen HTTP-server-implementatie voor Java schrijven kunt, Zie EchoServer toepassing voorbeeld op https://github.com/Azure-Samples/service-fabric-java-getting-started.
>
>

## <a name="communicating-with-a-service"></a>Communicatie met een service
De betrouwbare Services API biedt de volgende bibliotheken voor het schrijven van clients die met de services communiceren.

### <a name="service-endpoint-resolution"></a>Service-eindpunt resolutie
De eerste stap voor communicatie met een service is een eindpuntadres van de partitie of het exemplaar van de service die u wilt praten omzetten. De `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` hulpprogrammaklasse is een eenvoudige primitieve die helpt clients bij het bepalen van het eindpunt van een service tijdens runtime. In Service Fabric-terminologie, het proces voor het bepalen van het eindpunt van een service wordt aangeduid als de *service-eindpunt resolutie*.

Als u wilt verbinding maken met services binnen een cluster, kunnen ServicePartitionResolver worden gemaakt met de standaardinstellingen. Dit is het aanbevolen gebruik voor de meeste situaties:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

Als u wilt verbinding maken met services in een ander cluster, kan een ServicePartitionResolver worden gemaakt met een reeks cluster gateway eindpunten. Houd er rekening mee dat gateway eindpunten alleen verschillende eindpunten zijn voor de verbinding met hetzelfde cluster. Bijvoorbeeld:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

U kunt ook `ServicePartitionResolver` kan een functie worden opgegeven voor het maken van een `FabricClient` intern gebruik:

```csharp
public delegate FabricClient CreateFabricClientDelegate();
```
```java
public FabricServicePartitionResolver(CreateFabricClient createFabricClient) {
...
}

public interface CreateFabricClient {
    public FabricClient getFabricClient();
}
```

`FabricClient`het object dat wordt gebruikt om te communiceren met het Service Fabric-cluster voor verschillende bewerkingen op het cluster is. Dit is handig als u meer controle wilt over de interactie van een service-omzetter partitie met het cluster. `FabricClient`voert caching intern en is meestal duur voor het maken, dus is het belangrijk om opnieuw te `FabricClient` exemplaren zo veel mogelijk.

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

Een methode resolve wordt vervolgens gebruikt voor het ophalen van het adres van een service of de partitie van een service voor gepartitioneerde services.

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();

ResolvedServicePartition partition =
    await resolver.ResolveAsync(new Uri("fabric:/MyApp/MyService"), new ServicePartitionKey(), cancellationToken);
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();

CompletableFuture<ResolvedServicePartition> partition =
    resolver.resolveAsync(new URI("fabric:/MyApp/MyService"), new ServicePartitionKey());
```

Een serviceadres kan worden omgezet eenvoudig met een ServicePartitionResolver, maar meer werk is vereist om te controleren of dat het omgezette adres correct kan worden gebruikt. De client moet detecteren of de verbindingspoging is mislukt vanwege een tijdelijke fout en kan opnieuw worden uitgevoerd (bijv, service verplaatst of is tijdelijk niet beschikbaar), of een permanente fout (bijv, service is verwijderd of de aangevraagde resource bestaat niet meer). Service-exemplaren of replica's kunnen verplaatsen van het knooppunt naar knooppunt op elk gewenst moment om verschillende redenen. Het adres van de omgezet via ServicePartitionResolver mogelijk verlopen op het moment dat de clientcode verbinding probeert te maken. In dat geval opnieuw de client moet opnieuw omzetten van het adres. De vorige bieden `ResolvedServicePartition` geeft aan dat de omzetter moet opnieuw wilt proberen in plaats van gewoon ophalen een adres in de cache.

Normaal gesproken moet de clientcode werken niet met de ServicePartitionResolver rechtstreeks. Het is gemaakt en doorgegeven aan communicatie client fabrieken in de betrouwbare Services-API. De factory's gebruiken de resolver intern voor het genereren van een clientobject dat kan worden gebruikt om te communiceren met de services.

### <a name="communication-clients-and-factories"></a>Communicatie-clients en fabrieken
De communicatie factory bibliotheek implementeert een typische opnieuw patroon voor afhandeling van fouten die gemakkelijker is bezig met opnieuw verbindingen met opgelost service-eindpunten. De factory-bibliotheek biedt een mechanisme voor het opnieuw proberen terwijl u de fout-handlers opgeven.

`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`Hiermee definieert u de basis-interface geïmplementeerd door een client-factory van communicatie waarmee clients die met een Service Fabric-service communiceren kunnen wordt geproduceerd. De uitvoering van de CommunicationClientFactory is afhankelijk van de communicatiestack gebruikt door de Service Fabric-service waar de client wil communiceren. De betrouwbare Services API biedt een `CommunicationClientFactoryBase<TCommunicationClient>`. Dit biedt een basisimplementatie van de interface CommunicationClientFactory en taken die gemeenschappelijk voor alle communicatie stapels zijn worden uitgevoerd. (Deze taken omvatten het gebruik van een ServicePartitionResolver om te bepalen van het service-eindpunt). Clients implementeren meestal de abstracte klasse CommunicationClientFactoryBase voor het afhandelen van logica die specifiek is voor de communicatiestack.

De client communicatie wordt alleen een adres ontvangt en gebruikt deze verbinding maken met een service. De client kan elk protocol dat deze wil gebruiken.

```csharp
class MyCommunicationClient : ICommunicationClient
{
    public ResolvedServiceEndpoint Endpoint { get; set; }

    public string ListenerName { get; set; }

    public ResolvedServicePartition ResolvedServicePartition { get; set; }
}
```
```java
public class MyCommunicationClient implements CommunicationClient {

    private ResolvedServicePartition resolvedServicePartition;
    private String listenerName;
    private ResolvedServiceEndpoint endPoint;

    /*
     * Getters and Setters
     */
}
```

Clientfabriek van de is voornamelijk verantwoordelijk voor het maken van clients voor communicatie. Voor clients die een permanente verbinding, zoals een HTTP-client niet onderhouden moet de factory alleen maken en de client terug. Andere protocollen die een permanente verbinding, zoals sommige binaire protocollen onderhoudt worden ook gevalideerd door de factory om te bepalen of de verbinding moet opnieuw worden gemaakt.  

```csharp
public class MyCommunicationClientFactory : CommunicationClientFactoryBase<MyCommunicationClient>
{
    protected override void AbortClient(MyCommunicationClient client)
    {
    }

    protected override Task<MyCommunicationClient> CreateClientAsync(string endpoint, CancellationToken cancellationToken)
    {
    }

    protected override bool ValidateClient(MyCommunicationClient clientChannel)
    {
    }

    protected override bool ValidateClient(string endpoint, MyCommunicationClient client)
    {
    }
}
```
```java
public class MyCommunicationClientFactory extends CommunicationClientFactoryBase<MyCommunicationClient> {

    @Override
    protected boolean validateClient(MyCommunicationClient clientChannel) {
    }

    @Override
    protected boolean validateClient(String endpoint, MyCommunicationClient client) {
    }

    @Override
    protected CompletableFuture<MyCommunicationClient> createClientAsync(String endpoint) {
    }

    @Override
    protected void abortClient(MyCommunicationClient client) {
    }
}
```

Ten slotte is een uitzonderings-handler verantwoordelijk voor het bepalen welke actie moet worden uitgevoerd wanneer een uitzondering optreedt. Uitzonderingen worden onderverdeeld in **herstelbare** en **niet-herstelbare**.

* **Niet-herstelbare** uitzonderingen ophalen gewoon terug naar de aanroeper opnieuw.
* **herstelbare** uitzonderingen kunnen verder worden onderverdeeld in **tijdelijke** en **tijdelijke**.
  * **Tijdelijke** uitzonderingen zijn die opnieuw kunnen eenvoudig worden verzonden zonder dat het adres van het service-eindpunt opnieuw op te lossen. Hierbij wordt tijdelijke netwerkproblemen of service-foutberichten dan degene die wijzen op dat het adres van het service-eindpunt bestaat niet.
  * **Niet-tijdelijke** uitzonderingen zijn die het adres van het service-eindpunt worden opnieuw opgelost vereisen. Het gaat hierbij om uitzonderingen die wijzen op dat het service-eindpunt kan niet worden bereikt, is die aangeeft van de service verplaatst naar een ander knooppunt.

De `TryHandleException` maakt een beslissing over een bepaalde uitzondering. Als het **niet weet** welke beslissingen nemen over een uitzondering moet deze uitvoer retourneren **false**. Als het **weet** welke beslissen, moet het resultaat dienovereenkomstig ingesteld en geretourneerd **true**.

```csharp
class MyExceptionHandler : IExceptionHandler
{
    public bool TryHandleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings, out ExceptionHandlingResult result)
    {
        // if exceptionInformation.Exception is known and is transient (can be retried without re-resolving)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, true, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;


        // if exceptionInformation.Exception is known and is not transient (indicates a new service endpoint address must be resolved)
        result = new ExceptionHandlingRetryResult(exceptionInformation.Exception, false, retrySettings, retrySettings.DefaultMaxRetryCount);
        return true;

        // if exceptionInformation.Exception is unknown (let the next IExceptionHandler attempt to handle it)
        result = null;
        return false;
    }
}
```
```java
public class MyExceptionHandler implements ExceptionHandler {

    @Override
    public ExceptionHandlingResult handleException(ExceptionInformation exceptionInformation, OperationRetrySettings retrySettings) {        

        /* if exceptionInformation.getException() is known and is transient (can be retried without re-resolving)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), true, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;


        /* if exceptionInformation.getException() is known and is not transient (indicates a new service endpoint address must be resolved)
         */
        result = new ExceptionHandlingRetryResult(exceptionInformation.getException(), false, retrySettings, retrySettings.getDefaultMaxRetryCount());
        return true;

        /* if exceptionInformation.getException() is unknown (let the next ExceptionHandler attempt to handle it)
         */
        result = null;
        return false;

    }
}
```
### <a name="putting-it-all-together"></a>Kort samengevat
Met een `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, en `IExceptionHandler(C#) / ExceptionHandler(Java)` gebaseerd op een communicatieprotocol een `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` helemaal verpakt en biedt de afhandeling van fouten en service partitie adres resolutie lus om deze onderdelen.

```csharp
private MyCommunicationClientFactory myCommunicationClientFactory;
private Uri myServiceUri;

var myServicePartitionClient = new ServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

var result = await myServicePartitionClient.InvokeWithRetryAsync(async (client) =>
   {
      // Communicate with the service using the client.
   },
   CancellationToken.None);

```
```java
private MyCommunicationClientFactory myCommunicationClientFactory;
private URI myServiceUri;

FabricServicePartitionClient myServicePartitionClient = new FabricServicePartitionClient<MyCommunicationClient>(
    this.myCommunicationClientFactory,
    this.myServiceUri,
    myPartitionKey);

CompletableFuture<?> result = myServicePartitionClient.invokeWithRetryAsync(client -> {
      /* Communicate with the service using the client.
       */
   });

```

## <a name="next-steps"></a>Volgende stappen
* Een voorbeeld bekijken van HTTP-communicatie tussen services in een [C#-voorbeeldproject op GitHUb](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/WordCount) of [Java-voorbeeldproject op GitHUb](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Services/WatchDog).
* [Externe procedureaanroepen weer dat met Reliable Services voor externe toegang](service-fabric-reliable-services-communication-remoting.md)
* [Web-API die gebruikmaakt van OWIN in Reliable Services](service-fabric-reliable-services-communication-webapi.md)
* [WCF-communicatie met behulp van Reliable Services](service-fabric-reliable-services-communication-wcf.md)
