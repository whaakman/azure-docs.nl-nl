---
title: Overzicht van Reliable Services communicatie | Microsoft Docs
description: Overzicht van Reliable Services communicatiemodel, met inbegrip van openen listeners op services, het oplossen van eindpunten en communicatie tussen services.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: BharatNarasimman
ms.assetid: 36217988-420e-409d-b0a4-e0e875b6eac8
ms.service: service-fabric
ms.devlang: multiple
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: 15b45cadc69830827952d87ffc2315b06b07b02c
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58663397"
---
# <a name="how-to-use-the-reliable-services-communication-apis"></a>Het gebruik van de communicatie Reliable Services API 's
Azure Service Fabric als platform is volledig agnostische over de communicatie tussen services. Alle protocollen en stacks zijn aanvaardbaar van UDP naar HTTP. Het is aan de ontwikkelaar van de service om te kiezen hoe services moeten communiceren. Het framework van de toepassing betrouwbare Services bevat ingebouwde communicatie-stacks en API's die u gebruiken kunt om te maken van de Communicatieonderdelen van uw aangepaste.

## <a name="set-up-service-communication"></a>Stelt u de servicecommunicatie
De Reliable Services-API maakt gebruik van een eenvoudige interface voor servicecommunicatie. Als u wilt openen een eindpunt voor uw service, moet u gewoon deze interface implementeren:

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

Vervolgens kunt u uw communicatie-listener-toepassing toevoegen door deze in een onderdrukking van de methode op basis van een service-klasse worden geretourneerd.

Voor stateless services:

```csharp
public class MyStatelessService : StatelessService
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

```java
    @Override
    protected List<ServiceReplicaListener> createServiceReplicaListeners() {
        ...
    }
    ...
```

```csharp
public class MyStatefulService : StatefulService
{
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
        ...
    }
    ...
}
```

In beide gevallen moet u een verzameling van listeners geretourneerd. Hiermee wordt uw service om te luisteren op meerdere eindpunten, mogelijk met behulp van verschillende protocollen, met behulp van meerdere listeners. Bijvoorbeeld, u mogelijk een HTTP-listener en een afzonderlijke WebSocket-listener. Elke listener haalt een naam en de resulterende verzameling van *naam: adres* paren worden weergegeven als een JSON-object wanneer een client een aanvraag de luisterende adressen voor een service-exemplaar of een partitie.

In een staatloze service retourneert de onderdrukking een verzameling van ServiceInstanceListeners. Een `ServiceInstanceListener` bevat een functie maken een `ICommunicationListener(C#) / CommunicationListener(Java)` en geeft deze een naam. Voor stateful services retourneert de onderdrukking een verzameling van ServiceReplicaListeners. Dit is enigszins afwijken van de stateless equivalent, omdat een `ServiceReplicaListener` heeft een optie om een `ICommunicationListener` op secundaire replica's. Niet alleen kunt u meerdere communicatielisteners gebruiken in een service, maar u kunt ook opgeven welke listeners accepteren van aanvragen voor secundaire replica's en welke alleen op de primaire replica's luistert.

Bijvoorbeeld, u kunt een ServiceRemotingListener RPC-aanroepen wordt alleen op primaire replica's hebben en een tweede, aangepaste listener die met lezen op secundaire replica's via HTTP-aanvragen:

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
> Bij het maken van meerdere listeners voor een service, elke listener **moet** een unieke naam.
>
>

Ten slotte beschrijven de eindpunten die vereist zijn voor de service in de [servicemanifest](service-fabric-application-and-service-manifests.md) onder de sectie op eindpunten.

```xml
<Resources>
    <Endpoints>
      <Endpoint Name="WebServiceEndpoint" Protocol="http" Port="80" />
      <Endpoint Name="OtherServiceEndpoint" Protocol="tcp" Port="8505" />
    <Endpoints>
</Resources>

```

De communicatielistener toegang tot de eindpunt-resources toegewezen aan deze vanuit de `CodePackageActivationContext` in de `ServiceContext`. De listener kunt start luistert naar aanvragen wanneer deze wordt geopend.

```csharp
var codePackageActivationContext = serviceContext.CodePackageActivationContext;
var port = codePackageActivationContext.GetEndpoint("ServiceEndpoint").Port;

```
```java
CodePackageActivationContext codePackageActivationContext = serviceContext.getCodePackageActivationContext();
int port = codePackageActivationContext.getEndpoint("ServiceEndpoint").getPort();

```

> [!NOTE]
> Eindpunt resources gelden voor de hele service-pakket en ze worden toegewezen door de Service Fabric het servicepakket is geactiveerd. Meerdere service-replica's die worden gehost in de dezelfde ServiceHost kunnen delen dezelfde poort. Dit betekent dat de communicatielistener delen van de poort ondersteunen moet. De aanbevolen manier is voor de communicatielistener voor de partitie-ID en replica/exemplaar-ID gebruiken bij het genereren van de listen-adres.
>
>

### <a name="service-address-registration"></a>Registratie van service-adres
Een system-service met de naam de *Naming-Service* wordt uitgevoerd op Service Fabric-clusters. De Naming-Service is een registrar voor services en de adressen die op elk exemplaar of de replica van de service luistert. Wanneer de `OpenAsync(C#) / openAsync(Java)` -methode van een `ICommunicationListener(C#) / CommunicationListener(Java)` is voltooid, wordt de return-waarde wordt geregistreerd in de Naming-Service. Deze waarde die wordt gepubliceerd in de Naming-Service is een tekenreeks waarvan de waarde van alles op alle zijn kan. Deze tekenreekswaarde is wat clients zien wanneer ze voor een adres voor de service uit de Naming-Service vragen.

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

Service Fabric biedt API's waarmee clients en andere services kunnen vervolgens vraagt u voor dit adres met de servicenaam. Dit is belangrijk omdat het serviceadres niet statisch is. Services zijn in het cluster voor de bron-balancing en de beschikbaarheid doeleinden verplaatst. Dit is het mechanisme waarmee clients kunnen omzetten van het luisteren naar adres voor een service.

> [!NOTE]
> Zie voor een volledig overzicht van het schrijven van een communicatielistener [Web-API voor Service Fabric-services met OWIN zelf die als host fungeert](service-fabric-reliable-services-communication-webapi.md) voor C#, terwijl voor Java kunt u uw eigen HTTP-server-implementatie Zie EchoServer Voorbeeld van de toepassing op https://github.com/Azure-Samples/service-fabric-java-getting-started.
>
>

## <a name="communicating-with-a-service"></a>Communiceren met een service
De Reliable Services-API biedt de volgende bibliotheken voor het schrijven van clients die met services communiceren.

### <a name="service-endpoint-resolution"></a>Service-eindpunt resolutie
De eerste stap bij de communicatie met een service is een eindpuntadres van de partitie of het exemplaar van de service die u communiceren wilt met oplossen. De `ServicePartitionResolver(C#) / FabricServicePartitionResolver(Java)` hulpprogrammaklasse is een eenvoudige primitief waarmee clients bepalen het eindpunt van een service tijdens runtime. In de Service Fabric-terminologie, het proces voor het bepalen van het eindpunt van een service wordt aangeduid als de *service-eindpunt resolutie*.

Als u wilt verbinding maken met services binnen een cluster, kunnen ServicePartitionResolver worden gemaakt met de standaardinstellingen. Dit is het aanbevolen gebruik voor de meeste situaties:

```csharp
ServicePartitionResolver resolver = ServicePartitionResolver.GetDefault();
```
```java
FabricServicePartitionResolver resolver = FabricServicePartitionResolver.getDefault();
```

Als u wilt verbinding maken met services in een ander cluster, kan een ServicePartitionResolver worden gemaakt met een set eindpunten van de cluster-gateway. Houd er rekening mee dat eindpunten van de gateway alleen verschillende eindpunten voor de verbinding met hetzelfde cluster. Bijvoorbeeld:

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver("mycluster.cloudapp.azure.com:19000", "mycluster.cloudapp.azure.com:19001");
```

U kunt ook `ServicePartitionResolver` een functie kan worden opgegeven voor het maken van een `FabricClient` intern gebruiken:

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

`FabricClient` is het object dat wordt gebruikt om te communiceren met de Service Fabric-cluster voor verschillende bewerkingen op het cluster. Dit is handig als u meer controle wilt over hoe een service partitie resolver met uw cluster communiceert. `FabricClient` voert intern caching en is in het algemeen duur voor het maken, dus is het belangrijk om te gebruiken `FabricClient` exemplaren zo veel mogelijk.

```csharp
ServicePartitionResolver resolver = new  ServicePartitionResolver(() => CreateMyFabricClient());
```
```java
FabricServicePartitionResolver resolver = new  FabricServicePartitionResolver(() -> new CreateFabricClientImpl());
```

Een methode resolve wordt vervolgens gebruikt om op te halen van het adres van een service of de servicepartitie van een voor gepartitioneerde services.

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

Een serviceadres kan worden omgezet eenvoudig met behulp van een ServicePartitionResolver, maar meer werk is vereist om te controleren of dat de omgezette adres correct kan worden gebruikt. De client moet detecteren of de verbindingspoging is mislukt vanwege een tijdelijke fout en kan opnieuw worden uitgevoerd (bijv, service verplaatst of is tijdelijk niet beschikbaar), of een permanente fout (bijvoorbeeld de service is verwijderd of de aangevraagde resource bestaat niet meer). Service-exemplaren of replica's kunnen verplaatsen van het knooppunt voor knooppunt op elk gewenst moment om verschillende redenen. Het adres van de service opgelost via ServicePartitionResolver mogelijk verouderd zijn op het moment dat de clientcode verbinding probeert te maken. In dat geval opnieuw de client moet het adres opnieuw omzetten. Leveren van de vorige `ResolvedServicePartition` geeft aan dat de conflictoplosser moet het opnieuw proberen in plaats van gewoon ophalen een adres in de cache.

Normaal gesproken moet de clientcode werken niet met de ServicePartitionResolver rechtstreeks. Het is gemaakt en doorgegeven aan communicatie client factory's in de betrouwbare Services-API. De fabrieken intern gebruikmaken van de conflictoplosser voor het genereren van een clientobject dat kan worden gebruikt om te communiceren met services.

### <a name="communication-clients-and-factories"></a>Communicatie-clients en Factory 's
De communicatie factory-bibliotheek implementeert een patroon voor typische afhandeling van fouten opnieuw proberen die opnieuw proberen verbindingen met opgelost service-eindpunten gemakkelijker maakt. De factory-bibliotheek biedt het mechanisme voor opnieuw proberen terwijl u de foutenhandlers bieden.

`ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)` Hiermee definieert u de basis-interface geïmplementeerd door een clientfabriek van communicatie die door clients die met een Service Fabric-service communiceren kunnen gemaakt. De implementatie van de CommunicationClientFactory, is afhankelijk van de communicatiestack die wordt gebruikt door de Service Fabric-service waar de client wil om te communiceren. De Reliable Services-API biedt een `CommunicationClientFactoryBase<TCommunicationClient>`. Dit biedt een basis-implementatie van de interface CommunicationClientFactory en voert taken uit die gemeenschappelijk voor alle communicatie-stacks zijn. (Deze taken omvatten met behulp van een ServicePartitionResolver om te bepalen van het service-eindpunt). Clients implementeren gewoonlijk de abstracte klasse CommunicationClientFactoryBase voor het afhandelen van logica die specifiek is voor de communicatiestack.

De client communicatie wordt alleen een adres ontvangt en gebruikt deze verbinding maken met een service. De client kan de protocol software wil gebruiken.

```csharp
public class MyCommunicationClient : ICommunicationClient
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

De clientfabriek is primair verantwoordelijk is voor het maken van clients voor communicatie. Voor clients die geen van een permanente verbinding, zoals een HTTP-client onderhouden, moet de factory alleen maken en de client geretourneerd. Andere protocollen die een permanente verbinding, zoals sommige binaire protocollen onderhouden moeten ook worden gevalideerd door de factory om te bepalen of de verbinding moet opnieuw worden gemaakt.  

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

Ten slotte is een uitzonderingshandler verantwoordelijk voor het bepalen welke actie moet worden uitgevoerd wanneer er een uitzondering optreedt. Uitzonderingen zijn onderverdeeld in **herstelbare** en **niet-herstelbare**.

* **Niet-herstelbare** uitzonderingen gewoon ophalen rethrown terug naar de oproepende functie.
* **herstelbare** uitzonderingen kunnen verder worden onderverdeeld in **tijdelijke** en **niet-tijdelijke**.
  * **Tijdelijke** uitzonderingen zijn die alleen kunnen worden verwerkt zonder dat het adres van het service-eindpunt opnieuw op te lossen. Hierbij wordt tijdelijke netwerkproblemen of service-foutberichten die wijzen op dat het adres van het service-eindpunt bestaat niet.
  * **Niet-tijdelijke** uitzonderingen zijn die vereisen het adres van het service-eindpunt dat worden opnieuw opgelost. Deze omvatten uitzonderingen die wijzen op dat het service-eindpunt kan niet worden bereikt, is die wijzen op de service verplaatst naar een ander knooppunt.

De `TryHandleException` een beslissing heeft genomen over een bepaalde uitzonderingen. Als het **niet weet** welke beslissingen nemen over een uitzondering moet worden geretourneerd **false**. Als het **weet** welke besluit te maken, moet het resultaat dienovereenkomstig ingesteld en retourneren **waar**.

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
### <a name="putting-it-all-together"></a>Alles samenvoegen
Met een `ICommunicationClient(C#) / CommunicationClient(Java)`, `ICommunicationClientFactory(C#) / CommunicationClientFactory(Java)`, en `IExceptionHandler(C#) / ExceptionHandler(Java)` gebaseerd op een communicatieprotocol een `ServicePartitionClient(C#) / FabricServicePartitionClient(Java)` alles bij elkaar verpakt en biedt de afhandeling van fouten en service partitie adres resolutie lus om deze onderdelen.

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
* [ASP.NET Core met betrouwbare Services](service-fabric-reliable-services-communication-aspnetcore.md)
* [Externe procedureaanroepen met externe communicatie Reliable Services](service-fabric-reliable-services-communication-remoting.md)
* [WCF-communicatie via Reliable Services](service-fabric-reliable-services-communication-wcf.md)
