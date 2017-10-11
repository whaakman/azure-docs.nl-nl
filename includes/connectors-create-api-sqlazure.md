### <a name="prerequisites"></a>Vereisten
* Een Azure-account; kunt u een [gratis account](https://azure.microsoft.com/free)
* Een [Azure SQL Database](../articles/sql-database/sql-database-get-started.md) met de verbindingsinformatie, met inbegrip van de servernaam, databasenaam en gebruikersnaam en wachtwoord. Deze informatie is opgenomen in de verbindingsreeks voor de SQL-Database:
  
    Server tcp =:*yoursqlservername*. database.windows.net,1433;Initial Catalog =*yourqldbname*; Beveiliginsinfo = False; Gebruikers-ID = {your_username}; Wachtwoord = {your_password}; Voor MultipleActiveResultSets = False; Versleutelen = True; TrustServerCertificate = False; Verbindingstime-out = 30;
  
    Lees meer over [Azure SQL-Databases](https://azure.microsoft.com/services/sql-database).

> [!NOTE]
> Wanneer u een Azure SQL Database maakt, kunt u ook de voorbeelddatabases die deel uitmaakt van SQL maken. 
> 
> 

Voordat u uw Azure SQL Database in een logische app, verbinding maken met uw SQL-Database. U kunt dit eenvoudig doen in uw logische app in de Azure portal.  

Verbinding maken met uw Azure SQL Database met behulp van de volgende stappen uit:  

1. Een logische app maken. In de ontwerpfunctie voor Logic Apps een trigger toevoegen en voeg vervolgens een actie. Selecteer **beheerde API's van Microsoft weergeven** in de vervolgkeuzelijst en voer vervolgens 'sql' in het zoekvak. Selecteer een van de acties:  
   
    ![Stap voor SQL Azure-verbinding maken](./media/connectors-create-api-sqlazure/sql-actions.png)
2. Als u verbindingen met SQL-Database nog niet eerder hebt gemaakt, wordt u gevraagd de verbindingsdetails op te geven:  
   
    ![Stap voor SQL Azure-verbinding maken](./media/connectors-create-api-sqlazure/connection-details.png) 
3. Geef de details van de SQL-Database. Eigenschappen met een sterretje zijn vereist.
   
   | Eigenschap | Details |
   | --- | --- |
   | Verbinding maken via de Gateway |Laat dit uitgeschakeld. Dit wordt gebruikt bij het verbinden met een lokale SQL Server. |
   | Verbindingsnaam * |Voer een naam voor de verbinding. |
   | Naam van SQL Server * |Voer de naam van de server; Dit is ongeveer *servername.database.windows.net*. Naam van de server wordt weergegeven in de eigenschappen van de SQL-Database in de Azure portal en ook weergegeven in de verbindingsreeks. |
   | SQL-databasenaam * |Voer de naam opgegeven van de SQL-Database. Deze wordt vermeld in de eigenschappen van de SQL-Database in de verbindingsreeks: Initial Catalog =*yoursqldbname*. |
   | Gebruikersnaam * |Geef de gebruikersnaam die u hebt gemaakt bij de SQL-Database is gemaakt. Deze wordt vermeld in de eigenschappen van de SQL-Database in de Azure portal. |
   | Wachtwoord * |Voer het wachtwoord die u hebt gemaakt bij de SQL-Database is gemaakt. |
   
    Deze referenties worden gebruikt voor het autoriseren van uw logische app verbinding maken en toegang tot uw SQL-gegevens. Hierna kunt er uw Verbindingsdetails ongeveer als volgt:  
   
    ![Stap voor SQL Azure-verbinding maken](./media/connectors-create-api-sqlazure/sample-connection.png) 
4. Selecteer **Maken**. 
5. U ziet dat de verbinding is gemaakt. Ga nu verder met de overige stappen in uw logische app: 
   
    ![Stap voor SQL Azure-verbinding maken](./media/connectors-create-api-sqlazure/table.png)

