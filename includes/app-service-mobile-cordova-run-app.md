
1. Ga naar de [Azure-portal]. Klik op **Door alles bladeren** > **Mobile Apps** > de back-end die u zojuist hebt gemaakt. Klik in de instellingen van de mobiele app op **Snelstartgids** > **Cordova**. Selecteer onder **Uw clienttoepassing configureren** de optie **Een nieuwe app maken** en klik vervolgens op **Downloaden**. Er wordt dan een volledig Cordova-project voor een app gedownload, dat vooraf is geconfigureerd om verbinding te maken met uw back-end.

2. Pak het gedownloade zip-bestand uit in een map op de harde schijf, navigeer naar het oplossingsbestand (.sln) en open dit met Visual Studio.

5. Kies in Visual Studio het oplossingsplatform (Android, iOS of Windows) in de vervolgkeuzelijst naast de begin-pijl en selecteer een specifiek implementatieapparaat of een emulator door op de vervolgkeuzelijst op de groene pijl te klikken. Houd er rekening mee dat u het standaard-Android-platform en de standaard-Ripple-emulator kunt gebruiken. Voor geavanceerdere zelfstudies moet u een ondersteund apparaat of ondersteunde emulator selecteren. 

6. Druk op F5 of klik op de groene pijl om uw Cordova-app te bouwen en uit te voeren. Accepteer het beveiligingsdialoogvenster in de emulator waarin om toegang tot het netwerk wordt gevraagd, als dit wordt weergegeven.   

7. Nadat de app is gestart op het apparaat of in de emulator, typt u betekenisvolle tekst in **Enter new text** (Voer nieuwe tekst in), zoals _Complete the tutorial_ (Voltooi de zelfstudie) en klik vervolgens op de knop **Add** (Toevoegen).  
Er wordt nu een POST-aanvraag verzonden naar de back-end van Azure die u eerder hebt geïmplementeerd. De back-end voegt gegevens van de aanvraag toe aan de TodoItem-tabel in de SQL-database en stuurt informatie over de nieuw opgeslagen items terug naar de mobiele app. Deze gegevens worden in de lijst in de mobiele app weergegeven.

    ![](./media/app-service-mobile-cordova-quickstart/quickstart-startup.png)
    
8. Herhaal de voorgaande drie stappen voor elk apparaatplatform dat u wilt ondersteunen.

[Azure-portal]: https://portal.azure.com/



<!--HONumber=sep16_HO1-->


