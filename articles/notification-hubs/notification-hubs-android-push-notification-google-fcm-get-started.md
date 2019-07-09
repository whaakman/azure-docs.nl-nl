---
title: Pushmeldingen naar Android-apps met Azure Notification Hubs en Firebase Cloud Messaging | Microsoft Docs
description: In deze zelfstudie leert u hoe u met Azure Notification Hubs en Google Firebase Cloud Messaging pushmeldingen verzendt naar Android-apparaten.
services: notification-hubs
documentationcenter: android
keywords: pushmeldingen,pushmelding,android-pushmelding, firebase cloud messaging
author: jwargo
manager: patniko
editor: spelluru
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/30/2019
ms.author: jowargo
ms.openlocfilehash: f2efa9b7e1e534f93e4ea01ba52740c8c5ac7b02
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67653839"
---
# <a name="tutorial-push-notifications-to-android-devices-by-using-azure-notification-hubs-and-google-firebase-cloud-messaging"></a>Zelfstudie: Pushmeldingen naar Android-apparaten met behulp van Azure Notification Hubs en Google Firebase Cloud Messaging

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

In deze zelfstudie wordt gedemonstreerd hoe u met Azure Notification Hubs en Firebase Cloud Messaging (FCM) meldingen kunt pushen naar een Android-toepassing. In deze zelfstudie gaat u een lege Android-app maken die pushmeldingen ontvangt via Firebase Cloud Messaging (FCM).

De volledige code voor deze zelfstudie kan worden gedownload [vanuit GitHub](https://github.com/Azure/azure-notificationhubs-android/tree/master/samples/FCMTutorialApp).

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een Android Studio-project maken.
> * Een Firebase-project maken dat Firebase Cloud Messaging ondersteunt.
> * Maak een hub.
> * Uw app verbinden met de hub.
> * De app testen.

## <a name="prerequisites"></a>Vereisten

U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/free/) voor meer informatie. 

U moet ook de volgende items: 

* De nieuwste versie van [Android Studio](https://go.microsoft.com/fwlink/?LinkId=389797)
* Android 2.3 of hoger voor Firebase Cloud Messaging
* Google Repository revisie 27 of hoger voor Firebase Cloud Messaging
* Google Play-Services 9.0.2 of hoger voor Firebase Cloud Messaging

Het voltooien van deze zelfstudie is een vereiste voor het uitvoeren van alle andere Notification Hubs-zelfstudies voor Android-apps.

## <a name="create-an-android-studio-project"></a>Een Android Studio-project maken

1. Start Android Studio.
2. Selecteer **bestand**, wijst u **nieuw**, en selecteer vervolgens **nieuw Project**. 
2. Op de **Kies uw project** weergeeft, schakelt **lege activiteit**, en selecteer vervolgens **volgende**. 
3. Op de **configureren van uw project** pagina, de volgende stappen uit: 
    1. Voer een naam in voor de toepassing.
    2. Geef een locatie op voor het opslaan van de project-bestanden. 
    3. Selecteer **Finish**. 

        ![Het project configureren](./media/notification-hubs-android-push-notification-google-fcm-get-started/configure-project.png)

## <a name="create-a-firebase-project-that-supports-fcm"></a>Een Firebase-project maken dat FCM ondersteunt

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-hub"></a>Een hub configureren

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-firebase-cloud-messaging-settings-for-the-hub"></a>Firebase Cloud Messaging-instellingen voor de hub configureren

1. In het linkerdeelvenster onder **instellingen** Selecteer **Google (GCM/FCM)** . 
2. Voer de **serversleutel** van het FCM-project dat u eerder hebt opgeslagen. 
3. Selecteer op de werkbalk **opslaan**. 

    ![Azure Notification Hub - Google (FCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/fcm-server-key.png)
4. De Azure-portal wordt een bericht weergegeven in waarschuwingen die de hub is bijgewerkt. De knop **Opslaan** kan niet worden gekozen. 

De hub is nu geconfigureerd om te werken met Firebase Cloud Messaging. U hebt ook de verbindingsreeksen die nodig zijn voor het verzenden van meldingen naar een apparaat en een app registreren voor het ontvangen van meldingen.

## <a id="connecting-app"></a>Uw app verbinden met de Notification Hub

### <a name="add-google-play-services-to-the-project"></a>Google Play-services aan het project toevoegen

1. Selecteer in Android Studio **extra** in het menu en selecteer vervolgens **SDK Manager**. 
2. Selecteer de doelversie van de Android SDK die wordt gebruikt in uw project. Selecteer vervolgens **Pakketgegevens weergeven**. 

    ![Android SDK Manager - doelversie selecteren](./media/notification-hubs-android-studio-add-google-play-services/notification-hubs-android-studio-sdk-manager.png)
3. Selecteer **Google APIs**, als deze nog niet geïnstalleerd.

    ![Android SDK Manager - Google API’s geselecteerd](./media/notification-hubs-android-studio-add-google-play-services/googole-apis-selected.png)
4. Schakel over naar het tabblad **SDK-hulpprogramma's** . Als u al Google Play Services nog niet hebt geïnstalleerd, selecteert u **Google Play Services** zoals wordt weergegeven in de volgende afbeelding. Selecteer vervolgens **toepassen** te installeren. Noteer het SDK-pad om het in een later stadium te kunnen gebruiken.

    ![Android SDK Manager - Google Play Services geselecteerd](./media/notification-hubs-android-studio-add-google-play-services/google-play-services-selected.png)
3. Klik op **OK** zodra het dialoogvenster **Wijziging bevestigen** wordt weergegeven. De vereiste onderdelen worden geïnstalleerd met behulp van het installatieprogramma voor onderdelen. Selecteer **Voltooien** zodra de onderdelen zijn geïnstalleerd.
4. Selecteer **OK** om het dialoogvenster **Instellingen voor nieuwe projecten** te sluiten.  
5. Selecteer **nu synchroniseren** pictogram op de werkbalk.
1. Open het bestand AndroidManifest.xml en voegt u de volgende code aan de *toepassing* tag.

    ```xml
    <meta-data android:name="com.google.android.gms.version"
         android:value="@integer/google_play_services_version" />
    ```


### <a name="add-azure-notification-hubs-libraries"></a>Azure Notification Hubs-bibliotheken toevoegen

1. In het Build.Gradle-bestand voor de app, voeg de volgende regels in het gedeelte met afhankelijkheden.

    ```gradle
    implementation 'com.microsoft.azure:notification-hubs-android-sdk:0.6@aar'
    implementation 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```

2. Voeg de volgende opslagplaats toe na het gedeelte met afhankelijkheden.

    ```gradle
    repositories {
        maven {
            url "https://dl.bintray.com/microsoftazuremobile/SDK"
        }
    }
    ```

### <a name="add-google-firebase-support"></a>Ondersteuning voor Google Firebase toevoegen

1. In het Build.Gradle-bestand voor de app, voeg de volgende regels in de **afhankelijkheden** sectie als deze nog niet bestaan. 

    ```gradle
    implementation 'com.google.firebase:firebase-core:16.0.8'
    implementation 'com.google.firebase:firebase-messaging:17.3.4'
    ```

2. Als deze nog niet is is, moet u de volgende invoegtoepassing toevoegen aan het einde van het bestand. 

    ```gradle
    apply plugin: 'com.google.gms.google-services'
    ```
3. Selecteer **Nu synchroniseren** op de werkbalk.

### <a name="update-the-androidmanifestxml-file"></a>Het bestand AndroidManifest.xml bijwerken

1. Nadat u uw FCM-registratietoken hebben ontvangen, gebruiken deze [registreren met Azure Notification Hubs](notification-hubs-push-notification-registration-management.md). Ondersteuning van deze registratie op de achtergrond met behulp van een `IntentService` met de naam `RegistrationIntentService`. Deze service wordt ook vernieuwd voor uw FCM-registratietoken.

    Voeg de volgende servicedefinitie toe aan het bestand AndroidManifest.xml in de `<application>`-tag.

    ```xml
    <service
        android:name=".RegistrationIntentService"
        android:exported="false">
    </service>
    ```

2. U moet ook een ontvanger voor het ontvangen van meldingen definiëren. Voeg de volgende ontvangerdefinitie toe aan het bestand AndroidManifest.xml in de `<application>`-tag. 

    ```xml
    <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
        android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <category android:name="<your package name>" />
        </intent-filter>
    </receiver>
    ```

    > [!IMPORTANT]
    > Vervang de `<your package NAME>` tijdelijke aanduiding door de werkelijke pakketnaam, dat wordt weergegeven aan de bovenkant van het bestand AndroidManifest.xml.
3. De volgende vereiste FCM gerelateerde machtigingen toevoegen hieronder de `</application>` tag.

    ```xml
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```

### <a name="add-code"></a>Voeg code toe:

1. Vouw in de Project-weergave **app** > **src** > **main** > **java** uit. Met de rechtermuisknop op de pakketmap onder **java**, selecteer **nieuw**, en selecteer vervolgens **Java-klasse**. Voer **NotificationSettings** voor de naam en selecteer vervolgens **OK**.

    Zorg ervoor dat u de volgende drie tijdelijke aanduidingen in de volgende code bijwerkt voor de klasse `NotificationSettings`:

   * **HubListenConnectionString**: De verbindingsreeks **DefaultListenAccessSignature** voor de hub. U kunt deze verbindingsreeks kopiëren door te klikken op **toegangsbeleid** in uw hub in de [Azure Portal].
   * **HubName**: Gebruik de naam van uw hub die wordt weergegeven in het hub-pagina in de [Azure Portal].

     `NotificationSettings`-code:

        ```java
        public class NotificationSettings {
            public static String HubName = "<Your HubName>";
            public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
        }
        ```

     > [!IMPORTANT]
     > Voer de **naam** en de **DefaultListenSharedAccessSignature** van uw hub voordat u doorgaat. 

3. Voeg een andere nieuwe klasse toe aan uw project met de naam `RegistrationIntentService`. Deze klasse implementeert de `IntentService` interface. Ook verwerkt deze [vernieuwen van het FCM-token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) en [registreren bij de notification hub](notification-hubs-push-notification-registration-management.md).

    Gebruik de volgende code voor deze klasse.

    ```java
    import android.app.IntentService;
    import android.content.Intent;
    import android.content.SharedPreferences;
    import android.preference.PreferenceManager;
    import android.util.Log;
    import com.google.android.gms.tasks.OnSuccessListener;
    import com.google.firebase.iid.FirebaseInstanceId;
    import com.google.firebase.iid.InstanceIdResult;
    import com.microsoft.windowsazure.messaging.NotificationHub;
    import java.util.concurrent.TimeUnit;

    public class RegistrationIntentService extends IntentService {

        private static final String TAG = "RegIntentService";
        String FCM_token = null;

        private NotificationHub hub;

        public RegistrationIntentService() {
            super(TAG);
        }

        @Override
        protected void onHandleIntent(Intent intent) {

            SharedPreferences sharedPreferences = PreferenceManager.getDefaultSharedPreferences(this);
            String resultString = null;
            String regID = null;
            String storedToken = null;

            try {
                FirebaseInstanceId.getInstance().getInstanceId().addOnSuccessListener(new OnSuccessListener<InstanceIdResult>() { 
                    @Override 
                    public void onSuccess(InstanceIdResult instanceIdResult) { 
                        FCM_token = instanceIdResult.getToken(); 
                        Log.d(TAG, "FCM Registration Token: " + FCM_token); 
                    } 
                }); 
                TimeUnit.SECONDS.sleep(1);

                // Storing the registration ID that indicates whether the generated token has been
                // sent to your server. If it is not stored, send the token to your server.
                // Otherwise, your server should have already received the token.
                if (((regID=sharedPreferences.getString("registrationID", null)) == null)){

                    NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                            NotificationSettings.HubListenConnectionString, this);
                    Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                    regID = hub.register(FCM_token).getRegistrationId();

                    // If you want to use tags...
                    // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                    // regID = hub.register(token, "tag1,tag2").getRegistrationId();

                    resultString = "New NH Registration Successfully - RegId : " + regID;
                    Log.d(TAG, resultString);

                    sharedPreferences.edit().putString("registrationID", regID ).apply();
                    sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                }

                // Check to see if the token has been compromised and needs refreshing.
                else if ((storedToken=sharedPreferences.getString("FCMtoken", "")) != FCM_token) {

                    NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                            NotificationSettings.HubListenConnectionString, this);
                    Log.d(TAG, "NH Registration refreshing with token : " + FCM_token);
                    regID = hub.register(FCM_token).getRegistrationId();

                    // If you want to use tags...
                    // Refer to : https://azure.microsoft.com/documentation/articles/notification-hubs-routing-tag-expressions/
                    // regID = hub.register(token, "tag1,tag2").getRegistrationId();

                    resultString = "New NH Registration Successfully - RegId : " + regID;
                    Log.d(TAG, resultString);

                    sharedPreferences.edit().putString("registrationID", regID ).apply();
                    sharedPreferences.edit().putString("FCMtoken", FCM_token ).apply();
                }

                else {
                    resultString = "Previously Registered Successfully - RegId : " + regID;
                }
            } catch (Exception e) {
                Log.e(TAG, resultString="Failed to complete registration", e);
                // If an exception happens while fetching the new token or updating registration data
                // on a third-party server, this ensures that we'll attempt the update at a later time.
            }

            // Notify UI that registration has completed.
            if (MainActivity.isVisible) {
                MainActivity.mainActivity.ToastNotify(resultString);
            }
        }
    }
    ```

4. In de `MainActivity` klasse, voeg de volgende `import` instructies boven de klassendeclaratie.

    ```java
    import com.google.android.gms.common.ConnectionResult;
    import com.google.android.gms.common.GoogleApiAvailability;
    import com.microsoft.windowsazure.notifications.NotificationsManager;
    import android.content.Intent;
    import android.util.Log;
    import android.widget.TextView;
    import android.widget.Toast;
    ```

5. Voeg de volgende leden toe bovenaan de klasse. U gebruikt deze velden [om de beschikbaarheid van Google Play Services te controleren, zoals aanbevolen door Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

    ```java
    public static MainActivity mainActivity;
    public static Boolean isVisible = false;
    private static final String TAG = "MainActivity";
    private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
    ```

6. In de `MainActivity` klasse, voegt u de volgende methode om te controleren of de beschikbaarheid van Google Play Services.

    ```java
    /**
    * Check the device to make sure it has the Google Play Services APK. If
    * it doesn't, display a dialog box that enables  users to download the APK from
    * the Google Play Store or enable it in the device's system settings.
    */

    private boolean checkPlayServices() {
        GoogleApiAvailability apiAvailability = GoogleApiAvailability.getInstance();
        int resultCode = apiAvailability.isGooglePlayServicesAvailable(this);
        if (resultCode != ConnectionResult.SUCCESS) {
            if (apiAvailability.isUserResolvableError(resultCode)) {
                apiAvailability.getErrorDialog(this, resultCode, PLAY_SERVICES_RESOLUTION_REQUEST)
                        .show();
            } else {
                Log.i(TAG, "This device is not supported by Google Play Services.");
                ToastNotify("This device is not supported by Google Play Services.");
                finish();
            }
            return false;
        }
        return true;
    }
    ```

7. In de `MainActivity` klasse, voeg de volgende code waarmee wordt gecontroleerd of Google Play Services voordat u de `IntentService` aan de registratie van uw FCM-token ophalen en registreren met de hub:

    ```java
    public void registerWithNotificationHubs()
    {
        if (checkPlayServices()) {
            // Start IntentService to register this application with FCM.
            Intent intent = new Intent(this, RegistrationIntentService.class);
            startService(intent);
        }
    }
    ```

8. In de `OnCreate` -methode van de `MainActivity` klasse, voeg de volgende code om het registratieproces te starten wanneer de activiteit wordt gemaakt:

    ```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mainActivity = this;
        registerWithNotificationHubs();
    }
    ```

9. Om te controleren of de status en het rapport status van de app in uw app, voeg deze extra methoden voor het `MainActivity`:

    ```java
    @Override
    protected void onStart() {
        super.onStart();
        isVisible = true;
    }

    @Override
    protected void onPause() {
        super.onPause();
        isVisible = false;
    }

    @Override
    protected void onResume() {
        super.onResume();
        isVisible = true;
    }

    @Override
    protected void onStop() {
        super.onStop();
        isVisible = false;
    }

    public void ToastNotify(final String notificationMessage) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(MainActivity.this, notificationMessage, Toast.LENGTH_LONG).show();
                TextView helloText = (TextView) findViewById(R.id.text_hello);
                helloText.setText(notificationMessage);
            }
        });
    }
    ```

10. Voor de methode `ToastNotify` wordt het besturingselement *Hello World* gebruikt `TextView` om de status en kennisgevingen permanent in de app te melden. Voeg in de indeling **res** > **layout** > **activity_main.xml** de volgende id toe voor het besturingselement.

    ```java
    android:id="@+id/text_hello"
    ```

11. Vervolgens voegt u een subklasse toe voor de ontvanger die u hebt gedefinieerd in AndroidManifest.xml. Voeg een andere nieuwe klasse toe aan uw project met de naam `MyHandler`.

12. Voeg boven in `MyHandler.java` de volgende importinstructie toe:

    ```java
    import android.app.NotificationChannel;
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;
    import android.media.RingtoneManager;
    import android.net.Uri;
    import android.os.Build;
    import android.os.Bundle;
    import android.support.v4.app.NotificationCompat;
    import com.microsoft.windowsazure.notifications.NotificationsHandler;    
    import com.microsoft.windowsazure.notifications.NotificationsManager;
    ```

13. Voeg de volgende code voor de `MyHandler` klasse, zodat dit een subklasse van `com.microsoft.windowsazure.notifications.NotificationsHandler`.

    Deze code overschrijft de `OnReceive`-methode, zodat de handler de ontvangen meldingen rapporteert. De handler verzendt ook de pushmelding naar Android Notification Manager met de methode `sendNotification()`. Roep de `sendNotification()` methode wanneer de app wordt niet uitgevoerd en een melding wordt ontvangen.

    ```java
    public class MyHandler extends NotificationsHandler {
        public static final String NOTIFICATION_CHANNEL_ID = "nh-demo-channel-id";
        public static final String NOTIFICATION_CHANNEL_NAME = "Notification Hubs Demo Channel";
        public static final String NOTIFICATION_CHANNEL_DESCRIPTION = "Notification Hubs Demo Channel";
    
        public static final int NOTIFICATION_ID = 1;
        private NotificationManager mNotificationManager;
        NotificationCompat.Builder builder;
        Context ctx;
    
        @Override
        public void onReceive(Context context, Bundle bundle) {
            ctx = context;
            String nhMessage = bundle.getString("message");
            sendNotification(nhMessage);
            if (MainActivity.isVisible) {
                MainActivity.mainActivity.ToastNotify(nhMessage);
            }
        }
    
        private void sendNotification(String msg) {
    
            Intent intent = new Intent(ctx, MainActivity.class);
            intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
    
            mNotificationManager = (NotificationManager)
                    ctx.getSystemService(Context.NOTIFICATION_SERVICE);
    
            PendingIntent contentIntent = PendingIntent.getActivity(ctx, 0,
                    intent, PendingIntent.FLAG_ONE_SHOT);
    
            Uri defaultSoundUri = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION);
            NotificationCompat.Builder notificationBuilder = new NotificationCompat.Builder(
                    ctx,
                    NOTIFICATION_CHANNEL_ID)
                    .setContentText(msg)
                    .setPriority(NotificationCompat.PRIORITY_HIGH)
                    .setSmallIcon(android.R.drawable.ic_popup_reminder)
                    .setBadgeIconType(NotificationCompat.BADGE_ICON_SMALL);
    
            notificationBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, notificationBuilder.build());
        }
    
        public static void createChannelAndHandleNotifications(Context context) {
            if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
                NotificationChannel channel = new NotificationChannel(
                        NOTIFICATION_CHANNEL_ID,
                        NOTIFICATION_CHANNEL_NAME,
                        NotificationManager.IMPORTANCE_HIGH);
                channel.setDescription(NOTIFICATION_CHANNEL_DESCRIPTION);
                channel.setShowBadge(true);
    
                NotificationManager notificationManager = context.getSystemService(NotificationManager.class);
                notificationManager.createNotificationChannel(channel);
                NotificationsManager.handleNotifications(context, "", MyHandler.class);
            }
        }
    }
    ```

14. In Android Studio in het menu Selecteer **bouwen** > **Project opnieuw opbouwen** om ervoor te zorgen dat er geen fouten in uw code. Als u een foutbericht ontvangt over de `ic_launcher` pictogram, verwijdert u de volgende instructie uit het bestand AndroidManifest.xml: 

    ```
        android:icon="@mipmap/ic_launcher"
    ```
15. De app uitvoeren op uw apparaat en controleer of dat deze correct is geregistreerd bij de hub.

    > [!NOTE]
    > Registratie kan mislukken tijdens de eerste keer starten totdat de `onTokenRefresh()` methode van de exemplaar-id-service wordt aangeroepen. De vernieuwing moet een succesvolle registratie met de Notification Hub tot stand brengen.

    ![De registratie van het apparaat is voltooid](./media/notification-hubs-android-push-notification-google-fcm-get-started/device-registration.png)

## <a name="test-send-notification-from-the-notification-hub"></a>Testen van melding verzenden vanuit de Notification Hub

U kunt pushmeldingen vanuit verzenden de [Azure Portal] door de volgende stappen uit:

1. Selecteer in de Azure portal, op de Notification Hub-pagina voor uw hub, **verzenden testen** in de **probleemoplossing** sectie.
3. Selecteer voor **Platforms** de optie **Android**.
4. Selecteer **Verzenden**.  U weergegeven niet een melding op het Android-apparaat nog omdat u de mobiele app nog niet hebt uitgevoerd. Nadat u de mobiele app uitvoeren, selecteert u de **verzenden** knop opnieuw op de melding ziet.
5. Zie het resultaat van de bewerking in de lijst aan de onderkant.

    ![Azure Notification Hubs - Verzenden testen](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)
6. U ziet de melding op uw apparaat. 

    ![Melding op apparaat](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-on-device.png)
    

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

### <a name="run-the-mobile-app-on-emulator"></a>De mobiele app uitvoeren in een emulator
Voordat u pushmeldingen binnen een emulator test, controleert u of de installatiekopie van de emulator het Google API-niveau die u hebt gekozen voor uw app ondersteunt. Als uw installatiekopie geen ondersteuning voor native Google APIs, krijgt u mogelijk de **SERVICE\_niet\_beschikbaar** uitzondering.

Bovendien, zorg ervoor dat u uw Google-account hebt toegevoegd aan uw actieve emulator onder **instellingen** > **Accounts**. Anders pogingen om opnieuw te registreren bij FCM kunnen leiden tot de **verificatie\_mislukt** uitzondering.

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie gebruikt u Firebase Cloud Messaging om te verzenden naar alle Android-apparaten die zijn geregistreerd bij de service. Ga verder met de volgende zelfstudie als u wilt weten hoe u pushmeldingen kunt verzenden naar specifieke apparaten:

> [!div class="nextstepaction"]
>[Zelfstudie: Pushmeldingen verzenden naar specifieke Android-apparaten](push-notifications-android-specific-devices-firebase-cloud-messaging.md)

<!-- Images. -->

<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: https://go.microsoft.com/fwlink/?LinkId=389800
[Notification Hubs Guidance]: notification-hubs-push-notification-overview.md
[Azure Portal]: https://portal.azure.com
