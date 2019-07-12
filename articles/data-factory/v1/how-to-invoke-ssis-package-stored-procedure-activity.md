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
ms.openlocfilehash: 030617d3afd73c68793ca0a1d6185264c92b791f
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839898"
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

1. Voer **Get-AzDataFactorySlice** voor meer informatie over alle segmenten van de uitvoer gegevensset **, is de uitvoertabel van de pijplijn.

    ```powershell
    Get-AzDataFactorySlice $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```
    De StartDateTime die u hier opgeeft, is dezelfde begintijd die u hebt opgegeven in de JSON van de pijplijn. 
1. Voer **Get-AzDataFactoryRun** om op te halen van de details van activiteit actief is gedurende een bepaald segment.

    ```powershell
    Get-AzDataFactoryRun $df -DatasetName sprocsampleout -StartDateTime 2017-10-01T00:00:00Z
    ```

    U kunt deze cmdlet blijven uitvoeren tot u ziet dat het segment de status **Gereed** of **Mislukt** heeft gekregen. 

    U kunt de volgende query uitvoeren op basis van de SSISDB-database in uw Azure SQL-server om te controleren of het pakket dat wordt uitgevoerd. 

    ```sql
    select * from catalog.executions
    ```

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over de activiteit opgeslagen procedure de [Stored Procedure-activiteit](data-factory-stored-proc-activity.md) artikel.

