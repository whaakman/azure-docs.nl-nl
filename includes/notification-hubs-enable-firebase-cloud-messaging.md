

1. Meld u aan bij de [Firebase-console](https://firebase.google.com/console/). Maak een nieuw Firebase-project als u er nog geen hebt.
2. Nadat u uw project hebt gemaakt, selecteert u **Firebase toevoegen aan uw Android-app**. Volg de instructies.

    ![Firebase toevoegen aan uw Android-app](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-add-firebase-to-android-app.png)
3. Selecteer in de Firebase-console het tandwiel van uw project. Selecteer vervolgens **Projectinstellingen**.

    ![Selecteer Projectinstellingen](./media/notification-hubs-enable-firebase-cloud-messaging/notification-hubs-firebase-console-project-settings.png)
4. Selecteer in uw Projectinstellingen het tabblad **Algemeen**. Download vervolgens het bestand **google-services.json** dat de Server-API-sleutel en de Client-ID bevat.
5. Selecteer het tabblad **Cloudberichten** in uw projectinstellingen en kopieer vervolgens de waarde van de **oude serversleutel**. U gebruikt deze waarde om het toegangsbeleid voor de notification hub te configureren.
