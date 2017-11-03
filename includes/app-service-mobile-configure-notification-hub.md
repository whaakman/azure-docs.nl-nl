Maakt gebruik van de functie Mobile Apps van Azure App Service [Azure Notification Hubs] verzenden pushes, zodat u een notification hub voor uw mobiele app configureren.

1. In de [Azure-portal], gaat u naar **App Services**, en klik vervolgens op de back-end van uw app. Onder **instellingen**, klikt u op **Push**.
2. Klik op **Connect** naar een notification hub-resource toevoegen aan de app. U kunt een hub maken of verbinding maken met een bestaande.

    ![](./media/app-service-mobile-create-notification-hub/configure-hub-flow.png)

U hebt nu een notification hub verbonden met uw mobiele Apps van back-end-project. Later configureert u deze notification hub verbinding maken met een platform notification system (PNS) om naar apparaten te pushen.

[Azure-portal]: https://portal.azure.com/
[Azure Notification Hubs]: https://azure.microsoft.com/en-us/documentation/articles/notification-hubs-push-notification-overview/
