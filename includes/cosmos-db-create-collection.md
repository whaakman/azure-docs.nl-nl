U kunt nu het hulpprogramma Data Explorer in Azure Portal gebruiken om een database en een verzameling te maken. 

1. Klik op **Gegevensverkenner** > **nieuwe verzameling**. 
    
    De **verzameling toevoegen** gebied aan de rechterkant wordt weergegeven, moet u mogelijk scrollen rechts om het te bekijken.

    ![De Azure portal Data Explorer verzameling toevoegen-blade](./media/cosmos-db-create-collection/azure-cosmosdb-data-explorer.png)

2. In de **verzameling toevoegen** pagina, voert u de instellingen voor de nieuwe verzameling.

    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Database-id|Taken|Voer *taken* als de naam voor de nieuwe database. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen /, \\, # of ? bevatten en mogen niet eindigen met een spatie.
    Verzamelings-id|Items|Voer *Items* als de naam voor de nieuwe verzameling. Verzameling-ID's hebben dezelfde vereisten als databasenamen teken.
    Opslagcapaciteit| Vast (10 GB)|Wijzig de waarde in **vast (10 GB)**. Deze waarde is de opslagcapaciteit van de database.
    Doorvoer|400 RU|Wijzig de doorvoer in 400 aanvraageenheden per seconde (RU/s). U kunt de doorvoer later opschalen als u de latentie wilt beperken.
    Partitiesleutel|/category|Een partitiesleutel waarmee gegevens gelijkmatig worden gedistribueerd naar elke partitie. Het is belangrijk dat u de juiste partitiesleutel selecteert bij het maken van een prestatieverzameling. Ga voor meer informatie hierover naar [Ontwerpen voor partitioneren](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

    Klik op **OK**.

    Data Explorer wordt weergegeven voor de nieuwe database en verzameling.

    ![De Azure portal Data Explorer, toont de nieuwe database en verzameling](./media/cosmos-db-create-collection/azure-cosmos-db-new-collection.png)