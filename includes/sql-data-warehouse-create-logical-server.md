### <a name="create-a-new-logical-sql-server-in-the-azure-portal"></a>Een nieuwe logische SQL-server maken in Azure Portal

1. Klik op **Nieuw**, zoek naar **logische server** en druk vervolgens op **ENTER**.

    ![logische server zoeken](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. **SQL-server (logische server)** selecteren 

    ![logische server selecteren](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. Klik op **Maken** om de blade van de nieuwe SQL-server (logische server) te openen.

   <kbd> ![blade logische server openen](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd>
    <kbd>![blade logische server](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png) </kbd>
  
3. Geef in het tekstvak Servernaam van de SQL-serverblade (logische server) een geldige naam op voor de nieuwe logische server. Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.
    
    ![nieuwe servernaam](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > De volledige geldige naam voor uw nieuwe server is <uw_servernaam>.database.windows.net.
    >
    
4. Geef in het tekstvak Aanmeldgegevens van serverbeheerder een gebruikersnaam op voor de aanmelding voor SQL-verificatie voor deze server. Deze aanmelding wordt de principal-aanmelding op serverniveau genoemd. Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.
    
    ![Aanmeldgegevens SQL-beheerder](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. Geef in de tekstvakken **Wachtwoord** en **Wachtwoord bevestigen** een wachtwoord op voor het server-principal-aanmeldingsaccount. Een groen vinkje geeft aan dat u een geldig wachtwoord hebt opgegeven.
    
    ![Wachtwoord SQL-beheerder](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. Selecteer een abonnement waarmee u toestemming hebt om objecten te maken.

    ![abonnement](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. Selecteer in het tekstvak Resourcegroep **Nieuwe maken** en geef vervolgens in hetzelfde tekstvak een geldige naam op voor de nieuwe resourcegroep. U kunt ook een bestaande resourcegroep gebruiken. Een groen vinkje geeft aan dat u een geldige naam hebt opgegeven.

    ![nieuwe resourcegroep](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. Selecteer in het tekstvak **Locatie** een geschikt datacentrum voor uw locatie, bijvoorbeeld 'Australië - oost'.
    
    ![serverlocatie](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > Het tekstvak voor **Toegang van Azure-services tot server toestaan** kan niet worden gewijzigd op deze blade. U kunt deze instelling op de serverfirewallblade wijzigen. Zie [Aan de slag met beveiliging](../articles/sql-database/sql-database-manage-servers-portal.md) voor meer informatie.
    >
    
9. Klik op **Create**.

    ![knop Maken](./media/sql-data-warehouse-create-logical-server/create.png)



<!--HONumber=Feb17_HO3-->


