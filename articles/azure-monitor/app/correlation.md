---
title: Azure Application Insights-telemetriecorrelatie | Microsoft Docs
description: Application Insights-telemetriecorrelatie
services: application-insights
documentationcenter: .net
author: lgayhardt
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/14/2019
ms.reviewer: sergkanz
ms.author: lagayhar
ms.openlocfilehash: bcb62b311426c7582202ced3714d01c429118ab5
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56268410"
---
# <a name="telemetry-correlation-in-application-insights"></a>Telemetriecorrelatie in Application Insights

Elke logische bewerking moet in de wereld van microservices, te worden uitgevoerd in verschillende onderdelen van de service. Elk van deze onderdelen afzonderlijk door kan worden bewaakt [Azure Application Insights](../../azure-monitor/app/app-insights-overview.md). Het onderdeel web-app communiceert met de provider-authenticatiecomponent om gebruikersreferenties te valideren, en met de API-component om gegevens voor visualisatie. De API-component kan gegevens van andere services op te vragen en cacheprovider onderdelen gebruiken om u te waarschuwen de facturering component over deze aanroep. Application Insights ondersteunt gedistribueerde telemetriecorrelatie, die u gebruiken voor het detecteren van welk onderdeel is verantwoordelijk voor fouten of afname van de prestaties.

In dit artikel wordt uitgelegd dat het gegevensmodel dat wordt gebruikt door Application Insights voor het correleren van telemetrie die is verzonden door meerdere onderdelen. Het bevat informatie over de context doorgeven technieken en protocollen. Dit geldt ook voor de uitvoering van de correlatie-concepten in verschillende talen en platforms.

## <a name="data-model-for-telemetry-correlation"></a>Gegevensmodel voor telemetriecorrelatie

Application Insights definieert een [gegevensmodel](../../azure-monitor/app/data-model.md) voor gedistribueerde telemetriecorrelatie. Als u wilt koppelen telemetrie aan de logische werking, elk telemetrie-item heeft een contextveld met de naam `operation_Id`. Deze id wordt gedeeld door elk telemetrie-item in de gedistribueerde tracering. Dus zelfs met verlies van telemetrie van één laag, kunt u nog steeds koppelen telemetrie die zijn gerapporteerd door andere onderdelen.

Een gedistribueerde logische bewerking bestaat gewoonlijk uit een reeks kleinere bewerkingen, die aanvragen verwerkt door een van de onderdelen. Deze bewerkingen worden gedefinieerd door [aanvragen telemetrie](../../azure-monitor/app/data-model-request-telemetry.md). Elke aanvraag telemetrische gegevens is een eigen `id` die het identificeert een unieke en internationaal niveau opereren. En alle telemetrie-items (zoals traces en uitzonderingen) die gekoppeld aan deze aanvraag zijn moeten de `operation_parentId` op de waarde van de aanvraag `id`.

Elke uitgaande bewerking, zoals een HTTP-aanroep naar een ander onderdeel, wordt vertegenwoordigd door [afhankelijkheidstelemetrie](../../azure-monitor/app/data-model-dependency-telemetry.md). Afhankelijkheidstelemetrie bepaalt ook een eigen `id` wereldwijd uniek is. Telemetrie, gestart door deze afhankelijkheidsaanroep maakt gebruik van deze aanvraag `id` als de `operation_parentId`.

U kunt een weergave van de gedistribueerde logische bewerking bouwen met behulp van `operation_Id`, `operation_parentId`, en `request.id` met `dependency.id`. Deze velden worden ook de volgorde oorzakelijke van telemetrie aanroepen definiëren.

In een omgeving met microservices gaat traceringen van onderdelen u naar de andere opslag-items. Elk onderdeel hebben een eigen instrumentatiesleutel in Application Insights. Als u telemetrie voor de logische werking, moet u gegevens van elk opslagitem op te vragen. Wanneer het aantal items van de opslag enorm is, moet u een hint over waar u het volgende bekijken. Het gegevensmodel van Application Insights definieert twee velden om op te lossen dit probleem: `request.source` en `dependency.target`. Het eerste veld de component aangeduid die de aanvraag afhankelijkheid gestart en de tweede wordt aangegeven welk component geretourneerd het antwoord van de afhankelijkheidsaanroep.

## <a name="example"></a>Voorbeeld

We nemen een voorbeeld van een toepassing met de naam van de prijzen voor voorraad, waarin een prijs van een bestand met behulp van een externe API aangeroepen `Stock`. De prijzen voor voorraad-toepassing heeft een pagina met de naam `Stock page` die de client-webbrowser wordt geopend met behulp van `GET /Home/Stock`. De toepassing query's de `Stock` API met behulp van een HTTP-aanroep `GET /api/stock/value`.

U kunt de resulterende telemetrie analyseren door een query uit te voeren:

```kusto
(requests | union dependencies | union pageViews)
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

Merk op dat de hoofdmap wordt gedeeld door alle telemetrie-items in de resultaten `operation_Id`. Wanneer een Ajax-aanroep wordt uitgevoerd vanaf de pagina, een nieuwe unieke ID (`qJSXU`) is toegewezen aan de afhankelijkheidstelemetrie en de ID van de paginaweergave wordt gebruikt als `operation_ParentId`. Vervolgens gebruikt de Ajax-ID als de serveraanvraag `operation_ParentId`.

| itemType   | naam                      | Id           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| pageView   | Aandelen pagina                |              | STYz               | STYz         |
| afhankelijkheid | GET-/Home/voorraad           | qJSXU        | STYz               | STYz         |
| aanvraag    | GET Home/voorraad            | KqKwlrSt9PA= | qJSXU              | STYz         |
| afhankelijkheid | /Api/stock/value ophalen      | bBrf2L7mm2g= | KqKwlrSt9PA=       | STYz         |

Wanneer de aanroep `GET /api/stock/value` wordt gemaakt met een externe service, die u wilt weten wat de identiteit van die server zodat u kunt instellen de `dependency.target` veld op de juiste wijze. Wanneer de externe service biedt geen ondersteuning voor bewaking, `target` is ingesteld op de hostnaam van de service (bijvoorbeeld `stock-prices-api.com`). Echter, als de service zichzelf identificeert door te retourneren een vooraf gedefinieerde HTTP-header `target` bevat van de service-identiteit waarmee Application Insights aan het bouwen van een gedistribueerde tracering door het opvragen van telemetrie van die service.

## <a name="correlation-headers"></a>Correlatie-headers

Er wordt gewerkt aan een RFC-voorstel voor de [correlatie HTTP-protocol](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Dit voorstel definieert twee headers:

- `Request-Id`: De unieke ID van de aanroep uitvoert.
- `Correlation-Context`: Hiermee voert de verzameling naam / waarde-paren van de eigenschappen van gedistribueerde tracering.

De standaard definieert ook twee schema's voor `Request-Id` generatie: platte en hiërarchische. Met de platte schema, een bekende `Id` sleutel is gedefinieerd voor de `Correlation-Context` verzameling.

Application Insights definieert de [extensie](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) voor de correlatie HTTP-protocol. Hierbij `Request-Context` naam / waarde-paren worden doorgegeven van de verzameling van eigenschappen die worden gebruikt door de oproepende of opgeroepen. De Application Insights SDK maakt gebruik van deze header om in te stellen `dependency.target` en `request.source` velden.

### <a name="w3c-distributed-tracing"></a>W3C gedistribueerde tracering

Nu we overstappen naar [W3C-indeling voor gedistribueerde tracering](https://w3c.github.io/trace-context/). Hiermee wordt gedefinieerd:

- `traceparent`: De unieke bewerkings-ID en de unieke id van de aanroep uitvoert.
- `tracestate`: Uitvoert voor het traceren van systeemspecifieke context.

#### <a name="enable-w3c-distributed-tracing-support-for-classic-aspnet-apps"></a>W3C gedistribueerde tracering ondersteuning voor klassieke ASP.NET-apps inschakelen

Deze functie is beschikbaar in `Microsoft.ApplicationInsights.Web` en `Microsoft.ApplicationInsights.DependencyCollector` vanaf versie 2.8.0-beta1 pakketten.
Dit standaard uitgeschakeld. Als u wilt inschakelen, wijzigt u `ApplicationInsights.config`:

- Onder `RequestTrackingTelemetryModule`, voeg de `EnableW3CHeadersExtraction` element met de waarde ingesteld op `true`.
- Onder `DependencyTrackingTelemetryModule`, voeg de `EnableW3CHeadersInjection` element met de waarde ingesteld op `true`.

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>W3C gedistribueerde tracering ondersteuning voor ASP.NET Core-apps inschakelen

Deze functie is in `Microsoft.ApplicationInsights.AspNetCore` versie 2.5.0-beta1 en `Microsoft.ApplicationInsights.DependencyCollector` versie 2.8.0-beta1.
Dit standaard uitgeschakeld. Als u wilt inschakelen, stelt `ApplicationInsightsServiceOptions.RequestCollectionOptions.EnableW3CDistributedTracing` naar `true`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry(o => 
        o.RequestCollectionOptions.EnableW3CDistributedTracing = true );
    // ....
}
```

#### <a name="enable-w3c-distributed-tracing-support-for-java-apps"></a>W3C gedistribueerde tracering ondersteuning voor Java-apps inschakelen

- **Binnenkomende configuration**

  - Voor Java EE-apps, het volgende toevoegen aan de `<TelemetryModules>` code binnen ApplicationInsights.xml:

    ```xml
    <Add type="com.microsoft.applicationinsights.web.extensibility.modules.WebRequestTrackingTelemetryModule>
       <Param name = "W3CEnabled" value ="true"/>
       <Param name ="enableW3CBackCompat" value = "true" />
    </Add>
    ```
  - Voeg de volgende eigenschappen voor Spring Boot-apps:

    - `azure.application-insights.web.enable-W3C=true`
    - `azure.application-insights.web.enable-W3C-backcompat-mode=true`

- **Uitgaande configuratie**

  Voeg de volgende AI-Agent.xml:

  ```xml
  <Instrumentation>
    <BuiltIn enabled="true">
      <HTTP enabled="true" W3C="true" enableW3CBackCompat="true"/>
    </BuiltIn>
  </Instrumentation>
  ```

  > [!NOTE]
  > Compatibiliteit met eerdere versies modus standaard is ingeschakeld en de `enableW3CBackCompat` parameter is optioneel. Gebruik dit alleen als u wilt uitschakelen voor achterwaartse compatibiliteit.
  >
  > In het ideale geval zou u dit uitschakelen bij alle services zijn bijgewerkt naar nieuwere versies van SDK's die ondersteuning bieden voor de W3C-protocol. Het is raadzaam dat u naar deze nieuwe SDK's zo snel mogelijk verplaatsen.

> [!IMPORTANT]
> Zorg ervoor dat zowel inkomende als uitgaande configuraties zijn precies hetzelfde.

## <a name="opentracing-and-application-insights"></a>OpenTracing en Application Insights

De [OpenTracing gegevensmodel specificatie](https://opentracing.io/) en Application Insights-gegevensmodellen toewijzen in de volgende manier:

| Application Insights                  | OpenTracing                                       |
|------------------------------------   |-------------------------------------------------  |
| `Request`, `PageView`                 | `Span` met `span.kind = server`                  |
| `Dependency`                          | `Span` met `span.kind = client`                  |
| `Id` van `Request` en `Dependency`    | `SpanId`                                          |
| `Operation_Id`                        | `TraceId`                                         |
| `Operation_ParentId`                  | `Reference` van het type `ChildOf` (het bovenliggende bereik)   |

Zie voor meer informatie de [gegevensmodel van Application Insights-telemetrie](../../azure-monitor/app/data-model.md). 

Zie voor definities van OpenTracing concepten, de OpenTracing [specificatie](https://github.com/opentracing/specification/blob/master/specification.md) en [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md).

## <a name="telemetry-correlation-in-net"></a>Telemetriecorrelatie in .NET

Na verloop van tijd gedefinieerd .NET verschillende manieren voor het correleren van Telemetrie en diagnostische logboeken:

- `System.Diagnostics.CorrelationManager` Hiermee kunt u gegevens van [LogicalOperationStack en ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). 
- `System.Diagnostics.Tracing.EventSource` en Event Tracing voor Windows (ETW) definieert de [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx) methode.
- `ILogger` maakt gebruik van [Log Scopes](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). 
- Windows Communication Foundation (WCF) en HTTP-kabel van 'huidige' context doorgeven.

Deze methoden niet hebt echter ondersteuning voor automatische gedistribueerde tracering ingeschakeld. `DiagnosticSource` is een manier voor de ondersteuning van automatische cross-machine correlatie. .NET-bibliotheken 'DiagnosticSource' ondersteunen en automatische cross-machine-doorgifte van de correlatie-context via het transport, zoals HTTP toestaan.

De [handleiding voor activiteiten](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) in `DiagnosticSource` vindt u de basisbeginselen van het bijhouden van activiteiten.

ASP.NET Core 2.0 ondersteunt extractie van HTTP-headers en een nieuwe activiteit te starten.

`System.Net.HttpClient`, biedt vanaf versie 4.1.0, ondersteuning voor automatische injecteren van de correlatie HTTP-headers en tracering de HTTP-aanroep als een activiteit.

Er is een nieuwe HTTP-module [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/), voor klassieke ASP.NET. Deze module telemetriecorrelatie implementeert met behulp van `DiagnosticSource`. Er wordt een activiteit die op basis van binnenkomende aanvraagheaders gestart. Deze ook een koppeling tussen telemetrie van de verschillende fasen van de aanvraag verwerkt, zelfs voor gevallen wanneer elk stadium van de verwerking van Internet Information Services (IIS) wordt uitgevoerd op een andere beheerde thread.

Maakt gebruik van de Application Insights-SDK, beginnend met versie 2.4.0-beta1, `DiagnosticSource` en `Activity` voor het verzamelen van Telemetrie en deze koppelen aan de huidige activiteit.

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-the-java-sdk"></a>Telemetriecorrelatie in de Java-SDK

De [Application Insights-SDK voor Java](../../azure-monitor/app/java-get-started.md) automatische correlatie van telemetrie, te beginnen met versie 2.0.0 ondersteunt. Vult automatisch `operation_id` voor alle telemetrie (zoals traceringen, uitzonderingen en aangepaste gebeurtenissen) binnen het bereik van een aanvraag is uitgegeven. Deze ook zorgt voor het doorgeven van de correlatie-headers (zoals eerder beschreven) voor service-naar-service aanroepen via HTTP, als de [Java SDK agent](../../azure-monitor/app/java-agent.md) is geconfigureerd.

> [!NOTE]
> Alleen aanroepen via Apache HTTPClient worden ondersteund voor de correlatie-functie. Als u Spring RestTemplate of Feign, kunnen beide worden gebruikt met Apache HTTPClient achter de schermen.

Op dit moment wordt niet automatische context doorgeven voor messaging-technologieën (zoals Kafka, RabbitMQ of Azure Service Bus) ondersteund. Het is echter mogelijk handmatig code voor dergelijke scenario's met behulp van de `trackDependency` en `trackRequest` API's. In deze API's, een afhankelijkheidstelemetrie vertegenwoordigt een bericht wordt in de wachtrij door een producent en de aanvraag een bericht dat wordt verwerkt door een consument. In dit geval beide `operation_id` en `operation_parentId` moeten worden doorgevoerd in de eigenschappen van het bericht.

<a name="java-role-name"></a>
## <a name="role-name"></a>Rolnaam

Soms wilt u mogelijk aanpassen van de manier waarop de namen van onderdelen worden weergegeven in de [Toepassingsoverzicht](../../azure-monitor/app/app-map.md). Om dit te doen, kunt u handmatig instellen de `cloud_RoleName` op een van de volgende manieren:

- Als u Spring Boot aan de Application Insights Spring Boot starter gebruiken, wordt de enige vereiste wijziging is het instellen van uw aangepaste naam voor de toepassing in de application.properties-bestand.

  `spring.application.name=<name-of-app>`

  De Spring Boot starter wijst automatisch `cloudRoleName` op de waarde die u invoert voor de `spring.application.name` eigenschap.

- Als u de `WebRequestTrackingFilter`, wordt de `WebAppNameContextInitializer` naam van de toepassing wordt automatisch ingesteld. Voeg het volgende toe aan uw configuratiebestand (ApplicationInsights.xml):

  ```XML
  <ContextInitializers>
    <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebAppNameContextInitializer" />
  </ContextInitializers>
  ```

- Als u de contextklasse van de cloud gebruiken:

  ```Java
  telemetryClient.getContext().getCloud().setRole("My Component Name");
  ```

## <a name="next-steps"></a>Volgende stappen

- Schrijven [aangepaste telemetrie](../../azure-monitor/app/api-custom-events-metrics.md).
- Meer informatie over [cloud_RoleName instellen](../../azure-monitor/app/app-map.md#set-cloudrolename) voor andere SDK's.
- Onboarding alle onderdelen van uw microservice voor Application Insights. Bekijk de [ondersteunde platforms](../../azure-monitor/app/platforms.md).
- Zie de [gegevensmodel](../../azure-monitor/app/data-model.md) voor Application Insights-typen.
- Meer informatie over het [uitbreiden en telemetrie filteren](../../azure-monitor/app/api-filtering-sampling.md).
- Controleer de [Application Insights config verwijzing](configuration-with-applicationinsights-config.md).
