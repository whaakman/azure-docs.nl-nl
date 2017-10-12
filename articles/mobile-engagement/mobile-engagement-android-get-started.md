---
title: Aan de slag met Azure Mobile Engagement voor Android-apps
description: Informatie over het gebruik van Azure Mobile Engagement met analyses en pushmeldingen voor Android-apps.
services: mobile-engagement
documentationcenter: android
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3c286c6d-cfef-4e3e-9b2c-715429fe82db
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: dc255a930bf71e6ef6d964bc5e3472a38ce4e467
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a>Aan de slag met Azure Mobile Engagement voor Android-apps
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp leest u hoe u Azure Mobile Engagement gebruikt om inzicht te krijgen in het gebruik van uw apps, en om pushmeldingen te verzenden aan gesegmenteerde gebruikers van een Android-toepassing.
Deze zelfstudie laat een eenvoudig broadcast-scenario met Mobile Engagement zien. U maakt een lege Android-app die basisgegevens verzamelt en pushmeldingen ontvangt via Google Cloud Messaging (GCM).

## <a name="prerequisites"></a>Vereisten
Voltooiing van deze zelfstudie vereist de [Android-hulpprogramma's voor ontwikkelaars](https://developer.android.com/sdk/index.html), waaronder de Integrated Development Environment van Android Studio en het meest recente Android-platform.

Ook is [Mobile Engagement Android SDK](https://aka.ms/vq9mfn) vereist.

> [!IMPORTANT]
> U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started) voor meer informatie.
>
>

## <a name="set-up-mobile-engagement-for-your-android-app"></a>Mobile Engagement instellen voor uw Android-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a>Uw app verbinden met de back-end van Mobile Engagement
Deze zelfstudie toont een 'basisintegratie', de minimale set die vereist is voor het verzamelen van gegevens en verzenden van een pushmelding. U maakt een eenvoudige app met Android Studio ter illustratie van de integratie.

De volledige integratiedocumentatie is te vinden in de [Mobile Engagement Android SDK-integratie](mobile-engagement-android-sdk-overview.md).

### <a name="create-an-android-project"></a>Een Android-project maken
1. Start **Android Studio** en selecteer **Start a new Android Studio project** in het pop-upvenster.

    ![][1]
2. Geef een naam voor de app en het bedrijfsdomein op. Schrijf op wat u invult, omdat u deze gegevens later nodig hebt. Klik op **Volgende**.

    ![][2]
3. Selecteer de doel-vormfactor en het API-niveau en klik op **Next**.

   > [!NOTE]
   > Mobile Engagement vereist minimaal API-niveau 10 (Android 2.3.3).
   >
   >

    ![][3]
4. Selecteer hier **Blank Activity** (Blanco activiteit). Dit is het enige scherm voor deze app. Klik vervolgens op **Next** (Volgende).

    ![][4]
5. Laat verder alle standaardinstellingen zoals ze zijn en klik op **Finish**.

    ![][5]

Android Studio maakt nu de demo-app waarin we Mobile Engagement gaan integreren.

### <a name="include-the-sdk-library-in-your-project"></a>De SDK-bibliotheek opnemen in uw project
1. Download de [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).
2. Pak het bestand uit in een map op uw computer.
3. Zoek de JAR-bibliotheek voor de huidige versie van deze SDK en kopieer die naar het Klembord.

      ![][6]
4. Navigeer naar de sectie **Project** (1) en plak de JAR in de map libs (2).

      ![][7]
5. Synchroniseer het project om de bibliotheek te laden.

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a>Uw app verbinden met de back-end van Mobile Engagement via de verbindingsreeks
1. Kopieer de volgende regels code in het maken van de activiteit (dit moet worden uitgevoerd op één plek in de toepassing, meestal de belangrijkste activiteit). Open voor deze voorbeeld-app de MainActivity onder src -> main -> java en voeg het volgende toe:

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);
2. Zet de verwijzingen om door op Alt+Enter te drukken of de volgende importinstructies toe te voegen:

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;
3. Ga terug naar de klassieke Azure-portal op de pagina **Verbindingsgegevens** van de app en kopieer de **verbindingsreeks**.

      ![][9]
4. Plak deze in de `setConnectionString`-parameter, waarbij u de gehele tekenreeks vervangt die in de volgende code wordt weergegeven:

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a>Machtigingen en een servicedeclaratie toevoegen
1. Voeg machtigingen toe aan het bestand Manifest.xml van uw project direct vóór of na de tag `<application>`:

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
2. Voeg deze code toe tussen de tags `<application>` en `</application>` om de agentservice te declareren:

        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
3. Vervang in de code die u hebt geplakt `"<Your application name>"` in het label, dat wordt weergegeven in het menu **Settings** (Instellingen) waar u de services kunt zien die worden uitgevoerd op het apparaat. U kunt bijvoorbeeld het woord 'Service' aan dat label toevoegen.

### <a name="send-a-screen-to-mobile-engagement"></a>Een scherm naar Mobile Engagement verzenden
U dient ten minste één scherm (activiteit) naar de back-end van Mobile Engagement te sturen om te beginnen met het verzenden van gegevens en om ervoor te zorgen dat de gebruikers actief zijn.

Ga naar **MainActivity.java** en voeg het volgende toe ter vervanging van de basisklasse van **MainActivity** met **EngagementActivity**:

    public class MainActivity extends EngagementActivity {

> [!NOTE]
> Als de basisklasse niet *Activity* is, raadpleegt u [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md) (Geavanceerde Android-rapportages) voor het overnemen van verschillende klassen.
>
>

Maak de volgende regel onzichtbaar voor dit eenvoudige voorbeeldscenario:

    // setSupportActionBar(toolbar);

Als u de `ActionBar` in uw app wilt behouden, verwijzen wij u naar [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md) (Geavanceerde Android-rapportages).

## <a name="connect-app-with-real-time-monitoring"></a>App verbinden met realtime-bewaking
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a>Pushmeldingen en in-app-berichten inschakelen
Tijdens een campagne kunt u met Mobile Engagement met uw gebruikers communiceren en ze bereiken met pushmeldingen en in-app-berichten. Deze module heet REACH in de Mobile Engagement-portal.
In de volgende gedeelten stelt u de app in om die meldingen en berichten te ontvangen.

### <a name="copy-sdk-resources-in-your-project"></a>SDK-bronnen naar uw project kopiëren
1. Ga terug naar de gedownloade SDK-inhoud en kopieer de map **res**.

    ![][10]
2. Ga terug naar Android Studio, selecteer de map **main** van uw projectbestanden en plak de gekopieerde map om de resources toe te voegen aan het project.

    ![][11]

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a>Volgende stappen
Ga naar [Android SDK](mobile-engagement-android-sdk-overview.md) voor uitgebreide informatie over de SDK-integratie.

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
