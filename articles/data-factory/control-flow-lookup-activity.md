---
title: De activiteit opzoeken in Azure Data Factory | Microsoft Docs
description: Informatie over het gebruik van de activiteit Lookup voor het opzoeken van een waarde van een externe bron. Er kan naar deze uitvoer worden verwezen door volgende activiteiten.
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: shlo
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2017
ms.author: spelluru
ms.openlocfilehash: 30173f8eea2ccbbcd44018596cf34b3769a64b50
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="lookup-activity-in-azure-data-factory"></a>De activiteit opzoeken in Azure Data Factory
De Lookup Activity kan worden gebruikt om een record/tabelnaam/waarde van een externe bron te lezen of op te zoeken. Er kan naar deze uitvoer worden verwezen door volgende activiteiten. 

De volgende gegevensbronnen worden momenteel ondersteund voor het opzoeken van:
- JSON-bestand in Azure Blob
- Lokale JSON-bestand
- Azure SQL Database (JSON-gegevens geconverteerd van een query)
- Azure Table Storage (JSON-gegevens geconverteerd van een query)

Lookup-activiteit is handig als u wilt een lijst met bestanden dynamisch ophalen-records/tabellen vanuit een configuratiebestand of een gegevensbron. De uitvoer van de activiteit kan verder worden gebruikt door andere activiteiten uit te voeren specifieke verwerking op alleen die items.

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Data Factory V1 documentatie](v1/data-factory-introduction.md).


## <a name="example"></a>Voorbeeld
In dit voorbeeld kopieert de kopieeractiviteit gegevens uit een SQL-tabel in Azure SQL-database naar Azure Blob Storage. De naam van de SQL-tabel is opgeslagen in een JSON-bestand in Blob Storage. De activiteit opzoeken, zoekt u de naam van de tabel tijdens runtime. Deze aanpak kunt JSON dynamisch worden gewijzigd zonder pijplijnen/gegevenssets opnieuw distribueren. 

### <a name="pipeline"></a>Pijplijn
Deze pipeline bevat twee activiteiten: **opzoeken** en **kopie**. 

- De activiteit Lookup is geconfigureerd voor gebruik van de LookupDataset die naar een locatie in een Azure Blob Storage verwijst. De activiteit opzoeken leest de naam van de SQL-tabel uit een JSON-bestand op deze locatie. 
- De kopieeractiviteit wordt de uitvoer van de activiteit lookup (naam van de SQL-tabel). De tabelnaam in de bron-gegevensset (SourceDataset) is geconfigureerd voor gebruik van de uitvoer van de activiteit opzoeken. De kopieeractiviteit kopieert gegevens van de SQL-tabel naar een locatie in Azure Blob-opslag die is opgegeven door de SinkDataset. 


```json
{
    "name": "LookupPipelineDemo",
    "properties": {
        "activities": [
            {
                "name": "LookupActivity",
                "type": "Lookup",
                "typeProperties": {
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
                        "sqlReaderQuery": "select * from @{activity('LookupActivity').output.tableName}" 
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
De gegevensset lookup verwijst naar het bestand sourcetable.json in de map opzoeken in de Azure-opslag door AzureStorageLinkedService opgegeven. 

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

### <a name="source-dataset-for-the-copy-activity"></a>Gegevensset voor de kopieeractiviteit bron
De bron-gegevensset maakt gebruik van de uitvoer van de activiteit opzoeken, die de naam van de SQL-tabel. De kopieeractiviteit kopieert gegevens van deze SQL-tabel naar een locatie in Azure Blob Storage is opgegeven door de sink-gegevensset. 

```json
{
    "name": "SourceDataset",
    "properties": {
        "type": "AzureSqlTable",
        "typeProperties":{
            "tableName": "@{activity('LookupActivity').output.tableName}"
        },
        "linkedServiceName": {
            "referenceName": "AzureSqlLinkedService",
            "type": "LinkedServiceReference"
        }
    }
}
```

### <a name="sink-dataset-for-the-copy-activity"></a>Gegevensset voor de kopieeractiviteit sink
De kopieeractiviteit kopieert gegevens van de SQL-tabel filebylookup.csv-bestand in het csv-map in de Azure-opslag dat is opgegeven door de AzureStorageLinkedService. 

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
Deze Azure SQL-database bevat de gegevens moeten worden gekopieerd naar de blob-opslag. 

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
  "tableName": "Table1",
}
{
   "Id": "2",
  "tableName": "Table2",
}
```
#### <a name="array-of-objects"></a>Matrix met objecten

```json
[ 
    {
        "Id": "1",
          "tableName": "Table1",
    }
    {
        "Id": "2",
        "tableName": "Table2",
    }
]
```



## <a name="type-properties"></a>Type-eigenschappen
Naam | Beschrijving | Type | Vereist
---- | ----------- | ---- | --------
Gegevensset | Het kenmerk dataset is voor de dataset-verwijzing voor de zoekopdracht. Op dit moment wordt zijn de gegevensset ondersteunde typen:<ul><li>FileShareDataset</li><li>AzureBlobDataset</li><li>AzureSqlTableDataset</li><li>AzureTableDataset</li> | Sleutel-waardepaar | Ja
Bron | Eigenschappen van gegevensset-specifieke gegevensbron, dezelfde zijn als kopieerbron voor de activiteit | Sleutel-waardepaar | Nee
firstRowOnly | Retourneert de eerste rij of alle rijen. | Booleaanse waarde | Nee

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Get Metadata Activity](control-flow-get-metadata-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
