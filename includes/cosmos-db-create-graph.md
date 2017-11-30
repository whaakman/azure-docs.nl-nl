U kunt nu het hulpprogramma Data Explorer in Azure Portal gebruiken om een grafiekdatabase te maken. 

1. Selecteer in het menu aan de linkerkant van de Azure Portal **Data Explorer (Preview)**.

2. Onder **Data Explorer (Preview)**, selecteer **nieuwe grafiek**. Vul vervolgens de pagina met behulp van de volgende informatie:

    ![Data Explorer in de Azure Portal](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer.png)

    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Database-id|voorbeelddatabase|Voer *-voorbeelddatabase* als de naam voor de nieuwe database. Databasenaam moet tussen 1 en 255 tekens en mag niet `/ \ # ?` of een spatie.
    Graaf-id|voorbeeldgrafiek|Voer *voorbeeld grafiek* als de naam voor de nieuwe verzameling. Namen van de grafiek hebben dezelfde vereisten als de database-id's teken.
    Opslagcapaciteit| 10 GB|Laat de standaardwaarde staan. Dit is de opslagcapaciteit van de database.
    Doorvoer|400 RU‘s|Laat de standaardwaarde staan. U kunt de doorvoer later opschalen als u de latentie wilt beperken.
    Partitiesleutel|/FirstName|Een partitiesleutel waarmee gegevens gelijkmatig worden gedistribueerd naar elke partitie. Het selecteren van de juiste partitiesleutel is belangrijk bij het maken van een grafiek zodat. Zie voor meer informatie [ontwerpen voor het partitioneren van](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

3. Nadat het formulier is ingevuld, selecteert u **OK**.
