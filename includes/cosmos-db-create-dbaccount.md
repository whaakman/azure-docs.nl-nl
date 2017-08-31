1. Meld u in een nieuw venster aan bij [Azure Portal](https://portal.azure.com/).
2. Klik in het deelvenster links op **Nieuw**, klik op **Databases** en klik vervolgens onder **Azure Cosmos DB** op **Maken**.
   
   ![Het deelvenster Databases in Azure Portal](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. Geef op de blade **Nieuw account** de gewenste configuratie op voor dit Azure Cosmos DB-account. 

    Met Azure Cosmos DB kunt u een van de vier programmeermodellen kiezen: Gremlin (Graph), MongoDB, SQL (DocumentDB) en Tabel (sleutelwaarde). Voor elk van deze modellen is momenteel een afzonderlijk account vereist.
    
    In dit artikel programmeren we met de DocumentDB API. Daarom kiest u bij het invullen van het formulier voor **SQL (DocumentDB)**. Als u grafiekgegevens voor een sociale media-app hebt of sleutel/waarde-gegevens (tabel) of gegevens die zijn gemigreerd vanuit een MongoDB-app, vergeet dan niet dat Azure Cosmos DB een maximaal beschikbaar, wereldwijd gedistribueerd databaseserviceplatform heeft voor al uw bedrijfskritische toepassingen.

    Vul de velden in op de blade **Nieuw account** en gebruik de gegevens in de volgende schermopname hierbij als richtlijn. Uw waarden kunnen verschillen van de waarden in de schermopname.
 
    ![De blade Nieuw account voor Azure Cosmos DB](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)

    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Id|*Unieke waarde*|Een unieke naam die dit Azure Cosmos DB-account identificeert. Omdat *documents.azure.com* is toegevoegd aan de id die u hebt opgegeven om uw URI te maken, gebruikt u een unieke maar identificeerbare id. De id mag alleen kleine letters, cijfers en het koppelteken (-) bevatten en moet 3 tot 50 tekens lang zijn.
    API|SQL (DocumentDB)|Verderop in dit artikel gaan we programmeren met de [DocumentDB API](../articles/documentdb/documentdb-introduction.md).|
    Abonnement|*Uw abonnement*|Het Azure-abonnement dat u wilt gebruiken voor dit Azure Cosmos DB-account. 
    Resourcegroep|*Dezelfde waarde als id*|De nieuwe resourcegroepnaam voor het account. Gebruik dezelfde naam als uw id om het uzelf gemakkelijk te maken. 
    Locatie|*De regio het dichtst bij uw gebruikers*|De geografische locatie waar u het Azure Cosmos DB-account gaat hosten. Kies de locatie die zich het dichtst bij uw gebruikers bevindt, zodat ze de snelst mogelijke toegang tot de gegevens hebben.
4. Klik op **Maken** om het account te maken.
5. Klik op de bovenste werkbalk op het pictogram **Meldingen** (het ![pictogram Meldingen](./media/cosmos-db-create-dbaccount/notification-icon.png)) om het implementatieproces te controleren.

    ![Het deelvenster Meldingen in Azure Portal](./media/cosmos-db-create-dbaccount-graph/azure-documentdb-nosql-notification.png)

6.  Wanneer in het venster Meldingen wordt aangegeven dat de implementatie is voltooid, sluit u dit venster en opent u het nieuwe account vanuit de tegel **Alle resources** op het dashboard. 

    ![Het DocumentDB-account op de tegel Alle resources](./media/cosmos-db-create-dbaccount/all-resources.png)
 
