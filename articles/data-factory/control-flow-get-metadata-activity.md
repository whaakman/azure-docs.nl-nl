---
title: Ophalen van metagegevens van activiteit in Azure Data Factory | Microsoft Docs
description: Meer informatie over hoe u de SQL Server opgeslagen Procedure activiteit kunt gebruiken om aan te roepen, een opgeslagen procedure in een Azure SQL Database of een Azure SQL Data Warehouse van een Data Factory-pijplijn.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: shlo
ms.openlocfilehash: 91ef3f9f15797c8c0c599e8c01070369e1af0b58
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Ophalen van metagegevens van activiteit in Azure Data Factory
GetMetadata activiteit kan worden gebruikt voor het ophalen van **metagegevens** van alle gegevens in Azure Data Factory. Deze activiteit wordt alleen ondersteund voor data Factory van versie 2. Het kan worden gebruikt in de volgende scenario's:

- De informatie over de metagegevens van alle gegevens valideren
- Een pijplijn wordt geactiveerd wanneer gegevens gereed / beschikbaar

De volgende functionaliteit is beschikbaar in de Controlestroom:

- De uitvoer van activiteit GetMetadata kan worden gebruikt in voorwaardelijke expressies validatie uitvoeren.
- Een pijplijn kan worden geactiveerd als voorwaarde wordt voldaan via komen-totdat herhalen

> [!NOTE]
> Dit artikel is van toepassing op versie 2 van Data Factory, dat zich momenteel in de previewfase bevindt. Als u van versie 1 van de Data Factory-service gebruikmaakt (GA) is algemeen beschikbaar is, raadpleegt u [Data Factory V1 documentatie](v1/data-factory-introduction.md).

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

De activiteit GetMetadata duurt een gegevensset als een vereiste invoer en uitvoer van de metagegevens beschikbaar als uitvoer van activiteit. Op dit moment wordt worden de volgende connectors met bijbehorende ophalen mogelijk meatadata ondersteund:

### <a name="supported-connectors"></a>Ondersteunde connectors

**Opslag van bestanden:**

| Connector-Metadata | itemName<br>(bestand/map) | itemType<br>(bestand/map) | grootte<br>(bestand) | gemaakt<br>(bestand/map) | LastModified<br>(bestand/map) |childItems<br>(map) |contentMD5<br>(bestand) | structuur<br/>(bestand) | aantal kolommen<br>(bestand) | Er bestaat<br>(bestand/map) |
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| Azure Blob | √/√ | √/√ | √ | x/x | √/√ | √ | √ | √ | √ | √/√ |
| Azure Data Lake Store | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| Azure File Storage | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| Bestandssysteem | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| SFTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| FTP | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |

**Relationele database:**

| Connector-Metadata | structuur | aantal kolommen | Er bestaat |
|:--- |:--- |:--- |:--- |
| Azure SQL Database | √ | √ | √ |
| Azure SQL Data Warehouse | √ | √ | √ |
| SQL Server | √ | √ | √ |

### <a name="metadata-options"></a>Opties voor metagegevens

De volgende typen van de metagegevens kunnen worden opgegeven in de lijst met GetMetadata activiteit om op te halen:

| Metagegevens van type | Beschrijving |
|:--- |:--- |
| itemName | Naam van het bestand of map. |
| itemType | Type van het bestand of map. Uitvoerwaarde `File` of `Folder`. |
| grootte | De grootte van het bestand in bytes. Van toepassing op het bestand alleen. |
| gemaakt | Gemaakte datum/tijd van het bestand of map. |
| LastModified | Datum/tijd van het bestand of map het laatst is gewijzigd. |
| childItems | Lijst met submappen en bestanden in de opgegeven map. Van toepassing op de map alleen. Uitvoerwaarde is een lijst met de naam en type van elk onderliggend item. |
| contentMD5 | MD5 van het bestand. Van toepassing op het bestand alleen. |
| structuur | De structuur van de gegevens in het bestand of de relationele database-tabel. Waarde voor uitvoer is een lijst met de naam van kolom en kolomtype. |
| aantal kolommen | Het aantal kolommen in het bestand of de relationele tabel. |
| Er bestaat| Hiermee wordt aangegeven of een bestand/map/tabel of niet bestaat. Houd er rekening mee dat als 'bestaat' is opgegeven in de veldenlijst GetaMetadata, de activiteit treedt er geen fout zelfs als het item (bestand/map/tabel) bestaat niet; in plaats daarvan de methode retourneert `exists: false` in de uitvoer. |

>[!TIP]
>Als u valideren wilt of een bestand/map/tabel of niet bestaat, geef `exists` in de veldenlijst GetMetadata activiteit kunt u controleren de `exists: true/false` het gevolg zijn van de uitvoer van de activiteit. Als `exists` is niet geconfigureerd in de lijst de GetMetadata activiteit mislukt wanneer het object is niet gevonden.

## <a name="syntax"></a>Syntaxis

**GetMetadata activiteit:**

```json
{
    "name": "MyActivity",
    "type": "GetMetadata",
    "typeProperties": {
        "fieldList" : ["size", "lastModified", "structure"],
        "dataset": {
            "referenceName": "MyDataset",
            "type": "DatasetReference"
        }
    }
}
```

**Gegevensset:**

```json
{
    "name": "MyDataset",
    "properties": {
    "type": "AzureBlob",
        "linkedService": {
            "referenceName": "StorageLinkedService",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "folderPath":"container/folder",
            "filename": "file.json",
            "format":{
                "type":"JsonFormat"
            }
        }
    }
}
```

## <a name="type-properties"></a>Type-eigenschappen

GetMetadata activiteit kunt momenteel de volgende soorten informatie over metagegevens ophalen.

Eigenschap | Beschrijving | Vereist
-------- | ----------- | --------
Veldenlijst | Bevat de typen metagegevens die zijn vereist. Zie voor meer informatie [opties voor metagegevens](#metadata-options) sectie op ondersteunde metagegevens. | Ja 
Gegevensset | De verwijzingsgegevensset waarvan activiteit metagegevens worden opgehaald door de activiteit GetMetadata is. Zie [ondersteunde mogelijkheden](#supported-capabilities) sectie op ondersteunde connectors en verwijzen naar de connector onderwerp bij de details van de syntaxis van de gegevensset. | Ja

## <a name="sample-output"></a>Voorbeelduitvoer

Het resultaat GetMetadata wordt weergegeven in de uitvoer van activiteit. Hieronder ziet u twee voorbeelden met opties voor volledige metagegevens geselecteerd in de lijst met velden als verwijzing. Als u het resultaat van de volgende activiteit, gebruikt u het patroon van `@{activity('MyGetMetadataActivity').output.itemName}`.

### <a name="get-a-files-metadata"></a>Ophalen van metagegevens van het bestand

```json
{
  "exists": true,
  "itemName": "test.csv",
  "itemType": "File",
  "size": 104857600,
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "contentMD5": "cMauY+Kz5zDm3eWa9VpoyQ==",
  "structure": [
    {
        "name": "id",
        "type": "Int64"
    },
    {
        "name": "name",
        "type": "String"
    }
  ],
  "columnCount": 2
}
```

### <a name="get-a-folders-metadata"></a>Een map metagegevens ophalen

```json
{
  "exists": true,
  "itemName": "testFolder",
  "itemType": "Folder",
  "lastModified": "2017-02-23T06:17:09Z",
  "created": "2017-02-23T06:17:09Z",
  "childItems": [
    {
      "name": "test.avro",
      "type": "File"
    },
    {
      "name": "folder hello",
      "type": "Folder"
    }
  ]
}
```

## <a name="next-steps"></a>Volgende stappen
Zie andere controlestroomactiviteiten die door Data Factory worden ondersteund: 

- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)