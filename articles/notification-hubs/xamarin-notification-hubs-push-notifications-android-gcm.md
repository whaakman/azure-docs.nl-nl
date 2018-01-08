---
title: Aan de slag met Notification Hubs voor Xamarin.Android-apps | Microsoft Docs
description: In deze zelfstudie leert u hoe u met Azure Notification Hubs pushmeldingen verzendt naar een Xamarin.Android-toepassing.
author: jwhitedev
manager: kpiteira
editor: 
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/22/2017
ms.author: jawh
ms.openlocfilehash: 4cb3aaa3d4e577e45f01f245d3898c033092f5a3
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="get-started-with-notification-hubs-for-xamarinandroid-apps"></a>Aan de slag met Notification Hubs voor Xamarin.Android-apps
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie wordt gedemonstreerd hoe u met Azure Notification Hubs pushmeldingen verzendt naar een Xamarin.Android-toepassing. U maakt een lege Xamarin.Android-app die pushmeldingen ontvangt via Firebase Cloud Messaging (FCM). Als u klaar bent, kunt u de Notification Hub gebruiken om pushmeldingen uit te zenden naar alle apparaten waarop uw app wordt uitgevoerd. De voltooide code is beschikbaar in het [NotificationHubs-app][GitHub]-voorbeeld.

In deze zelfstudie wordt een eenvoudig scenario voor het uitzenden met Notification Hubs beschreven.

## <a name="before-you-begin"></a>Voordat u begint
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

De volledige code voor deze zelfstudie vindt u [hier][GitHub] op GitHub.

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* [Visual Studio met Xamarin] onder Windows of [Visual Studio voor Mac] onder OS X.
* Actief Google-account

Het voltooien van deze zelfstudie is een vereiste voor alle andere Notification Hubs-zelfstudies voor Xamarin.Android-apps.

> [!IMPORTANT]
> U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F) voor meer informatie.
> 
> 

## <a name="enable-firebase-cloud-messaging"></a>Firebase Cloud-berichten inschakelen
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a>Uw Notification Hub configureren
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">

<li><p>Kies bovenaan het tabblad <b>Configureren</b>, voer de waarde voor <b>API-sleutel</b> in die u in de vorige sectie hebt verkregen en selecteer <b>Opslaan</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

De Notification Hub is geconfigureerd om te werken met FCM en u hebt de verbindingsreeksen om uw app te registreren voor het ontvangen van meldingen en voor het verzenden van pushmeldingen.

## <a name="connect-your-app-to-the-notification-hub"></a>Uw app verbinden met de Notification Hub
Eerst maakt u een nieuw project. 

1. Kies in Visual Studio **Nieuwe oplossing** > **Android-app** en selecteer **Volgende**.
   
      ![Visual Studio - Een nieuw Android-project maken][22]

2. Voer uw **appnaam** en **id** in. Kies de **doelplatforms** die u wilt ondersteunen en kies **Volgende** en **Maken**.
   
      ![Visual Studio - Android app configureren][23]

    Hierop wordt een nieuw Android-project gemaakt.

3. Open de projecteigenschappen door met de rechtermuisknop op het nieuwe project in de weergave Oplossing te klikken en **Opties** te kiezen. Selecteer **Android-toepassing** in de sectie **Opbouwen**.
   
    Zorg ervoor dat de eerste letter van de **pakketnaam** een kleine letter is.
   
   > [!IMPORTANT]
   > De eerste letter van de pakketnaam moet een kleine letter zijn. Als dat niet zo is, worden er toepassingsmanifestfouten weergegeven wanneer u hieronder uw app voor pushmeldingen registreert.
   > 
   > 
   
      ![Visual Studio - Opties voor Android-project][24]
4. U kunt **Minimumversie van Android** desgewenst instellen op een ander API-niveau.
5. Ook kunt u **Doelversie van Android** instellen op een andere API-versie waarvoor u wilt ontwikkelen (dit moet API-niveau 8 of hoger zijn).
6. Kies **OK** en sluit het dialoogvenster Projectopties.

### <a name="add-the-required-packages-to-your-project"></a>Voeg de vereiste pakketten toe aan het project

1. Klik met de rechtermuisknop op het project en selecteer **Toevoegen** > **NuGet-pakketten toevoegen**
2. Zoek naar **Xamarin.Azure.NotificationHubs.Android** en voeg dat toe aan het project.
3. Zoek naar **Xamarin.Firebase.Messaging** en voeg dat toe aan het project.

### <a name="set-up-notification-hubs-in-your-project"></a>Notification Hubs in uw project instellen
1. Verzamel de volgende informatie voor uw Android-app en Notification Hub:
   
   * **Verbindingsreeks voor luisteren**: kies op het dashboard in [Azure Portal] de optie **Verbindingsreeksen weergeven**. Kopieer de *DefaultListenSharedAccessSignature*-verbindingsreeks voor deze waarde.
   * **Hubnaam**: dit is de naam van uw hub in [Azure Portal]. Bijvoorbeeld *mynotificationhub2*.
     
2. Maak een klasse **Constants.cs** voor uw Xamarin-project en definieer de volgende constantewaarden in de klasse. Vervang de tijdelijke aanduidingen door de waarden.
    
    ```csharp
        public static class Constants
        {
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
        }
    ```

3. Voeg de volgende using-instructies toe aan **MainActivity.cs**:
   
    ```csharp
        using Android.Util;
    ```

4. Voeg een exemplaarvariabele toe aan **MainActivity.cs** die wordt gebruikt voor het weergeven van een waarschuwingsvenster wanneer de app wordt uitgevoerd:
   
    ```csharp
        public const string TAG = "MainActivity";
    ```

5. Voeg de volgende code toe aan `OnCreate` in **MainActivity.cs** na `base.OnCreate(savedInstanceState)`:

    ```csharp   
        if (Intent.Extras != null)
        {
            foreach (var key in Intent.Extras.KeySet())
            {
                if(key!=null)
                {
                    var value = Intent.Extras.GetString(key);
                    Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
                }
            }
        }
    ```

6. Klik met de rechtermuisknop op uw project en voeg de `google-services.json` toe die u eerder uit uw Firebase-project hebt gedownload. Klik met de rechtermuisknop op het toegevoegde bestand en stel de build-actie in op `GoogleServicesJson`

    ![Visual Studio - google-services.json configureren][25]

7. Maak een nieuwe klasse, **MyFirebaseIIDService**.

8. Voeg de volgende using-instructies toe aan **MainActivity.cs**:
   
    ```csharp
        using System;
        using Android.App;
        using Firebase.Iid;
        using Android.Util;
        using WindowsAzure.Messaging;
        using System.Collections.Generic;
    ```

9. Voeg in **MyFirebaseIIDService.cs** het volgende toe boven de **class**-declaratie en laat uw klasse gegevens overnemen uit **FirebaseInstanceIdService**:
   
    ```csharp
        [Service]
        [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
        public class MyFirebaseIIDService : FirebaseInstanceIdService
    ```

10. Voeg in **MyFirebaseIIDService.cs** de volgende code toe:
   
    ```csharp
        const string TAG = "MyFirebaseIIDService";
        NotificationHub hub;

        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "FCM token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }

        void SendRegistrationToServer(string token)
        {
            // Register with Notification Hubs
            hub = new NotificationHub(Constants.NotificationHubName,
                                      Constants.ListenConnectionString, this);

            var tags = new List<string>() { };
            var regID = hub.Register(token, tags.ToArray()).RegistrationId;

            Log.Debug(TAG, $"Successful registration of ID {regID}");
        }
    ```

11. Maak een andere nieuwe klasse voor uw project en geef deze de naam **MyFirebaseMessagingService**.

12. Voeg de volgende using-instructies toe aan **MyFirebaseMessagingService.cs**.
    
    ```csharp
        using System;
        using System.Linq;
        using Android;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Firebase.Messaging;
    ```

13. Voeg het volgende toe boven de klassendeclaratie en laat uw klasse gegevens overnemen uit **FirebaseMessagingService**:
    
    ```csharp
        [Service]
        [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
        public class MyFirebaseIIDService : FirebaseMessagingService
    ```
    
14. Voeg de volgende code toe aan **MyFirebaseMessagingService.cs**:
    
    ```csharp
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            if(message.GetNotification()!= null)
            {
                //These is how most messages will be received
                Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
                SendNotification(message.GetNotification().Body);
            }
            else 
            {
                //Only used for debugging payloads sent from the Azure portal
                SendNotification(message.Data.Values.First());

            }

        }

        void SendNotification(string messageBody)
        {
            var intent = new Intent(this, typeof(MainActivity));
            intent.AddFlags(ActivityFlags.ClearTop);
            var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

            var notificationBuilder = new Notification.Builder(this)
                        .SetContentTitle("FCM Message")
                        .SetSmallIcon(Resource.Drawable.ic_launcher)
                        .SetContentText(messageBody)
                        .SetAutoCancel(true)
                        .SetContentIntent(pendingIntent);

            var notificationManager = NotificationManager.FromContext(this);

            notificationManager.Notify(0, notificationBuilder.Build());
        }
    ```

15. Uw app uitvoeren op uw apparaat of geladen emulator

## <a name="send-notifications-from-the-portal"></a>Meldingen verzenden vanuit de portal
U kunt ontvangst van meldingen in uw app testen met de optie *Test verzenden* in [Azure Portal]. Hiermee wordt een pushmelding als test naar uw apparaat verzonden.

![Azure Portal - Test verzenden][30]

Pushmeldingen worden gewoonlijk in een back-endservice zoals Mobile Services of ASP.NET verzonden via een compatibele bibliotheek. U kunt de REST API ook rechtstreeks gebruiken om meldingsberichten te verzenden als er geen bibliotheek beschikbaar is voor uw back-end.

Hier volgt een lijst met andere zelfstudies die u kunt bekijken voor het verzenden van meldingen:

* ASP.NET: zie [Notification Hubs gebruiken om pushmeldingen naar gebruikers te verzenden].
* Azure Notification Hubs Java SDK: zie [How to use Notification Hubs from Java](notification-hubs-java-push-notification-tutorial.md) (Notification Hubs gebruiken vanuit Java) voor het verzenden van meldingen vanuit Java. Dit is getest in Eclipse voor Android-ontwikkeling.
* PHP: zie [How to use Notification Hubs from PHP](notification-hubs-php-push-notification-tutorial.md) (Notification Hubs gebruiken vanuit PHP).

## <a name="next-steps"></a>Volgende stappen
In dit eenvoudige voorbeeld hebt u meldingen uitgezonden naar al uw Android-apparaten. Als u zich op specifieke gebruikers wilt richten, raadpleegt u de zelfstudie [Notification Hubs gebruiken om pushmeldingen naar gebruikers te verzenden]. Als u gebruikers wilt indelen op belangengroepen, raadpleegt u [Notification Hubs gebruiken om belangrijk nieuws te verzenden]. Lees meer over het gebruik van Notification Hubs in [Richtlijnen voor Notification Hubs] en in [Notification Hubs-procedure voor Android].

<!-- Anchors. -->
[Enable Google Cloud Messaging]: #register
[Configure your Notification Hub]: #configure-hub
[Connecting your app to the Notification Hub]: #connecting-app
[Run your app with the emulator]: #run-app
[Send notifications from your back-end]: #send
[Next steps]:#next-steps

<!-- Images. -->

[11]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-configure-android.png

[13]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app1.png
[15]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-app3.png

[18]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app7.png
[19]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-android-app8.png

[20]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-console-app.png
[21]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-android-toast.png
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png
[24]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-xamarin-android-app-options.png
[25]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-google-services-json.png

[30]: ./media/notification-hubs-android-get-started/notification-hubs-test-send.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js
[Visual Studio met Xamarin]: https://docs.microsoft.com/en-us/visualstudio/install/install-visual-studio
[Visual Studio voor Mac]: https://www.visualstudio.com/vs/visual-studio-mac/

[Azure Portal]: https://portal.azure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Richtlijnen voor Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs-procedure voor Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Notification Hubs gebruiken om pushmeldingen naar gebruikers te verzenden]: /manage/services/notification-hubs/notify-users-aspnet
[Notification Hubs gebruiken om belangrijk nieuws te verzenden]: /manage/services/notification-hubs/breaking-news-dotnet
[GitHub]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid
