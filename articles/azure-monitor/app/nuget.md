---
title: Azure Application Insights - afhankelijkheid automatisch verzamelen | Microsoft Docs
description: Application Insights automatisch verzameld en afhankelijkheden visualiseren
services: application-insights
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: reference
ms.date: 10/16/2018
ms.author: mbullwin
ms.openlocfilehash: 3ad2f4788a765366066023724772f5432d0d56eb
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54108340"
---
# <a name="application-insights-nuget-packages"></a>Application Insights NuGet-pakketten

Hieronder volgt de huidige lijst van stabiele versie van NuGet-pakketten voor Application Insights.

## <a name="common-packages-for-aspnet"></a>Algemene pakketten voor ASP.NET

| Pakketnaam | Stabiele versie | Description | Downloaden |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights | 2.8.0 | Kernfunctionaliteit voor de overdracht van alle Telemetrietypen voor Application Insights biedt en is een afhankelijk pakket voor alle andere Application Insights-pakketten | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) |
|Microsoft.ApplicationInsights.Agent.Intercept | 2.4.0 | Hiermee worden onderschept methodeaanroepen | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent.Intercept/) |
| Microsoft.ApplicationInsights.DependencyCollector | 2.8.0 | Application Insights-afhankelijkheid Collector voor .NET-toepassingen. Dit is een afhankelijk pakket voor Application Insights platform-specifieke pakketten en biedt automatisch verzamelen van afhankelijkheidstelemetrie. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.8.0 | Application Insights prestaties tellers Collector kunt u voor het verzenden van gegevens die door de prestatiemeteritems zijn verzameld met Application Insights. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) |
| Microsoft.ApplicationInsights.Web | 2.8.0 | Application Insights voor .NET-webtoepassingen | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Web/) |
| Microsoft.ApplicationInsights.WindowsServer | 2.8.0 | Application Insights Windows Server NuGet-pakket biedt automatisch verzamelen van telemetrie van application insights voor .NET-toepassingen. Dit pakket kan worden gebruikt als een afhankelijk pakket voor Application Insights platform-specifieke pakketten of als een zelfstandige pakket voor .NET-toepassingen die niet worden gedekt door platform-specifieke pakketten (zoals voor .NET-werkrollen). | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)  
| Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel | 2.8.0 | Biedt een kanaal telemetrie naar Application Insights Windows Server-SDK die telemetrie in offlinescenario's blijft behouden. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/) |

## <a name="common-packages-for-aspnet-core"></a>Algemene pakketten voor ASP.NET Core

| Pakketnaam | Stabiele versie | Description | Downloaden |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.AspNetCore | 2.5.0 | Application Insights voor ASP.NET Core web-toepassingen. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AspNetCore/) |
| Microsoft.ApplicationInsights | 2.8.0 | Dit pakket biedt kernfunctionaliteit voor de overdracht van alle Telemetrietypen voor Application Insights en is van een afhankelijk pakket voor alle andere Application Insights-pakketten | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) |
| Microsoft.ApplicationInsights.DependencyCollector | 2.8.0 | Application Insights-afhankelijkheid Collector voor .NET-toepassingen. Dit is een afhankelijk pakket voor Application Insights platform-specifieke pakketten en biedt automatisch verzamelen van afhankelijkheidstelemetrie. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DependencyCollector/) |
| Microsoft.ApplicationInsights.PerfCounterCollector | 2.8.0 | Application Insights prestaties tellers Collector kunt u voor het verzenden van gegevens die door de prestatiemeteritems zijn verzameld met Application Insights. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.PerfCounterCollector/) |
| Microsoft.ApplicationInsights.WindowsServer | 2.8.0 | Application Insights Windows Server NuGet-pakket biedt automatisch verzamelen van telemetrie van application insights voor .NET-toepassingen. Dit pakket kan worden gebruikt als een afhankelijk pakket voor Application Insights platform-specifieke pakketten of als een zelfstandige pakket voor .NET-toepassingen die niet worden gedekt door platform-specifieke pakketten (zoals voor .NET-werkrollen). | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer/)  |
| Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel | 2.8.0 | Biedt een kanaal telemetrie naar Application Insights Windows Server-SDK die telemetrie in offlinescenario's blijft behouden. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.WindowsServer.TelemetryChannel/) |

## <a name="listenerscollectorsappenders"></a>Listeners/collectors/appenders

| Pakketnaam | Stabiele versie | Description | Downloaden |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.DiagnosticSourceListener | 2.7.2 |  Hiermee kunt het doorsturen van gebeurtenissen uit DiagnosticSource naar Application Insights. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.DiagnosticSourceListener/) |
| Microsoft.ApplicationInsights.EventSourceListener | 2.7.2 | Application Insights EventSourceListener kunt gegevens uit de gebeurtenisbron gebeurtenissen verzenden naar Application Insights. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventSourceListener/) |
| Microsoft.ApplicationInsights.EtwCollector | 2.7.2 | Application Insights EtwCollector kunt gegevens uit Event Tracing voor Windows (ETW) te verzenden naar Application Insights. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EtwCollector/) |
| Microsoft.ApplicationInsights.TraceListener | 2.7.2 | Een aangepaste TraceListener zodat u kunt traceerlogboekberichten verzenden naar Application Insights. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.TraceListener/) |
| Microsoft.ApplicationInsights.Log4NetAppender | 2.7.2 | Een aangepaste appender toe zodat u kunt Log4Net logboekberichten te verzenden naar Application Insights. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Log4NetAppender/)
| Microsoft.ApplicationInsights.NLogTarget | 2.7.2 |  een aangepaste doel zodat u kunt NLog logboekberichten te verzenden naar Application Insights. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.NLogTarget/)
| Microsoft.ApplicationInsights.SnapshotCollector | 1.3.1 | Uitzonderingen in uw toepassing bewaakt en verzamelt automatisch momentopnamen voor offline analyse. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector/)

## <a name="service-fabric"></a>Service Fabric

| Pakketnaam | Stabiele versie | Description | Downloaden |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.ServiceFabric | 2.2.0 | Dit pakket biedt automatische inrichting van telemetrie met de service fabric-context de toepassing wordt uitgevoerd in. Gebruik deze NuGet niet voor Native Service Fabric-toepassingen. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.ServiceFabric/) |
| Microsoft.ApplicationInsights.ServiceFabric.Native | 2.2.0 | Application Insights-module voor service fabric-toepassingen. Gebruik deze NuGet alleen voor Native Service Fabric-toepassingen. Voor toepassingen in containers worden uitgevoerd, Microsoft.ApplicationInsights.ServiceFabric-pakket te gebruiken. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.ServiceFabric.Native/) |  

## <a name="status-monitor"></a>Statusmonitor

| Pakketnaam | Stabiele versie | Description | Downloaden |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.Agent_x64 | 2.2.1 |  Schakelt de gegevensverzameling van de runtime voor x64 toepassingen | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent_x64/) |
| Microsoft.ApplicationInsights.Agent_x86 | 2.2.1 |  Schakelt de gegevensverzameling van de runtime voor x86 toepassingen. | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Agent_x86/) |

Deze pakketten maken deel van de kernfunctionaliteit van runtimecontrole [Status Monitor](../../azure-monitor/app/monitor-performance-live-website-now.md). U hoeft niet te deze pakketten rechtstreeks downloaden, gebruikt u alleen de Status Monitor-installatieprogramma. Als u wilt weten meer over hoe deze pakketten werken achter de schermen [blogbericht](https://apmtips.com/blog/2016/11/18/how-application-insights-status-monitor-not-monitors-dependencies/) uit een van onze ontwikkelaars is een goede start.

## <a name="additional-packages"></a>Extra pakketten

| Pakketnaam | Stabiele versie | Description | Downloaden |
|-------------------------------|-----------------------|------------|----|
| Microsoft.ApplicationInsights.AzureWebSites | 2.6.5 | Deze extensie kunt de bewaking van Application Insights op een Azure App Service. SDK-versie 2.6.1. Instructies: Toepassingsinstellingen 'APPINSIGHTS_INSTRUMENTATIONKEY' met uw ikey toevoegen en de Web-App als u wilt een kracht opnieuw opstarten.| [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.AzureWebSites/) |
| Microsoft.ApplicationInsights.Injector | 2.6.7 | Dit pakket bevat de vereiste bestanden voor zonder code Application Insights-injectie | [Het pakket downloaden](https://www.nuget.org/packages/Microsoft.ApplicationInsights.Injector/) |

## <a name="next-steps"></a>Volgende stappen

- Monitor [ASP.NET Core](../../azure-monitor/app/asp-net-core.md).
- Profileren van ASP.NET Core [Azure Linux-web-apps](../../azure-monitor/app/profiler-aspnetcore-linux.md).
- Fouten opsporen in ASP.NET [momentopnamen](../../azure-monitor/app/snapshot-debugger.md).