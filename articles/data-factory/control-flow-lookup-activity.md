---
title: Lookup-activiteit in Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van Lookup-activiteit om te controleren of een waarde van een externe bron. Deze uitvoer kan verder worden verwezen door volgende activiteiten.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: shlo
ms.openlocfilehash: bc695bf8398a39460eff9bbe4f791ba92b0fa7e0
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54019304"
---
# <a name="lookup-activity-in-azure-data-factory"></a>Lookup-activiteit in Azure Data Factory

Lookup-activiteit kunt u een gegevensset ophalen uit een van de Azure Data Factory-ondersteunde gegevensbronnen. Gebruik deze in het volgende scenario:
- Dynamisch te bepalen welke objecten moeten worden uitgevoerd in een volgende activiteit, in plaats van code de objectnaam. Enkele voorbeelden van het object zijn tabellen en bestanden.

Lookup-activiteit leest en retourneert de inhoud van een configuratiebestand of de tabel. Het wordt ook het resultaat van het uitvoeren van een query of een opgeslagen procedure. De uitvoer van activiteit Lookup kan worden gebruikt in een latere kopiëren of een transformatieactiviteit als het een singleton-waarde. De uitvoer kan worden gebruikt in een ForEach-activiteit als het een matrix van kenmerken.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

De volgende gegevensbronnen worden ondersteund voor een Lookup-activiteit. Het grootste aantal rijen dat kan worden geretourneerd door de Lookup-activiteit is 5.000, maximaal 2 MB in grootte. Op dit moment de langste duur voor het opzoeken van activiteit voordat de time-out is één uur.

[!INCLUDE [data-factory-v2-supported-data-stores](../../includes/data-factory-v2-supported-data-stores-for-lookup-activity.md)]

## <a name="syntax"></a>Syntaxis

```json
{
    "name": "LookupActivity",
    "type": "Lookup",
    "typeProperties": {
        "source": {
            "type": "<source type>"
            <additional source specific properties (optional)>
        },
        "dataset": { 
            "referenceName": "<source dataset name>",
            "type": "DatasetReference"
        },
        "firstRowOnly": false
    }
}
```

## <a name="type-properties"></a>Type-eigenschappen
Name | Description | Type | Vereist?
---- | ----------- | ---- | --------
Gegevensset | Bevat de gegevensset-referentie voor de zoekopdracht. Informatie van de **gegevensseteigenschappen** sectie in elke betreffende connector-artikel. | Sleutel/waarde-paar | Ja
source | Bevat de eigenschappen van gegevensset-specifieke gegevensbron, gelijk zijn aan de Kopieeractiviteit-bron. Informatie van de **Kopieeractiviteit eigenschappen** sectie in elke betreffende connector-artikel. | Sleutel/waarde-paar | Ja
firstRowOnly | Hiermee wordt aangegeven of alleen de eerste rij of alle rijen retourneren. | Booleaans | Nee. De standaardwaarde is `true`.

> [!NOTE]

> * Kolommen met de gegevensbron **ByteArray** type worden niet ondersteund.
> * **Structuur** in definities van de gegevensset wordt niet ondersteund. Gebruik voor tekstindeling bestanden, de rij met koppen de kolomnaam te geven.
> * Als de lookup-bron een JSON-bestand is, de `jsonPathDefinition` instellen voor het omzetten van het JSON-object wordt niet ondersteund. De volledige objecten opgehaald.

## <a name="use-the-lookup-activity-result-in-a-subsequent-activity"></a>Gebruik het resultaat van de activiteit opzoeken in een volgende activiteit

De lookup-resultaat wordt geretourneerd de `output` sectie van de uitvoering van de activiteit.

* **Wanneer `firstRowOnly` is ingesteld op `true` (standaard)**, de indeling van de uitvoer is zoals wordt weergegeven in de volgende code. De lookup-resultaat is onder een vaste `firstRow` sleutel. Als u het resultaat van de volgende activiteit, gebruikt u het patroon van `@{activity('MyLookupActivity').output.firstRow.TableName}`.

    ```json
    {
        "firstRow":
        {
            "Id": "1",
            "TableName" : "Table1"
        }
    }
    ```

* **Wanneer `firstRowOnly` is ingesteld op `false`** , de indeling van de uitvoer is zoals wordt weergegeven in de volgende code. Een `count` veld geeft aan hoeveel records geretourneerd. Gedetailleerde waarden worden weergegeven onder een vaste `value` matrix. In dat geval de Lookup-activiteit wordt gevolgd door een [Foreach-activiteit](control-flow-for-each-activity.md). U geeft de `value` -matrix naar de ForEach-activiteit `items` veld met behulp van het patroon van `@activity('MyLookupActivity').output.value`. Met access-elementen in de `value` matrix, gebruikt u de volgende syntaxis: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. Een voorbeeld is `@{activity('lookupActivity').output.value[0].tablename}`.

    ```json
    {
        "count": "2",
        "value": [
            {
                "Id": "1",
                "TableName" : "Table1"
            },
            {
                "Id": "2",
                "TableName" : "Table2"
            }
        ]
    } 
    ```

### <a name="copy-activity-example"></a>Voorbeeld van de activiteit kopiëren
In dit voorbeeld kopieert Kopieeractiviteit gegevens vanuit een SQL-tabel in uw Azure SQL Database-exemplaar naar Azure Blob-opslag. De naam van de SQL-tabel is opgeslagen in een JSON-bestand in Blob-opslag. De Lookup-activiteit, zoekt u de naam van de tabel tijdens runtime. JSON wordt dynamisch gewijzigd met behulp van deze benadering. U hoeft niet opnieuw implementeren van pijplijnen of gegevenssets. 

In dit voorbeeld ziet u lookup voor alleen de eerste rij. Voor het opzoeken van alle rijen en om te koppelen van de resultaten met ForEach-activiteit, raadpleegt u de voorbeelden in [meerdere tabellen bulksgewijs kopiëren met behulp van Azure Data Factory](tutorial-bulk-copy.md).

### <a name="pipeline"></a>Pijplijn
Deze pijplijn bevat twee activiteiten: Lookup- en kopiëren. 

- De Lookup-activiteit is geconfigureerd voor het gebruik van **LookupDataset**, die verwijst naar een locatie in Azure Blob-opslag. De Lookup-activiteit leest de naam van de SQL-tabel uit een JSON-bestand op deze locatie. 
- Kopieeractiviteit maakt gebruik van de uitvoer van de Lookup-activiteit, dit de naam van de SQL-tabel is. De **tableName** eigenschap in de **SourceDataset** is geconfigureerd voor het gebruik van de uitvoer van de Lookup-activiteit. Activiteit kopieën gegevens kopiëren van de SQL-tabel naar een locatie in Azure Blob-opslag. De locatie wordt opgegeven door de **SinkDataset** eigenschap. 

```json
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "LookupActivity",
                "type": "Lookup",
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                    },
                    "dataset": { 
                        "referenceName": "LookupDataset", 
                        "type": "DatasetReference" 
                    }
                }
            },
            {
                "name": "CopyActivity",
                "type": "Copy",
                "typeProperties": {
                    "source": { 
                        "type": "SqlSource", 
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.firstRow.tableName}" 
                    },
                    "sink": { 
                        "type": "BlobSink" 
                    }
                },                
                "dependsOn": [ 
                    { 
                        "activity": "LookupActivity", 
                        "dependencyConditions": [ "Succeeded" ] 
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
            }
        ]
    }
}
```

### <a name="lookup-dataset"></a>Lookup-gegevensset
De **lookup** gegevensset is de **sourcetable.json** bestand in de map voor het opzoeken van Azure Storage opgegeven door de **AzureStorageLinkedService** type. 

```json
{
    "name": "LookupDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "lookup",
            "fileName": "sourcetable.json",
            "format": {
                "type": "JsonFormat",
                "filePattern": "SetOfObjects"
            }
        },
        "linkedServiceName": {
            "referenceName": "AzureStorageLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="source-dataset-for-copy-activity"></a>**Bron** gegevensset voor de Kopieeractiviteit
De **bron** gegevensset maakt gebruik van de uitvoer van de Lookup-activiteit, dit de naam van de SQL-tabel is. Activiteit kopieën gegevens kopiëren van deze SQL-tabel naar een locatie in Azure Blob-opslag. De locatie wordt opgegeven door de **sink** gegevensset. 

```json
{
    "name": "SourceDataset",
    "properties": {
        "type": "AzureSqlTable",
        "typeProperties":{
            "tableName": "@{activity('LookupActivity').output.firstRow.tableName}"
        },
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="sink-dataset-for-copy-activity"></a>**Sink-** gegevensset voor de Kopieeractiviteit
Kopieeractiviteit worden gegevens gekopieerd van de SQL-tabel naar de **filebylookup.csv** -bestand in de **csv** map in Azure Storage. Het bestand is opgegeven door de **AzureStorageLinkedService** eigenschap. 

```json
{
    "name": "SinkDataset",
    "properties": {
        "type": "AzureBlob",
        "typeProperties": {
            "folderPath": "csv",
            "fileName": "filebylookup.csv",
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

### <a name="azure-storage-linked-service"></a>Een gekoppelde Azure Storage-service
Dit opslagaccount bevat de JSON-bestand met de namen van de SQL-tabellen. 

```json
{
    "properties": {
        "type": "AzureStorage",
        "typeProperties": {
            "connectionString": {
                "value": "DefaultEndpointsProtocol=https;AccountName=<StorageAccountName>;AccountKey=<StorageAccountKey>",
                "type": "SecureString"
            }
        }
    },
        "name": "AzureStorageLinkedService"
}
```

### <a name="azure-sql-database-linked-service"></a>Een gekoppelde Azure SQL Database-service
Dit exemplaar van Azure SQL Database bevat de gegevens moeten worden gekopieerd naar Blob-opslag. 

```json
{
    "name": "AzureSqlLinkedService",
    "properties": {
        "type": "AzureSqlDatabase",
        "description": "",
        "typeProperties": {
            "connectionString": {
                "value": "Server=<server>;Initial Catalog=<database>;User ID=<user>;Password=<password>;",
                "type": "SecureString"
            }
        }
    }
}
```

### <a name="sourcetablejson"></a>SourceTable.JSON

#### <a name="set-of-objects"></a>Set van objecten

```json
{
  "Id": "1",
  "tableName": "Table1"
}
{
   "Id": "2",
  "tableName": "Table2"
}
```

#### <a name="array-of-objects"></a>Matrix met objecten

```json
[ 
    {
        "Id": "1",
        "tableName": "Table1"
    },
    {
        "Id": "2",
        "tableName": "Table2"
    }
]
```

## <a name="limitations-and-workarounds"></a>Beperkingen en tijdelijke oplossingen

Hier zijn enkele beperkingen van de Lookup-activiteit en voorgestelde oplossingen.

| Beperking | Tijdelijke oplossing |
|---|---|
| De Lookup-activiteit heeft maximaal 5000 rijen en een maximale grootte van 2 MB. | Ontwerp een twee niveaus pijplijn waar de buitenste pijplijn doorloopt over een binnenste-pijplijn waarmee gegevens worden opgehaald die niet groter is dan het maximum aantal rijen of de grootte. |
| | |

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [Execute Pipeline-activiteit](control-flow-execute-pipeline-activity.md)
- [Activiteit ForEach](control-flow-for-each-activity.md)
- [De activiteit GetMetadata](control-flow-get-metadata-activity.md)
- [Activiteit Web](control-flow-web-activity.md)
