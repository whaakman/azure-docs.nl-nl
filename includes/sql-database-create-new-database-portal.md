
<!--
includes/sql-database-create-new-database-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md

-->
## Een nieuwe Azure SQL-database maken
Voer de volgende stappen uit in de Azure-portal als u een nieuwe Azure SQL-database wilt maken op een nieuwe of bestaande logische server met Azure SQL Database.

1. Als u momenteel niet bent verbonden, maakt u eerst verbinding met [Azure Portal](http://portal.azure.com).
2. Klik op **Nieuw**, typ **SQL Database** en klik op **SQL Database (nieuwe database)**.
   
     ![Nieuwe database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-1.png)
3. Klik op **SQL Database (nieuwe database)**.
   
     ![Nieuwe database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-2.png)
4. Klik op **Maken** om een nieuwe database te maken in de service SQL Database.
   
     ![Nieuwe database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-3.png)
5. Geef de waarden op voor de volgende servereigenschappen:
   
   * Databasenaam
   * Abonnement: dit is alleen van toepassing als u meerdere abonnementen hebt.
   * Resourcegroep: als u nog maar net begint, gebruik dan de resourcegroep van de logische server.
   * Bron selecteren: u kunt een lege database, voorbeeldgegevens of een Azure-databaseback-up selecteren. Zie de koppelingen aan het einde van dit artikel voor het migreren van een on-premises SQL Server-database of het laden van gegevens met behulp van het BCP-opdrachtregelprogramma.
   * Server: een nieuwe of bestaande logische server.
   * Aanmeldgegevens van serverbeheerder
   * Wachtwoord
   * Prijscategorie: als u nog maar net begint, gebruik dan de standaardwaarde S0.
   * Sortering: dit is alleen van toepassing als een lege database is gekozen.
     
        ![New database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-4.png)
6. Klik op **Create**. In het systeemvak ziet u dat de implementatie is gestart.
   
    ![Nieuwe database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-5.png)
7. Wacht tot de implementatie is voltooid voordat u verder gaat met de volgende stap.
   
     ![Nieuwe database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-6.png)

<!--HONumber=sep16_HO1-->


