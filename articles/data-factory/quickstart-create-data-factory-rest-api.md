---
title: Een Azure data factory maken met behulp van REST-API | Microsoft Docs
description: "Maak een Azure data factory om gegevens te kopiëren van een locatie in een Azure Blob Storage naar een andere locatie in dezelfde Blob Storage."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: 
ms.devlang: rest-api
ms.topic: hero-article
ms.date: 01/22/2018
ms.author: jingwang
ms.openlocfilehash: 9c12c5e016e6bfa5dceab6dc77086fe018c43f59
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="create-an-azure-data-factory-and-pipeline-by-using-the-rest-api"></a>Een Azure data factory en pijplijn maken door de REST-API te gebruiken
> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versie 1 - Algemene beschikbaarheid](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Versie 2 - Preview](quickstart-create-data-factory-rest-api.md)

Azure Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee u gegevensgestuurde werkstromen kunt maken in de cloud. Op deze manier kunt u de verplaatsing en transformatie van gegevens indelen en automatiseren. Met Azure Data Factory kunt u gegevensgestuurde werkstromen (ook wel pijplijnen) maken en plannen die gegevens uit verschillende gegevensarchieven kunnen opnemen en de gegevens kunnen verwerken/transformeren met behulp van rekenservices zoals Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics en Azure Machine Learning. Daarnaast kunt u de uitvoergegevens publiceren naar gegevensarchieven zoals Azure SQL Data Warehouse, zodat BI-toepassingen (business intelligence) ze kunnen gebruiken. 

In deze snelstartgids wordt beschreven hoe u REST-API kunt gebruiken om een Azure data factory te maken. Met de pijplijn in deze data factory worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Aan de slag met versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* **Azure-abonnement**. Als u geen abonnement hebt, kunt u een [gratis proefaccount](http://azure.microsoft.com/pricing/free-trial/) maken.
* **Een Azure Storage-account**. U gebruikt de blob-opslag als gegevensopslag voor **bron** en **sink**. Als u geen Azure-opslagaccount hebt, raadpleegt u het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) om een account te maken.
* Maak een **blob-container** in Blob Storage, maak een **invoermap** in de container en upload een aantal bestanden naar de map. U kunt hulpprogramma's zoals [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) gebruiken om verbinding te maken met Azure Blob Storage, een blob-container te maken, een invoerbestand te uploaden en het uitvoerbestand te controleren.
* Installeer **Azure PowerShell**. Volg de instructies in [How to install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Azure PowerShell installeren en configureren). Deze snelstartgids maakt gebruik van PowerShell om REST-API-aanroepen aan te roepen.
* **Maak een toepassing in Azure Active Directory** aan de hand van [deze instructie](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application). Noteer de volgende waarden voor gebruik in latere stappen: **toepassings-id**, **verificatiesleutel** en **tenant-id**. Wijs de toepassing toe aan de rol '**Inzender**'.

## <a name="set-global-variables"></a>Globale variabelen instellen

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
2. Voer de volgende opdrachten uit nadat u de plaatsaanduidingen hebt vervangen door uw eigen waarden, om globale variabelen in te stellen voor gebruik in latere stappen.

    ```powershell
    $tenantID = "<your tenant ID>"
    $appId = "<your application ID>"
    $authKey = "<your authentication key for the application>"
    $subsId = "<your subscription ID to create the factory>"
    $resourceGroup = "<your resource group to create the factory>"
    $dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>"
    $apiVersion = "2017-09-01-preview"
    ```

## <a name="authenticate-with-azure-ad"></a>Verifiëren met Azure AD

Voer de volgende opdrachten uit om te verifiëren met Azure Active Directory (AAD):

```powershell
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]"https://login.microsoftonline.com/${tenantId}"
$cred = New-Object -TypeName Microsoft.IdentityModel.Clients.ActiveDirectory.ClientCredential -ArgumentList ($appId, $authKey)
$result = $AuthContext.AcquireToken("https://management.core.windows.net/", $cred)
$authHeader = @{
'Content-Type'='application/json'
'Accept'='application/json'
'Authorization'=$result.CreateAuthorizationHeader()
} 
```

## <a name="create-a-data-factory"></a>Een gegevensfactory maken

Voer de volgende opdrachten uit om een data factory te maken:

```powershell
$request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}?api-version=${apiVersion}"
$body = @"
{
    "name": "$dataFactoryName",
    "location": "East US",
    "properties": {},
    "identity": {
        "type": "SystemAssigned"
    }
}
"@
$response = Invoke-RestMethod -Method PUT -Uri $request -Header $authHeader -Body $body
$response | ConvertTo-Json
```

Houd rekening met de volgende punten:

* De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als de volgende fout zich voordoet, wijzigt u de naam en probeert u het opnieuw.

    ```
    Data factory name "ADFv2QuickStartDataFactory" is not available.
    ```
* Momenteel kunt u in Data Factory V2 alleen data factory's maken in de regio's VS - oost, VS - oost 2 en West-Europa. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.

Dit is het voorbeeldantwoord:

```json

{
    "name":  "<dataFactoryName>",
    "tags": {

            },
    "properties":  {
        "provisioningState":  "Succeeded",
        "loggingStorageAccountKey":  "**********",
        "createTime":  "2017-09-14T06:22:59.9106216Z",
        "version":  "2017-09-01-preview"
    },
    "identity":  {
        "type":  "SystemAssigned",
        "principalId":  "<service principal ID>",
        "tenantId":  "<tenant ID>"
    },
    "id":  "dataFactoryName",
    "type":  "Microsoft.DataFactory/factories",
    "location":  "East US"
} 

```

## <a name="create-linked-services"></a>Gekoppelde services maken

U maakt gekoppelde services in een gegevensfactory om uw gegevensarchieven en compute-services aan de gegevensfactory te koppelen. In deze snelstartgids hoeft u maar één gekoppelde Azure Storage-service te maken die in het voorbeeld wordt gebruikt als zowel de bron voor het kopiëren en als de sinkopslag, met de naam 'AzureStorageLinkedService'.

Voer de volgende opdrachten uit om een gekoppelde service te maken met de naam **AzureStorageLinkedService**:

Vervang &lt;accountName&gt; en &lt;accountKey&gt; door de naam en sleutel van uw Azure-opslagaccount voordat u de opdrachten uitvoert.

```powershell
$request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/linkedservices/AzureStorageLinkedService?api-version=${apiVersion}"
$body = @"
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
"@
$response = Invoke-RestMethod -Method PUT -Uri $request -Header $authHeader -Body $body
$response | ConvertTo-Json
```

Hier volgt een voorbeeld van uitvoer:

```json
{
    "id":  "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<dataFactoryName>/linkedservices/AzureStorageLinkedService",
    "name":  "AzureStorageLinkedService",
    "properties":  {
                       "type":  "AzureStorage",
                       "typeProperties":  {
                                              "connectionString":  "@{value=**********; type=SecureString}"
                                          }
                   },
    "etag":  "0000c552-0000-0000-0000-59b1459c0000"
}
```

## <a name="create-datasets"></a>Gegevenssets maken

U definieert een gegevensset die de gegevens vertegenwoordigt die van een bron naar een sink moeten worden gekopieerd. In dit voorbeeld verwijst deze Blob-gegevensset naar de gekoppelde Azure Storage-service die u in de vorige stap hebt gemaakt. De gegevensset heeft een parameter nodig waarvan de waarde wordt ingesteld in een activiteit die de gegevensset verbruikt. De parameter wordt gebruikt om het 'folderPath' te maken dat verwijst naar de plaats waar de gegevens zich bevinden/zijn opgeslagen.

```powershell
$request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/datasets/BlobDataset?api-version=${apiVersion}"
$body = @"
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
"@
$response = Invoke-RestMethod -Method PUT -Uri $request -Header $authHeader -Body $body
$response | ConvertTo-Json
```

Hier volgt een voorbeeld van uitvoer:

```json
{
    "id":  "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<dataFactoryName>/datasets/BlobDataset",
    "name":  "BlobDataset",
    "properties":  {
                       "type":  "AzureBlob",
                       "typeProperties":  {
                                              "folderPath":  "@{value=@{dataset().path}; type=Expression}"
                                          },
                       "linkedServiceName":  {
                                                 "referenceName":  "AzureStorageLinkedService",
                                                 "type":  "LinkedServiceReference"
                                             },
                       "parameters":  {
                                          "path":  "@{type=String}"
                                      }
                   },
    "etag":  "0000c752-0000-0000-0000-59b1459d0000"
}
```

## <a name="create-pipeline"></a>Pijplijn maken

In dit voorbeeld bevat deze pijplijn één activiteit en heeft deze twee parameters: het pad van de invoerblob en het pad van de uitvoerblob. De waarden voor deze parameters worden ingesteld wanneer de pijplijn wordt geactiveerd of uitgevoerd. De kopieeractiviteit verwijst naar dezelfde blobgegevensset die u in de vorige stap heb gemaakt als invoer en uitvoer. Wanneer de dataset wordt gebruikt als invoergegevensset, wordt het invoerpad opgegeven. En wanneer de dataset wordt gebruikt als uitvoergegevensset, wordt het uitvoerpad opgegeven. 

```powershell
$request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelines/Adfv2QuickStartPipeline?api-version=${apiVersion}"
$body = @"
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
"@
$response = Invoke-RestMethod -Method PUT -Uri $request -Header $authHeader -Body $body
$response | ConvertTo-Json
```

Hier volgt een voorbeeld van uitvoer:

```json
{
    "id":  "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.DataFactory/factories/<dataFactoryName>/pipelines/Adfv2QuickStartPipeline",
    "name":  "Adfv2QuickStartPipeline",
    "properties":  {
                       "activities":  [
                                          "@{name=CopyFromBlobToBlob; type=Copy; inputs=System.Object[]; outputs=System.Object[]; typeProperties=}"
                                      ],
                       "parameters":  {
                                          "inputPath":  "@{type=String}",
                                          "outputPath":  "@{type=String}"
                                      }
                   },
    "etag":  "0000c852-0000-0000-0000-59b1459e0000"
}
```

## <a name="create-pipeline-run"></a>Pijplijnuitvoering maken

In deze stap stelt u de waarden van de parameters **inputPath** en **outputPath** die in de pijplijn worden opgegeven, in op de werkelijke waarden van de bron- en sinkblobpaden, en activeert u het uitvoeren van de pijplijn. De pijplijn-run-id die in de hoofdtekst van het antwoord wordt geretourneerd, wordt later in de bewakings-API gebruikt.

Vervang voordat u het bestand opslaat de waarde van **inputPath** en **outputPath** door het bron- en sinkblobpad waarvan en waarnaar u gegevens kopieert.


```powershell
$request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelines/Adfv2QuickStartPipeline/createRun?api-version=${apiVersion}"
$body = @"
{
    "inputPath": "<the path to existing blob(s) to copy data from, e.g. containername/path>",
    "outputPath": "<the blob path to copy data to, e.g. containername/path>"
}
"@
$response = Invoke-RestMethod -Method POST -Uri $request -Header $authHeader -Body $body
$response | ConvertTo-Json
$runId = $response.runId
```

Hier volgt een voorbeeld van uitvoer:

```json
{
    "runId":  "2f26be35-c112-43fa-9eaa-8ba93ea57881"
}
```

## <a name="monitor-pipeline"></a>De pijplijn bewaken

1. Voer het volgende script uit om continu de status van de pijplijnuitvoering te controleren totdat het kopiëren van de gegevens is voltooid.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}?api-version=${apiVersion}"
    while ($True) {
        $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
        Write-Host  "Pipeline run status: " $response.Status -foregroundcolor "Yellow"

        if ($response.Status -eq "InProgress") {
            Start-Sleep -Seconds 15
        }
        else {
            $response | ConvertTo-Json
            break
        }
    }
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```json
    {
        "key":  "000000000-0000-0000-0000-00000000000",
        "timestamp":  "2017-09-07T13:12:39.5561795Z",
        "runId":  "000000000-0000-0000-0000-000000000000",
        "dataFactoryName":  "<dataFactoryName>",
        "pipelineName":  "Adfv2QuickStartPipeline",
        "parameters":  [
                        "inputPath: <inputBlobPath>",
                        "outputPath: <outputBlobPath>"
                    ],
        "parametersCount":  2,
        "parameterNames":  [
                            "inputPath",
                            "outputPath"
                        ],
        "parameterNamesCount":  2,
        "parameterValues":  [
                                "<inputBlobPath>",
                                "<outputBlobPath>"
                            ],
        "parameterValuesCount":  2,
        "runStart":  "2017-09-07T13:12:00.3710792Z",
        "runEnd":  "2017-09-07T13:12:39.5561795Z",
        "durationInMs":  39185,
        "status":  "Succeeded",
        "message":  ""
    }
    ```

2. Voer het volgende script uit om uitvoeringsdetails van de kopieeractiviteit op te halen, zoals de omvang van de gelezen of weggeschreven gegevens.

    ```PowerShell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}/activityruns?api-version=${apiVersion}&startTime="+(Get-Date).ToString('yyyy-MM-dd')+"&endTime="+(Get-Date).AddDays(1).ToString('yyyy-MM-dd')+"&pipelineName=Adfv2QuickStartPipeline"
    $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
    $response | ConvertTo-Json
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```json
    {
        "value":  [
                    {
                        "id":  "000000000-0000-0000-0000-00000000000",
                        "timestamp":  "2017-09-07T13:12:38.4780542Z",
                        "pipelineRunId":  "000000000-0000-00000-0000-0000000000000",
                        "pipelineName":  "Adfv2QuickStartPipeline",
                        "status":  "Succeeded",
                        "failureType":  "",
                        "linkedServiceName":  "",
                        "activityName":  "CopyFromBlobToBlob",
                        "activityType":  "Copy",
                        "activityStart":  "2017-09-07T13:12:02.3299261Z",
                        "activityEnd":  "2017-09-07T13:12:38.4780542Z",
                        "duration":  36148,
                        "input":  "@{source=; sink=}",
                        "output":  "@{dataRead=331452208; dataWritten=331452208; copyDuration=22; throughput=14712.9; errors=System.Object[]; effectiveIntegrationRuntime=DefaultIntegrationRuntime (West US); usedCloudDataMovementUnits=2; billedDuration=22}",
                        "error":  "@{errorCode=; message=; failureType=; target=CopyFromBlobToBlob}"
                    }
                ]
    }
    ```

## <a name="verify-the-output"></a>De uitvoer controleren

Gebruik Azure Storage Explorer om te controleren of de blob(s) is/zijn gekopieerd van het 'inputBlobPath' naar het 'outputBlobPath' dat u hebt opgegeven bij het maken van een pijplijnuitvoering.

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