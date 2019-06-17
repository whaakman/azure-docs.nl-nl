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
ms.openlocfilehash: 638c06e1854504dcb7ff34b1d9df56694556c421
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64939781"
---
# <a name="aspnet-core-in-azure-service-fabric-reliable-services"></a>ASP.NET Core in Azure Service Fabric Reliable Services

ASP.NET Core is een open-source en platformoverschrijdende framework. Dit framework is ontworpen voor het bouwen van cloud-gebaseerde, met internet verbonden toepassingen, zoals web-apps, IoT-apps en mobiele back-ends.

In dit artikel is een uitgebreide handleiding voor het hosten van ASP.NET Core-services in Service Fabric Reliable Services met behulp van de **Microsoft.ServiceFabric.AspNetCore.** set van NuGet-pakketten.

Zie voor een inleidende zelfstudie voor ASP.NET Core in Service Fabric en instructies voor het beheer van uw ontwikkelomgeving instellen [zelfstudie: Maken en implementeren van een toepassing met een ASP.NET Core Web-API front-end service en een stateful back-endservice](service-fabric-tutorial-create-dotnet-app.md).

De rest van dit artikel wordt ervan uitgegaan dat u al bekend bent met ASP.NET Core. Als dit niet het geval is, lees u via de [basisprincipes van ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/index).

## <a name="aspnet-core-in-the-service-fabric-environment"></a>ASP.NET Core in de Service Fabric-omgeving

ASP.NET Core en Service Fabric-apps kunnen worden uitgevoerd op .NET Core of .NET Framework volledig. U kunt ASP.NET Core gebruiken in twee verschillende manieren in Service Fabric:
 - **Als een Gast kan worden uitgevoerd wordt gehost**. Op deze manier wordt voornamelijk gebruikt om de bestaande ASP.NET Core-toepassingen uitvoeren in Service Fabric met zonder code te wijzigen.
 - **Uitgevoerd in een betrouwbare service**. Op deze manier zorgt voor een betere integratie met de Service Fabric-runtime en stateful ASP.NET Core, kunnen services.

De rest van dit artikel wordt uitgelegd hoe u ASP.NET Core binnen een betrouwbare service, met de onderdelen van de ASP.NET Core-integratie die worden geleverd met de Service Fabric SDK gebruiken.

## <a name="service-fabric-service-hosting"></a>Hosting van service Fabric-service

In Service Fabric, een of meer exemplaren en/of replica's van uw service worden uitgevoerd in een *host serviceproces*: een uitvoerbaar bestand dat uw servicecode wordt uitgevoerd. U, als de auteur van een service, de eigenaar het hostproces van de service en Service Fabric wordt geactiveerd en controleert u het voor u.

Traditionele ASP.NET (maximaal MVC 5) is nauw gekoppeld aan IIS via System.Web.dll. ASP.NET Core biedt een scheiding tussen de webserver en uw web-App. Dankzij deze scheiding kunt webtoepassingen overdraagbaar tussen verschillende webservers. Ook kunnen webservers *zelf-hostend*. Dit betekent dat u kunt een webserver starten in uw eigen proces, in plaats van een proces dat eigendom van specifieke web server-software, zoals IIS.

Als u wilt combineren van een Service Fabric-service en ASP.NET, hetzij als een Gast kan worden uitgevoerd of in een betrouwbare service, moet u ASP.NET binnen uw host-serviceproces gestart. ASP.NET Core zelf die als host fungeert, kunt u om dit te doen.

## <a name="hosting-aspnet-core-in-a-reliable-service"></a>ASP.NET Core in een betrouwbare service die als host fungeert
Normaal gesproken zelf-hostend ASP.NET Core-toepassingen maken een WebHost in het toegangspunt van een toepassing, zoals de `static void Main()` methode in `Program.cs`. In dit geval is de levenscyclus van de WebHost gebonden aan de levenscyclus van het proces.

![Hosting van ASP.NET Core in een proces][0]

Maar het toegangspunt dat van toepassing is niet de juiste plaats voor het maken van een WebHost in een betrouwbare service. Dat komt doordat het toegangspunt voor de toepassing wordt alleen gebruikt voor het registreren van een servicetype met de Service Fabric-runtime, zodat deze exemplaren van het type van deze service kunt maken. De WebHost moet worden gemaakt in een betrouwbare service zelf. Binnen het hostproces van de service gaat service-exemplaren en/of replica's via meerdere productlevenscyclus. 

Een betrouwbare Service-exemplaar wordt vertegenwoordigd door uw serviceklasse die is afgeleid van `StatelessService` of `StatefulService`. De communicatiestack voor een service is opgenomen in een `ICommunicationListener` -implementatie in uw serviceklasse. De `Microsoft.ServiceFabric.AspNetCore.*` NuGet-pakketten bevatten implementaties van `ICommunicationListener` die starten en beheren van de ASP.NET Core WebHost voor Kestrel of HTTP.sys in een betrouwbare service.

![Diagram voor het hosten van ASP.NET Core in een betrouwbare service][1]

## <a name="aspnet-core-icommunicationlisteners"></a>ASP.NET Core ICommunicationListeners
De `ICommunicationListener` implementaties voor Kestrel en HTTP.sys in de `Microsoft.ServiceFabric.AspNetCore.*` NuGet-pakketten zijn vergelijkbaar gebruikspatronen. Maar ze iets andere acties die specifiek zijn voor elke webserver uitvoeren. 

Beide communicatielisteners bieden een constructor die de volgende argumenten neemt:
 - **`ServiceContext serviceContext`** : Dit is de `ServiceContext` object dat informatie over de service bevat.
 - **`string endpointName`** : Dit is de naam van een `Endpoint` configuratie in ServiceManifest.xml. Het is vooral waar de twee communicatielisteners verschillen. HTTP.sys *vereist* een `Endpoint` configuratie, terwijl Kestrel niet.
 - **`Func<string, AspNetCoreCommunicationListener, IWebHost> build`** : Dit is een lambda die u implementeert, in die u maakt en retourneren een `IWebHost`. Hiermee kunt u configureren `IWebHost` de manier waarop u dat gewend bent in een ASP.NET Core-toepassing. De lambda biedt een URL die wordt gegenereerd voor u, afhankelijk van de Service Fabric-integratie-opties die u gebruikt en de `Endpoint` configuratie die u opgeeft. U kunt vervolgens wijzigen of gebruikt u die URL om de webserver te starten.

## <a name="service-fabric-integration-middleware"></a>Service Fabric-integratiemiddleware
De `Microsoft.ServiceFabric.AspNetCore` NuGet-pakket bevat de `UseServiceFabricIntegration` uitbreidingsmethode op `IWebHostBuilder` die Service Fabric-bewuste middleware toevoegt. Deze middleware configureert de Kestrel of HTTP.sys `ICommunicationListener` voor het registreren van een unieke service-URL met de Service Fabric Naming-Service. Het valideert aanvragen van clients zodat clients verbinding maken met de juiste service vervolgens. 

Deze stap is nodig om te voorkomen dat clients per ongeluk verbinding maken met de verkeerde service. Dat komt doordat, in een gedeelde-host-omgeving, zoals Service Fabric, meerdere webtoepassingen kunnen uitvoeren op de dezelfde fysieke of virtuele machine maar unieke hostnamen niet gebruiken. In dit scenario wordt beschreven in de volgende sectie in meer detail.

### <a name="a-case-of-mistaken-identity"></a>Een aanvraag van onjuiste identiteit
Service-replica's, ongeacht het protocol, luisteren op een combinatie van unieke IP: poort. Nadat de replica van een service is gestart op een eindpunt IP: poort luistert, rapporteert deze die eindpuntadres aan de Service Fabric Naming-Service. Er, kunnen clients of andere services detecteren. Als services dynamisch toegewezen toepassingspoorten, kan een service-replica hetzelfde IP: poort-eindpunt van een andere service eerder op dezelfde fysieke of virtuele machine tegelijk gebruiken. Dit kan een client deze per ongeluk verbinding maken met de verkeerde service veroorzaken. In dit scenario kan optreden als de volgende reeks gebeurtenissen treedt op:

 1. Service A luistert op 10.0.0.1:30000 via HTTP. 
 2. -Client wordt omgezet Service A en adres 10.0.0.1:30000 opgehaald.
 3. Service A verplaatst naar een ander knooppunt.
 4. Service B wordt geplaatst op 10.0.0.1 en dezelfde poort 30000 coincidentally gebruikt.
 5. Client probeert verbinding maken met een service met in de cache adres 10.0.0.1:30000.
 6. Client is nu verbonden met de service B, geen verbonden met de verkeerde service.

Dit kan leiden tot fouten op willekeurige tijdstippen die kunnen lastig zijn om op te sporen.

### <a name="using-unique-service-urls"></a>Met behulp van unieke service-URL 's
Services kunnen om te voorkomen dat deze bugs, het posten van een eindpunt naar de Naming-Service met een unieke id en vervolgens valideren dat de unieke id tijdens het aanvragen van clients. Dit is een gezamenlijke actie tussen services in een vertrouwde omgeving van niet-schadelijke-tenant. Het biedt geen veilige service-verificatie in een omgeving met onveilige-tenants.

In een vertrouwde omgeving, de middleware die wordt toegevoegd door de `UseServiceFabricIntegration` methode wordt een unieke id automatisch toegevoegd aan het adres dat is verzonden naar de Naming-Service. Gevalideerd die bij elke aanvraag-id. Als komt niet overeen met de id, retourneert de middleware onmiddellijk een respons HTTP 410 geworden.

Services die gebruikmaken van een dynamisch toegewezen poort moet maken gebruik van deze middleware.

Services die gebruikmaken van een vaste unieke poort hebben geen dit probleem in een omgeving met gezamenlijke. Een vaste unieke poort wordt meestal gebruikt voor dergelijke services die een bekende poort voor clienttoepassingen verbinding maken met nodig hebt. De meeste internet gerichte webtoepassingen gebruikt bijvoorbeeld poort 80 of 443 voor web browser verbindingen. In dit geval wordt mag niet de unieke id worden ingeschakeld.

Het volgende diagram toont de aanvraagstroom met de middleware ingeschakeld:

![Integratie van service Fabric ASP.NET Core][2]

Kestrel zowel HTTP.sys `ICommunicationListener` implementaties dit mechanisme op exact dezelfde manier gebruiken. Hoewel HTTP.sys intern onderscheid maken aanvragen op basis van unieke URL-paden tussen met behulp van de onderliggende **HTTP.sys** poort functie, die de functionaliteit is voor het delen *niet* die worden gebruikt door het HTTP.sys `ICommunicationListener`implementatie. Dat komt doordat dit resulteert in een HTTP 503- en HTTP 404-fout statuscodes in de eerder beschreven scenario. Die op zijn beurt waardoor het lastig voor clients om te bepalen het doel van de fout, zoals HTTP 503- en HTTP 404 vaak gebruikt worden om aan te geven van andere fouten. 

Dus zowel Kestrel en HTTP.sys `ICommunicationListener` implementaties is Apparaatgebruik zonder middleware geleverd door de `UseServiceFabricIntegration` uitbreidingsmethode. Clients moeten daarom alleen actie uit te voeren een service-eindpunt opnieuw omzetten 410 HTTP-antwoord.

## <a name="httpsys-in-reliable-services"></a>HTTP.sys in Reliable Services
U kunt HTTP.sys in Reliable Services door het importeren van de **Microsoft.ServiceFabric.AspNetCore.HttpSys** NuGet-pakket. Dit pakket bevat `HttpSysCommunicationListener`, een implementatie van `ICommunicationListener`. `HttpSysCommunicationListener` Hiermee kunt u een ASP.NET Core WebHost binnen een betrouwbare service maakt met behulp van HTTP.sys als de webserver.

HTTP.sys is gebouwd op de [Windows HTTP-Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510(v=vs.85).aspx). Deze API maakt gebruik van de **HTTP.sys** kernelstuurprogramma HTTP-aanvragen verwerken en ze doorsturen naar processen die worden uitgevoerd van webtoepassingen. Hiermee kunt meerdere processen op de dezelfde fysieke of virtuele machine om webtoepassingen te hosten op dezelfde poort, disambiguated door een unieke URL-pad of de hostnaam naam. Deze functies zijn nuttig bij het Service Fabric voor het hosten van meerdere websites in hetzelfde cluster.

>[!NOTE]
>HTTP.sys-implementatie werkt alleen op het Windows-platform.

Het volgende diagram illustreert hoe HTTP.sys gebruikt de **HTTP.sys** kernelstuurprogramma op Windows voor het delen van poort:

![HTTP.sys-diagram][3]

### <a name="httpsys-in-a-stateless-service"></a>HTTP.sys in een staatloze service
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

### <a name="httpsys-in-a-stateful-service"></a>HTTP.sys in een stateful service

`HttpSysCommunicationListener` op dit moment is niet bedoeld voor gebruik in stateful services vanwege problemen met de onderliggende **HTTP.sys** poort functie voor het delen. Zie voor meer informatie de volgende sectie op dynamische poorttoewijzing met HTTP.sys. Kestrel is voor stateful services de voorgestelde webserver.

### <a name="endpoint-configuration"></a>Eindpuntconfiguratie

Een `Endpoint` configuratie is vereist voor webservers die gebruikmaken van de Windows HTTP-Server API, met inbegrip van HTTP.sys. Webservers die gebruikmaken van de API van Windows HTTP-Server moeten eerst de URL met HTTP.sys reserveren (dit wordt doorgaans uitgevoerd met de [netsh](https://msdn.microsoft.com/library/windows/desktop/cc307236(v=vs.85).aspx) hulpprogramma). 

Deze actie is vereist verhoogde bevoegdheden die de services geldt geen standaard. De opties 'http' of 'https' voor de `Protocol` eigenschap van de `Endpoint` configuratie in ServiceManifest.xml specifiek gebruikt om toegang te geven van de Service Fabric-runtime voor het registreren van een URL met HTTP.sys namens u worden gebruikt. Dit gebeurt met behulp van de [ *sterk jokerteken* ](https://msdn.microsoft.com/library/windows/desktop/aa364698(v=vs.85).aspx) URL-voorvoegsel.

Bijvoorbeeld, om te reserveren `http://+:80` voor een service, gebruikt u de volgende configuratie in ServiceManifest.xml:

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

#### <a name="use-httpsys-with-a-static-port"></a>HTTP.sys gebruiken met een statische poort
Voor het gebruik van een statische poort met HTTP.sys, geef het poortnummer in de `Endpoint` configuratie:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" Port="80" />
    </Endpoints>
  </Resources>
```

#### <a name="use-httpsys-with-a-dynamic-port"></a>HTTP.sys gebruiken met een dynamische poort
Voor het gebruik van een dynamisch toegewezen poort met HTTP.sys, laat de `Port` eigenschap in de `Endpoint` configuratie:

```xml
  <Resources>
    <Endpoints>
      <Endpoint Protocol="http" Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
```

Een dynamische poort toegewezen door een `Endpoint` configuratie biedt slechts één poort *per hostproces*. Het huidige Service Fabric-hostingmodel kan meerdere exemplaren van de service en/of replica's in hetzelfde proces worden gehost. Dit betekent dat elke delen dezelfde poort als toegewezen via de `Endpoint` configuratie. Meerdere **HTTP.sys** exemplaren een poort kunnen delen met behulp van de onderliggende **HTTP.sys** poort functie voor het delen. Maar dit wordt niet ondersteund door `HttpSysCommunicationListener` vanwege de complicaties die deze voor aanvragen van clients introduceert. Voor gebruik van dynamische poort is Kestrel het voorgestelde webserver.

## <a name="kestrel-in-reliable-services"></a>Kestrel in Reliable Services
U kunt Kestrel in Reliable Services door het importeren van de **Microsoft.ServiceFabric.AspNetCore.Kestrel** NuGet-pakket. Dit pakket bevat `KestrelCommunicationListener`, een implementatie van `ICommunicationListener`. `KestrelCommunicationListener` Hiermee kunt u een ASP.NET Core WebHost binnen een betrouwbare service maakt met behulp van Kestrel als de webserver.

Kestrel is dat een platformoverschrijdende webserver voor ASP.NET Core op basis van libuv, een platformoverschrijdende asynchrone i/o-bibliotheek. In tegenstelling tot HTTP.sys, niet Kestrel een gecentraliseerde endpoint-manager gebruikt. Ook in tegenstelling tot HTTP.sys, Kestrel biedt geen ondersteuning voor poort delen tussen meerdere processen. Elk exemplaar van Kestrel moet een unieke poort gebruiken.

![Kestrel diagram][4]

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

In dit voorbeeld wordt een singleton-instantie van `IReliableStateManager` is opgegeven voor de container WebHost afhankelijkheid injectie. Dit is niet strikt noodzakelijk is, maar kunt u gebruikmaken van `IReliableStateManager` en betrouwbare verzamelingen in uw MVC-controller actiemethoden.

Een `Endpoint` configuratienaam is *niet* die `KestrelCommunicationListener` in een stateful service. Dit wordt uitgelegd in de volgende sectie in meer detail.

### <a name="configure-kestrel-to-use-https"></a>Kestrel configureren voor gebruik van HTTPS
Bij het inschakelen van HTTPS met Kestrel in uw service, moet u verschillende opties voor de luisterende instellen. Update de `ServiceInstanceListener` gebruiken een *EndpointHttps* eindpunt en luistert op een specifieke poort (bijvoorbeeld poort 443). Wanneer u de web-host voor het gebruik van de webserver Kestrel configureert, moet u Kestrel om te luisteren naar IPv6-adressen op alle netwerkinterfaces configureren: 

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

Zie voor een compleet voorbeeld van een zelfstudie [Kestrel configureren voor gebruik van HTTPS](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#configure-kestrel-to-use-https).


### <a name="endpoint-configuration"></a>Eindpuntconfiguratie
Een `Endpoint` configuratie is niet vereist voor het gebruik van Kestrel. 

Kestrel is een eenvoudige losse-webserver. In tegenstelling tot HTTP.sys (of HttpListener), de eigenschap hoeft niet een `Endpoint` configuratie in ServiceManifest.xml omdat er is geen URL registratie voordat u begint vereist. 

#### <a name="use-kestrel-with-a-static-port"></a>Kestrel voor gebruik met een statische poort
U kunt een statische poort in configureren de `Endpoint` ServiceManifest.xml-configuratie voor gebruik met Kestrel. Hoewel dit niet strikt noodzakelijk is, biedt deze twee mogelijke voordelen:
 - Als de poort niet in het bereik valt, wordt deze geopend via de firewall van het besturingssysteem door Service Fabric.
 - De URL die u via `KestrelCommunicationListener` deze poort wordt gebruikt.

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

Als u geen gebruik maakt van ServiceManifest.xml een `Endpoint` configuratie, laat u de naam in de `KestrelCommunicationListener` constructor. In dit geval wordt een dynamische poort gebruikt. Zie de volgende sectie voor meer informatie hierover.

#### <a name="use-kestrel-with-a-dynamic-port"></a>Kestrel voor gebruik met een dynamische poort
De automatische poorttoewijzing uit kan niet worden gebruikt door kestrel het `Endpoint` configuratie in ServiceManifest.xml. Dat komt omdat automatische toewijzing van poort een `Endpoint` configuratie wordt een unieke poort per toegewezen *proces host*, en een afzonderlijke host-proces kan meerdere Kestrel exemplaren bevatten. Dit werkt niet met Kestrel omdat deze biedt geen ondersteuning voor het delen van poorten. Elk exemplaar Kestrel moet daarom een unieke poort worden geopend.

Voor het gebruik van dynamisch toewijzen van poorten met Kestrel, laat de `Endpoint` configuratie in ServiceManifest.xml volledig, en niet door de naam van een eindpunt op te geven de `KestrelCommunicationListener` constructor, als volgt te werk:

```csharp
new KestrelCommunicationListener(serviceContext, (url, listener) => ...
```

In deze configuratie `KestrelCommunicationListener` wordt automatisch een niet-gebruikte poort selecteren in het poortbereik voor toepassingen.

## <a name="service-fabric-configuration-provider"></a>Service Fabric-configuratieprovider
App-configuratie in ASP.NET Core is gebaseerd op sleutel / waarde-paren tot stand gebracht door de configuratieprovider. Lezen [configuratie in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/) om te begrijpen meer op algemene ASP.NET Core-configuratie-ondersteuning.

Deze sectie wordt beschreven hoe de provider van de configuratie van Service Fabric is geïntegreerd met ASP.NET Core-configuratie door het importeren van de `Microsoft.ServiceFabric.AspNetCore.Configuration` NuGet-pakket.

### <a name="addservicefabricconfiguration-startup-extensions"></a>AddServiceFabricConfiguration opstarten extensies
Na het importeren de `Microsoft.ServiceFabric.AspNetCore.Configuration` NuGet-pakket, moet u de configuratie van Service Fabric-bron registreren met ASP.NET Core configuratie-API. U dit doen door het controleren van **AddServiceFabricConfiguration** -extensies in de `Microsoft.ServiceFabric.AspNetCore.Configuration` naamruimte tegen `IConfigurationBuilder`.

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

Nu de ASP.NET Core-service toegang tot de Service Fabric-configuratie-instellingen, net als elke andere toepassingsinstellingen. Bijvoorbeeld, kunt u het patroon opties om instellingen te laden in sterk getypeerde objecten.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<MyOptions>(Configuration);  // Strongly typed configuration object.
    services.AddMvc();
}
```
### <a name="default-key-mapping"></a>Belangrijkste standaardtoewijzing
De configuratieprovider voor de Service Fabric omvat standaard de pakketnaam, de naam van sectie en de naam van eigenschap. Samen vormen deze de sleutel van de configuratie van ASP.NET Core als volgt:
```csharp
$"{this.PackageName}{ConfigurationPath.KeyDelimiter}{section.Name}{ConfigurationPath.KeyDelimiter}{property.Name}"
```

Bijvoorbeeld, als u een configuratiepakket met de naam hebben `MyConfigPackage` met de volgende inhoud en klikt u vervolgens de configuratiewaarde beschikbaar zullen zijn op ASP.NET Core `IConfiguration` via *MyConfigPackage:MyConfigSection:MyParameter*.
```xml
<?xml version="1.0" encoding="utf-8" ?>
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">  
  <Section Name="MyConfigSection">
    <Parameter Name="MyParameter" Value="Value1" />
  </Section>  
</Settings>
```
### <a name="service-fabric-configuration-options"></a>Service Fabric-configuratie-opties
De provider van de configuratie van Service Fabric ondersteunt ook `ServiceFabricConfigurationOptions` te wijzigen van het standaardgedrag van de toewijzing van de sleutel.

#### <a name="encrypted-settings"></a>Versleutelde instellingen
Service Fabric ondersteunt versleutelde instellingen, zoals de configuratieprovider voor de Service Fabric. De versleutelde instellingen niet worden ontsleuteld naar ASP.NET Core `IConfiguration` standaard. De versleutelde waarden worden opgeslagen in plaats daarvan. Maar als u de waarde op te slaan in ASP.NET Core IConfiguration niet ontsleutelen wilt, kunt u instellen de *DecryptValue* vlag op false in de `AddServiceFabricConfiguration` extensie, als volgt te werk:

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
Service Fabric biedt ondersteuning voor meerdere configuratiepakketten. Naam van het pakket is standaard opgenomen in de configuratiesleutel. Maar u kunt de `IncludePackageName` vlag in op false, als volgt:
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
De configuratieprovider voor de Service Fabric biedt ook ondersteuning voor meer geavanceerde scenario's voor het aanpassen van de belangrijkste toewijzing met `ExtractKeyFunc` en de waarden met aangepaste-extract `ExtractValueFunc`. U kunt ook het hele proces van het invullen van gegevens uit de Service Fabric-configuratie van ASP.NET Core-configuratie met behulp van wijzigen `ConfigAction`.

De volgende voorbeelden ziet u hoe u `ConfigAction` om aan te passen invullen van de gegevens:
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

### <a name="configuration-updates"></a>Configuratie-updates
De configuratieprovider voor de Service Fabric biedt ook ondersteuning voor configuratie-updates. Kunt u ASP.NET Core `IOptionsMonitor` meldingen voor wachtwoordwijzigingen ontvangen en gebruik vervolgens `IOptionsSnapshot` laden van configuratiegegevens. Zie voor meer informatie, [ASP.NET Core-opties](https://docs.microsoft.com/aspnet/core/fundamentals/configuration/options).

Deze opties worden standaard ondersteund. Er kunnen geen nieuwe code is nodig om in te schakelen van configuratie-updates.

## <a name="scenarios-and-configurations"></a>Scenario's en configuraties
Deze sectie bevat de combinatie van de webserver, poortconfiguratie, Service Fabric-integratie-opties en diverse instellingen die wij aanbevelen voor het oplossen van de volgende scenario's:
 - Extern blootgesteld stateless ASP.NET Core-services
 - Alleen-intern ASP.NET Core staatloze services
 - Alleen-intern ASP.NET Core stateful services

Een **extern beschikbaar gesteld service** is een App die wordt aangegeven dat een eindpunt dat wordt aangeroepen vanuit buiten het cluster, gewoonlijk via een load balancer.

Een **alleen interne** service is een waarvan eindpunt alleen vanuit binnen het cluster aangeroepen wordt.

> [!NOTE]
> Stateful service-eindpunten mag niet in het algemeen worden blootgesteld aan internet. Clusters achter load balancers die geen informatie over Service Fabric service-oplossing, zoals Azure Load Balancer, zijn geen stateful services blootstellen worden. Dat komt doordat de load balancer is niet mogelijk om te zoeken en het routeren van verkeer naar de juiste stateful service-replica. 

### <a name="externally-exposed-aspnet-core-stateless-services"></a>Extern blootgesteld stateless ASP.NET Core-services
Kestrel is de voorgestelde webserver voor front-endservices die externe, internetgerichte HTTP-eindpunten beschikbaar maken. Op Windows krijgt u HTTP.sys poort delen-optie waarmee u voor het hosten van meerdere webservices op dezelfde set knooppunten met behulp van dezelfde poort. In dit scenario, de webservices van elkaar worden onderscheiden door de hostnaam of het pad zonder afhankelijkheid van een front-end-proxy- of -gateway voor HTTP-routering.
 
Wanneer toegang heeft tot het internet, moet een stateless service een bekende en stabiel-eindpunt dat bereikbaar is via een load balancer gebruiken. U vindt deze URL voor gebruikers van uw toepassing. U wordt aangeraden de volgende configuratie:

|  |  | **Opmerkingen** |
| --- | --- | --- |
| Webserver | Kestrel | Kestrel is de gewenste webserver, omdat het wordt ondersteund in Windows en Linux. |
| Poortconfiguratie | Statische | Een bekende statische poort moet worden geconfigureerd in de `Endpoints` configuratie van ServiceManifest.xml, zoals 80 voor HTTP en 443 voor HTTPS. |
| ServiceFabricIntegrationOptions | Geen | Gebruik de `ServiceFabricIntegrationOptions.None` optie bij het configureren van Service Fabric-integratiemiddleware, zodat de service niet proberen om te valideren van binnenkomende aanvragen voor een unieke id. Externe gebruikers van uw toepassing kennen niet de unieke identificatiegegevens die gebruikmaakt van de middleware. |
| Aantal instanties | -1 | In de standaard use cases, het aantal instanties instelling moet worden ingesteld op *-1*. Dit wordt gedaan om een exemplaar is beschikbaar op alle knooppunten die verkeer van een load balancer ontvangen. |

Als meerdere extern beschikbaar gemaakte services dezelfde set knooppunten delen, kunt u HTTP.sys gebruiken met een unieke maar stabiel URL-pad. U kunt dit doen door het wijzigen van de URL die is opgegeven tijdens het configureren van IWebHost. Houd er rekening mee dat dit alleen van toepassing HTTP.sys is.

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
Stateless services die binnen het cluster alleen worden aangeroepen vanuit de unieke URL's moeten gebruiken en dynamisch toegewezen poorten om te controleren of de samenwerking tussen meerdere services. U wordt aangeraden de volgende configuratie:

|  |  | **Opmerkingen** |
| --- | --- | --- |
| Webserver | Kestrel | Hoewel u HTTP.sys voor interne stateless services gebruiken kunt, is Kestrel de aanbevolen om toe te staan van meerdere exemplaren van de service voor het delen van een host.  |
| Poortconfiguratie | dynamisch wordt toegewezen | Meerdere replica's van een stateful service deelt een proces of het hostbesturingssysteem en dus moet unieke poorten. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Met dynamische poorttoewijzing, deze instelling voorkomt u dat het onjuiste identiteit probleem die eerder zijn beschreven. |
| InstanceCount | Alle | Het aantal instanties instelling kan worden ingesteld op een willekeurige waarde die nodig zijn om de service. |

### <a name="internal-only-stateful-aspnet-core-service"></a>Alleen-intern stateful ASP.NET Core-service
Stateful services die alleen worden opgeroepen binnen het cluster moeten dynamisch toegewezen poorten gebruiken om te zorgen voor samenwerking tussen meerdere services. U wordt aangeraden de volgende configuratie:

|  |  | **Opmerkingen** |
| --- | --- | --- |
| Webserver | Kestrel | De `HttpSysCommunicationListener` is niet bedoeld voor gebruik door stateful services waarin de replica's een hostproces delen. |
| Poortconfiguratie | dynamisch wordt toegewezen | Meerdere replica's van een stateful service deelt een proces of het hostbesturingssysteem en dus moet unieke poorten. |
| ServiceFabricIntegrationOptions | UseUniqueServiceUrl | Met dynamische poorttoewijzing, deze instelling voorkomt u dat het onjuiste identiteit probleem die eerder zijn beschreven. |

## <a name="next-steps"></a>Volgende stappen
[Foutopsporing uitvoeren in uw Service Fabric-toepassing met behulp van Visual Studio](service-fabric-debugging-your-application.md)


<!--Image references-->
[0]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-standalone.png
[1]:./media/service-fabric-reliable-services-communication-aspnetcore/webhost-servicefabric.png
[2]:./media/service-fabric-reliable-services-communication-aspnetcore/integration.png
[3]:./media/service-fabric-reliable-services-communication-aspnetcore/httpsys.png
[4]:./media/service-fabric-reliable-services-communication-aspnetcore/kestrel.png
