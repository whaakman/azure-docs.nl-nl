---
title: Een Azure data factory maken met behulp van PowerShell| Microsoft Docs
description: "Maak een Azure data factory om gegevens te kopiëren van een locatie in een Azure Blob Storage naar een andere locatie in dezelfde Blob Storage."
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
ms.date: 09/26/2017
ms.author: jingwang
ms.openlocfilehash: 1e9109581a1943a77e91e7fa034873dc2a15a5e6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-data-factory-and-pipeline-using-powershell"></a>Een data factory en pijplijn maken met behulp van PowerShell
Azure Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee u gegevensgestuurde werkstromen kunt maken in de cloud. Op deze manier kunt u de verplaatsing en transformatie van gegevens indelen en automatiseren. Met Azure Data Factory kunt u gegevensgestuurde werkstromen (ook wel pijplijnen) maken en plannen die gegevens uit verschillende gegevensarchieven kunnen opnemen en de gegevens kunnen verwerken/transformeren met behulp van rekenservices zoals Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics en Azure Machine Learning. Daarnaast kunt u de uitvoergegevens publiceren naar gegevensarchieven zoals Azure SQL Data Warehouse, zodat BI-toepassingen (business intelligence) ze kunnen gebruiken. 

In deze snelstartgids wordt beschreven hoe u PowerShell kunt gebruiken om een Azure data factory te maken. Met de pijplijn in deze data factory worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* **Azure Storage-account**. U gebruikt de blob-opslag als de gegevensopslag voor de **bron** en de **sink**. Als u geen Azure Storage-account hebt, raadpleegt u het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) om er een te maken. 
* Maak een **blob-container** in Blob Storage, maak een **invoermap** in de container en upload een aantal bestanden naar de map. U kunt hulpprogramma's zoals [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) gebruiken om verbinding te maken met Azure Blob Storage, een blob-container te maken, een invoerbestand te uploaden en het uitvoerbestand te controleren.
* **Azure PowerShell**. Volg de instructies in [How to install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Azure PowerShell installeren en configureren).

## <a name="create-a-data-factory"></a>Een data factory maken

1. Start **PowerShell**. Houd Azure PowerShell geopend tot het einde van deze snelstartgids. Als u het programma sluit en opnieuw opent, moet u de opdrachten opnieuw uitvoeren.

    Voer de volgende opdracht uit en geef de gebruikersnaam en het wachtwoord op waarmee u zich aanmeldt bij Azure Portal:
        
    ```powershell
    Login-AzureRmAccount
    ```        
    Voer de volgende opdracht uit om alle abonnementen voor dit account weer te geven:

    ```powershell
    Get-AzureRmSubscription
    ```
    Voer de volgende opdracht uit om het abonnement te selecteren waarmee u wilt werken. Vervang **SubscriptionId** door de id van uw Azure-abonnement:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```
2. Voer de cmdlet **Set AzureRmDataFactoryV2** uit om een data factory te maken. Vervang voordat u de opdracht uitvoert de tijdelijke aanduidingen door uw eigen waarden. Vervang de **plaatsaanduidingen** door uw eigen waarden. 

    Definieer een variabele voor de naam van de resourcegroep die u later kunt gebruiken in PowerShell-opdrachten. 
    ```powershell
    $resourceGroupName = "<your resource group to create the factory>";
    ```

    Definieer een variabele voor de naam van de data factory die u later kunt gebruiken in PowerShell-opdrachten. 

    ```powershell
    $dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>";
    ```

    Voer de volgende opdracht uit om een data factory te maken. 
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

    Houd rekening met de volgende punten:

    * De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als de volgende fout zich voordoet, wijzigt u de naam en probeert u het opnieuw.

        ```
        The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
        ```

    * Als u Data Factory-exemplaren wilt maken, moet u bijdrager of beheerder zijn van het Azure-abonnement.
    * Momenteel kunt u in Data Factory V2 alleen data factory's maken in de regio VS-Oost. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.

## <a name="create-a-linked-service"></a>Een gekoppelde service maken

Maak gekoppelde services in een data factory om uw gegevensarchieven en compute-services aan de gegevensfactory te koppelen. In deze snelstartgids hoeft u maar één gekoppelde Azure Storage-service te maken, die in dit voorbeeld wordt gebruikt als zowel de bron- als de sinkopslag, met de naam 'AzureStorageLinkedService'.

1. Maak een JSON-bestand met de naam **AzureStorageLinkedService.json** in de map **C:\ADFv2QuickStartPSH** met de volgende inhoud (maak de map ADFv2QuickStartPSH als deze nog niet bestaat): Vervang &lt;accountName&gt; en &lt;accountKey&gt; door de naam en sleutel van uw Azure Storage-account voordat u het bestand opslaat.

    ```json
    {
        "name": "AzureStorageLinkedService",
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountName>;AccountKey=<accountKey>",
                    "type": "SecureString"
                }
            }
        }
    }
    ```

2. Schakel in **Azure PowerShell** over naar de map **ADFv2QuickStartPSH**.

3. Voer de cmdlet **Set-AzureRmDataFactoryV2LinkedService** uit om de gekoppelde service **AzureStorageLinkedService** te maken. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureStorageLinkedService" -DefinitionFile ".\AzureStorageLinkedService.json"
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

## <a name="create-a-dataset"></a>Een gegevensset maken

U definieert een gegevensset die de gegevens vertegenwoordigt die van een bron naar een sink moeten worden gekopieerd. In dit voorbeeld verwijst deze Blob-gegevensset naar de gekoppelde Azure Storage-service die u in de vorige stap hebt gemaakt. De gegevensset heeft een parameter nodig waarvan de waarde wordt ingesteld in een activiteit die de gegevensset verbruikt. De parameter wordt gebruikt om het 'folderPath' te maken dat verwijst naar de plaats waar de gegevens zich bevinden/zijn opgeslagen.

1. Maak een JSON-bestand met de naam **BlobDataset.json** in de map **C:\ADFv2QuickStartPSH**. Geef dit bestand de volgende inhoud:

    ```json
    {
        "name": "BlobDataset",
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": {
                    "value": "@{dataset().path}",
                    "type": "Expression"
                }
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
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "BlobDataset" -DefinitionFile ".\BlobDataset.json"
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

In dit voorbeeld bevat deze pijplijn één activiteit en heeft deze twee parameters: het pad van de invoerblob en het pad van de uitvoerblob. De waarden voor deze parameters worden ingesteld wanneer de pijplijn wordt geactiveerd of uitgevoerd. De kopieeractiviteit verwijst naar dezelfde blobgegevensset die u in de vorige stap heb gemaakt als invoer en uitvoer. Wanneer de dataset wordt gebruikt als invoergegevensset, wordt het invoerpad opgegeven. En wanneer de dataset wordt gebruikt als uitvoergegevensset, wordt het uitvoerpad opgegeven. 

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
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "Adfv2QuickStartPipeline" -DefinitionFile ".\Adfv2QuickStartPipeline.json"
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

    Vervang voordat u het bestand opslaat de waarde van 'inputPath' en 'outputPath' door het bron- en sinkblobpad waarvan en waarnaar u gegevens kopieert.

    ```json
    {
        "inputPath": "<the path to existing blob(s) to copy data from, e.g. containername/foldername>",
        "outputPath": "<the blob path to copy data to, e.g. containername/foldername>"
    }
    ```

2. Voer de cmdlet **Invoke-AzureRmDataFactoryV2Pipeline** uit om een pijplijn uitvoering te maken en de parameterwaarden door te geven. Ook wordt hiermee de id voor de pijplijnuitvoering vastgelegd voor toekomstige controle.

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName "Adfv2QuickStartPipeline" -ParameterFile .\PipelineParameters.json
    ```

## <a name="monitor-a-pipeline-run"></a>Een pijplijnuitvoering controleren

1. Voer het volgende script uit om continu de status van de pijplijnuitvoering te controleren totdat het kopiëren van de gegevens is voltooid.

    ```powershell
    while ($True) {
        $run = Get-AzureRmDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

        if ($run) {
            if ($run.Status -ne 'InProgress') {
                Write-Host "Pipeline run finished. The status is: " $run.Status -foregroundcolor "Yellow"
                $run
                break
            }
            Write-Host  "Pipeline is running...status: InProgress" -foregroundcolor "Yellow"
        }

        Start-Sleep -Seconds 30
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

2. Voer het volgende script uit om uitvoeringsdetails van de kopieeractiviteit op te halen, zoals de omvang van de gelezen of weggeschreven gegevens.

    ```powershell
    Write-Host "Activity run details:" -foregroundcolor "Yellow"
    $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $result
    
    Write-Host "Activity 'Output' section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"
    
    Write-Host "\nActivity 'Error' section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n"
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

## <a name="verify-the-output"></a>De uitvoer controleren
Gebruik hulpprogramma's als [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) om te controleren of de blob(s) is/zijn gekopieerd van het 'inputBlobPath' naar het 'outputBlobPath'.

## <a name="clean-up-resources"></a>Resources opschonen
De resources die u hebt gemaakt in de Quick Start kunt u op twee manieren opschonen. U kunt de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) verwijderen, met alle resources uit de resourcegroep. Als u de andere resources intact wilt houden, verwijdert u alleen de data factory die u in deze zelfstudie hebt gemaakt.

Voer de volgende opdracht uit om de gehele resourcegroep te verwijderen: 
```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

Voer de volgende opdracht uit om alleen de data factory te verwijderen: 

```powershell
Remove-AzureRmDataFactoryV2 -Name "<NameOfYourDataFactory>" -ResourceGroupName "<NameOfResourceGroup>"
```

## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag. Doorloop de [zelfstudies](tutorial-copy-data-dot-net.md) voor meer informatie over het gebruiken van Data Factory in andere scenario's. 