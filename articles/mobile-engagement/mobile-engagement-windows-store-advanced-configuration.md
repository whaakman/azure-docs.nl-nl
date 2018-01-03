---
title: Geavanceerde configuratie voor Windows universele Apps Engagement SDK
description: Geavanceerde configuratieopties voor Azure Mobile Engagement met universele Windows-Apps
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6d85dd5d-ac07-43ba-bbe4-e91c3a17690b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: cb9454212c94cf65093219c3d24c71277ede7877
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Geavanceerde configuratie voor Windows universele Apps Engagement SDK
> [!div class="op_single_selector"]
> * [Universeel Windows](mobile-engagement-windows-store-advanced-configuration.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-configuration.md)
> 
> 

Deze procedure wordt beschreven hoe verschillende configuratieopties voor Azure Mobile Engagement Android-apps configureren.

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="advanced-configuration"></a>Geavanceerde configuratie
### <a name="disable-automatic-crash-reporting"></a>Automatische crashrapporten uitschakelen
U kunt de automatische crash rapportagefunctie van Engagement uitschakelen. Klik, wanneer er een niet-verwerkte uitzondering optreedt, Engagement, gebeurt er niets.

> [!WARNING]
> Als u deze functie uitschakelen en vervolgens als een niet-verwerkte-crash in uw app plaatsvindt, Engagement geen de crash verzendt **en** wordt de sessie en taken niet gesloten.
> 
> 

Schakel automatische crashrapporten aanpassen van uw configuratie, afhankelijk van de manier waarop die u het gedeclareerd:

#### <a name="from-engagementconfigurationxml-file"></a>Van `EngagementConfiguration.xml` bestand
Rapport crash ingesteld op `false` tussen `<reportCrash>` en `</reportCrash>` labels.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Van `EngagementConfiguration` object tijdens runtime
Rapport crash ingesteld op false met behulp van uw EngagementConfiguration-object.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Uitschakelen van realtime rapportage
Standaard wordt de Engagement-service-rapporten Logboeken in realtime. Als uw toepassing wordt vaak logboeken rapporteert, is het beter voor het bufferen van de logboeken en in één keer rapporteren op regelmatige basis. Dit heet 'burst modus'.

Roep de methode om dit te doen:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Het argument is een waarde in **milliseconden**. Als u activeren, de real-time logboekregistratie wilt, roept u de methode geen parameters of met de waarde 0.

Burst-modus verhoogt de batterij enigszins, maar heeft een invloed op de Engagement-Monitor: alle sessies en taken duur zijn afgerond op de drempelwaarde burst (dus sessies en korter zijn dan de drempelwaarde burst is mogelijk niet zichtbaar taken). U kunt het beste een ' burst ' drempelwaarde niet langer dan 30000 (30s) gebruikt. Opgeslagen logboeken zijn beperkt tot 300 items. Als het verzenden te lang is, kunt u sommige logboeken gaan verloren.

> [!WARNING]
> De drempelwaarde burst kan niet worden geconfigureerd op een periode van minder dan één seconde. Als u doet dit, wordt de SDK bevat een tracering met de fout en stelt automatisch opnieuw op de standaardwaarde nul seconden. Dit activeert de SDK om de logboeken in realtime te rapporteren.
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview
