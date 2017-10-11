
1. In de [Azure-portal](https://portal.azure.com/), klikt u op **door alles bladeren** > **App Services**, en klikt u op uw back-end van Mobile Apps. Onder **instellingen**, klikt u op **App Service-Push**, en klik vervolgens op de naam van uw notification hub.
2. Ga naar **Windows (WNS)**, voer de **beveiligingssleutel** (clientgeheim) en **pakket-SID** die u hebt verkregen via de Live Services site en klik vervolgens op **opslaan** .

    ![Stel de WNS-sleutel in de portal](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

Uw back-end is nu geconfigureerd voor het gebruik van WNS om pushmeldingen te verzenden.
