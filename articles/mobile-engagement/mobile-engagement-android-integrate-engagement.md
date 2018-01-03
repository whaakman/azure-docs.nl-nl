---
title: Azure Mobile Engagement Android SDK-integratie
description: Meest recente updates en -procedures voor Android-SDK voor Azure Mobile Engagement
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: a5487793-1a12-4f6c-a1cf-587c5a671e6b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 35bd92e52b7a02f58620a03156902f9f91be57ae
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="how-to-integrate-engagement-on-android"></a>Het integreren van Engagement voor Android
> [!div class="op_single_selector"]
> * [Windows Universal](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-integrate-engagement.md)
> 
> 

Deze procedure beschrijft de eenvoudigste manier Engagement Analytics en bewaking van functies in uw Android-toepassing te activeren.

> [!IMPORTANT]
> Het minimale Android SDK-API-niveau moet 10 of hoger (Android 2.3.3 of hoger).
> 
> 

De volgende stappen zijn voldoende is voor het rapport van logboeken die nodig zijn voor alle statistische gegevens over gebruikers, sessies, activiteiten, Crashes en Technicals compute wordt geactiveerd. Het rapport van logboeken die nodig zijn voor het berekenen van andere statistieken zoals gebeurtenissen, fouten en taken worden uitgevoerd handmatig met behulp van de Engagement-API (Zie [hoe u de geavanceerde Mobile Engagement in uw Android-API tagging](mobile-engagement-android-use-engagement-api.md) aangezien deze statistieken zijn toepassingen die afhankelijk zijn.

## <a name="embed-the-engagement-sdk-and-service-into-your-android-project"></a>De Engagement SDK en de service insluiten in uw Android-project
Download de Android SDK uit [hier](https://aka.ms/vq9mfn) ophalen `mobile-engagement-VERSION.jar` en plaats deze in de `libs` map van uw Android-project (de map libs maken als deze nog niet bestaat).

> [!IMPORTANT]
> Als u uw toepassingspakket met ProGuard bouwt, moet u bepaalde klassen houden. U kunt de volgende configuratie-fragment:
> 
> -openbare klasse houden * breidt android.os.IInterface-klasse com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {houden
> 
> <methods>; }
> 
> 

Geef de verbindingsreeks Engagement door het aanroepen van de volgende methode in de activiteit starten:

            EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
            engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
            EngagementAgent.getInstance(this).init(engagementConfiguration);

De verbindingsreeks voor uw toepassing wordt weergegeven op de Azure Portal.

* Indien deze ontbreken, voeg de volgende machtigingen voor Android (voordat de `<application>` tag):
  
          <uses-permission android:name="android.permission.INTERNET"/>
          <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
* Voeg de volgende sectie (tussen de `<application>` en `</application>` labels):
  
          <service
            android:name="com.microsoft.azure.engagement.service.EngagementService"
            android:exported="false"
            android:label="<Your application name>Service"
            android:process=":Engagement"/>
* Wijziging `<Your application name>` door de naam van uw toepassing.

> [!TIP]
> De `android:label` kenmerk kunt u de naam van de Engagement-service die wordt weergegeven voor de eindgebruikers op het scherm 'Services uitgevoerd' van hun telefoon. Het verdient aanbeveling voor dit kenmerk instelt op `"<Your application name>Service"` (bijvoorbeeld `"AcmeFunGameService"`).
> 
> 

Geven de `android:process` kenmerk zorgt ervoor dat de Engagement-service wordt uitgevoerd in een eigen proces (Engagement uitgevoerd in hetzelfde proces als uw toepassing wordt Controleer uw main/UI-thread mogelijk minder goed).

> [!NOTE]
> Alle code die u in plaatsen `Application.onCreate()` en andere retouraanroepen toepassing wordt uitgevoerd voor de processen van uw toepassing, inclusief de Engagement-service. Er kunnen ongewenste neveneffecten (zoals overbodige geheugentoewijzingen en threads in de Engagement proces, dubbele broadcast ontvangers of services).
> 
> 

Als u negeren `Application.onCreate()`, verdient het aanbeveling om toe te voegen het volgende codefragment aan het begin van uw `Application.onCreate()` functie:

             public void onCreate()
             {
               if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
                 return;

               ... Your code...
             }

U kunt hetzelfde doen voor `Application.onTerminate()`, `Application.onLowMemory()` en `Application.onConfigurationChanged(...)`.

U kunt ook uitbreiden `EngagementApplication` in plaats van het uitbreiden van `Application`: de callback `Application.onCreate()` heeft de controle van het proces en aanroepen `Application.onApplicationProcessCreate()` alleen als het huidige proces niet degene die als host fungeert voor de Engagement-service is, dezelfde regels voor de andere gelden retouraanroepen.

## <a name="basic-reporting"></a>Basic-rapportage
### <a name="recommended-method-overload-your-activity-classes"></a>Aanbevolen methode: overbelasting uw `Activity` klassen
Om het rapport van de logboeken die zijn vereist voor Engagement berekenen gebruikers, sessies, activiteiten, Crashes en technische statistieken activeert, alleen hebt u alle uw `*Activity` onderliggende klassen overnemen van de bijbehorende `Engagement*Activity` klassen (bijvoorbeeld) Als uw verouderde activiteit breidt `ListActivity`, zorg er breidt `EngagementListActivity`).

**Zonder Engagement:**

            package com.company.myapp;

            import android.app.Activity;
            import android.os.Bundle;

            public class MyApp extends Activity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

**Met Engagement:**

            package com.company.myapp;

            import com.microsoft.azure.engagement.activity.EngagementActivity;
            import android.os.Bundle;

            public class MyApp extends EngagementActivity
            {
              @Override
              public void onCreate(Bundle savedInstanceState)
              {
                super.onCreate(savedInstanceState);
                setContentView(R.layout.main);
              }
            }

> [!IMPORTANT]
> Wanneer u `EngagementListActivity` of `EngagementExpandableListActivity`, moet u ervoor zorgen dat een aanroep naar `requestWindowFeature(...);` wordt uitgevoerd vóór de aanroep van `super.onCreate(...);`, anders een crash wordt uitgevoerd.
> 
> 

U vindt deze klassen in de `src` map, en kunt kopiëren naar uw project. De klassen zich ook in de **JavaDoc**.

### <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternatieve methode: call `startActivity()` en `endActivity()` handmatig
Als u niet kunt of niet wilt overbelasten uw `Activity` klassen, u kunt in plaats daarvan starten en beëindigen van uw activiteiten door aan te roepen `EngagementAgent`van methoden rechtstreeks.

> [!IMPORTANT]
> De Android-SDK wordt nooit aangeroepen door de `endActivity()` methode, zelfs wanneer de toepassing wordt afgesloten (voor Android toepassingen zijn daadwerkelijk nooit gesloten). Het is dus *maximaal* aanbevolen om aan te roepen de `startActivity()` methode in de `onResume` retouraanroep van *alle* uw activiteiten, en de `endActivity()` methode in de `onPause()` retouraanroep van *Alle* uw activiteiten. Dit is de enige manier om ervoor te zorgen dat sessies niet wordt gelekt. Als een sessie is gelekt, wordt Engagement nooit verbroken door de service uit de back-end Engagement (omdat de service verbonden blijft als een sessie in behandeling is).
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

In dit voorbeeld lijkt veel op het `EngagementActivity` klasse en varianten, waarvan de broncode wordt verstrekt in de `src` map.

## <a name="test"></a>Testen
Nu uw integratie controleren door te voeren van uw mobiele app in een emulator of het apparaat en verifiëren van de registratie van een sessie op het tabblad Monitor.

De volgende secties zijn optioneel.

## <a name="location-reporting"></a>Locatierapportage
Als u locaties worden gerapporteerd wilt, moet u een paar regels van configuratie toevoegen (tussen de `<application>` en `</application>` labels).

### <a name="lazy-area-location-reporting"></a>Vertraagde locatiemelding
Vertraagde locatiemelding kunt voor het rapporteren van het land, streek en plaats die is gekoppeld aan apparaten. Dit type locatie reporting maakt alleen gebruik van netwerklocaties (gebaseerd op de cel-ID of Wi-Fi). Het gebied van het apparaat wordt maximaal eenmaal per sessie gerapporteerd. De GPS wordt nooit gebruikt en dit type locatie rapport heeft slechts weinig (en niet naar Nee zeggen) dus impact op de accu.

Gerapporteerde gebieden worden gebruikt voor het berekenen van geografische statistieken over gebruikers, sessies, gebeurtenissen en fouten. Ze kunnen ook worden gebruikt als criterium in Reach-campagnes.

Om in te schakelen vertraagde locatiemelding, kunt u dit doen met behulp van de configuratie van de eerder genoemde in deze procedure:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setLazyAreaLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

U moet ook de volgende machtigingen toevoegen, indien deze ontbreken:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Of u kunt blijven gebruiken ``ACCESS_FINE_LOCATION`` als u al in uw toepassing gebruikt.

### <a name="real-time-location-reporting"></a>Rapportage van realtime locatie
Rapportage van realtime locatie kunt voor het rapporteren van de breedtegraad en lengtegraad die is gekoppeld aan apparaten. Standaard alleen gebruik van dit type locatie reporting netwerklocaties (gebaseerd op de cel-ID of Wi-Fi), en de rapportage is alleen beschikbaar wanneer de toepassing wordt uitgevoerd in de voorgrond (dat wil zeggen tijdens een sessie).

Realtime-locaties zijn *niet* om te berekenen van statistieken. Hun enige doel is dat het gebruik van realtime geofencing \<Reach-doelgroep-geofencing\> criterium in Reach-campagnes.

Om realtime locatie reporting, kunt u dit doen met behulp van de configuratie van de eerder genoemde in deze procedure:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

U moet ook de volgende machtigingen toevoegen, indien deze ontbreken:

            <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>

Of u kunt blijven gebruiken ``ACCESS_FINE_LOCATION`` als u al in uw toepassing gebruikt.

#### <a name="gps-based-reporting"></a>GPS op basis van rapportage
Rapportage van realtime locatie alleen gebruikt standaard op basis van netwerklocaties. Gebruik het configuratieobject zodat het gebruik van een GPS op basis van locaties (die veel meer nauwkeurige):

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setFineRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

U moet ook de volgende machtigingen toevoegen, indien deze ontbreken:

            <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>

#### <a name="background-reporting"></a>Achtergrond rapportage
Rapportage van realtime locatie is standaard alleen actief wanneer de toepassing wordt uitgevoerd in de voorgrond (dat wil zeggen tijdens een sessie). Gebruik het configuratieobject zodat reporting ook op de achtergrond:

    EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
    engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
    engagementConfiguration.setRealtimeLocationReport(true);
    engagementConfiguration.setBackgroundRealtimeLocationReport(true);
    EngagementAgent.getInstance(this).init(engagementConfiguration);

> [!NOTE]
> Wanneer de toepassing op de achtergrond, wordt alleen uitgevoerd op het netwerk locaties worden gerapporteerd, zelfs als u de GPS ingeschakeld.
> 
> 

Het rapport achtergrond-locatie wordt gestopt als de gebruiker het apparaat opnieuw is opgestart, kunt u dit om deze automatisch opnieuw opgestart tijdens het opstarten toevoegen:

            <receiver android:name="com.microsoft.azure.engagement.EngagementLocationBootReceiver"
               android:exported="false">
               <intent-filter>
                  <action android:name="android.intent.action.BOOT_COMPLETED" />
               </intent-filter>
            </receiver>

U moet ook de volgende machtigingen toevoegen, indien deze ontbreken:

            <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

### <a name="android-m-permissions"></a>Android M machtigingen
Vanaf Android M, sommige machtigingen tijdens runtime worden beheerd en moet de goedkeuring van een gebruiker.

De machtigingen van de runtime worden uitgeschakeld voor nieuwe installaties van apps standaard als u Android-API-niveau 23 richt. Anders zal het worden standaard ingeschakeld.

De gebruiker kan in-of uitschakelen die machtigingen in de apparaat-instellingen. Het uitschakelen van machtigingen van Systeemmenu is funest achtergrondprocessen van de toepassing, dit is een systeemgedrag en heeft geen invloed op de mogelijkheid voor het ontvangen van pushmeldingen op de achtergrond.

In de context van Mobile Engagement zijn de machtigingen die moeten worden goedgekeurd tijdens runtime:

* `ACCESS_COARSE_LOCATION`
* `ACCESS_FINE_LOCATION`
* `WRITE_EXTERNAL_STORAGE`(alleen bij Android-API-niveau 23 voor deze doelen)

De externe opslag wordt alleen gebruikt voor Reach grote afbeelding functie. Als u vindt deze machtiging aan leiden tot uitvaltijd voor gebruikers vragen, kunt u deze verwijderen als u deze gebruikt voor Mobile Engagement alleen maar koste u grote afbeelding functie uitschakelt.

Voor de locatie-functies, moet u machtigingen voor gebruiker met behulp van een dialoogvenster standaardbesturingssysteem aanvragen. Als de gebruiker heeft goedgekeurd, moet u vertellen ``EngagementAgent`` te nemen die wijziging rekening in realtime (anders de wijziging wordt verwerkt de volgende keer dat de gebruiker de toepassing opent).

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
         * Request location permission, but this won't explain why it is needed to the user.
         * The standard Android documentation explains with more details how to display a rationale activity to explain the user why the permission is needed in your application.
         * Putting COARSE vs FINE has no impact here, they are part of the same group for runtime permission management.
         */
        if (checkSelfPermission(android.Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.ACCESS_FINE_LOCATION }, 0);

        /* Only if you want to keep features using external storage */
        if (checkSelfPermission(android.Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED)
          requestPermissions(new String[] { android.Manifest.permission.WRITE_EXTERNAL_STORAGE }, 1);
      }
    }

    @Override
    public void onRequestPermissionsResult(int requestCode, String[] permissions, int[] grantResults)
    {
      /* Only a positive location permission update requires engagement agent refresh, hence the request code matching from above function */
      if (requestCode == 0 && grantResults[0] == PackageManager.PERMISSION_GRANTED)
        getEngagementAgent().refreshPermissions();
    }

## <a name="advanced-reporting"></a>Geavanceerde rapportage
Eventueel, als u rapporteren toepassing specifieke gebeurtenissen, fouten en taken wilt, moet u de Engagement-API gebruiken via de methoden van de `EngagementAgent` klasse. Een object van deze klasse is opgehaald door het aanroepen van de `EngagementAgent.getInstance()` statische methode.

De Engagement-API kunt u alle Engagement geavanceerde mogelijkheden gebruiken en wordt beschreven in de manier waarop de Engagement-API gebruiken voor Android (en van de technische documentatie over de `EngagementAgent` klasse).

## <a name="advanced-configuration-in-androidmanifestxml"></a>Geavanceerde configuratie (in AndroidManifest.xml)
### <a name="wake-locks"></a>Wake-vergrendelingen
Als u wilt om ervoor te zorgen dat de statistieken worden verzonden in realtime bij gebruik van Wi-Fi of wanneer het scherm uitgeschakeld is, voegt u de volgende optionele machtiging:

            <uses-permission android:name="android.permission.WAKE_LOCK"/>

### <a name="crash-report"></a>Foutrapportage
Als u uitschakelen van foutenrapporten wilt, voeg deze toe (tussen de `<application>` en `</application>` labels):

            <meta-data android:name="engagement:reportCrash" android:value="false"/>

### <a name="burst-threshold"></a>Burst drempelwaarde
Standaard wordt de Engagement-service-rapporten Logboeken in realtime. Als uw toepassing Logboeken heel vaak rapporteert, is het beter voor het bufferen van de logboeken en om te rapporteren ze allemaal tegelijk op een vaste tijd base (dit is de modus' burst' genoemd). Om dit te doen, voegt u dit (tussen de `<application>` en `</application>` labels):

            <meta-data android:name="engagement:burstThreshold" android:value="{interval between too bursts (in milliseconds)}"/>

De modus ' burst ' iets langere levensduur maar heeft een invloed op de Monitor Engagement: alle sessies en taken duur wordt afgerond op de drempelwaarde burst (dus sessies en korter zijn dan de drempelwaarde burst is mogelijk niet zichtbaar taken). Het is raadzaam een ' burst ' drempelwaarde niet langer dan 30000 (30s) gebruiken.

### <a name="session-timeout"></a>Sessietime-out
Een sessie is beëindigd per 10 standaard, na het einde van de laatste activiteit (dit gebeurt doorgaans de thuis- of Back-toets, door het instellen van de telefoon inactief of door te gaan naar een andere toepassing). Dit is om te voorkomen dat een sessie-splitsing telkens wanneer de gebruiker afsluiten en terugkeren naar de toepassing zeer snel (dit kan gebeuren wanneer hij een installatiekopie worden opgepikt controleren een melding, enz.). U wilt wijzigen van deze parameter. Om dit te doen, voegt u dit (tussen de `<application>` en `</application>` labels):

            <meta-data android:name="engagement:sessionTimeout" android:value="{session timeout (in milliseconds)}"/>

## <a name="disable-log-reporting"></a>Melden van logboek uitschakelen
### <a name="using-a-method-call"></a>Met behulp van een methodeaanroep van
Als u stoppen met het verzenden van Logboeken Engagement wilt, kunt u bellen:

            EngagementAgent.getInstance(context).setEnabled(false);

Deze aanroep is permanent: een gedeelde voorkeuren-bestand wordt gebruikt.

Als Engagement actief is wanneer u deze functie aanroept, duurt 1 minuut voor de service te stoppen. Maar deze start de service helemaal de volgende keer dat Won't u de toepassing wordt gestart.

U kunt reporting opnieuw door het aanroepen van dezelfde functie met logboek inschakelen `true`.

### <a name="integration-in-your-own-preferenceactivity"></a>Integratie in uw eigen`PreferenceActivity`
In plaats van deze functie aanroept, kunt u deze instelling ook integreren rechtstreeks in uw bestaande `PreferenceActivity`.

U kunt Engagement om uw voorkeurenbestand (met de gewenste modus) te gebruiken kunt configureren in de `AndroidManifest.xml` het bestand met `application meta-data`:

* De `engagement:agent:settings:name` sleutel wordt gebruikt voor het definiëren van de naam van het bestand gedeelde voorkeuren.
* De `engagement:agent:settings:mode` sleutel wordt gebruikt voor het definiëren van de modus van het bestand gedeelde voorkeuren, moet u dezelfde modus als in uw `PreferenceActivity`. De modus moet worden doorgegeven als een getal: als u een combinatie van constante vlaggen in uw code gebruikt, controleert u de totale waarde.

Gebruik altijd engagement de `engagement:key` Booleaanse sleutel in het voorkeurenbestand voor het beheren van deze instelling.

Het volgende voorbeeld van `AndroidManifest.xml` ziet u de standaardwaarden:

            <application>
                [...]
                <meta-data
                  android:name="engagement:agent:settings:name"
                  android:value="engagement.agent" />
                <meta-data
                  android:name="engagement:agent:settings:mode"
                  android:value="0" />

Vervolgens kunt u toevoegen een `CheckBoxPreference` in de indeling van uw voorkeur zoals de volgende:

            <CheckBoxPreference
              android:key="engagement:enabled"
              android:defaultValue="true"
              android:title="Use Engagement"
              android:summaryOn="Engagement is enabled."
              android:summaryOff="Engagement is disabled." />

<!-- URLs. -->
[Device API]: http://go.microsoft.com/?linkid=9876094
