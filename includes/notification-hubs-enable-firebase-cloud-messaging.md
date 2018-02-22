

1. Meld u aan bij de [Firebase-console](https://firebase.google.com/console/). Maak een nieuw Firebase-project als u er nog geen hebt.
2. Nadat u uw project maakt, selecteert u **Firebase toevoegen aan uw Android-app**. Volg de instructies die worden geleverd.

    ![Firebase toevoegen aan uw Android-app](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Selecteer in de console Firebase het tandwiel voor uw project. Selecteer vervolgens **projectinstellingen**.

    ![Selecteer projectinstellingen](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Selecteer de **algemene** tabblad in de instellingen van uw project. Download de **google services.json** -bestand met de Server-API-sleutel en de Client-ID.
5. Selecteer de **Cloud Messaging** tabblad in de instellingen van uw project en kopieer de waarde van de **Legacy-serversleutel**. Deze waarde kunt u het toegangsbeleid van de notification hub configureren.
