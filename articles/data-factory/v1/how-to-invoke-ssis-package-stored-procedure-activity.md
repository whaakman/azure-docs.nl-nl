---
title: Aanroepen van SSIS-pakket met behulp van Azure Data Factory - activiteit opgeslagen Procedure | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een pakket SQL Server Integration Services (SSIS) van een Azure Data Factory-pijplijn met behulp van de activiteit opgeslagen Procedure aanroepen.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: article
ms.date: 12/07/2017
ms.author: jingwang
ms.openlocfilehash: ff0d1e5d644926864641ceb3e3c3a102167c1fd2
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Aanroepen van een SSIS-pakket met de activiteit opgeslagen procedure in Azure Data Factory
In dit artikel wordt beschreven hoe een SSIS-pakket van een Azure Data Factory-pijplijn met behulp van een activiteit opgeslagen procedure aanroepen. 

> [!NOTE]
> In dit artikel is van toepassing op versie 1 van de Data Factory, die in het algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de openbare Preview, Zie [aanroepen SSIS-pakketten met behulp van de activiteit opgeslagen procedure in versie 2](../how-to-invoke-ssis-package-stored-procedure-activity.md).

## <a name="prerequisites"></a>Vereisten

### <a name="azure-sql-database"></a>Azure SQL Database 
De procedures in dit artikel maakt gebruik van een Azure SQL-database die als host fungeert voor de SSIS-catalogus. U kunt ook een Azure SQL beheerd-exemplaar (afgeschermd voorbeeld).

### <a name="create-an-azure-ssis-integration-runtime"></a>Een Azure SSIS Integration Runtime maken
Een Azure-SSIS-integratie runtime maken als u geen volgt u de stapsgewijze instructies in de [zelfstudie: implementeren SSIS-pakketten](../tutorial-deploy-ssis-packages-azure.md). U moet een gegevensfactory van versie 2 voor het maken van een Azure-SSIS-integratie runtime maken. 

### <a name="azure-powershell"></a>Azure PowerShell
De nieuwste Azure PowerShell-modules installeren door de instructies te volgen in [installeren en configureren van Azure PowerShell](/powershell/azure/install-azurerm-ps).

## <a name="create-a-data-factory"></a>Een gegevensfactory maken
De volgende procedure bevat stappen voor het maken van een gegevensfactory. U maakt een pijplijn met een activiteit opgeslagen procedure in deze gegevensfactory. De activiteit opgeslagen procedure voert een opgeslagen procedure in de SSISDB-database om uit te voeren van uw SSIS-pakket.

1. Definieer een variabele voor de naam van de resourcegroep die u later gaat gebruiken in PowerShell-opdrachten. Kopieer de tekst van de volgende opdracht naar PowerShell, geef tussen dubbele aanhalingstekens een naam op voor de [Azure-resourcegroep](../../azure-resource-manager/resource-group-overview.md) en voer de opdracht uit. Bijvoorbeeld: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFTutorialResourceGroup";
    ```

    Als de resourcegroep al bestaat, wilt u waarschijnlijk niet dat deze wordt overschreven. Wijs een andere waarde toe aan de `$ResourceGroupName`-variabele en voer de opdracht opnieuw uit.
2. Voer de volgende opdracht uit om de resourcegroep te maken: 

    ```powershell
    $ResGrp = New-AzureRmResourceGroup $resourceGroupName -location 'eastus'
    ``` 
    Als de resourcegroep al bestaat, wilt u waarschijnlijk niet dat deze wordt overschreven. Wijs een andere waarde toe aan de `$ResourceGroupName`-variabele en voer de opdracht opnieuw uit. 
3. Definieer een variabele voor de naam van de data factory. 

    > [!IMPORTANT]
    >  Werk de naam van de data factory zodanig bij dat deze uniek is. 

    ```powershell
    $DataFactoryName = "ADFTutorialFactory";
    ```

5. Voer de volgende voor het maken van de gegevensfactory **New-AzureRmDataFactory** , de locatie en ResourceGroupName-eigenschap van de variabele $ResGrp met de cmdlet: 
    
    ```powershell       
    $df = New-AzureRmDataFactory -ResourceGroupName $ResourceGroupName -Name $dataFactoryName -Location "East US"
    $df 
    ```

Houd rekening met de volgende punten:

* De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als de volgende fout zich voordoet, wijzigt u de naam en probeert u het opnieuw.

    ```
    The specified Data Factory name 'ADFTutorialFactory' is already in use. Data Factory names must be globally unique.
    ```
* Als u Data Factory-exemplaren wilt maken, moet het gebruikersaccount waarmee u zich bij Azure aanmeldt, lid zijn van de rollen **Inzender** of **Eigenaar**, of moet dit een **beheerder** van het Azure-abonnement zijn.
* Momenteel kunt u met Data Factory versie 2 alleen data factory's maken in de regio's VS - oost, VS - oost 2 en West-Europa. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.

### <a name="create-an-azure-sql-database-linked-service"></a>Een gekoppelde Azure SQL Database-service maken
Maak een gekoppelde service die als host fungeert voor uw Azure SQL database koppelen de SSIS-catalogus aan uw gegevensfactory. Data Factory maakt gebruik van informatie in deze gekoppelde service verbinding maken met de SSISDB-database en voert een opgeslagen procedure voor het uitvoeren van SSIS-pakket. 

1. Maak een JSON-bestand met de naam **AzureSqlDatabaseLinkedService.json** in **C:\ADF\RunSSISPackage** map met de volgende inhoud: (de map ADFv2TutorialBulkCopy maken als deze nog niet bestaat).

    > [!IMPORTANT]
    > Vervang &lt;servername&gt;, &lt;databasename&gt;, &lt;username&gt;@&lt;servername&gt; en &lt;password&gt; door de waarden van uw Azure SQL Database voordat u het bestand opslaat.

    ```json
    {
        "name": "AzureSqlDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": "Server=tcp:<AZURE SQL SERVER NAME>.database.windows.net,1433;Database=SSISDB;User ID=<USERNAME>;Password=<PASSWORD>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
            }
        }
        }
    ```
2. In **Azure PowerShell**, overschakelen naar de **C:\ADF\RunSSISPackage** map.
3. Voer de **New-AzureRmDataFactoryLinkedService** cmdlet voor het maken van de gekoppelde service: **AzureSqlDatabaseLinkedService**. 

    ```powershell
    New-AzureRmDataFactoryLinkedService $df -File ".\AzureSqlDatabaseLinkedService.json"
    ```

## <a name="create-an-output-dataset"></a>Een uitvoergegevensset maken
Deze uitvoergegevensset is een dummy gegevensset die de planning van de pipeline-stations. U ziet dat de frequentie is ingesteld op uur en interval is ingesteld op 1. Daarom wordt de pijplijn uitgevoerd zodra een uur in de pijplijn start- en eindtijd. 

1. Maak een OuputDataset.json-bestand met de volgende inhoud: 
    
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
2. Voer de **nieuw AzureRmDataFactoryDataset** cmdlet om een gegevensset te maken. 

    ```powershell
    New-AzureRmDataFactoryDataset $df -File ".\OutputDataset.json"
    ```

## <a name="create-a-pipeline-with-stored-procedure-activity"></a>Een pijplijn maken met de activiteit opgeslagen procedure 
In deze stap maakt u een pijplijn met een activiteit opgeslagen procedure. De activiteit roept de sp_executesql opgeslagen procedure voor het uitvoeren van SSIS-pakket. 

1. Maak een JSON-bestand met de naam **MyPipeline.json** in de **C:\ADF\RunSSISPackage** map met de volgende inhoud:

    > [!IMPORTANT]
    > Vervang &lt;MAPNAAM&gt;, &lt;PROJECTNAAM&gt;, &lt;PAKKETNAAM&gt; met namen van de map, project en -pakket in de catalogus SSIS voordat u het bestand opslaat. 

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
                        "stmt": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'SsisAdfTest', @project_name=N'MyProject', @package_name=N'Package.dtsx', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
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

2. Maken van de pijplijn: **RunSSISPackagePipeline**, voert de **Set AzureRmDataFactoryPipeline** cmdlet.

    ```powershell
    $DFPipeLine = New-AzureRmDataFactoryPipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

## <a name="create-a-pipeline-run"></a>Een pijplijnuitvoering maken
Gebruik de **Invoke-AzureRmDataFactoryPipeline** cmdlet uitvoeren van de pijplijn. De cmdlet retourneert de id voor de pijplijnuitvoering voor toekomstige controle.

```powershell
$RunId = New-AzureRmDataFactoryPipeline $df -File ".\MyPipeline.json"
```

## <a name="monitor-the-pipeline-run"></a>De pijplijnuitvoering controleren.

2. Voer **Get-AzureRmDataFactorySlice** voor meer informatie over alle segmenten van de uitvoer gegevensset **, dit de uitvoertabel van de pijplijn is.

    ```PowerShell
    Get-AzureRmDataFactorySlice $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```
    De StartDateTime die u hier opgeeft, is dezelfde begintijd die u hebt opgegeven in de JSON van de pijplijn. 
3. Voer **Get-AzureRmDataFactoryRun** uit om voor een bepaald segment gegevens over het uitvoeren van de activiteit op te halen.

    ```PowerShell
    Get-AzureRmDataFactoryRun $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```

    U kunt deze cmdlet blijven uitvoeren tot u ziet dat het segment de status **Gereed** of **Mislukt** heeft gekregen. Wanneer het segment de status Gereed heeft, controleert u de map **partitioneddata** in de container **adfgetstarted** in uw blobopslag voor de uitvoergegevens.  Het maken van een on-demand HDInsight-cluster duurt normaal gesproken enige tijd.

    U kunt de volgende query uitvoeren tegen de SSISDB-database in uw Azure SQL-server om te controleren of het pakket uitgevoerd. 

    ```sql
    select * from catalog.executions
    ```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de activiteit opgeslagen procedure de [activiteit opgeslagen Procedure](data-factory-stored-proc-activity.md) artikel.

