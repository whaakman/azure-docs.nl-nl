---
title: Service-communicatie met de ASP.NET Core | Microsoft Docs
description: Informatie over het gebruik van ASP.NET Core in staatloze en stateful Reliable Services.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: 8aa4668d-cbb6-4225-bd2d-ab5925a868f2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 05/02/2017
ms.author: vturecek
ms.openlocfilehash: 8ac4d409f7363e8b4ae98be659a627ac8db8d787
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="aspnet-core-in-service-fabric-reliable-services"></a>ASP.NET Core in Service Fabric Reliable Services

ASP.NET Core is een nieuw open source en platformoverschrijdende framework voor het bouwen van moderne cloud-gebaseerde Internet verbonden toepassingen, zoals web-apps, IoT-apps en mobiele back-ends. 

In dit artikel is een uitgebreide handleiding voor ASP.NET Core hostingservices in Service Fabric Reliable Services met behulp van de **Microsoft.ServiceFabric.AspNetCore.** * set NuGet-pakketten.

Zie voor een inleidende zelfstudie voor ASP.NET Core in Service Fabric en instructies over het ophalen van uw ontwikkelomgeving instellen [bouwen van een webfront-end voor uw toepassing met behulp van ASP.NET Core](service-fabric-add-a-web-frontend.md).

De rest van dit artikel wordt ervan uitgegaan dat u al bekend bent met ASP.NET Core. Als u niet het geval is, wordt aangeraden lezen via de [ASP.NET Core grondbeginselen](https://docs.microsoft.com/aspnet/core/fundamentals/index).

## <a name="aspnet-core-in-the-service-fabric-environment"></a>ASP.NET Core in de Service Fabric-omgeving

Hoewel ASP.NET Core apps op .NET Core of op het volledige .NET Framework uitvoeren kunnen, Service Fabric services momenteel kunnen alleen worden uitgevoerd op het volledige .NET Framework. Dit betekent dat wanneer u een ASP.NET Core Service Fabric-service maakt, moet u nog steeds het volledige .NET Framework richt.

ASP.NET Core kunnen worden gebruikt op twee verschillende manieren in Service Fabric:
 - **Als het uitvoerbare bestand van een gast gehost**. Dit is vooral op bestaande ASP.NET Core toepassingen uitvoeren op Service Fabric zonder wijzigingen code gebruikt.
 - **Uitgevoerd binnen een betrouwbare Service**. Dit kunt betere integratie met de Service Fabric-runtime en stateful ASP.NET Core services.

De rest van dit artikel wordt uitgelegd hoe u ASP.NET Core binnen een betrouwbare Service met de integratieonderdelen van ASP.NET Core die worden geleverd met de Service Fabric SDK. 

## <a name="service-fabric-service-hosting"></a>Hosting van service Fabric-service

In Service Fabric, een of meer exemplaren en/of replica's van uw service worden uitgevoerd in een *service hostproces*, een uitvoerbaar bestand dat uw servicecode wordt uitgevoerd. U, als de auteur van een service, eigenaar van het hostproces van de service en Service Fabric wordt geactiveerd en wordt deze voor u bewaakt.

Traditionele ASP.NET (maximaal MVC 5) is nauw gekoppeld aan IIS via System.Web.dll. ASP.NET Core biedt een scheiding tussen de webserver en uw webtoepassing. Hiermee kan webtoepassingen overdraagbaar tussen verschillende webservers en kunt u ook webservers *zelf gehost*, wat betekent dat u start een webserver in uw eigen proces, in plaats van een proces dat eigendom is van een specifieke web Server-software zoals IIS. 

Om een combinatie van een Service Fabric-service en ASP.NET als het uitvoerbare bestand van een gastbesturingssysteem of in een betrouwbare Service, moet u ASP.NET binnen het hostproces van uw service te starten. ASP.NET Core zelf hosting kunt u om dit te doen.

## <a name="hosting-aspnet-core-in-a-reliable-service"></a>Hosting van ASP.NET Core in een betrouwbare Service
Normaal gesproken zelf gehoste ASP.NET Core toepassingen maken een WebHost in het toegangspunt voor een toepassing, zoals de `static void Main()` methode in `Program.cs`. In dit geval is de levenscyclus van de WebHost gebonden aan de levenscyclus van het proces.

![ASP.NET Core hosten in een proces][0]

Het toegangspunt dat van toepassing is echter niet de juiste plaats te maken van een WebHost in een betrouwbare Service omdat het toegangspunt voor de toepassing alleen gebruikt wordt voor een servicetype registreren bij de Service Fabric-runtime, zodat deze exemplaren van dat servicetype kan maken. De WebHost moet worden gemaakt in een betrouwbare Service zelf. Binnen het hostproces van de service, kunnen service-exemplaren en/of replica's doorlopen die meerdere levenscycli. 

Een betrouwbare Service-exemplaar wordt vertegenwoordigd door uw serviceklasse die zijn afgeleid van `StatelessService` of `StatefulService`. De communicatiestack voor een service deel uitmaakt van een `ICommunicationListener` -implementatie in uw serviceklasse. De `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet-pakketten bevatten implementaties van `ICommunicationListener` die starten en beheren van de ASP.NET Core WebHost voor Kestrel of WebListener in een betrouwbare Service.

![Hosting van ASP.NET Core in een betrouwbare Service][1]

## <a name="aspnet-core-icommunicationlisteners"></a>ASP.NET Core ICommunicationListeners
De `ICommunicationListener` implementaties voor Kestrel en WebListener in de `Microsoft.ServiceFabric.Services.AspNetCore.*` NuGet-pakketten hebben vergelijkbare gebruikspatronen maar enigszins verschillende acties die specifiek zijn voor elke webserver worden uitgevoerd. 

Beide communicatielisteners bieden een constructor die de volgende argumenten:
 - **`ServiceContext serviceContext`**: De `ServiceContext` object dat informatie over de service bevat.
 - **`string endpointName`**: de naam van een `Endpoint` configuratie in ServiceManifest.xml. Dit is vooral waarbij de twee communicatielisteners verschillen: WebListener **vereist** een `Endpoint` configuratie, maar niet door Kestrel.
 - **`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: een lambda die u implementeert in die u maakt en retourneren een `IWebHost`. Hiermee kunt u configureren `IWebHost` de manier waarop u dat gewend bent in een toepassing ASP.NET Core. De lambda biedt een URL die wordt gegenereerd voor u, afhankelijk van de Service Fabric-integratie u opties gebruiken en de `Endpoint` configuratie die u opgeeft. Dat URL vervolgens kan worden gewijzigd of als gebruikt-is het starten van de webserver.

## <a name="service-fabric-integration-middleware"></a>Service Fabric-integratie middleware
De `Microsoft.ServiceFabric.Services.AspNetCore` NuGet-pakket bevat de `UseServiceFabricIntegration` extensiemethode op `IWebHostBuilder` die Service Fabric-bewuste middleware toevoegt. Deze middleware configureert u de Kestrel of WebListener `ICommunicationListener` een unieke URL registreren bij de Service Fabric Naming Service en controleert vervolgens clientaanvragen zodat clients verbinding maken met de juiste service. Dit is nodig in een omgeving met gedeelde host zoals Service Fabric, waarbij meerdere webtoepassingen kunnen uitvoeren op de dezelfde fysieke of virtuele machine, maar gebruik geen unieke hostnamen, om te voorkomen dat clients per ongeluk verbinding maken met de verkeerde service. Dit scenario wordt in de volgende sectie uitvoeriger beschreven.

### <a name="a-case-of-mistaken-identity"></a>Een aanvraag van onjuiste identiteit
Service-replica's, ongeacht het protocol, luisteren op de combinatie van een uniek IP: poort. Wanneer een replica van de service is begonnen met luisteren op een eindpunt IP: poort, rapporteert dat eindpuntadres dat de Service Fabric Naming Service waar deze kan worden gedetecteerd door clients of andere services. Als services toepassing dynamisch toegewezen poorten, kan een service-replica hetzelfde IP: poort eindpunt van een andere service die op de dezelfde fysieke of virtuele machine voorheen tegelijk gebruiken. Dit kan ertoe leiden dat een client mistakely verbinding met de verkeerde service. Dit kan gebeuren als de volgende reeks gebeurtenissen optreden:

 1. Een service luistert op 10.0.0.1:30000 via HTTP. 
 2. Client-Service A wordt omgezet en adres 10.0.0.1:30000 opgehaald
 3. Een service wordt verplaatst naar een ander knooppunt.
 4. Service B 10.0.0.1 is geplaatst en dezelfde poort 30000 tegelijk worden gebruikt.
 5. Client probeert verbinding maken met een service met het adres in cache 10.0.0.1:30000.
 6. Client is nu is verbonden met de service geen B is verbonden met de verkeerde service.

Dit kan fouten veroorzaken op willekeurige tijdstippen die mogelijk moeilijk op te sporen. 

### <a name="using-unique-service-urls"></a>Met unieke service-URL 's
Om dit te voorkomen, kunnen services een eindpunt posten naar de Naming Service met een unieke id en vervolgens valideren die de unieke id tijdens het aanvragen van clients. Dit is een gezamenlijke actie tussen services in een vertrouwde omgeving van niet-onveilige-tenant. Dit biedt geen verificatie van de beveiligde service in een onveilige tenant-omgeving.

In een vertrouwde omgeving, de middleware die wordt toegevoegd door de `UseServiceFabricIntegration` methode wordt een unieke id automatisch toegevoegd aan het adres dat is verzonden naar de Naming Service en valideert die bij elke aanvraag-id. Als de id komt niet overeen met, retourneert de middleware onmiddellijk een antwoord HTTP 410 geworden.

Services die gebruikmaken van een dynamisch toegewezen poort moet maken gebruik van deze middleware.

Services die gebruikmaken van een vaste unieke poort hoeft dit probleem niet in een gezamenlijke omgeving. Een unieke vaste poort wordt doorgaans gebruikt voor extern gerichte services die een bekende poort clienttoepassingen die verbinding maken met nodig hebt. Bijvoorbeeld: de meeste internetgerichte webtoepassingen poort 80 of 443 gebruikt voor web browser verbindingen. In dit geval wordt worden de unieke id niet ingeschakeld.

Het volgende diagram toont de aanvraagstroom met de middleware ingeschakeld:

![Service Fabric ASP.NET Core-integratie][2]

Zowel Kestrel als WebListener `ICommunicationListener` implementaties dit mechanisme op exact dezelfde manier gebruiken. Hoewel WebListener kunt aanvragen op basis van unieke URL-paden door de onderliggende intern onderscheiden *http.sys* poort functie die de functionaliteit voor het delen *niet* die wordt gebruikt door de WebListener `ICommunicationListener` implementatie omdat die in een HTTP 503- en HTTP 404 fout statuscodes in het scenario dat eerder is beschreven resulteert. Op zijn beurt waardoor het moeilijk voor clients om te bepalen van de intentie van de fout, zoals HTTP 503- en HTTP 404 al vaak gebruikt wordt om aan te geven van andere fouten. Dus zowel Kestrel en WebListener `ICommunicationListener` implementaties standaardiseren op de middleware geleverd door de `UseServiceFabricIntegration` uitbreidingsmethode zodat clients alleen hoeft uit te voeren een service-eindpunt te zetten actie op 410 HTTP-antwoorden.

## <a name="weblistener-in-reliable-services"></a>WebListener in Reliable Services
WebListener kan worden gebruikt in een betrouwbare Service door het importeren van de **Microsoft.ServiceFabric.AspNetCore.WebListener** NuGet-pakket. Dit pakket bevat `WebListenerCommunicationListener`, een implementatie van `ICommunicationListener`, waarmee u een ASP.NET Core WebHost binnen een betrouwbare Service maken WebListener gebruikt als de webserver.

WebListener is gebaseerd op de [Windows HTTP-Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx). Dit maakt gebruik van de *http.sys* kernel-stuurprogramma dat door IIS wordt gebruikt voor het verwerken van HTTP-aanvragen en deze doorsturen naar webtoepassingen die worden uitgevoerd. Hierdoor kunnen meerdere processen op de dezelfde fysieke of virtuele machine naar een host-webtoepassingen op dezelfde poort ondubbelzinnig gemaakt door een unieke URL-pad of de hostnaam. Deze functies zijn nuttig in Service Fabric voor het hosten van meerdere websites in hetzelfde cluster.

Het volgende diagram illustreert hoe WebListener gebruikt de *http.sys* kernel-stuurprogramma in Windows voor het delen van poort:

![HTTP.sys][3]

### <a name="weblistener-in-a-stateless-service"></a>WebListener in een stateless service
Te gebruiken `WebListener` in een stateless service overschrijven de `CreateServiceInstanceListeners` methode en retourneren een `WebListenerCommunicationListener` exemplaar:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseWebListener()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build()))
    };
}
```

### <a name="weblistener-in-a-stateful-service"></a>WebListener in een stateful service

`WebListenerCommunicationListener`op dit moment niet is ontworpen voor gebruik in stateful services vanwege problemen met de onderliggende *http.sys* poort functie voor het delen. Zie voor meer informatie de volgende sectie op dynamische poorttoewijzing met WebListener. Kestrel is de aanbevolen webserver voor stateful services.

### <a name="endpoint-configuration"></a>Endpoint-configuratie

Een `Endpoint` configuratie is vereist voor webservers die gebruikmaken van de Windows HTTP-Server API, met inbegrip van WebListener. Webservers die gebruikmaken van de Windows HTTP-Server API moeten eerst hun URL met reserveren *http.sys* (dit is normaal gesproken bewerkstelligd met de [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) tool). Deze actie is vereist voor verhoogde bevoegdheden die uw services standaard geen hebben. De opties 'http' of 'https' voor de `Protocol` eigenschap van de `Endpoint` configuratie in *ServiceManifest.xml* worden gebruikt om specifiek te instrueren van de Service Fabric-runtime registratie van een URL met  *HTTP.sys* op uw rekening met de [ *sterk jokertekens* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL-voorvoegsel.

Om bijvoorbeeld te reserveren `http://+:80` voor een service de volgende configuratie moet worden gebruikt in ServiceManifest.xml:

```xml
<ServiceManifest ... >
    ...
    <Resources>
        <Endpoints>
            <Endpoint Name="ServiceEndpoint" Protocol="http" Port="80" />
        </Endpoints>
    </Resources>

</ServiceManifest>
```

En de naam van het eindpunt moet worden doorgegeven aan de `WebListenerCommunicationListener` constructor:

```csharp
 new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     return new WebHostBuilder()
         .UseWebListener()
         .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
         .UseUrls(url)
         .Build();
 })
```

#### <a name="use-weblistener-with-a-static-port"></a>WebListener gebruiken met een statische poort
Als u wilt gebruiken een statische poort met WebListener, geef het poortnummer in de `Endpoint` configuratie:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-weblistener-with-a-dynamic-port"></a>WebListener gebruiken met een dynamische poort
Als u wilt gebruiken een dynamisch toegewezen poort met WebListener, laat de `Port` eigenschap in de `Endpoint` configuratie:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

Merk op dat een dynamische poort is toegewezen door een `Endpoint` configuratie biedt slechts één poort *per hostproces*. Het huidige model van de Service Fabric-hosting kunt meerdere exemplaren van de service en/of replica's worden gehost in hetzelfde proces, wat betekent dat elke dezelfde poort delen als toegewezen via de `Endpoint` configuratie. Meerdere exemplaren van WebListener kunnen delen een poort die door de onderliggende *http.sys* poort delen onderdeel, maar die niet wordt ondersteund door `WebListenerCommunicationListener` vanwege de problemen die het geeft voor clientaanvragen. Voor het gebruik van dynamische poort is Kestrel de aanbevolen webserver.

## <a name="kestrel-in-reliable-services"></a>Kestrel in Reliable Services
Kestrel kan worden gebruikt in een betrouwbare Service door het importeren van de **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet-pakket. Dit pakket bevat `KestrelCommunicationListener`, een implementatie van `ICommunicationListener`, waarmee u een ASP.NET Core WebHost binnen een betrouwbare Service maken Kestrel gebruikt als de webserver.

Kestrel is dat een platformoverschrijdende webserver voor ASP.NET Core op basis van libuv, een platformoverschrijdende asynchrone i/o-bibliotheek. In tegenstelling tot WebListener, Kestrel gebruikt geen een gecentraliseerde eindpunt manager zoals *http.sys*. En in tegenstelling tot WebListener, Kestrel biedt geen ondersteuning voor poort delen tussen meerdere processen. Elk exemplaar van Kestrel moet een unieke poort gebruiken.

![kestrel][4]

### <a name="kestrel-in-a-stateless-service"></a>Kestrel in een stateless service
Te gebruiken `Kestrel` in een stateless service overschrijven de `CreateServiceInstanceListeners` methode en retourneren een `KestrelCommunicationListener` exemplaar:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                        services => services
                            .AddSingleton<StatelessServiceContext>(serviceContext))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

### <a name="kestrel-in-a-stateful-service"></a>Kestrel in een stateful service
Te gebruiken `Kestrel` in een stateful service overschrijven de `CreateServiceReplicaListeners` methode en retourneren een `KestrelCommunicationListener` exemplaar:

```csharp
protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
{
    return new ServiceReplicaListener[]
    {
        new ServiceReplicaListener(serviceContext =>
            new KestrelCommunicationListener(serviceContext, (url, listener) =>
                new WebHostBuilder()
                    .UseKestrel()
                    .ConfigureServices(
                         services => services
                             .AddSingleton<StatefulServiceContext>(serviceContext)
                             .AddSingleton<IReliableStateManager>(this.StateManager))
                    .UseContentRoot(Directory.GetCurrentDirectory())
                    .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.UseUniqueServiceUrl)
                    .UseStartup<Startup>()
                    .UseUrls(url)
                    .Build();
            ))
    };
}
```

In dit voorbeeld wordt een singleton-instantie van `IReliableStateManager` is opgegeven voor de WebHost afhankelijkheid injectie container. Dit is niet strikt noodzakelijk zijn, maar Hiermee kunt u gebruiken `IReliableStateManager` en betrouwbare verzamelingen in uw MVC-controller-actiemethoden.

Houd er rekening mee dat een `Endpoint` configuratienaam is **niet** opgegeven voor `KestrelCommunicationListener` in een stateful service. Dit wordt uitgelegd in de volgende sectie uitvoeriger.

### <a name="endpoint-configuration"></a>Endpoint-configuratie
Een `Endpoint` configuratie is niet vereist voor het gebruik van Kestrel. 

Kestrel is een eenvoudige zelfstandige webserver; in tegenstelling tot WebListener (of HttpListener), hoeft deze niet een `Endpoint` configuratie in *ServiceManifest.xml* omdat er geen registratie voorafgaand aan de URL nodig. 

#### <a name="use-kestrel-with-a-static-port"></a>Kestrel gebruiken met een statische poort
Een statische poort kan worden geconfigureerd in de `Endpoint` ServiceManifest.xml-configuratie voor gebruik met Kestrel. Hoewel dit niet strikt noodzakelijk is, heeft deze twee mogelijke voordelen:
 1. Als de poort niet in het poortbereik van de toepassing valt, wordt het geopend via de firewall van het besturingssysteem door de Service Fabric.
 2. De URL die u via `KestrelCommunicationListener` deze poort wordt gebruikt.

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

Als een `Endpoint` is geconfigureerd, de naam moet worden doorgegeven in de `KestrelCommunicationListener` constructor: 

```csharp
new KestrelCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) => ...
```

Als een `Endpoint` configuratie wordt niet gebruikt, laat u de naam in de `KestrelCommunicationListener` constructor. In dit geval wordt een dynamische poort gebruikt. Zie de volgende sectie voor meer informatie.

#### <a name="use-kestrel-with-a-dynamic-port"></a>Kestrel gebruiken met een dynamische poort
Kestrel niet gebruiken de automatische poorttoewijzing van de `Endpoint` configuratie in ServiceManifest.xml, omdat automatische poorttoewijzing van een `Endpoint` configuratie wijst een unieke poort per *proces host*, en een proces voor één host kan meerdere exemplaren van Kestrel bevatten. Aangezien Kestrel biedt geen ondersteuning voor het delen van de poort, werkt dit niet zoals elk exemplaar Kestrel moet worden geopend op een unieke poort.

Voor het gebruik van dynamische poorttoewijzing met Kestrel, laat u gewoon de `Endpoint` configuratie in ServiceManifest.xml volledig, en geeft u de naam van een eindpunt niet door de `KestrelCommunicationListener` constructor:

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

In deze configuratie `KestrelCommunicationListener` wordt automatisch een niet-gebruikte poort selecteren in het poortbereik van toepassing.

## <a name="scenarios-and-configurations"></a>Scenario's en configuraties
In deze sectie worden de volgende scenario's beschreven en de aanbevolen combinatie van de webserver, poortconfiguratie, opties voor Service Fabric-integratie en diverse instellingen voor een goed functionerende service biedt:
 - Extern blootgesteld ASP.NET Core staatloze service
 - Alleen intern-ASP.NET Core staatloze service
 - Alleen intern-ASP.NET Core stateful service

Een **extern blootgesteld** -service maakt dat toegang biedt tot een eindpunt dat bereikbaar is vanaf buiten het cluster, meestal via een load balancer.

Een **alleen interne** -service is een waarvan eindpunt alleen bereikbaar is vanaf binnen het cluster is.

> [!NOTE]
> Stateful service-eindpunten moeten in het algemeen niet worden blootgesteld aan Internet. Clusters die zich achter de load balancers die zich niet bewust zijn van de oplossing van het Service Fabric-service, zoals Azure Load Balancer, zich niet meer zichtbaar stateful services maken, omdat de load balancer kan niet worden om te zoeken en verkeer naar de juiste sturen stateful service-replica. 

### <a name="externally-exposed-aspnet-core-stateless-services"></a>Extern blootgesteld ASP.NET Core stateless services
WebListener is de aanbevolen webserver voor front-end-services die externe, internetgerichte HTTP-eindpunten in Windows. Dit biedt betere bescherming tegen aanvallen en functies die Kestrel niet, zoals Windows-verificatie en -poort delen bestaat ondersteunt. 

Kestrel wordt niet ondersteund als een edge (internetgerichte)-server op dit moment. Een reverse proxy-server zoals IIS of Nginx moet worden gebruikt voor het afhandelen van verkeer van het openbare Internet.
 
Wanneer blootgesteld aan Internet, moet een stateless service een bekende en stabiele eindpunt dat bereikbaar is via een load balancer gebruiken. Dit is de URL die u aan gebruikers van uw toepassing biedt. De volgende configuratie wordt aanbevolen:

|  |  | **Opmerkingen bij de** |
| --- | --- | --- |
| Webserver | WebListener | Als de service is alleen beschikbaar worden gemaakt met een vertrouwd netwerk, een intranet kan Kestrel worden gebruikt. Anders is WebListener de gewenste optie. |
| Poortconfiguratie | Statische | Een bekende statische poort moet worden geconfigureerd in de `Endpoints` configuratie van ServiceManifest.xml, zoals 80 voor HTTP en 443 voor HTTPS. |
| ServiceFabricIntegrationOptions | Geen | De `ServiceFabricIntegrationOptions.None` moet worden gebruikt bij het Service Fabric-integratie middleware configureren zodat de service niet probeert te valideren van binnenkomende aanvragen voor een unieke id. Externe gebruikers van uw toepassing weet niet de unieke gegevens die worden gebruikt door de middleware. |
| Aantal exemplaren | -1 | In een typische gebruiksvoorbeelden, moet het aantal exemplaren instellen '1' worden ingesteld zodat een exemplaar is beschikbaar op alle knooppunten die verkeer van een load balancer ontvangt. |

Als meerdere extern blootgestelde services dezelfde set knooppunten delen, moet een unieke maar stabiele URL-pad worden gebruikt. Dit kan worden bereikt door het wijzigen van de URL die is opgegeven bij het configureren van IWebHost. Houd er rekening mee dat dit alleen van toepassing op WebListener.

 ```csharp
 new WebListenerCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     url += "/MyUniqueServicePath";
 
     return new WebHostBuilder()
         .UseWebListener()
         ...
         .UseUrls(url)
         .Build();
 })
 ```

### <a name="internal-only-stateless-aspnet-core-service"></a>Alleen intern staatloze ASP.NET Core-service
Stateless services die alleen worden aangeroepen vanuit binnen het cluster moeten unieke URL's gebruiken en dynamisch toegewezen poorten om ervoor te zorgen samenwerking tussen meerdere services. De volgende configuratie wordt aanbevolen:

|  |  | **Opmerkingen bij de** |
| --- | --- | --- |
| Webserver | kestrel | Hoewel WebListener kan worden gebruikt voor interne stateless services, is Kestrel de aanbevolen server zodat meerdere exemplaren van de service voor het delen van een host.  |
| Poortconfiguratie | dynamisch toegewezen | Meerdere replica's van een stateful service kunnen een hostproces of hostbesturingssysteem delen en moeten dus unieke poorten. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Bij toewijzing van de dynamische poort is deze instelling voorkomt dat het probleem onjuiste identiteit is eerder beschreven. |
| InstanceCount | alle | Het aantal exemplaren instelling kan worden ingesteld op een andere waarde nodig om de service. |

### <a name="internal-only-stateful-aspnet-core-service"></a>Alleen intern stateful ASP.NET Core-service
Stateful services die alleen worden aangeroepen vanuit binnen het cluster moeten dynamisch toegewezen poorten gebruiken om ervoor te zorgen samenwerking tussen meerdere services. De volgende configuratie wordt aanbevolen:

|  |  | **Opmerkingen bij de** |
| --- | --- | --- |
| Webserver | kestrel | De `WebListenerCommunicationListener` is niet ontworpen voor gebruik door stateful services waarin de replica's een hostproces delen. |
| Poortconfiguratie | dynamisch toegewezen | Meerdere replica's van een stateful service kunnen een hostproces of hostbesturingssysteem delen en moeten dus unieke poorten. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Bij toewijzing van de dynamische poort is deze instelling voorkomt dat het probleem onjuiste identiteit is eerder beschreven. |

## <a name="next-steps"></a>Volgende stappen
[Fouten opsporen in uw Service Fabric-toepassing met behulp van Visual Studio](service-fabric-debugging-your-application.md)

<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
