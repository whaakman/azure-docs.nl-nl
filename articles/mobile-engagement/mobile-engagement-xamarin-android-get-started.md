---
title: Aan de slag met Azure Mobile Engagement voor Xamarin.Android
description: Informatie over het gebruik van Azure Mobile Engagement met analyses en pushmeldingen voor Xamarin.Android-apps.
services: mobile-engagement
documentationcenter: xamarin
author: piyushjo
manager: erikre
editor: 
ms.assetid: fb68cf98-08a2-41b5-8e59-757469de3fe7
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/16/2016
ms.author: piyushjo
ms.openlocfilehash: 7b3d01b32c2d5a40448fc22861cd45f612238f2f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Aan de slag met Azure Mobile Engagement voor Xamarin.Android-apps
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

In dit onderwerp leest u hoe u Azure Mobile Engagement gebruikt om inzicht te krijgen in het gebruik van uw apps, en om pushmeldingen te verzenden aan gesegmenteerde gebruikers van een Xamarin.Android-toepassing.
Deze zelfstudie laat een eenvoudig broadcast-scenario met Mobile Engagement zien. U maakt een lege Xamarin.Android-app die basisgegevens verzamelt en pushmeldingen ontvangt via Google Cloud Messaging (GCM).

> [!NOTE]
> De Azure Mobile Engagement-service wordt in maart 2018 beëindigd en is momenteel alleen beschikbaar voor bestaande klanten. Zie [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/) voor meer informatie.

Voor deze zelfstudie hebt u het volgende nodig:

* [Xamarin Studio](http://xamarin.com/studio). U kunt ook Visual Studio gebruiken met Xamarin, maar deze zelfstudie gebruikt Xamarin Studio. Zie [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (Installatie voor Visual Studio en Xamarin) voor installatie-instructies.
* [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started) voor meer informatie.
> 
> 

## <a id="setup-azme"></a>Mobile Engagement instellen voor uw Android-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Uw app verbinden met de back-end van Mobile Engagement
Deze zelfstudie toont een ‘basisintegratie’, de minimale set die vereist is voor het verzamelen van gegevens en verzenden van een pushmelding. 

We gaan een eenvoudige app maken met Xamarin Studio ter illustratie van de integratie.

### <a name="create-a-new-xamarinandroid-project"></a>Een nieuw Xamarin.Android-project maken
1. Start **Xamarin Studio** en ga naar **File** -> **New** -> **Solution**. 
   
    ![][1]
2. Selecteer **Android App**, controleer of de geselecteerde taal **C#** is en klik op **Next**.
   
    ![][2]
3. Vul de velden **App Name** en **Organization Identifier** in. Zorg dat er een vinkje staat bij **Google Play Services** en klik vervolgens op **Next**. 
   
    ![][3]
4. Werk de velden **Project Name**, **Solution Name** en **Location** bij indien nodig en klik op **Create**.
   
    ![][4]

Xamarin Studio maakt de app waarin we Mobile Engagement gaan integreren. 

### <a name="connect-your-app-to-mobile-engagement-backend"></a>Uw app verbinden met de back-end van Mobile Engagement
1. Klik met de rechtermuisknop op de map **Packages** in het venster Solution en selecteer **Add Packages…**.
   
    ![][5]
2. Zoeken de **Microsoft Azure Mobile Engagement Xamarin SDK** en voeg die toe aan uw oplossing.  
   
    ![][6]
3. Open **MainActivity.cs** en voeg het volgende toe met instructies:
   
        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;
4. In de methode `OnCreate` voegt u het volgende toe om de verbinding met de back-end van Mobile Engagement te initialiseren. Zorg ervoor dat u uw **verbindingsreeks** toevoegt. 
   
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

### <a name="add-permissions-and-a-service-declaration"></a>Machtigingen en een servicedeclaratie toevoegen
1. Open het bestand **Manifest.xml** onder de map Properties. Selecteer het tabblad Source zodat u de XML-bron direct kunt bijwerken.
2. Voeg de volgende machtigingen toe aan het bestand Manifest.xml (dat zich bevindt onder de map **Properties**) van uw project, direct vóór of na de tag `<application>`:
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
3. Voeg het volgende toe tussen de tags `<application>` en `</application>` om de agent-service te declareren:
   
        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
4. In de code die u zojuist hebt geplakt, vervangt u `"<Your application name>"` in het label. Dit wordt weergegeven in het menu **Instellingen**, waar gebruikers de services kunnen zien die worden uitgevoerd op het apparaat. U kunt bijvoorbeeld het woord 'Service' aan dat label toevoegen.

### <a name="send-a-screen-to-mobile-engagement"></a>Een scherm naar Mobile Engagement verzenden
Om te beginnen met het verzenden van gegevens en ervoor te zorgen dat de gebruikers actief zijn, moet u ten minste één scherm naar de back-end van Mobile Engagement sturen. Om dit te doen zorgt u ervoor dat de `MainActivity` overneemt van `EngagementActivity` in plaats van `Activity`.

    public class MainActivity : EngagementActivity

Als u niet kunt overnemen van `EngagementActivity`, moet u vervolgens de methode `.StartActivity` en `.EndActivity` toevoegen in respectievelijk `OnResume` en `OnPause`.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }

            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

## <a id="monitor"></a>App verbinden met realtime-bewaking
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Pushmeldingen en in-app-berichten inschakelen
Met Mobile Engagement kunt u communiceren met uw gebruikers en ze bereiken met pushmeldingen en in-app-berichten in de context van campagnes. Deze module heet REACH in de Mobile Engagement-portal.
In de volgende secties stelt u de app in om die te ontvangen.

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
