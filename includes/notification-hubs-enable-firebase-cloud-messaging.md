

1. Meld u aan bij de [Firebase-console](https://firebase.google.com/console/). Maak een nieuw Firebase-project als u er nog geen hebt.
2. Klik, nadat uw project is gemaakt, op **Add Firebase to your Android app** en volg de instructies.

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Klik in de Firebase-console op het tandwiel voor uw project en klik vervolgens op **Project Settings**.

    ![](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Klik op de **algemene** tabblad in de instellingen van uw project en downloadt u de **google services.json** -bestand met de Server-API-sleutel en de Client-ID.
5. Klik op de **Cloud Messaging** tabblad in de instellingen van uw project en kopieer de waarde van de **Legacy-serversleutel**. Deze waarde wordt gebruikt voor het configureren van het toegangsbeleid van de notification hub.
