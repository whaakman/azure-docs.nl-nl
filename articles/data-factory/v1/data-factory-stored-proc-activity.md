---
title: SQL Server opgeslagen Procedure-activiteit
description: Meer informatie over hoe u de SQL Server opgeslagen Procedure activiteit kunt gebruiken om aan te roepen, een opgeslagen procedure in een Azure SQL Database of een Azure SQL Data Warehouse van een Data Factory-pijplijn.
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: be0bdf771327e57a75a4f95b513f9e80aeaef5a4
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="sql-server-stored-procedure-activity"></a>SQL Server opgeslagen Procedure-activiteit
> [!div class="op_single_selector" title1="Transformation Activities"]
> * [Hive-activiteit](data-factory-hive-activity.md) 
> * [Pig-activiteit](data-factory-pig-activity.md)
> * [MapReduce-activiteit](data-factory-map-reduce.md)
> * [Hadoop-Streamingactiviteit](data-factory-hadoop-streaming-activity.md)
> * [Spark-activiteit](data-factory-spark.md)
> * [Machine Learning-batchuitvoeringsactiviteit](data-factory-azure-ml-batch-execution-activity.md)
> * [Machine Learning-activiteit resources bijwerken](data-factory-azure-ml-update-resource-activity.md)
> * [Opgeslagen procedureactiviteit](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL-activiteit](data-factory-usql-activity.md)
> * [Aangepaste activiteit .NET](data-factory-use-custom-activities.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Azure Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service, die zich in de preview, Zie [transformatie van gegevens met behulp van de activiteit opgeslagen procedure in Gegevensfactory versie 2](../transform-data-using-stored-procedure.md).

## <a name="overview"></a>Overzicht
U activiteiten voor gegevenstransformatie gebruiken in een Data Factory [pijplijn](data-factory-create-pipelines.md) transformeren en verwerken van onbewerkte gegevens in de voorspellingen en inzichten. De activiteit opgeslagen Procedure is een van de activiteiten voor gegevenstransformatie die ondersteuning biedt voor Data Factory. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) artikel, hetgeen een algemeen overzicht van gegevenstransformatie en de ondersteunde transformatieactiviteiten in de Data Factory toont.

U kunt de activiteit opgeslagen Procedure gebruiken om aan te roepen een opgeslagen procedure in een van de volgende gegevens worden opgeslagen in uw onderneming of op Azure een virtuele machine (VM): 

- Azure SQL Database
- Azure SQL Data Warehouse
- SQL Server-Database.  Als u van SQL Server gebruikmaakt, Data Management Gateway installeren op dezelfde computer die als host fungeert voor de database of op een afzonderlijke computer die toegang heeft tot de database. Data Management Gateway is een onderdeel dat wordt verbinding gemaakt op-premises/op virtuele machine van Azure met cloudservices gegevensbronnen op een manier veilig en beheerd. Zie [Data Management Gateway](data-factory-data-management-gateway.md) artikel voor meer informatie.

> [!IMPORTANT]
> Wanneer gegevens worden gekopieerd naar Azure SQL Database of SQL Server, kunt u de **SqlSink** in een kopieeractiviteit om aan te roepen een opgeslagen procedure met behulp van de **sqlWriterStoredProcedureName** eigenschap. Zie voor meer informatie [aanroepen van opgeslagen procedure uit kopieeractiviteit](data-factory-invoke-stored-procedure-from-copy-activity.md). Zie voor meer informatie over de eigenschap connector artikelen te volgen: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties). Een opgeslagen procedure aanroepen tijdens het kopiëren van gegevens in een Azure SQL Data Warehouse met behulp van een kopieeractiviteit wordt niet ondersteund. Maar u kunt de activiteit opgeslagen procedure gebruiken om aan te roepen, een opgeslagen procedure in een SQL Data Warehouse. 
>  
> Wanneer het kopiëren van gegevens van Azure SQL Database of SQL Server of Azure SQL Data Warehouse, kunt u configureren **SqlSource** in een kopieeractiviteit om aan te roepen, een opgeslagen procedure voor het lezen van gegevens uit de brondatabase met behulp van de  **sqlReaderStoredProcedureName** eigenschap. Zie voor meer informatie de volgende artikelen voor de connector: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)          


De volgende procedure maakt gebruik van de activiteit opgeslagen Procedure in een pijplijn om aan te roepen, een opgeslagen procedure in een Azure SQL database. 

## <a name="walkthrough"></a>Walkthrough
### <a name="sample-table-and-stored-procedure"></a>Voorbeeldtabel en opgeslagen procedure
1. Maak de volgende **tabel** in uw Azure SQL Database met behulp van SQL Server Management Studio of een ander hulpmiddel u vertrouwd met bent. De datum-kolom is de datum en tijd wanneer de bijbehorende ID wordt gegenereerd.

    ```SQL
    CREATE TABLE dbo.sampletable
    (
        Id uniqueidentifier,
        datetimestamp nvarchar(127)
    )
    GO

    CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable(Id);
    GO
    ```
    ID is uniek geïdentificeerd en de datum-kolom is de datum en tijd wanneer de bijbehorende ID wordt gegenereerd.
    
    ![Voorbeeldgegevens](./media/data-factory-stored-proc-activity/sample-data.png)

    In dit voorbeeld wordt de opgeslagen procedure is in een Azure SQL Database. Als de opgeslagen procedure in een Azure SQL Data Warehouse en SQL Server-Database is, is de methode is vergelijkbaar. Voor een SQL Server-database, moet u een [Data Management Gateway](data-factory-data-management-gateway.md).
2. Maak de volgende **opgeslagen procedure** die voegt de gegevens in voor de **sampletable**.

    ```SQL
    CREATE PROCEDURE sp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > **Naam** en **hoofdlettergebruik** van de parameter (DateTime in dit voorbeeld) moet overeenkomen met die van de parameter die is opgegeven in de pipeline/JSON van de activiteit. Zorg ervoor dat in de definitie van de opgeslagen procedure  **@**  wordt gebruikt als een voorvoegsel voor de parameter.

### <a name="create-a-data-factory"></a>Een gegevensfactory maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **nieuw** Klik op het menu links op **Intelligence en analyse**, en klik op **Data Factory**.

    ![Nieuwe gegevensfactory](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. In de **nieuwe gegevensfactory** blade Voer **SProcDF** voor de naam. Azure Data Factory-namen zijn **globaal unieke**. U moet het voorvoegsel van de naam van de gegevensfactory met de naam van uw de geslaagde maken van de fabriek inschakelen.

   ![Nieuwe gegevensfactory](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. Selecteer uw **Azure-abonnement**.
5. Voor **resourcegroep**, voer een van de volgende stappen uit:
   1. Klik op **nieuw** en voer een naam voor de resourcegroep.
   2. Klik op **gebruik bestaande** en selecteer een bestaande resourcegroep.  
6. Selecteer de **locatie** voor de gegevensfactory.
7. Selecteer **vastmaken aan dashboard** zodat u de gegevensfactory op het dashboard volgende keer dat u zich ziet aanmeldt.
8. Klik op de blade **Nieuwe gegevensfactory** op **Maken**.
9. U ziet de gegevensfactory wordt gemaakt in de **dashboard** van de Azure portal. Nadat de gegevensfactory is gemaakt, ziet u de gegevensfactorypagina. Hierop wordt de inhoud van de gegevensfactory weergegeven.

   ![Data Factory-startpagina](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Een Azure SQL gekoppelde service maken
Nadat de gegevensfactory is gemaakt, maakt u een Azure SQL gekoppelde service die is gekoppeld aan uw Azure SQL database met de tabel sampletable en sp_sample opgeslagen procedure aan uw gegevensfactory.

1. Klik op **auteur en implementeren van** op de **Data Factory** blade voor **SProcDF** starten van de Data Factory-Editor.
2. Klik op **nieuwe gegevensopslag** op de opdracht en kies **Azure SQL Database**. U ziet het JSON-script voor het maken van een Azure SQL gekoppelde service in de editor.

   ![Nieuwe gegevensopslag](media/data-factory-stored-proc-activity/new-data-store.png)
3. Breng de volgende wijzigingen in de JSON-script:

   1. Vervang `<servername>` met de naam van uw Azure SQL Database-server.
   2. Vervang `<databasename>` met de database waarin u de tabel en de opgeslagen procedure hebt gemaakt.
   3. Vervang `<username@servername>` met het gebruikersaccount dat toegang tot de database heeft.
   4. Vervang `<password>` met het wachtwoord voor het gebruikersaccount.

      ![Nieuwe gegevensopslag](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. Voor het implementeren van de gekoppelde service, klikt u op **implementeren** op de opdrachtbalk. Controleer of u de AzureSqlLinkedService in de structuurweergave links.

    ![structuurweergave met gekoppelde service](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Een uitvoergegevensset maken
U kunt een uitvoergegevensset voor een activiteit opgeslagen procedure moet opgeven, zelfs als de opgeslagen procedure geen gegevens produceert. Dat is omdat het de uitvoergegevensset die schijven van de planning van de activiteit (hoe vaak de activiteit wordt uitgevoerd - per uur, dagelijks, enzovoort). De uitvoergegevensset moet gebruiken een **gekoppelde service** die verwijst naar een Azure SQL Database of een Azure SQL Data Warehouse of een SQL Server-Database die u wilt de opgeslagen procedure om uit te voeren. De uitvoergegevensset kan fungeren als een manier om door te geven het resultaat van de opgeslagen procedure voor verdere verwerking door een andere activiteit ([activiteiten chaining](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in de pijplijn. Echter, Data Factory automatisch schrijft niet de uitvoer van een opgeslagen procedure bij deze dataset. De opgeslagen procedure die naar een SQL-tabel die de uitvoergegevensset die naar verwijst schrijft is. In sommige gevallen kan de uitvoergegevensset mag een **dummy gegevensset** (een gegevensset die verwijst naar een tabel die de uitvoer van de opgeslagen procedure niet echt bijhoudt). Dit dummy-gegevensset wordt alleen gebruikt om de planning voor het uitvoeren van de activiteit opgeslagen procedure opgeven. 

1. Klik op **... Meer** op de werkbalk klikt **nieuwe gegevensset**, en klik op **Azure SQL**. **Nieuwe gegevensset** op de opdrachtbalk en selecteer **Azure SQL**.

    ![structuurweergave met gekoppelde service](media/data-factory-stored-proc-activity/new-dataset.png)
2. De volgende JSON-script in in de JSON-editor kopiëren en plakken.

    ```JSON
    {                
        "name": "sprocsampleout",
        "properties": {
            "type": "AzureSqlTable",
            "linkedServiceName": "AzureSqlLinkedService",
            "typeProperties": {
                "tableName": "sampletable"
            },
            "availability": {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }
    ```
3. Voor het implementeren van de gegevensset, klikt u op **implementeren** op de opdrachtbalk. Controleer of u de gegevensset in de structuurweergave.

    ![Structuurweergave met gekoppelde services](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Een pijplijn maken met SqlServerStoredProcedure activiteit
Nu gaan we een pijplijn maken met een activiteit opgeslagen procedure. 

U ziet de volgende eigenschappen: 

- De **type** eigenschap is ingesteld op **SqlServerStoredProcedure**. 
- De **storedProcedureName** in het type eigenschappen is ingesteld op **sp_sample** (naam van de opgeslagen procedure).
- De **storedProcedureParameters** sectie bevat een parameter met de naam **datum/tijd**. Naam en het hoofdlettergebruik van de parameter in JSON moeten overeenkomen met de naam en het hoofdlettergebruik van de parameter in de definitie van de opgeslagen procedure. Als u null zijn voor een parameter doorgeven moet, gebruikt u de syntaxis: `"param1": null` (alle kleine letters).
 
1. Klik op **... Meer** op de opdrachtbalk klikken en op **nieuwe pijplijn**.
2. Kopiëren en plakken in het volgende JSON-fragment:   

    ```JSON
    {
        "name": "SprocActivitySamplePipeline",
        "properties": {
            "activities": [
                {
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
                        "storedProcedureName": "sp_sample",
                        "storedProcedureParameters": {
                            "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)"
                        }
                    },
                    "outputs": [
                        {
                            "name": "sprocsampleout"
                        }
                    ],
                    "scheduler": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "name": "SprocActivitySample"
                }
            ],
             "start": "2017-04-02T00:00:00Z",
             "end": "2017-04-02T05:00:00Z",
            "isPaused": false
        }
    }
    ```
3. Voor het implementeren van de pijplijn, klikt u op **implementeren** op de werkbalk.  

### <a name="monitor-the-pipeline"></a>De pijplijn bewaken
1. Klik op **X** om de blades van de Data Factory-editor te sluiten en om terug te keren naar de Data Factory-blade. Klik op **Diagram**.

    ![Tegel diagram](media/data-factory-stored-proc-activity/data-factory-diagram-tile.png)
2. In de **diagramweergave** ziet u een overzicht van de pijplijnen en gegevenssets die voor deze zelfstudie worden gebruikt.

    ![Tegel diagram](media/data-factory-stored-proc-activity/data-factory-diagram-view.png)
3. Dubbelklik in de diagramweergave op de gegevensset `sprocsampleout`. U ziet de segmenten in de status Ready heeft. Moet er vijf segmenten omdat een segment wordt geproduceerd voor elk uur tussen de begintijd en eindtijd van de JSON.

    ![Tegel diagram](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. Wanneer een segment is in **gereed** staat, voer een `select * from sampletable` -query op de Azure SQL database om te controleren dat de gegevens is ingevoegd in de tabel door de opgeslagen procedure.

   ![uitvoergegevens](./media/data-factory-stored-proc-activity/output.png)

   Zie [bewaken van de pijplijn](data-factory-monitor-manage-pipelines.md) voor gedetailleerde informatie over het Azure Data Factory-pijplijnen bewaken.  


## <a name="specify-an-input-dataset"></a>Geef een invoergegevensset
In dit overzicht heeft activiteit opgeslagen procedure geen eventuele invoergegevenssets. Als u een invoergegevensset opgeeft, wordt de activiteit opgeslagen procedure niet uitgevoerd totdat het segment met invoergegevensset beschikbaar (in de status Ready heeft is). De gegevensset mag een externe gegevensset (die wordt niet gemaakt door een andere activiteit in de dezelfde pijplijn) of een interne gegevensset die wordt geproduceerd door een upstream-activiteit (de activiteit die wordt uitgevoerd voordat deze activiteit). U kunt meerdere invoergegevenssets voor de activiteit opgeslagen procedure opgeven. Als u doet dit, de activiteit opgeslagen procedure wordt alleen uitgevoerd als alle segmenten van de invoergegevensset beschikbaar (in de status Ready heeft zijn). Invoergegevensset kan niet worden gebruikt in de opgeslagen procedure als een parameter. Het wordt alleen gebruikt om te controleren van de afhankelijkheid voordat u de activiteit opgeslagen procedure start.

## <a name="chaining-with-other-activities"></a>Keten met andere activiteiten
Als u koppelen aan deze activiteit een upstream-activiteit wilt, geeft u de uitvoer van de upstream-activiteit als invoer van deze activiteit. Wanneer u doet dit, wordt de activiteit opgeslagen procedure niet uitvoeren totdat de upstream-activiteit is voltooid en de uitvoergegevensset van de upstream-activiteit (in de status gereed) dat beschikbaar is. U kunt uitvoergegevenssets uit meerdere activiteiten van de upstream opgeven als invoer gegevenssets van de activiteit opgeslagen procedure. Wanneer u doet dit, de activiteit opgeslagen procedure wordt alleen uitgevoerd als alle segmenten van de invoergegevensset beschikbaar zijn.  

In het volgende voorbeeld wordt de uitvoer van de kopieerbewerking is: OutputDataset die invoer van de activiteit opgeslagen procedure. Daarom wordt de activiteit opgeslagen procedure niet uitvoeren totdat de kopieerbewerking is voltooid en het segment OutputDataset beschikbaar (in de status Ready heeft is). Als u meerdere invoergegevenssets opgeeft, wordt de activiteit opgeslagen procedure niet uitvoeren totdat alle segmenten van de invoergegevensset (in de status gereed) beschikbaar zijn. De invoer gegevenssets kan niet rechtstreeks worden gebruikt als parameters voor de activiteit opgeslagen procedure. 

Zie voor meer informatie over het koppelen van activiteiten [meerdere activiteiten in een pijplijn](data-factory-create-pipelines.md#multiple-activities-in-a-pipeline)

```json
{

    "name": "ADFTutorialPipeline",
    "properties": {
        "description": "Copy data from a blob to blob",
        "activities": [
            {
                "type": "Copy",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "sink": {
                        "type": "BlobSink",
                        "writeBatchSize": 0,
                        "writeBatchTimeout": "00:00:00"
                    }
                },
                "inputs": [ { "name": "InputDataset" } ],
                "outputs": [ { "name": "OutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst"
                },
                "name": "CopyFromBlobToSQL"
            },
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "SPSproc"
                },
                "inputs": [ { "name": "OutputDataset" } ],
                "outputs": [ { "name": "SQLOutputDataset" } ],
                "policy": {
                    "timeout": "01:00:00",
                    "concurrency": 1,
                    "retry": 3
                },
                "name": "RunStoredProcedure"
            }

        ],
        "start": "2017-04-12T00:00:00Z",
        "end": "2017-04-13T00:00:00Z",
        "isPaused": false,
    }
}
```

Op deze manier om te koppelen van de activiteit van de procedure store met **downstream-activiteiten** (de activiteiten die worden uitgevoerd nadat de activiteit opgeslagen procedure is voltooid), de uitvoergegevensset van de activiteit opgeslagen procedure opgeven als invoer van de downstream activiteit in de pijplijn.

> [!IMPORTANT]
> Wanneer gegevens worden gekopieerd naar Azure SQL Database of SQL Server, kunt u de **SqlSink** in een kopieeractiviteit om aan te roepen een opgeslagen procedure met behulp van de **sqlWriterStoredProcedureName** eigenschap. Zie voor meer informatie [aanroepen van opgeslagen procedure uit kopieeractiviteit](data-factory-invoke-stored-procedure-from-copy-activity.md). Zie voor meer informatie over de eigenschap, de volgende artikelen voor de connector: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).
>  
> Wanneer het kopiëren van gegevens van Azure SQL Database of SQL Server of Azure SQL Data Warehouse, kunt u configureren **SqlSource** in een kopieeractiviteit om aan te roepen, een opgeslagen procedure voor het lezen van gegevens uit de brondatabase met behulp van de  **sqlReaderStoredProcedureName** eigenschap. Zie voor meer informatie de volgende artikelen voor de connector: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)          

## <a name="json-format"></a>JSON-indeling
Hier volgt de JSON-indeling voor het definiëren van een activiteit opgeslagen Procedure:

```JSON
{
    "name": "SQLSPROCActivity",
    "description": "description",
    "type": "SqlServerStoredProcedure",
    "inputs":  [ { "name": "inputtable"  } ],
    "outputs":  [ { "name": "outputtable" } ],
    "typeProperties":
    {
        "storedProcedureName": "<name of the stored procedure>",
        "storedProcedureParameters":  
        {
            "param1": "param1Value"
            …
        }
    }
}
```

De volgende tabel beschrijft deze JSON-eigenschappen:

| Eigenschap | Beschrijving | Vereist |
| --- | --- | --- |
| naam | Naam van de activiteit |Ja |
| description |Beschrijving van wat de activiteit wordt gebruikt |Nee |
| type | Moet worden ingesteld op: **SqlServerStoredProcedure** | Ja |
| Invoer | Optioneel. Als u een invoergegevensset opgeeft, moet dit beschikbaar is (in de status 'Gereed') voor de activiteit opgeslagen procedure moet worden uitgevoerd. Invoergegevensset kan niet worden gebruikt in de opgeslagen procedure als een parameter. Het wordt alleen gebruikt om te controleren van de afhankelijkheid voordat u de activiteit opgeslagen procedure start. |Nee |
| uitvoer | U moet een uitvoergegevensset voor een activiteit opgeslagen procedure opgeven. Uitvoergegevensset geeft de **planning** voor de activiteit opgeslagen procedure (elk uur, wekelijks, maandelijks, enzovoort). <br/><br/>De uitvoergegevensset moet gebruiken een **gekoppelde service** die verwijst naar een Azure SQL Database of een Azure SQL Data Warehouse of een SQL Server-Database die u wilt de opgeslagen procedure om uit te voeren. <br/><br/>De uitvoergegevensset kan fungeren als een manier om door te geven het resultaat van de opgeslagen procedure voor verdere verwerking door een andere activiteit ([activiteiten chaining](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in de pijplijn. Echter, Data Factory automatisch schrijft niet de uitvoer van een opgeslagen procedure bij deze dataset. De opgeslagen procedure die naar een SQL-tabel die de uitvoergegevensset die naar verwijst schrijft is. <br/><br/>In sommige gevallen kan de uitvoergegevensset mag een **dummy gegevensset**, die wordt alleen gebruikt om de planning voor het uitvoeren van de activiteit opgeslagen procedure opgeven. |Ja |
| storedProcedureName |Geef de naam van de opgeslagen procedure in de Azure SQL database of Azure SQL Data Warehouse of SQL Server-database die wordt vertegenwoordigd door de gekoppelde service die gebruikmaakt van de uitvoertabel. |Ja |
| storedProcedureParameters |Geef waarden op voor parameters van opgeslagen procedure. Als u null zijn voor een parameter doorgeven moet, gebruikt u de syntaxis: "param1": null (alle kleine letter). Zie het volgende voorbeeld voor meer informatie over het gebruik van deze eigenschap. |Nee |

## <a name="passing-a-static-value"></a>Het doorgeven van een statische waarde
Nu we eens het toevoegen van een andere kolom met de naam 'Scenario' in de tabel met een statische waarde 'Document voorbeeld' genoemd.

![Voorbeeldgegevens 2](./media/data-factory-stored-proc-activity/sample-data-2.png)

**Tabel:**

```SQL
CREATE TABLE dbo.sampletable2
(
    Id uniqueidentifier,
    datetimestamp nvarchar(127),
    scenario nvarchar(127)
)
GO

CREATE CLUSTERED INDEX ClusteredID ON dbo.sampletable2(Id);
```

**Opgeslagen procedure:**

```SQL
CREATE PROCEDURE sp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

Nu, geven de **Scenario** parameter en de waarde van de activiteit opgeslagen procedure. De **typeProperties** sectie in het voorgaande voorbeeld lijkt op het volgende fragment:

```JSON
"typeProperties":
{
    "storedProcedureName": "sp_sample",
    "storedProcedureParameters":
    {
        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
        "Scenario": "Document sample"
    }
}
```

**Data Factory-gegevensset:**

```JSON
{
    "name": "sprocsampleout2",
    "properties": {
        "published": false,
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "sampletable2"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

**Data Factory-pijplijn**

```JSON
{
    "name": "SprocActivitySamplePipeline2",
    "properties": {
        "activities": [
            {
                "type": "SqlServerStoredProcedure",
                "typeProperties": {
                    "storedProcedureName": "sp_sample2",
                    "storedProcedureParameters": {
                        "DateTime": "$$Text.Format('{0:yyyy-MM-dd HH:mm:ss}', SliceStart)",
                        "Scenario": "Document sample"
                    }
                },
                "outputs": [
                    {
                        "name": "sprocsampleout2"
                    }
                ],
                "scheduler": {
                    "frequency": "Hour",
                    "interval": 1
                },
                "name": "SprocActivitySample"
            }
        ],
        "start": "2016-10-02T00:00:00Z",
        "end": "2016-10-02T05:00:00Z"
    }
}
```