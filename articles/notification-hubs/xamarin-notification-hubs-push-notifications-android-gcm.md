---
title: Aan de slag met Notification Hubs voor Xamarin.Android-apps | Microsoft Docs
description: In deze zelfstudie leert u hoe u met Azure Notification Hubs pushmeldingen verzendt naar een Xamarin.Android-toepassing.
author: ysxu
manager: erikre
editor: 
services: notification-hubs
documentationcenter: xamarin
ms.assetid: 0be600fe-d5f3-43a5-9e5e-3135c9743e54
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 09/25/2017
ms.author: yuaxu
ms.openlocfilehash: f9fef96b71e0db7b15ff5208e9bd1a0b4ecf7211
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-notification-hubs-with-xamarin-for-android"></a>Aan de slag met Notification Hubs met Xamarin voor Android
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie wordt gedemonstreerd hoe u met Azure Notification Hubs pushmeldingen verzendt naar een Xamarin.Android-toepassing. U maakt een lege Xamarin.Android-app die pushmeldingen ontvangt via Google Cloud Messaging (GCM). Als u klaar bent, kunt u de Notification Hub gebruiken om pushmeldingen uit te zenden naar alle apparaten waarop uw app wordt uitgevoerd. De voltooide code is beschikbaar in het [NotificationHubs-app][GitHub]-voorbeeld.

In deze zelfstudie wordt een eenvoudig scenario voor het uitzenden met Notification Hubs beschreven.

## <a name="before-you-begin"></a>Voordat u begint
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

De volledige code voor deze zelfstudie vindt u [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/Xamarin/GetStartedXamarinAndroid) op GitHub.

## <a name="prerequisites"></a>Vereisten
Voor deze zelfstudie hebt u het volgende nodig:

* Visual Studio met Xamarin in Windows of Xamarin Studio in Mac OS X. U vindt de volledige installatie-instructies in [Instellen en installeren voor Visual Studio en Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).
* Actief Google-account
* [Azure Messaging-onderdeel]
* [Google Cloud Messaging-clientonderdeel]

Het voltooien van deze zelfstudie is een vereiste voor alle andere Notification Hubs-zelfstudies voor Xamarin.Android-apps.

> [!IMPORTANT]
> U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A9C9624B5&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fpartner-xamarin-notification-hubs-android-get-started%2F) voor meer informatie.
> 
> 

## <a name="enable-google-cloud-messaging"></a>Google Cloud Messaging inschakelen
[!INCLUDE [mobile-services-enable-Google-cloud-messaging](../../includes/mobile-services-enable-google-cloud-messaging.md)]

## <a name="configure-your-notification-hub"></a>Uw Notification Hub configureren
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="5">

<li><p>Kies bovenaan het tabblad <b>Configureren</b>, voer de waarde voor <b>API-sleutel</b> in die u in de vorige sectie hebt verkregen en selecteer <b>Opslaan</b>.</p>
</li>
</ol>
&emsp;&emsp;![](./media/notification-hubs-android-get-started/notification-hub-configure-android.png)

De Notification Hub is nu geconfigureerd om te werken met GCM en u hebt de verbindingsreeksen om uw app te registreren voor het ontvangen van meldingen en voor het verzenden van pushmeldingen.

## <a name="connect-your-app-to-the-notification-hub"></a>Uw app verbinden met de Notification Hub
Eerst maakt u een nieuw project.
1. Kies in Xamarin Studio **Nieuwe oplossing** > **Android-app** en selecteer **Volgende**.
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project1.png)

2. Voer uw **appnaam** en **id** in. Kies de **doelplatforms** die u wilt ondersteunen en kies vervolgens **Volgende** en **Maken**.
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-create-xamarin-android-project2.png)

    Hierop wordt een nieuw Android-project gemaakt.

3. Open de projecteigenschappen door met de rechtermuisknop op het nieuwe project in de weergave Oplossing te klikken en **Opties** te kiezen. Selecteer **Android-toepassing** in de sectie **Opbouwen**.
   
    Zorg ervoor dat de eerste letter van de **pakketnaam** een kleine letter is.
   
   > [!IMPORTANT]
   > De eerste letter van de pakketnaam moet een kleine letter zijn. Als dit niet zo is, worden er toepassingsmanifestfouten weergegeven wanneer u hieronder uw **BroadcastReceiver** en **IntentFilter** voor pushmeldingen registreert.
   > 
   > 
   
      ![](./media/partner-xamarin-notification-hubs-android-get-started/notification-hub--xamarin-android-app-options.png)
4. U kunt **Minimumversie van Android** desgewenst instellen op een ander API-niveau.
5. Ook kunt u **Doelversie van Android** instellen op een andere API-versie waarvoor u wilt ontwikkelen (dit moet een API-niveau 8 of hoger zijn).
6. Kies **OK** en sluit het dialoogvenster Projectopties.

### <a name="add-the-required-components-to-your-project"></a>De vereiste onderdelen toevoegen aan uw project
De Google Cloud Messaging-client die beschikbaar is in Xamarin Component Store vereenvoudigt het ondersteunen van pushmeldingen in Xamarin.Android.

1. Klik met de rechtermuisknop op de map Onderdelen in de Xamarin.Android-app en kies **Meer onderdelen ophalen**.
2. Zoek het onderdeel **Azure Messaging** en voeg dit toe aan het project.
3. Zoek het onderdeel **Google Cloud Messaging-client** en voeg dit toe aan het project.

### <a name="set-up-notification-hubs-in-your-project"></a>Notification Hubs in uw project instellen
1. Verzamel de volgende informatie voor uw Android-app en Notification Hub:
   
   * **GoogleProjectNumber**: haal deze projectnummerwaarde op uit het overzicht van uw app in de Google-portal voor ontwikkelaars. U hebt deze waarde genoteerd toen u de app in de portal maakte.
   * **Verbindingsreeks voor luisteren**: kies op het dashboard in de [klassieke Azure Portal] de optie **Verbindingsreeksen weergeven**. Kopieer de *DefaultListenSharedAccessSignature*-verbindingsreeks voor deze waarde.
   * **Hubnaam**: dit is de naam van uw hub uit de [klassieke Azure Portal]. Bijvoorbeeld *mynotificationhub2*.
     
2. Maak een klasse **Constants.cs** voor uw Xamarin-project en definieer de volgende constantewaarden in de klasse. Vervang de tijdelijke aanduidingen door de waarden.
     
        public static class Constants
        {
     
           public const string SenderID = "<GoogleProjectNumber>"; // Google API Project Number
           public const string ListenConnectionString = "<Listen connection string>";
           public const string NotificationHubName = "<hub name>";
        }
3. Voeg de volgende using-instructies toe aan **MainActivity.cs**:
   
        using Android.Util;
        using Gcm.Client;
4. Voeg een exemplaarvariabele toe aan de klasse `MainActivity` die wordt gebruikt voor het weergeven van een waarschuwingsvenster wanneer de app wordt uitgevoerd:
   
        public static MainActivity instance;
5. Maak de volgende methode in de klasse **MainActivity**:
   
        private void RegisterWithGCM()
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);
   
            // Register for push notifications
            Log.Info("MainActivity", "Registering...");
            GcmClient.Register(this, Constants.SenderID);
        }
6. Initialiseer in de `OnCreate`-methode van **MainActivity.cs**de `instance`-variabele en voeg een aanroep naar `RegisterWithGCM` toe:
   
        protected override void OnCreate (Bundle bundle)
        {
            instance = this;
   
            base.OnCreate (bundle);
   
            // Set your view from the "main" layout resource
            SetContentView (Resource.Layout.Main);
   
            // Get your button from the layout resource,
            // and attach an event to it
            Button button = FindViewById<Button> (Resource.Id.myButton);
   
            RegisterWithGCM();
        }
7. Maak een nieuwe klasse, **MyBroadcastReceiver**.
   
   > [!NOTE]
   > U wordt hieronder begeleid bij het maken van een geheel nieuwe klasse **BroadcastReceiver**. Voor een snel alternatief voor het handmatig maken van **MyBroadcastReceiver.cs** wordt u verwezen naar het bestand **GcmService.cs** in het Xamarin.Android-voorbeeldproject bij de [NotificationHubs-voorbeelden][GitHub]. Het dupliceren van **GcmService.cs** en wijzigen van klassenamen kan ook een uitstekend uitgangspunt zijn.
   > 
   > 
8. Voeg de volgende using-instructies toe aan **MyBroadcastReceiver.cs** (waarbij u verwijst naar het onderdeel en de assembly die u eerder hebt toegevoegd):
   
        using System.Collections.Generic;
        using System.Text;
        using Android.App;
        using Android.Content;
        using Android.Util;
        using Gcm.Client;
        using WindowsAzure.Messaging;
9. Voeg in **MyBroadcastReceiver.cs** de volgende machtigingsaanvragen toe tussen de **using**-instructies en de **namespace**-declaratie:
   
        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
   
        //GET_ACCOUNTS is needed only for Android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
10. Wijzig in **MyBroadcastReceiver.cs** de klasse **MyBroadcastReceiver** zodat deze overeenkomt met het volgende:
   
        [BroadcastReceiver(Permission=Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY },
            Categories = new string[] { "@PACKAGE_NAME@" })]
        public class MyBroadcastReceiver : GcmBroadcastReceiverBase<PushHandlerService>
        {
            public static string[] SENDER_IDS = new string[] { Constants.SenderID };
   
            public const string TAG = "MyBroadcastReceiver-GCM";
        }
11. Voeg een andere klasse in **MyBroadcastReceiver.cs** toe met de naam **PushHandlerService** die is afgeleid van **GcmServiceBase**. Zorg ervoor dat het kenmerk **Service** op de klasse wordt toegepast:
    
         [Service] // Must use the service tag
         public class PushHandlerService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }
             private NotificationHub Hub { get; set; }
    
             public PushHandlerService() : base(Constants.SenderID)
                {
                 Log.Info(MyBroadcastReceiver.TAG, "PushHandlerService() constructor");
             }
         }
12. Met **GcmServiceBase** worden de volgende methoden geïmplementeerd: **OnRegistered()**, **OnUnRegistered()**, **OnMessage()**, **OnRecoverableError()** en **OnError()**. De gebruikte **PushHandlerService**-implementatieklasse moet deze methoden overschrijven en deze methoden worden als reactie op de interactie met de Notification Hub geactiveerd.
13. Overschrijf de **OnRegistered()**-methode in **PushHandlerService** met de volgende code:
    
         protected override void OnRegistered(Context context, string registrationId)
         {
             Log.Verbose(MyBroadcastReceiver.TAG, "GCM Registered: " + registrationId);
             RegistrationID = registrationId;
    
             createNotification("PushHandlerService-GCM Registered...",
                                 "The device has been Registered!");
    
             Hub = new NotificationHub(Constants.NotificationHubName, Constants.ListenConnectionString,
                                         context);
             try
             {
                 Hub.UnregisterAll(registrationId);
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
    
             //var tags = new List<string>() { "falcons" }; // create tags if you want
             var tags = new List<string>() {};
    
             try
             {
                 var hubRegistration = Hub.Register(registrationId, tags.ToArray());
             }
             catch (Exception ex)
             {
                 Log.Error(MyBroadcastReceiver.TAG, ex.Message);
             }
         }
    
    > [!NOTE]
    > Zoals u ziet, kunnen met de bovenstaande **OnRegistered()**-code tags worden opgegeven om te registreren voor specifieke berichtenkanalen.
    > 
    > 
14. Overschrijf de **OnMessage**-methode in **PushHandlerService** met de volgende code:
    
        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info(MyBroadcastReceiver.TAG, "GCM Message Received!");
    
            var msg = new StringBuilder();
    
            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }
    
            string messageText = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty (messageText))
            {
                createNotification ("New hub message!", messageText);
            }
            else
            {
                createNotification ("Unknown message details", msg.ToString ());
            }
        }
15. Voeg de volgende **createNotification**- en **dialogNotify**-methoden toe aan **PushHandlerService** om gebruikers te informeren als een melding wordt ontvangen.
    
    > [!NOTE]
    > Het meldingsontwerp in Android versie 5.0 en hoger wijkt aanzienlijk af van vorige versies. Als u dit met Android 5.0 of hoger test, moet de app actief zijn om de melding te ontvangen. Zie [Android-meldingen](http://go.microsoft.com/fwlink/?LinkId=615880) voor meer informatie.
    > 
    > 
    
        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;
    
            //Create an intent to show UI
            var uiIntent = new Intent(this, typeof(MainActivity));
    
            //Create the notification
            var notification = new Notification(Android.Resource.Drawable.SymActionEmail, title);
    
            //Auto-cancel will remove the notification once the user touches it
            notification.Flags = NotificationFlags.AutoCancel;
    
            //Set the notification info
            //we use the pending intent, passing our ui intent over, which will get called
            //when the notification is tapped.
            notification.SetLatestEventInfo(this, title, desc, PendingIntent.GetActivity(this, 0, uiIntent, 0));
    
            //Show the notification
            notificationManager.Notify(1, notification);
            dialogNotify (title, desc);
        }
    
        protected void dialogNotify(String title, String message)
        {
    
            MainActivity.instance.RunOnUiThread(() => {
                AlertDialog.Builder dlg = new AlertDialog.Builder(MainActivity.instance);
                AlertDialog alert = dlg.Create();
                alert.SetTitle(title);
                alert.SetButton("Ok", delegate {
                    alert.Dismiss();
                });
                alert.SetMessage(message);
                alert.Show();
            });
        }
16. Overschrijf abstracte leden **OnUnRegistered()**, **OnRecoverableError()** en **OnError()** zodat uw code wordt gecompileerd:
    
        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Verbose(MyBroadcastReceiver.TAG, "GCM Unregistered: " + registrationId);
    
            createNotification("GCM Unregistered...", "The device has been unregistered!");
        }
    
        protected override bool OnRecoverableError(Context context, string errorId)
        {
            Log.Warn(MyBroadcastReceiver.TAG, "Recoverable Error: " + errorId);
    
            return base.OnRecoverableError (context, errorId);
        }
    
        protected override void OnError(Context context, string errorId)
        {
            Log.Error(MyBroadcastReceiver.TAG, "GCM Error: " + errorId);
        }

## <a name="run-your-app-in-the-emulator"></a>Uw app uitvoeren in de emulator
Als u deze app in de emulator uitvoert, gebruikt u een Android Virtual Device (AVD) dat ondersteuning biedt voor Google API's.

> [!IMPORTANT]
> Als u pushmeldingen wilt ontvangen, moet u een Google-account instellen op uw Android Virtual Device. (Ga in de emulator naar **Instellingen** en kies **Account toevoegen**.) Zorg er ook voor dat de emulator is verbonden met internet.
> 
> [!NOTE]
> Het meldingsontwerp in Android versie 5.0 en hoger wijkt aanzienlijk af van vorige versies. Zie [Android-meldingen](http://go.microsoft.com/fwlink/?LinkId=615880) voor meer informatie.
> 
> 

1. In **Extra** kiest u **Android Emulator Manager openen**. Selecteer uw apparaat en kies **Bewerken**.
   
      ![][18]
2. Selecteer **Google API's** in **Doel** en kies **OK**.
   
      ![][19]
3. Kies in de bovenste werkbalk de optie **Uitvoeren** en selecteer uw app. Hiermee start u de emulator en voert u de app uit.
   
   De app haalt de *registrationId* op uit GCM en wordt geregistreerd bij de Notification Hub.

## <a name="send-notifications-from-your-backend"></a>Meldingen verzenden vanuit uw back-end
U kunt de ontvangst van meldingen in uw app testen door meldingen in de [klassieke Azure Portal] te verzenden via het foutopsporingstabblad op de Notification Hub (zie onderstaande scherm).

![][30]

Pushmeldingen worden gewoonlijk in een back-endservice zoals Mobile Services of ASP.NET verzonden met een compatibele bibliotheek. U kunt de REST API ook direct gebruiken om meldingsberichten te verzenden als er geen bibliotheek beschikbaar is voor uw back-end.

Hier volgt een lijst met andere zelfstudies die u kunt bekijken voor het verzenden van meldingen:

* ASP.NET: zie [Notification Hubs gebruiken om pushmeldingen naar gebruikers te verzenden].
* Azure Notification Hubs Java SDK: zie [How to use Notification Hubs from Java](notification-hubs-java-push-notification-tutorial.md) (Notification Hubs gebruiken vanuit Java) voor het verzenden van meldingen vanuit Java. Dit is getest in Eclipse voor Android-ontwikkeling.
* PHP: zie [How to use Notification Hubs from PHP](notification-hubs-php-push-notification-tutorial.md) (Notification Hubs gebruiken vanuit PHP).

In de volgende subsecties van de zelfstudie verzendt u meldingen met een .NET-console-app en met een mobiele service via een knooppuntscript.

#### <a name="optional-send-notifications-by-using-a-net-app"></a>(Optioneel) Meldingen verzenden met een .NET-app
In deze sectie worden meldingen verzonden met een .NET-console-app.

1. Maak een nieuwe Visual C#-consoletoepassing:
   
      ![][20]
2. Kies in Visual Studio achtereenvolgens **Tools** > **NuGet Package Manager** en **Package Manager Console**.
   
    Hiermee wordt de Package Manager-console in Visual Studio weergegeven.
3. Stel in het venster Package Manager-console het **standaardproject** in op uw nieuwe consoletoepassingsproject en voer vervolgens in het consolevenster de volgende opdracht uit:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Hiermee wordt een verwijzing toegevoegd aan de Azure Notification Hubs SDK met het <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-pakket</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Open het bestand Program.cs en voeg de volgende `using`-instructie toe:
   
        using Microsoft.Azure.NotificationHubs;
5. Voeg in de klasse `Program` de volgende methode toe. Werk de tekst van de tijdelijke aanduidingen bij met uw *DefaultFullSharedAccessSignature*-verbindingsreeks en hubnaam uit de [klassieke Azure Portal].
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            await hub.SendGcmNativeNotificationAsync("{ \"data\" : {\"message\":\"Hello from Azure!\"}}");
        }
6. Voeg de volgende regels in de **Main**-methode toe:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Druk op de toets F5 om de app uit te voeren. U ontvangt een melding in de app.
   
      ![][21]

#### <a name="optional-send-notifications-by-using-a-mobile-service"></a>(Optioneel) Meldingen verzenden met een mobiele service
1. Volg [Aan de slag met Mobile Services].
2. Meld u aan bij de [klassieke Azure Portal] en selecteer uw mobiele service.
3. Selecteer het tabblad **Scheduler** aan de bovenkant.
   
      ![][22]
4. Maak een nieuwe geplande taak, voeg een naam in en selecteer **Op aanvraag**.
   
      ![][23]
5. Wanneer de taak is gemaakt, selecteert u de taaknaam. Ga vervolgens naar het tabblad **Script** op de bovenste balk.
6. Voeg het volgende script in de plannerfunctie in. Zorg ervoor dat de tijdelijke aanduidingen worden vervangen door de Notification Hub-naam en de verbindingsreeks voor *DefaultFullSharedAccessSignature* die u eerder hebt verkregen. Kies **Opslaan**.
   
        var azure = require('azure');
        var notificationHubService = azure.createNotificationHubService('<hub name>', '<connection string>');
        notificationHubService.gcm.send(null,'{"data":{"message" : "Hello from Mobile Services!"}}',
          function (error)
          {
            if (!error) {
               console.warn("Notification successful");
            }
            else
            {
              console.warn("Notification failed" + error);
            }
          }
        );
7. Kies op de onderste balk de optie **Eenmaal uitvoeren**. U ontvangt een pop-upmelding.

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
[22]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler1.png
[23]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hub-scheduler2.png

[30]: ./media/partner-xamarin-notification-hubs-android-get-started/notification-hubs-debug-hub-gcm.png


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Aan de slag met Mobile Services]: /develop/mobile/tutorials/get-started-xamarin-android/#create-new-service
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[klassieke Azure Portal]: https://manage.windowsazure.com/
[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Richtlijnen voor Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs-procedure voor Android]: http://msdn.microsoft.com/library/dn282661.aspx

[Notification Hubs gebruiken om pushmeldingen naar gebruikers te verzenden]: /manage/services/notification-hubs/notify-users-aspnet
[Notification Hubs gebruiken om belangrijk nieuws te verzenden]: /manage/services/notification-hubs/breaking-news-dotnet
[GCMClient Component page]: http://components.xamarin.com/view/GCMClient
[Xamarin.NotificationHub GitHub page]: https://github.com/SaschaDittmann/Xamarin.NotificationHub
[GitHub]: http://go.microsoft.com/fwlink/p/?LinkId=331329
[Google Cloud Messaging-clientonderdeel]: http://components.xamarin.com/view/GCMClient/
[Azure Messaging-onderdeel]: http://components.xamarin.com/view/azure-messaging
