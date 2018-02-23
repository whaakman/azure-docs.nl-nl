
1. In de [Azure-portal](https://portal.azure.com/), selecteer **door alles bladeren** > **App Services**. Vervolgens selecteert mobiele Apps van uw back-end. Onder **instellingen**, selecteer **App Service-Push**. Selecteer vervolgens de naam van uw notification hub.
2. Ga naar **Windows (WNS)**. Voer vervolgens de **beveiligingssleutel** (clientgeheim) en **pakket-SID** die u hebt verkregen via de Live Services site. Selecteer vervolgens **opslaan**.

    ![Stel de WNS-sleutel in de portal](./media/app-service-mobile-configure-wns/mobile-push-wns-credentials.png)

Uw back-end is nu geconfigureerd voor het gebruik van WNS om pushmeldingen te verzenden.
