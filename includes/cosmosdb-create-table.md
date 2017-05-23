U kunt Data Explorer nu gebruiken voor het maken van een grafiekcontainer en om gegevens toe te voegen aan uw database. 

1. Klik In Azure Portal in het navigatiemenu onder **Tabellen** op **Data Explorer**. 
2. Klik op de blade Data Explorer op **Nieuwe verzameling** en vul vervolgens de pagina in met behulp van de volgende gegevens.

    ![Data Explorer in Azure Portal](./media/cosmosdb-create-graph/azure-cosmosdb-data-explorer.png)

    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Database-id|voorbeelddatabase|De id voor de nieuwe database. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen `/ \ # ?` bevatten of eindigen op een spatie.
    Verzamelings-id|voorbeeldtabel|De id voor de nieuwe verzameling. Voor verzamelingsnamen gelden dezelfde tekenvereisten als voor database-id's.
    Opslagcapaciteit| 10 GB|Laat de standaardwaarde staan. Dit is de opslagcapaciteit van de database.
    Doorvoer|400 RU‘s|Laat de standaardwaarde staan. U kunt de doorvoer later opschalen als u de latentie wilt beperken.
    Partitiesleutel|/lastname|Een partitiesleutel waarmee gegevens gelijkmatig worden gedistribueerd naar elke partitie. Het is belangrijk dat u de juiste partitiesleutel selecteert bij het maken van een prestatieverzameling. Ga voor meer informatie hierover naar [Ontwerpen voor partitioneren](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

3. Zodra het formulier is ingevuld, klikt u op **OK**.