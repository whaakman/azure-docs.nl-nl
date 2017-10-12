---
title: Pushmeldingen verzenden naar Android met Azure Notification Hubs en Firebase Cloud Messaging | Microsoft Docs
description: In deze zelfstudie leert u hoe u met Azure Notification Hubs en Firebase Cloud Messaging pushmeldingen verzendt naar Android-apparaten.
services: notification-hubs
documentationcenter: android
keywords: pushmeldingen,pushmelding,android-pushmelding, firebase cloud messaging
author: ysxu
manager: erikre
editor: 
ms.assetid: 02298560-da61-4bbb-b07c-e79bd520e420
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: hero-article
ms.date: 07/14/2016
ms.author: yuaxu
ms.openlocfilehash: 45a3fa5c7190e039fd637c78a41eeb3f6ede9bc7
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="sending-push-notifications-to-android-with-azure-notification-hubs"></a>Pushmeldingen naar Android verzenden met Azure Notification Hubs
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Overzicht
> [!IMPORTANT]
> In dit onderwerp worden pushmeldingen met Google Firebase Cloud Messaging (FCM) getoond. Als u gebruikmaakt van Google Cloud Messaging (GCM), verwijzen wij u naar [Sending push notifications to Android with Azure Notification Hubs and GCM](notification-hubs-android-push-notification-google-gcm-get-started.md) (Pushmeldingen verzenden naar Android met Azure Notification Hubs en GCM).
> 
> 

In deze zelfstudie wordt gedemonstreerd hoe u met Azure Notification Hubs Firebase Cloud Messaging en pushmeldingen verzendt naar een Android-toepassing.
U maakt een lege Android-app die pushmeldingen ontvangt via Firebase Cloud Messaging (FCM).

[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

U kunt de voltooide code voor deze zelfstudie [hier](https://github.com/Azure/azure-notificationhubs-samples/tree/master/Android/GetStartedFirebase) downloaden op GitHub.

## <a name="prerequisites"></a>Vereisten
> [!IMPORTANT]
> U hebt een actief Azure-account nodig om deze zelfstudie te voltooien. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-android-get-started) voor meer informatie.
> 
> 

* Naast een actief Azure-account, zoals hierboven vermeld, hebt u voor deze zelfstudie nog de meest recente versie van [Android Studio](http://go.microsoft.com/fwlink/?LinkId=389797) nodig.
* Android 2.3 of hoger voor Firebase Cloud Messaging.
* Google Repository revisie 27 of hoger is vereist voor Firebase Cloud Messaging.
* Google Play-Services 9.0.2 of hoger voor Firebase Cloud Messaging.
* Het voltooien van deze zelfstudie is een vereiste voor alle andere Notification Hubs-zelfstudies voor Android-apps.

## <a name="create-a-new-android-studio-project"></a>Een nieuw Android Studio-project maken
1. Start een nieuw Android Studio-project in Android Studio.
   
       ![Android Studio - new project](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-new-project.png)
2. Kies de vormfactor voor **Telefoon en tablet** en de **minimale SDK** die u wilt ondersteunen. Klik op **Volgende**.
   
       ![Android Studio - project creation workflow](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-choose-form-factor.png)
3. Kies **Lege activiteit** als belangrijkste activiteit, klik op **Volgende** en klik vervolgens op **Voltooien**.

## <a name="create-a-project-that-supports-firebase-cloud-messaging"></a>Een project maken dat Firebase Cloud Messaging ondersteunt
[!INCLUDE [notification-hubs-enable-firebase-cloud-messaging](../../includes/notification-hubs-enable-firebase-cloud-messaging.md)]

## <a name="configure-a-new-notification-hub"></a>Een nieuwe Notification Hub configureren
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

&emsp;&emsp;6. Selecteer in de blade **Instellingen** van uw Notification Hub **Notification Services** en vervolgens **Google (GCM)**. Voer de FCM-serversleutel die u eerder hebt gekopieerd uit de [Firebase console](https://firebase.google.com/console/) in en klik op **Opslaan**.

&emsp;&emsp;![Azure Notification Hubs - Google (GCM)](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-gcm-api.png)

De Notification Hub is nu geconfigureerd voor Firebase Cloud Messaging en u hebt de verbindingsreeksen om uw app te registreren voor het ontvangen en verzenden van pushmeldingen.

## <a id="connecting-app"></a>Uw app verbinden met de Notification Hub
### <a name="add-google-play-services-to-the-project"></a>Google Play-services aan het project toevoegen
[!INCLUDE [Add Play Services](../../includes/notification-hubs-android-studio-add-google-play-services.md)]

### <a name="adding-azure-notification-hubs-libraries"></a>Azure Notification Hubs-bibliotheken toevoegen
1. Voeg in het bestand `Build.Gradle` voor de **app** de volgende regels toe in het gedeelte **afhankelijkheden**.
   
        compile 'com.microsoft.azure:notification-hubs-android-sdk:0.4@aar'
        compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@aar'
2. Voeg de volgende opslagplaats toe na het gedeelte **afhankelijkheden**.
   
        repositories {
            maven {
                url "http://dl.bintray.com/microsoftazuremobile/SDK"
            }
        }

### <a name="updating-the-androidmanifestxml"></a>De AndroidManifest.xml bijwerken
1. Als u FCM wilt ondersteunen, moet u een exemplaar-id listenerservice in de code implementeren die wordt gebruikt voor het [verkrijgen van registratietokens](https://firebase.google.com/docs/cloud-messaging/android/client#sample-register) met de [Google's FirebaseInstanceId API](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId). In deze zelfstudie noemen we deze klasse `MyInstanceIDService`. 
   
    Voeg de volgende servicedefinitie toe aan het bestand AndroidManifest.xml in de `<application>`-tag. 
   
        <service android:name=".MyInstanceIDService">
            <intent-filter>
                <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
            </intent-filter>
        </service>
2. Nadat we het FCM-registratietoken van de FirebaseInstanceId API hebben ontvangen, gebruiken we dit voor [registratie bij de Azure Notification Hub](notification-hubs-push-notification-registration-management.md). Deze registratie op de achtergrond wordt ondersteund met een `IntentService` met de naam `RegistrationIntentService`. Deze service ook is verantwoordelijk voor het vernieuwen van ons FCM-registratietoken.
   
    Voeg de volgende servicedefinitie toe aan het bestand AndroidManifest.xml in de `<application>`-tag. 
   
        <service
            android:name=".RegistrationIntentService"
            android:exported="false">
        </service>
3. Er wordt ook een ontvanger voor het ontvangen van meldingen gedefinieerd. Voeg de volgende ontvangerdefinitie toe aan het bestand AndroidManifest.xml in de `<application>`-tag. Vervang de tijdelijke aanduiding `<your package>` door de werkelijke pakketnaam die boven in het bestand `AndroidManifest.xml` wordt weergegeven.
   
        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
            android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="<your package name>" />
            </intent-filter>
        </receiver>
4. Voeg de volgende vereiste FCM gerelateerde machtigingen toe onder de `</application>`-tag. Vervang `<your package>` door de pakketnaam die boven in het bestand `AndroidManifest.xml` wordt weergegeven.
   
    Zie voor meer informatie over deze machtigingen [Een GCM Client App instellen voor Android](https://developers.google.com/cloud-messaging/android/client#manifest) en [Een GCM Client App voor Android migreren naar Firebase Cloud Messaging](https://developers.google.com/cloud-messaging/android/android-migrate-fcm#remove_the_permissions_required_by_gcm).
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />

### <a name="adding-code"></a>Code toevoegen
1. Vouw in de Project-weergave **app** > **src** > **main** > **java** uit. Klik met de rechtermuisknop op de pakketmap onder **java**, klik op **Nieuw** en klik vervolgens op **Java-klasse**. Voeg een nieuwe klasse met de naam `NotificationSettings` toe. 
   
    ![Android Studio - nieuwe Java-klasse](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hub-android-new-class.png)
   
    Zorg ervoor dat u de volgende drie tijdelijke aanduidingen in de volgende code bijwerkt voor de klasse `NotificationSettings`:
   
   * **SenderId**: De Verzenders-ID die u eerder hebt verkregen in het tabblad **Cloud Messaging** tab van uw projectinstellingen in de [Firebase console](https://firebase.google.com/console/).
   * **HubListenConnectionString**: de verbindingsreeks **DefaultListenAccessSignature** voor de hub. Kopieer deze verbindingsreeks door te klikken op **Toegangsbeleid** in de hubblade **Instellingen** in [Azure Portal].
   * **HubName**: gebruik de naam van uw Notification Hub die wordt weergegeven in de hubblade in [Azure Portal].
     
     `NotificationSettings`-code:
     
       public class NotificationSettings {
     
           public static String SenderId = "<Your project number>";
           public static String HubName = "<Your HubName>";
           public static String HubListenConnectionString = "<Enter your DefaultListenSharedAccessSignature connection string>";
       }
2. Voeg aan de hand van de bovenstaande stappen nog een klasse toe met de naam `MyInstanceIDService`. Dit is de implementatie van onze exemplaar-id listenerservice.
   
    De code voor deze klasse roept de `IntentService` aan voor het op de achtergrond [vernieuwen van het FCM-token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens).
   
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


1. Voeg een andere nieuwe klasse toe aan uw project met de naam `RegistrationIntentService`. Dit is de implementatie voor onze `IntentService` die zorgt voor [het vernieuwen van het FCM-token](https://developers.google.com/instance-id/guides/android-implementation#refresh_tokens) en [registratie bij de Notification Hub](notification-hubs-push-notification-registration-management.md).
   
    Gebruik de volgende code voor deze klasse.
   
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
   
                    // Storing the registration id that indicates whether the generated token has been
                    // sent to your server. If it is not stored, send the token to your server,
                    // otherwise your server should have already received the token.
                    if (((regID=sharedPreferences.getString("registrationID", null)) == null)){
   
                        NotificationHub hub = new NotificationHub(NotificationSettings.HubName,
                                NotificationSettings.HubListenConnectionString, this);
                        Log.d(TAG, "Attempting a new registration with NH using FCM token : " + FCM_token);
                        regID = hub.register(FCM_token).getRegistrationId();
   
                        // If you want to use tags...
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
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
                        // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
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
2. Voeg in uw klasse `MainActivity` de volgende instructies voor `import` toe boven de klassendeclaratie.
   
        import com.google.android.gms.common.ConnectionResult;
        import com.google.android.gms.common.GoogleApiAvailability;
        import com.microsoft.windowsazure.notifications.NotificationsManager;
        import android.content.Intent;
        import android.util.Log;
        import android.widget.TextView;
        import android.widget.Toast;
3. Voeg de volgende privé-leden toe aan de bovenkant van de klasse. We gebruiken deze [om de beschikbaarheid van Google Play Services te controleren, zoals aanbevolen door Google](https://developers.google.com/android/guides/setup#ensure_devices_have_the_google_play_services_apk).
   
        public static MainActivity mainActivity;
        public static Boolean isVisible = false;    
        private static final String TAG = "MainActivity";
        private static final int PLAY_SERVICES_RESOLUTION_REQUEST = 9000;
4. Voeg in uw klasse `MainActivity` de volgende methode toe om de beschikbaarheid van Google Play Services te controleren. 
   
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
5. Voeg in uw klasse `MainActivity` de volgende code toe waarmee Google Play Services wordt gecontroleerd voordat u uw `IntentService` aanroept om uw FCM-registratietoken op te halen en te registreren met de Notification Hub.
   
        public void registerWithNotificationHubs()
        {
            if (checkPlayServices()) {
                // Start IntentService to register this application with FCM.
                Intent intent = new Intent(this, RegistrationIntentService.class);
                startService(intent);
            }
        }
6. In de methode `OnCreate` van de klasse `MainActivity` voegt u de volgende code toe om het registratieproces te starten wanneer de activiteit wordt gemaakt.
   
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            super.onCreate(savedInstanceState);
            setContentView(R.layout.activity_main);
   
            mainActivity = this;
            NotificationsManager.handleNotifications(this, NotificationSettings.SenderId, MyHandler.class);
            registerWithNotificationHubs();
        }
7. Voeg deze extra methoden toe aan de `MainActivity` om de status van de app te controleren en een rapport van de status in uw app op te nemen.
   
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
8. Voor de methode `ToastNotify` wordt het besturingselement *Hello World* gebruikt `TextView` om de status en kennisgevingen permanent in de app te melden. In de indeling activity_main.xml voegt u de volgende id toe voor het besturingselement.
   
       android:id="@+id/text_hello"
9. Nu voegt u een subklasse toe voor de ontvanger die is gedefinieerd in AndroidManifest.xml. Voeg een andere nieuwe klasse toe aan uw project met de naam `MyHandler`.
10. Voeg boven in `MyHandler.java` de volgende importinstructie toe:
    
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.media.RingtoneManager;
        import android.net.Uri;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
        import com.microsoft.windowsazure.notifications.NotificationsHandler;
11. Voeg de volgende code toe voor de klasse `MyHandler`, zodat dit een subklasse van `com.microsoft.windowsazure.notifications.NotificationsHandler` wordt.
    
    Deze code overschrijft de methode `OnReceive`, zodat de handler de ontvangen meldingen rapporteert. De handler verzendt ook de pushmelding naar Android Notification Manager met de methode `sendNotification()`. De methode `sendNotification()` moet worden uitgevoerd wanneer de app niet actief is en een melding is ontvangen.
    
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
12. Klik in Android Studio in de menubalk op **Bouwen** > **Project opnieuw opbouwen** om ervoor te zorgen dat uw code geen fouten bevat.
13. Voer de app op uw apparaat en controleer of dat deze correct is geregistreerd in de notification hub. 
    
    > [!NOTE]
    > Registratie kan mislukken bij de eerste introductie tot de `onTokenRefresh()` methode van exemplaar-id-service wordt aangeroepen. De vernieuwing moet een succesvolle registratie met de notification hub tot stand brengen.
    > 
    > 

## <a name="sending-push-notifications"></a>Pushmeldingen verzenden
U kunt testen of u pushmeldingen in uw app ontvangt door deze meldingen via [Azure Portal] te verzenden. Ga naar het gedeelte **Probleemoplossing** in de hubblade, zoals hieronder weergegeven.

![Azure Notification Hubs - Verzenden testen](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-test-send.png)

[!INCLUDE [notification-hubs-sending-notifications-from-the-portal](../../includes/notification-hubs-sending-notifications-from-the-portal.md)]

## <a name="optional-send-push-notifications-directly-from-the-app"></a>(Optioneel) Pushmeldingen rechtstreeks vanuit de app verzenden
> [!IMPORTANT]
> Dit voorbeeld van het verzenden van meldingen vanuit de client-app wordt uitsluitend voor educatieve doeleinden aangeboden. Aangezien het hiervoor noodzakelijk is dat de `DefaultFullSharedAccessSignature` aanwezig is op de client-app, loopt uw Notification Hub het risico dat een gebruiker toegang kan krijgen en onbevoegd meldingen naar uw clients kan verzenden.
> 
> 

Normaal gesproken verzendt u meldingen via een back-endserver. Mogelijk wilt u pushmeldingen direct vanuit de clienttoepassing kunnen verzenden. In dit gedeelte wordt uitgelegd hoe u meldingen vanuit de client verzendt met de [Azure Notification Hub REST API](https://msdn.microsoft.com/library/azure/dn223264.aspx).

1. Vouw in de Project-weergave van Android Studio-Project **App** > **src** > **main** > **res** > **layout** uit. Open het indelingsbestand `activity_main.xml` en klik op het tabblad **Tekst** om de tekst van het bestand bij te werken. Werk de tekst bij met de onderstaande code, waarmee u nieuwe besturingselementen `Button` en `EditText` voor het verzenden van pushmeldingen toevoegt aan de Notification Hub. Voeg deze code onderin toe, net voor `</RelativeLayout>`.
   
        <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/send_button"
        android:id="@+id/sendbutton"
        android:layout_centerVertical="true"
        android:layout_centerHorizontal="true"
        android:onClick="sendNotificationButtonOnClick" />
   
        <EditText
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:id="@+id/editTextNotificationMessage"
        android:layout_above="@+id/sendbutton"
        android:layout_centerHorizontal="true"
        android:layout_marginBottom="42dp"
        android:hint="@string/notification_message_hint" />
2. Vouw in de Project-weergave van Android Studio-Project **App** > **src** > **main** > **res** > **values** uit. Open het bestand `strings.xml` en voeg de tekenreekswaarden toe waarnaar wordt verwezen door de nieuwe besturingselementen `Button` en `EditText`. Voeg deze onder in het bestand toe, net voor `</resources>`.
   
        <string name="send_button">Send Notification</string>
        <string name="notification_message_hint">Enter notification message text</string>
3. Voeg in het bestand `NotificationSetting.java` de volgende instelling toe voor de klasse `NotificationSettings`.
   
    Werk `HubFullAccess` bij met de verbindingsreeks **DefaultFullSharedAccessSignature** voor uw hub. U kunt deze verbindingsreeks kopiëren vanuit [Azure Portal] door te klikken op **Toegangsbeleid** op de blade **Instellingen** voor uw Notification Hub.
   
        public static String HubFullAccess = "<Enter Your DefaultFullSharedAccessSignature Connection string>";
4. Voeg in uw bestand `MainActivity.java` de volgende instructies voor `import` toe boven de klasse `MainActivity`.
   
        import java.io.BufferedOutputStream;
        import java.io.BufferedReader;
        import java.io.InputStreamReader;
        import java.io.OutputStream;
        import java.net.HttpURLConnection;
        import java.net.URL;
        import java.net.URLEncoder;
        import javax.crypto.Mac;
        import javax.crypto.spec.SecretKeySpec;
        import android.util.Base64;
        import android.view.View;
        import android.widget.EditText;
5. Voeg in uw bestand `MainActivity.java` de volgende leden toe boven de klasse `MainActivity`.    
   
        private String HubEndpoint = null;
        private String HubSasKeyName = null;
        private String HubSasKeyValue = null;
6. U moet een Software Access Signature (SaS)-token maken om een POST-aanvraag te verifiëren om berichten naar uw Notification Hub te kunnen verzenden. U doet dit door de belangrijkste gegevens uit de verbindingsreeks te parseren en vervolgens het SaS-token te maken, zoals vermeld in [Algemene concepten](http://msdn.microsoft.com/library/azure/dn495627.aspx) REST API-verwijzing. De volgende code is een voorbeeldimplementatie.
   
    Voeg in `MainActivity.java` de volgende methode toe aan de klasse `MainActivity` om uw verbindingsreeks te parseren.
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx
         * to parse the connection string so a SaS authentication token can be
         * constructed.
         *
         * @param connectionString This must be the DefaultFullSharedAccess connection
         *                         string for this example.
         */
        private void ParseConnectionString(String connectionString)
        {
            String[] parts = connectionString.split(";");
            if (parts.length != 3)
                throw new RuntimeException("Error parsing connection string: "
                        + connectionString);
   
            for (int i = 0; i < parts.length; i++) {
                if (parts[i].startsWith("Endpoint")) {
                    this.HubEndpoint = "https" + parts[i].substring(11);
                } else if (parts[i].startsWith("SharedAccessKeyName")) {
                    this.HubSasKeyName = parts[i].substring(20);
                } else if (parts[i].startsWith("SharedAccessKey")) {
                    this.HubSasKeyValue = parts[i].substring(16);
                }
            }
        }
7. Voeg in `MainActivity.java` de volgende methode toe aan de klasse `MainActivity` om een SAS-verificatietoken te maken.
   
        /**
         * Example code from http://msdn.microsoft.com/library/azure/dn495627.aspx to
         * construct a SaS token from the access key to authenticate a request.
         *
         * @param uri The unencoded resource URI string for this operation. The resource
         *            URI is the full URI of the Service Bus resource to which access is
         *            claimed. For example,
         *            "http://<namespace>.servicebus.windows.net/<hubName>"
         */
        private String generateSasToken(String uri) {
   
            String targetUri;
            String token = null;
            try {
                targetUri = URLEncoder
                        .encode(uri.toString().toLowerCase(), "UTF-8")
                        .toLowerCase();
   
                long expiresOnDate = System.currentTimeMillis();
                int expiresInMins = 60; // 1 hour
                expiresOnDate += expiresInMins * 60 * 1000;
                long expires = expiresOnDate / 1000;
                String toSign = targetUri + "\n" + expires;
   
                // Get an hmac_sha1 key from the raw key bytes
                byte[] keyBytes = HubSasKeyValue.getBytes("UTF-8");
                SecretKeySpec signingKey = new SecretKeySpec(keyBytes, "HmacSHA256");
   
                // Get an hmac_sha1 Mac instance and initialize with the signing key
                Mac mac = Mac.getInstance("HmacSHA256");
                mac.init(signingKey);
   
                // Compute the hmac on input data bytes
                byte[] rawHmac = mac.doFinal(toSign.getBytes("UTF-8"));
   
                // Using android.util.Base64 for Android Studio instead of
                // Apache commons codec
                String signature = URLEncoder.encode(
                        Base64.encodeToString(rawHmac, Base64.NO_WRAP).toString(), "UTF-8");
   
                // Construct authorization string
                token = "SharedAccessSignature sr=" + targetUri + "&sig="
                        + signature + "&se=" + expires + "&skn=" + HubSasKeyName;
            } catch (Exception e) {
                if (isVisible) {
                    ToastNotify("Exception Generating SaS : " + e.getMessage().toString());
                }
            }
   
            return token;
        }
8. Voeg in `MainActivity.java` de volgende methode toe aan de klasse `MainActivity` om het klikken op de knop **Melding verzenden** te activeren en de pushmelding naar de hub te verzenden met de ingebouwde REST-API.
   
        /**
         * Send Notification button click handler. This method parses the
         * DefaultFullSharedAccess connection string and generates a SaS token. The
         * token is added to the Authorization header on the POST request to the
         * notification hub. The text in the editTextNotificationMessage control
         * is added as the JSON body for the request to add a GCM message to the hub.
         *
         * @param v
         */
        public void sendNotificationButtonOnClick(View v) {
            EditText notificationText = (EditText) findViewById(R.id.editTextNotificationMessage);
            final String json = "{\"data\":{\"message\":\"" + notificationText.getText().toString() + "\"}}";
   
            new Thread()
            {
                public void run()
                {
                    try
                    {
                        // Based on reference documentation...
                        // http://msdn.microsoft.com/library/azure/dn223273.aspx
                        ParseConnectionString(NotificationSettings.HubFullAccess);
                        URL url = new URL(HubEndpoint + NotificationSettings.HubName +
                                "/messages/?api-version=2015-01");
   
                        HttpURLConnection urlConnection = (HttpURLConnection)url.openConnection();
   
                        try {
                            // POST request
                            urlConnection.setDoOutput(true);
   
                            // Authenticate the POST request with the SaS token
                            urlConnection.setRequestProperty("Authorization", 
                                generateSasToken(url.toString()));
   
                            // Notification format should be GCM
                            urlConnection.setRequestProperty("ServiceBusNotification-Format", "gcm");
   
                            // Include any tags
                            // Example below targets 3 specific tags
                            // Refer to : https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-routing-tag-expressions/
                            // urlConnection.setRequestProperty("ServiceBusNotification-Tags", 
                            //        "tag1 || tag2 || tag3");
   
                            // Send notification message
                            urlConnection.setFixedLengthStreamingMode(json.length());
                            OutputStream bodyStream = new BufferedOutputStream(urlConnection.getOutputStream());
                            bodyStream.write(json.getBytes());
                            bodyStream.close();
   
                            // Get reponse
                            urlConnection.connect();
                            int responseCode = urlConnection.getResponseCode();
                            if ((responseCode != 200) && (responseCode != 201)) {
                                BufferedReader br = new BufferedReader(new InputStreamReader((urlConnection.getErrorStream())));
                                String line;
                                StringBuilder builder = new StringBuilder("Send Notification returned " +
                                        responseCode + " : ")  ;
                                while ((line = br.readLine()) != null) {
                                    builder.append(line);
                                }
   
                                ToastNotify(builder.toString());
                            }
                        } finally {
                            urlConnection.disconnect();
                        }
                    }
                    catch(Exception e)
                    {
                        if (isVisible) {
                            ToastNotify("Exception Sending Notification : " + e.getMessage().toString());
                        }
                    }
                }
            }.start();
        }

## <a name="testing-your-app"></a>Uw app testen
#### <a name="push-notifications-in-the-emulator"></a>Pushmeldingen in de emulator
Als u pushmeldingen binnen een emulator wilt testen, moet u ervoor zorgen dat de installatiekopie van de emulator het Google API-niveau ondersteunt dat u voor uw app hebt gekozen. Als uw installatiekopie geen ondersteuning biedt voor native Google API’s, verschijnt de uitzondering **SERVICE\_NIET\_BESCHIKBAAR**.

Bovendien moet u ervoor zorgen dat u uw Google-account hebt toegevoegd aan uw actieve emulator onder **Instellingen** > **Accounts**. Anders kunnen pogingen om opnieuw te registreren bij GCM leiden tot de uitzondering **VERIFICATIE\_MISLUKT**.

#### <a name="running-the-application"></a>De toepassing uitvoeren
1. Start de app. U zult zien dat de registratie-id meldt dat de registratie is gelukt.
   
       ![Testing on Android - Channel registration](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-registered.png)
2. Voer een melding in die moet worden verzonden naar alle Android-apparaten die zijn geregistreerd bij de hub.
   
       ![Testing on Android - sending a message](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-set-message.png)
3. Druk op **Melding verzenden**. Alle apparaten waarop de app actief is, ontvangen een `AlertDialog`-exemplaar met de pushmelding. Apparaten waarop de app niet actief is, maar die eerder zijn geregistreerd voor pushmeldingen, ontvangen een melding in Android Notification Manager. Deze meldingen kunnen worden bekeken door omlaag te vegen vanuit de linkerbovenhoek.
   
       ![Testing on Android - notifications](./media/notification-hubs-android-push-notification-google-fcm-get-started/notification-hubs-android-studio-received-message.png)

## <a name="next-steps"></a>Volgende stappen
U kunt het beste de zelfstudie [Notification Hubs gebruiken om pushmeldingen naar gebruikers te verzenden] doornemen. Hierin ziet u hoe u meldingen van een ASP.NET-back-end verzendt met tags voor specifieke gebruikers.

Zie [Notification Hubs gebruiken om belangrijk nieuws te verzenden] als u gebruikers wilt indelen op belangengroep.

Zie [Richtlijnen voor Notification Hubs] voor meer algemene informatie over Notification Hubs.

<!-- Images. -->



<!-- URLs. -->
[Get started with push notifications in Mobile Services]: ../mobile-services-javascript-backend-android-get-started-push.md  
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Referencing a library project]: http://go.microsoft.com/fwlink/?LinkId=389800
[Azure Classic Portal]: https://manage.windowsazure.com/
[Richtlijnen voor Notification Hubs]: notification-hubs-push-notification-overview.md
[Notification Hubs gebruiken om pushmeldingen naar gebruikers te verzenden]: notification-hubs-aspnet-backend-gcm-android-push-to-user-google-notification.md
[Notification Hubs gebruiken om belangrijk nieuws te verzenden]: notification-hubs-aspnet-backend-android-xplat-segmented-gcm-push-notification.md
[Azure Portal]: https://portal.azure.com
