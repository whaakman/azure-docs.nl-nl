---
title: Geavanceerde opties voor de rapportage voor Azure Mobile Engagement Android SDK
description: Hierin wordt beschreven hoe u doen geavanceerde rapport om vast te leggen analytics voor Azure Mobile Engagement Android SDK
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7da7abd5-19d6-4892-94d8-818e5424b2cd
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 2a1445afa2c2fca1a31ad9c012b9c8a917ebf65c
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="advanced-reporting-with-engagement-on-android"></a>Geavanceerde Reporting met Engagement voor Android
> [!div class="op_single_selector"]
> * [Universeel Windows](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-reporting.md)
> 
> 

Dit onderwerp beschrijft aanvullende scenario's voor rapportage in uw Android-toepassing. U kunt deze opties toepassen op de app gemaakt in de [aan de slag](mobile-engagement-android-get-started.md) zelfstudie.

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

De zelfstudie u voltooid is opzettelijk direct en eenvoudig, maar er zijn geavanceerde opties die u kunt kiezen.

## <a name="modifying-your-activity-classes"></a>Wijzigen van uw `Activity` klassen
In de [zelfstudie aan de slag](mobile-engagement-android-get-started.md), alle moest u is om uw `*Activity` subklassen overnemen van de bijbehorende `Engagement*Activity` klassen. Bijvoorbeeld, als uw verouderde activiteit uitgebreid `ListActivity`, brengt u deze uitbreiden `EngagementListActivity`.

> [!IMPORTANT]
> Wanneer u `EngagementListActivity` of `EngagementExpandableListActivity`, moet u ervoor zorgen dat een aanroep naar `requestWindowFeature(...);` wordt uitgevoerd vóór de aanroep van `super.onCreate(...);`, anders een crash plaatsvindt.
> 
> 

U vindt deze klassen in de `src` map, en kunt kopiëren naar uw project. De klassen zich ook in de **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatieve methode: call `startActivity()` en `endActivity()` handmatig
Als u niet kunt of niet wilt overbelasten uw `Activity` klassen, u kunt in plaats daarvan starten en beëindigen van uw activiteiten door het aanroepen van de `EngagementAgent`van methoden rechtstreeks.

> [!IMPORTANT]
> De Android-SDK wordt nooit aangeroepen door de `endActivity()` methode, zelfs wanneer de toepassing wordt afgesloten (voor Android toepassingen zijn nooit gesloten). Het is dus *maximaal* aanbevolen om aan te roepen de `startActivity()` methode in de `onResume` retouraanroep van *alle* uw activiteiten, en de `endActivity()` methode in de `onPause()` retouraanroep van *Alle* uw activiteiten. Dit is de enige manier om ervoor te zorgen dat sessies niet gelekt. Als een sessie is gelekt, wordt de service Engagement nooit ontkoppelt van de back-end Engagement (omdat de service verbonden blijft als een sessie in behandeling is).
> 
> 

Hier volgt een voorbeeld:

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

In dit voorbeeld is vergelijkbaar met de `EngagementActivity` klasse en varianten, waarvan de broncode wordt verstrekt in de `src` map.

## <a name="using-applicationoncreate"></a>Met behulp van Application.onCreate()
Alle code die u in plaatsen `Application.onCreate()` en in andere toepassingen retouraanroepen voor de processen van uw toepassing, inclusief de Engagement-service wordt uitgevoerd. Er kunnen ongewenste neveneffecten hebben, zoals overbodige geheugentoewijzingen en threads in de Engagement-proces of dubbele broadcast ontvangers of services.

Als u negeren `Application.onCreate()`, wordt aangeraden het volgende codefragment toe te voegen aan het begin van uw `Application.onCreate()` functie:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

U kunt hetzelfde doen voor `Application.onTerminate()`, `Application.onLowMemory()`, en `Application.onConfigurationChanged(...)`.

U kunt ook uitbreiden `EngagementApplication` in plaats van het uitbreiden van `Application`: de callback `Application.onCreate()` heeft de controle van het proces en aanroepen `Application.onApplicationProcessCreate()` alleen als het huidige proces niet degene die als host fungeert voor de Engagement-service is, dezelfde regels voor de andere gelden retouraanroepen.

## <a name="tags-in-the-androidmanifestxml-file"></a>Labels in het bestand AndroidManifest.xml
In de service-code in het bestand AndroidManifest.xml de `android:label` kenmerk kunt u de naam van de service Engagement kiezen, zoals deze wordt weergegeven aan eindgebruikers in het scherm 'Services uitgevoerd' van hun telefoon. Het is raadzaam als dit kenmerk instelt op `"<Your application name>Service"` (bijvoorbeeld `"AcmeFunGameService"`).

Geven de `android:process` kenmerk zorgt ervoor dat de Engagement-service wordt uitgevoerd in een eigen proces (Engagement uitgevoerd in hetzelfde proces als uw toepassing de main/UI-thread mogelijk minder goed maakt).

## <a name="building-with-proguard"></a>Bouwen met ProGuard
Als u uw toepassingspakket met ProGuard bouwt, moet u bepaalde klassen houden. U kunt de volgende configuratie-fragment:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
     }
