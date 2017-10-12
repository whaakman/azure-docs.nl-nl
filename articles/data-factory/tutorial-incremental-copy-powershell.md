---
title: "Stapsgewijs gegevens kopiëren met behulp van Azure Data Factory | Microsoft Docs"
description: In deze zelfstudie maakt u een Azure Data Factory-pijplijn waarmee gegevens uit een Azure Blob-opslag stapsgewijs worden gekopieerd naar een Azure SQL-database.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/10/2017
ms.author: shlo
ms.openlocfilehash: 91b632b6d2c2917acf17e9d89c1b5a4b0f8b1c33
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="incrementally-load-data-from-azure-sql-database-to-azure-blob-storage"></a>Stapsgewijs gegevens uit Azure SQL Database laden in Azure Blob Storage
Azure Data Factory is een cloudgebaseerde gegevensintegratieservice waarmee u gegevensgestuurde werkstromen kunt maken in de cloud. Op deze manier kunt u de verplaatsing en transformatie van gegevens indelen en automatiseren. Met Azure Data Factory kunt u gegevensgestuurde werkstromen (ook wel pijplijnen) maken en plannen die gegevens uit verschillende gegevensarchieven kunnen opnemen en de gegevens kunnen verwerken/transformeren met behulp van rekenservices zoals Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics en Azure Machine Learning. Daarnaast kunt u de uitvoergegevens publiceren naar gegevensarchieven zoals Azure SQL Data Warehouse, zodat BI-toepassingen (business intelligence) ze kunnen gebruiken. 

Tijdens de integratie van de gegevens is een van de meest gebruikte scenario's om gegevens periodiek stapsgewijs te laden om bijgewerkte analyseresultaten te vernieuwen na het initiële laden en analyseren van de gegevens. In deze zelfstudie richt u zich alleen op het laden van nieuwe of bijgewerkte records uit de gegevensbronnen in gegevenssinks. Dit verloopt efficiënter vergeleken bij volledig laden, met name bij grote gegevenssets.    

U kunt Data Factory gebruiken om bovengrensoplossingen te maken voor het stapsgewijs laden van gegevens met behulp van de activiteiten Opzoeken, Kopiëren en Opgeslagen procedure in een pijplijn.  

In deze zelfstudie voert u de volgende stappen uit:

> [!div class="checklist"]
> * Bereid de gegevensopslag voor om de grenswaarde in op te slaan.   
> * Maak een gegevensfactory.
> * Maak gekoppelde services. 
> * Maak bron-, sink- en grenswaardegegevenssets.
> * Maak een pijplijn.
> * Voer de pijplijn uit.
> * De pijplijnuitvoering controleert. 

## <a name="overview"></a>Overzicht
De diagramoplossing op hoog niveau is: 

![Stapsgewijs gegevens laden](media\tutorial-Incrementally-load-data-from-azure-sql-to-blob\incrementally-load.png)

Dit zijn de belangrijke stappen bij het maken van deze oplossing: 

1. **Selecteer de grenswaardekolom**.
    Selecteer één kolom in de brongegevensopslag die kan worden gebruikt om de nieuwe of bijgewerkte records voor elke uitvoering te segmenteren. Normaal gesproken nemen de gegevens in deze geselecteerde kolom (bijvoorbeeld, last_modify_time of id) toe wanneer de rijen worden gemaakt of bijgewerkt. De maximale waarde in deze kolom wordt gebruikt als grenswaarde.
2. **Bereid een gegevensopslag voor om de grenswaarde in op te slaan**.   
    In deze zelfstudie slaat u de grenswaarde op in een Azure SQL-database.
3. **Maak een pijplijn met de volgende werkstroom:** 
    
    De pijplijn in deze oplossing heeft de volgende activiteiten:
  
    1. Maak twee **opzoekactiviteiten**. Gebruik de eerste opzoekactiviteit om de laatste grenswaarde op te halen. Gebruik de tweede opzoekactiviteit om de nieuwe grenswaarde op te halen. Deze grenswaarden worden doorgegeven aan de kopieeractiviteit. 
    2. Maak een **kopieeractiviteit** waarmee rijen uit de brongegevensopslag worden gekopieerd met een waarde uit de grenswaardekolom die groter is dan de oude grenswaarde en kleiner dan de nieuwe grenswaarde. Vervolgens worden de deltagegevens uit de brongegevensopslag als een nieuw bestand gekopieerd naar een Blob-opslag. 
    3. Maak een **opgeslagen procedureactiviteit** waarmee de grenswaarde wordt bijgewerkt voor de pijplijn die de volgende keer wordt uitgevoerd. 


Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="prerequisites"></a>Vereisten
* **Azure SQL-database**. U gebruikt de database als de **brongegevensopslag**. Als u geen Azure SQL-database hebt, raadpleegt u het artikel [Een Azure SQL-database maken](../sql-database/sql-database-get-started-portal.md) voor de stappen voor het maken van een account.
* **Azure Storage-account**. U gebruikt de Blob-opslag als de **sinkgegevensopslag**. Als u geen Azure Storage-account hebt, raadpleegt u het artikel [Een opslagaccount maken](../storage/common/storage-create-storage-account.md#create-a-storage-account) voor de stappen voor het maken van een account. Maak een container met de naam **adftutorial**. 
* **Azure PowerShell**. Volg de instructies in [How to install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps) (Azure PowerShell installeren en configureren).

### <a name="create-a-data-source-table-in-your-azure-sql-database"></a>Een gegevensbrontabel maken in de Azure SQL-database
1. Open **SQL Server Management Studio** in **Server Explorer**, klik met de rechtermuisknop op de database en kies de **Nieuwe query**.
2. Voer de volgende SQL-opdracht uit voor de Azure SQL-database om een tabel met de naam `data_source_table` te maken als gegevensbronopslag.  
    
    ```sql
    create table data_source_table
    (
        PersonID int,
        Name varchar(255),
        LastModifytime datetime
    );

    INSERT INTO data_source_table
    (PersonID, Name, LastModifytime)
    VALUES
    (1, 'aaaa','9/1/2017 12:56:00 AM'),
    (2, 'bbbb','9/2/2017 5:23:00 AM'),
    (3, 'cccc','9/3/2017 2:36:00 AM'),
    (4, 'dddd','9/4/2017 3:21:00 AM'),
    (5, 'eeee','9/5/2017 8:06:00 AM');
    ```
    In deze zelfstudie gebruikt u **LastModifytime** als de **grenswaardekolom**.  De gegevens in de brongegevensopslag worden weergegeven in de volgende tabel:

    ```
    PersonID | Name | LastModifytime
    -------- | ---- | --------------
    1 | aaaa | 2017-09-01 00:56:00.000
    2 | bbbb | 2017-09-02 05:23:00.000
    3 | cccc | 2017-09-03 02:36:00.000
    4 | dddd | 2017-09-04 03:21:00.000
    5 | eeee | 2017-09-05 08:06:00.000
    ```

### <a name="create-another-table-in-sql-database-to-store-the-high-watermark-value"></a>Nog een tabel in SQL Database maken om de bovengrenswaarde op te slaan
1. Voer de volgende SQL-opdracht uit voor de Azure SQL-database om een tabel met de naam `watermarktable` te maken om de grenswaarde op te slaan.  
    
    ```sql
    create table watermarktable
    (
    
    TableName varchar(255),
    WatermarkValue datetime,
    );
    ```
3. Stel de **standaardwaarde** van de bovengrens in met de tabelnaam van de brongegevensopslag.  (In deze zelfstudie is de tabelnaam: **data_source_table**)

    ```sql
    INSERT INTO watermarktable
    VALUES ('data_source_table','1/1/2010 12:00:00 AM')    
    ```
4. Controleer de gegevens in de tabel: `watermarktable`.
    
    ```sql
    Select * from watermarktable
    ```
    Uitvoer: 

    ```
    TableName  | WatermarkValue
    ----------  | --------------
    data_source_table | 2010-01-01 00:00:00.000
    ```

### <a name="create-a-stored-procedure-in-azure-sql-database"></a>Een opgeslagen procedure maken in een Azure SQL-database 

Voer de volgende opdracht uit om een opgeslagen procedure te maken in de Azure SQL-database.

```sql
CREATE PROCEDURE sp_write_watermark @LastModifiedtime datetime, @TableName varchar(50)
AS

BEGIN
    
    UPDATE watermarktable
    SET [WatermarkValue] = @LastModifiedtime 
WHERE [TableName] = @TableName
    
END
```

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
    Set-AzureRmDataFactoryV2 -ResourceGroupName "<your resource group to create the factory>" -Location "East US" -Name "<specify the name of data factory to create. It must be globally unique.>" 
    ```

    Houd rekening met de volgende punten:

    * De naam van de Azure-gegevensfactory moet wereldwijd uniek zijn. Als de volgende fout zich voordoet, wijzigt u de naam en probeert u het opnieuw.

        ```
        The specified Data Factory name '<data factory name>' is already in use. Data Factory names must be globally unique.
        ```

    * Als u Data Factory-exemplaren wilt maken, moet u bijdrager of beheerder zijn van het Azure-abonnement.
    * Momenteel kunt u in Data Factory V2 alleen data factory's maken in de regio VS-Oost. De gegevensopslagexemplaren (Azure Storage, Azure SQL Database, enzovoort) en berekeningen (HDInsight, enzovoort) die worden gebruikt in Data Factory, kunnen zich in andere regio's bevinden.


## <a name="create-linked-services"></a>Gekoppelde services maken
U maakt gekoppelde services in een gegevensfactory om uw gegevensarchieven en compute-services aan de gegevensfactory te koppelen. In deze sectie maakt u gekoppelde services in het Azure Storage-account en de Azure SQL-database. 

### <a name="create-azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service maakt.
1. Maak een JSON-bestand met de naam **AzureStorageLinkedService.json** in de map **C:\ADF** met de volgende inhoud: (Maak de map ADF als deze nog niet bestaat.) Vervang voordat u het bestand opslaat `<accountName>`, `<accountKey>` door de naam en de sleutel van uw Azure Storage-account.

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
2. Schakel in **Azure PowerShell** over naar de map **ADF**.
3. Voer de cmdlet **Set-AzureRmDataFactoryV2LinkedService** uit om de gekoppelde service **AzureStorageLinkedService** te maken. In het volgende voorbeeld geeft u de waarden door voor de parameters **ResourceGroupName** en **DataFactoryName**. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureStorageLinkedService" -File ".\AzureStorageLinkedService.json"
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```json
    LinkedServiceName : AzureStorageLinkedService
    ResourceGroupName : <resourceGroupName>
    DataFactoryName   : <dataFactoryName>
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureStorageLinkedService
    ```

### <a name="create-azure-sql-database-linked-service"></a>Maak een gekoppelde Azure SQL Database-service.
1. Maak een JSON-bestand met de naam **AzureSQLDatabaseLinkedService.json** in de map **C:\ADF** met de volgende inhoud: (maak de map ADF als deze nog niet bestaat.) Vervang voordat u het bestand opslaat **&lt;server&gt; en &lt;user id&gt;, en &lt;password&gt;** door de naam van uw Azure SQL-server, gebruikers-id en wachtwoord. 

    ```json
    {
        "name": "AzureSQLDatabaseLinkedService",
        "properties": {
            "type": "AzureSqlDatabase",
            "typeProperties": {
                "connectionString": {
                    "value": "Server = tcp:<server>.database.windows.net,1433;Initial Catalog=<database name>; Persist Security Info=False; User ID=<user name> ; Password=<password>; MultipleActiveResultSets = False; Encrypt = True; TrustServerCertificate = False; Connection Timeout = 30;",
                    "type": "SecureString"
                }
            }
        }
    }
    ```
2. Schakel in **Azure PowerShell** over naar de map **ADF**.
3. Voer de cmdlet **Set-AzureRmDataFactoryV2LinkedService** uit om de gekoppelde service **AzureSQLDatabaseLinkedService** te maken. 

    ```powershell
    Set-AzureRmDataFactoryV2LinkedService -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "AzureSQLDatabaseLinkedService" -File ".\AzureSQLDatabaseLinkedService.json"
    ```

    Hier volgt een voorbeeld van uitvoer:

    ```json
    LinkedServiceName : AzureSQLDatabaseLinkedService
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlDatabaseLinkedService
    ProvisioningState :
    ```

## <a name="create-datasets"></a>Gegevenssets maken
In deze stap maakt u gegevenssets om de bron- en sinkgegevens te vertegenwoordigen. 

### <a name="create-a-source-dataset"></a>Een brongegevensset maken

1. Maak een JSON-bestand met de naam SourceDataset.json in dezelfde map met de volgende inhoud: 

    ```json
    {
        "name": "SourceDataset",
        "properties": {
            "type": "AzureSqlTable",
            "typeProperties": {
                "tableName": "data_source_table"
            },
            "linkedServiceName": {
                "referenceName": "AzureSQLDatabaseLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }
   
    ```
    In deze zelfstudie gebruiken we de tabelnaam: **data_source_table**. Vervang deze naam als u een tabel gebruikt met een andere naam. 
2.  Voer de cmdlet Set-AzureRmDataFactoryV2Dataset uit om de gegevensset te maken: SourceDataset
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SourceDataset" -File ".\SourceDataset.json"
    ```

    Hier volgt een uitvoervoorbeeld van de cmdlet:
    
    ```json
    DatasetName       : SourceDataset
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlTableDataset
    ```

### <a name="create-a-sink-dataset"></a>Een sinkgegevensset maken

1. Maak een JSON-bestand met de naam SinkDataset.json in dezelfde map met de volgende inhoud: 

    ```json
    {
        "name": "SinkDataset",
        "properties": {
            "type": "AzureBlob",
            "typeProperties": {
                "folderPath": "adftutorial/incrementalcopy",
                "fileName": "@CONCAT('Incremental-', pipeline().RunId, '.txt')", 
                "format": {
                    "type": "TextFormat"
                }
            },
            "linkedServiceName": {
                "referenceName": "AzureStorageLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }   
    ```

    > [!IMPORTANT]
    > In dit fragment wordt ervan uitgegaan dat u een blobcontainer hebt met de naam **adftutorial** in de Azure Blob-opslag. Maak de container als deze bestaat niet (of) stel deze in op de naam van een bestaande container. De uitvoermap `incrementalcopy` wordt automatisch gemaakt als deze niet bestaat in de container. In deze zelfstudie wordt de bestandsnaam dynamisch gegenereerd met behulp van de expressie: `@CONCAT('Incremental-', pipeline().RunId, '.txt')`.
2.  Voer de cmdlet Set-AzureRmDataFactoryV2Dataset uit om de gegevensset te maken: SinkDataset
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "SinkDataset" -File ".\SinkDataset.json"
    ```

    Hier volgt een uitvoervoorbeeld van de cmdlet:
    
    ```json
    DatasetName       : SinkDataset
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureBlobDataset    
    ```

## <a name="create-a-dataset-for-watermark"></a>Een gegevensset maken voor de grenswaarde
In deze stap maakt u een gegevensset voor het opslaan van een bovengrenswaarde. 

1. Maak een JSON-bestand met de naam WatermarkDataset.json in dezelfde map met de volgende inhoud: 

    ```json
    {
        "name": " WatermarkDataset ",
        "properties": {
            "type": "AzureSqlTable",
            "typeProperties": {
                "tableName": "watermarktable"
            },
            "linkedServiceName": {
                "referenceName": "AzureSQLDatabaseLinkedService",
                "type": "LinkedServiceReference"
            }
        }
    }    
    ```
2.  Voer de cmdlet Set-AzureRmDataFactoryV2Dataset uit om de gegevensset te maken: WatermarkDataset
    
    ```powershell
    Set-AzureRmDataFactoryV2Dataset -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "WatermarkDataset" -File ".\WatermarkDataset.json"
    ```

    Hier volgt een uitvoervoorbeeld van de cmdlet:
    
    ```json
    DatasetName       : WatermarkDataset
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Structure         :
    Properties        : Microsoft.Azure.Management.DataFactory.Models.AzureSqlTableDataset    
    ```

## <a name="create-a-pipeline"></a>Een pijplijn maken
In deze zelfstudie maakt u een pijplijn met twee opzoekactiviteiten, één kopieeractiviteit en één opgeslagen procedureactiviteit als keten in één pijplijn. 


1. Maak een JSON-bestand: IncrementalCopyPipeline.json in dezelfde map met de volgende inhoud. 

    ```json
    {
        "name": "IncrementalCopyPipeline",
        "properties": {
            "activities": [
                {
                    "name": "LookupOldWaterMarkActivity",
                    "type": "Lookup",
                    "typeProperties": {
                        "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from watermarktable"
                        },
    
                        "dataset": {
                        "referenceName": "WatermarkDataset",
                        "type": "DatasetReference"
                        }
                    }
                },
                {
                    "name": "LookupNewWaterMarkActivity",
                    "type": "Lookup",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "select MAX(LastModifytime) as NewWatermarkvalue from data_source_table"
                        },
    
                        "dataset": {
                        "referenceName": "SourceDataset",
                        "type": "DatasetReference"
                        }
                    }
                },
                
                {
                    "name": "IncrementalCopyActivity",
                    "type": "Copy",
                    "typeProperties": {
                        "source": {
                            "type": "SqlSource",
                            "sqlReaderQuery": "select * from data_source_table where LastModifytime > '@{activity('LookupOldWaterMarkActivity').output.firstRow.WatermarkValue}' and LastModifytime <= '@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}'"
                        },
                        "sink": {
                            "type": "BlobSink"
                        }
                    },
                    "dependsOn": [
                        {
                            "activity": "LookupNewWaterMarkActivity",
                            "dependencyConditions": [
                                "Succeeded"
                            ]
                        },
                        {
                            "activity": "LookupOldWaterMarkActivity",
                            "dependencyConditions": [
                                "Succeeded"
                            ]
                        }
                    ],
    
                    "inputs": [
                        {
                            "referenceName": "SourceDataset",
                            "type": "DatasetReference"
                        }
                    ],
                    "outputs": [
                        {
                            "referenceName": "SinkDataset",
                            "type": "DatasetReference"
                        }
                    ]
                },
    
                {
                    "name": "StoredProceduretoWriteWatermarkActivity",
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
    
                        "storedProcedureName": "sp_write_watermark",
                        "storedProcedureParameters": {
                            "LastModifiedtime": {"value": "@{activity('LookupNewWaterMarkActivity').output.firstRow.NewWatermarkvalue}", "type": "datetime" },
                            "TableName":  { "value":"@{activity('LookupOldWaterMarkActivity').output.firstRow.TableName}", "type":"String"}
                        }
                    },
    
                    "linkedServiceName": {
                        "referenceName": "AzureSQLDatabaseLinkedService",
                        "type": "LinkedServiceReference"
                    },
    
                    "dependsOn": [
                        {
                            "activity": "IncrementalCopyActivity",
                            "dependencyConditions": [
                                "Succeeded"
                            ]
                        }
                    ]
                }
            ],
            
        }
    }
    ```json

    If you are using a source table with a name different from the one used in the tutorial (**data_source_table**), replace **data_source_table** in the sqlReaderQuery with the name of your source table. 
    

2. Run the Set-AzureRmDataFactoryV2Pipeline cmdlet to create the pipeline: IncrementalCopyPipeline.
    
   ```powershell
   Set-AzureRmDataFactoryV2Pipeline -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -Name "IncrementalCopyPipeline" -File ".\IncrementalCopyPipeline.json"
   ``` 

   Hier volgt een voorbeeld van uitvoer: 

   ```json
    PipelineName      : IncrementalCopyPipeline
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    Activities        : {LookupOldWaterMarkActivity, LookupNewWaterMarkActivity, IncrementalCopyActivity, StoredProceduretoWriteWatermarkActivity}
    Parameters        :
   ```
 
## <a name="run-the-pipeline"></a>De pijplijn uitvoeren

1. Voer de pijplijn **IncrementalCopyPipeline** uit met behulp van de cmdlet **Invoke-AzureRmDataFactoryV2Pipeline**. Vervang tijdelijke aanduidingen door de namen van uw eigen resourcegroep en gegevensfactory.

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -PipelineName "IncrementalCopyPipeline" -ResourceGroup "<your resource group>" -dataFactoryName "<your data factory name>"
    ``` 
2. Controleer de status van de pijplijn door de cmdlet Get-AzureRmDataFactoryV2ActivityRun uit te voeren totdat alle activiteiten worden uitgevoerd. Vervang tijdelijke aanduidingen door uw eigen juiste tijd voor de parameters RunStartedAfter en RunStartedBefore.  In deze zelfstudie gebruiken we -RunStartedAfter 2017/09/14 -RunStartedBefore 2017/09/15

    ```powershell
    Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $RunId -RunStartedAfter "<start time>" -RunStartedBefore "<end time>"
    ```

    Hier volgt een voorbeeld van uitvoer:
 
    ```json
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupNewWaterMarkActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {NewWatermarkvalue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:42:42 AM
    ActivityRunEnd    : 9/14/2017 7:42:50 AM
    DurationInMs      : 7777
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupOldWaterMarkActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {TableName, WatermarkValue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:42:42 AM
    ActivityRunEnd    : 9/14/2017 7:43:07 AM
    DurationInMs      : 25437
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : IncrementalCopyActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, rowsCopied, copyDuration...}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:43:10 AM
    ActivityRunEnd    : 9/14/2017 7:43:29 AM
    DurationInMs      : 19769
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : StoredProceduretoWriteWatermarkActivity
    PipelineRunId     : d4bf3ce2-5d60-43f3-9318-923155f61037
    PipelineName      : IncrementalCopyPipeline
    Input             : {storedProcedureName, storedProcedureParameters}
    Output            : {}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 7:43:32 AM
    ActivityRunEnd    : 9/14/2017 7:43:47 AM
    DurationInMs      : 14467
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}

    ```

## <a name="review-the-results"></a>De resultaten bekijken

1. In de Azure Blob-opslag (sinkopslag) ziet u dat de gegevens zijn gekopieerd naar het bestand dat is gedefinieerd in de SinkDataset.  In de huidige zelfstudie is de bestandsnaam `Incremental- d4bf3ce2-5d60-43f3-9318-923155f61037.txt`.  Open het bestand. U ziet records in het bestand die hetzelfde zijn als de gegevens in de Azure SQL-database.

    ```
    1,aaaa,2017-09-01 00:56:00.0000000
    2,bbbb,2017-09-02 05:23:00.0000000
    3,cccc,2017-09-03 02:36:00.0000000
    4,dddd,2017-09-04 03:21:00.0000000
    5,eeee,2017-09-05 08:06:00.0000000
    ``` 
2. Ga naar de laatste waarde in `watermarktable`. U ziet nu dat de grenswaarde is bijgewerkt.

    ```sql
    Select * from watermarktable
    ```
    
    Hier volgt een voorbeeld van uitvoer:
 
    TableName | WatermarkValue
    --------- | --------------
    data_source_table   2017-09-05  8:06:00.000

### <a name="insert-data-into-data-source-store-to-verify-delta-data-loading"></a>Gegevens invoegen in de gegevensbronopslag om te controleren of de deltagegevens worden geladen

1. Nieuwe gegevens invoegen in een Azure SQL-database (brongegevensopslag):

    ```sql
    INSERT INTO data_source_table
    VALUES (6, 'newdata','9/6/2017 2:23:00 AM')
    
    INSERT INTO data_source_table
    VALUES (7, 'newdata','9/7/2017 9:01:00 AM')
    ``` 

    De bijgewerkte gegevens in de Azure SQL-database zijn als volgt:

    ```
    PersonID | Name | LastModifytime
    -------- | ---- | --------------
    1 | aaaa | 2017-09-01 00:56:00.000
    2 | bbbb | 2017-09-02 05:23:00.000
    3 | cccc | 2017-09-03 02:36:00.000
    4 | dddd | 2017-09-04 03:21:00.000
    5 | eeee | 2017-09-05 08:06:00.000
    6 | newdata | 2017-09-06 02:23:00.000
    7 | newdata | 2017-09-07 09:01:00.000
    ```
2. Voer de pijplijn **IncrementalCopyPipeline** opnieuw uit met behulp van de cmdlet **Invoke-AzureRmDataFactoryV2Pipeline**. Vervang tijdelijke aanduidingen door de namen van uw eigen resourcegroep en gegevensfactory.

    ```powershell
    $RunId = Invoke-AzureRmDataFactoryV2Pipeline -PipelineName "IncrementalCopyPipeline" -ResourceGroup "<your resource group>" -dataFactoryName "<your data factory name>"
    ```
3. Controleer de status van de pijplijn door de cmdlet **Get-AzureRmDataFactoryV2ActivityRun** uit te voeren totdat alle activiteiten worden uitgevoerd. Vervang tijdelijke aanduidingen door uw eigen juiste tijd voor de parameters RunStartedAfter en RunStartedBefore.  In deze zelfstudie gebruiken we -RunStartedAfter 2017/09/14 -RunStartedBefore 2017/09/15

    ```powershell
    Get-AzureRmDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $RunId -RunStartedAfter "<start time>" -RunStartedBefore "<end time>"
    ```

    Hier volgt een voorbeeld van uitvoer:
 
    ```json
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupNewWaterMarkActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {NewWatermarkvalue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:52:26 AM
    ActivityRunEnd    : 9/14/2017 8:52:58 AM
    DurationInMs      : 31758
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : LookupOldWaterMarkActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, dataset}
    Output            : {TableName, WatermarkValue}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:52:26 AM
    ActivityRunEnd    : 9/14/2017 8:52:52 AM
    DurationInMs      : 25497
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : IncrementalCopyActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {source, sink}
    Output            : {dataRead, dataWritten, rowsCopied, copyDuration...}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:53:00 AM
    ActivityRunEnd    : 9/14/2017 8:53:20 AM
    DurationInMs      : 20194
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}
    
    ResourceGroupName : ADF
    DataFactoryName   : incrementalloadingADF
    ActivityName      : StoredProceduretoWriteWatermarkActivity
    PipelineRunId     : 2fc90ab8-d42c-4583-aa64-755dba9925d7
    PipelineName      : IncrementalCopyPipeline
    Input             : {storedProcedureName, storedProcedureParameters}
    Output            : {}
    LinkedServiceName :
    ActivityRunStart  : 9/14/2017 8:53:23 AM
    ActivityRunEnd    : 9/14/2017 8:53:41 AM
    DurationInMs      : 18502
    Status            : Succeeded
    Error             : {errorCode, message, failureType, target}

    ```
4.  In de Azure Blob-opslag ziet u dat een ander bestand is gemaakt in Azure Blob-opslag. In deze zelfstudie is de nieuwe bestandsnaam `Incremental-2fc90ab8-d42c-4583-aa64-755dba9925d7.txt`.  Als u dit bestand opent, ziet u 2 rijen met records:
5.  Ga naar de laatste waarde in `watermarktable`. U ziet nu dat de grenswaarde opnieuw is bijgewerkt

    ```sql
    Select * from watermarktable
    ```
    voorbeelduitvoer: 
    
    TableName | WatermarkValue
    --------- | ---------------
    data_source_table | 2017-09-07 09:01:00.000

     
## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u de volgende stappen uitgevoerd: 

> [!div class="checklist"]
> * Definieer een **grenswaardekolom** en sla deze op in Azure SQL Database.  
> * Maak een gegevensfactory.
> * Maak gekoppelde services voor SQL Database en Blob-opslag. 
> * Maak bron- en sinkgegevenssets.
> * Maak een pijplijn.
> * Voer de pijplijn uit.
> * Controleer de pijplijnuitvoering. 

Ga naar de volgende zelfstudie voor meer informatie over het transformeren van gegevens met behulp van een Spark-cluster in Azure:

> [!div class="nextstepaction"]
>[Gegevens transformeren met behulp van een Spark-cluster in de cloud](tutorial-transform-data-spark-powershell.md)



