1. Meld u in een nieuw browservenster aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het menu links op **Een resource maken**, klik op **Databases** en klik vervolgens onder **Azure Cosmos DB** op **Maken**. 
   
   ![Schermopname van Azure Portal waarbij Meer services en Azure Cosmos DB zijn gemarkeerd](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-1.png)

3. Voer op de pagina **Nieuw account** de instellingen in voor het nieuwe Azure Cosmos DB-account. 
 
    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Id|*Voer een unieke naam in*|Voer een unieke naam in om dit Azure Cosmos DB-account te identificeren. Omdat *documents.azure.com* is toegevoegd aan de id die u hebt opgegeven om uw URI te maken, gebruikt u een unieke maar identificeerbare id.<br><br>De id mag alleen kleine letters, cijfers en het koppelteken (-) bevatten en moet 3 tot 50 tekens lang zijn.
    API|Azure Table|De API bepaalt het type te maken account. Azure Cosmos DB biedt vijf API's die aansluiten bij de behoeften van uw toepassing: SQL (documentdatabase), Gremlin (grafiekdatabase) MongoDB (documentdatabase), Azure Table en Cassandra. Momenteel is voor elke API een afzonderlijk account nodig.<br><br>Selecteer **Azure Table**, omdat u in deze snelstartgids een tabel maakt die geschikt is voor de Table-API.<br><br>[Meer informatie over de Table-API](../articles/cosmos-db/table-introduction.md) |
    Abonnement|*Geef dezelfde unieke naam op zoals hierboven bij id*|Selecteer het Azure-abonnement dat u wilt gebruiken voor dit Azure Cosmos DB-account. 
    Resourcegroep|*Dezelfde waarde als id*|Voer een nieuwe resourcegroepnaam in voor het account. Gebruik dezelfde naam als uw id om het uzelf gemakkelijk te maken. 
    Locatie|*Selecteer de regio die het dichtst bij uw gebruikers is gelegen*|Selecteer de geografische locatie waar u het Azure Cosmos DB-account gaat hosten. Gebruik de locatie die zich het dichtst bij uw gebruikers bevindt, zodat ze de snelst mogelijke toegang tot de gegevens hebben.
    Georedundantie inschakelen| Leeg laten | Hiermee maakt u een gerepliceerde versie van uw database in een tweede (gekoppelde) regio. Laat dit leeg.  
    Vastmaken aan dashboard | Selecteer | Selecteer dit vakje, zodat uw nieuwe databaseaccount wordt toegevoegd aan uw portaldashboard voor eenvoudige toegang.

    Klik vervolgens op **Maken**.  

    ![Schermopname van de blade Nieuwe Azure Cosmos DB](./media/cosmos-db-create-dbaccount-table/create-nosql-db-databases-json-tutorial-2.png)

4. Het duurt enkele minuten om het account te maken. Tijdens het maken van het account geeft de portal de tegel **Azure Cosmos DB implementeren** weer.

    ![Het deelvenster Meldingen in Azure Portal](./media/cosmos-db-create-dbaccount-table/deploying-cosmos-db.png)

    Nadat het account is gemaakt, wordt de pagina **Gefeliciteerd! Uw Azure Cosmos DB-account is gemaakt** weergegeven.
