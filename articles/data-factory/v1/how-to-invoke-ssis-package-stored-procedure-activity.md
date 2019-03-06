---
title: Aanroepen van SSIS-pakket met Azure Data Factory - activiteit opgeslagen Procedure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een pakket met SQL Server Integration Services (SSIS) van een Azure Data Factory-pijplijn met behulp van de activiteit opgeslagen Procedure aanroepen.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: jingwang
ms.openlocfilehash: 859cd6cfd3db68dad2607f1dc8905facb43dd290
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57453768"
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Aanroepen van een SSIS-pakket met behulp van opgeslagen procedure-activiteit in Azure Data Factory
In dit artikel wordt beschreven hoe u een SSIS-pakket van een Azure Data Factory-pijplijn aanroepen met behulp van een opgeslagen procedure-activiteit. 

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [aanroepen SSIS-pakketten met behulp van opgeslagen procedure-activiteit in](../how-to-invoke-ssis-package-stored-procedure-activity.md).

## <a name="prerequisites"></a>Vereisten

### <a name="azure-sql-database"></a>Azure SQL Database 
De procedures in dit artikel maakt gebruik van een Azure SQL-database die als host fungeert voor de SSIS-catalogus. U kunt ook een Azure SQL Database Managed Instance.

### <a name="create-an-azure-ssis-integration-runtime"></a>Een Azure SSIS Integration Runtime maken
Een Azure-SSIS integratieruntime maken als u geen volgt u de stapsgewijze instructies in de [zelfstudie: SSIS-pakketten implementeren](../tutorial-create-azure-ssis-runtime-portal.md). U kunt Data Factory versie 1 niet gebruiken voor het maken van een Azure-SSIS integratieruntime. 

## <a name="azure-portal"></a>Azure Portal
In deze sectie gebruikt u de Azure-portal te maken van een Data Factory-pijplijn met een opgeslagen procedure-activiteit die een SSIS-pakket aanroept.

### <a name="create-a-data-factory"></a>Een gegevensfactory maken
Eerste stap is het maken van een data factory met behulp van de Azure-portal. 

1. Navigeer naar [Azure Portal](https://portal.azure.com). 
2. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory-menu.png)
2. Voer op de pagina **Nieuwe data factory** **ADFTutorialDataFactory** in als **naam**. 
      
     ![De pagina Nieuwe data factory](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als het volgende foutbericht wordt weergegeven voor het naamveld, wijzigt u de naam van de data factory (bijvoorbeeld uwnaamADFTutorialDataFactory). Zie het artikel [Data factory - Naamgevingsregels](data-factory-naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.

    `Data factory name ADFTutorialDataFactory is not available`
3. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
    Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V1** voor de **versie**.
5. Selecteer de **locatie** voor de gegevensfactory. In de vervolgkeuzelijst ziet u alleen locaties die worden ondersteund in Data Factory. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich op andere locaties bevinden.
6. Selecteer **Vastmaken aan dashboard**.     
7. Klik op **Create**.
8. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media//how-to-invoke-ssis-package-stored-procedure-activity/deploying-data-factory.png)
9. Na het aanmaken ziet u de pagina **Data Factory** zoals weergegeven in de afbeelding.
   
    ![Startpagina van de gegevensfactory](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)
10. Klik op **auteur en implementeer** tegel om te starten van de Data Factory-Editor.

    ![Data Factory Editor](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-editor.png)

### <a name="create-an-azure-sql-database-linked-service"></a>Een gekoppelde Azure SQL Database-service maken
Een gekoppelde service koppelt uw Azure SQL-database die als host fungeert de SSIS-catalogus aan uw data factory maken. Data Factory maakt gebruik van informatie in deze gekoppelde service verbinding maken met de SSISDB-database en voert een opgeslagen procedure voor het uitvoeren van een SSIS-pakket. 

1. In de Data Factory-Editor, klikt u op **nieuw gegevensarchief** in het menu en klik op **Azure SQL Database**. 

    ![Nieuwe gegevensopslag-Azure SQL Database >](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-sql-database-linked-service-menu.png)
2. Voer in het rechter deelvenster de volgende stappen uit:

    1. Vervang `<servername>` met de naam van uw Azure SQL-server. 
    2. Vervang `<databasename>` met **SSISDB** (naam van de SSIS-catalogusdatabase). 
    3. Vervang `<username@servername>` met de naam van de gebruiker die toegang tot de Azure SQL-server heeft. 
    4. Vervang `<password>` met het wachtwoord voor de gebruiker. 
    5. De gekoppelde service implementeren door te klikken op de **implementeren** op de werkbalk. 

        ![Een gekoppelde Azure SQL Database-service](./media/how-to-invoke-ssis-package-stored-procedure-activity/azure-sql-database-linked-service-definition.png)

### <a name="create-a-dummy-dataset-for-output"></a>Een dummy gegevensset voor uitvoer maken
Dit wordt een dummy-gegevensset die het schema van de pijplijn stuurt. U ziet dat de frequentie is ingesteld op elk uur en interval is ingesteld op 1. Daarom de pijplijn eenmaal per uur in de pijplijn start- en eindtijden uitgevoerd. 

1. Klik in het linkerdeelvenster van de Data Factory-Editor op **... Meer** -> **nieuwe gegevensset** -> **Azure SQL**.

    ![Meer nieuwe gegevensset ->](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-dataset-menu.png)
2. Kopieer de volgende JSON-fragment in de JSON-editor in het rechter deelvenster. 
    
    ```json
    {
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": { },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
3. Klik op **Implementeren** op de werkbalk. Deze actie wordt de gegevensset geïmplementeerd naar de Azure Data Factory-service. 

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Een pijplijn maken met de opgeslagen procedure-activiteit 
In deze stap maakt maken u een pijplijn met een opgeslagen procedure-activiteit. De activiteit roept de sp_executesql opgeslagen procedure voor het uitvoeren van uw SSIS-pakket. 

1. Klik in het linkerdeelvenster op **... Meer** en vervolgens op **Nieuwe pijplijn**.
2. Kopieer de volgende JSON-fragment in de JSON-editor: 

    > [!IMPORTANT]
    > Vervang &lt;mapnaam&gt;, &lt;projectnaam&gt;, &lt;pakketnaam&gt; door de namen van mappen, project en -pakket in de SSIS-catalogus voordat u het bestand opslaat.

    ```json
    {
        "name": "MyPipeline",
        "properties": {
            "activities": [{
                "name": "SprocActivitySample",
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_executesql",
                    "storedProcedureParameters": {
                        "stmt": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<folder name>', @project_name=N'<project name>', @package_name=N'<package name>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                    }
                },
                "outputs": [{
                    "name": "sprocsampleout"
                }],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }],
            "start": "2018-01-19T00:00:00Z",
            "end": "2018-01-19T05:00:00Z",
            "isPaused": false
        }
    }    
    ```
3. Klik op **Implementeren** op de werkbalk. Deze actie wordt de pijplijn geïmplementeerd naar de Azure Data Factory-service. 

### <a name="monitor-the-pipeline-run"></a>De pijplijnuitvoering controleren.
De planning op de uitvoergegevensset wordt per uur gedefinieerd. De eindtijd van de pijplijn is vijf uur van de starttijd. Daarom ziet u vijf pijplijnuitvoeringen. 

1. Sluit de editor voor windows, zodat u de startpagina van de data factory ziet. Klik op **bewaking en beheer** tegel. 

    ![Tegel Diagram](./media/how-to-invoke-ssis-package-stored-procedure-activity/monitor-manage-tile.png)
2. Update de **begintijd** en **eindtijd** naar **18-01-2018 08:30 uur** en **20-01-2018 08:30 uur**, en klikt u op **toepassen**. U ziet de **activiteitsvensters** die zijn gekoppeld aan de pijplijnuitvoering. 

    ![Activiteit windows](./media/how-to-invoke-ssis-package-stored-procedure-activity/activity-windows.png)

Zie voor meer informatie over het controleren van pijplijnen [bewaken en beheren van Azure Data Factory-pijplijnen met behulp van de bewaking en Beheerapp](data-factory-monitor-manage-app.md).

## <a name="azure-powershell"></a>Azure PowerShell
In deze sectie kunt u Azure PowerShell gebruiken voor het maken van een Data Factory-pijplijn met een opgeslagen procedure-activiteit die een SSIS-pakket aanroept.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Installeer de nieuwste Azure PowerShell-modules met de instructies in [Azure PowerShell installeren en configureren](/powershell/azure/install-az-ps).

### <a name="create-a-data-factory"></a>Een gegevensfactory maken
De volgende procedure bevat stappen voor het maken van een data factory. U maakt een pijplijn met een activiteit opgeslagen procedure in deze data factory. De activiteit opgeslagen procedure wordt een opgeslagen procedure uitgevoerd in de SSISDB-database om uit te voeren van uw SSIS-pakket.

1. Definieer een variabele voor de naam van de resourcegroep die u later gaat gebruiken in PowerShell-opdrachten. Kopieer de tekst van de volgende opdracht naar PowerShell, geef tussen dubbele aanhalingstekens een naam op voor de [Azure-resourcegroep](../../azure-resource-manager/resource-group-overview.md) en voer de opdracht uit. Bijvoorbeeld: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    Als de resourcegroep al bestaat, wilt u waarschijnlijk niet dat deze wordt overschreven. Wijs een andere waarde toe aan de `$ResourceGroupName`-variabele en voer de opdracht opnieuw uit.
2. Voer de volgende opdracht uit om de resourcegroep te maken: 

    ```powershell
    $ResGrp = New-AzResourceGroup $resourceGroupName -location 'eastus'
    ``` 
    Als de resourcegroep al bestaat, wilt u waarschijnlijk niet dat deze wordt overschreven. Wijs een andere waarde toe aan de `$ResourceGroupName`-variabele en voer de opdracht opnieuw uit. 
3. Definieer een variabele voor de naam van de data factory. 

    > [!IMPORTANT]
    >  Werk de naam van de data factory zodanig bij dat deze uniek is. 

    ```powershell
    $DataFactoryName = "ADFTutorialFactory";
    ```

5. Voor het maken van de data factory, voer de volgende **New-AzDataFactory** cmdlet, met behulp van de eigenschap Location en ResourceGroupName van de variabele $ResGrp: 
    
    ```powershell       
    $df = New-AzDataFactory -ResourceGroupName $ResourceGroupName -Name $dataFactoryName -Location "East US"
    ```

Houd rekening met de volgende punten:

* De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als de volgende fout zich voordoet, wijzigt u de naam en probeert u het opnieuw.

    ```
    The specified Data Factory name 'ADFTutorialFactory' is already in use. Data Factory names must be globally unique.
    ```
* Als u Data Factory-exemplaren wilt maken, moet het gebruikersaccount waarmee u zich bij Azure aanmeldt, lid zijn van de rollen **Inzender** of **Eigenaar**, of moet dit een **beheerder** van het Azure-abonnement zijn.

### <a name="create-an-azure-sql-database-linked-service"></a>Een gekoppelde Azure SQL Database-service maken
Een gekoppelde service koppelt uw Azure SQL-database die als host fungeert de SSIS-catalogus aan uw data factory maken. Data Factory maakt gebruik van informatie in deze gekoppelde service verbinding maken met de SSISDB-database en voert een opgeslagen procedure voor het uitvoeren van een SSIS-pakket. 

1. Maak een JSON-bestand met de naam **AzureSqlDatabaseLinkedService.json** in **C:\ADF\RunSSISPackage** map met de volgende inhoud: 

    > [!IMPORTANT]
    > Vervang &lt;servername&gt;, &lt;gebruikersnaam&gt;@&lt;servername&gt; en &lt;wachtwoord&gt; met waarden van uw Azure SQL-Database voordat u Als u het bestand opslaat.

    ```json
    {
        "name": "AzureSqlDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<servername>.database.windows.net,1433;Database=SSISDB;User ID=<username>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
        }
    ```
2. In **Azure PowerShell**, Ga naar de **C:\ADF\RunSSISPackage** map.
3. Voer de **New-AzDataFactoryLinkedService** cmdlet voor het maken van de gekoppelde service: **AzureSqlDatabaseLinkedService**. 

    ```powershell
    New-AzDataFactoryLinkedService $df -File ".\AzureSqlDatabaseLinkedService.json"
    ```

### <a name="create-an-output-dataset"></a>Een uitvoergegevensset maken
Dit wordt een dummy-gegevensset die het schema van de pijplijn stuurt. U ziet dat de frequentie is ingesteld op elk uur en interval is ingesteld op 1. Daarom de pijplijn eenmaal per uur in de pijplijn start- en eindtijden uitgevoerd. 

1. Maak een OutputDataset.json-bestand met de volgende inhoud: 
    
    ```json
    {
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": { },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
2. Voer de **New-AzDataFactoryDataset** cmdlet om een gegevensset te maken. 

    ```powershell
    New-AzDataFactoryDataset $df -File ".\OutputDataset.json"
    ```

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Een pijplijn maken met de opgeslagen procedure-activiteit 
In deze stap maakt maken u een pijplijn met een opgeslagen procedure-activiteit. De activiteit roept de sp_executesql opgeslagen procedure voor het uitvoeren van uw SSIS-pakket. 

1. Maak een JSON-bestand met de naam **MyPipeline.json** in de **C:\ADF\RunSSISPackage** map met de volgende inhoud:

    > [!IMPORTANT]
    > Vervang &lt;mapnaam&gt;, &lt;projectnaam&gt;, &lt;pakketnaam&gt; door de namen van mappen, project en -pakket in de SSIS-catalogus voordat u het bestand opslaat.

    ```json
    {
        "name": "MyPipeline",
        "properties": {
            "activities": [{
                "name": "SprocActivitySample",
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_executesql",
                    "storedProcedureParameters": {
                        "stmt": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<folder name>', @project_name=N'<project name>', @package_name=N'<package name>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                    }
                },
                "outputs": [{
                    "name": "sprocsampleout"
                }],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                }
            }],
            "start": "2017-10-01T00:00:00Z",
            "end": "2017-10-01T05:00:00Z",
            "isPaused": false
        }
    }    
    ```

2. De pijplijn maken: **RunSSISPackagePipeline**, voert de **New-AzDataFactoryPipeline** cmdlet.

    ```powershell
    $DFPipeLine = New-AzDataFactoryPipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

### <a name="monitor-the-pipeline-run"></a>De pijplijnuitvoering controleren.

2. Voer **Get-AzDataFactorySlice** voor meer informatie over alle segmenten van de uitvoer gegevensset **, is de uitvoertabel van de pijplijn.

    ```PowerShell
    Get-AzDataFactorySlice $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```
    De StartDateTime die u hier opgeeft, is dezelfde begintijd die u hebt opgegeven in de JSON van de pijplijn. 
3. Voer **Get-AzDataFactoryRun** om op te halen van de details van activiteit actief is gedurende een bepaald segment.

    ```PowerShell
    Get-AzDataFactoryRun $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```

    U kunt deze cmdlet blijven uitvoeren tot u ziet dat het segment de status **Gereed** of **Mislukt** heeft gekregen. 

    U kunt de volgende query uitvoeren op basis van de SSISDB-database in uw Azure SQL-server om te controleren of het pakket dat wordt uitgevoerd. 

    ```sql
    select * from catalog.executions
    ```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de activiteit opgeslagen procedure de [Stored Procedure-activiteit](data-factory-stored-proc-activity.md) artikel.

