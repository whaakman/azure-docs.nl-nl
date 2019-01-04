---
title: Haal meer uit Azure Application Insights | Microsoft Docs
description: Nadat het aan de slag met Application Insights, als volgt een overzicht van de functies die u kunt verkennen.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 7ec10a2d-c669-448d-8d45-b486ee32c8db
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/03/2017
ms.author: mbullwin
ms.openlocfilehash: 7580089782eb9330d5b533588265d156213f397f
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "54000577"
---
# <a name="more-telemetry-from-application-insights"></a>Meer telemetrie van Application Insights
Nadat u hebt [Application Insights toevoegen aan uw ASP.NET-code](../../azure-monitor/app/asp-net.md), er zijn enkele dingen die u doen kunt om nog meer telemetrie ophalen. 

| Bewerking | Wat u krijgt|
|---|---|
|(IIS-servers) [Installeer Status Monitor](https://go.microsoft.com/fwlink/?LinkId=506648) op elke servermachine.<br/>(Azure-web-apps) Open de Application Insights-blade in de Azure het Configuratiescherm voor de web-app.| [**Prestatiemeteritems**](../../azure-monitor/app/performance-counters.md)<br/>[**Uitzonderingen** ](asp-net-exceptions.md) - gedetailleerde traceringen stack<br/>[**Afhankelijkheden**](../../azure-monitor/app/asp-net-dependencies.md)|
|[Voeg het JavaScript-fragment toe aan uw webpagina 's](../../azure-monitor/app/javascript.md)|[Pagina prestaties](../../application-insights/app-insights-usage-overview.md), browseruitzonderingen, prestaties van AJAX. Aangepaste telemetrie van de client-side.|
|[Maak webtests voor beschikbaarheid](../../azure-monitor/app/monitor-web-app-availability.md)|Waarschuwingen ontvangen als uw site beschikbaar is|
|[Zorg ervoor dat buildinfo.config](https://msdn.microsoft.com/library/dn449058.aspx) wordt gegenereerd door het MSBuild|[Maken van aantekeningen in grafieken met metrische gegevens](https://blogs.msdn.microsoft.com/visualstudioalm/2013/11/14/implementing-deployment-markers-in-application-insights/)
|[Aangepaste gebeurtenissen en metrische gegevens schrijven](../../azure-monitor/app/api-custom-events-metrics.md)|Aantal zakelijke gebeurtenissen en metrische gegevens, en meer gedetailleerde informatie over gebruik bijhouden.|
|[Profileren van uw live site](https://aka.ms/AIProfilerPreview)|Gedetailleerde functie tijdsinstellingen van uw live web-app|






