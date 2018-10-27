---
title: Prestaties van Azure-web-apps controleren | Microsoft Docs
description: Prestaties controleren voor Azure-web-apps. Laad- en reactietijd voor grafieken, afhankelijkheidsinformatie en waarschuwingen instellen voor prestaties.
services: application-insights
documentationcenter: .net
author: mrbullwinkle
manager: carmonm
ms.assetid: 0b2deb30-6ea8-4bc4-8ed0-26765b85149f
ms.service: application-insights
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/25/2018
ms.author: mbullwin
ms.openlocfilehash: 91b050c4e1ca37c0af208d6df1ce4f8272235294
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50139768"
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

    ![Kies Application Insights onder instellingen.](./media/app-insights-azure-web-apps/settings-app-insights.png)

   * Te kiezen om een nieuwe resource te maken, tenzij u al een Application Insights-resource voor deze toepassing instellen. 

    > [!NOTE]
    > Wanneer u klikt op **OK** te maken van de nieuwe resource wordt u gevraagd naar **controle-instellingen toepassen**. Selecteren **doorgaan** uw nieuwe Application Insights-resource wordt gekoppeld aan uw web-app, dit ook **activeert u opnieuw opstarten van uw web-app**. 

    ![Uw web-app instrumenteren](./media/app-insights-azure-web-apps/create-resource.png)

2. Zodra de resource is gemaakt Selecteer **controleren en Application Insights-extensie voor de web-app-update** om te bevestigen dat de extensie up-to-date is.

     ![Controleren en bijwerken van de extensie voor site](./media/app-insights-azure-web-apps/check-and-update.png)

3. **Instrumenteer uw web-app** nadat Application Insights is geïnstalleerd.

   **Schakel bewaking aan clientzijde in** voor paginaweergave- en gebruikerstelemetrie.

   * Selecteer Instellingen > Toepassingsinstellingen
   * Voeg een nieuw sleutelwaardepaar toe bij App-instellingen:

    Sleutel: `APPINSIGHTS_JAVASCRIPT_ENABLED`

    Waarde:`true`
   * Sla de instellingen op met **Opslaan** en start de app opnieuw met **Opnieuw opstarten**.
4. Controle van uw app-gegevens verkennen door te selecteren **instellingen** > **Application Insights** > **meer in Application Insights weergeven**.

Desgewenst kunt u de app later bouwen met Application Insights.

*Hoe kan ik Application Insights verwijderen of overschakelen naar verzending naar een andere resource?*

* Open in Azure de blade voor web-app-beheer en open **Extensies** onder Ontwikkelprogramma's. Verwijder de extensie Application Insights. Kies vervolgens Application Insights onder Controleren en maak of selecteer de gewenste resource.

## <a name="build-the-app-with-application-insights"></a>De app bouwen met Application Insights
Application Insights kan gedetailleerdere telemetrie verstrekken door een SDK in uw app te installeren. U kunt met name traceerlogboeken verzamelen, [aangepaste telemetrie schrijven](app-insights-api-custom-events-metrics.md) en gedetailleerdere uitzonderingsrapporten ophalen.

1. Configureer **in Visual Studio** (2013-update 2 of hoger) Application Insights voor uw project.

    Met de rechtermuisknop op het webproject en selecteer **toevoegen > Application Insights** of **Project** > **Application Insights**  >  **Application Insights configureren**.

    ![Klik met de rechtermuisknop op het webproject en kies Application Insights toevoegen of configureren.](./media/app-insights-azure-web-apps/03-add.png)

    Als u wordt gevraagd om u aan te melden, gebruikt u de referenties voor uw Azure-account.

    De bewerking zorgt voor twee effecten:

   1. Maakt een Application Insights-resource in Azure, waarbij telemetrie wordt opgeslagen, geanalyseerd en weergegeven.
   2. Voegt het pakket Application Insights NuGet toe aan uw code (als die er nog niet is) en configureert deze zodanig dat telemetrie wordt verzonden naar de Azure-resource.
2. **Test de telemetrie** door de app uit te voeren op uw ontwikkelcomputer (F5).
3. **Publiceer de app** naar Azure op de gebruikelijke manier. 

*Hoe schakel ik over om naar een andere Application Insights-resource te verzenden?*

* Klik in Visual Studio met de rechtermuisknop op het project, kies **Application Insights configureren** en kies de gewenste resource. U krijgt de mogelijkheid om een nieuwe resource te maken. Opnieuw maken en implementeren

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

