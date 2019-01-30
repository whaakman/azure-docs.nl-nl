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
ms.date: 01/04/2019
ms.author: jowargo
ms.openlocfilehash: d758a46a19dcd109ee5786e25c886dd30b264f7e
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54447494"
---
# <a name="tutorial-push-notifications-to-android-devices-by-using-azure-notification-hubs-and-google-firebase-cloud-messaging"></a>Zelfstudie: Pushmeldingen naar Android-apparaten met behulp van Azure Notification Hubs en Google Firebase Cloud Messaging

[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

> [!IMPORTANT]
> In dit artikel worden pushmeldingen met Google Firebase Cloud Messaging (FCM) beschreven. Als u gebruikmaakt van Google Cloud Messaging (GCM), verwijzen wij u naar [Push notifications to Android devices with Azure Notification Hubs and GCM](notification-hubs-android-push-notification-google-gcm-get-started.md) (Meldingen pushen naar Android-apparaten met Azure Notification Hubs en GCM).

In deze zelfstudie wordt gedemonstreerd hoe u met Azure Notification Hubs Firebase Cloud Messaging meldingen kunt pushen naar een Android-toepassing. In deze zelfstudie gaat u een lege Android-app maken die pushmeldingen ontvangt via Firebase Cloud Messaging (FCM).

U kunt de voltooide code voor deze zelfstudie [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase) downloaden op GitHub.

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Een Android Studio-project maken.
> * Een Firebase-project maken dat Firebase Cloud Messaging ondersteunt.
> * Een Notification Hub maken.
> * Uw app verbinden met de Notification Hub.
> * De app testen.

## <a name="prerequisites"></a>Vereisten

U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/free/) voor meer informatie.

* Naast een actief Azure-account, zoals hierboven vermeld, hebt u voor deze zelfstudie nog de meest recente versie van [Android Studio](https://go.microsoft.com/fwlink/?LinkId=389797) nodig.
* Android 2.3 of hoger voor Firebase Cloud Messaging.
* Google Repository revisie 27 of hoger is vereist voor Firebase Cloud Messaging.
* Google Play-Services 9.0.2 of hoger voor Firebase Cloud Messaging.
* Het voltooien van deze zelfstudie is een vereiste voor alle andere Notification Hubs-zelfstudies voor Android-apps.

## <a name="create-an-android-studio-project"></a>Een Android Studio-project maken

1. Start een nieuw Android Studio-project in Android Studio.

    ![Android Studio - nieuw project](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)
2. Kies de vormfactor voor **Telefoon en tablet** en de **minimale SDK** die u wilt ondersteunen. Klik op **Volgende**.

    ![Android Studio - werkstroom voor het maken van een project](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)
3. Kies **Lege activiteit** als belangrijkste activiteit, klik op **Volgende** en klik vervolgens op **Voltooien**.

## <a name="create-a-firebase-project-that-supports-fcm"></a>Een Firebase-project maken dat FCM ondersteunt

[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-notification-hub"></a>Een Notification Hub configureren

[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

### <a name="configure-firebase-cloud-messaging-settings-for-the-hub"></a>Firebase Cloud Messaging-instellingen voor de hub configureren

1. Selecteer **Google (GCM)** in de categorie **MELDINGSINSTELLINGEN**.
2. Voer de API-sleutel (FCM-serversleutel) die u eerder hebt gekopieerd uit de [Firebase console](https://firebase.google.com/console/) in.
3. Selecteer **Opslaan** op de werkbalk.

    ![Azure Notification Hubs - Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

De Notification Hub is nu geconfigureerd voor Firebase Cloud Messaging en u hebt de verbindingsreeksen om uw app te registreren voor het ontvangen en verzenden van pushmeldingen.

## <a id="connecting-app"></a>Uw app verbinden met de Notification Hub

### <a name="add-google-play-services-to-the-project"></a>Google Play-services aan het project toevoegen

[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Azure Notification Hubs-bibliotheken toevoegen

1. Voeg in het bestand `Build.Gradle` voor de **app** de volgende regels toe in het gedeelte **afhankelijkheden**.

    ```text
    compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
    compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
    ```

2. Voeg de volgende opslagplaats toe na het gedeelte **afhankelijkheden**.

    ```text
    repositories {
        maven {
            url "http://dl.bintray.com/microsoftazuremobile/SDK"
        }
    }
    ```

### <a name="add-google-firebase-support"></a>Ondersteuning voor Google Firebase toevoegen

1. Voeg in het bestand `Build.Gradle` voor de **app** de volgende regels toe in het gedeelte **afhankelijkheden**.

    ```text
    compile 'com.google.firebase:firebase-core:12.0.0'
    ```

2. Voeg de volgende invoegtoepassing toe aan het einde van het bestand:

    ```text
    apply plugin: 'com.google.gms.google-services'
    ```

### <a name="updating-the-androidmanifestxml"></a>AndroidManifest.xml bijwerken

1. Als u FCM wilt ondersteunen, moet u een exemplaar-id listenerservice in de code implementeren die wordt gebruikt voor het [verkrijgen van registratietokens](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) met de [Google's FirebaseInstanceId API](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId). In deze zelfstudie is de naam van de klasse `MyInstanceIDService`.

    Voeg de volgende servicedefinitie toe aan het bestand AndroidManifest.xml in de `<application>`-tag.

    ```xml
    <service android:name=".MyInstanceIDService">
        <intent-filter>
            <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
        </intent-filter>
    </service>
    ```

2. Nadat u het FCM-registratietoken van de FirebaseInstanceId API hebt ontvangen, gebruikt u dit voor [registratie bij de Azure Notification Hub](notification-hubs-push-notification-registration-management.md). U ondersteunt deze registratie op de achtergrond met een `IntentService` met de naam `RegistrationIntentService`. Deze service is ook verantwoordelijk voor het vernieuwen van uw FCM-registratietoken.

    Voeg de volgende servicedefinitie toe aan het bestand AndroidManifest.xml in de `<application>`-tag.

    ```xml
    <service
        android:name=".RegistrationIntentService"
        android:exported="false">
    </service>
    ```

3. U moet bovendien een ontvanger voor het ontvangen van meldingen definiëren. Voeg de volgende ontvangerdefinitie toe aan het bestand AndroidManifest.xml in de `<application>`-tag. Vervang de tijdelijke aanduiding `<your package>` door de werkelijke pakketnaam die bovenaan het bestand `AndroidManifest.xml` wordt weergegeven.

    ```xml
    <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
        android:permission="com.google.android.c2dm.permission.SEND">
        <intent-filter>
            <action android:name="com.google.android.c2dm.intent.RECEIVE" />
            <category android:name="<your package name>" />
        </intent-filter>
    </receiver>
    ```

4. Voeg de volgende vereiste FCM-gerelateerde machtigingen toe onder de `</application>`-tag.

    Zie voor meer informatie over deze machtigingen [Een GCM Client App instellen voor Android](https://developers.google.com/cloud-messaging/android/client#manifest) en [Een GCM Client App voor Android migreren naar Firebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).

    ```xml
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
    ```

### <a name="adding-code"></a>Code toevoegen

1. Vouw in de Project-weergave **app** > **src** > **main** > **java** uit. Klik met de rechtermuisknop op de pakketmap onder **java**, klik op **Nieuw** en klik vervolgens op **Java-klasse**. Voeg een nieuwe klasse met de naam `NotificationSettings` toe.

    ![Android Studio - nieuwe Java-klasse](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)

    Zorg ervoor dat u de volgende drie tijdelijke aanduidingen in de volgende code bijwerkt voor de klasse `NotificationSettings`:

   * **SenderId**: De Verzenders-id die u eerder hebt verkregen op het tabblad **Cloud Messaging** van de projectinstellingen in de [Firebase-console](https://firebase.google.com/console/).
   * **HubListenConnectionString**: De verbindingsreeks **DefaultListenAccessSignature** voor de hub. Kopieer deze verbindingsreeks door te klikken op **Toegangsbeleid** in uw hub in [Azure Portal].
   * **HubName**: Gebruik de naam van uw Notification Hub die wordt weergegeven op de hubpagina in de [Azure Portal].

     `NotificationSettings`-code:

    ```java
    public class NotificationSettings {
        public static String SenderId = "<Your project number>";
        public static String HubName = "<Your HubName>";
        public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
    }
    ```

2. Voeg aan de hand van de voorgaande stappen een volgende klasse toe met de naam `MyInstanceIDService`. Deze klasse is de implementatie van uw exemplaar-id listenerservice.

    De code voor deze klasse roept uw `IntentService` aan voor het op de achtergrond [vernieuwen van het FCM-token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).

    ```java
    import android.content.Intent;
    import android.util.Log;
    import com.google.firebase.iid.FirebaseInstanceIdService;

    public class MyInstanceIDService extends FirebaseInstanceIdService {

        private static final String TAG = "MyInstanceIDService";

        @Override
        public void onTokenRefresh() {

            Log.d(TAG, "Refreshing GCM Registration Token");

            Intent intent = new Intent(this, RegistrationIntentService.class);
            startService(intent);
        }
    };
    ```

3. Voeg een andere nieuwe klasse toe aan uw project met de naam `RegistrationIntentService`. Met deze klasse implementeert u de `IntentService`-interface die zorgt voor [het vernieuwen van het FCM-token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) en [de registratie bij de Notification Hub](notification-hubs-push-notification-registration-management.md).

    Gebruik de volgende code voor deze klasse.

    ```java
    import android.app.IntentService;
    import android.content.Intent;
    import android.content.SharedPreferences;
    import android.preference.PreferenceManager;
    import android.util.Log;
    import com.google.firebase.iid.FirebaseInstanceId;
    import com.microsoft.windowsazure.messaging.NotificationHub;

    public class RegistrationIntentService extends IntentService {

        private static final String TAG = "RegIntentService";

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
                String FCM_token = FirebaseInstanceId.getInstance().getToken();
                Log.d(TAG, "FCM Registration Token: " + FCM_token);

                // Storing the registration ID that indicates whether the generated token has been
                // sent to your server. If it is not stored, send the token to your server,
                // otherwise your server should have already received the token.
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

                // Check if the token may have been compromised and needs refreshing.
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
                // If an exception happens while fetching the new token or updating our registration data
                // on a third-party server, this ensures that we'll attempt the update at a later time.
            }

            // Notify UI that registration has completed.
            if (MainActivity.isVisible) {
                MainActivity.mainActivity.ToastNotify(resultString);
            }
        }
    }
    ```

4. Voeg in uw klasse `MainActivity` de volgende instructies voor `import` toe boven de klassendeclaratie.

    ```java
    import com.google.android.gms.common.ConnectionResult;
    import com.google.android.gms.common.GoogleApiAvailability;
    import com.microsoft.windowsazure.notifications.NotificationsManager;
    import android.content.Intent;
    import android.util.Log;
    import android.widget.TextView;
    import android.widget.Toast;
    ```

5. Voeg de volgende privé-leden toe aan de bovenkant van de klasse. U gebruikt deze velden [om de beschikbaarheid van Google Play Services te controleren, zoals aanbevolen door Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).

    ```java
    public static MainActivity mainActivity;
    public static Boolean isVisible = false;
    private static final String TAG = "MainActivity";
    private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
    ```

6. Voeg in uw klasse `MainActivity` de volgende methode toe om de beschikbaarheid van Google Play Services te controleren.

    ```java
    /**
    * Check the device to make sure it has the Google Play Services APK. If
    * it doesn't, display a dialog that allows users to download the APK from
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

7. Voeg in uw klasse `MainActivity` de volgende code toe waarmee Google Play Services wordt gecontroleerd voordat u uw `IntentService` aanroept om uw FCM-registratietoken op te halen en te registreren met de Notification Hub.

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

8. In de methode `OnCreate` van de klasse `MainActivity` voegt u de volgende code toe om het registratieproces te starten wanneer de activiteit wordt gemaakt.

    ```java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        mainActivity = this;
        NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
        registerWithNotificationHubs();
    }
    ```

9. Voeg deze extra methoden toe aan de `MainActivity` om de status van de app te controleren en een rapport van de status in uw app op te nemen.

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

10. Voor de methode `ToastNotify` wordt het besturingselement *Hello World* gebruikt `TextView` om de status en kennisgevingen permanent in de app te melden. In de indeling activity_main.xml voegt u de volgende id toe voor het besturingselement.

    ```java
    android:id="@+id/text_hello"
    ```

11. Vervolgens gaat u een subklasse toevoegen voor de ontvanger die u hebt gedefinieerd in AndroidManifest.xml. Voeg een andere nieuwe klasse toe aan uw project met de naam `MyHandler`.

12. Voeg boven in `MyHandler.java` de volgende importinstructie toe:

    ```java
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;
    import android.media.RingtoneManager;
    import android.net.Uri;
    import android.os.Bundle;
    import android.support.v4.app.NotificationCompat;
    import com.microsoft.windowsazure.notifications.NotificationsHandler;
    ```

13. Voeg de volgende code toe voor de klasse `MyHandler`, zodat dit een subklasse van `com.microsoft.windowsazure.notifications.NotificationsHandler` wordt.

    Deze code overschrijft de `OnReceive`-methode, zodat de handler de ontvangen meldingen rapporteert. De handler verzendt ook de pushmelding naar Android Notification Manager met de methode `sendNotification()`. De methode `sendNotification()` moet worden uitgevoerd wanneer de app niet actief is en een melding is ontvangen.

    ```java
    public class MyHandler extends NotificationsHandler {
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
            NotificationCompat.Builder mBuilder =
                    new NotificationCompat.Builder(ctx)
                            .setSmallIcon(R.mipmap.ic_launcher)
                            .setContentTitle("Notification Hub Demo")
                            .setStyle(new NotificationCompat.BigTextStyle()
                                    .bigText(msg))
                            .setSound(defaultSoundUri)
                            .setContentText(msg);

            mBuilder.setContentIntent(contentIntent);
            mNotificationManager.notify(NOTIFICATION_ID, mBuilder.build());
        }
    }
    ```

14. Klik in Android Studio in de menubalk op **Bouwen** > **Project opnieuw opbouwen** om ervoor te zorgen dat uw code geen fouten bevat.

15. Voer de app op uw apparaat en controleer of dat deze correct is geregistreerd in de notification hub.

    > [!NOTE]
    > Registratie kan mislukken bij de eerste keer starten totdat de `onTokenRefresh()`-methode van exemplaar-id-service wordt aangeroepen. De vernieuwing moet een succesvolle registratie met de Notification Hub tot stand brengen.

## <a name="test-the-app"></a>De app testen

### <a name="test-send-notification-from-the-notification-hub"></a>Testen van melding verzenden vanuit de Notification Hub

U kunt pushmeldingen verzenden vanuit de [Azure Portal] door de volgende acties uit te voeren:

1. Selecteer **Test verzenden** in de sectie **Probleemoplossing**.
2. Selecteer voor **Platforms** de optie **Android**.
3. Selecteer **Verzenden**.  U ziet nog geen melding op het Android-apparaat omdat u daarop niet de mobiele app hebt uitgevoerd. Nadat u de mobiele app hebt uitgevoerd, selecteert u opnieuw **Verzenden** om de melding weer te geven.
4. Bekijk het **resultaat** van de bewerking in de lijst onderaan.

    ![Azure Notification Hubs - Verzenden testen](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

### <a name="run-the-mobile-app"></a>De mobiele app uitvoeren

Als u pushmeldingen binnen een emulator wilt testen, moet u ervoor zorgen dat de installatiekopie van de emulator het Google API-niveau ondersteunt dat u voor uw app hebt gekozen. Als uw installatiekopie geen ondersteuning biedt voor native Google API’s, verschijnt de uitzondering **SERVICE\_NIET\_BESCHIKBAAR**.

Bovendien moet uw Google-account zijn toegevoegd aan de actieve emulator onder **Instellingen** > **Accounts**. Anders kunnen pogingen om opnieuw te registreren bij GCM leiden tot de uitzondering **VERIFICATIE\_MISLUKT**.

1. Start de app. U zult zien dat de registratie-id meldt dat de registratie is gelukt.

    ![Testen op Android - Kanaalregistratie](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)
2. Voer een melding in die moet worden verzonden naar alle Android-apparaten die zijn geregistreerd bij de hub.

    ![Testen op Android - een bericht verzenden](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)
3. Druk op **Melding verzenden**. Alle apparaten waarop de app actief is, ontvangen een `AlertDialog`-exemplaar bij de pushmelding. Apparaten waarop de app niet actief is, maar die eerder zijn geregistreerd voor pushmeldingen, ontvangen een melding in Android Notification Manager. De meldingen kunnen worden bekeken door omlaag te vegen vanuit de linkerbovenhoek.

    ![Testen op Android - meldingen](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u Firebase Cloud Messaging gebruikt om meldingen te pushen naar Android-apparaten. Als u wilt weten hoe u meldingen kunt pushen met behulp van Google Cloud Messaging, gaat u naar de volgende zelfstudie:

> [!div class="nextstepaction"]
>[Push notifications to Android devices using Google Cloud Messaging](notification-hubs-android-push-notification-google-gcm-get-started.md) (Meldingen pushen naar Android-apparaten met Google Cloud Messaging)

<!-- Images. -->

<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Notification Hubs Guidance]: notification-hubs-push-notification-overview.md
[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Use Notification Hubs to send breaking news]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure Portal]: https://portal.azure.com
