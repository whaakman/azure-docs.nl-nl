---
title: 'Application Insights: talen, platforms en integraties | Microsoft Docs'
description: Talen, platforms en integraties die beschikbaar zijn voor Application Insights
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 974db106-54ff-4318-9f8b-f7b3a869e536
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/06/2019
ms.reviewer: olegan
ms.author: mbullwin
ms.openlocfilehash: db79f2b1d3857aac2f71d2e18a3949f068b746eb
ms.sourcegitcommit: 6b41522dae07961f141b0a6a5d46fd1a0c43e6b2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/15/2019
ms.locfileid: "67990062"
---
# <a name="languages---officially-supported-by-application-insights-team"></a>Talen: officieel ondersteund door Application Insights team

* [C#|VB (.NET)](../../azure-monitor/app/asp-net.md)
* [Java](../../azure-monitor/app/java-get-started.md)
* [JavaScript-webpagina's](../../azure-monitor/app/javascript.md)
* [Node.JS](../../azure-monitor/app/nodejs.md)

## <a name="community-sdks"></a>Community Sdk's

Er zijn een aantal Community-Azure-toepassing Insights-Sdk's die oorspronkelijk zijn gemaakt door micro soft. De Sdk's van de community worden niet officieel beheerd door micro soft. Kan geen ondersteuning bieden voor een SDK die niet voor komt in de lijst officieel ondersteunde. Deze Sdk's worden beschouwd als experimentele en worden niet aanbevolen voor productie gebruik.

## <a name="platforms-and-frameworks"></a>Platforms en frameworks
### <a name="instrumentation-for-already-deployed-applications-codeless-agent-based"></a>Instrumentatie voor toepassingen die al zijn geïmplementeerd (zonder code, op basis van een agent)
* [Azure VM-en Azure virtual machine-schaal sets](../../azure-monitor/app/azure-vm-vmss-apps.md)
* [Azure App Service](../../azure-monitor/app/azure-web-apps.md)
* [ASP.NET - voor apps die al live zijn](../../azure-monitor/app/monitor-performance-live-website-now.md)
* [Azure Cloud Services](../../azure-monitor/app/cloudservices.md), inclusief de web-en werk rollen
* [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-monitoring)
### <a name="instrumentation-through-code-sdks"></a>Instrumentatie via code (Sdk's)
* [ASP.NET](../../azure-monitor/app/asp-net.md)
* [ASP.NET Core](../../azure-monitor/app/asp-net-core.md)
* [Android](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Android](https://github.com/Microsoft/ApplicationInsights-Android) (App Center)
* [iOS](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Java EE](../../azure-monitor/app/java-get-started.md)
* [Node.JS](https://www.npmjs.com/package/applicationinsights)
* [Universele Windows-app](../../azure-monitor/learn/mobile-center-quickstart.md) (App Center)
* [Windows-bureaubladtoepassingen, -services en -werkrollen](../../azure-monitor/app/windows-desktop.md)

## <a name="logging-frameworks"></a>Frameworks voor logboekregistratie
* [ILogger](https://docs.microsoft.com/azure/azure-monitor/app/ilogger)
* [Log4Net, NLog of System.Diagnostics.Trace](../../azure-monitor/app/asp-net-trace-logs.md)
* [Java, Log4J of Logback](../../azure-monitor/app/java-trace-logs.md)
* [LogStash-invoegtoepassing](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-output-applicationinsights)
* [Azure Monitor](https://blogs.technet.microsoft.com/msoms/2016/09/26/application-insights-connector-in-oms/)

## <a name="export-and-data-analysis"></a>Exporteren en gegevens analyse
* [Power BI](https://blogs.msdn.com/b/powerbi/archive/2015/11/04/explore-your-application-insights-data-with-power-bi.aspx)
* [Stream Analytics](../../azure-monitor/app/export-power-bi.md)
