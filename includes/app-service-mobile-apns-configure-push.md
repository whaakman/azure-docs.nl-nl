

1. Start op uw Mac **Sleutelhangertoegang**. Op de linkernavigatiebalk onder **categorie**Open **mijn certificaten**. Zoek het SSL-certificaat dat u in de vorige sectie hebt gedownload en vervolgens de inhoud ervan worden vermeld. Selecteer alleen het certificaat (de persoonlijke sleutel niet selecteren). Vervolgens [exporteren](https://support.apple.com/kb/PH20122?locale=en_US).
2. In de [Azure-portal](https://portal.azure.com/), selecteer **door alles bladeren** > **App Services**. Vervolgens selecteert mobiele Apps van uw back-end. 
3. Onder **instellingen**, selecteer **App Service-Push**. Selecteer vervolgens de naam van uw notification hub. 
3. Ga naar **Apple Push Notification Services** > **-certificaat uploaden**. Upload het bestand .p12, als u de juiste **modus** (afhankelijk van of uw SSL-clientcertificaat uit eerder is productie of sandbox). Eventuele wijzigingen worden opgeslagen.

De service is nu geconfigureerd om te werken met pushmeldingen op iOS.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
