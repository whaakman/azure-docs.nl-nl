1. In uw **app** project, open het bestand `AndroidManifest.xml`. Vervang in de code in de volgende twee stappen  *`**my_app_package**`*  met de naam van het app-pakket voor uw project. Dit is de waarde van de `package` kenmerk van de `manifest` label.
2. De volgende nieuwe machtigingen toevoegen na de bestaande `uses-permission` element:

        <permission android:name="**my_app_package**.permission.C2D_MESSAGE"
            android:protectionLevel="signature" />
        <uses-permission android:name="**my_app_package**.permission.C2D_MESSAGE" />
        <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
        <uses-permission android:name="android.permission.GET_ACCOUNTS" />
        <uses-permission android:name="android.permission.WAKE_LOCK" />
3. Voeg de volgende code na de `application` bij begincode:

        <receiver android:name="com.microsoft.windowsazure.notifications.NotificationsBroadcastReceiver"
                                         android:permission="com.google.android.c2dm.permission.SEND">
            <intent-filter>
                <action android:name="com.google.android.c2dm.intent.RECEIVE" />
                <category android:name="**my_app_package**" />
            </intent-filter>
        </receiver>
4. Open het bestand *ToDoActivity.java*, en voeg de volgende importinstructie toe:

        import com.microsoft.windowsazure.notifications.NotificationsManager;
5. De volgende persoonlijke variabele toevoegen aan de klasse. Vervang  *`<PROJECT_NUMBER>`*  met het projectnummer dat wordt toegewezen door Google aan uw app in de vorige procedure.

        public static final String SENDER_ID = "<PROJECT_NUMBER>";
6. Wijzig de definitie van *MobileServiceClient* van **persoonlijke** naar **openbare statische**, zodat er nu als volgt uitziet:

        public static MobileServiceClient mClient;
7. Voeg een nieuwe klasse voor het afhandelen van meldingen. Open in Projectverkenner de **src** > **belangrijkste** > **java** knooppunten en met de rechtermuisknop op het knooppunt van de naam van pakket. Klik op **nieuw**, en klik vervolgens op **Java-klasse**.
8. In **naam**, type `MyHandler`, en klik vervolgens op **OK**.

    ![](./media/app-service-mobile-android-configure-push/android-studio-create-class.png)

9. Vervang in het bestand MyHandler de klassendeclaratie met:

        public class MyHandler extends NotificationsHandler {
10. Voeg de volgende importinstructies voor de `MyHandler` klasse:

        import com.microsoft.windowsazure.notifications.NotificationsHandler;
        import android.app.NotificationManager;
        import android.app.PendingIntent;
        import android.content.Context;
        import android.content.Intent;
        import android.os.AsyncTask;
        import android.os.Bundle;
        import android.support.v4.app.NotificationCompat;
11. Naast deze lid toevoegen aan de `MyHandler` klasse:

        public static final int NOTIFICATION_ID = 1;
12. In de `MyHandler` klasse, voeg de volgende code voor het onderdrukken van de **onRegistered** methode, waarmee u uw apparaat bij de mobiele service notification hub registreert.

        @Override
        public void onRegistered(Context context,  final String gcmRegistrationId) {
           super.onRegistered(context, gcmRegistrationId);

           new AsyncTask<Void, Void, Void>() {

               protected Void doInBackground(Void... params) {
                   try {
                       ToDoActivity.mClient.getPush().register(gcmRegistrationId);
                       return null;
                   }
                   catch(Exception e) {
                       // handle error                
                   }
                   return null;              
               }
           }.execute();
       }
13. In de `MyHandler` klasse, voeg de volgende code voor het onderdrukken van de **onReceive** methode, waardoor de melding moet worden weergegeven als het is ontvangen.

        @Override
        public void onReceive(Context context, Bundle bundle) {
               String msg = bundle.getString("message");

               PendingIntent contentIntent = PendingIntent.getActivity(context,
                       0, // requestCode
                       new Intent(context, ToDoActivity.class),
                       0); // flags

               Notification notification = new NotificationCompat.Builder(context)
                       .setSmallIcon(R.drawable.ic_launcher)
                       .setContentTitle("Notification Hub Demo")
                       .setStyle(new NotificationCompat.BigTextStyle().bigText(msg))
                       .setContentText(msg)
                       .setContentIntent(contentIntent)
                       .build();

               NotificationManager notificationManager = (NotificationManager)
                       context.getSystemService(Context.NOTIFICATION_SERVICE);
               notificationManager.notify(NOTIFICATION_ID, notification);
       }
14. Terug in het bestand TodoActivity.java bijwerken de **onCreate** methode van de *ToDoActivity* klasse melding handler klasse registreren. Zorg ervoor dat u deze Voeg code toe na de *MobileServiceClient* wordt geïnstantieerd.

        NotificationsManager.handleNotifications(this, SENDER_ID, MyHandler.class);

    Uw app is nu bijgewerkt ter ondersteuning van pushmeldingen.
