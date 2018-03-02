1. Meld u in een nieuw venster aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu links op **Een resource maken**, klik op **Databases** en klik vervolgens onder **Azure Cosmos DB** op **Maken**.
   
   ![Schermopname van Azure Portal waarbij Meer services en Azure Cosmos DB zijn gemarkeerd](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-1.png)

3. Geef in het deelvenster **Nieuw account** de gewenste configuratie op voor het Azure Cosmos DB-account. 

    Met Azure Cosmos DB kunt u een van de vier programmeermodellen kiezen: Gremlin (Graph), MongoDB, SQL en Table (sleutelwaarde). 
       
    In deze Quick Start programmeren we met de MongoDB-API, daarom kiest u bij het invullen van het formulier voor **MongoDB**. Maar als u grafiekgegevens voor een sociale media-app hebt, documentgegevens uit een catalogus-app, sleutelwaardegegevens (tabelgegevens), moet u er rekening mee houden dat Azure Cosmos DB een zeer beschikbare, globaal gedistribueerd databaseserviceplatform kan bieden voor alle bedrijfskritische toepassingen.

    Vul het deelvenster **Nieuw account** in en gebruik de gegevens in de tabel hierbij als richtlijn.
 
    ![Schermopname van het deelvenster Nieuwe Azure Cosmos DB](./media/cosmos-db-create-dbaccount-mongodb/create-nosql-db-databases-json-tutorial-2.png)
   
    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Id|*Unieke waarde*|Een unieke naam die u kiest om het Azure Cosmos DB-account aan te duiden. *documents.azure.com* is toegevoegd aan de id die u hebt opgegeven om uw URI te maken. Gebruik daarom een unieke maar identificeerbare id. De id mag alleen kleine letters, cijfers en het minteken ('-') bevatten, en moet tussen de 3 en 50 tekens lang zijn.
    API|MongoDB|De API bepaalt het type te maken account. Azure Cosmos DB biedt vijf API's die aansluiten bij de behoeften van uw toepassing: SQL (documentdatabase), Gremlin (grafiekdatabase) MongoDB (documentdatabase), Azure Table en Cassandra. Momenteel is voor elke API een afzonderlijk account nodig. <br><br>Selecteer **MongoDB**, omdat u in deze snelstartgids een documentdatabase maakt waarop query's kunnen worden uitgevoerd met behulp van MongoDB.<br><br>[Meer informatie over de MongoDB-API](../articles/cosmos-db/mongodb-introduction.md)|
    Abonnement|*Uw abonnement*|Het Azure-abonnement dat u wilt gebruiken voor het Azure Cosmos DB-account. 
    Resourcegroep|*Dezelfde waarde als id*|De nieuwe resourcegroepnaam voor het account. Gebruik dezelfde naam als uw id om het uzelf gemakkelijk te maken. 
    Locatie|*De regio het dichtst bij uw gebruikers*|De geografische locatie waar u het Azure Cosmos DB-account gaat hosten. Kies de locatie die zich het dichtst bij uw gebruikers bevindt, zodat ze de snelst mogelijke toegang tot de gegevens hebben.

4. Klik op **Maken** om het account te maken.
5. Klik op de werkbalk op **Meldingen** om het implementatieproces te bewaken.

    ![De melding Implementatie is gestart](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-nosql-notification.png)

6.  Als de implementatie is voltooid, opent u het nieuwe account via de tegel Alle resources. 

    ![Azure Cosmos DB-account op de tegel Alle resources](./media/cosmos-db-create-dbaccount-mongodb/azure-documentdb-all-resources.png)
