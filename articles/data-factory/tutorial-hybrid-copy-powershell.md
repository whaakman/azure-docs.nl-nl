---
title: "On-premises gegevens kopiëren naar de cloud met behulp van Azure Data Factory | Microsoft Docs"
description: "Meer informatie over het kopiëren van gegevens uit een on-premises gegevensopslag naar Azure-cloud met behulp van de zelf-hostende Integration Runtime in Azure Data Factory."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/06/2017
ms.author: jingwang
ms.openlocfilehash: 12ead91f6729aa3eb631cc453180ddea9bafe3df
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="copy-data-between-on-premises-and-cloud"></a>On-premises gegevens kopiëren naar de cloud

[!INCLUDE [data-factory-what-is-include-md](../../includes/data-factory-what-is-include.md)]

#### <a name="this-tutorial"></a>Deze zelfstudie

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u versie 1 van de Data Factory-service gebruikt, die algemeen beschikbaar is (GA), raadpleegt u [Documentatie van versie 1 van Data Factory](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

In deze zelfstudie gebruikt u Azure PowerShell om een Data Factory-pijplijn te maken waarmee gegevens worden gekopieerd van een on-premises SQL Server-database naar een Azure Blob-opslag. U maakt en gebruikt een zelf-hostende IR (Integration Runtime) van Azure Data Factory. Deze zorgt voor een integratie van on-premises gegevensopslagexemplaren en cloudgegevensopslag.  Zie [Quickstarts](quickstart-create-data-factory-dot-net.md) voor meer informatie over het gebruik van andere hulpprogramma's/SDK's voor het maken van een gegevensfactory.

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Maak een gegevensfactory.
> * Maak een zelf-hostende Integration Runtime.
> * Maak en versleutel een gekoppelde on-premises SQL Server-service op een zelf-hostende Integration Runtime.
> * Maak een gekoppelde Azure Storage-service.
> * Gegevenssets maakt voor SQL Server en Azure Storage.
> * Een pijplijn maakt met een kopieeractiviteit om de gegevens te verplaatsen.
> * Start een pijplijnuitvoering.
> * De uitvoering van de pijplijn en van de activiteit controleert.

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten

* **SQL Server**. In deze zelfstudie gebruikt u een on-premises SQL Server-database als een **brongegevensopslag**.
* **Azure Storage-account**. In deze zelfstudie gebruikt u Azure Blob-opslag als een **bestemming-/sinkgegevensopslag**. Als u geen Azure Storage-account hebt, raadpleegt u het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) voor de stappen voor het maken van een account.
* **Azure PowerShell**. Volg de instructies in [How to install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Azure PowerShell installeren en configureren).

## <a name="create-a-data-factory"></a>Een data factory maken

1. Start **PowerShell**. Houd Azure PowerShell open tot het einde van deze zelfstudie. Als u het programma sluit en opnieuw opent, moet u de opdrachten opnieuw uitvoeren.

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
2. Voer de cmdlet **Set AzureRmDataFactoryV2** uit om een data factory te maken. Vervang voordat u de opdracht uitvoert de tijdelijke aanduidingen door uw eigen waarden.

    ```powershell
    $resourceGroupName = "<your resource group to create the factory>"
    $dataFactoryName = "<specify the name of data factory to create. It must be globally unique.>"
    $df = Set-AzureRmDataFactoryV2 -ResourceGroupName $resourceGroupName -Location "East US" -Name $dataFactoryName
    ```

    Houd rekening met de volgende punten:

    * De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als de volgende fout zich voordoet, wijzigt u de naam en probeert u het opnieuw.

        ```
        Data factory name "<data factory name>" is not available.
        ```

    * Als u Data Factory-exemplaren wilt maken, moet u bijdrager of beheerder zijn van het Azure-abonnement.
    * Momenteel kunt u in Data Factory V2 alleen data factory's maken in de regio's VS - oost, VS - oost 2 en West-Europa. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.

## <a name="create-a-self-hosted-ir"></a>Een zelf-hostende IR maken

In deze sectie kunt u een zelf-hostende Integration Runtime maken en deze koppelen aan een on-premises knooppunt (machine).

1. Een zelf-hostende Integration Runtime maken. Gebruik een unieke naam voor het geval er al een Integration Runtime met dezelfde naam bestaat.

   ```powershell
   $integrationRuntimeName = "<your integration runtime name>"
   Set-AzureRmDataFactoryV2IntegrationRuntime -Name $integrationRuntimeName -Type SelfHosted -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName
   ```

   Hier volgt een voorbeeld van uitvoer:

   ```json
    Id                : /subscriptions/<subscription ID>/resourceGroups/ADFTutorialResourceGroup/providers/Microsoft.DataFactory/factories/onpremdf0914/integrationruntimes/myonpremirsp0914
    Type              : SelfHosted
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Name              : myonpremirsp0914
    Description       :
    ```
 

2. Voer de volgende opdracht uit om de status van de gemaakte zelf-hostende Integration Runtime op te halen.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntime -name $integrationRuntimeName -ResourceGroupName $resourceGroupName -DataFactoryName $dataFactoryName -Status
   ```

   Hier volgt een voorbeeld van uitvoer:

   ```json
   Nodes                     : {}
   CreateTime                : 9/14/2017 10:01:21 AM
   InternalChannelEncryption :
   Version                   :
   Capabilities              : {}
   ScheduledUpdateDate       :
   UpdateDelayOffset         :
   LocalTimeZoneOffset       :
   AutoUpdate                :
   ServiceUrls               : {eu.frontend.clouddatahub.net, *.servicebus.windows.net}
   ResourceGroupName         : <ResourceGroup name>
   DataFactoryName           : <DataFactory name>
   Name                      : <Integration Runtime name>
   State                     : NeedRegistration
   ```

3. Voer de volgende opdracht uit om verificatiesleutels op te halen voor het registreren van de zelf-hostende Integration Runtime met Data Factory-service in de cloud. Kopieer een van de sleutels voor het registreren van de zelf-hostende Integration Runtime.

   ```powershell
   Get-AzureRmDataFactoryV2IntegrationRuntimeKey -Name $integrationRuntimeName -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName | ConvertTo-Json
   ```

   Hier volgt een voorbeeld van uitvoer:

   ```json
   {
       "AuthKey1":  "IR@8437c862-d6a9-4fb3-87dd-7d4865a9e845@ab1@eu@VDnzgySwUfaj3pfSUxpvfsXXXXXXx4GHiyF4wboad0Y=",
       "AuthKey2":  "IR@8437c862-d6a9-4fb3-85dd-7d4865a9e845@ab1@eu@sh+k/QNJGBltXL46vXXXXXXXXOf/M1Gne5aVqPtbweI="
   }
   ```

4. [Download](https://www.microsoft.com/download/details.aspx?id=39717) de zelf-hostende Integration Runtime op een lokale Windows-machine en gebruik de in de vorige stap verkregen verificatiesleutel om de zelf-hostende Integration Runtime handmatig te registreren.

   ![Integration Runtime registreren](media/tutorial-hybrid-copy-powershell/register-integration-runtime.png)

   U ziet het volgende bericht wanneer de zelf-hostende Integration Runtime is geregistreerd:

   ![Registratie is voltooid](media/tutorial-hybrid-copy-powershell/registered-successfully.png)

   Wanneer het knooppunt is verbonden met de cloudservice, ziet u de volgende pagina:

   ![Knooppunt is verbonden](media/tutorial-hybrid-copy-powershell/node-is-connected.png)

## <a name="create-linked-services"></a>Gekoppelde services maken

### <a name="create-an-azure-storage-linked-service-destinationsink"></a>Een gekoppelde Azure Storage-service maken (bestemming/sink)

1. Maak een JSON-bestand met de naam **AzureStorageLinkedService.json** in de map **C:\ADFv2Tutorial** met de volgende inhoud. Maak de map ADFv2Tutorial als deze nog niet bestaat.  Vervang &lt;accountname&gt; en &lt;accountkey&gt; door de naam en de sleutel van uw Azure Storage-account.

   ```json
    {
        "properties": {
            "type": "AzureStorage",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
                }
            }
        },
        "name": "AzureStorageLinkedService"
    }
   ```

2. Schakel in **Azure PowerShell** over naar de map **ADFv2Tutorial**.

   Voer de cmdlet **Set-AzureRmDataFactoryV2LinkedService** uit om de gekoppelde service **AzureStorageLinkedService** te maken. De cmdlets die worden gebruikt in deze zelfstudie, nemen de waarden voor de parameters **ResourceGroupName** en **DataFactoryName**. U kunt ook het **DataFactory**-object doorgeven dat met de cdmlet Set-AzureRmDataFactoryV2 is geretourneerd. U hoeft dan niet telkens wanneer u een cmdlet uitvoert ResourceGroupName en DataFactoryName op te geven.

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
   ```

   Hier volgt een voorbeeld van uitvoer:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

### <a name="create-and-encrypt-a-sql-server-linked-service-source"></a>Een gekoppelde SQL Server-service (bron) maken en versleutelen

1. Maak een JSON-bestand met de naam **SqlServerLinkedService.json** in de map **C:\ADFv2Tutorial** met de volgende inhoud: vervang voordat u het bestand opslaat **&lt;servername>**, **&lt;databasename>**, **&lt;username>**, **&lt;servername>**, en **&lt;password>** door de waarden van uw SQL-server. Vervang de **naam>** van de **&lt;Integration** **Runtime** door de naam van uw Integration Runtime.

    ```json
    {
        "properties": {
            "type": "SqlServer",
            "typeProperties": {
                "connectionString": {
                    "type": "SecureString",
                    "value": "Server=<servername>;Database=<databasename>;User ID=<username>;Password=<password>;Timeout=60"
                }
            },
            "connectVia": {
                "type": "integrationRuntimeReference",
                "referenceName": "<integration runtime name>"
            }
        },
        "name": "SqlServerLinkedService"
    }
   ```
2. We kunnen **New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential** uitvoeren en de bovenstaande JSON-nettolading doorgeven om de gevoelige gegevens uit de JSON-nettolading te versleutelen in de on-premises Integration Runtime. Deze versleuteling zorgt ervoor dat de referenties zijn versleuteld met behulp van DPAPI (Data Protection Application Programming Interface) en lokaal zijn opgeslagen op het zelf-hostende Integration Runtime-knooppunt. De uitvoernettolading kan worden omgeleid naar een ander JSON-bestand (in dit geval encryptedLinkedService.json). Dit bestand bevat de versleutelde referenties.

    Vervang voordat u de opdracht uitvoert de **&lt;naam van de Integration Runtime&gt;** door de naam van uw Integration Runtime.

   ```powershell
   New-AzureRmDataFactoryV2LinkedServiceEncryptedCredential -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -IntegrationRuntimeName <integration runtime name> -File ".\SQLServerLinkedService.json" > encryptedSQLServerLinkedService.json
   ```

3. Voer de volgende opdracht uit door het JSON-bestand uit de vorige stap te gebruiken om de **SqlServerLinkedService** te maken:

   ```powershell
   Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $ResourceGroupName -Name "EncryptedSqlServerLinkedService" -File ".\encryptedSqlServerLinkedService.json"
   ```


## <a name="create-datasets"></a>Gegevenssets maken

### <a name="prepare-an-on-premises-sql-server-for-the-tutorial"></a>Een on-premises SQL-server voorbereiden voor de zelfstudie

In deze stap maakt u invoer- en uitvoergegevenssets die invoer- en uitvoergegevenssets voor de kopieerbewerking vertegenwoordigen (on-premises SQL Server-database => Azure Blob-opslag). Voordat u de gegevenssets maakt, voert u de volgende stappen uit (gedetailleerde stappen vindt u na de lijst):

- Maak een tabel met de naam **emp** in de SQL Server-database die u als gekoppelde service hebt toegevoegd aan de gegevensfactory, en voer een aantal voorbeeldvermeldingen toe aan de tabel.
- Maak een blobcontainer met de naam **adftutorial** in het Azure Blob-opslagaccount dat u als gekoppelde service hebt toegevoegd aan de gegevensfactory.


1. Gebruik in de database die u hebt opgegeven voor de met de on-premises SQL-server gekoppelde service, (**SqlServerLinkedService**) het volgende SQL-script om de tabel **emp** te maken in de database.

   ```sql   
     CREATE TABLE dbo.emp
     (
         ID int IDENTITY(1,1) NOT NULL,
         FirstName varchar(50),
         LastName varchar(50),
         CONSTRAINT PK_emp PRIMARY KEY (ID)
     )
     GO
   ```

2. Voer een aantal voorbeelden in de tabel in:

   ```sql
     INSERT INTO emp VALUES ('John', 'Doe')
     INSERT INTO emp VALUES ('Jane', 'Doe')
   ```



### <a name="create-a-dataset-for-source-sql-database"></a>Een gegevensset maken voor bron SQL-Database

1. Maak een JSON-bestand met de naam **SqlServerDataset.json** in de map **C:\ADFv2Tutorial** met de volgende inhoud:  

    ```json
    {
       "properties": {
            "type": "SqlServerTable",
            "typeProperties": {
                "tableName": "dbo.emp"
            },
            "structure": [
                 {
                    "name": "ID",
                    "type": "String"
                },
                {
                    "name": "FirstName",
                    "type": "String"
                },
                {
                    "name": "LastName",
                    "type": "String"
                }
            ],
            "linkedServiceName": {
                "referenceName": "EncryptedSqlServerLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "SqlServerDataset"
    }
    ```

2. De gegevensset **SqlServerDataset** maken: voer de cmdlet **Set-AzureRmDataFactoryV2Dataset** uit.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SqlServerDataset" -File ".\SqlServerDataset.json"
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```json
    DatasetName       : SqlServerDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         : {"name": "ID" "type": "String", "name": "FirstName" "type": "String", "name": "LastName" "type": "String"}
    Properties        : Microsoft.Azure.Management.DataFactory.Models.SqlServerTableDataset
    ```

### <a name="create-a-dataset-for-sink-azure-blob-storage"></a>Een gegevensset maken voor Azure Blob-sinkopslag

1. Maak een JSON-bestand met de naam **AzureBlobDataset.json** in de map **C:\ADFv2Tutorial** met de volgende inhoud:

    > [!IMPORTANT]
    > In deze voorbeeldcode wordt ervan uitgegaan dat u een container hebt met de naam **adftutorial** in de Azure Blob-opslag.

    ```json
    {
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/fromonprem",
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        },
        "name": "AzureBlobDataset"
    }
    ```

2. De gegevensset **AzureBlobDataset** maken: voer de cmdlet **Set-AzureRmDataFactoryV2Dataset** uit.

    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureBlobDataset" -File ".\AzureBlobDataset.json"
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```json
    DatasetName       : AzureBlobDataset
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset
    ```

## <a name="create-pipelines"></a>Pijplijnen maken

### <a name="create-the-pipeline-sqlservertoblobpipeline"></a>De pijplijn SqlServerToBlobPipeline maken

1. Maak een JSON-bestand met de naam **SqlServerToBlobPipeline.json** in de map **C:\ADFv2Tutorial** met de volgende inhoud:

    ```json
    {
       "name": "SQLServerToBlobPipeline",
        "properties": {
            "activities": [       
                {
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource"
                        },
                        "sink": {
                            "type":"BlobSink"
                        }
                    },
                    "name": "CopySqlServerToAzureBlobActivity",
                    "inputs": [
                        {
                            "referenceName": "SqlServerDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "AzureBlobDataset",
                            "type": "DatasetReference"
                        }
                    ]
                }
            ]
        }
    }
    ```

2. De pijplijn **SQLServerToBlobPipeline** maken: voer de cmdlet **Set-AzureRmDataFactoryV2Pipeline** uit.

    ```powershell
    Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SQLServerToBlobPipeline" -File ".\SQLServerToBlobPipeline.json"
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```json
    PipelineName      : SQLServerToBlobPipeline
    ResourceGroupName : ADFTutorialResourceGroup
    DataFactoryName   : onpremdf0914
    Activities        : {CopySqlServerToAzureBlobActivity}
    Parameters        :  
    ```



## <a name="start-and-monitor-a-pipeline-run"></a>Een pijplijnuitvoering starten en controleren

1. Start een pijplijnuitvoering voor de pijplijn SQLServerToBlobPipeline en leg de id voor de pijplijnuitvoering vast voor toekomstige controle.  

    ```powershell
    $runId = Invoke-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineName 'SQLServerToBlobPipeline'
    ```

2. Voer het volgende script uit om continu de uitvoeringsstatus van de pijplijn **SQLServerToBlobPipeline** te controleren, en druk het eindresultaat af.

    ```powershell
    while ($True) {
        $result = Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)

        if (($result | Where-Object { $_.Status -eq "InProgress" } | Measure-Object).count -ne 0) {
            Write-Host "Pipeline run status: In Progress" -foregroundcolor "Yellow"
            Start-Sleep -Seconds 30
        }
        else {
            Write-Host "Pipeline 'SQLServerToBlobPipeline' run finished. Result:" -foregroundcolor "Yellow"
            $result
            break
        }
    }
    ```

    Hier volgt een voorbeeld van de voorbeelduitvoer:

    ```jdon
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    ActivityName      : copy
    PipelineRunId     : 4ec8980c-62f6-466f-92fa-e69b10f33640
    PipelineName      : SQLServerToBlobPipeline
    Input             :  
    Output            :  
    LinkedServiceName :
    ActivityRunStart  : 9/13/2017 1:35:22 PM
    ActivityRunEnd    : 9/13/2017 1:35:42 PM
    DurationInMs      : 20824
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    ```

3. U kunt de run-id van de pijplijn **SQLServerToBlobPipeline** ophalen en vervolgens het gedetailleerde uitvoeringsresultaat van de activiteit controleren.

    ```powershell
    Write-Host "Pipeline 'SQLServerToBlobPipeline' run result:" -foregroundcolor "Yellow"
    ($result | Where-Object {$_.ActivityName -eq "CopySqlServerToAzureBlobActivity"}).Output.ToString()
    ```

    Hier volgt een voorbeeld van de voorbeelduitvoer:

    ```json
    {
      "dataRead": 36,
      "dataWritten": 24,
      "rowsCopied": 2,
      "copyDuration": 4,
      "throughput": 0.01,
      "errors": []
    }
    ```
4. Maak verbinding met de Azure Blob-sinkopslag en bevestig dat de gegevens juist zijn gekopieerd vanuit Azure SQL Database.

## <a name="next-steps"></a>Volgende stappen
Met de pijplijn in dit voorbeeld worden gegevens gekopieerd van de ene locatie naar een andere locatie in een Azure Blob-opslag. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een gegevensfactory maakt.
> * Een zelf-hostende Integration Runtime maakt
> * Een gekoppelde on-premises SQL Server-service maakt en versleutelt op een zelf-hostende Integration Runtime
> * Een gekoppelde Azure Storage-service maakt.
> * Gegevenssets maakt voor SQL Server en Azure Storage.
> * Een pijplijn maakt met een kopieeractiviteit om de gegevens te verplaatsen.
> * Start een pijplijnuitvoering.
> * De uitvoering van de pijplijn en van de activiteit controleert.

Zie het artikel [Ondersteunde gegevensopslagexemplaren](copy-activity-overview.md#supported-data-stores-and-formats) voor een lijst met gegevensopslagexemplaren die worden ondersteund als bronnen en sinks in Azure Data Factory.

Ga door naar de volgende zelfstudie voor informatie over het bulksgewijs kopiëren van gegevens uit een bron naar een bestemming:

> [!div class="nextstepaction"]
>[Gegevens bulksgewijs kopiëren](tutorial-bulk-copy.md)
