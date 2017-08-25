U kunt nu het hulpprogramma Data Explorer in Azure Portal gebruiken om een grafiekdatabase te maken. 

1. Klik in Azure Portal in het navigatiemenu links op **Data Explorer (preview-versie)**. 
2. Klik op de blade **Data Explorer (preview-versie)** op **Nieuwe grafiek**. Vul vervolgens de pagina in met behulp van de volgende gegevens.

    ![Data Explorer in de Azure Portal](./media/cosmos-db-create-graph/azure-cosmosdb-data-explorer.png)

    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Database-id|voorbeelddatabase|De id voor de nieuwe database. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen `/ \ # ?` bevatten of eindigen op een spatie.
    Graaf-id|voorbeeldgrafiek|De id voor de nieuwe graaf. Voor graafnamen gelden dezelfde tekenvereisten als voor database-id's.
    Opslagcapaciteit| 10 GB|Laat de standaardwaarde staan. Dit is de opslagcapaciteit van de database.
    Doorvoer|400 RU‘s|Laat de standaardwaarde staan. U kunt de doorvoer later opschalen als u de latentie wilt beperken.
    Partitiesleutel|/userid|Een partitiesleutel waarmee gegevens gelijkmatig worden gedistribueerd naar elke partitie. Het is belangrijk dat u de juiste partitiesleutel selecteert bij het maken van een prestatiegraaf. Ga voor meer informatie hierover naar [Ontwerpen voor partitioneren](../articles/cosmos-db/partition-data.md#designing-for-partitioning).

3. Zodra het formulier is ingevuld, klikt u op **OK**.