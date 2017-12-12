
<!--
includes/sql-database-include-connection-string-20-portalshots.md

Latest Freshness check:  2015-09-02 , GeneMi.

## Connection string
-->


### <a name="obtain-the-connection-string-from-the-azure-portal"></a>De verbindingsreeks ophalen via de Azure portal
Gebruik de [Azure-portal](https://portal.azure.com/) om op te halen van de verbindingsreeks die nodig is voor uw clientprogramma om te communiceren met Azure SQL Database. 

1. Selecteer **door alles bladeren** > **SQL-databases**.

2. Voer de naam van uw database in het filtertekstvak in de buurt van de linkerbovenhoek van de **SQL-databases** blade.

3. Selecteer de rij voor uw database.

4. Nadat de blade wordt weergegeven voor uw database, selecteer visual gemak de **minimaliseren** knoppen om samen te vouwen de blades die u voor bladeren en het filteren van de database gebruikt. 
   
    ![Filter voor het isoleren van uw database][10-FilterDatabase]
5. Selecteer op de blade voor uw database **databaseverbindingsreeksen tonen**.

6. Als u de verbindingsbibliotheek ADO.NET gebruiken wilt, kopieert u de tekenreeks met het label **ADO**. 
   
    ![Kopieer de verbindingsreeks ADO voor uw database][20-CopyAdoConnectionString]
7. In een indeling of een andere door de verbindingsinformatie in uw clientcode programma te plakken.

Zie voor meer informatie [verbindingsreeksen en configuratiebestanden](http://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->

[10-FilterDatabase]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-a.png

[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
