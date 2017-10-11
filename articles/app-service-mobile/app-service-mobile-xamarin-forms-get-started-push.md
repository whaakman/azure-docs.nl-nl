---
title: Pushmeldingen toevoegen aan uw Xamarin.Forms-app | Microsoft Docs
description: Informatie over het gebruik van Azure-services voor meerdere platforms pushmeldingen verzendt naar uw apps voor Xamarin.Forms.
services: app-service\mobile
documentationcenter: xamarin
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: d9b1ba9a-b3f2-4d12-affc-2ee34311538b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: 912367636f1b26b3b07fbd5fe3fe8ed053218fd5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="add-push-notifications-to-your-xamarinforms-app"></a>Pushmeldingen toevoegen aan uw Xamarin.Forms-app
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Overzicht
In deze zelfstudie hebt u pushmeldingen toevoegen aan de projecten die het gevolg zijn van de [Xamarin.Forms snel starten](app-service-mobile-xamarin-forms-get-started.md). Dit betekent dat een push-melding wordt verzonden naar alle clients van de platformoverschrijdende telkens wanneer een record wordt ingevoegd.

Als u het gedownloade quick start-serverproject niet gebruikt, moet u het push notification-uitbreidingspakket. Zie voor meer informatie [werken met de .NET-back-endserver SDK voor Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="prerequisites"></a>Vereisten
Voor iOS, moet u een [Apple Developer Program-lidmaatschap](https://developer.apple.com/programs/ios/) en een fysiek iOS-apparaat. De [iOS-simulator biedt geen ondersteuning voor pushmeldingen](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/iOS_Simulator_Guide/TestingontheiOSSimulator.html).

## <a name="configure-hub"></a>Een notification hub configureren
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="update-the-server-project-to-send-push-notifications"></a>Bijwerken van het serverproject om pushmeldingen te verzenden
[!INCLUDE [app-service-mobile-update-server-project-for-push-template](../../includes/app-service-mobile-update-server-project-for-push-template.md)]

## <a name="configure-and-run-the-android-project-optional"></a>Configureren en uitvoeren van het Android-project (optioneel)
Deze sectie om in te schakelen pushmeldingen voor het Xamarin.Forms Droid-project voor Android worden voltooid.

### <a name="enable-firebase-cloud-messaging-fcm"></a>Firebase Cloud Messaging (FCM) inschakelen
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

### <a name="configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm"></a>De back-end voor mobiele Apps voor het verzenden van push-aanvragen via FCM configureren
[!INCLUDE [app-service-mobile-android-configure-push](../../includes/app-service-mobile-android-configure-push.md)]

### <a name="add-push-notifications-to-the-android-project"></a>Pushmeldingen toevoegen aan de Android-project
U kunt met de back-end geconfigureerd met het FCM onderdelen en codes toevoegen aan de client om te registreren bij FCM. U kunt ook registreren voor pushmeldingen met Azure Notification Hubs via de back-end van Mobile Apps en meldingen ontvangen.

1. In de **Droid** project, met de rechtermuisknop op de **onderdelen** map en klik op **meer onderdelen ophalen...** . Zoek vervolgens naar de **Google Cloud Messaging-Client** onderdeel en voeg deze toe aan het project. Dit onderdeel biedt ondersteuning voor pushmeldingen voor een Xamarin.android-project.
2. Open het projectbestand MainActivity.cs en voeg de volgende instructie toe aan de bovenkant van het bestand:

        using Gcm.Client;
3. Voeg de volgende code naar de **OnCreate** methode na het aanroepen van **LoadApplication**:

        try
        {
            // Check to ensure everything's set up right
            GcmClient.CheckDevice(this);
            GcmClient.CheckManifest(this);

            // Register for push notifications
            System.Diagnostics.Debug.WriteLine("Registering...");
            GcmClient.Register(this, PushHandlerBroadcastReceiver.SENDER_IDS);
        }
        catch (Java.Net.MalformedURLException)
        {
            CreateAndShowDialog("There was an error creating the client. Verify the URL.", "Error");
        }
        catch (Exception e)
        {
            CreateAndShowDialog(e.Message, "Error");
        }
4. Voeg een nieuwe **CreateAndShowDialog** Help-methode, als volgt:

        private void CreateAndShowDialog(String message, String title)
        {
            AlertDialog.Builder builder = new AlertDialog.Builder(this);

            builder.SetMessage (message);
            builder.SetTitle (title);
            builder.Create().Show ();
        }
5. Voeg de volgende code naar de **MainActivity** klasse:

        // Create a new instance field for this activity.
        static MainActivity instance = null;

        // Return the current activity instance.
        public static MainActivity CurrentActivity
        {
            get
            {
                return instance;
            }
        }

    Hiermee wordt de huidige **MainActivity** exemplaar, zodat kan worden uitgevoerd op de belangrijkste UI-thread.
6. Initialisatie van de `instance` variabele aan het begin van de **OnCreate** methode als volgt.

        // Set the current instance of MainActivity.
        instance = this;
7. Voeg een nieuw klassebestand naar de **Droid** project met de naam `GcmService.cs`, en zorg ervoor dat de volgende **met** instructies aan de bovenkant van het bestand aanwezig zijn:

        using Android.App;
        using Android.Content;
        using Android.Media;
        using Android.Support.V4.App;
        using Android.Util;
        using Gcm.Client;
        using Microsoft.WindowsAzure.MobileServices;
        using Newtonsoft.Json.Linq;
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.Text;
8. Voeg de volgende machtigingsaanvragen aan de bovenkant van het bestand toe na de **met** instructies en vóór de **naamruimte** declaratie.

        [assembly: Permission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "@PACKAGE_NAME@.permission.C2D_MESSAGE")]
        [assembly: UsesPermission(Name = "com.google.android.c2dm.permission.RECEIVE")]
        [assembly: UsesPermission(Name = "android.permission.INTERNET")]
        [assembly: UsesPermission(Name = "android.permission.WAKE_LOCK")]
        //GET_ACCOUNTS is only needed for android versions 4.0.3 and below
        [assembly: UsesPermission(Name = "android.permission.GET_ACCOUNTS")]
9. De volgende klassendefinitie toevoegen aan de naamruimte.

       [BroadcastReceiver(Permission = Gcm.Client.Constants.PERMISSION_GCM_INTENTS)]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_MESSAGE }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_REGISTRATION_CALLBACK }, Categories = new string[] { "@PACKAGE_NAME@" })]
       [IntentFilter(new string[] { Gcm.Client.Constants.INTENT_FROM_GCM_LIBRARY_RETRY }, Categories = new string[] { "@PACKAGE_NAME@" })]
       public class PushHandlerBroadcastReceiver : GcmBroadcastReceiverBase<GcmService>
       {
           public static string[] SENDER_IDS = new string[] { "<PROJECT_NUMBER>" };
       }

   > [!NOTE]
   > Vervang **< PROJECT_NUMBER >** met het projectnummer van uw u eerder hebt genoteerd.    
   >
   >
10. Vervang de lege **GcmService** klasse met de volgende code, die de nieuwe broadcast ontvanger gebruikt:

         [Service]
         public class GcmService : GcmServiceBase
         {
             public static string RegistrationID { get; private set; }

             public GcmService()
                 : base(PushHandlerBroadcastReceiver.SENDER_IDS){}
         }
11. Voeg de volgende code naar de **GcmService** klasse. Overschrijft dit de **OnRegistered** gebeurtenis-handler en implementeert een **registreren** methode.

        protected override void OnRegistered(Context context, string registrationId)
        {
            Log.Verbose("PushHandlerBroadcastReceiver", "GCM Registered: " + registrationId);
            RegistrationID = registrationId;

            var push = TodoItemManager.DefaultManager.CurrentClient.GetPush();

            MainActivity.CurrentActivity.RunOnUiThread(() => Register(push, null));
        }

        public async void Register(Microsoft.WindowsAzure.MobileServices.Push push, IEnumerable<string> tags)
        {
            try
            {
                const string templateBodyGCM = "{\"data\":{\"message\":\"$(messageParam)\"}}";

                JObject templates = new JObject();
                templates["genericMessage"] = new JObject
                {
                    {"body", templateBodyGCM}
                };

                await push.RegisterAsync(RegistrationID, templates);
                Log.Info("Push Installation Id", push.InstallationId.ToString());
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine(ex.Message);
                Debugger.Break();
            }
        }

    Merk op dat deze code gebruikt de `messageParam` parameter in de registratie van de sjabloon.
12. Voeg de volgende code waarmee **OnMessage**:

        protected override void OnMessage(Context context, Intent intent)
        {
            Log.Info("PushHandlerBroadcastReceiver", "GCM Message Received!");

            var msg = new StringBuilder();

            if (intent != null && intent.Extras != null)
            {
                foreach (var key in intent.Extras.KeySet())
                    msg.AppendLine(key + "=" + intent.Extras.Get(key).ToString());
            }

            //Store the message
            var prefs = GetSharedPreferences(context.PackageName, FileCreationMode.Private);
            var edit = prefs.Edit();
            edit.PutString("last_msg", msg.ToString());
            edit.Commit();

            string message = intent.Extras.GetString("message");
            if (!string.IsNullOrEmpty(message))
            {
                createNotification("New todo item!", "Todo item: " + message);
                return;
            }

            string msg2 = intent.Extras.GetString("msg");
            if (!string.IsNullOrEmpty(msg2))
            {
                createNotification("New hub message!", msg2);
                return;
            }

            createNotification("Unknown message details", msg.ToString());
        }

        void createNotification(string title, string desc)
        {
            //Create notification
            var notificationManager = GetSystemService(Context.NotificationService) as NotificationManager;

            //Create an intent to show ui
            var uiIntent = new Intent(this, typeof(MainActivity));

            //Use Notification Builder
            NotificationCompat.Builder builder = new NotificationCompat.Builder(this);

            //Create the notification
            //we use the pending intent, passing our ui intent over which will get called
            //when the notification is tapped.
            var notification = builder.SetContentIntent(PendingIntent.GetActivity(this, 0, uiIntent, 0))
                    .SetSmallIcon(Android.Resource.Drawable.SymActionEmail)
                    .SetTicker(title)
                    .SetContentTitle(title)
                    .SetContentText(desc)

                    //Set the notification sound
                    .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))

                    //Auto cancel will remove the notification once the user touches it
                    .SetAutoCancel(true).Build();

            //Show the notification
            notificationManager.Notify(1, notification);
        }

    Dit binnenkomende berichten worden verwerkt en zendt deze naar de notification manager moet worden weergegeven.
13. **GcmServiceBase** vereist ook dat u voor het implementeren van de **OnUnRegistered** en **bij fout** handler-methoden, die u kunt als volgt:

        protected override void OnUnRegistered(Context context, string registrationId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "Unregistered RegisterationId : " + registrationId);
        }

        protected override void OnError(Context context, string errorId)
        {
            Log.Error("PushHandlerBroadcastReceiver", "GCM Error: " + errorId);
        }

U bent nu klaar test pushmeldingen in de app uitgevoerd op een Android-apparaat of de emulator.

### <a name="test-push-notifications-in-your-android-app"></a>Pushmeldingen testen in uw Android-app
De eerste twee stappen zijn vereist, alleen wanneer u op een emulator test.

1. Zorg ervoor dat u implementeert voor of foutopsporing op een virtueel apparaat met Google APIs ingesteld als doel, zoals hieronder wordt weergegeven in de Android Virtual Device manager.
2. Een Google-account toevoegen aan de Android-apparaat door te klikken op **Apps** > **instellingen** > **account toevoegen**. Volg de aanwijzingen een bestaande Google-account toevoegen aan het apparaat, of een nieuwe maken.
3. In Visual Studio of Xamarin Studio, met de rechtermuisknop op de **Droid** project en klik op **instellen als opstartproject**.
4. Klik op **uitvoeren** bouw het project en de app op uw Android-apparaat of emulator te starten.
5. Typ een taak in de app en klik vervolgens op het plusteken (**+**) pictogram.
6. Controleer of dat er een melding wordt ontvangen wanneer een item wordt toegevoegd.

## <a name="configure-and-run-the-ios-project-optional"></a>Configureren en uitvoeren van het iOS-project (optioneel)
Deze sectie gaat over het uitvoeren van het Xamarin iOS-project voor iOS-apparaten. Als u niet met iOS-apparaten werkt, kunt u deze sectie overslaan.

[!INCLUDE [Enable Apple Push Notifications](../../includes/enable-apple-push-notifications.md)]

#### <a name="configure-the-notification-hub-for-apns"></a>De notification hub configureren voor APNS
[!INCLUDE [app-service-mobile-apns-configure-push](../../includes/app-service-mobile-apns-configure-push.md)]

Vervolgens configureert u de instelling van de iOS-project in Xamarin Studio of Visual Studio.

[!INCLUDE [app-service-mobile-xamarin-ios-configure-project](../../includes/app-service-mobile-xamarin-ios-configure-project.md)]

#### <a name="add-push-notifications-to-your-ios-app"></a>Pushmeldingen toevoegen aan uw iOS-app
1. In de **iOS** project, opent u AppDelegate.cs en de volgende instructie toe te voegen aan de bovenkant van het codebestand.

        using Newtonsoft.Json.Linq;
2. In de **AppDelegate** klasse, Voeg een onderdrukking voor de **RegisteredForRemoteNotifications** gebeurtenis registreren voor meldingen:

        public override void RegisteredForRemoteNotifications(UIApplication application,
            NSData deviceToken)
        {
            const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
                {
                  {"body", templateBodyAPNS}
                };

            // Register for push with your mobile app
            Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
            push.RegisterAsync(deviceToken, templates);
        }
3. In **AppDelegate**, Voeg ook de volgende onderdrukking voor de **DidReceiveRemoteNotification** gebeurtenis-handler:

        public override void DidReceiveRemoteNotification(UIApplication application,
            NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
        {
            NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

            string alert = string.Empty;
            if (aps.ContainsKey(new NSString("alert")))
                alert = (aps[new NSString("alert")] as NSString).ToString();

            //show alert
            if (!string.IsNullOrEmpty(alert))
            {
                UIAlertView avAlert = new UIAlertView("Notification", alert, null, "OK", null);
                avAlert.Show();
            }
        }

    Deze methode binnenkomende berichten worden verwerkt terwijl de app wordt uitgevoerd.
4. In de **AppDelegate** klasse, het toevoegen van de volgende code naar de **FinishedLaunching** methode:

        // Register for push notifications.
        var settings = UIUserNotificationSettings.GetSettingsForTypes(
            UIUserNotificationType.Alert
            | UIUserNotificationType.Badge
            | UIUserNotificationType.Sound,
            new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();

    Dit biedt ondersteuning voor externe meldingen en push-registratie van aanvragen.

Uw app is nu bijgewerkt ter ondersteuning van pushmeldingen.

#### <a name="test-push-notifications-in-your-ios-app"></a>Pushmeldingen testen in uw iOS-app
1. Met de rechtermuisknop op het iOS-project en klik op **instellen als opstartproject**.
2. Druk op de **uitvoeren** knop of **F5** in Visual Studio om het project bouwen en starten van de app in een iOS-apparaat. Klik vervolgens op **OK** pushmeldingen accepteren.

   > [!NOTE]
   > U moet expliciet pushmeldingen accepteren van uw app. Deze aanvraag treedt alleen op de eerste keer dat de app wordt uitgevoerd.
   >
   >
3. Typ een taak in de app en klik vervolgens op het plusteken (**+**) pictogram.
4. Controleer of een melding wordt ontvangen, en klik vervolgens op **OK** kunnen worden verwijderd van de melding.

## <a name="configure-and-run-windows-projects-optional"></a>Configureren en uitvoeren van Windows-projecten (optioneel)
Deze sectie is voor het uitvoeren van de Xamarin.Forms-WinApp en WinPhone81-projecten voor Windows-apparaten. Deze stappen bieden ook ondersteuning voor Universal Windows Platform (UWP)-projecten. Als u niet met Windows-apparaten werkt, kunt u deze sectie overslaan.

#### <a name="register-your-windows-app-for-push-notifications-with-windows-notification-service-wns"></a>Uw Windows-app voor pushmeldingen registreren met Windows Notification Service (WNS)
[!INCLUDE [app-service-mobile-register-wns](../../includes/app-service-mobile-register-wns.md)]

#### <a name="configure-the-notification-hub-for-wns"></a>De notification hub configureren voor WNS
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

#### <a name="add-push-notifications-to-your-windows-app"></a>Pushmeldingen toevoegen aan uw Windows-app
1. Open in Visual Studio **App.xaml.cs** in een Windows-project en voeg de volgende instructies toe.

        using Newtonsoft.Json.Linq;
        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
        using <your_TodoItemManager_portable_class_namespace>;

    Vervang `<your_TodoItemManager_portable_class_namespace>` met de naamruimte van uw draagbare project met de `TodoItemManager` klasse.
2. Voeg het volgende in App.xaml.cs **InitNotificationsAsync** methode:

        private async Task InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            const string templateBodyWNS =
                "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

            JObject headers = new JObject();
            headers["X-WNS-Type"] = "wns/toast";

            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBodyWNS},
                {"headers", headers} // Needed for WNS.
            };

            await TodoItemManager.DefaultManager.CurrentClient.GetPush()
                .RegisterAsync(channel.Uri, templates);
        }

    Deze methode opgehaald van het meldingskanaal push en een sjabloon voor het ontvangen van Sjabloonmeldingen van uw notification hub geregistreerd. Een sjabloon melding die ondersteuning biedt voor *messageParam* aan deze client wordt geleverd.
3. Werk in App.xaml.cs de **OnLaunched** gebeurtenis-handler methodedefinitie door toe te voegen de `async` aanpassingsfunctie. Voeg vervolgens de volgende coderegel toe aan het einde van de methode:

        await InitNotificationsAsync();

    Dit zorgt ervoor dat de registratie voor pushberichten wordt gemaakt of vernieuwd telkens wanneer de app wordt gestart. Het is belangrijk om ervoor te zorgen dat het kanaal WNS-push altijd actief is.  
4. Open in Solution Explorer voor Visual Studio de **Package.appxmanifest** bestand en stel **Toast geschikt** naar **Ja** onder **meldingen**.
5. De app bouwen en controleer of er geen fouten. Uw client-app moet zich nu registreren voor de Sjabloonmeldingen van de back-end van Mobile Apps. Herhaal deze sectie voor elke Windows-project in uw oplossing.

#### <a name="test-push-notifications-in-your-windows-app"></a>Pushmeldingen testen in uw Windows-app
1. Met de rechtermuisknop op een Windows-project in Visual Studio, en klik op **instellen als opstartproject**.
2. Druk op de knop **Uitvoeren** om het project te bouwen en de app te starten.
3. Typ een naam voor een nieuwe taken in de app en klik vervolgens op het plusteken (**+**) pictogram toe te voegen.
4. Controleer of dat er een melding wordt ontvangen wanneer het item is toegevoegd.

## <a name="next-steps"></a>Volgende stappen
U kunt meer informatie over pushmeldingen:

* [Push notification problemen vaststellen](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Er zijn diverse redenen waarom meldingen mogelijk ophalen verwijderd of kunnen niet eindigen op apparaten. Dit onderwerp leest u hoe te analyseren en te achterhalen van de hoofdoorzaken van fouten voor push-melding.

U kunt ook door te gaan naar een van de volgende zelfstudies:

* [Verificatie toevoegen aan uw app ](app-service-mobile-xamarin-forms-get-started-users.md)  
  Ontdek hoe u gebruikers van uw app verifieert met een id-provider.
* [Offlinesynchronisatie voor uw app inschakelen](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  Ontdek hoe u offlineondersteuning voor uw app toevoegt met behulp van een back-end voor Mobile Apps. Met het offline synchroniseren gebruikers kunnen communiceren met een mobiele app&mdash;weergeven, toevoegen of wijzigen van gegevens&mdash;zelfs wanneer er geen netwerkverbinding.

<!-- Images. -->

<!-- URLs. -->
[Install Xcode]: https://go.microsoft.com/fwLink/p/?LinkID=266532
[Xcode]: https://go.microsoft.com/fwLink/?LinkID=266532
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
