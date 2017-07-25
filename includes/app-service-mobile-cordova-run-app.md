
1. Ga naar [Azure Portal].
2. Klik op **App Services** > de back-end die u hebt gemaakt.
3. Klik in de instellingen van de mobiele app op **Snelstartgids** > **Cordova**.
![Azure Portal met Quick Start voor Mobile Apps gemarkeerd][quickstart]
4. Selecteer onder **Uw clienttoepassing configureren** de optie **Een nieuwe app maken** en klik vervolgens op **Downloaden**.
2. Pak het gedownloade zip-bestand uit in een map op uw harde schijf, navigeer naar het oplossingsbestand (.sln) en open dit met Visual Studio.
3. Kies in Visual Studio het oplossingsplatform (Android, iOS of Windows) in de vervolgkeuzelijst naast de begin-pijl. Selecteer een specifiek implementatieapparaat of een emulator door op de vervolgkeuzelijst op de groene pijl te klikken. U kunt het standaard-Android-platform en de standaard-Ripple-emulator gebruiken. Voor geavanceerdere zelfstudies (bijv. over pushmeldingen) moet u een ondersteund apparaat of ondersteunde emulator selecteren.
4. Druk op F5 of klik op de groene pijl om uw Cordova-app te bouwen en uit te voeren. Accepteer het beveiligingsdialoogvenster in de emulator waarin om toegang tot het netwerk wordt gevraagd, als dit wordt weergegeven.
5. Nadat de app is gestart op het apparaat of in de emulator typt u zinvolle tekst in via **Voer nieuwe tekst in**, zoals *Voltooi de zelfstudie* en u klikt dan op de knop **Toevoegen**.

De back-end voegt gegevens van de aanvraag toe aan de TodoItem-tabel in de SQL Database en stuurt informatie over de nieuw opgeslagen items terug naar de mobiele app. Deze gegevens worden in de lijst in de mobiele app weergegeven.

U kunt stap 3 t/m 5 herhalen voor andere platforms.

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png

<!-- URLs -->
[Azure Portal]: https://portal.azure.com/
