---
title: Locatie rapportage voor Azure Mobile Engagement Android SDK
description: Hierin wordt beschreven hoe u de rapportage voor Azure Mobile Engagement Android SDK locatie configureren
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6cab5ed1-b767-46ac-9f0b-48a4e249d88c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 777d5719cce505b55dfb61c91dcac7e713b077a9
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="location-reporting-for-azure-mobile-engagement-android-sdk"></a>Locatie rapportage voor Azure Mobile Engagement Android SDK
> [!div class="op_single_selector"]
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Dit onderwerp wordt beschreven hoe u doet locatie rapportage voor uw Android-toepassing.

## <a name="prerequisites"></a>Vereisten
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

## <a name="location-reporting"></a>Locatierapportage
Als u locaties worden gerapporteerd wilt, moet u een paar regels van configuratie toevoegen (tussen de `<application>` en `</application>` labels).

### <a name="lazy-area-location-reporting"></a>Vertraagde locatiemelding
Vertraagde locatiemelding kunt melden van het land, streek en plaats die zijn gekoppeld aan apparaten. Dit type locatie reporting maakt alleen gebruik van netwerklocaties (gebaseerd op de cel-ID of Wi-Fi). Het gebied van het apparaat wordt maximaal eenmaal per sessie gerapporteerd. De GPS wordt nooit gebruikt en dit type locatie rapport heeft dus lage impact op de accu.

Gerapporteerde gebieden worden gebruikt voor het berekenen van geografische statistieken over gebruikers, sessies, gebeurtenissen en fouten. Ze kunnen ook worden gebruikt als criterium in Reach-campagnes.

U inschakelen vertraagde locatie voor rapportage met behulp van de configuratie van de eerder genoemde in deze procedure:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

U moet ook een machtiging voor een locatie opgeven. Deze code gebruikt ``COARSE`` machtiging:

    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Als uw app nodig is, kunt u ``ACCESS_FINE_LOCATION`` in plaats daarvan.

### <a name="real-time-location-reporting"></a>Rapportage van realtime locatie
Rapportage van realtime locatie kunt melden de breedtegraad en lengtegraad die zijn gekoppeld aan apparaten. Dit type locatie reporting gebruikt standaard alleen netwerklocaties, op basis van de cel-ID of Wi-Fi. De rapportage is alleen actief wanneer de toepassing wordt uitgevoerd in de voorgrond (bijvoorbeeld tijdens een sessie).

Realtime locaties zijn *niet* om te berekenen van statistieken. Hun enige doel is dat het gebruik van realtime geofencing \<Reach-doelgroep-geofencing\> criterium in Reach-campagnes.

Toevoegen om realtime locatie reporting, een coderegel op waarin u de verbindingsreeks Engagement voor de activiteit starten instellen. Het resultaat lijkt op het volgende:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

        You also need to specify a location permission. This code uses ``COARSE`` permission:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

        If your app requires it, you can use ``ACCESS_FINE_LOCATION`` instead.

#### <a name="gps-based-reporting"></a>GPS op basis van rapportage
Rapportage van realtime locatie alleen gebruikt standaard locaties op het netwerk. Gebruik het configuratieobject zodat het gebruik van op basis van een GPS locaties, iets wat veel nauwkeuriger zijn:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

U moet ook de volgende machtigingen toevoegen, indien deze ontbreken:

    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Achtergrond rapportage
Rapportage van realtime locatie is standaard alleen actief wanneer de toepassing wordt uitgevoerd in de voorgrond (bijvoorbeeld tijdens een sessie). Gebruik dit configuratieobject zodat reporting ook op de achtergrond:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> Wanneer de toepassing wordt uitgevoerd op de achtergrond, alleen locaties op het netwerk worden gerapporteerd, zelfs als u de GPS ingeschakeld.
> 
> 

Als de gebruiker het apparaat opnieuw is opgestart, wordt het rapport van de locatie achtergrond gestopt. Als u automatisch opnieuw opgestart tijdens het opstarten, voeg deze code.

    <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
           android:exported="false">
        <intent-filter>
            <action android:name="android.intent.action.BOOT_COMPLETED" />
        </intent-filter>
    </receiver>

U moet ook de volgende machtigingen toevoegen, indien deze ontbreken:

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

## <a name="android-m-permissions"></a>Android M machtigingen
Vanaf Android M, sommige machtigingen tijdens runtime worden beheerd en goedkeuring van een gebruiker nodig.

Als u Android-API-niveau 23 richt, zijn de runtime-machtigingen standaard uitgeschakeld voor nieuwe installaties van de app. Anders zijn ze standaard ingeschakeld.

U kunt in-of uitschakelen die machtigingen in de apparaat-instellingen. Het uitschakelen van machtigingen in het systeemmenu is funest de achtergrondprocessen van de toepassing, wat is een systeemgedrag en heeft geen invloed op de mogelijkheid voor het ontvangen van pushmeldingen op de achtergrond.

In de context van Mobile Engagement locatie reporting, zijn de machtigingen die moeten worden goedgekeurd tijdens runtime:

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`

Machtigingen van de gebruiker met een dialoogvenster standaardbesturingssysteem aanvragen. Als de gebruiker heeft goedgekeurd, laat u weten ``EngagementAgent`` te nemen die wijziging rekening in realtime. De wijziging wordt anders de volgende keer dat de gebruiker de toepassing opent verwerkt.

Hier volgt een voorbeeld van code te gebruiken in een activiteit van uw toepassing machtigingen aanvragen en doorsturen van het resultaat als positief voor ``EngagementAgent``:

    @Override
    public void onCreate(Bundle savedInstanceState)
    {
      /* Other code... */

      /* Request permissions */
      requestPermissions();
    }

    @TargetApi(Build.VERSION_CODES.M)
    private void requestPermissions()
    {
      /* Avoid crashing if not on Android M */
      if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M)
      {
        /*
         * Request location permission, but this doesn't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }
