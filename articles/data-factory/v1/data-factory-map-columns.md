---
title: Toewijzing van kolommen in Azure Data Factory | Microsoft Docs
description: Leer hoe u kolommen in de gegevensbron worden toegewezen aan de doelkolommen.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: 1b009ac2ca42e9804b88989b55b2e73524732550
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54017460"
---
# <a name="map-source-dataset-columns-to-destination-dataset-columns"></a>Bronkolommen voor de gegevensset worden toegewezen aan de gegevensset doelkolommen
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory. 

Kolomtoewijzing kan worden gebruikt om op te geven hoe kolommen opgegeven in de 'structuur"van de toewijzing van bron-tabel met kolommen opgegeven in de 'structuur" van sink-tabel. De **columnMapping** eigenschap is beschikbaar in de **typeProperties** sectie van de kopieeractiviteit.

Kolomtoewijzing ondersteunt de volgende scenario's:

* Alle kolommen in de structuur van de gegevensset bron worden toegewezen aan alle kolommen in de structuur van de sink-gegevensset.
* Een subset van de kolommen in de structuur van de gegevensset gegevensbron is toegewezen aan alle kolommen in de structuur van de sink-gegevensset.

Hier volgen foutcondities die leiden tot een uitzondering:

* Minder kolommen of meer kolommen in de 'structuur"van de sink-tabel dan opgegeven in de toewijzing.
* Dubbele toewijzing.
* Resultaat van de SQL-query heeft geen naam van een kolom die is opgegeven in de toewijzing.

> [!NOTE]
> De volgende voorbeelden voor Azure SQL en Azure Blob maar zijn van toepassing op elk gegevensarchief die ondersteuning biedt voor rechthoekige gegevenssets. Gegevensset en de gekoppelde servicedefinities in de voorbeelden om te verwijzen naar gegevens in de relevante gegevens aanpassen.

## <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Voorbeeld 1: kolom toewijzen uit Azure SQL naar Azure-blob
In dit voorbeeld heeft een structuur van de invoertabel en of deze verwijst naar een SQL-tabel in een Azure SQL database.

```json
{
    "name": "AzureSQLInput",
    "properties": {
        "structure": 
         [
           { "name": "userid"},
           { "name": "name"},
           { "name": "group"}
         ],
        "type": "AzureSqlTable",
        "linkedServiceName": "AzureSqlLinkedService",
        "typeProperties": {
            "tableName": "MyTable"
        },
        "availability": {
            "frequency": "Hour",
            "interval": 1
        },
        "external": true,
        "policy": {
            "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
            }
        }
    }
}
```

In dit voorbeeld wordt de uitvoertabel heeft een structuur en of deze verwijst naar een blob in een Azure blob-opslag.

```json
{
    "name": "AzureBlobOutput",
    "properties":
    {
         "structure": 
          [
                { "name": "myuserid"},
                { "name": "myname" },
                { "name": "mygroup"}
          ],
        "type": "AzureBlob",
        "linkedServiceName": "StorageLinkedService",
        "typeProperties": {
            "folderPath": "mycontainer/myfolder",
            "fileName":"myfile.csv",
            "format":
            {
                "type": "TextFormat",
                "columnDelimiter": ","
            }
        },
        "availability":
        {
            "frequency": "Hour",
            "interval": 1
        }
    }
}
```

De volgende JSON definieert een kopieeractiviteit in een pijplijn. De kolommen uit de bron die zijn toegewezen aan kolommen in sink (**columnMappings**) met behulp van de **Translator** eigenschap.

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "Copy",
    "inputs":  [ { "name": "AzureSQLInput"  } ],
    "outputs":  [ { "name": "AzureBlobOutput" } ],
    "typeProperties":    {
        "source":
        {
            "type": "SqlSource"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup, Name: MyName"
        }
    },
   "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
**Kolom toewijzing stroom:**

![Kolom toewijzing stroom](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Voorbeeld 2: kolom toewijzing met SQL-query uit Azure SQL naar Azure-blob
In dit voorbeeld wordt een SQL-query gebruikt om gegevens te extraheren uit Azure SQL in plaats van op te geven de tabelnaam en de kolomnamen in de sectie "structuur". 

```json
{
    "name": "CopyActivity",
    "description": "description", 
    "type": "CopyActivity",
    "inputs":  [ { "name": " AzureSQLInput"  } ],
    "outputs":  [ { "name": " AzureBlobOutput" } ],
    "typeProperties":
    {
        "source":
        {
            "type": "SqlSource",
            "SqlReaderQuery": "$$Text.Format('SELECT * FROM MyTable WHERE StartDateTime = \\'{0:yyyyMMdd-HH}\\'', WindowStart)"
        },
        "sink":
        {
            "type": "BlobSink"
        },
        "Translator": 
        {
            "type": "TabularTranslator",
            "ColumnMappings": "UserId: MyUserId, Group: MyGroup,Name: MyName"
        }
    },
    "scheduler": {
          "frequency": "Hour",
          "interval": 1
        }
}
```
In dit geval worden de resultaten van de query eerst toegewezen aan kolommen die zijn opgegeven in 'structuur"van de bron. De kolommen van bron "structuur" worden vervolgens toegewezen aan kolommen in sink "structuur" met regels die zijn opgegeven in de columnMappings.  Stel dat de query retourneert 5 kolommen, twee of meer kolommen dan die zijn opgegeven in de 'structuur"van de bron.

**Kolom toewijzing stroom**

![Kolom toewijzing stroom-2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a>Volgende stappen
Zie het artikel voor een zelfstudie over het gebruik van de Kopieeractiviteit: 

- [Gegevens kopiëren van Blob Storage naar SQL Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
