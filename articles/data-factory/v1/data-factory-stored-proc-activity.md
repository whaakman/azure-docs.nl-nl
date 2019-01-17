---
title: SQL Server opgeslagen Procedureactiviteit
description: Lees hoe u de SQL Server opgeslagen Procedure-activiteit kunt gebruiken om aan te roepen een opgeslagen procedure in een Azure SQL Database of Azure SQL Data Warehouse van een Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: douglasl
robots: noindex
ms.openlocfilehash: 3f13cb2626394d16a127b172bb69c4ab88121cdb
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54352526"
---
# <a name="sql-server-stored-procedure-activity"></a>SQL Server opgeslagen Procedureactiviteit
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
> * [Aangepaste .NET-activiteit](data-factory-use-custom-activities.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Azure Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u [gegevens transformeren met behulp van opgeslagen procedure-activiteit in Data Factory](../transform-data-using-stored-procedure.md).

## <a name="overview"></a>Overzicht
Gebruik van activiteiten voor gegevenstransformatie in een Data Factory [pijplijn](data-factory-create-pipelines.md) transformeren en verwerken van onbewerkte gegevens in voorspellingen en inzichten. De activiteit opgeslagen Procedure is een van de activiteiten voor gegevenstransformatie die ondersteuning biedt voor Data Factory. In dit artikel is gebaseerd op de [activiteiten voor gegevenstransformatie](data-factory-data-transformation-activities.md) artikel een algemeen overzicht van de gegevenstransformatie van en de ondersteunde transformatieactiviteiten in Data Factory geeft.

U kunt de activiteit opgeslagen Procedure gebruiken om aan te roepen een opgeslagen procedure in een van de volgende gegevensarchieven in uw onderneming of op een Azure-machine (VM): 

- Azure SQL Database
- Azure SQL Data Warehouse
- SQL Server Database.  Als u SQL Server gebruikt, installeert u Data Management Gateway op dezelfde computer die als host fungeert voor de database of op een afzonderlijke computer die toegang tot de database heeft. Data Management Gateway is een onderdeel waarmee gegevens verbonden gegevensbronnen on-premises/op virtuele Azure-machine met cloudservices op een veilige, beheerde manier. Zie [Data Management Gateway](data-factory-data-management-gateway.md) artikel voor meer informatie.

> [!IMPORTANT]
> Het kopiëren van gegevens in Azure SQL Database of SQL Server, kunt u de **SqlSink** in de kopieeractiviteit om aan te roepen een opgeslagen procedure met behulp van de **sqlWriterStoredProcedureName** eigenschap. Zie voor meer informatie, [aanroepen van de opgeslagen procedure van kopieeractiviteit](data-factory-invoke-stored-procedure-from-copy-activity.md). Voor meer informatie over de eigenschap, naar het volgende connector artikelen: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties). Aanroepen van een opgeslagen procedure tijdens het kopiëren van gegevens in een Azure SQL Data Warehouse met behulp van een kopieeractiviteit wordt niet ondersteund. Maar u kunt de opgeslagen procedure-activiteit gebruiken voor het aanroepen van een opgeslagen procedure in een SQL Data Warehouse. 
>  
> Het kopiëren van gegevens uit Azure SQL Database of SQL Server of Azure SQL Data Warehouse, kunt u configureren **SqlSource** in de kopieeractiviteit om aan te roepen een opgeslagen procedure voor het lezen van gegevens vanuit de brondatabase met behulp van de  **sqlReaderStoredProcedureName** eigenschap. Zie voor meer informatie de volgende artikelen voor de connector: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)          


De volgende procedure maakt gebruik van de activiteit opgeslagen Procedure in een pijplijn aanroepen van een opgeslagen procedure in een Azure SQL database. 

## <a name="walkthrough"></a>Walkthrough
### <a name="sample-table-and-stored-procedure"></a>Voorbeeldtabel en opgeslagen procedure
1. Maak de volgende **tabel** in uw Azure SQL Database met behulp van SQL Server Management Studio of een ander hulpmiddel dat u vertrouwd met bent. De kolom datum is de datum en tijd wanneer de bijbehorende ID wordt gegenereerd.

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
    -ID is uniek geïdentificeerd en de kolom datum is de datum en tijd wanneer de bijbehorende ID wordt gegenereerd.
    
    ![Voorbeeldgegevens](./media/data-factory-stored-proc-activity/sample-data.png)

    In dit voorbeeld wordt de opgeslagen procedure is in een Azure SQL Database. Als de opgeslagen procedure in een Azure SQL Data Warehouse en de SQL Server-Database is, is de methode is vergelijkbaar. Voor een SQL Server-database, moet u een [Data Management Gateway](data-factory-data-management-gateway.md).
2. Maak de volgende **opgeslagen procedure** die voegt de gegevens in op de **sampletable**.

    ```SQL
    CREATE PROCEDURE usp_sample @DateTime nvarchar(127)
    AS

    BEGIN
        INSERT INTO [sampletable]
        VALUES (newid(), @DateTime)
    END
    ```

   > [!IMPORTANT]
   > **Naam** en **hoofdlettergebruik** van de parameter (datum/tijd in dit voorbeeld) moet overeenkomen met die van de parameter is opgegeven in de pijplijn/JSON van de activiteit. Zorg ervoor dat in de definitie van de opgeslagen procedure **@** wordt gebruikt als een voorvoegsel voor de parameter.

### <a name="create-a-data-factory"></a>Een gegevensfactory maken
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **nieuw** in het menu links, klikt u op **Intelligence en analyse**, en klikt u op **Data Factory**.

    ![Nieuwe data factory](media/data-factory-stored-proc-activity/new-data-factory.png)    
3. In de **nieuwe data factory** blade Voer **SProcDF** voor de naam. Azure Data Factory-namen zijn **unieke**. U moet het voorvoegsel van de naam van de data factory met uw naam, zodat de is gemaakt van de factory.

   ![Nieuwe data factory](media/data-factory-stored-proc-activity/new-data-factory-blade.png)         
4. Selecteer uw **Azure-abonnement**.
5. Voer een van de volgende stappen uit voor **Resourcegroep**:
   1. Klik op **nieuw** en voer een naam voor de resourcegroep.
   2. Klik op **gebruik bestaande** en selecteer een bestaande resourcegroep.  
6. Selecteer de **locatie** voor de gegevensfactory.
7. Selecteer **vastmaken aan dashboard** zodat u de data factory op het dashboard volgende keer dat u zich ziet aanmeldt.
8. Klik op de blade **Nieuwe gegevensfactory** op **Maken**.
9. U ziet de data factory die worden gemaakt in de **dashboard** van de Azure-portal. Nadat de gegevensfactory is gemaakt, ziet u de gegevensfactorypagina. Hierop wordt de inhoud van de gegevensfactory weergegeven.

   ![Data Factory-startpagina](media/data-factory-stored-proc-activity/data-factory-home-page.png)

### <a name="create-an-azure-sql-linked-service"></a>Een gekoppelde Azure SQL-service maken
Nadat de gegevensfactory is gemaakt, maakt u een Azure SQL gekoppelde service die is gekoppeld aan uw Azure SQL-database waarin de sampletable tabel en usp_sample opgeslagen procedure, aan uw data factory.

1. Klik op **auteur en implementeer** op de **Data Factory** blade voor **SProcDF** om te starten van de Data Factory-Editor.
2. Klik op **nieuw gegevensarchief** op de opdracht en kies **Azure SQL Database**. U ziet het JSON-script voor het maken van een gekoppelde Azure SQL-service in de editor.

   ![Nieuwe gegevensopslag](media/data-factory-stored-proc-activity/new-data-store.png)
3. In het JSON-script, moet u de volgende wijzigingen aanbrengen:

   1. Vervang `<servername>` met de naam van uw Azure SQL Database-server.
   2. Vervang `<databasename>` met de database waarin u de tabel en de opgeslagen procedure hebt gemaakt.
   3. Vervang `<username@servername>` met het gebruikersaccount dat toegang tot de database heeft.
   4. Vervang `<password>` met het wachtwoord voor het gebruikersaccount.

      ![Nieuwe gegevensopslag](media/data-factory-stored-proc-activity/azure-sql-linked-service.png)
4. Voor het implementeren van de gekoppelde service, klikt u op **implementeren** op de opdrachtbalk. Bevestig dat u ziet dat de AzureSqlLinkedService in de structuurweergave aan de linkerkant.

    ![structuurweergave met gekoppelde service](media/data-factory-stored-proc-activity/tree-view.png)

### <a name="create-an-output-dataset"></a>Een uitvoergegevensset maken
Als er geen gegevens niet wordt geproduceerd door de opgeslagen procedure, moet u een uitvoergegevensset voor een opgeslagen procedure-activiteit opgeven. Dat komt doordat het is de uitvoergegevensset die het schema van de activiteit stuurt (hoe vaak de activiteit wordt uitgevoerd - per uur, dagelijks, enzovoort). De uitvoergegevensset moet gebruiken een **gekoppelde service** die verwijst naar een Azure SQL Database of een Azure SQL Data Warehouse of een SQL Server-Database waarin u wilt dat de opgeslagen procedure om uit te voeren. De uitvoergegevensset kan fungeren als een manier om door te geven het resultaat van de opgeslagen procedure voor verdere verwerking door een andere activiteit ([Koppelingsactiviteiten](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in de pijplijn. Echter, Data Factory wordt niet automatisch schrijven de uitvoer van een opgeslagen procedure aan deze gegevensset. Het is de opgeslagen procedure die naar een SQL-tabel die de uitvoergegevensset naar verwijst schrijft. In sommige gevallen kan de uitvoergegevensset mag een **dummy gegevensset** (een gegevensset die verwijst naar een tabel waarop geen echt uitvoer van de opgeslagen procedure). Deze dummy gegevensset wordt alleen gebruikt om de planning voor het uitvoeren van de opgeslagen procedure-activiteit opgeven. 

1. Klik op **... Meer** op de werkbalk klikt u op **nieuwe gegevensset**, en klikt u op **Azure SQL**. **Nieuwe gegevensset** op de opdrachtbalk en selecteer **Azure SQL**.

    ![structuurweergave met gekoppelde service](media/data-factory-stored-proc-activity/new-dataset.png)
2. De volgende JSON-script in om de JSON-editor te kopiëren en plakken.

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
3. Voor het implementeren van de gegevensset, klikt u op **implementeren** op de opdrachtbalk. Bevestig dat u de gegevensset in de structuurweergave wordt weergegeven ziet.

    ![structuurweergave met gekoppelde services](media/data-factory-stored-proc-activity/tree-view-2.png)

### <a name="create-a-pipeline-with-sqlserverstoredprocedure-activity"></a>Een pijplijn maken met SqlServerStoredProcedure activiteit
Nu gaan we een pijplijn maken met een opgeslagen procedure-activiteit. 

U ziet de volgende eigenschappen: 

- De **type** eigenschap is ingesteld op **SqlServerStoredProcedure**. 
- De **storedProcedureName** in het type eigenschappen is ingesteld op **usp_sample** (naam van de opgeslagen procedure).
- De **storedProcedureParameters** sectie bevat een parameter met de naam **datum-/**. Naam en het hoofdlettergebruik van de parameter in JSON moeten overeenkomen met de naam en het hoofdlettergebruik van de parameter in de definitie van de opgeslagen procedure. Als u null zijn voor een parameter doorgeven moet, gebruikt u de syntaxis: `"param1": null` (zonder hoofdletters).
 
1. Klik op **... Meer** op de opdrachtbalk en klik op **nieuwe pijplijn**.
2. De volgende JSON-codefragment kopiëren/plakken:   

    ```JSON
    {
        "name": "SprocActivitySamplePipeline",
        "properties": {
            "activities": [
                {
                    "type": "SqlServerStoredProcedure",
                    "typeProperties": {
                        "storedProcedureName": "usp_sample",
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
3. Dubbelklik op de gegevensset in de diagramweergave `sprocsampleout`. U ziet de segmenten in de status gereed. Er moet worden vijf segmenten omdat een segment wordt geproduceerd voor elk uur tussen de begin- en eindtijd van de JSON.

    ![Tegel diagram](media/data-factory-stored-proc-activity/data-factory-slices.png)
4. Wanneer een segment wordt **gereed** staat, voer een `select * from sampletable` query op de Azure SQL-database om te controleren dat de gegevens is ingevoegd in de tabel door de opgeslagen procedure.

   ![Uitvoergegevens](./media/data-factory-stored-proc-activity/output.png)

   Zie [bewaken van de pijplijn](data-factory-monitor-manage-pipelines.md) voor gedetailleerde informatie over het controleren van Azure Data Factory-pijplijnen.  


## <a name="specify-an-input-dataset"></a>Geef een invoergegevensset
In het scenario hoeft opgeslagen procedureactiviteit niet elke invoergegevenssets. Als u een invoergegevensset opgeeft, wordt de activiteit opgeslagen procedure niet uitvoeren tot het segment de status van de invoergegevensset beschikbaar (in de status gereed is). De gegevensset is een externe gegevensset (die niet door een andere activiteit in de dezelfde pijplijn wordt geproduceerd) of een interne gegevensset die wordt geproduceerd door een upstream-activiteit (de activiteit die wordt uitgevoerd voordat deze activiteit). U kunt meerdere invoergegevenssets voor de opgeslagen procedure-activiteit opgeven. Als u dit doet, wordt de activiteit opgeslagen procedure uitgevoerd alleen wanneer alle segmenten van de invoergegevensset beschikbaar (in de status gereed zijn). De invoergegevensset kan niet worden gebruikt in de opgeslagen procedure als een parameter. Het wordt alleen gebruikt om te controleren van de afhankelijkheid voordat u begint met de opgeslagen procedure-activiteit.

## <a name="chaining-with-other-activities"></a>Koppeling met andere activiteiten
Als u een upstream-activiteit met deze activiteit zijn gekoppeld wilt, geeft u de uitvoer van de upstream-activiteit als de invoer van deze activiteit. Als u dit doet, wordt de activiteit opgeslagen procedure niet uitvoeren totdat de upstream-activiteit is voltooid en de uitvoergegevensset van de upstream-activiteit (in de status gereed) dat beschikbaar is. U kunt uitvoergegevenssets uit meerdere upstream activiteiten opgeven als invoergegevenssets van de activiteit opgeslagen procedure. Als u dit doet, wordt de activiteit opgeslagen procedure uitgevoerd alleen wanneer alle segmenten van de invoergegevensset beschikbaar zijn.  

In het volgende voorbeeld wordt de uitvoer van de kopieeractiviteit: OutputDataset invoer van de activiteit opgeslagen procedure is. Daarom wordt de activiteit opgeslagen procedure niet uitvoeren totdat de kopieerbewerking is voltooid en het segment OutputDataset beschikbaar (in de status gereed is). Als u meerdere invoergegevenssets opgeeft, wordt de activiteit opgeslagen procedure niet uitvoeren totdat alle segmenten van de invoergegevensset beschikbaar (in de status gereed zijn). De invoer gegevenssets kunnen niet rechtstreeks als parameters voor de opgeslagen procedure-activiteit worden gebruikt. 

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

Op deze manier om te koppelen van de store procedure-activiteit met **downstream activiteiten** (de activiteiten die worden uitgevoerd nadat de activiteit opgeslagen procedure is voltooid), geef de uitvoergegevensset van de activiteit opgeslagen procedure als de invoer van de downstream-activiteit in de pijplijn.

> [!IMPORTANT]
> Het kopiëren van gegevens in Azure SQL Database of SQL Server, kunt u de **SqlSink** in de kopieeractiviteit om aan te roepen een opgeslagen procedure met behulp van de **sqlWriterStoredProcedureName** eigenschap. Zie voor meer informatie, [aanroepen van de opgeslagen procedure van kopieeractiviteit](data-factory-invoke-stored-procedure-from-copy-activity.md). Zie voor meer informatie over de eigenschap, de volgende artikelen voor de connector: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties).
>  
> Het kopiëren van gegevens uit Azure SQL Database of SQL Server of Azure SQL Data Warehouse, kunt u configureren **SqlSource** in de kopieeractiviteit om aan te roepen een opgeslagen procedure voor het lezen van gegevens vanuit de brondatabase met behulp van de  **sqlReaderStoredProcedureName** eigenschap. Zie voor meer informatie de volgende artikelen voor de connector: [Azure SQL Database](data-factory-azure-sql-connector.md#copy-activity-properties), [SQL Server](data-factory-sqlserver-connector.md#copy-activity-properties), [Azure SQL Data Warehouse](data-factory-azure-sql-data-warehouse-connector.md#copy-activity-properties)          

## <a name="json-format"></a>JSON-indeling
Dit is de JSON-indeling voor het definiëren van een activiteit opgeslagen Procedure:

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

| Eigenschap | Description | Vereist |
| --- | --- | --- |
| naam | Naam van de activiteit |Ja |
| description |Beschrijving van het doel waarvoor de activiteit wordt gebruikt |Nee |
| type | Moet worden ingesteld op: **SqlServerStoredProcedure** | Ja |
| invoer | Optioneel. Als u een invoergegevensset opgeeft, moet deze zijn beschikbaar (in de status 'Gereed') voor de opgeslagen procedure-activiteit om uit te voeren. De invoergegevensset kan niet worden gebruikt in de opgeslagen procedure als een parameter. Het wordt alleen gebruikt om te controleren van de afhankelijkheid voordat u begint met de opgeslagen procedure-activiteit. |Nee |
| uitvoer | U moet een uitvoergegevensset voor een opgeslagen procedure-activiteit opgeven. Uitvoergegevensset Hiermee geeft u de **planning** voor de opgeslagen procedure-activiteit (elk uur, wekelijks, maandelijks, enzovoort). <br/><br/>De uitvoergegevensset moet gebruiken een **gekoppelde service** die verwijst naar een Azure SQL Database of een Azure SQL Data Warehouse of een SQL Server-Database waarin u wilt dat de opgeslagen procedure om uit te voeren. <br/><br/>De uitvoergegevensset kan fungeren als een manier om door te geven het resultaat van de opgeslagen procedure voor verdere verwerking door een andere activiteit ([Koppelingsactiviteiten](data-factory-scheduling-and-execution.md#multiple-activities-in-a-pipeline) in de pijplijn. Echter, Data Factory wordt niet automatisch schrijven de uitvoer van een opgeslagen procedure aan deze gegevensset. Het is de opgeslagen procedure die naar een SQL-tabel die de uitvoergegevensset naar verwijst schrijft. <br/><br/>In sommige gevallen kan de uitvoergegevensset mag een **dummy gegevensset**, die wordt gebruikt om alleen op te geven van de planning voor het uitvoeren van de activiteit opgeslagen procedure. |Ja |
| storedProcedureName |Geef de naam van de opgeslagen procedure in de Azure SQL-database of Azure SQL Data Warehouse of SQL Server-database die wordt vertegenwoordigd door de gekoppelde service die gebruikmaakt van de uitvoertabel. |Ja |
| storedProcedureParameters |Geef waarden op voor de opgeslagen-procedureparameters. Als u moet null zijn voor een parameter door te geven, gebruikt u de syntaxis: "param1": null (alleen kleine letters). Zie het volgende voorbeeld voor meer informatie over het gebruik van deze eigenschap. |Nee |

## <a name="passing-a-static-value"></a>Doorgeven van een statische waarde
Nu we eens een andere kolom met de naam 'Scenario' toe te voegen in de tabel met een statische waarde met de naam 'Voorbeeld Document'.

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
CREATE PROCEDURE usp_sample2 @DateTime nvarchar(127) , @Scenario nvarchar(127)

AS

BEGIN
    INSERT INTO [sampletable2]
    VALUES (newid(), @DateTime, @Scenario)
END
```

Nu geven de **Scenario** parameter en de waarde van de activiteit opgeslagen procedure. De **typeProperties** sectie in het voorgaande voorbeeld ziet eruit als in het volgende codefragment:

```JSON
"typeProperties":
{
    "storedProcedureName": "usp_sample",
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
                    "storedProcedureName": "usp_sample2",
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
