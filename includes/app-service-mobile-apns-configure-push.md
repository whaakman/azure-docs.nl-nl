

1. Start op uw Mac **Sleutelhangertoegang**. Op de linkernavigatiebalk onder **categorie**Open **mijn certificaten**. Zoek het SSL-certificaat dat u in de vorige sectie hebt gedownload en de inhoud ervan worden vermeld. Alleen het certificaat selecteren (geen selecteert de persoonlijke sleutel), en [exporteren](https://support.apple.com/kb/PH20122?locale=en_US).
2. In de [Azure-portal](https://portal.azure.com/), klikt u op **door alles bladeren** > **App Services**, en klikt u op uw back-end van Mobile Apps. Onder **instellingen**, klikt u op **App Service-Push**, en klik vervolgens op de naam van uw notification hub. Ga naar **Apple Push Notification Services** > **-certificaat uploaden**. Upload het bestand .p12, als u de juiste **modus** (afhankelijk van of uw SSL-clientcertificaat uit eerder is productie of sandbox). Eventuele wijzigingen worden opgeslagen.

De service is nu geconfigureerd om te werken met pushmeldingen op iOS.

[1]: ./media/app-service-mobile-apns-configure-push/mobile-push-notification-hub.png
