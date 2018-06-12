---
title: Haal meer uit Azure Application Insights | Microsoft Docs
description: Na het aan de slag met Application Insights, volgt hier een samenvatting van de functies die u kunt verkennen.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 7ec10a2d-c669-448d-8d45-b486ee32c8db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 02/03/2017
ms.author: mbullwin
ms.openlocfilehash: 2f03083367de4e818bdc953ab76c28ff687f0a48
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35294333"
---
# <a name="more-telemetry-from-application-insights"></a>Meer telemetrie van Application Insights
Nadat u hebt [Application Insights toegevoegd aan uw ASP.NET-code](app-insights-asp-net.md), enkele dingen die u doen kunt om nog meer telemetrie ophalen. 

| Bewerking | Wat u krijgt|
|---|---|
|(IIS-servers) [Status Monitor installeren](http://go.microsoft.com/fwlink/?LinkId=506648) op elke servermachine.<br/>(Azure-web-apps) Open de blade Application Insights in het Azure Configuratiescherm voor de web-app.| [**Prestatiemeteritems**](app-insights-performance-counters.md)<br/>[**Uitzonderingen** ](app-insights-asp-net-exceptions.md) - gedetailleerde traceringen<br/>[**Afhankelijkheden**](app-insights-asp-net-dependencies.md)|
|[De JavaScript-fragment toevoegen aan uw webpagina 's](app-insights-javascript.md)|[Pagina prestaties](app-insights-web-track-usage.md), browseruitzonderingen, AJAX-prestaties. Aangepaste telemetrie van de clientzijde.|
|[Maak webtests voor beschikbaarheid](app-insights-monitor-web-app-availability.md)|Ontvang waarschuwingen als uw site uitvalt.|
|[Zorg ervoor dat buildinfo.config](https://msdn.microsoft.com/library/dn449058.aspx) wordt gegenereerd door het MSBuild|[Aantekeningen in metrische grafieken bouwen](https://blogs.msdn.microsoft.com/visualstudioalm/2013/11/14/implementing-deployment-markers-in-application-insights/)
|[Schrijven van aangepaste gebeurtenissen en metrische gegevens](app-insights-api-custom-events-metrics.md)|Zakelijke gebeurtenissen en metrische gegevens tellen, en meer gedetailleerde informatie over het gebruik bijhouden.|
|[Profiel van uw live site](https://aka.ms/AIProfilerPreview)|Gedetailleerde functie tijdsinstellingen van uw live web-app|






