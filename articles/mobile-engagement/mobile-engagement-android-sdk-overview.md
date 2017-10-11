---
title: Android SDK-integratie voor Azure Mobile Engagement
description: "Hierin wordt beschreven hoe u Azure Mobile Engagement SDK is geïntegreerd in de Android-apps"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a91ed04f-f3ce-4692-a6dd-b56a28d7dee8
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 07/17/2017
ms.author: piyushjo;ricksal
ms.openlocfilehash: 35935e911f1f17989beb71978396c6d1b7d601d6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="android-sdk-integration-for-azure-mobile-engagement"></a>Android SDK-integratie voor Azure Mobile Engagement
> [!div class="op_single_selector"]
> * [Universeel Windows](mobile-engagement-windows-store-sdk-overview.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-sdk-overview.md)
> * [iOS](mobile-engagement-ios-sdk-overview.md)
> * [Android](mobile-engagement-android-sdk-overview.md)
> 
> 

Dit document beschrijft alle integratie en configuratie van opties beschikbaar voor het Azure Mobile Engagement Android SDK.

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="advanced-features"></a>Geavanceerde functies
### <a name="reporting-features"></a>Rapportagefuncties
U kunt deze functies kunt toevoegen:

1. [Geavanceerde opties voor rapportage](mobile-engagement-android-advanced-reporting.md)
2. [Locatie rapportage-opties](mobile-engagement-android-location-reporting.md)
3. [Geavanceerde configuratieopties](mobile-engagement-android-advanced-configuration.md)

### <a name="notifications"></a>Meldingen:
[Het bereik (meldingen) integreren in uw Android-app](mobile-engagement-android-integrate-engagement-reach.md)

1. Google Cloud Messaging (GCM): [GCM integreren met Mobile Engagement](mobile-engagement-android-gcm-integrate.md)
2. Amazon Device Messaging (ADM): [ADM integreren met Mobile Engagement](mobile-engagement-android-adm-integrate.md)

### <a name="tag-plan-implementation"></a>Tag plan implementatie:
[Het gebruik van de geavanceerde Mobile Engagement tags API in uw Android-app](mobile-engagement-android-use-engagement-api.md)

## <a name="release-notes"></a>Releaseopmerkingen

### <a name="431-07172017"></a>4.3.1 (07/17/2017)
* Herstel van een crash die zelden optreden kan bij het aanroepen van `EngagementAgentUtils.isInDedicatedEngagementProcess`, die ook wordt gebruikt door de `EngagementApplication` klasse.

### <a name="430-06272017"></a>4.3.0 (06/27/2017)
* Android 8-ondersteuning (vorige versies van de SDK werken niet in Android 8).
* Er is geen meer afhankelijkheid van ondersteuningsbibliotheek.
* Verwijder `EngagementFragmentActivity` klasse.
* Vanwege [achtergrond uitvoering limieten](https://developer.android.com/preview/features/background.html) op Android 8 logboeken op de achtergrond kunnen worden uitgesteld totdat de gebruiker werkt met het apparaat, wordt dit een invloed hebben op campagne Push **geleverd** en **Systeemmelding weergegeven** statistieken wordt uitgesteld als de slaapstand van het apparaat (de melding wordt nog steeds worden weergegeven, wordt ring en Trillen in realtime zonder problemen).
* Vanwege [achtergrond locatie limieten](https://developer.android.com/preview/features/background-location-limits.html), de locatie op de achtergrond wordt niet regelmatig bijgewerkt op Android 8 realtime.

Zie voor alle versies, de [voltooien releaseopmerkingen](mobile-engagement-android-release-notes.md).

## <a name="upgrade-procedures"></a>Upgradeprocedures
Als u hebt al een oudere versie van onze SDK geïntegreerd in uw toepassing, raadpleegt u [Procedures Upgrade](mobile-engagement-android-upgrade-procedure.md).

