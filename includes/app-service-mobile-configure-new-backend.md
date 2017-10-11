
1. Klik op de **App Services** knop, selecteert u uw back-end van Mobile Apps, selecteer **Quick Start**, en selecteer vervolgens uw clientplatform (iOS, Android, Xamarin, Cordova).

    ![Azure-portal met Mobile Apps Quick Start gemarkeerd][quickstart]

2. Als een databaseverbinding niet is geconfigureerd, maakt u een door de volgende te doen:

    ![Azure-portal met Mobile Apps verbinding maken met database][connect]

    a. Maak een nieuwe SQL-database en de server.

    ![Azure-portal met Mobile Apps nieuwe database en de server maken][server]

    b. Wacht totdat de gegevensverbinding tot stand is gebracht.

    ![Azure portal gemaakt gegevensverbinding-melding][notification]

    c. Gegevensverbinding moet zijn gelukt.

    ![Azure portal melding 'U hebt al een gegevensverbinding'][already-connection]

3. Selecteer bij **2. Een tabel-API maken** de optie Node.js voor **Back-endtaal**. 
 
4. Accepteer de bevestiging en selecteer vervolgens **takentabel maken**.  
    Deze actie wordt een nieuwe taak item tabel gemaakt in uw database. 

    >[!IMPORTANT]
    > Alle inhoud overschakelen van een bestaande back-end voor Node.js worden overschreven. Als u wilt maken in plaats daarvan een .NET-back-end, Zie [werken met de .NET-back-end-server SDK voor Mobile Apps][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
