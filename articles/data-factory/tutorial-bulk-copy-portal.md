---
title: Gegevens bulksgewijs kopiëren met behulp van Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van Azure Data Factory en Activiteit kopiëren om gegevens bulksgewijs van een brongegevensopslag naar een doelgegevensopslag te kopiëren.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/10/2018
ms.author: jingwang
ms.openlocfilehash: 440b07b494b34db7ff3fcdf5d5ac830b165c339d
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="copy-multiple-tables-in-bulk-by-using-azure-data-factory"></a>Meerdere tabellen bulksgewijs kopiëren met behulp van Azure Data Factory
Deze zelfstudie demonstreert het **kopiëren van een aantal tabellen uit Azure SQL Database naar Azure SQL Data Warehouse**. U kunt hetzelfde patroon toepassen in andere kopieerscenario's. Bijvoorbeeld het kopiëren van tabellen van SQL Server/Oracle naar Azure SQL Database/Data Warehouse/Azure Blob, verschillende paden kopiëren van Blob naar Azure SQL Database-tabellen.

> [!NOTE]
> - Zie [Inleiding tot Azure Data Factory](introduction.md) als u niet bekend bent met Azure Data Factory.
> - Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Op hoog niveau bevat deze zelfstudie de volgende stappen:

> [!div class="checklist"]
> * Een data factory maken.
> * Gekoppelde Azure SQL Database-, Azure SQL Data Warehouse- en Azure Storage-services maken.
> * Gegevenssets voor Azure SQL Database en Azure SQL Data Warehouse maken.
> * Een pijplijn maken om de te kopiëren tabellen op te zoeken en een andere pijplijn om de kopieerbewerking daadwerkelijk uit te voeren. 
> * Een pijplijnuitvoering starten.
> * De uitvoering van de pijplijn en van de activiteit controleren.

In deze zelfstudie wordt Azure Portal gebruikt. Zie [Quickstarts](quickstart-create-data-factory-dot-net.md) (Snelstartgidsen) voor meer informatie over het gebruik van andere hulpprogramma's/SDK's voor het maken van een gegevensfactory. 

## <a name="end-to-end-workflow"></a>End-to-end werkstroom
In dit scenario gebruikt u een aantal tabellen in Azure SQL Database die u gaat kopiëren naar SQL Data Warehouse. Dit is de logische volgorde van de stappen in de werkstroom die in pijplijnen plaatsvindt:

![Werkstroom](media/tutorial-bulk-copy-portal/tutorial-copy-multiple-tables.png)

* De eerste pijplijn zoekt de lijst op met tabellen die moeten worden gekopieerd naar de sinkgegevensopslag.  U kunt ook een metagegevenstabel bijhouden waarin alle tabellen worden vermeld die moeten worden gekopieerd naar de sinkgegevensopslag. De pijplijn activeert vervolgens een andere pijplijn, die elke tabel in de database langsloopt en de bewerking uitvoert waarmee de gegevens worden gekopieerd.
* De tweede pijplijn voert de daadwerkelijke kopieerbewerking uit. De lijst met tabellen wordt gebruikt als parameter. Kopieer voor elke tabel in de lijst de specifieke tabel in Azure SQL Database naar de bijbehorende tabel in SQL Data Warehouse met behulp van [gefaseerd kopiëren via Blob storage en PolyBase](connector-azure-sql-data-warehouse.md#use-polybase-to-load-data-into-azure-sql-data-warehouse) (Engelstalig artikel) voor de beste prestaties. In dit voorbeeld wordt de lijst met tabellen in de eerste pijplijn doorgegeven als een waarde voor de parameter. 

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten
* **Een Azure Storage-account**. Het Azure Storage-account wordt gebruikt als faseringsblobopslag in de bulksgewijze kopieerbewerking. 
* **Azure SQL-database**. Deze database bevat de brongegevens. 
* **Azure SQL Data Warehouse**. Dit datawarehouse bevat de uit de SQL Database gekopieerde gegevens. 

### <a name="prepare-sql-database-and-sql-data-warehouse"></a>SQL Database en SQL Data Warehouse voorbereiden

**De Azure SQL Database-bron voorbereiden**:

Maak een Azure SQL Database met Adventure Works LT-testgegevens door het artikel [Create an Azure SQL database](../sql-database/sql-database-get-started-portal.md) (Een Azure SQL-database maken) te volgen. In deze zelfstudie worden alle tabellen van deze voorbeelddatabase naar een SQL0datawarehouse gekopieerd.

**De Azure SQL Data Warehouse-sink voorbereiden**:

1. Als u geen Azure SQL Data Warehouse hebt, raadpleegt u het artikel [Create a SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-get-started-tutorial.md) (Een Azure SQL-database maken) voor de stappen om er een te maken.

2. Maak bijbehorende tabelschema's in SQL Data Warehouse. U kunt het [hulpprogramma voor migratie](https://www.microsoft.com/download/details.aspx?id=49100) gebruiken om het **schema te migreren** van Azure SQL Database naar Azure SQL Data Warehouse. U gebruikt Azure Data Factory om gegevens in een latere stap te migreren/kopiëren.

## <a name="azure-services-to-access-sql-server"></a>Azure-services voor toegang tot SQL-server

Geef Azure-services toegang tot SQL-server voor zowel SQL Database als SQL Data Warehouse. Zorg ervoor dat de instelling **Toegang tot Azure-services toestaan** voor uw Azure SQL-server op **AAN** staat. Met deze instelling kan de Data Factory-service gegevens lezen uit uw Azure SQL Database en schrijven naar uw Azure SQL Data Warehouse. Voer de volgende stappen uit om dit te controleren en de instelling in te schakelen:

1. Klik op de hub **Meer services** aan de linkerkant en klik op **SQL-servers**.
2. Selecteer uw server en klik op **Firewall** onder **INSTELLINGEN**.
3. Klik op de pagina **Firewallinstellingen** op **AAN** bij **Toegang tot Azure-services toestaan**.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken
1. Start de webbrowser **Microsoft Edge** of **Google Chrome**. Op dit moment wordt de Data Factory-gebruikersinterface alleen ondersteund in de webbrowsers Microsoft Edge en Google Chrome.
1. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/tutorial-bulk-copy-portal/new-azure-data-factory-menu.png)
2. Voer op de pagina **New data factory** **ADFTutorialBulkCopyDF** in bij **name**. 
      
     ![De pagina Nieuwe data factory](./media/tutorial-bulk-copy-portal/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld uwnaamADFTutorialBulkCopyDF) en probeert u het opnieuw. Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
       `Data factory name “ADFTutorialBulkCopyDF” is not available`
3. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
      Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V2 (Preview)** als de **versie**.
5. Selecteer de **locatie** voor de gegevensfactory. Momenteel kunt u in Data Factory V2 alleen data factory's maken in de regio's VS - oost, VS - oost 2 en West-Europa. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.
6. Selecteer **Vastmaken aan dashboard**.     
7. Klik op **Create**.
8. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media//tutorial-bulk-copy-portal/deploying-data-factory.png)
9. Na het aanmaken ziet u de pagina **Data Factory** zoals weergegeven in de afbeelding.
   
    ![Startpagina van de gegevensfactory](./media/tutorial-bulk-copy-portal/data-factory-home-page.png)
10. Klik op **Author & Monitor** om de gebruikersinterface (UI) van Data Factory op een afzonderlijk tabblad te openen.
11. Ga op de pagina **get started** naar het tabblad **Edit** in het linkervenster, zoals wordt weergegeven in de volgende afbeelding:  

    ![Pagina Aan de slag](./media/tutorial-bulk-copy-portal/get-started-page.png)

## <a name="create-linked-services"></a>Gekoppelde services maken
U maakt gekoppelde services om uw gegevensarchieven en compute-services aan een gegevensfactory te koppelen. Een gekoppelde service beschikt over de verbindingsgegevens die de Data Factory-service tijdens runtime gebruikt om een verbinding met het gegevensarchief tot stand te brengen. 

In deze zelfstudie koppelt u uw Azure SQL Database-, Azure SQL Data Warehouse- en Azure Blob Storage-gegevensarchieven aan uw gegevensfactory. De Azure SQL-database fungeert als brongegevensarchief. Azure SQL Data Warehouse fungeert als het sink/doelgegevensarchief. Azure Blob Storage is bedoeld voor het vasthouden van de gegevens voordat deze worden geladen in SQL Data Warehouse met behulp van PolyBase. 

### <a name="create-the-source-azure-sql-database-linked-service"></a>Maak de gekoppelde Azure SQL Database-bronservice.
In deze stap maakt u een gekoppelde service om uw Azure SQL-database aan de gegevensfactory te koppelen. 

1. Klik op **Connections** onderaan het venster en klik op **+ New** op de werkbalk. 

    ![Knop Nieuwe gekoppelde service](./media/tutorial-bulk-copy-portal/new-linked-service-button.png)
2. In het venster **New Linked Service** selecteert u **Azure SQL Database** en klikt u op **Continue**. 

    ![Azure SQL-database selecteren](./media/tutorial-bulk-copy-portal/select-azure-sql-database.png)
3. Voer in het venster **New Linked Service** de volgende stappen uit: 

    1. Voer **AzureSqlDatabaseLinkedService** in bij **Name**. 
    2. Selecteer uw Azure SQL-server bij **Server name**
    3. Selecteer uw Azure SQL-database bij **Database name**. 
    4. Voer **naam van de gebruiker** in om verbinding te maken met de Azure SQL-database. 
    5. Voer het **wachtwoord** voor de gebruiker in. 
    6. Als u de verbinding met de Azure SQL-database wilt testen met de opgegeven informatie, klikt u op **Test connection**.
    7. Klik op **Opslaan**.

        ![Azure SQL Database-instellingen](./media/tutorial-bulk-copy-portal/azure-sql-database-settings.png)

### <a name="create-the-sink-azure-sql-data-warehouse-linked-service"></a>De gekoppelde Azure SQL Data Warehouse-sinkservice maken

1. Op het tabblad **Connections** klikt u nogmaals op **+ New** op de werkbalk. 
2. In het venster **New Linked Service** selecteert u **Azure SQL Data Warehouse** en klikt u op **Continue**. 
3. Voer in het venster **New Linked Service** de volgende stappen uit: 

    1. Voer **AzureStorageLinkedService** in bij **Name**. 
    2. Selecteer uw Azure SQL-server bij **Server name**
    3. Selecteer uw Azure SQL-database bij **Database name**. 
    4. Voer **naam van de gebruiker** in om verbinding te maken met de Azure SQL-database. 
    5. Voer het **wachtwoord** voor de gebruiker in. 
    6. Als u de verbinding met de Azure SQL-database wilt testen met de opgegeven informatie, klikt u op **Test connection**.
    7. Klik op **Opslaan**.

### <a name="create-the-staging-azure-storage-linked-service"></a>De gekoppelde Azure Storage-faseringsservice maken
In deze zelfstudie gebruikt u Azure Blob-opslag als een tussentijds faseringsgebied voor het inschakelen van PolyBase voor betere kopieerprestaties.

1. Op het tabblad **Connections** klikt u nogmaals op **+ New** op de werkbalk. 
2. In het venster **New Linked Service** selecteert u **Azure Blob Storage** en klikt u op **Continue**. 
3. Voer in het venster **New Linked Service** de volgende stappen uit: 

    1. Voer **AzureStorageLinkedService** in bij **Name**. 
    2. Selecteer uw **Azure Storage-account** bij **Storage account name**.
    4. Klik op **Opslaan**.


## <a name="create-datasets"></a>Gegevenssets maken
In deze zelfstudie maakt u bron- en sinkgegevenssets, waarmee de locatie wordt opgegeven waar de gegevens zijn opgeslagen. 

De invoergegevensset AzureSqlDatabaseDataset verwijst naar de AzureSqlDatabaseLinkedService. De gekoppelde service geeft de verbindingsreeks aan om verbinding maken met de database. De gegevensset bevat de naam van de database en de tabel met de brongegevens. 

De uitvoergegevensset AzureSqlDWDataset verwijst naar de AzureSqlDWLinkedService. De gekoppelde service geeft de verbindingsreeks aan om verbinding maken met het datawarehouse. De gegevensset bevat de database en de tabel waarnaar de gegevens worden gekopieerd. 

In deze zelfstudie zijn de bron- en doel-SQL-tabellen niet vastgelegd in de definities van de gegevensset. In plaats daarvan geeft de ForEach-activiteit de naam van de tabel tijdens runtime door aan de Copy-activiteit. 

### <a name="create-a-dataset-for-source-sql-database"></a>Een gegevensset maken voor bron SQL-Database

1. Klik op **+ (plus)** in het linkervenster en klik op **Dataset**. 

    ![Nieuw gegevenssetmenu](./media/tutorial-bulk-copy-portal/new-dataset-menu.png)
2. Selecteer in het venster **Nieuwe gegevensset** de optie **Azure SQL-database** en klik op **Voltooien**. Er is nu een nieuw tabblad met de titel **AzureSqlTable1**. 
    
    ![Azure SQL-database selecteren](./media/tutorial-bulk-copy-portal/select-azure-sql-database-dataset.png)
3. Voer in het eigenschappenvenster onderaan **AzureSqlDatabaseDataset** in als **Name**.

    ![Naam van de brongegevensset](./media/tutorial-bulk-copy-portal/source-dataset-general.png)
4. Open het tabblad **Connection** en voer de volgende stappen uit: 

    1. Selecteer **AzureSqlDatabaseLinkedService** bij **Linked service**.
    2. Selecteer een tabel bij **Table**. Dit is een tijdelijke tabel. U geeft een query voor de brongegevensset op tijdens het maken van een pijplijn. De query wordt gebruikt om gegevens te extraheren uit de Azure SQL-database. U kunt ook het selectievakje **Edit** inschakelen en **dummyName** invoeren als tabelnaam. 

    ![Pagina verbinding brongegevensset](./media/tutorial-bulk-copy-portal/source-dataset-connection-page.png)
 

### <a name="create-a-dataset-for-sink-sql-data-warehouse"></a>Een gegevensset maken voor de sink in SQL Data Warehouse

1. Klik op **+ (plus)** in het linkervenster en klik op **Dataset**. 
2. In het venster **New Dataset** selecteert u **Azure SQL Data Warehouse** en klikt u op **Finish**. Er is nu een nieuw tabblad met de titel **AzureSqlDWTable1**. 
3. Voer in het eigenschappenvenster onderaan **AzureSqlDWDataset** in als **Name**.
4. Open het tabblad **Connection*** en selecteer **AzureSqlDatabaseLinkedService** bij **Linked service**.
5. Ga naar het tabblad **Parameters** en klik op **+ New**

    ![Pagina verbinding brongegevensset](./media/tutorial-bulk-copy-portal/sink-dataset-new-parameter-button.png)
6. Voer **DWTableName** in als parameternaam. Als u deze naam kopieert/plakt vanaf de pagina, moet u ervoor zorgen dat er geen **spatie** volgt na **DWTableName**. 
7. In de sectie **Parameterized properties** voert u `@{dataset().DWTableName}` als **tableName**-eigenschap. De **tableName**-eigenschap van de gegevensset is ingesteld op de waarde die wordt doorgegeven als argument voor de **DWTableName**-parameter. De ForEach-activiteit doorloopt een lijst met tabellen en geeft deze één voor één door aan de Copy-activiteit. 
   
    ![Parameternaam](./media/tutorial-bulk-copy-portal/dwtablename-tablename.png)

## <a name="create-pipelines"></a>Pijplijnen maken
In deze zelfstudie gaat u twee pijplijnen maken: **IterateAndCopySQLTables** en **GetTableListAndTriggerCopyData**. 

De pijplijn **GetTableListAndTriggerCopyData** voert twee stappen uit:

* Zoekt de systeemtabel van Azure SQL Database op om de lijst met te kopiëren tabellen op te halen.
* Activeert de pijplijn **IterateAndCopySQLTables** om het kopiëren van de gegevens daadwerkelijk uit te voeren.

De pijplijn **GetTableListAndTriggerCopyData** gebruikt een lijst met tabellen als parameter. Voor elke tabel in de lijst worden gegevens uit de tabel in Azure SQL Database naar Azure SQL Data Warehouse gekopieerd met behulp van gefaseerd kopiëren en PolyBase.

### <a name="create-the-pipeline-iterateandcopysqltables"></a>De pijplijn IterateAndCopySQLTables maken

1. Klik op **+ (plus)** in het linkervenster en klik op **Pipeline**.

    ![Menu Nieuwe pijplijn](./media/tutorial-bulk-copy-portal/new-pipeline-menu.png)
2. In het venster Properties wijzigt u de naam van de pijplijn in **IterateAndCopySQLTables**. 

    ![Naam pijplijn](./media/tutorial-bulk-copy-portal/first-pipeline-name.png)
3. Open het tabblad **Parameters** en voer de volgende stappen uit: 

    1. Klik op **+ New**. 
    2. Voer **tableList** in als **Name**-parameter.
    3. Selecteer **Object** bij **Type**.

        ![Pijplijnparameter](./media/tutorial-bulk-copy-portal/first-pipeline-parameter.png)
4. Vouw in de werkset **Activities** de optie **Iteration & Conditions** uit en sleep de **ForEach**-activiteit naar het ontwerpoppervlak voor pijplijnen. U kunt ook zoeken naar activiteiten in de werkset **Activities**. Onderaan het venster **Properties** voert u **IterateSQLTables** bij **Name** in. 

    ![ForEach-activiteitnaam](./media/tutorial-bulk-copy-portal/for-each-activity-name.png)
5. Ga naar het tabblad **Settings** en voer `@pipeline().parameters.tableList` in bij **Items**.

    ![Instellingen voor ForEach-activiteit](./media/tutorial-bulk-copy-portal/for-each-activity-settings.png)
6. Als u een onderliggende activiteit wilt toevoegen aan de **ForEach**-activiteit, **dubbelklikt u op** de ForEach-activiteit (of) klikt u op **Edit (potloodpictogram)**. U krijgt de actiekoppelingen voor een activiteit alleen te zien wanneer u de actie selecteert. 

    ![ForEach-activiteitnaam](./media/tutorial-bulk-copy-portal/edit-for-each-activity.png)
7. Vouw in de werkset **Activities** de optie **DataFlow** uit, sleep de **Copy**-activiteit naar het ontwerpoppervlak voor pijplijnen en wijzig de naam in het venster Properties in **CopyData**. Let op het breadcrumb-menu bovenaan. IterateAndCopySQLTable is de naam van de pijplijn en IterateSQLTables is de naam van de ForEach-activiteit. Voor de ontwerpfunctie is het activiteitbereik actief. Als u vanuit de ForEach-editor wilt overschakelen naar de pijplijn-editor, klikt u op de koppeling in het breadcrumb-menu. 

    ![Kopiëren in ForEach](./media/tutorial-bulk-copy-portal/copy-in-for-each.png)
8. Open het tabblad **Source** en voer de volgende stappen uit:

    1. Selecteer **AzureSqlDatabaseDataset** bij **Source Dataset**. 
    2. Selecteer **Query** bij **User Query**. 
    3. Voer bij **Query** de volgende SQL-query in.

        ```sql
        SELECT * FROM [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ``` 

        ![Broninstellingen kopiëren](./media/tutorial-bulk-copy-portal/copy-source-settings.png)
9. Open het tabblad **Sink** en voer de volgende stappen uit: 

    1. Selecteer **AzureSqlDWDataset** bij **Sink Dataset**.
    2. Vouw **Polybase Settings** uit en selecteer **Allow polybase**. 
    3. Schakel de optie **Use Type default** uit. 
    4. Voer het volgende SQL-script in bij **Cleanup Script**. 

        ```sql
        TRUNCATE TABLE [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ```

        ![Sink-instellingen kopiëren](./media/tutorial-bulk-copy-portal/copy-sink-settings.png)
10. Ga naar het tabblad **Parameters**, schuif zo nodig naar beneden om de sectie **Sink Dataset** met de **DWTableName**-parameter weer te geven. Stel de waarde van deze parameter in op `[@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]`.

    ![Sink-parameters kopiëren](./media/tutorial-bulk-copy-portal/copy-sink-parameters.png)
11. Open het tabblad **Settings** en voer de volgende stappen uit: 

    1. Selecteer **True** bij **Enable Staging**.
    2. Selecteer **AzureStorageLinkedService** bij **Store Account Linked Service**.

        ![Faseringsmodus inschakelen](./media/tutorial-bulk-copy-portal/copy-sink-staging-settings.png)
12. Voor het valideren van de pijplijninstellingen klikt u op **Validate**. Controleer of er geen validatiefouten zijn. Sluit het venster **Pipeline Validation Report** door op **>>** te klikken.

    ![Validatierapport voor pijplijn](./media/tutorial-bulk-copy-portal/first-pipeline-validation-report.png)

### <a name="create-the-pipeline-gettablelistandtriggercopydata"></a>De pijplijn GetTableListAndTriggerCopyData maken

Deze pijplijn voert twee stappen uit:

* Zoekt de systeemtabel van Azure SQL Database op om de lijst met te kopiëren tabellen op te halen.
* Activeert de pijplijn 'IterateAndCopySQLTables' om het kopiëren van de gegevens daadwerkelijk uit te voeren.

1. Klik op **+ (plus)** in het linkervenster en klik op **Pipeline**.

    ![Menu Nieuwe pijplijn](./media/tutorial-bulk-copy-portal/new-pipeline-menu.png)
2. In het venster Properties wijzigt u de naam van de pijplijn in **GetTableListAndTriggerCopyData**. 

    ![Naam pijplijn](./media/tutorial-bulk-copy-portal/second-pipeline-name.png)
3. Vouw in de werkset **Activities** de optie **General** uit, sleep de **Lookup**-activiteit naar het ontwerpoppervlak voor pijplijnen en voer de volgende stappen uit:

    1. Voer **LookupTableList** in als **Name**. 
    2. Voer **De tabellijst ophalen uit de Azure SQL-database** in bij **Description**.

        ![Lookup-activiteit - pagina algemeen](./media/tutorial-bulk-copy-portal/lookup-general-page.png)
4. Ga naar de pagina **Settings** en voer de volgende stappen uit:

    1. Selecteer **AzureSqlDatabaseDataset** bij **Source Dataset**. 
    2. Selecteer **Query** bij **Use Query**. 
    3. Voer bij **Query** de volgende SQL-query in.

        ```sql
        SELECT TABLE_SCHEMA, TABLE_NAME FROM information_schema.TABLES WHERE TABLE_TYPE = 'BASE TABLE' and TABLE_SCHEMA = 'SalesLT' and TABLE_NAME <> 'ProductModel'
        ```
    4. Schakel het selectievakje voor het veld **First row only** uit.

        ![Lookup-activiteit - pagina instellingen](./media/tutorial-bulk-copy-portal/lookup-settings-page.png)
5. Sleep de **Execute Pipeline**-activiteit van de werkset Activities naar het ontwerpoppervlak voor pijplijnen en stel de naam in op **TriggerCopy**.

    ![Execute Pipeline-activiteit - pagina algemeen](./media/tutorial-bulk-copy-portal/execute-pipeline-general-page.png)    
6. Ga naar de pagina **Settings** en voer de volgende stappen uit: 

    1. Selecteer **IterateAndCopySQLTables** bij **Invoked pipeline**. 
    2. Vouw de sectie **Advanced** uit. 
    3. Klik op **+ New** in de sectie **Parameters**. 
    4. Voer **tableList** in als **Name**-parameter.
    5. Voer `@activity('LookupTableList').output.value` in als parameter**waarde**. U stelt de lijst met resultaten vanuit de Lookup-activiteit in als invoer voor de tweede pijplijn. De lijst met resultaten bevat de lijst met tabellen waarvan de gegevens naar de bestemming moeten worden gekopieerd. 

        ![Execute Pipeline-activiteit - pagina instellingen](./media/tutorial-bulk-copy-portal/execute-pipeline-settings-page.png)
7. **Verbind** de **Lookup**-activiteit met de **Execute Pipeline**-activiteit door het **groene vakje** dat aan de Lookup-activiteit is gekoppeld, naar de linkerkant van de Execute Pipeline-activiteit te slepen.

    ![Lookup- en Execute Pipeline-activiteiten verbinden](./media/tutorial-bulk-copy-portal/connect-lookup-execute-pipeline.png)
8. Valideer de pijplijn door te klikken op de knop **Validate** op de werkbalk. Controleer of er geen validatiefouten zijn. Sluit het venster **Pipeline Validation Report** door op **>>** te klikken.

    ![Tweede pijplijn - validatierapport](./media/tutorial-bulk-copy-portal/second-pipeline-validation-report.png)
9. Als u entiteiten (gegevenssets, pipelines, enzovoort) wilt publiceren naar de Data Factory-service, klikt u op **Publish All**. Wacht totdat de publicatie is uitgevoerd. 

    ![De knop Publiceren](./media/tutorial-bulk-copy-portal/publish.png)

## <a name="trigger-a-pipeline-run"></a>Een pijplijnuitvoering activeren

1. Controleer of het tabblad **GetTableListAndTriggerCopyData** actief is. 
2. Klik op **Trigger** en klik op **Trigger Now**. 

    ![Nu activeren](./media/tutorial-bulk-copy-portal/trigger-now.png)

## <a name="monitor-the-pipeline-run"></a>De pijplijnuitvoering controleren.

1. Ga naar het tabblad **Controleren**. Klik op **Refresh** totdat u uitvoeringen voor beide pijplijnen in uw oplossing ziet. Blijf de lijst vernieuwen totdat de status **Succeeded** wordt weergegeven. 

    ![Pijplijnuitvoeringen](./media/tutorial-bulk-copy-portal/pipeline-runs.png)
2. Als u uitvoeringen van activiteit wilt weergeven die gekoppeld zijn aan de pijplijn GetTableListAndTriggerCopyData, klikt u op de eerste koppeling in de koppeling Actions voor die pijplijn. Er moeten twee uitvoeringen van activiteit voor deze pijplijnuitvoering te zien zijn. 

    ![Uitvoering van activiteiten](./media/tutorial-bulk-copy-portal/activity-runs-1.png)    
3. Als u de uitvoer van de **Lookup**-activiteit wilt weergeven, klikt u op de koppeling in de kolom **Output** voor die activiteit. U kunt het venster **Output** maximaliseren en herstellen. Nadat u de uitvoer hebt bekeken, klikt u op **X** om het venster **Output** te sluiten.

    ```json
    {
        "count": 9,
        "value": [
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Customer"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductDescription"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Product"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductModelProductDescription"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "ProductCategory"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "Address"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "CustomerAddress"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "SalesOrderDetail"
            },
            {
                "TABLE_SCHEMA": "SalesLT",
                "TABLE_NAME": "SalesOrderHeader"
            }
        ],
        "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (East US)",
        "effectiveIntegrationRuntimes": [
            {
                "name": "DefaultIntegrationRuntime",
                "type": "Managed",
                "location": "East US",
                "billedDuration": 0,
                "nodes": null
            }
        ]
    }
    ```    
4. Als u wilt terugkeren naar de weergave **Pipeline Runs**, klikt u op de koppeling **Pipelines** bovenaan. Klik op **View Activity Runs** (de eerste koppeling in de kolom **Actions**) voor de pijplijn **IterateAndCopySQLTables**. U ziet nu uitvoer zoals weergegeven in de volgende afbeelding: u ziet dat er één **Copy**-uitvoering van activiteit is voor elke tabel in de uitvoer van de **Lookup**-activiteit. 

    ![Uitvoering van activiteiten](./media/tutorial-bulk-copy-portal/activity-runs-2.png)
5. Bevestig dat de gegevens zijn gekopieerd naar het beoogde SQL Data Warehouse dat u in deze zelfstudie hebt gebruikt. 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u de volgende stappen uitgevoerd: 

> [!div class="checklist"]
> * Een data factory maken.
> * Gekoppelde Azure SQL Database-, Azure SQL Data Warehouse- en Azure Storage-services maken.
> * Gegevenssets voor Azure SQL Database en Azure SQL Data Warehouse maken.
> * Een pijplijn maken om de te kopiëren tabellen op te zoeken en een andere pijplijn om de kopieerbewerking daadwerkelijk uit te voeren. 
> * Een pijplijnuitvoering starten.
> * De uitvoering van de pijplijn en van de activiteit controleren.

Ga door naar de volgende zelfstudie voor informatie over het incrementeel kopiëren van gegevens uit een bron naar een bestemming:
> [!div class="nextstepaction"]
>[Gegevens incrementeel kopiëren](tutorial-incremental-copy-portal.md)