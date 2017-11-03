---
title: SCOM-integratie met Application Insights | Microsoft Docs
description: Als u een SCOM-gebruiker bent, prestaties bewaken en onderzoeken van problemen met Application Insights. Uitgebreide dashboards, slimme waarschuwingen, krachtige diagnostische hulpprogramma's en analyse query's.
services: application-insights
documentationcenter: 
author: mrbullwinkle
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/12/2016
ms.author: mbullwin
ms.openlocfilehash: 35ea37b751909e14e616a965462b832e4e51bae0
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a>Toepassingsprestaties bewaken met Application Insights voor SCOM
Als u System Center Operations Manager (SCOM) gebruikt voor het beheren van uw servers, kunt u prestaties bewaken en onderzoeken van prestatieproblemen met behulp van [Azure Application Insights](app-insights-asp-net.md). Application Insights wordt uw webtoepassing inkomende aanvragen en uitgaande REST en SQL-aanroepen, uitzonderingen en logboektraceringen bewaakt. Het biedt dashboards metrische grafieken en slimme waarschuwingen, evenals krachtige diagnostische gegevens doorzoeken en analytische query's via deze telemetrie. 

U kunt overschakelen op de bewaking van Application Insights met behulp van een SCOM management pack.

## <a name="before-you-start"></a>Voordat u begint
We gaan ervan uit:

* U bekend bent met SCOM en SCOM 2012 R2 of 2016 te gebruiken voor het beheren van uw IIS-webservers.
* U hebt al geïnstalleerd op de servers die een webtoepassing die u wilt bewaken met Application Insights.
* App-framework-versie is .NET 4.5 of hoger.
* U hebt toegang tot een abonnement in [Microsoft Azure](https://azure.com) en zich aanmelden bij de [Azure-portal](https://portal.azure.com). Uw organisatie wellicht een abonnement en aan uw Microsoft-account kunt toevoegen.

(Het ontwikkelingsteam kan maken de [Application Insights-SDK](app-insights-asp-net.md) in de web-app. Deze instrumentation build-tijd biedt deze meer flexibiliteit bij het schrijven van aangepaste telemetrie. Echter, het maakt niet uit: u kunt de stappen die hier worden beschreven, met of zonder de SDK al ingebouwd.)

## <a name="one-time-install-application-insights-management-pack"></a>(Eenmaal) Application Insights management pack installeren
Op de computer waarop u Operations Manager uitvoert:

1. Een oude versie van het management pack verwijderen:
   1. Open in Operations Manager, beheer-, Management Packs. 
   2. Verwijder de oude versie.
2. Download en installeer het management pack uit de catalogus.
3. Start Operations Manager opnieuw.

## <a name="create-a-management-pack"></a>Een management pack maken
1. Open in Operations Manager **ontwerpen**, **.NET... met Application Insights**, **Wizard bewaking toevoegen**, en kies opnieuw **.NET... met de toepassing Insights**.
   
    ![](./media/app-insights-scom/020.png)
2. De naam van de configuratie na uw app. (Als u één app tegelijk softwareontwikkelaars hebben.)
   
    ![](./media/app-insights-scom/030.png)
3. Maak een nieuw management pack op dezelfde wizardpagina, of Selecteer een pakket dat u eerder voor Application Insights hebt gemaakt.
   
     (De Application Insights [management pack](https://technet.microsoft.com/library/cc974491.aspx) is een sjabloon, waaruit u een exemplaar maken. U kunt later opnieuw gebruiken hetzelfde exemplaar.)

    ![Typ de naam van de app in het tabblad algemene eigenschappen. Klik op Nieuw en typ een naam voor een management pack. Klik op OK en klik op volgende.](./media/app-insights-scom/040.png)

1. Kies een app die u wilt bewaken. De zoekfunctie zoekt tussen apps die zijn geïnstalleerd op uw servers.
   
    ![Over wat u moet het tabblad Monitor, klik op toevoegen, typt u deel van naam van de app, klik op zoeken, kies de app en vervolgens op toevoegen, OK.](./media/app-insights-scom/050.png)
   
    Het veld optionele bewaking scope kan worden gebruikt om op te geven van een subset van uw servers, als u niet wilt bewaken van de app in alle servers.
2. U moet uw referenties aan te melden bij Microsoft Azure opgeven op de volgende wizardpagina.
   
    Op deze pagina kunt u de Application Insights-resource waar u de telemetriegegevens worden geanalyseerd en weergegeven. 
   
   * Als de toepassing is geconfigureerd voor Application Insights tijdens de ontwikkeling, selecteert u de bestaande resource.
   * Anders maakt u een nieuwe resource met de naam van de app. Als er andere apps die deel uitmaken van hetzelfde systeem, plaatst u deze in dezelfde resourcegroep, om toegang aan de telemetrie gemakkelijker te beheren.
     
     U kunt deze instellingen later wijzigen.
     
     ![Klik op 'aanmelden' op het tabblad voor Application Insights-instellingen en geef de referenties van uw Microsoft-account voor Azure. Kies een abonnement, de resourcegroep en de resource.](./media/app-insights-scom/060.png)
3. Voltooi de wizard.
   
    ![Klik op Maken](./media/app-insights-scom/070.png)

Herhaal deze procedure voor elke app die u wilt bewaken.

Als u deze instellingen later wijzigen moet, opent u de eigenschappen van de monitor van het ontwerpen-venster opnieuw op.

![In het ontwerp, selecteer .NET Application Performance Monitoring met Application Insights, selecteer de monitor en klikt u op Eigenschappen.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a>Controleer of de bewaking
De monitor die u zoekt u naar uw app hebt geïnstalleerd op elke server. Wanneer zij de app constateert configureert het Application Insights Status Monitor om de app te bewaken. Indien nodig, worden eerst Status Monitor op de server installeert.

U kunt controleren welke exemplaren van de app heeft gevonden:

![Open in de bewaking, Application Insights](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a>Weergave telemetrie in Application Insights
In de [Azure-portal](https://portal.azure.com), blader naar de bron voor uw app. U [grafieken weergegeven telemetrie](app-insights-dashboards.md) van uw app. (Als dit nog niet wordt weergegeven om op de hoofdpagina nog, klikt u op livestream metrische gegevens.)

## <a name="next-steps"></a>Volgende stappen
* [Instellen van een dashboard](app-insights-dashboards.md) bij elkaar brengt de belangrijkste grafieken bewaking van deze en andere apps.
* [Meer informatie over metrische gegevens](app-insights-metrics-explorer.md)
* [Waarschuwingen instellen](app-insights-alerts.md)
* [Prestatieproblemen oplossen](app-insights-detect-triage-diagnose.md)
* [Krachtige Analytics-query 's](app-insights-analytics.md)
* [Webtests voor beschikbaarheid](app-insights-monitor-web-app-availability.md)

