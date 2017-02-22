

Als u de app wilt registreren voor pushmeldingen via APNS (Apple Push Notification Service), moet u een nieuw pushcertificaat, een nieuwe Apple-id en een nieuw inrichtingsprofiel voor het project maken in de ontwikkelaarsportal van Apple. De App-id bevat de configuratie-instellingen die ervoor zorgen dat via de app pushmeldingen kunnen worden verzonden en ontvangen. Deze instellingen bevatten het certificaat voor pushmeldingen dat nodig is om te verifiëren met APNS (Apple Push Notification Service)tijdens het verzenden en ontvangen van pushmeldingen. Zie de officiële documentatie van de [Apple Push Notification Service](http://go.microsoft.com/fwlink/p/?LinkId=272584) voor meer informatie over deze concepten.

#### <a name="generate-the-certificate-signing-request-file-for-the-push-certificate"></a>Het bestand met de aanvraag voor certificaatondertekening genereren voor het pushcertificaat
Deze stappen helpen u bij het maken van de aanvraag voor certificaatondertekening. Dit wordt gebruikt om een pushcertificaat te genereren dat moet worden gebruikt met APNS.

1. Voer op uw Mac het hulpprogramma Sleutelhangertoegang uit. Dit kan worden gestart vanuit de map **Hulpprogramma's** of de map **Overige** in Launchpad.
2. Klik op **Toegang tot sleutelhanger**, vouw **Certificaatassistent** uit, klik vervolgens op **Een certificaat bij een certificaatautoriteit aanvragen...**.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-request-cert-from-ca.png)
3. Selecteer uw **gebruikers-e-mailadres** en **algemene naam**, controleer of **Opgeslagen op schijf** is geselecteerd en klik vervolgens op **Doorgaan**. Laat het veld **E-mailadres CA** leeg omdat dit niet is vereist.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-csr-info.png)
4. Typ een naam voor het bestand met de aanvraag voor certificaatondertekening in **Opslaan als**, selecteer de locatie in **Waar**, klik vervolgens op **Opslaan**.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-save-csr.png)
   
      Het bestand met de aanvraag voor certificaatondertekening wordt opgeslagen op de geselecteerde locatie; de standaardlocatie is op het Bureaublad. Onthoud de locatie die u voor dit bestand hebt gekozen.

#### <a name="register-your-app-for-push-notifications"></a>Uw app voor pushmeldingen registreren
Maak een nieuwe expliciete App-id voor de Apple-toepassing en configureer deze ook voor pushmeldingen.  

1. Navigeer naar de [iOS-inrichtingsportal](http://go.microsoft.com/fwlink/p/?LinkId=272456), meld u met uw Apple-id aan bij het Apple-ontwikkelaarscentrum en klik op **Id's**. Klik vervolgens op **App-id's** en ten slotte op het **+**-teken om een nieuwe app te registreren.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-ios-appids.png)
2. Werk de volgende drie velden voor uw nieuwe app bij en klik vervolgens op **Doorgaan**:
   
   * **Naam**: typ een beschrijvende naam voor uw app in het veld **Naam** in de sectie **Beschrijving app-id**.
   * **Bundel-id**: onder de sectie **Expliciete app-id** typt u een **bundel-id** in het formulier `<Organization Identifier>.<Product Name>` zoals vermeld in de [App Distribution Guide](https://developer.apple.com/library/mac/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringYourApp/ConfiguringYourApp.html#//apple_ref/doc/uid/TP40012582-CH28-SW8) (Gids voor apps-distributie). Dit moet overeenkomen met wat ook wordt gebruikt in het project XCode, Xamarin of Cordova voor de app.
   * ** Pushmeldingen**: selecteer de optie **Pushmeldingen** in de sectie **App Services**.
     
     ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-appid-info.png)
3. Controleer de instelling op het scherm Uw app-id bevestigen en klik hierna op **Verzenden**
4. Nadat u de nieuwe app-id hebt verzonden, ziet u het scherm **Registratie voltooid**. Klik op **Gereed**.
5. Ga in het Developer Center onder de app-id’s naar de app-id die u zojuist hebt gemaakt en klik op de rij van die id. Door op de rij voor de app-id te klikken, worden de gegevens van de app weergegeven. Klik op de knop **Bewerken** onderaan.
6. Ga naar de onderkant van het scherm en klik op de knop **Certificaat maken** onder de sectie **Ontwikkeling push-SSL-certificaat**.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-create-cert.png)
   
       This will display the "Add iOS Certificate" assistant.
   
   > [!NOTE]
   > In deze zelfstudie wordt een ontwikkelingscertificaat gebruikt. Hetzelfde proces wordt gebruikt bij het registreren van een productiecertificaat. Zorg er wel voor dat u hetzelfde certificaattype gebruikt als u meldingen verzendt.
   > 
   > 
7. Klik op **Bestand kiezen** en ga naar de locatie waar de CSR voor het pushcertificaat is opgeslagen. Klik vervolgens op **Genereren**.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-cert-choose-csr.png)
8. Nadat het certificaat is gemaakt via de portal, klikt u op de knop **Downloaden**.
   
      ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-appid-download-cert.png)
   
       This downloads the signing certificate and saves it to your computer in your Downloads folder.
   
      ![](./media/notification-hubs-enable-apple-push-notifications/notification-hubs-cert-downloaded.png)
   
   > [!NOTE]
   > Standaard krijgt het gedownloade bestand, een ontwikkelingscertificaat, de naam **aps_development.cer**.
   > 
   > 
9. Dubbelklik op het gedownloade pushcertificaat **aps_development.cer**. Nu wordt het nieuwe certificaat in de sleutelhanger geïnstalleerd, zoals hieronder wordt weergegeven:
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-cert-in-keychain.png)
   
   > [!NOTE]
   > De naam in uw certificaat kan afwijken, maar wordt voorafgegaan door **Apple Development iOS Push Services:**.
   > 
   > 
10. Ctrl + klik in Sleutelhangertoegang op het nieuwe pushcertificaat dat u hebt gemaakt in de categorie **Certificaten**. Klik op **Exporteer**, geef het bestand een naam, selecteer de **.p12**-indeling en klik vervolgens op **Bewaar**.
    
    Onthoud de bestandsnaam en locatie van het geëxporteerde .p12-pushcertificaat. Deze gegevens worden gebruikt om verificatie via APNS in te schakelen door ze te uploaden naar de klassieke Azure-portal.

#### <a name="create-a-provisioning-profile-for-the-app"></a>Een inrichtingsprofiel voor de app maken
1. Terug in de <a href="http://go.microsoft.com/fwlink/p/?LinkId=272456" target="_blank">iOS-inrichtingsportal</a> selecteert u **Inrichtingsprofielen**, selecteert u **Alle** en klikt u vervolgens op de knop met het **+**-teken om een nieuw profiel te maken. Nu wordt de wizard **iOS-inrichtingsprofielen toevoegen** gestart.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-new-provisioning-profile.png)
2. Selecteer **Ontwikkeling iOS-app** onder **Ontwikkeling** als het inrichtingsprofieltype en klik vervolgens op **Doorgaan**.
3. Selecteer vervolgens in de vervolgkeuzelijst **App-id** de app-id die u zojuist hebt gemaakt en klik op **Doorgaan**
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-select-appid-for-provisioning.png)
4. Selecteer in het scherm **Certificaten selecteren** het ontwikkelingscertificaat dat u hebt gebruikt voor het ondertekenen van programmacode, en klik op **Doorgaan**. Dit is een ondertekeningscertificaat, niet het pushcertificaat dat u zojuist hebt gemaakt.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-cert.png)
5. Selecteer vervolgens de **apparaten** die u voor de tests wilt gebruiken en klik op **Doorgaan**.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-select-devices.png)
6. Kies tot slot een naam voor het profiel in **Profielnaam** en klik op **Genereren**.
   
       ![](./media/notification-hubs-xamarin-enable-apple-push-notifications/notification-hubs-provisioning-name-profile.png)



<!--HONumber=Feb17_HO1-->


