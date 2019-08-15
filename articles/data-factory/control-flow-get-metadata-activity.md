---
title: Meta gegevens activiteit in Azure Data Factory ophalen | Microsoft Docs
description: Meer informatie over hoe u de GetMetadata-activiteit in een Data Factory pijp lijn kunt gebruiken.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: ''
ms.assetid: 1c46ed69-4049-44ec-9b46-e90e964a4a8e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/12/2019
ms.author: jingwang
ms.openlocfilehash: 320e92e45f319e394b5a38b3f1e8ef3f314920b8
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68966345"
---
# <a name="get-metadata-activity-in-azure-data-factory"></a>Activiteit van meta gegevens in Azure Data Factory ophalen

De GetMetadata-activiteit kan worden gebruikt voor het ophalen van **meta gegevens** van alle gegevens in azure Data Factory. Deze activiteit kan worden gebruikt in de volgende scenario's:

- De meta gegevens van alle gegevens valideren
- Een pijp lijn activeren wanneer de gegevens gereed/beschikbaar zijn

De volgende functionaliteit is beschikbaar in de controle stroom:

- De uitvoer van de GetMetadata-activiteit kan worden gebruikt in voorwaardelijke expressies om validatie uit te voeren.
- Een pijp lijn kan worden geactiveerd wanneer aan de voor waarde wordt voldaan via do-until-lus

## <a name="supported-capabilities"></a>Ondersteunde mogelijkheden

De GetMetadata-activiteit neemt een gegevensset als vereiste invoer en voert uitvoer uit van meta gegevens die beschikbaar zijn als uitvoer van de activiteit. Op dit moment worden de volgende connectors met corresponderende meta gegevens ondersteund en wordt de Maxi maal ondersteunde grootte van de meta gegevens Maxi maal **1 MB**.

>[!NOTE]
>Als u GetMetadata-activiteit uitvoert op een zelf-hosted Integration Runtime, wordt de meest recente mogelijkheid ondersteund op versie 3,6 of hoger. 

### <a name="supported-connectors"></a>Ondersteunde connectors

**Bestands opslag:**

| Connector/meta gegevens | itemName<br>(bestand/map) | itemType<br>(bestand/map) | size<br>Profiler | gemaakt<br>(bestand/map) | lastModified<br>(bestand/map) |childItems<br>map |contentMD5<br>Profiler | structure<br/>Profiler | Aantal<br>Profiler | reeds<br>(bestand/map) |
|:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |:--- |
| [Amazon S3](connector-amazon-simple-storage-service.md) | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| [Google-Cloud opslag](connector-google-cloud-storage.md) | √/√ | √/√ | √ | x/x | √/√* | √ | x | √ | √ | √/√* |
| [Azure Blob](connector-azure-blob-storage.md) | √/√ | √/√ | √ | x/x | √/√* | √ | √ | √ | √ | √/√ |
| [Azure Data Lake Storage Gen1](connector-azure-data-lake-store.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [Azure Data Lake Storage Gen2](connector-azure-data-lake-storage.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [Azure File Storage](connector-azure-file-storage.md) | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| [Bestandssysteem](connector-file-system.md) | √/√ | √/√ | √ | √/√ | √/√ | √ | x | √ | √ | √/√ |
| [SFTP](connector-sftp.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |
| [FTP](connector-ftp.md) | √/√ | √/√ | √ | x/x | √/√ | √ | x | √ | √ | √/√ |

- Voor Amazon S3 en Google Cloud Storage geldt voor `lastModified` Bucket en de sleutel, maar niet voor de virtuele map; `exists` ; en de van toepassing op Bucket en sleutel, maar niet voor voegsel of virtuele map.
- Voor Azure Blob geldt de `lastModified` container en de blob, maar niet voor de virtuele map.

**Relationele data base:**

| Connector/meta gegevens | structure | Aantal | reeds |
|:--- |:--- |:--- |:--- |
| [Azure SQL Database](connector-azure-sql-database.md) | √ | √ | √ |
| [Beheerd exemplaar voor Azure SQL Database](connector-azure-sql-database-managed-instance.md) | √ | √ | √ |
| [Azure SQL Data Warehouse](connector-azure-sql-data-warehouse.md) | √ | √ | √ |
| [SQL Server](connector-sql-server.md) | √ | √ | √ |

### <a name="metadata-options"></a>Opties voor meta gegevens

De volgende meta gegevens typen kunnen worden opgegeven in de velden lijst GetMetadata om op te halen:

| Meta gegevens type | Description |
|:--- |:--- |
| itemName | De naam van het bestand of de map. |
| itemType | Het type van het bestand of de map. Uitvoer waarde is `File` of `Folder`. |
| size | Grootte van het bestand in bytes. Alleen van toepassing op het bestand. |
| gemaakt | Er is een datum/tijd gemaakt van het bestand of de map. |
| lastModified | Datum/tijd waarop het bestand of de map voor het laatst is gewijzigd. |
| childItems | Lijst met submappen en bestanden in de opgegeven map. Alleen van toepassing op de map. De uitvoer waarde is een lijst met de naam en het type van elk onderliggend item. |
| contentMD5 | MD5 van het bestand. Alleen van toepassing op het bestand. |
| structure | De gegevens structuur in het bestand of de relationele database tabel. De uitvoer waarde is een lijst met kolom namen en kolom typen. |
| Aantal | Het aantal kolommen in het bestand of de tabel relationeel. |
| reeds| Hiermee wordt aangegeven of een bestand/map/tabel bestaat of niet. Opmerking als ' exists ' is opgegeven in de GetaMetadata-veld lijst, mislukt de activiteit zelfs wanneer het item (bestand/map/tabel) niet bestaat. in plaats daarvan wordt `exists: false` er een resultaat in de uitvoer weer gegeven. |

>[!TIP]
>Als u wilt valideren of een bestand/map/tabel bestaat, geeft `exists` u in de velden lijst GetMetadata op. vervolgens kunt u het `exists: true/false` resultaat van de uitvoer van de activiteit controleren. Als `exists` niet is geconfigureerd in de lijst met velden, mislukt de GetMetadata-activiteit wanneer het object niet wordt gevonden.

>[!NOTE]
>Wanneer u meta gegevens ophaalt uit bestanden `modifiedDatetimeStart` en configureert en `modifiedDatetimeEnd`/of `childItems` , retourneert de in uitvoer alleen bestanden onder het opgegeven pad met de laatste wijzigings tijd tussen het bereik, maar geen submappen.

## <a name="syntax"></a>Syntaxis

**GetMetadata-activiteit:**

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

**Sets**

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

Momenteel kan de GetMetadata-activiteit de volgende typen meta gegevens ophalen.

Eigenschap | Description | Vereist
-------- | ----------- | --------
fieldList | Een lijst met de typen meta gegevens die zijn vereist. Zie de sectie Details in [meta gegevens opties](#metadata-options) voor ondersteunde meta gegevens. | Ja 
sets | De referentie gegevensset waarvan de meta gegevens activiteit moet worden opgehaald door de GetMetadata-activiteit. Zie de sectie [ondersteunde mogelijkheden](#supported-capabilities) op ondersteunde connectors en Raadpleeg het onderwerp over de syntaxis van de gegevensset voor informatie over de gegevens van een connector. | Ja
formatSettings | Toep assen bij gebruik van gegevensset voor indelings type. | Nee
storeSettings | Toep assen bij gebruik van gegevensset voor indelings type. | Nee

## <a name="sample-output"></a>Voorbeelduitvoer

Het resultaat van de GetMetadata wordt weer gegeven in de uitvoer van de activiteit. Hieronder vindt u twee voor beelden met uitgebreide opties voor meta gegevens die zijn geselecteerd in de velden lijst als verwijzing. Gebruik het patroon van `@{activity('MyGetMetadataActivity').output.itemName}`om het resultaat in de volgende activiteit te gebruiken.

### <a name="get-a-files-metadata"></a>Meta gegevens van een bestand ophalen

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

### <a name="get-a-folders-metadata"></a>Meta gegevens van een map ophalen

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
Zie andere controle stroom activiteiten die door Data Factory worden ondersteund: 

- [Execute Pipeline Activity](control-flow-execute-pipeline-activity.md)
- [Voor elke activiteit](control-flow-for-each-activity.md)
- [Lookup Activity](control-flow-lookup-activity.md)
- [Webactiviteit](control-flow-web-activity.md)
