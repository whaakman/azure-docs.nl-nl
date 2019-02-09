---
title: Slimme detectie in Azure Application Insights | Microsoft Docs
description: Application Insights voert een automatische grondige analyse van uw app-Telemetrie en waarschuwt u mogelijke problemen.
services: application-insights
documentationcenter: windows
author: mrbullwinkle
manager: carmonm
ms.assetid: 2eeb4a35-c7a1-49f7-9b68-4f4b860938b2
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: mbullwin
ms.openlocfilehash: edbd7000001ae6927078e2f1bb9e348cc78f9efa
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/08/2019
ms.locfileid: "55962108"
---
# <a name="smart-detection-in-application-insights"></a>Slimme detectie in Application Insights
 Slimme detectie waarschuwt automatisch u over potentiële prestatieproblemen in uw webtoepassing. Het voert proactieve analyse van de telemetrie die uw app naar verzendt [Application Insights](../../azure-monitor/app/app-insights-overview.md). Als er een plotselinge toename van de foutpercentages of abnormale patronen in de prestaties van de client of server is, krijgt u een waarschuwing. Deze functie heeft geen configuratie nodig. Het werkt als uw toepassing zoveel telemetrie verzendt.

U kunt waarschuwingen voor slimme detectie openen van de e-mailberichten ontvangt u, en van de blade voor slimme detectie.

## <a name="review-your-smart-detections"></a>Controleer uw Slimme detectie
U kunt detectie op twee manieren detecteren:

* **U ontvangt een e-mailbericht** van Application Insights. Hier volgt een voorbeeld:
  
    ![E-mailmelding](./media/proactive-diagnostics/03.png)
  
    Klik op de grote knop om meer details in de portal.
* **De tegel voor slimme detectie** op van uw app-overzicht blade geeft het aantal recente waarschuwingen. Klik op de tegel voor een overzicht van recente waarschuwingen.

![Recente detecties weergeven](./media/proactive-diagnostics/04.png)

Selecteer een waarschuwing om de details te bekijken.

## <a name="what-problems-are-detected"></a>Welke problemen worden gedetecteerd?
Er zijn drie soorten detectie:

* [Slimme detectie: afwijkende fouten](../../azure-monitor/app/proactive-failure-diagnostics.md). We gebruiken voor machine learning om in te stellen de verwachte frequentie van mislukte aanvragen voor uw app, correleren met belasting en andere factoren. Als het aantal fouten buiten de verwachte envelop gaat, sturen we een waarschuwing.
* [Slimme detectie: afwijkende prestaties](../../azure-monitor/app/proactive-performance-diagnostics.md). U vooraf meldingen krijgt als de reactietijd van een duur van de bewerking of een afhankelijkheid is steeds trager wordt vergeleken met de historische basislijn of als we een afwijkende patroon in de reactietijd of laadtijd van browserpagina identificeren.   
* [Slimme detectie - problemen met de Azure Cloud Service](https://azure.microsoft.com/blog/proactive-notifications-on-cloud-service-issues-with-azure-diagnostics-and-application-insights/). U kunt waarschuwingen krijgen als uw app wordt gehost in Azure Cloud Services en een rolinstantie heeft problemen met opstarten, regelmatige recycling of crashes runtime.

(De help-koppelingen in elke melding gaat u naar de relevante artikelen.)

## <a name="smart-detection-email-notifications"></a>E-mailmeldingen voor slimme detectie

Alle regels voor slimme detectie, met uitzondering van regels die zijn gemarkeerd als Preview-versie zijn standaard geconfigureerd voor e-mailmeldingen verzenden wanneer detecties worden gevonden.

Configureren van e-mailmeldingen voor een specifieke regel voor slimme detectie kan worden gedaan door het openen van de Slimme detectie **instellingen** blade en selecteren van de regel, dat wordt geopend met de **regel bewerken** blade.

U kunt ook kunt u de configuratie met behulp van Azure Resource Manager-sjablonen. [Regels voor slimme detectie van Application Insights beheren met behulp van Azure Resource Manager-sjablonen Zie](https://docs.microsoft.com/azure/azure-monitor/app/proactive-arm-config) voor meer informatie.

## <a name="video"></a>Video

> [!VIDEO https://channel9.msdn.com/events/Connect/2016/112/player]

## <a name="next-steps"></a>Volgende stappen
Deze diagnostische hulpprogramma's kunnen u de telemetrie van uw app controleren:

* [Metric explorer](../../azure-monitor/app/metrics-explorer.md)
* [Search explorer](../../azure-monitor/app/diagnostic-search.md)
* [Analytics - krachtige querytaal](../../azure-monitor/log-query/get-started-portal.md)

Slimme detectie is volledig automatisch. Maar misschien u graag enkele meer waarschuwingen instellen?

* [Handmatig geconfigureerde metrische waarschuwingen](../../azure-monitor/app/alerts.md)
* [Webtests voor beschikbaarheid](../../azure-monitor/app/monitor-web-app-availability.md) 

