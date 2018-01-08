

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer **Nieuw** > **Web en mobiel** > **Notification Hub**.
   
      ![Azure Portal - Een Notification Hub maken](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
3. Typ een unieke naam in het vak **Notification Hub**. Selecteer uw **regio**, het **abonnement** en de **resourcegroep** (als u er al een hebt). 
   
      Als u nog geen servicebus-naamruimte hebt, kunt u de standaardnaam gebruiken. Deze wordt gemaakt op basis van de naam van de hub (als de naamruimte beschikbaar is).
    
      Als u al een service bus-naamruimte hebt waarin u de hub wilt maken, gaat u als volgt te werk.

    a. In de gebied **Naamruimte** selecteert u de koppeling **Bestaande selecteren**. 
   
    b. Selecteer **Maken**.
   
      ![Azure Portal - eigenschappen van de Notification Hub instellen](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

4. Nadat u de naamruimte en de Notification Hub hebt gemaakt, opent u deze door **Alle resources** te selecteren en vervolgens de gemaakte Notification Hub uit de lijst te selecteren. 
   
      ![Azure Portal - Portal-pagina van Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-resources.png)

5. Selecteer **Toegangsbeleid** in de lijst. Let op de twee verbindingsreeksen die voor u beschikbaar zijn. Later moet u er pushmeldingen mee afhandelen.

      >[!IMPORTANT]
      >Gebruik **NIET** de DefaultFullSharedAccessSignature in uw toepassing. Deze mag alleen in uw back-end worden gebruikt.
      >
   
      ![Azure Portal - verbindingsreeksen voor de Notification Hub](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

