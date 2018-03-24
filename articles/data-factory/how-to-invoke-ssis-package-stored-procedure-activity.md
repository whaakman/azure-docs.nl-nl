---
title: Aanroepen van SSIS-pakket met behulp van Azure Data Factory - activiteit opgeslagen Procedure | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een pakket SQL Server Integration Services (SSIS) van een Azure Data Factory-pijplijn met behulp van de activiteit opgeslagen Procedure aanroepen.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: article
ms.date: 12/07/2017
ms.author: jingwang
ms.openlocfilehash: dba1accbedf32099cc92be820d0937f3573746a0
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="invoke-an-ssis-package-using-stored-procedure-activity-in-azure-data-factory"></a>Aanroepen van een SSIS-pakket met de activiteit opgeslagen procedure in Azure Data Factory
In dit artikel wordt beschreven hoe een SSIS-pakket van een Azure Data Factory-pijplijn met behulp van een activiteit opgeslagen procedure aanroepen. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [aanroepen SSIS-pakketten met behulp van de activiteit opgeslagen procedure in versie 1](v1/how-to-invoke-ssis-package-stored-procedure-activity.md).

## <a name="prerequisites"></a>Vereisten

### <a name="azure-sql-database"></a>Azure SQL Database 
De procedures in dit artikel maakt gebruik van een Azure SQL-database die als host fungeert voor de SSIS-catalogus. U kunt ook een Azure SQL beheerd-exemplaar (afgeschermd voorbeeld).

## <a name="create-an-azure-ssis-integration-runtime"></a>Een Azure SSIS Integration Runtime maken
Een Azure-SSIS-integratie runtime maken als u geen volgt u de stapsgewijze instructies in de [zelfstudie: implementeren SSIS-pakketten](tutorial-create-azure-ssis-runtime-portal.md).

## <a name="data-factory-ui-azure-portal"></a>Data Factory-gebruikersinterface (Azure-portal)
In deze sectie kunt u Data Factory-gebruikersinterface gebruiken voor het maken van een Data Factory-pijplijn met een opgeslagen procedure-activiteit die wordt aangeroepen SSIS-pakket.

### <a name="create-a-data-factory"></a>Een gegevensfactory maken
Eerste stap is het maken van een gegevensfactory met behulp van de Azure-portal. 

1. Start de webbrowser **Microsoft Edge** of **Google Chrome**. Op dit moment wordt de Data Factory-gebruikersinterface alleen ondersteund in de webbrowsers Microsoft Edge en Google Chrome.
2. Navigeer naar [Azure Portal](https://portal.azure.com). 
3. Klik op **Nieuw** in het linkermenu en klik vervolgens op **Gegevens en analyses** en **Data Factory**. 
   
   ![Nieuw -> DataFactory](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory-menu.png)
2. Voer op de pagina **Nieuwe data factory** **ADFTutorialDataFactory** in als **naam**. 
      
     ![De pagina Nieuwe data factory](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-azure-data-factory.png)
 
   De naam van de Azure-gegevensfactory moet **wereldwijd uniek** zijn. Als het volgende foutbericht wordt weergegeven voor het naamveld, wijzigt u de naam van de data factory (bijvoorbeeld uwnaamADFTutorialDataFactory). Zie het artikel [Data Factory - Naamgevingsregels](naming-rules.md) voor meer informatie over naamgevingsregels voor Data Factory-artefacten.
  
     ![Naam niet beschikbaar - fout](./media/how-to-invoke-ssis-package-stored-procedure-activity/name-not-available-error.png)
3. Selecteer het Azure-**abonnement** waarin u de gegevensfactory wilt maken. 
4. Voer een van de volgende stappen uit voor de **Resourcegroep**:
     
      - Selecteer **Bestaande gebruiken** en selecteer een bestaande resourcegroep in de vervolgkeuzelijst. 
      - Selecteer **Nieuwe maken** en voer de naam van een resourcegroep in.   
         
    Zie [Resourcegroepen gebruiken om Azure-resources te beheren](../azure-resource-manager/resource-group-overview.md) voor meer informatie.  
4. Selecteer **V2 (Preview)** als de **versie**.
5. Selecteer de **locatie** voor de gegevensfactory. In de vervolgkeuzelijst ziet u alleen locaties die worden ondersteund in Data Factory. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich op andere locaties bevinden.
6. Selecteer **Vastmaken aan dashboard**.     
7. Klik op **Create**.
8. Op het dashboard ziet u de volgende tegel met de status: **Gegevensfactory implementeren**. 

    ![tegel met de status 'gegevensfactory implementeren'](media//how-to-invoke-ssis-package-stored-procedure-activity/deploying-data-factory.png)
9. Na het aanmaken ziet u de pagina **Data Factory** zoals weergegeven in de afbeelding.
   
    ![Startpagina van de gegevensfactory](./media/how-to-invoke-ssis-package-stored-procedure-activity/data-factory-home-page.png)
10. Klik op de tegel **Maken en controleren** om de gebruikersinterface (UI) van Azure Data Factory te openen op een afzonderlijk tabblad. 

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Een pijplijn maken met de activiteit opgeslagen procedure
In deze stap gebruikt u de Data Factory-gebruikersinterface voor het maken van een pijplijn. U een opgeslagen procedure-activiteit toevoegen aan de pijplijn en configureren voor het uitvoeren van SSIS-pakket met behulp van de sp_executesql opgeslagen procedure. 

1. Klik in de pagina aan de slag, **maken pijplijn**: 

    ![Pagina Aan de slag](./media/how-to-invoke-ssis-package-stored-procedure-activity/get-started-page.png)
2. In de **activiteiten** werkset Vouw **algemene**, en slepen en neerzetten **opgeslagen Procedure** activiteit naar het ontwerpoppervlak pijplijn. 

    ![De activiteit opgeslagen procedure slepen en neerzetten](./media/how-to-invoke-ssis-package-stored-procedure-activity/drag-drop-sproc-activity.png)
3. In het eigenschappenvenster voor de activiteit opgeslagen procedure overschakelen naar de **-Account voor SQL** tabblad en klik op **+ nieuw**. U maken een verbinding met de Azure SQL database die als host fungeert voor de catalogus SSIS (SSIDB database). 
   
    ![Knop Nieuwe gekoppelde service](./media/how-to-invoke-ssis-package-stored-procedure-activity/new-linked-service-button.png)
4. Voer in het venster **Nieuwe gekoppelde service** de volgende stappen uit: 

    1. Selecteer **Azure SQL Database** voor **Type**.
    2. Selecteer uw Azure SQL-server die als host fungeert voor de SSISDB-database voor de **servernaam** veld.
    3. Selecteer **SSISDB** voor **databasenaam**.
    4. Voor **gebruikersnaam**, voer de naam van de gebruiker die toegang tot de database heeft.
    5. Voor **wachtwoord**, voer het wachtwoord van de gebruiker. 
    6. Test de verbinding met de database door te klikken op **verbinding testen** knop.
    7. Opslaan van de gekoppelde service door te klikken op de **opslaan** knop. 

        ![Een gekoppelde Azure SQL Database-service](./media/how-to-invoke-ssis-package-stored-procedure-activity/azure-sql-database-linked-service-settings.png)
5. In het eigenschappenvenster overschakelen naar de **opgeslagen Procedure** TAB-toets van de **-Account voor SQL** tabblad en voer de volgende stappen uit: 

    1. Selecteer **Bewerken**. 
    2. Voor de **opgeslagen procedurenaam** veld Enter `sp_executesql`. 
    3. Klik op **+ nieuw** in de **opgeslagen procedureparameters** sectie. 
    4. Voor **naam** invoeren van de parameter **instructie INSERT**. 
    5. Voor **type** invoeren van de parameter **tekenreeks**. 
    6. Voor **waarde** van de parameter, voert u de volgende SQL-query:

        Geef in de SQL-query de juiste waarden voor de **mapnaam**, **projectnaam**, en **naam_van_pakket** parameters. 

        ```sql
        DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<FOLDER name in SSIS Catalog>', @project_name=N'<PROJECT name in SSIS Catalog>', @package_name=N'<PACKAGE name>.dtsx', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END
        ```

        ![Een gekoppelde Azure SQL Database-service](./media/how-to-invoke-ssis-package-stored-procedure-activity/stored-procedure-settings.png)
6. Voor het valideren van de configuratie van de pipeline, klikt u op **valideren** op de werkbalk. Sluit het venster **Pipeline Validation Report** door op **>>** te klikken.

    ![Pijplijn valideren](./media/how-to-invoke-ssis-package-stored-procedure-activity/validate-pipeline.png)
7. De pijplijn publiceren naar Data Factory door te klikken op **Alles publiceren** knop. 

    ![Publiceren](./media/how-to-invoke-ssis-package-stored-procedure-activity/publish-all-button.png)    

### <a name="run-and-monitor-the-pipeline"></a>Uitvoeren en controleren van de pijplijn
In deze sectie een pijplijn run activeren en deze vervolgens te controleren. 

1. Als u wilt activeren van een pijplijn die worden uitgevoerd, klikt u op **Trigger** op de werkbalk en klikt u op **nu starten**. 

    ![Nu activeren](./media/how-to-invoke-ssis-package-stored-procedure-activity/trigger-now.png)
2. Selecteer in het venster **Pijplijnuitvoering** de optie **Voltooien**. 
3. Ga naar het tabblad **Controleren** aan de linkerkant. U ziet de pijplijn uitvoeren en de status ervan samen met andere informatie (zoals tijd uitvoeren starten). Als u de lijst wilt vernieuwen, klikt u op **Vernieuwen**.

    ![Pijplijnuitvoeringen](./media/how-to-invoke-ssis-package-stored-procedure-activity/pipeline-runs.png)
3. Klik op de koppeling **Uitvoeringen van activiteiten weergeven** in de kolom **Acties**. Er is slechts één activiteit die wordt uitgevoerd als de pijplijn slechts één activiteit (opgeslagen procedure heeft).

    ![Activiteiten bij uitvoering](./media/how-to-invoke-ssis-package-stored-procedure-activity/activity-runs.png) 4. Klik kunt u het volgende uitvoeren **query** tegen de SSISDB-database in uw Azure SQL-server om te controleren of het pakket uitgevoerd. 

    ```sql
    select * from catalog.executions
    ```

    ![Controleer of de pakket-uitvoeringen](./media/how-to-invoke-ssis-package-stored-procedure-activity/verify-package-executions.png)


> [!NOTE]
> U kunt ook een geplande trigger maken voor uw pijplijn, zodat de pijplijn wordt uitgevoerd volgens een schema (elk uur, dagelijks, enz.). Zie voor een voorbeeld [Maak een gegevensfactory - Data Factory UI](quickstart-create-data-factory-portal.md#trigger-the-pipeline-on-a-schedule).

## <a name="azure-powershell"></a>Azure PowerShell
In deze sectie kunt u Azure PowerShell gebruiken voor het maken van een Data Factory-pijplijn met een opgeslagen procedure-activiteit die wordt aangeroepen SSIS-pakket. 

Installeer de nieuwste Azure PowerShell-modules met de instructies in [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps). 

### <a name="create-a-data-factory"></a>Een gegevensfactory maken
U kunt de dezelfde data factory met de Azure-SSIS-IR gebruiken of maak een afzonderlijke gegevensfactory. De volgende procedure bevat stappen voor het maken van een gegevensfactory. U maakt een pijplijn met een activiteit opgeslagen procedure in deze gegevensfactory. De activiteit opgeslagen procedure voert een opgeslagen procedure in de SSISDB-database om uit te voeren van uw SSIS-pakket. 

1. Definieer een variabele voor de naam van de resourcegroep die u later gaat gebruiken in PowerShell-opdrachten. Kopieer de tekst van de volgende opdracht naar PowerShell, geef tussen dubbele aanhalingstekens een naam op voor de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) en voer de opdracht uit. Bijvoorbeeld: `"adfrg"`. 
   
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

5. Voer de volgende cmdlet **Set-AzureRmDataFactoryV2** uit om de data factory te maken met de eigenschap Location en ResourceGroupName van de variabele $ResGrp: 
    
    ```powershell       
    $DataFactory = Set-AzureRmDataFactoryV2 -ResourceGroupName $ResGrp.ResourceGroupName -Location $ResGrp.Location -Name $dataFactoryName 
    ```

Houd rekening met de volgende punten:

* De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als de volgende fout zich voordoet, wijzigt u de naam en probeert u het opnieuw.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```
* Als u Data Factory-exemplaren wilt maken, moet het gebruikersaccount waarmee u zich bij Azure aanmeldt, lid zijn van de rollen **Inzender** of **Eigenaar**, of moet dit een **beheerder** van het Azure-abonnement zijn.
* Momenteel kunt u met Data Factory versie 2 alleen data factory's maken in de regio's VS - oost, VS - oost 2 en West-Europa. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.

### <a name="create-an-azure-sql-database-linked-service"></a>Een gekoppelde Azure SQL Database-service maken
Maak een gekoppelde service die als host fungeert voor uw Azure SQL database koppelen de SSIS-catalogus aan uw gegevensfactory. Data Factory maakt gebruik van informatie in deze gekoppelde service verbinding maken met de SSISDB-database en voert een opgeslagen procedure voor het uitvoeren van SSIS-pakket. 

1. Maak een JSON-bestand met de naam **AzureSqlDatabaseLinkedService.json** in **C:\ADF\RunSSISPackage** map met de volgende inhoud: 

    > [!IMPORTANT]
    > Vervang &lt;servername&gt;, &lt;gebruikersnaam&gt;, en &lt;wachtwoord&gt; met waarden van uw Azure SQL-Database voordat u het bestand opslaat.

    ```json
    {
        "name": "AzureSqlDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=tcp:<servername>.database.windows.net,1433;Database=SSISDB;User ID=<username>;Password=<password>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30"
                }
            }
        }
    }
    ```

2. In **Azure PowerShell**, overschakelen naar de **C:\ADF\RunSSISPackage** map.

3. Voer de cmdlet **Set-AzureRmDataFactoryV2LinkedService** uit om de gekoppelde service **AzureSqlDatabaseLinkedService** te maken. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "AzureSqlDatabaseLinkedService" -File ".\AzureSqlDatabaseLinkedService.json"
    ```

### <a name="create-a-pipeline-with-stored-procedure-activity"></a>Een pijplijn maken met de activiteit opgeslagen procedure 
In deze stap maakt u een pijplijn met een activiteit opgeslagen procedure. De activiteit roept de sp_executesql opgeslagen procedure voor het uitvoeren van SSIS-pakket. 

1. Maak een JSON-bestand met de naam **RunSSISPackagePipeline.json** in de **C:\ADF\RunSSISPackage** map met de volgende inhoud:

    > [!IMPORTANT]
    > Vervang &lt;MAPNAAM&gt;, &lt;PROJECTNAAM&gt;, &lt;PAKKETNAAM&gt; met namen van de map, project en -pakket in de catalogus SSIS voordat u het bestand opslaat. 

    ```json
    {
        "name": "RunSSISPackagePipeline",
        "properties": {
            "activities": [
                {
                    "name": "My SProc Activity",
                    "description":"Runs an SSIS package",
                    "type": "SqlServerStoredProcedure",
                    "linkedServiceName": {
                        "referenceName": "AzureSqlDatabaseLinkedService",
                        "type": "LinkedServiceReference"
                    },
                    "typeProperties": {
                        "storedProcedureName": "sp_executesql",
                        "storedProcedureParameters": {
                            "stmt": {
                                "value": "DECLARE @return_value INT, @exe_id BIGINT, @err_msg NVARCHAR(150)    EXEC @return_value=[SSISDB].[catalog].[create_execution] @folder_name=N'<FOLDER NAME>', @project_name=N'<PROJECT NAME>', @package_name=N'<PACKAGE NAME>', @use32bitruntime=0, @runinscaleout=1, @useanyworker=1, @execution_id=@exe_id OUTPUT    EXEC [SSISDB].[catalog].[set_execution_parameter_value] @exe_id, @object_type=50, @parameter_name=N'SYNCHRONIZED', @parameter_value=1    EXEC [SSISDB].[catalog].[start_execution] @execution_id=@exe_id, @retry_count=0    IF(SELECT [status] FROM [SSISDB].[catalog].[executions] WHERE execution_id=@exe_id)<>7 BEGIN SET @err_msg=N'Your package execution did not succeed for execution ID: ' + CAST(@exe_id AS NVARCHAR(20)) RAISERROR(@err_msg,15,1) END"
                            }
                        }
                    }
                }
            ]
        }
    }
    ```

2. Maken van de pijplijn: **RunSSISPackagePipeline**, voert de **Set AzureRmDataFactoryV2Pipeline** cmdlet.

    ```powershell
    $DFPipeLine = Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "RunSSISPackagePipeline" -DefinitionFile ".\RunSSISPackagePipeline.json"
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```
    PipelineName      : Adfv2QuickStartPipeline
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {CopyFromBlobToBlob}
    Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

### <a name="create-a-pipeline-run"></a>Een pijplijnuitvoering maken
Gebruik de **Invoke-AzureRmDataFactoryV2Pipeline** cmdlet uitvoeren van de pijplijn. De cmdlet retourneert de id voor de pijplijnuitvoering voor toekomstige controle.

```powershell
$RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineName $DFPipeLine.Name
```

### <a name="monitor-the-pipeline-run"></a>De pijplijnuitvoering controleren.

Voer het volgende PowerShell-script uit om continu de status van de pijplijnuitvoering te controleren totdat het kopiëren van de gegevens is voltooid. Kopieer/plak het volgende script in het PowerShell-venster en druk op ENTER. 

```powershell
while ($True) {
    $Run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -PipelineRunId $RunId

    if ($Run) {
        if ($run.Status -ne 'InProgress') {
            Write-Output ("Pipeline run finished. The status is: " +  $Run.Status)
            $Run
            break
        }
        Write-Output  "Pipeline is running...status: InProgress"
    }

    Start-Sleep -Seconds 10
}   
```

### <a name="create-a-trigger"></a>Geen trigger maken
In de vorige stap, moet u de pijplijn op aanvraag aangeroepen. U kunt ook een schema-trigger voor het uitvoeren van de pijplijn op een planning (elk uur, dagelijks, enz.) maken.

1. Maak een JSON-bestand met de naam **MyTrigger.json** in **C:\ADF\RunSSISPackage** map met de volgende inhoud: 

    ```json
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Hour",
                    "interval": 1,
                    "startTime": "2017-12-07T00:00:00-08:00",
                    "endTime": "2017-12-08T00:00:00-08:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "RunSSISPackagePipeline"
                    },
                    "parameters": {}
                }
            ]
        }
    }    
    ```
2. In **Azure PowerShell**, overschakelen naar de **C:\ADF\RunSSISPackage** map.
3. Voer de **Set AzureRmDataFactoryV2Trigger** cmdlet, waarbij de trigger wordt gemaakt. 

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" -DefinitionFile ".\MyTrigger.json"
    ```
4. Standaard wordt de trigger is gestopt. Start de trigger met de **Start AzureRmDataFactoryV2Trigger** cmdlet. 

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResGrp.ResourceGroupName -DataFactoryName $DataFactory.DataFactoryName -Name "MyTrigger" 
    ```
5. Bevestig dat de trigger is gestart door het uitvoeren van de **Get-AzureRmDataFactoryV2Trigger** cmdlet. 

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"     
    ```    
6. Voer de volgende opdracht na het volgende uur. Bijvoorbeeld, als de huidige tijd 3:25 uur UTC is, voer de opdracht op 4 uur UTC. 
    
    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-06" -TriggerRunStartedBefore "2017-12-09"
    ```

    U kunt de volgende query uitvoeren tegen de SSISDB-database in uw Azure SQL-server om te controleren of het pakket uitgevoerd. 

    ```sql
    select * from catalog.executions
    ```


## <a name="next-steps"></a>Volgende stappen
U kunt ook de pijplijn met de Azure-portal bewaken. Zie voor stapsgewijze instructies [bewaken van de pijplijn](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).
