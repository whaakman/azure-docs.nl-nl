---
title: Lookup-activiteit in Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van lookup-activiteit om te controleren of een waarde van een externe bron. Er kan naar deze uitvoer worden verwezen door volgende activiteiten.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/15/2018
ms.author: shlo
ms.openlocfilehash: 25ed439674fcf7136e29034eb97e0652ae9ba111
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38237829"
---
# <a name="lookup-activity-in-azure-data-factory"></a>Lookup-activiteit in Azure Data Factory

Lookup-activiteit kan worden gebruikt om een gegevensset ophalen uit een van de ADF-ondersteunde gegevensbron.  Het kan worden gebruikt in het volgende scenario:
- Dynamisch te bepalen welke objecten (bestanden, tabellen, enzovoort) om te werken op in een volgende activiteit, in plaats van hard-coding naam van het object

Lookup-activiteit kan lezen en de inhoud van een configuratiebestand, een configuratietabel of het resultaat van het uitvoeren van een query of een opgeslagen procedure.  De uitvoer van activiteit Lookup kan worden gebruikt in een latere kopiëren of activiteiten voor gegevenstransformatie als het een singleton-waarde of in een ForEach-activiteit gebruikt als het is een matrix van kenmerken.

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

De volgende gegevensbronnen worden ondersteund voor het opzoeken van. Het maximum aantal rijen kan worden geretourneerd door het opzoeken van de activiteit is **5000**, en tot **2MB** in grootte. En de maximale duur voor opzoekactiviteit voordat de time-out is momenteel één uur.

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
Naam | Beschrijving | Type | Vereist?
---- | ----------- | ---- | --------
gegevensset | Bevat de gegevensset-referentie voor de zoekopdracht. Informatie ophalen uit de sectie 'Eigenschappen van gegevensset' in elke betreffende connector-artikel. | Sleutel/waarde-paar | Ja
source | Bevat de eigenschappen van gegevensset-specifieke gegevensbron, hetzelfde als de bron voor kopiëren-activiteit. Informatie ophalen uit de sectie 'Eigenschappen van de Kopieeractiviteit' in elke betreffende connector-artikel. | Sleutel/waarde-paar | Ja
firstRowOnly | Hiermee wordt aangegeven of alleen de eerste rij of alle rijen retourneren. | Booleaans | Nee. De standaardwaarde is `true`.

**Houd rekening met de volgende punten:**

1. Bronkolom met ByteArray type wordt niet ondersteund.
2. Structuur wordt niet ondersteund in de definitie van de gegevensset. Voor indeling tekstbestanden specifiek, kunt u de rij met koppen de kolomnaam te geven.
3. Als de lookup-bron een JSON-bestanden is, de `jsonPathDefinition` instellen voor opnieuw vormgeven van het JSON-object wordt niet ondersteund, het gehele objecten wordt opgehaald.

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

* **Wanneer `firstRowOnly` is ingesteld op `false`** , de indeling van de uitvoer is zoals wordt weergegeven in de volgende code. Een `count` veld geeft aan hoeveel records worden geretourneerd, en gedetailleerde waarden worden weergegeven onder een vaste `value` matrix. In dat geval de lookup-activiteit meestal wordt gevolgd door een [Foreach-activiteit](control-flow-for-each-activity.md). U kunt doorgeven de `value` -matrix naar de ForEach-activiteit `items` veld met behulp van het patroon van `@activity('MyLookupActivity').output.value`. Met access-elementen in de `value` matrix, gebruikt u de volgende syntaxis: `@{activity('lookupActivity').output.value[zero based index].propertyname}`. Hier volgt een voorbeeld: `@{activity('lookupActivity').output.value[0].tablename}`.

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

## <a name="example"></a>Voorbeeld
In dit voorbeeld kopieert de kopieeractiviteit gegevens vanuit een SQL-tabel in uw Azure SQL Database-exemplaar naar Azure Blob-opslag. De naam van de SQL-tabel is opgeslagen in een JSON-bestand in Blob-opslag. De lookup-activiteit, zoekt u de naam van de tabel tijdens runtime. Deze aanpak kunt u JSON naar dynamisch worden gewijzigd zonder deze opnieuw implementeren van pijplijnen of gegevenssets. 

In dit voorbeeld ziet u lookup voor alleen de eerste rij. Voor het opzoeken van alle rijen en om te koppelen van de resultaten met ForEach-activiteit, raadpleegt u de voorbeelden in [meerdere tabellen bulksgewijs kopiëren met behulp van Azure Data Factory](tutorial-bulk-copy.md).

### <a name="pipeline"></a>Pijplijn
Deze pijplijn bevat twee activiteiten: *lookup* en *kopie*. 

- De lookup-activiteit is geconfigureerd voor het gebruik van LookupDataset, die naar een locatie in Azure Blob-opslag verwijst. De lookup-activiteit leest de naam van de SQL-tabel uit een JSON-bestand op deze locatie. 
- De kopieeractiviteit maakt gebruik van de uitvoer van de lookup-activiteit (naam van de SQL-tabel). De eigenschap tableName in de brongegevensset (SourceDataset) is geconfigureerd voor het gebruik van de uitvoer van de lookup-activiteit. De kopieeractiviteit kopieert gegevens van de SQL-tabel naar een locatie in Azure Blob-opslag die is opgegeven door de eigenschap SinkDataset. 


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
De lookup-gegevensset verwijst naar de *sourcetable.json* bestand in de map voor het opzoeken van Azure Storage die opgegeven door de AzureStorageLinkedService-type. 

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

### <a name="source-dataset-for-the-copy-activity"></a>Brongegevensset voor de kopieeractiviteit
De brongegevensset maakt gebruik van de uitvoer van de lookup-activiteit, dit de naam van de SQL-tabel is. De kopieeractiviteit kopieert gegevens van deze SQL-tabel naar een locatie in Azure Blob-opslag die opgegeven door de sink-gegevensset. 

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

### <a name="sink-dataset-for-the-copy-activity"></a>Sink-gegevensset voor de kopieeractiviteit
De kopieeractiviteit worden gegevens gekopieerd van de SQL-tabel naar de *filebylookup.csv* -bestand in de *csv* map in de Azure-opslag die opgegeven door de eigenschap AzureStorageLinkedService. 

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

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [Execute Pipeline-activiteit](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Activiteit metagegevens ophalen](control-flow-get-metadata-activity.md)
- [Activiteit Web](control-flow-web-activity.md)
