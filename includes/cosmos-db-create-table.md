U kunt Data Explorer nu gebruiken voor het maken van een tabel en om gegevens toe te voegen aan uw database. 

1. Klik in Azure Portal in het navigatiemenu op **Data Explorer (Preview)**. 
2. Klik in de blade Data Explorer op **Nieuwe tabel** en vul vervolgens de pagina in met behulp van de volgende gegevens.

    ![Data Explorer in Azure Portal](./media/cosmos-db-create-table/azure-cosmosdb-data-explorer.png)

    Instelling|Voorgestelde waarde|Beschrijving
    ---|---|---
    Tabel-id|voorbeeldtabel|De id voor de nieuwe tabel. Voor tabelnamen gelden dezelfde tekenvereisten als voor database-id's. Databasenamen moeten tussen de 1 en 255 tekens zijn en mogen geen `/ \ # ?` bevatten of eindigen op een spatie.
    Opslagcapaciteit| 10 GB|Laat de standaardwaarde staan. Dit is de opslagcapaciteit van de database.
    Doorvoer|400 RU‘s|Laat de standaardwaarde staan. U kunt de [doorvoer](../articles/cosmos-db/request-units.md) later opschalen als u de latentie wilt beperken.
    RU/m|Uit|Gebruik de standaardwaarde.

3. Zodra het formulier is ingevuld, klikt u op **OK**.