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
ms.date: 11/14/2017
ms.author: jingwang
ms.openlocfilehash: 63e4c654409651f6655da1bed6ab2f544cf024dd
ms.sourcegitcommit: c25cf136aab5f082caaf93d598df78dc23e327b9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="create-an-azure-data-factory-and-pipeline-using-powershell"></a>Een Azure data factory en pijplijn maken met behulp van PowerShell
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versie 2 - Preview](quickstart-create-data-factory-powershell.md)

In deze QuickStart wordt beschreven hoe u PowerShell kunt gebruiken om een Azure data factory te maken. Met de pijplijn die u in deze data factory maakt, worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag. Zie [Zelfstudie: Gegevens transformeren met Spark](transform-data-using-spark.md) voor meer informatie over het transformeren van gegevens met Azure Data Factory. 

Dit artikel is geen gedetailleerde introductie tot de Data Factory-service. Zie [Inleiding tot Azure Data Factory](introduction.md) voor een inleiding tot Azure Data Factory-service.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).


## <a name="prerequisites"></a>Vereisten

### <a name="azure-subscription"></a>Azure-abonnement
Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

### <a name="azure-storage-account"></a>Azure Storage-account
In deze QuickStart gaat u een algemeen Azure Storage-account (met name Blob Storage) gebruiken als zowel **bron-** als **sink/doel-**gegevensopslag. Zie het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) als u geen Azure Storage-account hebt voor algemene doeleinden en er een wilt maken. 

#### <a name="get-storage-account-name-and-account-key"></a>De naam en sleutel van een opslagaccount ophalen
In deze QuickStart gaat u de naam en sleutel van uw Azure Storage-account gebruiken. De volgende procedure bevat stappen waarmee u de naam en sleutel van uw opslagaccount kunt ophalen. 

1. Open een browser en navigeer naar [Azure Portal](https://portal.azure.com). Meld u aan met uw Azure-gebruikersnaam en -wachtwoord. 
2. Klik op **Meer services >** in het menu links, filter op het trefwoord **Opslag** en selecteer **Opslagaccounts**.

    ![Zoeken naar een opslagaccount](media/quickstart-create-data-factory-powershell/search-storage-account.png)
3. Filter in de lijst met opslagaccounts op uw opslagaccount (indien nodig) en selecteer vervolgens **uw opslagaccount**. 
4. Selecteer op de pagina **Opslagaccount** de optie **Toegangssleutels** in het menu.

    ![De naam en sleutel van het opslagaccount ophalen](media/quickstart-create-data-factory-powershell/storage-account-name-key.png)
5. Kopieer de waarden voor de velden **Opslagaccountnaam** en **key1** naar het klembord. Plak deze in Kladblok of een andere editor en sla ze op.  

#### <a name="create-input-folder-and-files"></a>Invoermap en bestanden maken
In deze sectie maakt u in uw Azure Blob-opslag een blobcontainer met de naam adftutorial. Vervolgens maakt u in de container een map met de naam Invoer en uploadt u een voorbeeldbestand in de map Invoer. 

1. Installeer [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) als deze nog niet op uw computer is geïnstalleerd. 
2. Start **Microsoft Azure Storage Explorer** op uw computer.   
3. Selecteer in het venster **Verbinding maken met Azure Storage** de optie **Een opslagaccountnaam en -sleutel gebruiken** en klik op **Volgende**. Als het venster **Verbinding maken met Azure Storage** niet wordt weergegeven, klikt u in de structuurweergave met de rechtermuisknop op **Opslagaccounts** en klikt u op **Verbinding maken met Azure Storage**. 

    ![Verbinding maken met Azure Storage](media/quickstart-create-data-factory-powershell/storage-explorer-connect-azure-storage.png)
4. Plak in het venster **Toevoegen met naam en sleutel** de **accountnaam** en de **accountsleutel** die u tijdens de vorige stap hebt opgeslagen. Klik op **Volgende**. 
5. In het venster **Samenvatting verbinding** klikt u op **Verbinding maken**.
6. Controleer of uw opslagaccount voorkomt in de structuurweergave onder **(Lokaal en Toegevoegd)** -> **Opslagaccounts**. 
7. Vouw **Blob-containers** uit en bevestig dat de blobcontainer **adftutorial** niet bestaat. Als deze al bestaat, slaat u de volgende stappen voor het maken van de container over. 
8. Klik met de rechtermuisknop op **Blob-containers** en selecteer **Blob-container maken**.

    ![Blob-container maken](media/quickstart-create-data-factory-powershell/stroage-explorer-create-blob-container-menu.png)
9. Voer **adftutorial** in als naam en druk op **ENTER**. 
10. Controleer of de container **adftutorial** in de structuurweergave is geselecteerd. 
11. Klik op **Nieuwe map** op de werkbalk. 

    ![Mapknop maken](media/quickstart-create-data-factory-powershell/stroage-explorer-new-folder-button.png)
12. Voer in het venster **Een nieuwe virtuele map maken** **gegevens** in bij **Naam** en klik op **OK**. 

    ![Dialoogvenster Map maken](media/quickstart-create-data-factory-powershell/storage-explorer-create-new-directory-dialog.png)
13. Start **Kladblok** en maak een bestand met de naam **emp.txt** met de volgende inhoud: 
    
    ```
    John, Doe
    Jane, Doe
    ```    
    Sla dit op in de map **c:\ADFv2QuickStartPSH**, maak de map **ADFv2QuickStartPSH** als deze niet al bestaat. 
14. Klik op **Uploaden** op de werkbalk en selecteer **Bestanden uploaden**. 

    ![Knop Uploaden](media/quickstart-create-data-factory-powershell/storage-explorer-upload-button.png)
15. Selecteer `...` in het venster **Bestanden uploaden** bij **Bestanden**. 
16. Navigeer in het venster **Map selecteren die u wilt uploaden** naar de map met **emp.txt** en selecteer het bestand. 

    ![Dialoogvenster Bestanden uploaden](media/quickstart-create-data-factory-powershell/storage-explorer-upload-files-dialog.png)
17. Klik in het venster **Bestanden uploaden** op **Uploaden**. 

### <a name="azure-powershell"></a>Azure PowerShell

#### <a name="install-azure-powershell"></a>Azure PowerShell installeren
Installeer de nieuwste versie van Azure PowerShell als u deze niet al op uw computer hebt. 

1. Navigeer in uw webbrowser naar de pagina [Azure SDK-downloads en SDK’s](https://azure.microsoft.com/downloads/). 
2. Klik op **Windows installeren** in de sectie **Opdrachtregelprogramma's** -> **PowerShell**. 
3. Voer het **MSI**-bestand uit om Azure PowerShell te installeren. 

Zie [Azure PowerShell installeren en configureren](/powershell/azure/install-azurerm-ps) voor gedetailleerde instructies. 

#### <a name="log-in-to-azure-powershell"></a>Aanmelden bij Azure PowerShell
Start **PowerShell** op uw computer. Houd Azure PowerShell geopend tot het einde van deze QuickStart. Als u het programma sluit en opnieuw opent, moet u deze opdrachten opnieuw uitvoeren.

1. Voer de volgende opdracht uit en geef de gebruikersnaam en het wachtwoord op waarmee u zich aanmeldt bij Azure Portal:
       
    ```powershell
    Login-AzureRmAccount
    ```        
2. Als u meerdere Azure-abonnementen hebt, voert u de volgende opdracht uit om alle abonnementen voor dit account weer te geven:

    ```powershell
    Get-AzureRmSubscription
    ```
3. Voer de volgende opdracht uit om het abonnement te selecteren waarmee u wilt werken. Vervang **SubscriptionId** door de id van uw Azure-abonnement:

    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<SubscriptionId>"       
    ```

## <a name="create-a-data-factory"></a>Een data factory maken
1. Definieer een variabele voor de naam van de resourcegroep die u later gaat gebruiken in PowerShell-opdrachten. Kopieer de tekst van de volgende opdracht naar PowerShell, geef tussen dubbele aanhalingstekens een naam op voor de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) en voer de opdracht uit. 
   
     ```powershell
    $resourceGroupName = "<Specify a name for the Azure resource group>";
    ```
2. Definieer een variabele voor de naam van de data factory die u later kunt gebruiken in PowerShell-opdrachten. 

    ```powershell
    $dataFactoryName = "<Specify a name for the data factory. It must be globally unique.>";
    ```
1. Definieer een variabele voor de locatie van de data factory: 

    ```powershell
    $location = "East US"
    ```
4. Voer de volgende opdracht uit om de resourcegroep te maken: 

    ```powershell
    New-AzureRmResourceGroup $resourceGroupName $location
    ``` 
    Als de resourcegroep al bestaat, wilt u waarschijnlijk niet dat deze wordt overschreven. Wijs een andere waarde toe aan de `$resourceGroupName`-variabele en probeer het opnieuw. Als u de resourcegroep met anderen wilt delen, gaat u verder met de volgende stap. 
5. Voer de volgende cmdlet **Set AzureRmDataFactoryV2** uit om de data factory te maken: 
    
    ```powershell       
    Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName 
    ```

Houd rekening met de volgende punten:

* De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als de volgende fout zich voordoet, wijzigt u de naam en probeert u het opnieuw.

    ```
    The specified Data Factory name 'ADFv2QuickStartDataFactory' is already in use. Data Factory names must be globally unique.
    ```

* Als u Data Factory-exemplaren wilt maken, moet u **bijdrager** of **beheerder** zijn van het Azure-abonnement.
* Momenteel kunt u met Data Factory versie 2 alleen data factory's maken in de regio's VS - oost, VS - oost 2 en West-Europa. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.

## <a name="create-a-linked-service"></a>Een gekoppelde service maken

Maak gekoppelde services in een data factory om uw gegevensarchieven en compute-services aan de gegevensfactory te koppelen. In deze QuickStart hoeft u maar één gekoppelde Azure Storage-service te maken, die in dit voorbeeld wordt gebruikt als zowel de bron- als de sinkopslag, met de naam 'AzureStorageLinkedService'.

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

U definieert een gegevensset die de gegevens vertegenwoordigt die van een bron naar een sink moeten worden gekopieerd. In dit voorbeeld verwijst deze Blob-gegevensset naar de gekoppelde Azure Storage-service die u in de vorige stap hebt gemaakt. De gegevensset heeft een parameter nodig waarvan de waarde wordt ingesteld in een activiteit die de gegevensset verbruikt. De parameter wordt gebruikt om het **folderPath** te maken dat verwijst naar de plaats waar de gegevens zich bevinden/zijn opgeslagen.

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
  
In dit voorbeeld bevat deze pijplijn één activiteit en heeft deze twee parameters: het pad van de invoerblob en het pad van de uitvoerblob. De waarden voor deze parameters worden ingesteld wanneer de pijplijn wordt geactiveerd of uitgevoerd. De kopieeractiviteit maakt gebruik van dezelfde blobgegevensset die u in de vorige stap heb gemaakt als invoer en uitvoer. Wanneer de dataset wordt gebruikt als invoergegevensset, wordt het invoerpad opgegeven. En wanneer de dataset wordt gebruikt als uitvoergegevensset, wordt het uitvoerpad opgegeven. 

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

    Vervang de waarde van **inputPath** en **outputPath** door het bron- en sinkblobpad als u andere containers en mappen gebruikt.

    ```json
    {
        "inputPath": "adftutorial/input",
        "outputPath": "adftutorial/output"
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
De uitvoermap wordt automatisch door de pijplijn gemaakt in de blobcontainer adftutorial. Vervolgens wordt het bestand emp.txt gekopieerd van de invoermap naar de uitvoermap. Gebruik [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) om te controleren of de blob(s) is/zijn gekopieerd van het 'inputBlobPath' naar het 'outputBlobPath'. 

## <a name="clean-up-resources"></a>Resources opschonen
De resources die u hebt gemaakt in de Quick Start kunt u op twee manieren opschonen. U kunt de [Azure-resourcegroep](../azure-resource-manager/resource-group-overview.md) verwijderen, met alle resources uit de resourcegroep. Als u de andere resources intact wilt houden, verwijdert u alleen de data factory die u in deze zelfstudie hebt gemaakt.

Voer de volgende opdracht uit om de gehele resourcegroep te verwijderen: 
```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

Voer de volgende opdracht uit om alleen de data factory te verwijderen: 

```powershell
Remove-AzureRmDataFactoryV2 -Name $dataFactoryName -ResourceGroupName $resourceGroupName
```

## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag. Doorloop de [zelfstudies](tutorial-copy-data-dot-net.md) voor meer informatie over het gebruiken van Data Factory in andere scenario's. 
