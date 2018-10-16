---
title: Azure Application Insights-Telemetriecorrelatie | Microsoft Docs
description: Application Insights-telemetriecorrelatie
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/09/2018
ms.reviewer: sergkanz
ms.author: mbullwin
ms.openlocfilehash: d9b6f5c08eed5efceafc71feaf654ad8f4fcafa0
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341120"
---
# <a name="telemetry-correlation-in-application-insights"></a>Telemetriecorrelatie in Application Insights

Elke logische bewerking moet in de wereld van microservices zijn werk uit te voeren in de verschillende onderdelen van de service. Elk van deze onderdelen afzonderlijk kan worden gecontroleerd door [Application Insights](app-insights-overview.md). Het webonderdeel-app communiceert met authentication-provider-onderdeel voor het valideren van de referenties van gebruiker en met het onderdeel API om gegevens voor visualisatie. De API-component op zijn beurt kunt opvragen van gegevens uit andere services en onderdelen van de cache-serviceprovider gebruiken en op de hoogte stellen de facturering component over deze aanroep. Application Insights ondersteunt gedistribueerde telemetriecorrelatie. Hiermee kunt u voor het detecteren van welk component is verantwoordelijk voor het fouten of afname van de prestaties.

In dit artikel wordt uitgelegd dat het gegevensmodel dat wordt gebruikt door Application Insights voor het correleren van telemetrie die is verzonden door meerdere onderdelen. Het bevat informatie over de context doorgeven technieken en protocollen. Dit geldt ook voor de implementatie van de correlatie-concepten in verschillende talen en platforms.

## <a name="telemetry-correlation-data-model"></a>Telemetrie correlatie-gegevensmodel

Application Insights definieert een [gegevensmodel](application-insights-data-model.md) voor gedistribueerde telemetriecorrelatie. Als u wilt koppelen telemetrie aan de logische werking, elk telemetrie-item heeft een contextveld met de naam `operation_Id`. Deze id wordt gedeeld door elk telemetrie-item in de gedistribueerde tracering. Dus zelfs met het verlies van telemetrie van één laag kunt u nog steeds telemetrie die zijn gerapporteerd door andere onderdelen koppelen.

Gedistribueerde logische bewerking bestaat gewoonlijk uit een set kleinere bewerkingen - aanvragen verwerkt door een van de onderdelen. Deze bewerkingen worden gedefinieerd door [aanvragen telemetrie](application-insights-data-model-request-telemetry.md). Elke aanvraag telemetrische gegevens is een eigen `id` die deze wereldwijd uniek wordt aangeduid. En alle telemetrie - traceringen, uitzonderingen, enz. die zijn gekoppeld aan deze aanvraag moet de `operation_parentId` op de waarde van de aanvraag `id`.

Elke uitgaande bewerking, zoals http-aanroep naar een ander onderdeel dat wordt vertegenwoordigd door [afhankelijkheidstelemetrie](application-insights-data-model-dependency-telemetry.md). Afhankelijkheidstelemetrie bepaalt ook een eigen `id` wereldwijd uniek is. Aanvraagtelemetrie, gestart door deze afhankelijkheidsaanroep wordt gebruikt als `operation_parentId`.

U kunt de weergave van het gebruik van gedistribueerde logische bewerking bouwen `operation_Id`, `operation_parentId`, en `request.id` met `dependency.id`. Deze velden worden ook de volgorde oorzakelijke van telemetrie aanroepen definiëren.

Traceringen van onderdelen mogelijk in microservices-omgeving gaat u naar de andere opslagruimten. Elk onderdeel hebben een eigen instrumentatiesleutel in Application Insights. Als u telemetrie voor de logische werking, moet u om gegevens te doorzoeken vanuit elke opslag. Wanneer het aantal opslagruimten enorm is, moet u een hint op waar ze moeten kijken volgende hebben.

Application Insights gegevensmodel definieert twee velden om op te lossen dit probleem: `request.source` en `dependency.target`. Het eerste veld de component aangeduid die de aanvraag afhankelijkheid gestart en de tweede wordt aangegeven welk component geretourneerd het antwoord van de afhankelijkheidsaanroep.


## <a name="example"></a>Voorbeeld

We nemen een voorbeeld van een toepassing voorraad prijzen met een prijs van een bestand met behulp van de externe API voorraden API genoemd. De prijzen voor voorraad-toepassing heeft een pagina `Stock page` geopend door de client web browser via `GET /Home/Stock`. De toepassing query's in de voorraad-API met behulp van een HTTP-aanroep `GET /api/stock/value`.

U kunt analyseren resulterende telemetrie een query uit te voeren:

```
(requests | union dependencies | union pageViews) 
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

In het resultaat weergeven Houd er rekening mee dat de hoofdmap wordt gedeeld door alle telemetrie-items `operation_Id`. Wanneer ajax aanroepen uitgevoerd vanaf de pagina - nieuwe unieke id `qJSXU` is toegewezen aan de afhankelijkheidstelemetrie en de paginaweergave-id wordt gebruikt als `operation_ParentId`. Op zijn beurt serveraanvraag maakt gebruik van ajax-id als `operation_ParentId`, enzovoort.

| itemType   | naam                      | id           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| pageView   | Aandelen pagina                |              | STYz               | STYz         |
| afhankelijkheid | GET-/Home/voorraad           | qJSXU        | STYz               | STYz         |
| aanvraag    | GET Home/voorraad            | KqKwlrSt9PA= | qJSXU              | STYz         |
| afhankelijkheid | /Api/stock/value ophalen      | bBrf2L7mm2g = | KqKwlrSt9PA=       | STYz         |

Nu als de aanroep `GET /api/stock/value` aangebracht in een externe service die u wilt weten van de identiteit van die server. U kunt instellen `dependency.target` veld op de juiste wijze. Wanneer de externe service biedt geen ondersteuning voor het controle - `target` is ingesteld op de hostnaam van de service, zoals `stock-prices-api.com`. Maar als deze service zichzelf identificeert door te retourneren een vooraf gedefinieerde HTTP-header - `target` bevat van de service-identiteit waarmee Application Insights aan het bouwen van gedistribueerde tracering door het opvragen van telemetrie van die service. 

## <a name="correlation-headers"></a>Correlatie-headers

Er wordt gewerkt aan RFC voorstel voor de [correlatie HTTP-protocol](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v1.md). Dit voorstel definieert twee headers:

- `Request-Id` de unieke id van de aanroep uitvoeren
- `Correlation-Context` -de naam van waarde-paren verzameling van de eigenschappen van gedistribueerde tracering uitvoeren

De standaard definieert ook twee schema's van `Request-Id` generation - platte en hiërarchische. Met het schema van platte, er is een bekende `Id` sleutel gedefinieerd voor de `Correlation-Context` verzameling.

Application Insights definieert de [extensie](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) voor de correlatie HTTP-protocol. Hierbij `Request-Context` waarde-paren worden doorgegeven van de verzameling van eigenschappen die worden gebruikt door de oproepende of getimed naam. Application Insights-SDK maakt gebruik van deze header om in te stellen `dependency.target` en `request.source` velden.

### <a name="w3c-distributed-tracing"></a>W3C gedistribueerde tracering

We worden overgezet naar voor (W3C gedistribueerde tracering indeling) [https://w3c.github.io/distributed-tracing/report-trace-context.html]. Hiermee wordt gedefinieerd:
- `traceparent` -unieke bewerkings-id en de unieke id van de aanroep uitvoert
- `tracestate` -specifieke systeemcontext tracering uitvoert.

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-classic-apps"></a>W3C gedistribueerde tracering ondersteuning voor ASP.NET, klassiek apps inschakelen

Deze functie is beschikbaar in de Microsoft.ApplicationInsights.Web en Microsoft.ApplicationInsights.DependencyCollector pakketten vanaf versie 2.8.0-beta1.
Het is **uit** standaard, als u wilt inschakelen, wijzigt `ApplicationInsights.config`:

* onder `RequestTrackingTelemetryModule` toevoegen `EnableW3CHeadersExtraction` element met de waarde ingesteld op `true`
* onder `DependencyTrackingTelemetryModule` toevoegen `EnableW3CHeadersInjection` element met de waarde ingesteld op `true`

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>W3C gedistribueerde tracering ondersteuning voor ASP.NET Core-apps inschakelen

Deze functie is in Microsoft.ApplicationInsights.AspNetCore met versie 2.5.0-beta1 en Microsoft.ApplicationInsights.DependencyCollector versie 2.8.0-beta1.
Het is **uit** zodat het standaard `ApplicationInsightsServiceOptions.RequestCollectionOptions.EnableW3CDistributedTracing` naar `true`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry(o => 
        o.RequestCollectionOptions.EnableW3CDistributedTracing = true );
    // ....
}
```

## <a name="open-tracing-and-application-insights"></a>Open tracerings- en Application Insights

De [Open tracering van gegevensmodel specificatie](http://opentracing.io/) en Application Insights-gegevensmodellen toewijzen in de volgende manier:

| Application Insights                  | Tracering openen                                      |
|------------------------------------   |-------------------------------------------------  |
| `Request`, `PageView`                 | `Span` met `span.kind = server`                  |
| `Dependency`                          | `Span` met `span.kind = client`                  |
| `Id` van `Request` en `Dependency`    | `SpanId`                                          |
| `Operation_Id`                        | `TraceId`                                         |
| `Operation_ParentId`                  | `Reference` van het type `ChildOf` (het bovenliggende bereik)   |

Zie voor meer informatie over de Application Insights-gegevensmodel [gegevensmodel](application-insights-data-model.md). 

Zie de Open tracering [specificatie](https://github.com/opentracing/specification/blob/master/specification.md) en [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) voor definities van Open tracering concepten.


## <a name="telemetry-correlation-in-net"></a>Telemetriecorrelatie in .NET

Na verloop van tijd gedefinieerd .NET aantal manieren om te correleren Telemetrie en diagnostische logboeken. Er is `System.Diagnostics.CorrelationManager` toestaan om bij te houden [LogicalOperationStack en ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). `System.Diagnostics.Tracing.EventSource` en Windows ETW definieert u de methode [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx). `ILogger` maakt gebruik van [Log Scopes](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). WCF- en HTTP-kabel van 'huidige' context doorgeven.

Deze methoden niet hebt echter ondersteuning voor automatische gedistribueerde tracering ingeschakeld. `DiagnosticsSource` is een manier voor de ondersteuning van automatische cross-machine correlatie. .NET-bibliotheken ondersteuning voor bron van diagnostische gegevens en automatische cross-machine-doorgifte van de correlatie-context via het transport, zoals http toestaan.

De [handleiding voor activiteiten](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) in de bron van diagnostische gegevens vindt u de basisbeginselen van het bijhouden van activiteiten. 

ASP.NET Core 2.0 ondersteunt extractie van Http-Headers en de nieuwe activiteit te starten. 

`System.Net.HttpClient` vanaf versie `4.1.0` biedt ondersteuning voor automatische injecteren van de correlatie Http-Headers en de http-aanroep als een activiteit bij te houden.

Er is een nieuwe Http-Module [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) voor het klassieke ASP.NET. Deze module implementeert met behulp van DiagnosticsSource telemetriecorrelatie. Deze activiteit op basis van binnenkomende aanvraagheaders wordt gestart. Deze ook een koppeling tussen telemetrie van de verschillende stadia van aanvraagverwerking. Zelfs voor de gevallen wanneer elk stadium van de verwerking van IIS wordt uitgevoerd op een andere beheren-threads.

Application Insights-SDK versie `2.4.0-beta1` DiagnosticsSource en -activiteit gebruikt voor het verzamelen van Telemetrie en deze koppelen aan de huidige activiteit. 

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-the-java-sdk"></a>Telemetriecorrelatie in de Java-SDK
De [Application Insights Java SDK](app-insights-java-get-started.md) biedt ondersteuning voor automatische correlatie van telemetrie, te beginnen met versie `2.0.0`. Vult automatisch `operation_id` voor alle telemetrie (traceringen, uitzonderingen, aangepaste gebeurtenissen, enzovoort), dat binnen het bereik van een aanvraag is uitgegeven. Deze ook zorgt dat de correlatie-headers (hierboven beschreven) voor service-to-service aanroepen via HTTP doorgeven als de [Java SDK agent](app-insights-java-agent.md) is geconfigureerd. Opmerking: alleen aanroepen via Apache HTTP-Client worden ondersteund voor de correlatie-functie. Als u Spring Rest-sjabloon of Feign, kunnen beide worden gebruikt met Apache HTTP-Client achter de schermen.

Automatische context doorgeven via berichttechnologieën (bijvoorbeeld Kafka, RabbitMQ, Azure Service Bus) wordt momenteel niet ondersteund. Het is echter mogelijk handmatig code voor dergelijke scenario's met behulp van de `trackDependency` en `trackRequest` API's, waarbij een afhankelijkheidstelemetrie vertegenwoordigt een bericht wordt in de wachtrij door een producent en de aanvraag een bericht dat wordt verwerkt door een consument. In dit geval beide `operation_id` en `operation_parentId` moeten worden doorgevoerd in de eigenschappen van het bericht.

<a name="java-role-name"></a>
### <a name="role-name"></a>Naam van rol
Soms wilt u mogelijk aanpassen van de manier waarop de namen van onderdelen worden weergegeven in de [Toepassingsoverzicht](app-insights-app-map.md). Om dit te doen, kunt u handmatig instellen de `cloud_roleName` op een van de volgende manieren:

Via een telemetrie-initializer (alle telemetrie-items zijn met tags)
```Java
public class CloudRoleNameInitializer extends WebTelemetryInitializerBase {

    @Override
    protected void onInitializeTelemetry(Telemetry telemetry) {
        telemetry.getContext().getTags().put(ContextTagKeys.getKeys().getDeviceRoleName(), "My Component Name");
    }
  }
```
Via de [apparaat contextklasse](https://docs.microsoft.com/et-ee/java/api/com.microsoft.applicationinsights.extensibility.context._device_context) (alleen deze telemetrie-item is gemarkeerd)
```Java
telemetry.getContext().getDevice().setRoleName("My Component Name");
```

## <a name="next-steps"></a>Volgende stappen

- [Schrijf aangepaste telemetrie](app-insights-api-custom-events-metrics.md)
- Onboarding alle onderdelen van uw micro-service voor Application Insights. Bekijk [ondersteunde platforms](app-insights-platforms.md).
- Zie [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en -gegevensmodel.
- Meer informatie over het [uitbreiden en telemetrie filteren](app-insights-api-filtering-sampling.md).
- [Application Insights-config-verwijzing](app-insights-configuration-with-applicationinsights-config.md)

