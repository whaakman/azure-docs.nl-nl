---
title: Toewijzing van gegevensset kolommen in Azure Data Factory | Microsoft Docs
description: Informatie over het toewijzen van kolommen in de gegevensbron aan kolommen van de bestemming.
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: b2cb55bc0c3888fa05b2dd49df10ec157c8bf95e
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="map-source-dataset-columns-to-destination-dataset-columns"></a>Bronkolommen gegevensset toewijzen aan bestemming gegevensset kolommen
> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. 

Kolomtoewijzing kan worden gebruikt om op te geven hoe kolommen opgegeven in de 'structuur"van tabeltoewijzing van bron naar kolommen opgegeven in de 'structuur' van tabel sink. De **columnMapping** eigenschap is beschikbaar in de **typeProperties** sectie van de kopieeractiviteit.

Kolomtoewijzing ondersteunt de volgende scenario's:

* Alle kolommen in de structuur van de gegevensset bron worden toegewezen aan alle kolommen in de structuur van de gegevensset sink.
* Een subset van de kolommen in de structuur van de gegevensbron dataset wordt toegewezen aan alle kolommen in de structuur van de gegevensset sink.

Hier volgen de fouten die ertoe leiden dat een uitzondering:

* Minder kolommen of meer kolommen in de 'structuur' van tabel sink dan opgegeven in de toewijzing.
* Dubbele toewijzing.
* Resultaat van de SQL-query heeft niet de naam van een kolom die is opgegeven in de toewijzing.

> [!NOTE]
> De volgende voorbeelden zijn voor Azure SQL en Azure Blob, maar zijn van toepassing op een gegevensopslag die ondersteuning biedt voor rechthoekige gegevenssets. Dataset en definities van de gekoppelde service in de voorbeelden om te verwijzen naar gegevens in de relevante gegevens aanpassen.

## <a name="sample-1--column-mapping-from-azure-sql-to-azure-blob"></a>Voorbeeld 1 – toewijzingskolommen van Azure SQL naar Azure blob
In dit voorbeeld heeft een structuur van de invoertabel en deze verwijst naar een SQL-tabel in een Azure SQL database.

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

In dit voorbeeld wordt de uitvoertabel heeft een structuur en deze verwijst naar een blob in Azure blob storage.

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

De volgende JSON definieert een kopieeractiviteit in een pijplijn. De kolommen van bron toegewezen aan kolommen in sink (**columnMappings**) met behulp van de **conversieprogramma** eigenschap.

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
**Stroom van de toewijzing van kolom:**

![Kolom toewijzing stroom](./media/data-factory-map-columns/column-mapping-flow.png)

## <a name="sample-2--column-mapping-with-sql-query-from-azure-sql-to-azure-blob"></a>Voorbeeld 2: toewijzingskolommen met SQL-query uit Azure SQL naar Azure blob
In dit voorbeeld wordt een SQL-query om gegevens te extraheren uit de SQL Azure in plaats van de tabelnaam en de kolomnamen van de te geven in de sectie 'structuur' gebruikt. 

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
De queryresultaten worden in dit geval eerst toegewezen aan kolommen die is opgegeven in 'structuur' van de bron. De kolommen van bron 'structuur' worden vervolgens toegewezen aan kolommen in sink 'structuur' met regels die zijn opgegeven in de columnMappings.  Stel dat de query retourneert 5 kolommen, twee of meer kolommen dan die zijn opgegeven in de 'structuur' van de bron.

**Kolom toewijzing stroom**

![Kolom toewijzing stroom-2](./media/data-factory-map-columns/column-mapping-flow-2.png)

## <a name="next-steps"></a>Volgende stappen
Zie het artikel voor een zelfstudie over het gebruik van de Kopieeractiviteit: 

- [Gegevens kopiëren van Blob-opslag naar SQL-Database](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
