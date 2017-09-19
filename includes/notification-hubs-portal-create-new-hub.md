

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **Nieuw** > **Web en mobiel** > **Notification Hub**.
   
      ![Azure Portal - Een Notification Hub maken](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
3. Typ een unieke naam in het vak **Notification Hub**. Selecteer uw **regio**, het **abonnement** en de **resourcegroep** (als u er al een hebt). 
   
    Als u al een service bus-naamruimte hebt waarin u de hub wilt maken, gaat u als volgt te werk:

    a. In de gebied **Naamruimte** selecteert u de koppeling **Bestaande selecteren**. 
   
    b. Selecteer **Maken**.

    Als u nog geen servicebus-naamruimte hebt, kunt u de standaardnaam gebruiken. Deze wordt gemaakt op basis van de naam van de hub (als de naamruimte beschikbaar is).
   
      ![Azure Portal - eigenschappen van de Notification Hub instellen](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

    Nadat u de naamruimte en Notification Hub hebt gemaakt, wordt Azure Portal geopend. 
   
      ![Azure Portal - Portal-pagina van Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-page.png)

4. Selecteer **instellingen** > **Toegangsbeleid**. Let op de twee verbindingsreeksen die voor u beschikbaar zijn. Later moet u er pushmeldingen mee afhandelen.
   
      ![Azure Portal - verbindingsreeksen voor de Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

