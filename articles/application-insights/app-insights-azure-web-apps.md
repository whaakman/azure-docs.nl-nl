---
title: Prestaties van Azure-web-apps controleren | Microsoft Docs
description: Prestaties controleren voor Azure-web-apps. Laad- en reactietijd voor grafieken, afhankelijkheidsinformatie en waarschuwingen instellen voor prestaties.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: azure-portal
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/05/2017
ms.author: mbullwin
ms.openlocfilehash: e3615280ec902833248d9acc1c9348d68e5c5e82
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="monitor-azure-web-app-performance"></a>Prestaties van Azure-web-apps controleren
In de [Azure Portal](https://portal.azure.com) kunt u instellen dat de prestaties van uw [Azure-web-apps](../app-service/app-service-web-overview.md) worden gecontroleerd. Met [Azure Application Insights](app-insights-overview.md) instrumenteert u uw app om telemetrie over de eigen activiteiten te sturen naar de Application Insights-service, waar de gegevens worden opgeslagen en geanalyseerd. Daar kunnen metrische grafieken en zoekfuncties worden gebruikt om problemen vast te stellen, prestaties te verbeteren en het gebruik te beoordelen.

## <a name="run-time-or-build-time"></a>Runtime of buildtime
U kunt de controle configureren door de app op twee manieren te instrumenteren:

* **Runtime**: u kunt een uitbreiding van de prestatiecontrole selecteren wanneer uw web-app al gepubliceerd is. Het is niet nodig uw app opnieuw te bouwen of opnieuw te installeren. U ontvangt een standaardset aan pakketten voor het controleren van reactietijden, succespercentages, uitzonderingen, afhankelijkheden, enzovoort. 
* **Buildtime**: u kunt een pakket installeren in uw app in ontwikkeling. Deze optie is veelzijdiger. Naast dezelfde standaardpakketten kunt u code schrijven voor het aanpassen van de telemetrie of voor het verzenden van uw eigen telemetrie. U kunt specifieke activiteiten of gebeurtenissen vastleggen volgens de semantiek van uw app-domein. 

## <a name="run-time-instrumentation-with-application-insights"></a>Runtime-instrumentatiesleutel met Application Insights
Als u al een web-app uitvoert in Azure, is er al sprake van enige controle: frequentie van aanvragen en fouten. Voeg Application Insights toe om de controle uit te breiden met reactietijden, controle van het aanroepen van afhankelijkheden, slimme detectie en de krachtige Log Analytics-querytaal. 

1. **Selecteer Application Insights** in het Azure-configuratiescherm voor uw web-app.
   
    ![Kies Application Insights onder Controleren.](./media/app-insights-azure-web-apps/05-extend.png)
   
   * Kies voor het maken van een nieuwe resource, tenzij u voor deze app al langs een andere weg een Application Insights-resource hebt ingesteld.
2. **Instrumenteer uw web-app** nadat Application Insights is geïnstalleerd. 
   
    ![Uw web-app instrumenteren](./media/app-insights-azure-web-apps/restart-web-app-for-insights.png)

   **Schakel bewaking aan clientzijde in** voor paginaweergave- en gebruikerstelemetrie.

   * Selecteer Instellingen > Toepassingsinstellingen
   * Voeg een nieuw sleutelwaardepaar toe bij App-instellingen: 
   
    Sleutel: `APPINSIGHTS_JAVASCRIPT_ENABLED` 
    
    Waarde:`true`
   * Sla de instellingen op met **Opslaan** en start de app opnieuw met **Opnieuw opstarten**.
3. **Controleer uw app**.  [Verken de gegevens](#explore-the-data).

Desgewenst kunt u de app later bouwen met Application Insights.

*Hoe kan ik Application Insights verwijderen of overschakelen naar verzending naar een andere resource?*

* Open in Azure de blade voor web-app-beheer en open **Extensies** onder Ontwikkelprogramma's. Verwijder de extensie Application Insights. Kies vervolgens Application Insights onder Controleren en maak of selecteer de gewenste resource.

## <a name="build-the-app-with-application-insights"></a>De app bouwen met Application Insights
Application Insights kan gedetailleerdere telemetrie verstrekken door een SDK in uw app te installeren. U kunt met name traceerlogboeken verzamelen, [aangepaste telemetrie schrijven](app-insights-api-custom-events-metrics.md) en gedetailleerdere uitzonderingsrapporten ophalen.

1. Configureer **in Visual Studio** (2013-update 2 of hoger) Application Insights voor uw project.

    Klik met de rechtermuisknop op het webproject en selecteer **Toevoegen > Application Insights** of **Application Insights configureren**.
   
    ![Klik met de rechtermuisknop op het webproject en kies Application Insights toevoegen of configureren.](./media/app-insights-azure-web-apps/03-add.png)
   
    Als u wordt gevraagd om u aan te melden, gebruikt u de referenties voor uw Azure-account.
   
    De bewerking zorgt voor twee effecten:
   
   1. Maakt een Application Insights-resource in Azure, waarbij telemetrie wordt opgeslagen, geanalyseerd en weergegeven.
   2. Voegt het pakket Application Insights NuGet toe aan uw code (als die er nog niet is) en configureert deze zodanig dat telemetrie wordt verzonden naar de Azure-resource.
2. **Test de telemetrie** door de app uit te voeren op uw ontwikkelcomputer (F5).
3. **Publiceer de app** naar Azure op de gebruikelijke manier. 

*Hoe schakel ik over om naar een andere Application Insights-resource te verzenden?*

* Klik in Visual Studio met de rechtermuisknop op het project, kies **Application Insights configureren** en kies de gewenste resource. U krijgt de mogelijkheid om een nieuwe resource te maken. Opnieuw maken en implementeren

## <a name="explore-the-data"></a>De gegevens verkennen
1. Op de Application Insights-blade van het web-app-configuratiescherm ziet u Live Metrics, waar aanvragen en fouten binnen luttele seconden nadat ze zijn opgetreden worden weergegeven. Dit is bijzonder handige informatie wanneer u uw app opnieuw publiceert, omdat u problemen onmiddellijk te zien krijgt.
2. Klik door naar de volledige Application Insights-resource.

    ![Doorklikken](./media/app-insights-azure-web-apps/view-in-application-insights.png)

    U kunt er ook rechtstreeks naartoe gaan via Azure-resourcenavigatie.

1. Klik op een grafiek voor gedetailleerdere gegevens:
   
    ![Klik op een grafiek in de Application Insights-overzichtsblade](./media/app-insights-azure-web-apps/07-dependency.png)
   
    U kunt [metrische blades aanpassen](app-insights-metrics-explorer.md).
2. Klik door om afzonderlijke gebeurtenissen en hun eigenschappen te zien:
   
    ![Klik op een gebeurtenistype om een zoekactie te openen die op dat type is gefilterd](./media/app-insights-azure-web-apps/08-requests.png)
   
    Let op de '...'-koppeling waarmee u alle eigenschappen kunt openen.
   
    U kunt [zoekacties aanpassen](app-insights-diagnostic-search.md).

Gebruik de [Log Analytics-querytaal](app-insights-analytics-tour.md) voor krachtigere zoekacties in uw telemetrie.

## <a name="more-telemetry"></a>Meer telemetrie

* [Gegevens voor laden van webpagina](app-insights-javascript.md)
* [Aangepaste telemetrie](app-insights-api-custom-events-metrics.md)

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/100/player]

## <a name="next-steps"></a>Volgende stappen
* [Voer de profiler uit in uw live app](app-insights-profiler.md).
* [Azure Functions](https://github.com/christopheranderson/azure-functions-app-insights-sample): Azure Functions bewaken met Application Insights
* [Schakel diagnostische Azure-gegevens in](app-insights-azure-diagnostics.md) om te verzenden naar Application Insights.
* [Controleer metrische gegevens voor servicestatus](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) om ervoor te zorgen dat de service beschikbaar is en reageert.
* [Ontvang waarschuwingsmeldingen](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) wanneer er operationele gebeurtenissen plaatsvinden of metrische gegevens een drempelwaarde overschrijden.
* Gebruik [Application Insights voor JavaScript-apps en -webpagina's](app-insights-javascript.md) om clienttelemetrie op te halen uit de browsers die een webpagina bezoeken.
* [Stel webtests voor beschikbaarheid in](app-insights-monitor-web-app-availability.md) om te worden gewaarschuwd als uw site niet actief is.

