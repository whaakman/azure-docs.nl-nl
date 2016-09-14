1.  Meld u aan bij de [Azure Portal](https://portal.azure.com/).
2.  Klik in de snelbalk achtereenvolgens op **Nieuw**, **Gegevens en opslag** en **DocumentDB (NoSQL)**.

    ![Schermopname van Azure Portal waarbij Meer services en DocumentDB (NoSQL) zijn gemarkeerd](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)  

3. In de blade **Nieuw account** geeft u de gewenste configuratie op voor het DocumentDB-account.

    ![Schermopname van de nieuwe DocumentDB-blade](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)


    - In het vak **ID** typt u de naam die het DocumentDB-account aanduidt.  Wanneer de **id** is gevalideerd, verschijnt een groen vinkje in het vak **ID**. De **ID**-waarde wordt de hostnaam binnen de URI. De **id** mag alleen kleine letters, cijfers en het minteken ('-') bevatten, en moet tussen 3 en 50 tekens zijn. Houd er rekening mee dat *documents.azure.com* wordt toegevoegd aan de eindpuntnaam die u kiest. Het resultaat hiervan wordt het eindpunt van uw DocumentDB-account.

    - Selecteer als **Abonnement** het Azure-abonnement dat u voor het DocumentDB-account wilt gebruiken. Als uw account slechts één abonnement heeft, wordt dit account standaard geselecteerd.

    - In **Resourcegroep** selecteert of maakt u een resourcegroep voor uw DocumentDB-account.  Standaard wordt een nieuwe resourcegroep gemaakt. Zie [Using the Azure portal to manage your Azure resources](../articles/azure-portal/resource-group-portal.md) (De Azure-portal gebruiken om uw Azure-resources te beheren) voor meer informatie.

    - Gebruik **Locatie** om de geografische locatie op te geven waar uw DocumentDB-account moet worden gehost. 
    
    - Schakel **Aan dashboard vastmaken** in, zodat u gemakkelijk toegang krijgt tot uw account en resources die u in de toekomst maakt.  

4.  Zodra de opties voor het nieuwe DocumentDB-account zijn geconfigureerd, klikt u op **Maken**. U kunt de voortgang op het startboard volgen als u de status van de implementatie wilt controleren.  
    ![Schermopname van de tegel Maken op het startboard - Onlinedatabase maken](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-3.png)  

    Of u kunt de voortgang via Notification Hubs bekijken.  

    ![Snel databases maken: schermopname van Notification Hubs, waarop wordt weergegeven dat het DocumentDB-account wordt gemaakt](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-4.png)  

    ![Schermopname van Notification Hubs, waarop wordt weergegeven dat het DocumentDB-account is gemaakt en naar een resourcegroep is geïmplementeerd - melding van Onlinedatabase maken](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-5.png)

5.  Nadat het DocumentDB-account is gemaakt, kan het met de standaardinstellingen worden gebruikt. Merk op dat de standaardconsistentie van de DocumentDB-account is ingesteld op **Sessie**.  U kunt de standaardconsistentie aanpassen door op **Standaardconsistentie** in het resourcemenu te klikken. Zie [Consistency levels in DocumentDB](../articles/azure-portal/resource-group-portal.md) (Consistentieniveaus in DocumentDB) voor meer informatie over de consistentieniveaus die DocumentDB te bieden heeft.

    ![Schermopname van de blade Resourcegroep - begin met het ontwikkelen van de toepassing](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-6.png)  

    ![Schermopname van de blade Consistentieniveau - sessie Consistentie](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-7.png)  

[Procedure: een DocumentDB-account maken]: #Howto
[Volgende stappen]: #NextSteps
[documentdb-manage]:../articles/documentdb/documentdb-manage.md



<!-----HONumber=sep16_HO1-->


