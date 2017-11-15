---
title: Azure Application Insights telemetrie correlatie | Microsoft Docs
description: Application Insights telemetrie correlatie
services: application-insights
documentationcenter: .net
author: SergeyKanzhelev
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/25/2017
ms.author: mbullwin
ms.openlocfilehash: e821a640d3d75e712c022bd681eb07b83da91911
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="telemetry-correlation-in-application-insights"></a>De correlatie telemetrie in Application Insights

In de wereld van micro services vereist elke logische bewerking werk dat in de verschillende onderdelen van de service. Elk van deze onderdelen afzonderlijk kan worden bewaakt met [Application Insights](app-insights-overview.md). Het webonderdeel-app communiceert met provider onderdeel om gebruikersreferenties te valideren en met het onderdeel API om gegevens te verkrijgen voor visualisatie. De API-onderdeel op zijn beurt kunt opvragen van gegevens van andere services en onderdelen van de cache-serviceprovider gebruiken en kennis van de facturering component over deze aanroep. Application Insights ondersteunt gedistribueerde telemetrie correlatie. Hiermee kunt u bepalen welk onderdeel is verantwoordelijk voor fouten of vermindering in prestaties.

Dit artikel wordt uitgelegd dat het gegevensmodel dat wordt gebruikt door de Application Insights voor het correleren van telemetrie verzonden door meerdere onderdelen. Er wordt aangegeven in de context doorgifte van technieken en protocollen. Het omvat tevens de uitvoering van de correlatie-concepten in verschillende talen en platforms.

## <a name="telemetry-correlation-data-model"></a>Telemetrie correlatie-gegevensmodel

Application Insights definieert een [gegevensmodel](application-insights-data-model.md) voor gedistribueerde telemetrie correlatie. Telemetrie koppelt u de logische werking, elk telemetrie-item heeft een contextveld `operation_Id`. Deze id wordt gedeeld door elk telemetrie-item in de gedistribueerde tracering. Dus zelfs met verlies van telemetrie van één laag kunt u nog steeds telemetrie die zijn gerapporteerd door de andere onderdelen koppelen.

Gedistribueerde logische bewerking bestaat gewoonlijk uit een reeks kleinere bewerkingen - aanvragen verwerkt door een van de onderdelen. Deze bewerkingen zijn gedefinieerd door [aanvragen telemetrie](application-insights-data-model-request-telemetry.md). Elke aanvraagtelemetrie heeft zijn eigen `id` die deze globaal unieke wijze identificeert. En alle telemetrie - traceringen, uitzonderingen, enz. die zijn gekoppeld aan deze aanvraag moet worden ingesteld de `operation_parentId` aan de waarde van de aanvraag `id`.

Elke uitgaande bewerking zoals http-aanroep naar een ander onderdeel dat wordt vertegenwoordigd door [afhankelijkheidstelemetrie](application-insights-data-model-dependency-telemetry.md). Afhankelijkheidstelemetrie definieert ook een eigen `id` globaal uniek is. Aanvraagtelemetrie, door deze afhankelijkheidsaanroep gestart wordt gebruikt als `operation_parentId`.

U kunt de weergave van het gebruik van gedistribueerde logische bewerking opbouwen `operation_Id`, `operation_parentId`, en `request.id` met `dependency.id`. Deze velden worden ook de volgorde oorzakelijk verband van telemetrie aanroepen definiëren.

Traces van onderdelen mogelijk in micro services-omgeving gaat u naar de andere gelijksoortig. Elk onderdeel hebben een eigen instrumentatiesleutel in Application Insights. Als u telemetrie voor de logische werking, moet u gegevens opvragen uit elke opslag. Wanneer het aantal gelijksoortig grote is, moet u beschikken over een hint op waar u wilt zoeken volgende.

Application Insights-gegevensmodel definieert twee velden om dit probleem te verhelpen: `request.source` en `dependency.target`. Het eerste veld duidt het onderdeel met de afhankelijkheid aanvraag gestart, en de tweede welk onderdeel de reactie van de afhankelijkheidsaanroep van geretourneerd.


## <a name="example"></a>Voorbeeld

U gaat nu een voorbeeld van een toepassing STOCK prijzen met een prijs van een bestand met de externe API voorraden API aangeroepen. De toepassing STOCK prijzen heeft een pagina `Stock page` geopend via de client web browser `GET /Home/Stock`. De toepassing de API STOCK query's met behulp van een HTTP-aanroep `GET /api/stock/value`.

U kunt analyseren resulterende telemetrie een query uit te voeren:

```
(requests | union dependencies | union pageViews) 
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

In de resultaat-weergave opmerking dat alle telemetrie-items de hoofdmap delen `operation_Id`. Ajax aanroep uitgevoerd vanaf de pagina - nieuwe unieke id `qJSXU` is toegewezen aan de afhankelijkheidstelemetrie en van pageView-id wordt gebruikt als `operation_ParentId`. Serveraanvraag gebruikt op zijn beurt van ajax-id als `operation_ParentId`, enzovoort.

| itemType   | naam                      | id           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| pageView   | Vooraf gedefinieerde pagina                |              | STYz               | STYz         |
| afhankelijkheid | GET-/Home/voorraad           | qJSXU        | STYz               | STYz         |
| Aanvraag    | GET-startpagina/voorraad            | KqKwlrSt9PA = | qJSXU              | STYz         |
| afhankelijkheid | /Api/stock/value ophalen      | bBrf2L7mm2g = | KqKwlrSt9PA =       | STYz         |

Nu wanneer de aanroep `GET /api/stock/value` aangebracht in een externe service die u wilt weten van de identiteit van die server. U kunt instellen `dependency.target` veld op de juiste wijze. Wanneer de externe service biedt geen ondersteuning voor het controle - `target` is ingesteld op de hostnaam van de service, zoals `stock-prices-api.com`. Echter als die service zichzelf identificeert door te retourneren van een vooraf gedefinieerde HTTP-header - `target` bevat van de service-identiteit waarmee Application Insights gedistribueerde trace verder door het uitvoeren van query's telemetrie van die service. 

## <a name="correlation-headers"></a>Correlatie headers

We werken over RFC voorstel voor de [correlatie HTTP-protocol](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v1.md). Dit voorstel definieert twee headers worden gebruikt:

- `Request-Id`de globaal unieke id van de aanroep uitvoeren
- `Correlation-Context`-de naam van waarde-paren verzameling van de gedistribueerde trace-eigenschappen uitvoeren

De standaard definieert ook twee schema's van `Request-Id` generatie - platte en hiërarchische. Met het schema platte, er is een bekende `Id` sleutel gedefinieerd voor de `Correlation-Context` verzameling.

Application Insights definieert de [extensie](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) voor de correlatie HTTP-protocol. Hierbij `Request-Context` naam / waardeparen naar de verzameling van eigenschappen die worden gebruikt door de onmiddellijke aanroeper of opgeroepen is doorgegeven. Application Insights-SDK maakt gebruik van deze header instellen `dependency.target` en `request.source` velden.

## <a name="open-tracing-and-application-insights"></a>Open tracering en Application Insights

[Open tracering](http://opentracing.io/) en Application Insights gegevens ziet er modellen 

- `request`, `pageView` wordt toegewezen aan **Span** met`span.kind = server`
- `dependency`toegewezen aan **Span** met`span.kind = client`
- `id`van een `request` en `dependency` wordt toegewezen aan **Span.Id**
- `operation_Id`toegewezen aan **TraceId**
- `operation_ParentId`toegewezen aan **verwijzing** van het type`ChildOf`

Zie [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en data model.

Zie [specificatie](https://github.com/opentracing/specification/blob/master/specification.md) en [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) voor definities van de concepten Open tracering.


## <a name="telemetry-correlation-in-net"></a>Telemetrie correlatie in .NET

Na verloop van tijd gedefinieerd .NET verschillende manieren met elkaar correleren Telemetrie en diagnostische logboeken. Er is `System.Diagnostics.CorrelationManager` toestaan om bij te houden [LogicalOperationStack en ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). `System.Diagnostics.Tracing.EventSource`en Windows (ETW) definieert u de methode [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx). `ILogger`maakt gebruik van [logboek Scopes](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). WCF- en HTTP-kabel van 'huidige' context doorgeven.

Deze methoden niet echter ondersteuning voor automatische gedistribueerde tracering inschakelen. `DiagnosticsSource`is een manier voor de ondersteuning van automatische cross-machine correlatie. .NET-bibliotheken ondersteunen diagnostische gegevens en automatische cross-machine doorgifte van de correlatie-context via het transport zoals http toestaan.

De [handleiding voor activiteiten](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) in de bron van de diagnostische gegevens vindt u de basisbeginselen van het bijhouden van activiteiten. 

ASP.NET Core 2.0 ondersteunt extractie van Http-Headers en de nieuwe activiteit wordt gestart. 

`System.Net.HttpClient`versie `<fill in>` ondersteunt automatische injectie van de correlatie Http-Headers en bijhouden van de http-aanroep als een activiteit.

Er is een nieuwe Http-Module [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) voor het klassieke ASP.NET. Deze module implementeert telemetrie correlatie met DiagnosticsSource. Deze activiteit op basis van binnenkomende aanvraagheaders wordt gestart. Deze correleert ook telemetrie van de verschillende stadia van aanvraagverwerking. Ook voor gevallen wanneer elke fase van de verwerking van IIS wordt uitgevoerd op een andere beheren threads.

Application Insights-SDK versie `2.4.0-beta1` DiagnosticsSource en activiteit voor het verzamelen van telemetriegegevens en deze koppelen aan de huidige activiteit worden gebruikt. 

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste telemetrie schrijven](app-insights-api-custom-events-metrics.md)
- Ingebouwde alle onderdelen van uw micro-service op de Application Insights. Bekijk [ondersteunde platforms](app-insights-platforms.md).
- Zie [gegevensmodel](application-insights-data-model.md) voor Application Insights-typen en data model.
- Meer informatie over hoe [uitbreiden en het filteren van telemetrie](app-insights-api-filtering-sampling.md).
