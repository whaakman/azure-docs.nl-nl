---
title: "Gegevens in Blob Storage kopiëren met behulp van Azure Data Factory | Microsoft Docs"
description: "Maak een data factory in Azure om gegevens van één map naar een andere map te kopiëren in dezelfde instantie van Azure Blob Storage."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: powershell
ms.topic: hero-article
ms.date: 01/22/2018
ms.author: jingwang
ms.openlocfilehash: 40a8e3a0ac0130eb12193a07c4bf367efe29b9d9
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="create-an-azure-data-factory-using-powershell"></a>Een data factory in Azure maken met behulp van PowerShell 
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versie 2 - Preview](quickstart-create-data-factory-powershell.md)

In deze snelstartgids wordt beschreven hoe u PowerShell kunt gebruiken om een Azure data factory te maken. Met de pijplijn die u in deze data factory maakt, worden gegevens **gekopieerd** van één map naar een andere map in een Azure Blob Storage. Zie [Zelfstudie: Gegevens transformeren met Spark](transform-data-using-spark.md) voor meer informatie over het **transformeren** van gegevens met Azure Data Factory. 

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).
>
> Dit artikel is geen gedetailleerde introductie tot de Data Factory-service. Zie [Inleiding tot Azure Data Factory](introduction.md) voor een inleiding tot Azure Data Factory-service.

[!INCLUDE [data-factory-quickstart-prerequisites](../../includes/data-factory-quickstart-prerequisites.md)] 

### <a name="azure-powershell"></a>Azure PowerShell
Installeer de nieuwste Azure PowerShell-modules met de instructies in [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps).

#### <a name="log-in-to-powershell"></a>Aanmelden bij PowerShell

1. Start **PowerShell** op uw computer. Houd PowerShell geopend tot het einde van deze QuickStart. Als u het programma sluit en opnieuw opent, moet u deze opdrachten opnieuw uitvoeren.
2. Voer de volgende opdracht uit en geef de gebruikersnaam en het wachtwoord op waarmee u zich aanmeldt bij Azure Portal:
       
    ```powershell
    Login-AzureRmAccount
    ```        
3. Voer de volgende opdracht uit om alle abonnementen voor dit account weer te geven:

    ```powershell
    Get-AzureRmSubscription
    ```
4. Als u meerdere abonnementen hebt, voert u de volgende opdracht uit om het abonnement te selecteren waarmee u wilt werken. Vervang **SubscriptionId** door de id van uw Azure-abonnement:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Een gegevensfactory maken
1. Definieer een variabele voor de naam van de resourcegroep die u later gaat gebruiken in PowerShell-opdrachten. Kopieer de tekst van de volgende opdracht naar PowerShell, geef tussen dubbele aanhalingstekens een naam op voor de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) en voer de opdracht uit. Bijvoorbeeld: `"adfrg"`. 
   
     ```powershell
    $resourceGroupName = "ADFQuickStartRG";
    ```

    Als de resourcegroep al bestaat, wilt u waarschijnlijk niet dat deze wordt overschreven. Wijs een andere waarde toe aan de `$ResourceGroupName`-variabele en voer de opdracht opnieuw uit.
2. Voer de volgende opdracht uit om de resourcegroep te maken: 

    ```powershell
    $ResGrp = New-AzureRmResourceGroup $resourceGroupName -location 'East US'
    ``` 
    Als de resourcegroep al bestaat, wilt u waarschijnlijk niet dat deze wordt overschreven. Wijs een andere waarde toe aan de `$ResourceGroupName`-variabele en voer de opdracht opnieuw uit. 
3. Definieer een variabele voor de naam van de data factory. 

    > [!IMPORTANT]
    >  Werk de naam van de data factory zodanig bij dat deze uniek is. Bijvoorbeeld: ADFTutorialFactorySP1127. 

    ```powershell
    $dataFactoryName = "ADFQuickStartFactory";
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

## <a name="create-a-linked-service"></a>Een gekoppelde service maken

Maak gekoppelde services in een data factory om uw gegevensarchieven en compute-services aan de gegevensfactory te koppelen. In deze QuickStart gaat u een gekoppelde Azure Storage-service maken die als bron- en als sinkopslag wordt gebruikt. De gekoppelde service beschikt over de verbindingsgegevens die de Data Factory-service tijdens runtime gebruikt om er een verbinding mee tot stand te brengen.

1. Maak een JSON-bestand met de naam **AzureStorageLinkedService.json** in de map **C:\ADFv2QuickStartPSH** met de volgende inhoud (maak de map ADFv2QuickStartPSH als deze nog niet bestaat): 

    > [!IMPORTANT]
    > Vervang &lt;accountName&gt; en &lt;accountKey&gt; door de naam en sleutel van uw Azure Storage-account voordat u het bestand opslaat.

    ```json
    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>;EndpointSuffix=core.windows.net",
                    "type": "SecureString"
                }
            }
        }
    }
    ```
    Als u Kladblok gebruikt, selecteert u **Alle bestanden** voor het veld **Opslaan als** in het dialoogvenster **Opslaan als**. Als u dat niet doet, wordt mogelijk de extensie `.txt` toegevoegd aan het bestand. Bijvoorbeeld `AzureStorageLinkedService.json.txt`. Als u het bestand in Verkenner maakt voordat u het opent in Kladblok, ziet u de extensie `.txt` mogelijk niet omdat de optie **Extensies voor bekende bestandstypen verbergen** standaard is ingeschakeld. Verwijder de extensie `.txt` voordat u doorgaat met de volgende stap.
2. Schakel in **PowerShell** over naar de map **ADFv2QuickStartPSH**.

    ```powershell
    Set-Location 'C:\ADFv2QuickStartPSH'
    ```
3. Voer de cmdlet **Set-AzureRmDataFactoryV2LinkedService** uit om de gekoppelde service **AzureStorageLinkedService** te maken. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "AzureStorageLinkedService" -DefinitionFile ".\AzureStorageLinkedService.json"
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

## <a name="create-a-dataset"></a>Een gegevensset maken
Tijdens deze stap gaat u een gegevensset definiëren die de gegevens vertegenwoordigt die van een bron naar een sink moeten worden gekopieerd. De gegevensset is van het type **AzureBlob**. Deze gegevensset verwijst naar de **gekoppelde Azure Storage-service** die u in de vorige stap hebt gemaakt. Er is een parameter nodig om de eigenschap **folderPath** te kunnen samenstellen. Als invoergegevensset wordt tijdens de kopieeractiviteit in de pijplijn het invoerpad als een waarde voor deze parameter doorgegeven. Op een soortgelijke manier wordt voor een uitvoergegevensset tijdens de kopieerbewerking het uitvoerpad als een waarde voor deze parameter doorgegeven. 

1. Maak een JSON-bestand met de naam **BlobDataset.json** in de map **C:\ADFv2QuickStartPSH**. Geef dit bestand de volgende inhoud:

    ```json
    {
        "name": "BlobDataset",
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "@{dataset().path}"
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            },
            "parameters": {
                "path": {
                    "type": "String"
                }
            }
        }
    }
    ```

2. De gegevensset **BlobDataset** maken: voer de cmdlet **Set-AzureRmDataFactoryV2Dataset** uit.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "BlobDataset" -DefinitionFile ".\BlobDataset.json"
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```
    DatasetName       : BlobDataset
    ResourceGroupName : <resourceGroupname>
    DataFactoryName   : <dataFactoryName>
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-a-pipeline"></a>Een pijplijn maken
  
In deze QuickStart gaat u een pijplijn maken met één activiteit waarvoor twee parameters nodig zijn: het pad van de invoerblob en het pad van de uitvoerblob. De waarden voor deze parameters worden ingesteld wanneer de pijplijn wordt geactiveerd of uitgevoerd. De kopieeractiviteit maakt gebruik van dezelfde blobgegevensset die u in de vorige stap heb gemaakt als invoer en uitvoer. Wanneer de dataset wordt gebruikt als invoergegevensset, wordt het invoerpad opgegeven. En wanneer de dataset wordt gebruikt als uitvoergegevensset, wordt het uitvoerpad opgegeven. 

1. Maak een JSON-bestand met de naam **Adfv2QuickStartPipeline.json** in de map **C:\ADFv2QuickStartPSH**. Geef dit bestand de volgende inhoud:

    ```json
    {
        "name": "Adfv2QuickStartPipeline",
        "properties": {
            "activities": [
                {
                    "name": "CopyFromBlobToBlob",
                    "type": "Copy",
                    "inputs": [
                        {
                            "referenceName": "BlobDataset",
                            "parameters": {
                                "path": "@pipeline().parameters.inputPath"
                            },
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "BlobDataset",
                            "parameters": {
                                "path": "@pipeline().parameters.outputPath"
                            },
                            "type": "DatasetReference"
                        }
                    ],
                    "typeProperties": {
                        "source": {
                            "type": "BlobSource"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    }
                }
            ],
            "parameters": {
                "inputPath": {
                    "type": "String"
                },
                "outputPath": {
                    "type": "String"
                }
            }
        }
    }
    ```

2. De pijplijn **Adfv2QuickStartPipeline** maken: voer de cmdlet **Set-AzureRmDataFactoryV2Pipeline** uit.

    ```powershell
    $DFPipeLine = Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -Name "Adfv2QuickStartPipeline" -DefinitionFile ".\Adfv2QuickStartPipeline.json"
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```
    PipelineName      : Adfv2QuickStartPipeline
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Activities        : {CopyFromBlobToBlob}
    Parameters        : {[inputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification], [outputPath, Microsoft.Azure.Management.DataFactory.Models.ParameterSpecification]}
    ```

## <a name="create-a-pipeline-run"></a>Een pijplijnuitvoering maken

In deze stap stelt u de waarden voor de pijpelijnparameters **inputPath** en **outputPath** in op de werkelijke waarden van de bron- en sinkblobpaden. Vervolgens maakt u een pijplijnuitvoering met behulp van deze argumenten. 

1. Maak een JSON-bestand met de naam **PipelineParameters.json** in de map **C:\ADFv2QuickStartPSH**. Geef dit bestand de volgende inhoud:

    ```json
    {
        "inputPath": "adftutorial/input",
        "outputPath": "adftutorial/output"
    }
    ```
2. Voer de cmdlet **Invoke-AzureRmDataFactoryV2Pipeline** uit om een pijplijn uitvoering te maken en de parameterwaarden door te geven. De cmdlet retourneert de id voor de pijplijnuitvoering voor toekomstige controle.

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineName $DFPipeLine.Name -ParameterFile .\PipelineParameters.json
    ```

## <a name="monitor-the-pipeline-run"></a>De pijplijnuitvoering controleren.

1. Voer het volgende PowerShell-script uit om continu de status van de pijplijnuitvoering te controleren totdat het kopiëren van de gegevens is voltooid. Kopieer/plak het volgende script in het PowerShell-venster en druk op ENTER. 

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

    Hier volgt een uitvoervoorbeeld van de pijplijnuitvoering:

    ```
    Pipeline is running...status: InProgress
    Pipeline run finished. The status is:  Succeeded
    
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : SPTestFactory0928
    RunId             : 0000000000-0000-0000-0000-0000000000000
    PipelineName      : Adfv2QuickStartPipeline
    LastUpdated       : 9/28/2017 8:28:38 PM
    Parameters        : {[inputPath, adftutorial/input], [outputPath, adftutorial/output]}
    RunStart          : 9/28/2017 8:28:14 PM
    RunEnd            : 9/28/2017 8:28:38 PM
    DurationInMs      : 24151
    Status            : Succeeded
    Message           :
    ```

    Als u de volgende fout ziet:
    ```
    Activity CopyFromBlobToBlob failed: Failed to detect region of linked service 'AzureStorage' : 'AzureStorageLinkedService' with error '[Region Resolver] Azure Storage failed to get address for DNS. Warning: System.Net.Sockets.SocketException (0x80004005): No such host is known
    ```
    moet u de volgende stappen uitvoeren: 
    1. Controleer in AzureStorageLinkedService.json of de naam en sleutel van uw Azure Storage-account juist zijn. 
    2. Controleer of de indeling van de verbindingsreeks juist is. De eigenschappen, bijvoorbeeld AccountName en AccountKey, worden gescheiden door puntkomma's (`;`). 
    3. Als er rondom de accountnaam en de accountsleutel vierkante haken staan, verwijdert u deze. 
    4. Hier volgt een voorbeeld van een verbindingsreeks: 

        ```json
        "connectionString": {
            "value": "DefaultEndpointsProtocol=https;AccountName=mystorageaccountname;AccountKey=mystorageacountkey;EndpointSuffix=core.windows.net",
            "type": "SecureString"
        }
        ```
    5. Maak de gekoppelde service opnieuw door de stappen in de sectie [Een gekoppelde service maken](#create-a-linked-service) te volgen. 
    6. Voer de pijplijn opnieuw uit door de stappen in de sectie [Een pijplijnuitvoering maken](#create-a-pipeline-run) te volgen. 
    7. Voer de huidige controleopdracht opnieuw uit als u de nieuwe pijplijnuitvoering wilt controleren. 
1. Voer het volgende script uit om uitvoeringsdetails van de kopieeractiviteit op te halen, zoals de omvang van de gelezen of weggeschreven gegevens.

    ```powershell
    Write-Output "Activity run details:"
    $Result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $DataFactory.DataFactoryName -ResourceGroupName $ResGrp.ResourceGroupName -PipelineRunId $RunId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $Result

    Write-Output "Activity 'Output' section:"
    $Result.Output -join "`r`n"

    Write-Output "Activity 'Error' section:"
    $Result.Error -join "`r`n"
    ```
3. Controleer of de uitvoer die u ziet vergelijkbaar is met de volgende voorbeelduitvoer van de uitvoering van de activiteit:

    ```json
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : SPTestFactory0928
    ActivityName      : CopyFromBlobToBlob
    PipelineRunId     : 00000000000-0000-0000-0000-000000000000
    PipelineName      : Adfv2QuickStartPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, copyDuration, throughput...}
    LinkedServiceName :
    ActivityRunStart  : 9/28/2017 8:28:18 PM
    ActivityRunEnd    : 9/28/2017 8:28:36 PM
    DurationInMs      : 18095
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    Activity 'Output' section:
    "dataRead": 38
    "dataWritten": 38
    "copyDuration": 7
    "throughput": 0.01
    "errors": []
    "effectiveIntegrationRuntime": "DefaultIntegrationRuntime (West US)"
    "usedCloudDataMovementUnits": 2
    "billedDuration": 14
    ```

[!INCLUDE [data-factory-quickstart-verify-output-cleanup.md](../../includes/data-factory-quickstart-verify-output-cleanup.md)] 

## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag. Doorloop de [zelfstudies](tutorial-copy-data-dot-net.md) voor meer informatie over het gebruiken van Data Factory in andere scenario's. 
