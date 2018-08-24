---
author: conceptdev
ms.author: crdun
ms.service: app-service-mobile
ms.topic: include
ms.date: 08/23/2018
ms.openlocfilehash: 654bc3745768fccea41d7c3991142bf7183b54be
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42811545"
---
1. In uw **app** project, open het bestand `AndroidManifest.xml`. Voeg de volgende code na de `application` openingstag:

    ```xml
    <service android:name=".ToDoMessagingService">
        <intent-filter>
            <action android:name="com.google.firebase.MESSAGING_EVENT"/>
        </intent-filter>
    </service>
    <service android:name=".ToDoInstanceIdService">
        <intent-filter>
            <action android:name="com.google.firebase.INSTANCE_ID_EVENT"/>
        </intent-filter>
    </service>
    ```

2. Open het bestand `ToDoActivity.java`, en breng de volgende wijzigingen:

    - Voeg de importinstructie toe:

        ```java
        import com.google.firebase.iid.FirebaseInstanceId;
        ```

    - De definitie van `MobileServiceClient` van **persoonlijke** naar **persoonlijke statische**, zodat er nu als volgt uitziet:

        ```java
        private static MobileServiceClient mClient;
        ```

    - Voeg `registerPush` methode:

        ```java
        public static void registerPush() {
            final String token = FirebaseInstanceId.getInstance().getToken();
            if (token != null) {
                new AsyncTask<Void, Void, Void>() {
                    protected Void doInBackground(Void... params) {
                        mClient.getPush().register(token);
                        return null;
                    }
                }.execute();
            }
        }
        ```

    - Update de **onCreate** -methode van de `ToDoActivity` klasse. Zorg ervoor dat u deze Voeg code toe na de `MobileServiceClient` wordt geïnstantieerd.

        ```java
        registerPush();
        ```

3. Voeg een nieuwe klasse voor het afhandelen van meldingen. Open in Projectverkenner het **app** > **java** > **uw project-naamruimte** knooppunten, en met de rechtermuisknop op het knooppunt van de naam van pakket. Klik op **nieuw**, en klik vervolgens op **Java-klasse**. Typ in het vak naam `ToDoMessagingService`, en klik vervolgens op OK. Vervang daarna de klassendeclaratie met:

    ```java
    import android.app.Notification;
    import android.app.NotificationManager;
    import android.app.PendingIntent;
    import android.content.Context;
    import android.content.Intent;

    import com.google.firebase.messaging.FirebaseMessagingService;
    import com.google.firebase.messaging.RemoteMessage;

    public class ToDoMessagingService extends FirebaseMessagingService {

        private static final int NOTIFICATION_ID = 1;

        @Override
        public void onMessageReceived(RemoteMessage remoteMessage) {
            String message = remoteMessage.getData().get("message");
            if (message != null) {
                sendNotification("Notification Hub Demo", message);
            }
        }

        private void sendNotification(String title, String messageBody) {
            PendingIntent contentIntent = PendingIntent.getActivity(this, 0, new Intent(this, ToDoActivity.class), 0);
            Notification.Builder notificationBuilder = new Notification.Builder(this)
                    .setSmallIcon(R.drawable.ic_launcher)
                    .setContentTitle(title)
                    .setContentText(messageBody)
                    .setContentIntent(contentIntent);
            NotificationManager notificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
            if (notificationManager != null) {
                notificationManager.notify(NOTIFICATION_ID, notificationBuilder.build());
            }
        }
    }
    ```

4. Voeg een andere klasse voor het afhandelen van token-updates. Maak `ToDoInstanceIdService` java klasse en vervang de klassendeclaratie met:

    ```java
    import com.google.firebase.iid.FirebaseInstanceIdService;

    public class ToDoInstanceIdService extends FirebaseInstanceIdService {

        @Override
        public void onTokenRefresh() {
            ToDoActivity.registerPush();
        }
    }
    ```

Uw app wordt nu bijgewerkt ter ondersteuning van pushmeldingen.
