1. Meld u in een nieuw venster aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in de snelbalk achtereenvolgens op **Nieuw**, **Databases** en **NoSQL (DocumentDB)**.
   
   ![Schermopname van Azure Portal waarbij Meer services en DocumentDB (NoSQL) zijn gemarkeerd](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)  
3. In de blade **Nieuw account** geeft u de gewenste configuratie op voor het DocumentDB-account.
   
    ![Schermopname van de nieuwe DocumentDB-blade](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)
   
   * In het vak **Id** typt u de naam die het DocumentDB-account aanduidt.  Wanneer de **id** is gevalideerd, verschijnt een groen vinkje in het vak **Id**. De waarde van **Id** wordt de hostnaam binnen de URI. De **id** mag alleen kleine letters, cijfers en het minteken ('-') bevatten, en moet tussen de 3 en 50 tekens zijn. Houd er rekening mee dat *documents.azure.com* wordt toegevoegd aan de eindpuntnaam die u kiest. Het resultaat hiervan wordt het eindpunt van uw DocumentDB-account.
   * Selecteer in het vak **NoSQL-API** de optie **DocumentDB**.  
   * Selecteer als **Abonnement** het Azure-abonnement dat u voor het DocumentDB-account wilt gebruiken. Als uw account slechts één abonnement heeft, wordt dit account standaard geselecteerd.
   * In **Resourcegroep** selecteert of maakt u een resourcegroep voor uw DocumentDB-account.  Standaard wordt een nieuwe resourcegroep gemaakt. Zie [Using the Azure portal to manage your Azure resources](../articles/azure-portal/resource-group-portal.md) (Azure Portal gebruiken om uw Azure-resources te beheren) voor meer informatie.
   * Gebruik **Locatie** om de geografische locatie op te geven waar uw DocumentDB-account moet worden gehost. 
4. Zodra de opties voor het nieuwe DocumentDB-account zijn geconfigureerd, klikt u op **Maken**. Controleer de hub Meldingen als u de status van de implementatie wilt bekijken.  
   
   ![Snel databases maken: schermopname van Notification Hubs, waarop wordt weergegeven dat het DocumentDB-account wordt gemaakt](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-4.png)  
   
   ![Schermopname van Notification Hubs, waarop wordt weergegeven dat het DocumentDB-account is gemaakt en naar een resourcegroep is geïmplementeerd - melding van Onlinedatabase maken](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-5.png)
5. Nadat het DocumentDB-account is gemaakt, kan het met de standaardinstellingen worden gebruikt. Als u de standaardinstellingen wilt controleren, klikt u op het pictogram **NoSQL (DocumentDB)** in de snelbalk en daarna op uw nieuwe account. Klik vervolgens in het menu Resource op **Standaardconsistentie**.

   ![Schermopname die u toont hoe u uw Azure DocumentDB-databaseaccount opent in Azure Portal](./media/documentdb-create-dbaccount/azure-documentdb-database-open-account-portal.png)  

   De standaardconsistentie van het DocumentDB-account is ingesteld op **Sessie**.  U kunt de standaardconsistentie aanpassen door een van de andere consistentieopties te selecteren. Zie [Consistency levels in DocumentDB](../articles/documentdb/documentdb-consistency-levels.md) (Consistentieniveaus in DocumentDB) voor meer informatie over de consistentieniveaus die DocumentDB te bieden heeft.

[How to: Create a DocumentDB account]: #Howto
[Next steps]: #NextSteps
[documentdb-manage]:../articles/documentdb/documentdb-manage.md
