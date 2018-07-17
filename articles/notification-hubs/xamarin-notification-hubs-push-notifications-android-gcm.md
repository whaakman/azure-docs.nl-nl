---
title: Pushmeldingen verzenden naar Xamarin.Android-apps met Azure Notification Hubs | Microsoft Docs
description: In deze zelfstudie leert u hoe u met Azure Notification Hubs pushmeldingen verzendt naar een Xamarin.Android-toepassing.
author: dimazaid
manager: kpiteira
editor: spelluru
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 4c537b06c4ff50d90d9fd5a847b378038f252790
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38972271"
---
# <a name="tutorial-push-notifications-to-xamarinandroid-apps-using-azure-notification-hubs"></a>Zelfstudie: Pushmeldingen verzenden naar Xamarin.Android-apps met Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie wordt gedemonstreerd hoe u met Azure Notification Hubs pushmeldingen verzendt naar een Xamarin.Android-toepassing. U maakt een lege Xamarin.Android-app die pushmeldingen ontvangt via Firebase Cloud Messaging (FCM). U gebruikt u de Notification Hub om pushmeldingen uit te zenden naar alle apparaten waarop uw app wordt uitgevoerd. De voltooide code is beschikbaar in het [NotificationHubs-app][GitHub]-voorbeeld.

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een Firebase-project maken en Firebase Cloud Messaging inschakelen
> * Een Notification Hub maken
> * Een Xamarin.Android-app maken en deze verbinden met de Notification Hub
> * Testmeldingen verzenden vanuit Azure Portal

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.
- [Visual Studio met Xamarin] onder Windows of [Visual Studio voor Mac] onder OS X.
- Actief Google-account

## <a name="create-a-firebase-project-and-enable-firebase-cloud-messaging"></a>Een Firebase-project maken en Firebase Cloud Messaging inschakelen
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="create-a-notification-hub"></a>Een Notification Hub maken
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-gcm-settings-for-the-notification-hub"></a>GCM-instellingen configureren voor de Notification Hub

1. Selecteer **Google (GCM)** in de sectie **MELDINGSINSTELLINGEN**. 
2. Voer de sleutel voor de oude server in die u eerder hebt genoteerd in de Google Firebase Console. 
3. Selecteer **Opslaan** op de werkbalk. 

    ![](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

De Notification Hub is geconfigureerd om te werken met FCM en u hebt de verbindingsreeksen om uw app te registreren voor het ontvangen van meldingen en voor het verzenden van pushmeldingen.

## <a name="create-xamarinandroid-app-and-connect-it-to-notification-hub"></a>Een Xamarin.Android-app maken en deze verbinden met de Notification Hub

### <a name="create-visual-studio-project-and-add-nuget-packages"></a>Visual Studio-project maken en NuGet-pakketten toevoegen
1. Wijs in Visual Studio naar **File**, selecteer **New** en selecteer vervolgens **Project**. 
   
      ![Visual Studio - Een nieuw Android-project maken](./media/partner-xamarin-notification-hubs-android-get-started/new-project-dialog.png)
2. Vouw in het venster **Solution Explorer** het item **Properties** uit en klik op **AndroidManifest.xml**. Wijzig de naam van het pakket zodat deze overeenkomt met de pakketnaam die u hebt ingevoerd tijdens het toevoegen van Firebase Cloud Messaging aan uw project in Google Firebase Console. 

      ![Pakketnaam in GCM](./media/partner-xamarin-notification-hubs-android-get-started/package-name-gcm.png)
3. Klik met de rechtermuisknop op het project en selecteer **Manage NuGet Packages...**. 
4. Selecteer het tabblad **Browse**. Zoek naar **Xamarin.GooglePlayServices.Base**. Selecteer **Xamarin.GooglePlayServices.Base** in de lijst met resultaten. Selecteer vervolgens **Install**. 

      ![Google Play Services NuGet](./media/partner-xamarin-notification-hubs-android-get-started/google-play-services-nuget.png)
5. Zoek in het venster **NuGet Package Manager** naar **Xamarin.Firebase.Messaging**. Selecteer **Xamarin.Firebase.Messaging** in de lijst met resultaten. Selecteer vervolgens **Install**. 
6. Zoek nu naar **Xamarin.Azure.NotificationHubs.Android**. Selecteer **Xamarin.Azure.NotificationHubs.Android** in de lijst met resultaten. Selecteer vervolgens **Install**. 

### <a name="add-the-google-services-json-file"></a>Voeg het JSON-bestand van Google Services toe

1. Kopieer het bestand **google-services.json** dat u hebt gedownload uit Google Firebase Console naar de projectmap.
2. Voeg **google-services.json** toe aan het project.
3. Selecteer **google-services.json** in het venster **Solution Explorer**.
4. Stel in het deelvenster **Properties** de optie Build Action in op **GoogleServicesJson**. Als u **GoogleServicesJson** niet ziet, sluit u Visual Studio, opent u het programma opnieuw, opent u het project opnieuw en probeert u het nog een keer. 

      ![Build Action van GoogleServicesJson](./media/partner-xamarin-notification-hubs-android-get-started/google-services-json-build-action.png)

### <a name="set-up-notification-hubs-in-your-project"></a>Notification Hubs in uw project instellen

#### <a name="registering-with-firebase-cloud-messaging"></a>Firebase Cloud Messaging registreren

Open het bestand **AndroidManifest.xml** en voeg de volgende `<receiver>`-elementen toe aan het element `<application>`:

        <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
        <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
          <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
            <category android:name="${applicationId}" />
          </intent-filter>
        </receiver>

1. Verzamel de volgende informatie voor uw Android-app en Notification Hub:
   
   * **Verbindingsreeks voor luisteren**: kies op het dashboard in [Azure Portal] de optie **Verbindingsreeksen weergeven**. Kopieer de *DefaultListenSharedAccessSignature*-verbindingsreeks voor deze waarde.
   * **Hubnaam**: de naam van uw hub in [Azure Portal]. Bijvoorbeeld *mynotificationhub2*.
     
2. Klik in het venster **Solution Explorer** met de rechtermuisknop op uw **project**, wijs naar **Add** en selecteer **Class**. 
4. Maak een klasse **Constants.cs** voor uw Xamarin-project en definieer de volgende constantewaarden in de klasse. Vervang de tijdelijke aanduidingen door de waarden.
    
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
7. Maak een nieuwe klasse, **MyFirebaseIIDService**, op de manier waarop u de klasse **Constants** hebt gemaakt. 
8. Voeg de volgende using-instructies toe aan **MainActivity.cs**:
   
    ```csharp
    using Android.App;
    using Android.Util;
    using WindowsAzure.Messaging;
    using Firebase.Iid;
    ```

9. Voeg in **MyFirebaseIIDService.cs** de volgende **class**-declaratie toe en laat uw klasse gegevens overnemen uit **FirebaseInstanceIdService**:
   
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
        using Android.App;
        using Android.Util;
        using Firebase.Messaging;
    ```
13. Voeg het volgende toe boven de klassendeclaratie en laat uw klasse gegevens overnemen uit **FirebaseMessagingService**:
    
    ```csharp
        [Service]
        [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
        public class MyFirebaseMessagingService : FirebaseMessagingService
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
15. **Compileer** het nieuwe project. 
16. **Voer uw app uit** op uw apparaat of in een geladen emulator.

## <a name="send-test-notification-from-the-azure-portal"></a>Testmeldingen verzenden vanuit Azure Portal
U kunt ontvangst van meldingen in uw app testen met de optie *Test verzenden* in [Azure Portal]. Er wordt dan een pushmelding als test naar uw apparaat verzonden.

![Azure Portal - Test verzenden](media/partner-xamarin-notification-hubs-android-get-started/send-test-notification.png)

Pushmeldingen worden gewoonlijk in een back-endservice zoals Mobile Services of ASP.NET verzonden via een compatibele bibliotheek. U kunt de REST API ook rechtstreeks gebruiken om meldingsberichten te verzenden als er geen bibliotheek beschikbaar is voor uw back-end.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u meldingen uitgezonden naar al uw Android-apparaten die zijn geregistreerd bij de back-end. Als u wilt weten hoe u pushmeldingen kunt verzenden naar specifieke Android-apparaten, gaat u verder met de volgende zelfstudie: 

> [!div class="nextstepaction"]
>[Pushmeldingen verzenden naar specifieke apparaten](notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md)


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
[Visual Studio met Xamarin]: https://docs.microsoft.com/visualstudio/install/install-visual-studio
[Visual Studio voor Mac]: https://www.visualstudio.com/vs/visual-studio-mac/

[Azure Portal]: https://portal.azure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Use Notification Hubs to push notifications to users]: /manage/services/notification-hubs/notify-users-aspnet
[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet
[GitHub]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid
