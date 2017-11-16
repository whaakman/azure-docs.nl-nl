U kunt het hulpprogramma Data Explorer nu in de Azure portal gebruiken om een database en tabel te maken. 

1. Klik op **Gegevensverkenner** > **nieuwe tabel**. 
    
    De **tabel toevoegen** gebied aan de rechterkant wordt weergegeven, moet u mogelijk scrollen rechts om het te bekijken.

    ![Data Explorer in de Azure Portal](./media/cosmos-db-create-table/azure-cosmosdb-data-explorer.png)

2. In de **tabel toevoegen** pagina, voert u de instellingen voor de nieuwe tabel.

    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Tabel-id|voorbeeldtabel|De id voor de nieuwe tabel. Voor tabelnamen gelden dezelfde tekenvereisten als voor database-id's. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen `/ \ # ?` bevatten of eindigen op een spatie.
    Opslagcapaciteit| Vast (10 GB)|Wijzig de waarde in **vast (10 GB)**. Deze waarde is de opslagcapaciteit van de database.
    Doorvoer|400 RU‘s|Wijzig de doorvoer in 400 aanvraageenheden per seconde (RU/s). U kunt de doorvoer later opschalen als u de latentie wilt beperken.

    Klik op **OK**.

    Data Explorer wordt weergegeven voor de nieuwe database en tabel.

    ![De Azure portal Data Explorer, toont de nieuwe database en verzameling](./media/cosmos-db-create-table/azure-cosmos-db-new-table.png)