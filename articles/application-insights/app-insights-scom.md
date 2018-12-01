---
title: SCOM-integratie met Application Insights | Microsoft Docs
description: Als u een SCOM-gebruiker bent, prestaties bewaken en diagnosticeren van problemen met Application Insights. Uitgebreide dashboards, slimme waarschuwingen, krachtige diagnostische hulpmiddelen en analyseren van query's.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 08/20/2018
ms.author: mbullwin
ms.openlocfilehash: d7c3134c36707fcfa079968d19d93b73d7a5f5cc
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52724074"
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a>Toepassingsprestaties bewaken met Application Insights voor SCOM
Als u System Center Operations Manager (SCOM) gebruikt voor het beheren van uw servers, kunt u prestaties bewaken en diagnosticeren van prestatieproblemen met behulp van [Azure Application Insights](app-insights-asp-net.md). Application Insights bewaakt uw webtoepassing inkomende aanvragen, uitgaande REST en SQL-aanroepen, uitzonderingen en logboektraceringen. Het biedt dashboards met grafieken met metrische gegevens en slimme waarschuwingen, evenals krachtige diagnostische gegevens doorzoeken en analytische query's in deze telemetrie. 

U kunt overschakelen op de bewaking van Application Insights met behulp van een managementpack van SCOM.

> [!IMPORTANT]
> Dit Management Pack voor System Center Operations Manager is nu **afgeschaft**. Het biedt geen ondersteuning voor de meest recente Application Insights-SDK's en niet meer wordt aanbevolen.

## <a name="before-you-start"></a>Voordat u begint
We nemen:

* U bekend bent met SCOM en SCOM 2012 R2 of 2016 te gebruiken voor het beheren van uw IIS-webservers.
* U hebt al geïnstalleerd op uw servers een webtoepassing die u wilt controleren met Application Insights.
* App-framework-versie is .NET 4.5 of hoger.
* U hebt toegang tot een abonnement in [Microsoft Azure](https://azure.com) en kunnen zich aanmelden bij de [Azure-portal](https://portal.azure.com). Uw organisatie kan een abonnement hebben en aan uw Microsoft-account kunt toevoegen.

(Het ontwikkelingsteam kan bouwen de [Application Insights-SDK](app-insights-asp-net.md) in de web-app. Deze build-runtime-instrumentatiesleutel biedt ze meer flexibiliteit bij het schrijven van aangepaste telemetrie. Echter, het maakt niet uit: u kunt de stappen die hier worden beschreven, met of zonder de SDK al ingebouwd.)

## <a name="one-time-install-application-insights-management-pack"></a>(Één keer) Application Insights managementpack installeren
Op de computer waarop u Operations Manager uitvoert:

1. Een oude versie van het managementpack verwijderen:
   1. In Operations Manager, beheer, Management Packs te openen. 
   2. Verwijder de oude versie.
2. Download en installeer het managementpack uit de catalogus.
3. Operations Manager opnieuw.

## <a name="create-a-management-pack"></a>Een managementpack maken
1. Open in de Operations Manager, **ontwerpen**, **.NET... met de Application Insights**, **Wizard bewaking toevoegen**, en kies nogmaals **.NET... met de toepassing Insights**.
   
    ![](./media/app-insights-scom/020.png)
2. De naam van de configuratie na uw app. (Als u één app tegelijk softwareontwikkelaars hebben.)
   
    ![](./media/app-insights-scom/030.png)
3. Maak een nieuw managementpack op dezelfde wizardpagina, of Selecteer een pakket dat u eerder voor Application Insights hebt gemaakt.
   
     (De Application Insights [managementpack](https://technet.microsoft.com/library/cc974491.aspx) is een sjabloon, van waaruit u een exemplaar maken. U kunt opnieuw gebruiken hetzelfde exemplaar later opnieuw.)

    ![Typ de naam van de app op het tabblad algemene eigenschappen. Klik op Nieuw en typ een naam voor een managementpack. Klik op OK en klik op volgende.](./media/app-insights-scom/040.png)

1. Kies een app die u wilt bewaken. De zoekfunctie zoekt tussen apps die zijn geïnstalleerd op uw servers.
   
    ![Over wat u moet het tabblad Monitor, klik op toevoegen, typt u deel van naam van de app, en klik op zoeken, kiest u de app en klikt u vervolgens op toevoegen, OK.](./media/app-insights-scom/050.png)
   
    Optioneel veld van de scope bewaking kan worden gebruikt om op te geven van een subset van uw servers, als u niet wilt voor het bewaken van de app in alle servers.
2. Op de volgende wizardpagina gaan, moet u eerst Geef uw referenties aanmelden bij Microsoft Azure.
   
    Op deze pagina kunt u de Application Insights-resource waar u de telemetriegegevens worden geanalyseerd en weergegeven. 
   
   * Als de toepassing is geconfigureerd voor Application Insights tijdens de ontwikkeling, selecteert u de bestaande resource.
   * Anders maakt u een nieuwe resource met de naam van de app. Als er andere apps die deel uitmaken van hetzelfde systeem, plaatst u deze in dezelfde resourcegroep bevinden, om toegang tot de telemetrie gemakkelijker te beheren.
     
     U kunt deze instellingen later wijzigen.
     
     ![Op het tabblad instellingen van Application Insights, klikt u op 'aanmelden' en geef de referenties van uw Microsoft-account voor Azure. Kies een abonnement, resourcegroep en resources.](./media/app-insights-scom/060.png)
3. Voltooi de wizard.
   
    ![Klik op Maken](./media/app-insights-scom/070.png)

Herhaal deze procedure voor elke app die u wilt bewaken.

Als u wilt deze instellingen later wijzigen, opent u opnieuw de eigenschappen van de monitor van het venster ontwerpen.

![In het ontwerp, selecteer .NET Application Performance Monitoring met Application Insights, selecteert u de monitor en klikt u op Eigenschappen.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a>Controleer of bewaking
De monitor die u zoekt naar uw app hebt geïnstalleerd op elke server. Gevonden waar de app, configureert het Application Insights Status Monitor om de app te bewaken. Indien nodig, worden eerst Status Monitor op de server installeert.

U kunt controleren welke instanties van de app heeft gevonden:

![Open in de bewaking, Application Insights](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a>Telemetrie in Application Insights weergeven
In de [Azure-portal](https://portal.azure.com), blader naar de resource voor uw app. U [grafieken met de telemetrie bekijken](app-insights-dashboards.md) vanuit uw app. (Als deze nog niet op de hoofdpagina nog weergegeven, klikt u op Live Metrics Stream.)

## <a name="next-steps"></a>Volgende stappen
* [Instellen van een dashboard](app-insights-dashboards.md) samenbrengen van de belangrijkste grafieken bewaking van deze en andere apps.
* [Meer informatie over metrische gegevens](app-insights-metrics-explorer.md)
* [Waarschuwingen instellen](app-insights-alerts.md)
* [Oorzaak van prestatieproblemen achterhalen](app-insights-detect-triage-diagnose.md)
* [Krachtige Analytics-query 's](app-insights-analytics.md)
* [Webtests voor beschikbaarheid](app-insights-monitor-web-app-availability.md)

