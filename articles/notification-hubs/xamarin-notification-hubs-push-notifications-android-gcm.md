---
title: Pushmeldingen verzenden naar Xamarin.Android-apps met Azure Notification Hubs | Microsoft Docs
description: In deze zelfstudie leert u hoe u met Azure Notification Hubs pushmeldingen verzendt naar een Xamarin.Android-toepassing.
author: jwargo
manager: patniko
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
ms.date: 03/28/2019
ms.author: jowargo
ms.openlocfilehash: 03cfecb2faaacbe1017fb4e7acfa3c475c18a9ab
ms.sourcegitcommit: f8c592ebaad4a5fc45710dadc0e5c4480d122d6f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58620017"
---
# <a name="tutorial-push-notifications-to-xamarinandroid-apps-using-azure-notification-hubs"></a>Zelfstudie: Pushmeldingen verzenden naar Xamarin.Android-apps met behulp van Azure Notification Hubs

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt gedemonstreerd hoe u met Azure Notification Hubs pushmeldingen verzendt naar een Xamarin.Android-toepassing. U maakt een lege Xamarin.Android-app die pushmeldingen ontvangt via Firebase Cloud Messaging (FCM). U gebruikt u de Notification Hub om pushmeldingen uit te zenden naar alle apparaten waarop uw app wordt uitgevoerd. De voltooide code is beschikbaar in de [NotificationHubs-app](https://github.com/Azure/azure-notificationhubs-dotnet/tree/master/Samples/Xamarin/GetStartedXamarinAndroid) voorbeeld.

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een Firebase-project maken en Firebase Cloud Messaging inschakelen
> * Een Notification Hub maken
> * Een Xamarin.Android-app maken en deze verbinden met de Notification Hub
> * Testmeldingen verzenden vanuit Azure Portal

## <a name="prerequisites"></a>Vereisten

* **Azure-abonnement**. Als u nog geen abonnement op Azure hebt, [maakt u een gratis Azure-account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.
* [Visual Studio met Xamarin] onder Windows of [Visual Studio voor Mac] onder OS X.
* Actief Google-account

## <a name="create-a-firebase-project-and-enable-firebase-cloud-messaging"></a>Een Firebase-project maken en Firebase Cloud Messaging inschakelen

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging-xamarin.md)]

## <a name="create-a-notification-hub"></a>Een Notification Hub maken

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-gcm-settings-for-the-notification-hub"></a>GCM-instellingen configureren voor de Notification Hub

1. Selecteer **Google (GCM)** in de sectie **MELDINGSINSTELLINGEN**.
2. Voer de sleutel voor de **oude server** in die u eerder hebt genoteerd in de Google Firebase Console.
3. Selecteer **Opslaan** op de werkbalk.

    ![](./media/notification-hubs-android-get-started/notification-hubs-gcm-api.png)

De Notification Hub is geconfigureerd om te werken met FCM en u hebt de verbindingsreeksen om uw app te registreren voor het ontvangen van meldingen en voor het verzenden van pushmeldingen.

## <a name="create-a-xamarinandroid-app-and-connect-it-to-notification-hub"></a>Een Xamarin.Android-app maken en deze verbinden met de Notification Hub

### <a name="create-visual-studio-project-and-add-nuget-packages"></a>Visual Studio-project maken en NuGet-pakketten toevoegen

1. Open in Visual Studio het menu **File** en selecteer **New** en vervolgens **Project**. In de **nieuw Project** venster, voert u deze stappen uit: 
    1. Vouw **geïnstalleerde**, **Visual C#** , en klik vervolgens op **Android**.
    2. Selecteer **Android-App (Xamarin)** in de lijst. 
    3. Voer een **naam** in voor het project. 
    4. Selecteer een **locatie** voor het project. 
    5. Selecteer **OK** 

        ![Het dialoogvenster Nieuw project](./media/partner-xamarin-notification-hubs-android-get-started/new-project-dialog-new.png)        
2. Op de **nieuwe Android-App** in het dialoogvenster, selecteer **lege App**, en selecteer **OK**. 

    ![Het dialoogvenster Nieuw project](./media/partner-xamarin-notification-hubs-android-get-started/new-android-app-dialog.png)
1. Vouw in het venster **Solution Explorer** het item **Properties** uit en klik op **AndroidManifest.xml**. Wijzig de naam van het pakket zodat deze overeenkomt met de pakketnaam die u hebt ingevoerd tijdens het toevoegen van Firebase Cloud Messaging aan uw project in Google Firebase Console.

    ![Pakketnaam in GCM](./media/partner-xamarin-notification-hubs-android-get-started/package-name-gcm.png)
3. Klik met de rechtermuisknop op het project en selecteer **Manage NuGet Packages...**.
4. Selecteer het tabblad **Browse**. Zoek naar **Xamarin.GooglePlayServices.Base**. Selecteer **Xamarin.GooglePlayServices.Base** in de lijst met resultaten. Selecteer vervolgens **Install**.

    ![Google Play Services NuGet](./media/partner-xamarin-notification-hubs-android-get-started/google-play-services-nuget.png)
5. Zoek in het venster **NuGet Package Manager** naar **Xamarin.Firebase.Messaging**. Selecteer **Xamarin.Firebase.Messaging** in de lijst met resultaten. Selecteer vervolgens **Install**.
6. Zoek nu naar **Xamarin.Azure.NotificationHubs.Android**. Selecteer **Xamarin.Azure.NotificationHubs.Android** in de lijst met resultaten. Selecteer vervolgens **Install**.

### <a name="add-the-google-services-json-file"></a>Voeg het JSON-bestand van Google Services toe

1. Kopieer het bestand `google-services.json` dat u uit de Google Firebase Console hebt gedownload naar de projectmap.
2. Voeg `google-services.json` toe aan het project.
3. Selecteer `google-services.json` in het venster **Solution Explorer**.
4. Stel in het deelvenster **Properties** de optie Build Action in op **GoogleServicesJson**. Als u **GoogleServicesJson** niet ziet, sluit u Visual Studio, opent u het programma opnieuw, opent u het project opnieuw en probeert u het nog een keer.

    ![Build Action van GoogleServicesJson](./media/partner-xamarin-notification-hubs-android-get-started/google-services-json-build-action.png)

### <a name="set-up-notification-hubs-in-your-project"></a>Notification Hubs in uw project instellen

#### <a name="registering-with-firebase-cloud-messaging"></a>Firebase Cloud Messaging registreren

1. Open het bestand `AndroidManifest.xml` en voeg de volgende `<receiver>`-elementen toe aan het element `<application>`:

    ```xml
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
        </intent-filter>
    </receiver>
    ```
2. Voeg de volgende instructies **voordat de toepassing** element. 

    ```xml
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    <uses-permission android:name="android.permission.WAKE_LOCK" />
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    ```
1. Verzamel de volgende informatie voor uw Android-app en Notification Hub:

   * **Verbindingsreeks voor luisteren**: Kies op het dashboard in de [Azure-portal] de optie **Verbindingsreeksen weergeven**. Kopieer de verbindingsreeks `DefaultListenSharedAccessSignature` voor deze waarde.
   * **Hubnaam**: Naam van uw hub in de [Azure-portal]. Bijvoorbeeld *mynotificationhub2*.
3. Klik in het venster **Solution Explorer** met de rechtermuisknop op uw **project**, selecteer **Add** en vervolgens **Class**.
4. Maak een klasse `Constants.cs` voor uw Xamarin-project en definieer de volgende constantewaarden in de klasse. Vervang de tijdelijke aanduidingen door de waarden.

    ```csharp
    public static class Constants
    {
        public const string ListenConnectionString = "<Listen connection string>";
        public const string NotificationHubName = "<hub name>";
    }
    ```
5. Voeg de volgende using-instructies toe aan `MainActivity.cs`:

    ```csharp
    using Android.Util;
    using Android.Gms.Common;
    ```
6. De volgende eigenschappen toevoegen aan de klasse MainActivity. De TAG-variabele wordt gebruikt om weer te geven van een waarschuwingsvenster wanneer de app wordt uitgevoerd:

    ```csharp
    public const string TAG = "MainActivity";
    internal static readonly string CHANNEL_ID = "my_notification_channel";
    ```
7. Voeg de volgende methode toe aan de klasse MainActivity. Er wordt gecontroleerd of **Google Play Services** zijn beschikbaar op het apparaat. 

    ```csharp
    public bool IsPlayServicesAvailable()
    {
        int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
        if (resultCode != ConnectionResult.Success)
        {
            if (GoogleApiAvailability.Instance.IsUserResolvableError(resultCode))
                Log.Debug(TAG, GoogleApiAvailability.Instance.GetErrorString(resultCode));
            else
            {
                Log.Debug(TAG, "This device is not supported");
                Finish();
            }
            return false;
        }
     
        Log.Debug(TAG, "Google Play Services is available.");
        return true;
    }
    ```
1. Voeg de volgende methode toe aan de MainActivity-klasse die wordt gemaakt van een meldingskanaal voor.

    ```csharp
    private void CreateNotificationChannel()
    {
        if (Build.VERSION.SdkInt < BuildVersionCodes.O)
        {
            // Notification channels are new in API 26 (and not a part of the
            // support library). There is no need to create a notification
            // channel on older versions of Android.
            return;
        }
     
        var channelName = CHANNEL_ID;
        var channelDescription = string.Empty;
        var channel = new NotificationChannel(CHANNEL_ID, channelName, NotificationImportance.Default)
        {
            Description = channelDescription
        };
     
        var notificationManager = (NotificationManager)GetSystemService(NotificationService);
        notificationManager.CreateNotificationChannel(channel);
    }
    ```
1. Voeg in `MainActivity.cs` de volgende code toe aan `OnCreate`, achter `base.OnCreate(savedInstanceState)`:

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
    
    IsPlayServicesAvailable();
    CreateNotificationChannel();
    ```
8. Maak een nieuwe klasse `MyFirebaseIIDService` zoals u ook klasse `Constants` hebt gemaakt.
9. Voeg de volgende using-instructies toe aan `MyFirebaseIIDService.cs`:

    ```csharp
    using Android.App;
    using Android.Util;
    using WindowsAzure.Messaging;
    using Firebase.Iid;
    ```

10. Voeg in `MyFirebaseIIDService.cs` de volgende `class`-declaratie toe en zorg dat uw klasse eigenschappen overneemt van `FirebaseInstanceIdService`:

    ```csharp
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class MyFirebaseIIDService : FirebaseInstanceIdService
    ```
11. Voeg in `MyFirebaseIIDService.cs` de volgende code toe:

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
12. Maak nog een nieuwe klasse voor uw project en noem deze `MyFirebaseMessagingService`.
13. Voeg de volgende using-instructies toe aan `MyFirebaseMessagingService.cs`.

    ```csharp
    using Android.App;
    using Android.Util;
    using Firebase.Messaging;
    using Android.OS;
    using Android.Support.V4.App;
    using Build = Android.OS.Build;
    ```
14. Voeg het volgende toe boven de klassedeclaratie en laat uw klasse gegevens overnemen van `FirebaseMessagingService`:

    ```csharp
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class MyFirebaseMessagingService : FirebaseMessagingService
    ```
15. Voeg de volgende code toe aan `MyFirebaseMessagingService.cs`:

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

        var notificationBuilder = new NotificationCompat.Builder(this)
                    .SetContentTitle("FCM Message")
                    .SetSmallIcon(Resource.Drawable.ic_launcher)
                    .SetContentText(messageBody)
                    .SetAutoCancel(true)
                    .SetShowWhen(false)
                    .SetContentIntent(pendingIntent);

        if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
        {
            notificationBuilder.SetChannelId(MainActivity.CHANNEL_ID);
        }

        var notificationManager = NotificationManager.FromContext(this);

        notificationManager.Notify(0, notificationBuilder.Build());
    }
    ```
16. **Compileer** het nieuwe project.
17. **Voer uw app uit** op uw apparaat of in een geladen emulator.

## <a name="send-test-notification-from-the-azure-portal"></a>Testmeldingen verzenden vanuit Azure Portal

U kunt ontvangst van meldingen in uw app testen met de optie **Test verzenden** in [Azure-portal]. Er wordt dan een pushmelding als test naar uw apparaat verzonden.

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
[Submit an app page]: https://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: https://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: https://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js
[Visual Studio met Xamarin]: https://docs.microsoft.com/visualstudio/install/install-visual-studio
[Visual Studio voor Mac]: https://www.visualstudio.com/vs/visual-studio-mac/
[Azure-portal]: https://portal.azure.com/
[wns object]: https://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: https://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Android]: https://msdn.microsoft.com/library/dn282661.aspx
[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-ios-apple-apns-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[GitHub]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid
