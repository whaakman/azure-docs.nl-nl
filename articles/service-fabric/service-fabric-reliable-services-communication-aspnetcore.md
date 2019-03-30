---
title: Communicatie met de ASP.NET Core-service | Microsoft Docs
description: Informatie over het gebruik van ASP.NET Core in staatloze en stateful Reliable Services.
services: service-fabric
documentationcenter: .net
author: vturecek
manager: chackdan
editor: ''
ms.assetid: 8aa4668d-cbb6-4225-bd2d-ab5925a868f2
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: required
ms.date: 10/12/2018
ms.author: vturecek
ms.openlocfilehash: 98cc6ee2428523b93b42fca73daadc118103b7d7
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58667477"
---
# <a name="aspnet-core-in-service-fabric-reliable-services"></a>ASP.NET Core in Service Fabric Reliable Services

ASP.NET Core is een nieuw open-source en platformoverschrijdende framework voor het bouwen van moderne cloud-gebaseerde Internet verbonden toepassingen, zoals web-apps, IoT-apps en mobiele back-ends. 

In dit artikel is een uitgebreide handleiding voor het hosten van ASP.NET Core-services in met behulp van Service Fabric Reliable Services de **Microsoft.ServiceFabric.AspNetCore.** * set van NuGet-pakketten.

Zie voor een inleidende zelfstudie voor ASP.NET Core in Service Fabric en instructies voor het beheer van uw ontwikkelomgeving instellen [maken van een .NET-toepassing](service-fabric-tutorial-create-dotnet-app.md).

De rest van dit artikel wordt ervan uitgegaan dat u al bekend bent met ASP.NET Core. Als u niet het geval is, wordt aangeraden lezen via de [basisprincipes van ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/index).

## <a name="aspnet-core-in-the-service-fabric-environment"></a>ASP.NET Core in de Service Fabric-omgeving

ASP.NET Core en Service Fabric-apps kunnen worden uitgevoerd op .NET Core en volledige .NET Framework. ASP.NET Core kan op twee verschillende manieren in Service Fabric worden gebruikt:
 - **Als een Gast kan worden uitgevoerd wordt gehost**. Dit wordt hoofdzakelijk gebruikt bestaande ASP.NET Core-toepassingen uitvoeren in Service Fabric met zonder code te wijzigen.
 - **Uitgevoerd in een betrouwbare Service**. Dit zorgt voor een betere integratie met de Service Fabric-runtime en stateful ASP.NET Core, kunnen services.

De rest van dit artikel wordt uitgelegd hoe u ASP.NET Core binnen een betrouwbare Service met behulp van de ASP.NET Core-integratieonderdelen die worden geleverd met de Service Fabric SDK gebruiken. 

## <a name="service-fabric-service-hosting"></a>Hosting van service Fabric-service

In Service Fabric, een of meer exemplaren en/of replica's van uw service worden uitgevoerd in een *host serviceproces*, een uitvoerbaar bestand dat uw servicecode wordt uitgevoerd. U, als de auteur van een service, de eigenaar het hostproces van de service en Service Fabric wordt geactiveerd en controleert u het voor u.

Traditionele ASP.NET (maximaal MVC 5) is nauw gekoppeld aan IIS via System.Web.dll. ASP.NET Core biedt een scheiding tussen de webserver en uw web-App. Hiermee worden webtoepassingen overdraagbaar tussen verschillende webservers en kunt u ook webservers *zelf-hostend*, wat betekent dat u kunt een webserver starten in uw eigen proces, in plaats van een proces dat eigendom is van specifieke web Serversoftware, zoals IIS. 

Als u wilt combineren van een Service Fabric-service en ASP.NET, hetzij als een Gast kan worden uitgevoerd of in een betrouwbare Service, moet u ASP.NET gestart binnen het hostproces van uw service zijn. ASP.NET Core zelf die als host fungeert, kunt u om dit te doen.

## <a name="hosting-aspnet-core-in-a-reliable-service"></a>ASP.NET Core in een betrouwbare Service die als host fungeert
Normaal gesproken zelf-hostend ASP.NET Core-toepassingen maken een WebHost in het toegangspunt van een toepassing, zoals de `static void Main()` methode in `Program.cs`. In dit geval is de levenscyclus van de WebHost gebonden aan de levenscyclus van het proces.

![Hosting van ASP.NET Core in een proces][0]

Het toegangspunt dat van toepassing is echter niet de juiste plaats voor het maken van een WebHost in een betrouwbare Service omdat het toegangspunt voor de toepassing alleen gebruikt wordt voor het registreren van een servicetype met de Service Fabric-runtime, zodat deze exemplaren van dat servicetype kan maken. De WebHost moet worden gemaakt in een betrouwbare Service zelf. Binnen het hostproces van de service gaat service-exemplaren en/of replica's via meerdere levenscycli. 

Een betrouwbare Service-exemplaar wordt vertegenwoordigd door uw serviceklasse die is afgeleid van `StatelessService` of `StatefulService`. De communicatiestack voor een service is opgenomen in een `ICommunicationListener` -implementatie in uw serviceklasse. De `Microsoft.ServiceFabric.AspNetCore.*` NuGet-pakketten bevatten implementaties van `ICommunicationListener` die starten en beheren van de ASP.NET Core WebHost voor Kestrel of HttpSys in een betrouwbare Service.

![ASP.NET Core in een betrouwbare Service die als host fungeert][1]

## <a name="aspnet-core-icommunicationlisteners"></a>ASP.NET Core ICommunicationListeners
De `ICommunicationListener` implementaties voor Kestrel en HttpSys in de `Microsoft.ServiceFabric.AspNetCore.*` NuGet-pakketten zijn vergelijkbaar gebruikspatronen maar enigszins verschillende acties die specifiek zijn voor elke webserver uitvoeren. 

Beide communicatielisteners bieden een constructor die de volgende argumenten neemt:
 - **`ServiceContext serviceContext`**: De `ServiceContext` object dat informatie over de service bevat.
 - **`string endpointName`**: de naam van een `Endpoint` configuratie in ServiceManifest.xml. Dit is vooral waar de twee communicatielisteners verschillen: HttpSys **vereist** een `Endpoint` configuratie, maar Kestrel niet.
 - **`Func<string, AspNetCoreCommunicationListener, IWebHost> build`**: een lambda die u implementeert in die u maakt en retourneren een `IWebHost`. Hiermee kunt u configureren `IWebHost` de manier waarop u dat gewend bent in een ASP.NET Core-toepassing. De lambda-biedt een URL die wordt gegenereerd voor u, afhankelijk van de Service Fabric-integratie u opties gebruiken en de `Endpoint` configuratie die u opgeeft. Die URL vervolgens kan worden gewijzigd of als gebruikt-is om te beginnen de webserver.

## <a name="service-fabric-integration-middleware"></a>Service Fabric-integratiemiddleware
De `Microsoft.ServiceFabric.AspNetCore` NuGet-pakket bevat de `UseServiceFabricIntegration` uitbreidingsmethode op `IWebHostBuilder` die Service Fabric-bewuste middleware toevoegt. Deze middleware configureert de Kestrel of HttpSys `ICommunicationListener` voor het registreren van een unieke service-URL met de Service Fabric Naming-Service en vervolgens valideert aanvragen van clients zodat clients verbinding maken met de juiste service. Dit is nodig in een gedeelde-host-omgeving, zoals Service Fabric, waarbij meerdere webtoepassingen kunnen uitvoeren op de dezelfde fysieke of virtuele machine, maar gebruik geen unieke hostnamen, om te voorkomen dat clients per ongeluk verbinding maken met de verkeerde service. In dit scenario wordt beschreven in de volgende sectie in meer detail.

### <a name="a-case-of-mistaken-identity"></a>Een aanvraag van onjuiste identiteit
Service-replica's, ongeacht het protocol, luisteren op een combinatie van unieke IP: poort. Nadat de replica van een service is gestart op een eindpunt IP: poort luistert, rapporteert deze die eindpuntadres aan de Service Fabric Naming-Service waar deze kan worden gedetecteerd door clients of andere services. Als services toepassingspoorten dynamisch wordt toegewezen, kan een service-replica hetzelfde IP: poort-eindpunt van een andere service die eerder op de dezelfde fysieke of virtuele machine was tegelijk gebruiken. Dit kan een client deze per ongeluk verbinding maken met de verkeerde service veroorzaken. Dit kan gebeuren als de volgende reeks gebeurtenissen treedt op:

 1. Service A luistert op 10.0.0.1:30000 via HTTP. 
 2. Client-Service een oplossing en adres 10.0.0.1:30000 opgehaald
 3. Service A verplaatst naar een ander knooppunt.
 4. Service B wordt geplaatst op 10.0.0.1 en dezelfde poort 30000 coincidentally gebruikt.
 5. Client probeert verbinding maken met een service met in de cache adres 10.0.0.1:30000.
 6. Client is nu is verbonden met de service geen B is verbonden met de verkeerde service.

Dit kan leiden tot fouten op willekeurige tijdstippen die kunnen lastig zijn om op te sporen. 

### <a name="using-unique-service-urls"></a>Met behulp van unieke service-URL 's
Om dit te voorkomen, kunnen services plaatsen van een eindpunt naar de Naming-Service met een unieke id en vervolgens valideren dat de unieke id tijdens het aanvragen van clients. Dit is een gezamenlijke actie tussen services in een vertrouwde omgeving van niet-schadelijke-tenant. Dit biedt geen veilige service-verificatie in een omgeving met onveilige-tenants.

In een vertrouwde omgeving, de middleware die wordt toegevoegd door de `UseServiceFabricIntegration` methode wordt een unieke id automatisch toegevoegd aan het adres dat is verzonden naar de Naming-Service en wordt gevalideerd die bij elke aanvraag-id. Als de id komt niet overeen met, retourneert de middleware onmiddellijk een respons HTTP 410 geworden.

Services die gebruikmaken van een dynamisch toegewezen poort moet maken gebruik van deze middleware.

Services die gebruikmaken van een vaste unieke poort hebben geen dit probleem in een omgeving met gezamenlijke. Een vaste unieke poort wordt meestal gebruikt voor dergelijke services die een bekende poort voor clienttoepassingen verbinding maken met nodig hebt. De meeste Internet gerichte webtoepassingen gebruikt bijvoorbeeld poort 80 of 443 voor web browser verbindingen. In dit geval moet de unieke id niet worden ingeschakeld.

Het volgende diagram toont de aanvraagstroom met de middleware ingeschakeld:

![Integratie van service Fabric ASP.NET Core][2]

Kestrel zowel HttpSys `ICommunicationListener` implementaties dit mechanisme op exact dezelfde manier gebruiken. Hoewel HttpSys intern onderscheid maken tussen aanvragen op basis van unieke URL-paden door de onderliggende *http.sys* poort functie, die de functionaliteit is voor het delen *niet* die worden gebruikt door de HttpSys `ICommunicationListener` implementatie omdat dat tot HTTP 503- en HTTP 404-fout statuscodes in de eerder beschreven scenario leiden zal. Die op zijn beurt waardoor het lastig voor clients om te bepalen het doel van de fout, zoals HTTP 503- en HTTP 404 al vaak worden gebruikt om aan te geven van andere fouten. Dus zowel Kestrel en HttpSys `ICommunicationListener` implementaties is Apparaatgebruik zonder de middleware geleverd door de `UseServiceFabricIntegration` uitbreidingsmethode zodat clients hoeft uit te voeren een service-eindpunt opnieuw omzetten actie op 410 HTTP-antwoorden.

## <a name="httpsys-in-reliable-services"></a>HttpSys in Reliable Services
HttpSys kan worden gebruikt in een betrouwbare Service door het importeren van de **Microsoft.ServiceFabric.AspNetCore.HttpSys** NuGet-pakket. Dit pakket bevat `HttpSysCommunicationListener`, een implementatie van `ICommunicationListener`, die kunt u een ASP.NET Core WebHost binnen een betrouwbare Service maken met HttpSys als de webserver.

HttpSys is gebouwd op de [Windows HTTP-Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx). Dit maakt gebruik van de *http.sys* kernelstuurprogramma dat door IIS wordt gebruikt voor het HTTP-aanvragen verwerken en ze doorsturen aan processen waarop webtoepassingen worden uitgevoerd. Hiermee kunt meerdere processen op de dezelfde fysieke of virtuele machine om webtoepassingen te hosten op dezelfde poort, disambiguated door een unieke URL-pad of de hostnaam. Deze functies zijn nuttig bij het Service Fabric voor het hosten van meerdere websites in hetzelfde cluster.

>[!NOTE]
>HttpSys implementatie werkt alleen op Windows-platform.

Het volgende diagram illustreert hoe HttpSys gebruikt de *http.sys* kernelstuurprogramma op Windows voor het delen van poort:

![http.sys][3]

### <a name="httpsys-in-a-stateless-service"></a>HttpSys in een staatloze service
Gebruik `HttpSys` in een staatloze service, overschrijven de `CreateServiceInstanceListeners` methode en keer terug een `HttpSysCommunicationListener` exemplaar:

```csharp
protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
{
    return new ServiceInstanceListener[]
    {
        new ServiceInstanceListener(serviceContext =>
            new HttpSysCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
                new WebHostBuilder()
                    .UseHttpSys()
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

### <a name="httpsys-in-a-stateful-service"></a>HttpSys in een stateful service

`HttpSysCommunicationListener` op dit moment niet is ontworpen voor gebruik in stateful services vanwege problemen met de onderliggende *http.sys* poort functie voor het delen. Zie voor meer informatie de volgende sectie op dynamische poorttoewijzing met HttpSys. Kestrel is voor stateful services de aanbevolen webserver.

### <a name="endpoint-configuration"></a>Eindpuntconfiguratie

Een `Endpoint` configuratie is vereist voor webservers die gebruikmaken van de Windows HTTP-Server API, met inbegrip van HttpSys. Webservers die gebruikmaken van de API van Windows HTTP-Server moeten eerst de URL met reserveren *http.sys* (dit wordt doorgaans uitgevoerd met de [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) hulpprogramma). Deze actie is vereist verhoogde bevoegdheden die uw services standaard geen hebben. De opties 'http' of 'https' voor de `Protocol` eigenschap van de `Endpoint` configuratie in *ServiceManifest.xml* worden gebruikt om de Service Fabric-runtime voor het registreren van een URL met de opdracht geven om precies te  *HTTP.sys* op uw rekening met de [ *sterk jokerteken* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL-voorvoegsel.

Bijvoorbeeld, om te reserveren `http://+:80` voor een service, de volgende configuratie moet worden gebruikt in ServiceManifest.xml:

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

En de naam van het eindpunt moet worden doorgegeven aan de `HttpSysCommunicationListener` constructor:

```csharp
 new HttpSysCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     return new WebHostBuilder()
         .UseHttpSys()
         .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
         .UseUrls(url)
         .Build();
 })
```

#### <a name="use-httpsys-with-a-static-port"></a>HttpSys gebruiken met een statische poort
Voor het gebruik van een statische poort met HttpSys, geef het poortnummer in de `Endpoint` configuratie:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-httpsys-with-a-dynamic-port"></a>HttpSys gebruiken met een dynamische poort
Voor het gebruik van een dynamisch toegewezen poort met HttpSys, laat de `Port` eigenschap in de `Endpoint` configuratie:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

Een dynamische poort toegewezen door een `Endpoint` configuratie biedt slechts één poort *per hostproces*. Het huidige Service Fabric-hostingmodel kan meerdere exemplaren van de service en/of replica's worden gehost in hetzelfde proces, wat betekent dat elke dezelfde poort delen als toegewezen via de `Endpoint` configuratie. Meerdere exemplaren van HttpSys kunnen delen een poort met behulp van de onderliggende *http.sys* poort delen onderdeel, maar die niet wordt ondersteund door `HttpSysCommunicationListener` vanwege de complicaties die deze voor aanvragen van clients introduceert. Voor gebruik van dynamische poort is Kestrel het aanbevolen webserver.

## <a name="kestrel-in-reliable-services"></a>Kestrel in Reliable Services
Kestrel kan worden gebruikt in een betrouwbare Service door het importeren van de **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet-pakket. Dit pakket bevat `KestrelCommunicationListener`, een implementatie van `ICommunicationListener`, die kunt u een ASP.NET Core WebHost binnen een betrouwbare Service maken met behulp van Kestrel als de webserver.

Kestrel is dat een platformoverschrijdende webserver voor ASP.NET Core op basis van libuv, een platformoverschrijdende asynchrone i/o-bibliotheek. In tegenstelling tot HttpSys, Kestrel gebruikt niet de manager van een gecentraliseerde eindpunt zoals *http.sys*. En in tegenstelling tot HttpSys, Kestrel biedt geen ondersteuning voor poort delen tussen meerdere processen. Elk exemplaar van Kestrel moet een unieke poort gebruiken.

![kestrel][4]

### <a name="kestrel-in-a-stateless-service"></a>Kestrel in een staatloze service
Gebruik `Kestrel` in een staatloze service, overschrijven de `CreateServiceInstanceListeners` methode en keer terug een `KestrelCommunicationListener` exemplaar:

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
Gebruik `Kestrel` in een stateful service, overschrijven de `CreateServiceReplicaListeners` methode en keer terug een `KestrelCommunicationListener` exemplaar:

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

In dit voorbeeld wordt een singleton-instantie van `IReliableStateManager` is opgegeven voor de container WebHost afhankelijkheid injectie. Dit is niet strikt noodzakelijk is, maar dit kunt u gebruiken `IReliableStateManager` en betrouwbare verzamelingen in uw MVC-controller actiemethoden.

Een `Endpoint` configuratienaam is **niet** die `KestrelCommunicationListener` in een stateful service. Dit wordt uitgelegd in de volgende sectie in meer detail.

### <a name="configure-kestrel-to-use-https"></a>Kestrel configureren voor gebruik van HTTPS
Bij het inschakelen van HTTPS met Kestrel in uw service, moet u verschillende opties voor de luisterende instellen.  Update de `ServiceInstanceListener` te gebruiken van een eindpunt EndpointHttps en luistert op een specifieke poort (zoals poort 443). Bij het configureren van de web-host als Kestrel server wilt gebruiken, moet u Kestrel om te luisteren naar IPv6-adressen op alle netwerkinterfaces configureren: 

```csharp
new ServiceInstanceListener(
serviceContext =>
    new KestrelCommunicationListener(
        serviceContext,
        "EndpointHttps",
        (url, listener) =>
        {
            ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting Kestrel on {url}");

            return new WebHostBuilder()
                .UseKestrel(opt =>
                {
                    int port = serviceContext.CodePackageActivationContext.GetEndpoint("EndpointHttps").Port;
                    opt.Listen(IPAddress.IPv6Any, port, listenOptions =>
                    {
                        listenOptions.UseHttps(GetCertificateFromStore());
                        listenOptions.NoDelay = true;
                    });
                })
                .ConfigureAppConfiguration((builderContext, config) =>
                {
                    config.AddJsonFile("appsettings.json", optional: false, reloadOnChange: true);
                })

                .ConfigureServices(
                    services => services
                        .AddSingleton<HttpClient>(new HttpClient())
                        .AddSingleton<FabricClient>(new FabricClient())
                        .AddSingleton<StatelessServiceContext>(serviceContext))
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseStartup<Startup>()
                .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                .UseUrls(url)
                .Build();
        }))
```

Zie voor een compleet voorbeeld gebruikt in een zelfstudie, [Kestrel configureren voor gebruik van HTTPS](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#configure-kestrel-to-use-https).


### <a name="endpoint-configuration"></a>Eindpuntconfiguratie
Een `Endpoint` configuratie is niet vereist als Kestrel gebruiken. 

Kestrel is een eenvoudige zelfstandige webserver; in tegenstelling tot HttpSys (of HttpListener), hoeft deze niet een `Endpoint` configuratie in *ServiceManifest.xml* omdat deze geen registratie voordat u start URL vereist. 

#### <a name="use-kestrel-with-a-static-port"></a>Kestrel voor gebruik met een statische poort
Een statische poort kan worden geconfigureerd de `Endpoint` ServiceManifest.xml-configuratie voor gebruik met Kestrel. Hoewel dit niet strikt noodzakelijk is, biedt twee mogelijke voordelen:
 1. Als de poort niet binnen het bereik valt, wordt deze geopend via de firewall van het besturingssysteem door Service Fabric.
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

#### <a name="use-kestrel-with-a-dynamic-port"></a>Kestrel voor gebruik met een dynamische poort
Kestrel niet gebruiken de automatische poorttoewijzing van de `Endpoint` configuratie in ServiceManifest.xml, omdat automatische poorttoewijzing van een `Endpoint` configuratie wordt een unieke poort per toegewezen *proces host*, en een enkel proces kan meerdere exemplaren van Kestrel bevatten. Aangezien Kestrel biedt geen ondersteuning voor het delen van de poort, werkt dit niet zoals u elke instantie Kestrel moet worden geopend op een unieke poort.

Voor het gebruik van dynamisch toewijzen van poorten met Kestrel, laat de `Endpoint` configuratie in ServiceManifest.xml volledig, en niet door de naam van een eindpunt voor de `KestrelCommunicationListener` constructor:

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

In deze configuratie `KestrelCommunicationListener` wordt automatisch een niet-gebruikte poort selecteren in het poortbereik voor toepassingen.

## <a name="service-fabric-configuration-provider"></a>Service Fabric-Configuratieprovider
Appconfiguratie in ASP.NET Core is gebaseerd op sleutel / waarde-paren worden vastgesteld door de configuratieproviders, lezen [configuratie in ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/) om te begrijpen meer op algemene ASP.NET Core-configuratie-ondersteuning.

Deze sectie beschrijft de Configuratieprovider voor Service Fabric om te integreren met ASP.NET Core-configuratie door het importeren van de `Microsoft.ServiceFabric.AspNetCore.Configuration` NuGet-pakket.

### <a name="addservicefabricconfiguration-startup-extensions"></a>AddServiceFabricConfiguration opstarten-extensies
Nadat u importeren hebt `Microsoft.ServiceFabric.AspNetCore.Configuration` NuGet-pakket, moet u de configuratie van Service Fabric-bron registreren met ASP.NET Core configuratie-API door **AddServiceFabricConfiguration** uitbreidingen in `Microsoft.ServiceFabric.AspNetCore.Configuration` naamruimte op basis van `IConfigurationBuilder`

```csharp
using Microsoft.ServiceFabric.AspNetCore.Configuration;

public Startup(IHostingEnvironment env)
{
    var builder = new ConfigurationBuilder()
        .SetBasePath(env.ContentRootPath)
        .AddJsonFile("appsettings.json", optional: false, reloadOnChange: true)
        .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true)
        .AddServiceFabricConfiguration() // Add Service Fabric configuration settings.
        .AddEnvironmentVariables();
    Configuration = builder.Build();
}

public IConfigurationRoot Configuration { get; }
```

De ASP.NET Core-service hebben nu toegang tot de configuratie-instellingen voor Service Fabric, net als elke andere toepassingsinstellingen. Bijvoorbeeld, kunt u het patroon opties om instellingen te laden in sterk getypeerde objecten.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<MyOptions>(Configuration);  // Strongly typed configuration object.
    services.AddMvc();
}
```
### <a name="default-key-mapping"></a>Standaardsleutel toewijzen
Service Fabric-elementen voor configuratie-provider bevat standaard pakketnaam, de naam van sectie en de naam van de eigenschap samen om te vormen van de configuratie van asp.net core-sleutel van de volgende functie:
```csharp
$"{this.PackageName}{ConfigurationPath.KeyDelimiter}{section.Name}{ConfigurationPath.KeyDelimiter}{property.Name}"
```

Bijvoorbeeld, als u een configuratiepakketten met de naam hebt `MyConfigPackage` met onderstaande inhoud, klikt u vervolgens de configuratiewaarde beschikbaar zullen zijn op ASP.NET Core `IConfiguration` via sleutel *MyConfigPackage:MyConfigSection:MyParameter*
```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">  
  <Section Name="MyConfigSection">
    <Parameter Name="MyParameter" Value="Value1" />
  </Section>  
</Settings>
```
### <a name="service-fabric-configuration-options"></a>Configuratieopties voor service Fabric
Configuratieprovider voor service Fabric ondersteunt ook `ServiceFabricConfigurationOptions` te wijzigen van het standaardgedrag van de toewijzing van de sleutel.

#### <a name="encrypted-settings"></a>Versleutelde instellingen
Service Fabric biedt ondersteuning voor het versleutelen van de instellingen, biedt dit ook ondersteuning voor Service Fabric-Configuratieprovider. Beveiligde volgen door de instellingen voor versleutelde are't descrypted standaard ASP.NET Core-standaard principe `IConfiguration`, de versleutelde waarde worden er opgeslagen in plaats daarvan. Als u wilt de waarde op te slaan in ASP.NET Core IConfiguration niet ontsleutelen kan u DecryptValue-vlag ingesteld op false in `AddServiceFabricConfiguration` extensies als volgt te werk:

```csharp
public Startup()
{
    ICodePackageActivationContext activationContext = FabricRuntime.GetActivationContext();
    var builder = new ConfigurationBuilder()        
        .AddServiceFabricConfiguration(activationContext, (options) => options.DecryptValue = true); // set flag to decrypt the value
    Configuration = builder.Build();
}
```
#### <a name="multiple-configuration-packages"></a>Meerdere configuratiepakketten
Service Fabric biedt ondersteuning voor meerdere configuratiepakketten. Naam van het pakket is standaard opgenomen in de configuratie van de sleutel. U kunt instellen de `IncludePackageName` vlag aan het standaardgedrag wijzigen.
```csharp
public Startup()
{
    ICodePackageActivationContext activationContext = FabricRuntime.GetActivationContext();
    var builder = new ConfigurationBuilder()        
        // exclude package name from key.
        .AddServiceFabricConfiguration(activationContext, (options) => options.IncludePackageName = false); 
    Configuration = builder.Build();
}
```
#### <a name="custom-key-mapping-value-extraction-and-data-population"></a>Aangepaste toewijzing van de sleutel, waarde extractie en invullen van de gegevens
Naast de bovenstaande 2 vlaggen voor het wijzigen van het standaardgedrag Configuratieprovider voor Service Fabric biedt ook ondersteuning voor meer geavanceerde scenario's op aangepast de belangrijkste toewijzing via `ExtractKeyFunc` en pak de waarden via op aangepast `ExtractValueFunc`. U kunt ook wijzigen in de gehele procedure voor het vullen van gegevens uit de Service Fabric-configuratie van ASP.NET Core-configuratie via `ConfigAction`.

De volgende voorbeelden ziet u voor het gebruik van `ConfigAction` om aan te passen invullen van de gegevens.
```csharp
public Startup()
{
    ICodePackageActivationContext activationContext = FabricRuntime.GetActivationContext();
    
    this.valueCount = 0;
    this.sectionCount = 0;
    var builder = new ConfigurationBuilder();
    builder.AddServiceFabricConfiguration(activationContext, (options) =>
        {
            options.ConfigAction = (package, configData) =>
            {
                ILogger logger = new ConsoleLogger("Test", null, false);
                logger.LogInformation($"Config Update for package {package.Path} started");

                foreach (var section in package.Settings.Sections)
                {
                    this.sectionCount++;

                    foreach (var param in section.Parameters)
                    {
                        configData[options.ExtractKeyFunc(section, param)] = options.ExtractValueFunc(section, param);
                        this.valueCount++;
                    }
                }

                logger.LogInformation($"Config Update for package {package.Path} finished");
            };
        });
  Configuration = builder.Build();
}
```
### <a name="configuration-update"></a>Configuratie-Update
Configuratieprovider voor service Fabric biedt ook ondersteuning voor configuratie-Update en kunt u ASP.NET Core `IOptionsMonitor` voor het ontvangen van meldingen voor wachtwoordwijzigingen maar ook met `IOptionsSnapshot` laden van configuratiegegevens. Zie voor meer informatie, [ASP.NET Core mogelijk](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/configuration/options).

Dit wordt standaard ondersteund en er kunnen geen nieuwe codering nodig zijn om in te schakelen van configuratie-update.

## <a name="scenarios-and-configurations"></a>Scenario's en configuraties
In deze sectie worden de volgende scenario's beschreven en vindt u de aanbevolen combinatie van de webserver, poortconfiguratie, Service Fabric-integratie-opties en diverse instellingen om een goed werkende service:
 - Extern blootgesteld stateless ASP.NET Core-service
 - Stateless service van ASP.NET Core intern alleen-lezen
 - Stateful service van ASP.NET Core intern alleen-lezen

Een **extern blootgesteld** service is een App die wordt aangegeven dat een eindpunt bereikbaar is vanaf buiten het cluster, gewoonlijk via een load balancer.

Een **alleen interne** service is een waarvan eindpunt alleen bereikbaar is vanaf binnen het cluster is.

> [!NOTE]
> Stateful service-eindpunten moeten in het algemeen niet worden blootgesteld aan Internet. Clusters die zich achter de load balancers die geen informatie over Service Fabric service-oplossing, zoals Azure Load Balancer, zijn niet beschikbaar stateful services maken, omdat de load balancer niet mogelijk om te zoeken en het routeren van verkeer naar de juiste stateful service-replica. 

### <a name="externally-exposed-aspnet-core-stateless-services"></a>Extern blootgesteld stateless ASP.NET Core-services
Kestrel is de aanbevolen webserver voor front-endservices die externe, internetgerichte HTTP-eindpunten beschikbaar maken. Op Windows, kan HttpSys worden gebruikt om poort delen mogelijkheid waarmee u voor het hosten van meerdere webservices op dezelfde set knooppunten met behulp van dezelfde poort, onderscheiden door de hostnaam of het pad, zonder een front-end-proxy- of -gateway voor HTTP-routering kunt bieden.
 
Wanneer toegang heeft tot het Internet, moet een stateless service een bekende en stabiel-eindpunt dat bereikbaar is via een load balancer gebruiken. Dit is de URL die u aan gebruikers van uw toepassing biedt. De volgende configuratie wordt aanbevolen:

|  |  | **Opmerkingen** |
| --- | --- | --- |
| Webserver | Kestrel | Kestrel is de gewenste webserver, omdat het wordt ondersteund in Windows en Linux. |
| Poortconfiguratie | statisch | Een bekende statische poort moet worden geconfigureerd in de `Endpoints` configuratie van ServiceManifest.xml, zoals 80 voor HTTP en 443 voor HTTPS. |
| ServiceFabricIntegrationOptions | Geen | De `ServiceFabricIntegrationOptions.None` optie moet worden gebruikt bij het configureren van Service Fabric-integratiemiddleware zodat de service niet probeert te valideren van binnenkomende aanvragen voor een unieke id. Externe gebruikers van uw toepassing weet niet de unieke identificerende gegevens die worden gebruikt door de middleware. |
| Aantal instanties | -1 | In standaard use cases, moet het aantal instanties instellen worden ingesteld op "-1" zodat een exemplaar is beschikbaar op alle knooppunten die verkeer van een load balancer ontvangen. |

Als meerdere extern beschikbaar gemaakte services dezelfde set knooppunten delen, kan HttpSys worden gebruikt met een unieke maar stabiel URL-pad. Dit kan worden bereikt door het wijzigen van de URL die is opgegeven tijdens het configureren van IWebHost. Houd er rekening mee dat dit alleen van toepassing op HttpSys.

 ```csharp
 new HttpSysCommunicationListener(serviceContext, "ServiceEndpoint", (url, listener) =>
 {
     url += "/MyUniqueServicePath";
 
     return new WebHostBuilder()
         .UseHttpSys()
         ...
         .UseUrls(url)
         .Build();
 })
 ```

### <a name="internal-only-stateless-aspnet-core-service"></a>Alleen-intern stateless ASP.NET Core-service
Stateless services die binnen het cluster alleen worden aangeroepen vanuit de unieke URL's moeten gebruiken en dynamisch toegewezen poorten om te controleren of de samenwerking tussen meerdere services. De volgende configuratie wordt aanbevolen:

|  |  | **Opmerkingen** |
| --- | --- | --- |
| Webserver | Kestrel | Hoewel HttpSys kunnen worden gebruikt voor interne stateless services, is Kestrel de aanbevolen om toe te staan van meerdere exemplaren van de service voor het delen van een host.  |
| Poortconfiguratie | dynamisch wordt toegewezen | Meerdere replica's van een stateful service kunnen een hostproces of het hostbesturingssysteem delen en dus moet unieke poorten. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Met dynamische poorttoewijzing, deze instelling voorkomt u dat het onjuiste identiteit probleem die eerder zijn beschreven. |
| InstanceCount | willekeurig | Het aantal instanties instelling kan worden ingesteld op een willekeurige waarde die nodig zijn om de service. |

### <a name="internal-only-stateful-aspnet-core-service"></a>Alleen-intern stateful ASP.NET Core-service
Stateful services die alleen worden opgeroepen binnen het cluster moeten dynamisch toegewezen poorten gebruiken om te zorgen voor samenwerking tussen meerdere services. De volgende configuratie wordt aanbevolen:

|  |  | **Opmerkingen** |
| --- | --- | --- |
| Webserver | Kestrel | De `HttpSysCommunicationListener` is niet bedoeld voor gebruik door stateful services waarin de replica's een hostproces delen. |
| Poortconfiguratie | dynamisch wordt toegewezen | Meerdere replica's van een stateful service kunnen een hostproces of het hostbesturingssysteem delen en dus moet unieke poorten. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Met dynamische poorttoewijzing, deze instelling voorkomt u dat het onjuiste identiteit probleem die eerder zijn beschreven. |

## <a name="next-steps"></a>Volgende stappen
[Foutopsporing uitvoeren in uw Service Fabric-toepassing met behulp van Visual Studio](service-fabric-debugging-your-application.md)

<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
