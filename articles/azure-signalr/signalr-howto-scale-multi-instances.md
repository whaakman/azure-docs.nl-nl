---
title: Schalen met meerdere instanties voor Azure SignalR Service
description: "In veel scenario's vergroten/verkleinen moet klant vaak meerdere exemplaren richten en te configureren voor het gebruik ervan samen om een grootschalige implementatie te maken. Bijvoorbeeld: sharding heeft dat meerdere exemplaren ondersteunen."
author: sffamily
ms.service: signalr
ms.topic: conceptual
ms.date: 03/27/2019
ms.author: zhshang
ms.openlocfilehash: e284a0492774e02cab79db6d9006c1718a7fcfc9
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58623181"
---
# <a name="how-to-scale-signalr-service-with-multiple-instances"></a>Hoe u SignalR-Service met meerdere exemplaren schalen?
De meest recente SignalR Service-SDK biedt ondersteuning voor meerdere eindpunten voor SignalR-Service-exemplaren. U kunt deze functie gebruiken voor het schalen van de gelijktijdige verbindingen, of deze gebruiken voor de regio-overschrijdende messaging.

## <a name="for-aspnet-core"></a>Voor ASP.NET Core

### <a name="how-to-add-multiple-endpoints-from-config"></a>Hoe kan ik meerdere eindpunten toevoegen uit bestand config?

Configuratie met sleutel `Azure:SignalR:ConnectionString` of `Azure:SignalR:ConnectionString:` voor SignalR-Service-verbindingsreeks.

Als de sleutel met begint `Azure:SignalR:ConnectionString:`, moet de indeling `Azure:SignalR:ConnectionString:{Name}:{EndpointType}`, waarbij `Name` en `EndpointType` zijn eigenschappen van de `ServiceEndpoint` object en zijn toegankelijk via code.

U kunt meerdere exemplaar tekenreeksen voor de verbinding met de volgende toevoegen `dotnet` opdrachten:

```batch
dotnet user-secrets set Azure:SignalR:ConnectionString:east-region-a <ConnectionString1>
dotnet user-secrets set Azure:SignalR:ConnectionString:east-region-b:primary <ConnectionString2>
dotnet user-secrets set Azure:SignalR:ConnectionString:backup:secondary <ConnectionString3>
```

### <a name="how-to-add-multiple-endpoints-from-code"></a>Hoe kan ik meerdere eindpunten toevoegen vanuit code?

Een `ServicEndpoint` klasse wordt geïntroduceerd om te beschrijven van de eigenschappen van een Azure SignalR Service-eindpunt.
U kunt meerdere exemplaareindpunten configureren bij het gebruik van Azure SignalR Service-SDK via:
```cs
services.AddSignalR()
        .AddAzureSignalR(options => 
        {
            options.Endpoints = new ServiceEndpoint[]
            {
                // Note: this is just a demonstration of how to set options.Endpoints
                // Having ConnectionStrings explicitly set inside the code is not encouraged
                // You can fetch it from a safe place such as Azure KeyVault
                new ServiceEndpoint("<ConnectionString0>"),
                new ServiceEndpoint("<ConnectionString1>", type: EndpointType.Primary, name: "east-region-a"),
                new ServiceEndpoint("<ConnectionString2>", type: EndpointType.Primary, name: "east-region-b"),
                new ServiceEndpoint("<ConnectionString3>", type: EndpointType.Secondary, name: "backup"),
            };
        });
```

### <a name="how-to-customize-endpoint-router"></a>Over het aanpassen van de router van het eindpunt?

De SDK gebruikt standaard de [DefaultEndpointRouter](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR/EndpointRouters/DefaultEndpointRouter.cs) om op te halen eindpunten.

#### <a name="default-behavior"></a>Standaardgedrag 
1. Client aangevraagde routering

    Wanneer clients `/negotiate` met de app-server. Standaard-SDK **selecteert willekeurig** één eindpunt van de set beschikbare service-eindpunten.

2. Server-berichtroutering

    Als * bericht verzenden naar een specifieke ** verbinding *** en de doelverbinding wordt doorgestuurd naar de huidige server, gaat het bericht rechtstreeks naar dat eindpunt verbonden. Anders worden de berichten worden verzonden naar elke Azure SignalR-eindpunt.

#### <a name="customize-routing-algorithm"></a>Routering algoritme aanpassen
Wanneer u speciale kennis om te bepalen welke de berichten u naar gaat moeten eindpunten hebt, kunt u uw eigen router maken.

Een aangepaste router als voorbeeld hieronder wordt gedefinieerd wanneer groepen die beginnen met `east-` altijd gaat u naar het eindpunt met de naam `east`:

```cs
private class CustomRouter : EndpointRouterDecorator
{
    public override IEnumerable<ServiceEndpoint> GetEndpointsForGroup(string groupName, IEnumerable<ServiceEndpoint> endpoints)
    {
        // Override the group broadcast behavior, if the group name starts with "east-", only send messages to endpoints inside east
        if (groupName.StartsWith("east-"))
        {
            return endpoints.Where(e => e.Name.StartsWith("east-"));
        }

        return base.GetEndpointsForGroup(groupName, endpoints);
    }
}
```

Een ander voorbeeld hieronder, die standaard overschrijft onderhandelen gedrag, om te selecteren van de eindpunten is afhankelijk van waar de app-server zich bevindt.

```cs
private class CustomRouter : EndpointRouterDecorator
{
    public override ServiceEndpoint GetNegotiateEndpoint(HttpContext context, IEnumerable<ServiceEndpoint> endpoints)
    {
        // Override the negotiate behavior to get the endpoint from query string
        var endpointName = context.Request.Query["endpoint"];
        if (endpointName.Count == 0)
        {
            context.Response.StatusCode = 400;
            var response = Encoding.UTF8.GetBytes("Invalid request");
            context.Response.Body.Write(response, 0, response.Length);
            return null;
        }

        return endpoints.FirstOrDefault(s => s.Name == endpointName && s.Online) // Get the endpoint with name matching the incoming request
               ?? base.GetNegotiateEndpoint(context, endpoints); // Or fallback to the default behavior to randomly select one from primary endpoints, or fallback to secondary when no primary ones are online
    }
}
```

Vergeet niet om u te registreren van de router voor het gebruik van DI-container:

```cs
services.AddSingleton(typeof(IEndpointRouter), typeof(CustomRouter));
services.AddSignalR()
        .AddAzureSignalR(
            options => 
            {
                options.Endpoints = new ServiceEndpoint[]
                {
                    new ServiceEndpoint(name: "east", connectionString: "<connectionString1>"),
                    new ServiceEndpoint(name: "west", connectionString: "<connectionString2>"),
                    new ServiceEndpoint("<connectionString3>")
                };
            });
```

## <a name="for-aspnet"></a>Voor ASP.NET

### <a name="how-to-add-multiple-endpoints-from-config"></a>Hoe kan ik meerdere eindpunten toevoegen uit bestand config?

Configuratie met sleutel `Azure:SignalR:ConnectionString` of `Azure:SignalR:ConnectionString:` voor SignalR-Service-verbindingsreeks.

Als de sleutel met begint `Azure:SignalR:ConnectionString:`, moet de indeling `Azure:SignalR:ConnectionString:{Name}:{EndpointType}`, waarbij `Name` en `EndpointType` zijn eigenschappen van de `ServiceEndpoint` object en zijn toegankelijk via code.

U kunt meerdere exemplaar verbindingsreeksen naar toevoegen `web.config`:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <connectionStrings>
    <add name="Azure:SignalR:ConnectionString" connectionString="<ConnectionString1>"/>
    <add name="Azure:SignalR:ConnectionString:en-us" connectionString="<ConnectionString2>"/>
    <add name="Azure:SignalR:ConnectionString:zh-cn:secondary" connectionString="<ConnectionString3>"/>
    <add name="Azure:SignalR:ConnectionString:Backup:secondary" connectionString="<ConnectionString4>"/>
  </connectionStrings>
  ...
</configuration>
```

### <a name="how-to-add-multiple-endpoints-from-code"></a>Hoe kan ik meerdere eindpunten toevoegen vanuit code?

Een `ServicEndpoint` klasse wordt geïntroduceerd om te beschrijven van de eigenschappen van een Azure SignalR Service-eindpunt.
U kunt meerdere exemplaareindpunten configureren bij het gebruik van Azure SignalR Service-SDK via:

```cs
app.MapAzureSignalR(
    this.GetType().FullName, 
    options => {
            options.Endpoints = new ServiceEndpoint[]
            {
                // Note: this is just a demonstration of how to set options.Endpoints
                // Having ConnectionStrings explicitly set inside the code is not encouraged
                // You can fetch it from a safe place such as Azure KeyVault
                new ServiceEndpoint("<ConnectionString1>"),
                new ServiceEndpoint("<ConnectionString2>"),
                new ServiceEndpoint("<ConnectionString3>"),
            }
        });
```

### <a name="how-to-customize-router"></a>Over het aanpassen van de router?

Het enige verschil tussen ASP.NET SignalR en ASP.NET Core SignalR is het type HTTP-context voor `GetNegotiateEndpoint`. Voor ASP.NET SignalR, is het van [IOwinContext](https://github.com/Azure/azure-signalr/blob/dev/src/Microsoft.Azure.SignalR.AspNet/EndpointRouters/DefaultEndpointRouter.cs#L19) type.

Hieronder volgt het aangepaste onderhandelen voorbeeld voor ASP.NET SignalR:

```cs
private class CustomRouter : EndpointRouterDecorator
{
    public override ServiceEndpoint GetNegotiateEndpoint(IOwinContext context, IEnumerable<ServiceEndpoint> endpoints)
    {
        // Override the negotiate behavior to get the endpoint from query string
        var endpointName = context.Request.Query["endpoint"];
        if (string.IsNullOrEmpty(endpointName))
        {
            context.Response.StatusCode = 400;
            context.Response.Write("Invalid request.");
            return null;
        }

        return endpoints.FirstOrDefault(s => s.Name == endpointName && s.Online) // Get the endpoint with name matching the incoming request
               ?? base.GetNegotiateEndpoint(context, endpoints); // Or fallback to the default behavior to randomly select one from primary endpoints, or fallback to secondary when no primary ones are online
    }
}
```

Vergeet niet om u te registreren van de router voor het gebruik van DI-container:

```cs
var hub = new HubConfiguration();
var router = new CustomRouter();
hub.Resolver.Register(typeof(IEndpointRouter), () => router);
app.MapAzureSignalR(GetType().FullName, hub, options => {
    options.Endpoints = new ServiceEndpoint[]
                {
                    new ServiceEndpoint(name: "east", connectionString: "<connectionString1>"),
                    new ServiceEndpoint(name: "west", connectionString: "<connectionString2>"),
                    new ServiceEndpoint("<connectionString3>")
                };
});
```

## <a name="configuration-in-cross-region-scenarios"></a>Configuratie in verschillende regio 's

De `ServiceEndpoint` object heeft een `EndpointType` eigenschap met de waarde `primary` of `secondary`.

`primary` eindpunten zijn de eindpunten van voorkeur om clientverkeer te ontvangen en worden beschouwd als betrouwbaardere netwerkverbindingen; `secondary` eindpunten worden beschouwd als minder betrouwbaar netwerkverbindingen en alleen voor de server van de nemen op clientverkeer, bijvoorbeeld berichten uitzenden, niet voor nemen client serververkeer worden gebruikt.

In de regio-overschrijdende gevallen netwerk niet stabiel. Voor één app-server zich bevindt *VS-Oost*, het SignalR-Service-eindpunt zich in dezelfde *VS-Oost* regio kan worden geconfigureerd als `primary` en eindpunten in andere regio's die zijn gemarkeerd als `secondary`. In deze configuratie, service-eindpunten in andere regio's kunnen **ontvangen** berichten van dit *VS-Oost* app-server, maar er is geen **regio-overschrijdende** clients doorgestuurd naar Deze appserver. De architectuur wordt weergegeven in het onderstaande diagram:

![Grensoverschrijdende Infra](./media/signalr-howto-scale-multi-instances/cross_geo_infra.png)

Wanneer een client probeert `/negotiate` met de appserver, met de standaardrouter, SDK **selecteert willekeurig** één eindpunt van de set beschikbare `primary` eindpunten. Wanneer het eindpunt is beschikbaar, klikt u vervolgens SDK **selecteert willekeurig** van alle beschikbare `secondary` eindpunten. Het eindpunt is gemarkeerd als **beschikbaar** wanneer de verbinding tussen server en het service-eindpunt actief is.

In de regio-overschrijdende scenario, wanneer een client probeert `/negotiate` met de app-server die wordt gehost in *VS-Oost*, door standaard deze altijd retourneert de `primary` eindpunt zich in dezelfde regio bevinden. Wanneer alle *VS-Oost* eindpunten zijn niet beschikbaar, de client wordt omgeleid naar de eindpunten in andere regio's. Failover hieronder wordt het scenario in detail beschreven.

![Normaal onderhandelen](./media/signalr-howto-scale-multi-instances/normal_negotiate.png)

## <a name="fail-over"></a>Failover

Wanneer alle `primary` eindpunten zijn niet beschikbaar is, van de client `/negotiate` kiest uit de beschikbare `secondary` eindpunten. Dit mechanisme failover vereist dat elk eindpunt als fungeren moet `primary` eindpunt op ten minste één app-server.

![Failover](./media/signalr-howto-scale-multi-instances/failover_negotiate.png)

## <a name="next-steps"></a>Volgende stappen

In deze handleiding, hebt u geleerd over het configureren van meerdere exemplaren binnen dezelfde toepassing voor het schalen, sharding en regio-overschrijdende scenario's.

Ondersteuning voor meerdere eindpunten kunnen ook worden gebruikt in hoge beschikbaarheid en noodherstel herstelscenario's.

> [!div class="nextstepaction"]
> [SignalR-Service voor herstel na noodgevallen en hoge beschikbaarheid instellen](./signalr-concept-disaster-recovery.md)
