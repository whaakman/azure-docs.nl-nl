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
ms.topic: tutorial
ms.date: 06/22/2018
ms.author: jingwang
ms.openlocfilehash: 16741461df2431cbf4433899dd375741e944ce0f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58112565"
---
# <a name="copy-multiple-tables-in-bulk-by-using-azure-data-factory"></a>Meerdere tabellen bulksgewijs kopiëren met behulp van Azure Data Factory
Deze zelfstudie demonstreert het **kopiëren van een aantal tabellen uit Azure SQL Database naar Azure SQL Data Warehouse**. U kunt hetzelfde patroon toepassen in andere kopieerscenario's. Bijvoorbeeld het kopiëren van tabellen van SQL Server/Oracle naar Azure SQL Database/Data Warehouse/Azure Blob, verschillende paden kopiëren van Blob naar Azure SQL Database-tabellen.

> [!NOTE]
> - Zie [Inleiding tot Azure Data Factory](introduction.md) als u niet bekend bent met Azure Data Factory.

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

1. Maak bijbehorende tabelschema's in SQL Data Warehouse. U kunt het [hulpprogramma voor migratie](https://www.microsoft.com/download/details.aspx?id=49100) gebruiken om het **schema te migreren** van Azure SQL Database naar Azure SQL Data Warehouse. U gebruikt Azure Data Factory om gegevens in een latere stap te migreren/kopiëren.

## <a name="azure-services-to-access-sql-server"></a>Azure-services voor toegang tot SQL-server

Geef Azure-services toegang tot SQL-server voor zowel SQL Database als SQL Data Warehouse. Zorg ervoor dat de instelling **Toegang tot Azure-services toestaan** voor uw Azure SQL-server op **AAN** staat. Met deze instelling kan de Data Factory-service gegevens lezen uit uw Azure SQL Database en schrijven naar uw Azure SQL Data Warehouse. Voer de volgende stappen uit om dit te controleren en de instelling in te schakelen:

1. Klik op de hub **Meer services** aan de linkerkant en klik op **SQL-servers**.
1. Selecteer uw server en klik op **Firewall** onder **INSTELLINGEN**.
1. Klik op de pagina **Firewallinstellingen** op **AAN** bij **Toegang tot Azure-services toestaan**.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken
1. Start de webbrowser **Microsoft Edge** of **Google Chrome**. Op dit moment wordt de Data Factory-gebruikersinterface alleen ondersteund in de webbrowsers Microsoft Edge en Google Chrome.
1. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/tutorial-bulk-copy-portal/new-azure-data-factory-menu.png)
1. Voer op de pagina **New data factory** **ADFTutorialBulkCopyDF** in bij **name**. 
      
     ![De pagina Nieuwe data factory](./media/tutorial-bulk-copy-portal/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als u het volgende foutbericht krijgt, wijzigt u de naam van de gegevensfactory (bijvoorbeeld uwnaamADFTutorialBulkCopyDF) en probeert u het opnieuw. Zie het artikel [Data factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
       `Data factory name “ADFTutorialBulkCopyDF” is not available`
1. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
1. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
   - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
   - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
     Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
1. Selecteer **V2** als de **versie**.
1. Selecteer de **locatie** voor de gegevensfactory. Voor een lijst met Azure-regio's waarin Data Factory momenteel beschikbaar is, selecteert u op de volgende pagina de regio's waarin u geïnteresseerd bent, vouwt u vervolgens **Analytics** uit en gaat u naar **Data Factory**: [Beschikbare producten per regio](https://azure.microsoft.com/global-infrastructure/services/). De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.
1. Selecteer **Vastmaken aan dashboard**.     
1. Klik op **Create**.
1. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

     ![tegel met de status 'gegevensfactory implementeren'](media//tutorial-bulk-copy-portal/deploying-data-factory.png)
1. Na het aanmaken ziet u de pagina **Data Factory** zoals weergegeven in de afbeelding.
   
     ![Startpagina van de gegevensfactory](./media/tutorial-bulk-copy-portal/data-factory-home-page.png)
1. Klik op **Author & Monitor** om de gebruikersinterface (UI) van Data Factory op een afzonderlijk tabblad te openen.
1. Ga op de pagina **get started** naar het tabblad **Edit** in het linkervenster, zoals wordt weergegeven in de volgende afbeelding:  

     ![Pagina Aan de slag](./media/tutorial-bulk-copy-portal/get-started-page.png)

## <a name="create-linked-services"></a>Gekoppelde services maken
U maakt gekoppelde services om uw gegevensarchieven en compute-services aan een gegevensfactory te koppelen. Een gekoppelde service beschikt over de verbindingsgegevens die de Data Factory-service tijdens runtime gebruikt om een verbinding met het gegevensarchief tot stand te brengen. 

In deze zelfstudie koppelt u uw Azure SQL Database-, Azure SQL Data Warehouse- en Azure Blob Storage-gegevensarchieven aan uw gegevensfactory. De Azure SQL-database fungeert als brongegevensarchief. Azure SQL Data Warehouse fungeert als het sink/doelgegevensarchief. Azure Blob Storage is bedoeld voor het vasthouden van de gegevens voordat deze worden geladen in SQL Data Warehouse met behulp van PolyBase. 

### <a name="create-the-source-azure-sql-database-linked-service"></a>Maak de gekoppelde Azure SQL Database-bronservice.
In deze stap maakt u een gekoppelde service om uw Azure SQL-database aan de gegevensfactory te koppelen. 

1. Klik op **Connections** onderaan het venster en klik op **+ New** op de werkbalk. 

    ![Knop Nieuwe gekoppelde service](./media/tutorial-bulk-copy-portal/new-linked-service-button.png)
1. In het venster **New Linked Service** selecteert u **Azure SQL Database** en klikt u op **Continue**. 

    ![Azure SQL-database selecteren](./media/tutorial-bulk-copy-portal/select-azure-sql-database.png)
1. Voer in het venster **New Linked Service** de volgende stappen uit: 

    1. Voer **AzureSqlDatabaseLinkedService** in bij **Name**. 
    1. Selecteer uw Azure SQL-server bij **Server name**
    1. Selecteer uw Azure SQL-database bij **Database name**. 
    1. Voer **naam van de gebruiker** in om verbinding te maken met de Azure SQL-database. 
    1. Voer het **wachtwoord** voor de gebruiker in. 
    1. Als u de verbinding met de Azure SQL-database wilt testen met de opgegeven informatie, klikt u op **Test connection**.
    1. Klik op **Opslaan**.

        ![Azure SQL Database-instellingen](./media/tutorial-bulk-copy-portal/azure-sql-database-settings.png)

### <a name="create-the-sink-azure-sql-data-warehouse-linked-service"></a>De gekoppelde Azure SQL Data Warehouse-sinkservice maken

1. Op het tabblad **Connections** klikt u nogmaals op **+ New** op de werkbalk. 
1. In het venster **New Linked Service** selecteert u **Azure SQL Data Warehouse** en klikt u op **Continue**. 
1. Voer in het venster **New Linked Service** de volgende stappen uit: 

    1. Voer **AzureStorageLinkedService** in bij **Name**. 
    1. Selecteer uw Azure SQL-server bij **Server name**
    1. Selecteer uw Azure SQL-database bij **Database name**. 
    1. Voer **naam van de gebruiker** in om verbinding te maken met de Azure SQL-database. 
    1. Voer het **wachtwoord** voor de gebruiker in. 
    1. Als u de verbinding met de Azure SQL-database wilt testen met de opgegeven informatie, klikt u op **Test connection**.
    1. Klik op **Opslaan**.

### <a name="create-the-staging-azure-storage-linked-service"></a>De gekoppelde Azure Storage-faseringsservice maken
In deze zelfstudie gebruikt u Azure Blob-opslag als een tussentijds faseringsgebied voor het inschakelen van PolyBase voor betere kopieerprestaties.

1. Op het tabblad **Connections** klikt u nogmaals op **+ New** op de werkbalk. 
1. In het venster **New Linked Service** selecteert u **Azure Blob Storage** en klikt u op **Continue**. 
1. Voer in het venster **New Linked Service** de volgende stappen uit: 

    1. Voer **AzureStorageLinkedService** in bij **Name**. 
    1. Selecteer uw **Azure Storage-account** bij **Storage account name**.
    1. Klik op **Opslaan**.


## <a name="create-datasets"></a>Gegevenssets maken
In deze zelfstudie maakt u bron- en sinkgegevenssets, waarmee de locatie wordt opgegeven waar de gegevens zijn opgeslagen. 

De invoergegevensset **AzureSqlDatabaseDataset** verwijst naar de **AzureSqlDatabaseLinkedService**. De gekoppelde service geeft de verbindingsreeks aan om verbinding maken met de database. De gegevensset bevat de naam van de database en de tabel met de brongegevens. 

De uitvoergegevensset **AzureSqlDWDataset** verwijst naar de **AzureSqlDWLinkedService**. De gekoppelde service geeft de verbindingsreeks aan om verbinding maken met het datawarehouse. De gegevensset bevat de database en de tabel waarnaar de gegevens worden gekopieerd. 

In deze zelfstudie zijn de bron- en doel-SQL-tabellen niet vastgelegd in de definities van de gegevensset. In plaats daarvan geeft de ForEach-activiteit de naam van de tabel tijdens runtime door aan de Copy-activiteit. 

### <a name="create-a-dataset-for-source-sql-database"></a>Een gegevensset maken voor bron SQL-Database

1. Klik op **+ (plus)** in het linkervenster en klik op **Dataset**. 

    ![Nieuw gegevenssetmenu](./media/tutorial-bulk-copy-portal/new-dataset-menu.png)
1. Selecteer in het venster **Nieuwe gegevensset** de optie **Azure SQL-database** en klik op **Voltooien**. Er is nu een nieuw tabblad met de titel **AzureSqlTable1**. 
    
    ![Azure SQL-database selecteren](./media/tutorial-bulk-copy-portal/select-azure-sql-database-dataset.png)
1. Voer in het eigenschappenvenster onderaan **AzureSqlDatabaseDataset** in als **Name**.

1. Ga naar het tabblad **Verbinding** en voer de volgende stappen uit: 

   1. Selecteer **AzureSqlDatabaseLinkedService** bij **Linked service**.
   1. Selecteer een tabel bij **Table**. Dit is een tijdelijke tabel. U geeft een query voor de brongegevensset op tijdens het maken van een pijplijn. De query wordt gebruikt om gegevens te extraheren uit de Azure SQL-database. U kunt ook het selectievakje **Edit** inschakelen en **dummyName** invoeren als tabelnaam. 

      ![Pagina verbinding brongegevensset](./media/tutorial-bulk-copy-portal/source-dataset-connection-page.png)
 

### <a name="create-a-dataset-for-sink-sql-data-warehouse"></a>Een gegevensset maken voor de sink in SQL Data Warehouse

1. Klik op **+ (plus)** in het linkervenster en klik op **Dataset**. 
1. In het venster **New Dataset** selecteert u **Azure SQL Data Warehouse** en klikt u op **Finish**. Er is nu een nieuw tabblad met de titel **AzureSqlDWTable1**. 
1. Voer in het eigenschappenvenster onderaan **AzureSqlDWDataset** in als **Name**.
1. Ga naar het tabblad **Parameters**, klik op **+ Nieuw** en voer **DWTableName** in als de parameternaam. Als u deze naam kopieert/plakt vanaf de pagina, moet u ervoor zorgen dat er geen **spatie** volgt na **DWTableName**. 

    ![Pagina verbinding brongegevensset](./media/tutorial-bulk-copy-portal/sink-dataset-new-parameter.png)

1. Ga naar het tabblad **Verbinding**, 

    a. Selecteer **AzureSqlDatabaseLinkedService** als **Gekoppelde service**.

    b. Schakel voor **Tabel** de optie **Bewerken**in, klik op het invoervak van de tabelnaam en klik op de onderstaande koppeling **Dynamische inhoud toevoegen**. 
    
    ![Parameternaam](./media/tutorial-bulk-copy-portal/table-name-parameter.png)

    c. Klik op de pagina **Dynamische inhoud toevoegen** op de **DWTAbleName** onder **Parameters**. Het expressietekstvak wordt nu automatisch ingevuld`@dataset().DWTableName`. Klik vervolgens op **Voltooien**. De **tableName**-eigenschap van de gegevensset is ingesteld op de waarde die wordt doorgegeven als argument voor de **DWTableName**-parameter. De ForEach-activiteit doorloopt een lijst met tabellen en geeft deze één voor één door aan de Copy-activiteit. 

    ![Opbouwfunctie voor gegevenssetparameters](./media/tutorial-bulk-copy-portal/dataset-parameter-builder.png)

## <a name="create-pipelines"></a>Pijplijnen maken
In deze zelfstudie maakt u twee pijplijnen: **IterateAndCopySQLTables** en **GetTableListAndTriggerCopyData**. 

De pijplijn **GetTableListAndTriggerCopyData** voert twee stappen uit:

* Zoekt de systeemtabel van Azure SQL Database op om de lijst met te kopiëren tabellen op te halen.
* Activeert de pijplijn **IterateAndCopySQLTables** om het kopiëren van de gegevens daadwerkelijk uit te voeren.

De pijplijn **GetTableListAndTriggerCopyData** gebruikt een lijst met tabellen als parameter. Voor elke tabel in de lijst worden gegevens uit de tabel in Azure SQL Database naar Azure SQL Data Warehouse gekopieerd met behulp van gefaseerd kopiëren en PolyBase.

### <a name="create-the-pipeline-iterateandcopysqltables"></a>De pijplijn IterateAndCopySQLTables maken

1. Klik op **+ (plus)** in het linkervenster en klik op **Pipeline**.

    ![Menu Nieuwe pijplijn](./media/tutorial-bulk-copy-portal/new-pipeline-menu.png)
1. Geef op het tabblad **Algemeen** **IterateAndCopySQLTables** op als naam. 

1. Open het tabblad **Parameters** en voer de volgende stappen uit: 

    1. Klik op **+ New**. 
    1. Voer **tableList** in als **Name**-parameter.
    1. Selecteer **Matrix** bij **Type**.

        ![Pijplijnparameter](./media/tutorial-bulk-copy-portal/first-pipeline-parameter.png)
1. Vouw in de werkset **Activities** de optie **Iteration & Conditions** uit en sleep de **ForEach**-activiteit naar het ontwerpoppervlak voor pijplijnen. U kunt ook zoeken naar activiteiten in de werkset **Activiteiten**. 

    a. Voer onderaan het tabblad **Algemeen** **IterateSQLTables** bij **Naam** in. 

    b. Ga naar het tabblad **Instellingen**, klik op het invoervak voor **Items** en klik vervolgens op de onderstaande koppeling **Dynamische inhoud toevoegen**. 

    ![Instellingen voor ForEach-activiteit](./media/tutorial-bulk-copy-portal/for-each-activity-settings.png)

    c. Vouw op de pagina **Dynamische inhoud toevoegen** de sectie Systeemvariabelen en functies samen en klik op de **tableList** onder **Parameters**. Het expressietekstvak wordt nu automatisch ingevuld`@pipeline().parameter.tableList`. Klik vervolgens op **Voltooien**. 

    ![Opbouwfunctie voor Foreach-parameters](./media/tutorial-bulk-copy-portal/for-each-parameter-builder.png)
    
    d. Ga naar het tabblad **Activiteiten** en klik op **Activiteit toevoegen** om een onderliggende activiteit toe te voegen aan de activiteit **ForEach**.

1. Vouw in de **Activiteiten**-werkset de optie **Gegevensstroom** uit. Gebruik vervolgens slepen-en-neerzetten om de **kopieeractiviteit** te verplaatsen naar het ontwerpoppervlak voor pijplijnen. Let op het breadcrumb-menu bovenaan. IterateAndCopySQLTable is de naam van de pijplijn en IterateSQLTables is de naam van de ForEach-activiteit. Voor de ontwerpfunctie is het activiteitbereik actief. Als u vanuit de ForEach-editor wilt overschakelen naar de pijplijn-editor, klikt u op de koppeling in het breadcrumb-menu. 

    ![Kopiëren in ForEach](./media/tutorial-bulk-copy-portal/copy-in-for-each.png)
1. Open het tabblad **Source** en voer de volgende stappen uit:

    1. Selecteer **AzureSqlDatabaseDataset** bij **Source Dataset**. 
    1. Selecteer **Query** bij **User Query**. 
    1. Klik op het invoervak **Query** -> Selecteer de koppeling **Dynamische inhoud toevoegen** hieronder -> voer de volgende expressie in voor **Query** -> selecteer **Voltooien**.

        ```sql
        SELECT * FROM [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ``` 

        ![Broninstellingen kopiëren](./media/tutorial-bulk-copy-portal/copy-source-settings.png)
1. Open het tabblad **Sink** en voer de volgende stappen uit: 

    1. Selecteer **AzureSqlDWDataset** bij **Sink Dataset**.
    1. Klik op het invoervak voor VALUE van de parameter DWTableName -> Selecteer de koppeling **Dynamische inhoud toevoegen** hieronder, voer de expressie `[@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]` als script in -> Selecteer **voltooien**.
    1. Vouw **Polybase Settings** uit en selecteer **Allow polybase**. 
    1. Schakel de optie **Use Type default** uit. 
    1. Klik op het invoervak **Prekopieerscript** -> Selecteer de koppeling **Dynamische inhoud toevoegen** hieronder -> voer de volgende expressie als script in -> selecteer **Voltooien**. 

        ```sql
        TRUNCATE TABLE [@{item().TABLE_SCHEMA}].[@{item().TABLE_NAME}]
        ```

        ![Sink-instellingen kopiëren](./media/tutorial-bulk-copy-portal/copy-sink-settings.png)

1. Ga naar het tabblad **Instellingen** en voer de volgende stappen uit: 

    1. Selecteer **True** bij **Enable Staging**.
    1. Selecteer **AzureStorageLinkedService** bij **Store Account Linked Service**.

        ![Faseringsmodus inschakelen](./media/tutorial-bulk-copy-portal/copy-sink-staging-settings.png)

1. Klik in de bovenste pijplijnwerkbalk op **Valideren** om de instellingen voor de pijplijn te valideren. Controleer of er geen validatiefouten zijn. Sluit het venster **Pipeline Validation Report** door op **>>** te klikken.

### <a name="create-the-pipeline-gettablelistandtriggercopydata"></a>De pijplijn GetTableListAndTriggerCopyData maken

Deze pijplijn voert twee stappen uit:

* Zoekt de systeemtabel van Azure SQL Database op om de lijst met te kopiëren tabellen op te halen.
* Activeert de pijplijn 'IterateAndCopySQLTables' om het kopiëren van de gegevens daadwerkelijk uit te voeren.

1. Klik op **+ (plus)** in het linkervenster en klik op **Pipeline**.

    ![Menu Nieuwe pijplijn](./media/tutorial-bulk-copy-portal/new-pipeline-menu.png)
1. In het venster Properties wijzigt u de naam van de pijplijn in **GetTableListAndTriggerCopyData**. 

1. Vouw in de werkset **Activities** de optie **General** uit, sleep de **Lookup**-activiteit naar het ontwerpoppervlak voor pijplijnen en voer de volgende stappen uit:

    1. Voer **LookupTableList** in als **Name**. 
    1. Voer **De tabellijst ophalen uit de Azure SQL-database** in bij **Description**.

        ![Lookup-activiteit - pagina algemeen](./media/tutorial-bulk-copy-portal/lookup-general-page.png)
1. Ga naar de pagina **Settings** en voer de volgende stappen uit:

    1. Selecteer **AzureSqlDatabaseDataset** bij **Source Dataset**. 
    1. Selecteer **Query** bij **Use Query**. 
    1. Voer bij **Query** de volgende SQL-query in.

        ```sql
        SELECT TABLE_SCHEMA, TABLE_NAME FROM information_schema.TABLES WHERE TABLE_TYPE = 'BASE TABLE' and TABLE_SCHEMA = 'SalesLT' and TABLE_NAME <> 'ProductModel'
        ```
    1. Schakel het selectievakje voor het veld **First row only** uit.

        ![Lookup-activiteit - pagina instellingen](./media/tutorial-bulk-copy-portal/lookup-settings-page.png)
1. Sleep de **Execute Pipeline**-activiteit van de werkset Activities naar het ontwerpoppervlak voor pijplijnen en stel de naam in op **TriggerCopy**.

    ![Execute Pipeline-activiteit - pagina algemeen](./media/tutorial-bulk-copy-portal/execute-pipeline-general-page.png)    
1. Ga naar de pagina **Settings** en voer de volgende stappen uit: 

    1. Selecteer **IterateAndCopySQLTables** bij **Invoked pipeline**. 
    1. Vouw de sectie **Advanced** uit. 
    1. Klik op **+ New** in de sectie **Parameters**. 
    1. Voer **tableList** in als **Name**-parameter.
    1. Klik op het invoervak VALUE -> selecteer de koppeling **Dynamische inhoud toevoegen** hieronder - voer `@activity('LookupTableList').output.value` als waarde voor de tabelnaam -> selecteer **Voltooien**. U stelt de lijst met resultaten vanuit de Lookup-activiteit in als invoer voor de tweede pijplijn. De lijst met resultaten bevat de lijst met tabellen waarvan de gegevens naar de bestemming moeten worden gekopieerd. 

        ![Execute Pipeline-activiteit - pagina instellingen](./media/tutorial-bulk-copy-portal/execute-pipeline-settings-page.png)
1. **Verbind** de **Lookup**-activiteit met de **Execute Pipeline**-activiteit door het **groene vakje** dat aan de Lookup-activiteit is gekoppeld, naar de linkerkant van de Execute Pipeline-activiteit te slepen.

    ![Lookup- en Execute Pipeline-activiteiten verbinden](./media/tutorial-bulk-copy-portal/connect-lookup-execute-pipeline.png)
1. Valideer de pijplijn door te klikken op de knop **Validate** op de werkbalk. Controleer of er geen validatiefouten zijn. Sluit het venster **Pipeline Validation Report** door op **>>** te klikken.

1. Als u entiteiten (gegevenssets, pijplijnen, enzovoort) wilt publiceren naar de Data Factory-service, klikt u boven aan het venster op **Alles publiceren**. Wacht totdat de publicatie is uitgevoerd. 

## <a name="trigger-a-pipeline-run"></a>Een pijplijnuitvoering activeren

Ga naar de pijplijn **GetTableListAndTriggerCopyData**, klik op **Activeren** en klik op **Nu activeren**. 

![Nu activeren](./media/tutorial-bulk-copy-portal/trigger-now.png)

## <a name="monitor-the-pipeline-run"></a>De pijplijnuitvoering controleren.

1. Ga naar het tabblad **Controleren**. Klik op **Refresh** totdat u uitvoeringen voor beide pijplijnen in uw oplossing ziet. Blijf de lijst vernieuwen totdat de status **Succeeded** wordt weergegeven. 

    ![Pijplijnuitvoeringen](./media/tutorial-bulk-copy-portal/pipeline-runs.png)
1. Als u uitvoeringen van activiteit wilt weergeven die gekoppeld zijn aan de pijplijn GetTableListAndTriggerCopyData, klikt u op de eerste koppeling in de koppeling Actions voor die pijplijn. Er moeten twee uitvoeringen van activiteit voor deze pijplijnuitvoering te zien zijn. 

    ![Uitvoering van activiteiten](./media/tutorial-bulk-copy-portal/activity-runs-1.png)    
1. Als u de uitvoer van de **Lookup**-activiteit wilt weergeven, klikt u op de koppeling in de kolom **Output** voor die activiteit. U kunt het venster **Output** maximaliseren en herstellen. Nadat u de uitvoer hebt bekeken, klikt u op **X** om het venster **Output** te sluiten.

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
1. Als u wilt terugkeren naar de weergave **Pipeline Runs**, klikt u op de koppeling **Pipelines** bovenaan. Klik op **View Activity Runs** (de eerste koppeling in de kolom **Actions**) voor de pijplijn **IterateAndCopySQLTables**. De uitvoer wordt weergegeven zoals in de volgende afbeelding: U ziet dat er één **kopieer**activiteit is uitgevoerd voor elke tabel in de uitvoer van de **opzoek**activiteit. 

    ![Uitvoering van activiteiten](./media/tutorial-bulk-copy-portal/activity-runs-2.png)
1. Bevestig dat de gegevens zijn gekopieerd naar het beoogde SQL Data Warehouse dat u in deze zelfstudie hebt gebruikt. 

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
