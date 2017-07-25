
1. Klik op **App Services** > selecteer de back-end van uw mobiele app > klik op **Quickstart** > uw clientplatform (iOS, Android, Xamarin, Cordova).

![De Azure-portal met Mobiele apps snel starten gemarkeerd][quickstart]

2. Als de verbinding met de database niet is geconfigureerd, moet u een gegevensverbinding maken.

![Azure Portal met Mobile Apps: verbinding maken met BD][connect]

  * Maak een nieuwe SQL-database en -server.

  ![Azure Portal met Mobile Apps: nieuwe BD en server maken][server]

  * Wacht totdat de gegevensverbinding tot stand is gebracht.

  ![Azure Portal met Mobile Apps: melding bij tot stand brengen van gegevensverbinding][notification]

  * Gegevensverbinding moet zijn gelukt.

  ![Azure Portal met Mobile Apps: melding bij tot stand brengen van gegevensverbinding][already-connection]

3. Selecteer bij **2. Een tabel-API maken** de optie Node.js voor **Back-endtaal**. Accepteer de bevestiging en klik op **Takentabel maken**. Er wordt dan een nieuwe *taken*tabel in uw database gemaakt. Houd er rekening mee dat als u een bestaande back-end overzet naar Node.js, alle inhoud wordt overschreven. Als u in plaats daarvan een .NET-back-end wilt maken, [volgt u deze instructies][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
